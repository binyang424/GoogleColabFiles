---
title: PyQt5高级表格编辑器开发模版
post_date: 2025-04-19 13:27:00
---

# PyQt5 高级表格编辑器开发实录

本文分享一个基于 PyQt5 开发的**高级表格编辑器**，涵盖了桌面表格应用中常见的操作与功能设计，具有完整、严谨的工程结构，适合中高级开发者参考与扩展。

## 核心功能概览

- **单元格编辑功能**  
  
  支持标准的复制（Ctrl+C）、粘贴（Ctrl+V）功能，自动检测矩形区域，确保数据完整性。
  
- **结构调整功能**  
  
  支持在当前选中位置插入行/列，或删除选中行/列，方便灵活调整表格结构。
  
- **数据清理功能**  
  
  可一键清除选中单元格内容，保持表格干净简洁。
  
- **查找与替换功能**  
  
  提供查找与替换对话框（非模态），支持：
  
  - 高亮所有匹配项
  - 滚动定位到匹配单元格
  - 单条替换与全部替换
  - 可选区分大小写匹配模式
  
- **撤销替换功能**  
  
  内置撤销栈机制，支持撤销最近的替换操作（Ctrl+Z），提升用户操作的可恢复性。
  
- **右键菜单操作**  
  
  通过自定义右键菜单快速访问所有功能，提高操作效率与用户体验。
  
- **状态栏交互反馈**  
  
  所有重要操作均通过状态栏实时提示，增强用户对系统状态的感知。
  
- **异常安全与日志系统**  
  
  对关键操作进行异常捕获和日志记录，保障应用在运行过程中的稳定性与可维护性。

## 技术细节

- 主界面基于 `QMainWindow` 构建，搭配 `QTableWidget` 完成数据表格展现与编辑。
- 查找与替换功能使用了自定义的 `QDialog`，并实现了数据高亮、定位和替换逻辑。
- 核心操作如撤销、状态提示、粘贴时自动扩展行列等，均充分考虑了用户体验与易用性。
- 通过 `logging` 模块标准化错误处理与调试输出，便于后期维护和二次开发。

## 应用场景

本项目适用于以下场景：

- 快速构建内部数据管理工具
- 桌面端轻量级表格编辑器开发
- PyQt5 高级应用功能学习与实践
- 二次开发 Excel 简化版表格编辑器

## 代码

```python
# -*- coding: utf-8 -*-
"""
Advanced Table Editor - Robust Version
Features:
- Copy/Paste, Insert/Delete Rows & Columns, Clear Cells
- Find & Replace (Highlight All, Scroll to Found, Case Sensitivity)
- Undo Replace (Ctrl+Z)
- Context Menu & Keyboard Shortcuts
- Status Bar Messages, Robust Error Handling
"""

import sys
import logging
from PyQt5.QtWidgets import (
    QApplication, QMainWindow, QWidget, QTableWidget, QTableWidgetItem,
    QVBoxLayout, QHBoxLayout, QMenu, QAction, QDialog, QLabel,
    QLineEdit, QPushButton, QCheckBox, QMessageBox, QStatusBar
)
from PyQt5.QtCore import Qt, QPoint, QTimer

# -----------------------------------------------------------------------------
# Logging Configuration
# -----------------------------------------------------------------------------
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s [%(levelname)s] %(name)s: %(message)s'
)
logger = logging.getLogger(__name__)


# -----------------------------------------------------------------------------
# Search & Replace Dialog (Modeless)
# -----------------------------------------------------------------------------
class SearchReplaceDialog(QDialog):
    """
    A non-modal dialog for find & replace operations.
    """
    def __init__(self, parent=None):
        super().__init__(parent)
        try:
            self.setWindowTitle("查找与替换")
            self.setModal(False)
            self.setFixedSize(400, 200)

            self.layout = QVBoxLayout(self)

            # Find Input
            self.layout.addWidget(QLabel("查找内容："))
            self.search_input = QLineEdit(self)
            self.layout.addWidget(self.search_input)

            # Replace Input
            self.layout.addWidget(QLabel("替换为："))
            self.replace_input = QLineEdit(self)
            self.layout.addWidget(self.replace_input)

            # Case Sensitivity Checkbox
            self.case_checkbox = QCheckBox("区分大小写", self)
            self.layout.addWidget(self.case_checkbox)

            # Buttons
            btn_layout = QHBoxLayout()
            self.find_btn = QPushButton("查找全部", self)
            self.replace_btn = QPushButton("替换当前", self)
            self.replace_all_btn = QPushButton("全部替换", self)
            btn_layout.addWidget(self.find_btn)
            btn_layout.addWidget(self.replace_btn)
            btn_layout.addWidget(self.replace_all_btn)
            self.layout.addLayout(btn_layout)

        except Exception:
            logger.exception("Failed to initialize SearchReplaceDialog")


# -----------------------------------------------------------------------------
# Main Table Widget
# -----------------------------------------------------------------------------
class TableWidget(QTableWidget):
    """
    A QTableWidget subclass with extended editing: copy/paste,
    insert/delete rows & columns, clear cells, find/replace, undo.
    """
    def __init__(self, rows, cols, parent=None):
        super().__init__(rows, cols, parent)
        try:
            # Context menu
            self.setContextMenuPolicy(Qt.CustomContextMenu)
            self.customContextMenuRequested.connect(self.open_context_menu)

            # Search/Replace state
            self.search_results = []
            self.current_idx = -1
            self.last_search = ""
            self.case_sensitive = False

            # Undo stack: list of tuples (row, col, old_text)
            self.undo_stack = []
        except Exception:
            logger.exception("Failed to initialize TableWidget")

    # -------------------------------------------------------------------------
    # Keyboard Shortcuts Handling
    # -------------------------------------------------------------------------
    def keyPressEvent(self, e):
        try:
            if e.modifiers() & Qt.ControlModifier:
                if e.key() == Qt.Key_C:
                    self.copy_selection()
                    return
                elif e.key() == Qt.Key_V:
                    self.paste_selection()
                    return
                elif e.key() == Qt.Key_F:
                    self.open_search_dialog()
                    return
                elif e.key() == Qt.Key_Z:
                    self.undo_replace()
                    return
            super().keyPressEvent(e)
        except Exception:
            logger.exception("Error in keyPressEvent")

    # -------------------------------------------------------------------------
    # Copy Selected Cells as Tab-Delimited Text
    # -------------------------------------------------------------------------
    def copy_selection(self):
        try:
            sel = self.selectedIndexes()
            if not sel:
                self._status("复制失败：没有选中单元格", 2000)
                return

            rows = sorted({i.row() for i in sel})
            cols = sorted({i.column() for i in sel})
            # Ensure contiguous rectangle
            if len(rows) * len(cols) != len(sel):
                self._status("复制失败：请选择矩形区域", 2000)
                return

            # Build text
            text_rows = []
            for r in rows:
                row_items = []
                for c in cols:
                    it = self.item(r, c)
                    row_items.append(it.text() if it else "")
                text_rows.append("\t".join(row_items))
            QApplication.clipboard().setText("\n".join(text_rows))
            self._status("已复制", 1000)
        except Exception:
            logger.exception("Error in copy_selection")

    # -------------------------------------------------------------------------
    # Paste Clipboard Text into Table
    # -------------------------------------------------------------------------
    def paste_selection(self):
        try:
            sel = self.selectedIndexes()
            if not sel:
                self._status("粘贴失败：没有选中单元格", 2000)
                return

            start_r = sel[0].row()
            start_c = sel[0].column()
            data = QApplication.clipboard().text().split("\n")
            for i, row in enumerate(data):
                cols = row.split("\t")
                for j, val in enumerate(cols):
                    if not val:
                        continue
                    r = start_r + i
                    c = start_c + j
                    # Expand table if needed
                    if r >= self.rowCount():
                        self.setRowCount(r + 1)
                    if c >= self.columnCount():
                        self.setColumnCount(c + 1)
                    self.setItem(r, c, QTableWidgetItem(val))
            self._status("已粘贴", 1000)
        except Exception:
            logger.exception("Error in paste_selection")

    # -------------------------------------------------------------------------
    # Insert and Delete Rows / Columns
    # -------------------------------------------------------------------------
    def insert_row(self):
        try:
            sel = self.selectedIndexes()
            if sel:
                self.insertRow(sel[0].row())
                self._status("插入行", 1000)
        except Exception:
            logger.exception("Error in insert_row")

    def insert_column(self):
        try:
            sel = self.selectedIndexes()
            if sel:
                self.insertColumn(sel[0].column())
                self._status("插入列", 1000)
        except Exception:
            logger.exception("Error in insert_column")

    def delete_row(self):
        try:
            rows = sorted({i.row() for i in self.selectedIndexes()}, reverse=True)
            for r in rows:
                self.removeRow(r)
            self._status("删除行", 1000)
        except Exception:
            logger.exception("Error in delete_row")

    def delete_column(self):
        try:
            cols = sorted({i.column() for i in self.selectedIndexes()}, reverse=True)
            for c in cols:
                self.removeColumn(c)
            self._status("删除列", 1000)
        except Exception:
            logger.exception("Error in delete_column")

    # -------------------------------------------------------------------------
    # Clear Selected Cells
    # -------------------------------------------------------------------------
    def clear_cells(self):
        try:
            for idx in self.selectedIndexes():
                self.setItem(idx.row(), idx.column(), QTableWidgetItem(""))
            self._status("已清除选中单元格内容", 1000)
        except Exception:
            logger.exception("Error in clear_cells")

    # -------------------------------------------------------------------------
    # Open Context Menu
    # -------------------------------------------------------------------------
    def open_context_menu(self, pos: QPoint):
        try:
            menu = QMenu(self)
            menu.addAction(self._action("复制", self.copy_selection, "Ctrl+C"))
            menu.addAction(self._action("粘贴", self.paste_selection, "Ctrl+V"))
            menu.addAction(self._action("查找/替换", self.open_search_dialog, "Ctrl+F"))
            menu.addSeparator()
            menu.addAction(self._action("插入行", self.insert_row))
            menu.addAction(self._action("插入列", self.insert_column))
            menu.addAction(self._action("删除行", self.delete_row))
            menu.addAction(self._action("删除列", self.delete_column))
            menu.addSeparator()
            menu.addAction(self._action("清除内容", self.clear_cells))
            menu.addAction(self._action("撤销替换", self.undo_replace, "Ctrl+Z"))
            menu.exec_(self.viewport().mapToGlobal(pos))
        except Exception:
            logger.exception("Error in open_context_menu")

    def _action(self, text, slot, shortcut=None):
        act = QAction(text, self)
        if shortcut:
            act.setShortcut(shortcut)
        act.triggered.connect(slot)
        return act

    # -------------------------------------------------------------------------
    # Search & Replace Methods
    # -------------------------------------------------------------------------
    def open_search_dialog(self):
        try:
            # If already open, just raise
            if hasattr(self, 'sr_dialog') and self.sr_dialog.isVisible():
                self.sr_dialog.raise_()
                return
            self.sr_dialog = SearchReplaceDialog(self)
            self.sr_dialog.find_btn.clicked.connect(lambda: self.find_all(self.sr_dialog))
            self.sr_dialog.replace_btn.clicked.connect(lambda: self.replace_current(self.sr_dialog))
            self.sr_dialog.replace_all_btn.clicked.connect(lambda: self.replace_all(self.sr_dialog))
            self.sr_dialog.show()
        except Exception:
            logger.exception("Error in open_search_dialog")

    def find_all(self, dlg):
        try:
            self.clear_highlights()
            txt = dlg.search_input.text()
            if not txt:
                return
            self.case_sensitive = dlg.case_checkbox.isChecked()
            self.search_results.clear()
            for r in range(self.rowCount()):
                for c in range(self.columnCount()):
                    it = self.item(r, c)
                    if it:
                        cell = it.text()
                        if (txt in cell if self.case_sensitive else txt.lower() in cell.lower()):
                            self.search_results.append((r, c))
                            it.setBackground(Qt.yellow)
            if self.search_results:
                # scroll to first
                first_r, first_c = self.search_results[0]
                self.setCurrentCell(first_r, first_c)
                self.scrollToItem(self.item(first_r, first_c))
                self._status(f"找到 {len(self.search_results)} 处匹配", 2000)
            else:
                QMessageBox.information(self, "查找", "未找到匹配项")
        except Exception:
            logger.exception("Error in find_all")

    def replace_current(self, dlg):
        try:
            if not self.search_results:
                return
            r, c = self.search_results[0]
            it = self.item(r, c)
            if it:
                old = it.text()
                new = dlg.replace_input.text()
                self.undo_stack.append((r, c, old))
                it.setText(new)
            self.find_all(dlg)
            self._status("替换当前完成", 1500)
        except Exception:
            logger.exception("Error in replace_current")

    def replace_all(self, dlg):
        try:
            if not self.search_results:
                return
            replacement = dlg.replace_input.text()
            count = 0
            for r, c in self.search_results:
                it = self.item(r, c)
                if it:
                    old = it.text()
                    self.undo_stack.append((r, c, old))
                    it.setText(replacement)
                    count += 1
            self.find_all(dlg)
            QMessageBox.information(self, "全部替换", f"共替换 {count} 处匹配")
        except Exception:
            logger.exception("Error in replace_all")

    # -------------------------------------------------------------------------
    # Undo Last Replace
    # -------------------------------------------------------------------------
    def undo_replace(self):
        try:
            if not self.undo_stack:
                self._status("无可撤销操作", 2000)
                return
            r, c, old = self.undo_stack.pop()
            self.setItem(r, c, QTableWidgetItem(old))
            self._status("已撤销替换", 1000)
        except Exception:
            logger.exception("Error in undo_replace")

    # -------------------------------------------------------------------------
    # Clear Highlights
    # -------------------------------------------------------------------------
    def clear_highlights(self):
        try:
            for r in range(self.rowCount()):
                for c in range(self.columnCount()):
                    it = self.item(r, c)
                    if it:
                        it.setBackground(Qt.white)
        except Exception:
            logger.exception("Error in clear_highlights")

    # -------------------------------------------------------------------------
    # Helper: Show status message
    # -------------------------------------------------------------------------
    def _status(self, message, timeout=1000):
        try:
            mw = self.window()
            if isinstance(mw, QMainWindow):
                mw.statusBar().showMessage(message, timeout)
        except Exception:
            logger.exception("Error in _status")



# -----------------------------------------------------------------------------
# Main Application Window
# -----------------------------------------------------------------------------
class MainWindow(QMainWindow):
    """
    Main window containing the TableWidget and status bar.
    """
    def __init__(self):
        super().__init__()
        try:
            self.setWindowTitle("高级表格编辑器")
            self.resize(900, 600)
            # Status Bar
            self.setStatusBar(QStatusBar(self))

            # Central Widget
            central = QWidget(self)
            layout = QVBoxLayout(central)
            self.table = TableWidget(100, 10, self)
            layout.addWidget(self.table)
            self.setCentralWidget(central)
        except Exception:
            logger.exception("Failed to initialize MainWindow")


# -----------------------------------------------------------------------------
# Application Entry Point
# -----------------------------------------------------------------------------
if __name__ == "__main__":
    try:
        app = QApplication(sys.argv)
        main_win = MainWindow()
        main_win.show()
        sys.exit(app.exec_())
    except Exception:
        logger.exception("Unhandled exception in main")
```

