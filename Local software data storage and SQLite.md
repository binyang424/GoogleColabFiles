---
title: 单机（不联网）软件的数据存储
post_status: publish
post_date: 2024-05-21 19:00:00
---

# 一、单机软件（不联网）的数据存储

## 1、SQLite

### 简介

SQLite是一种轻量级的关系型数据库引擎（关系型数据库管理系统），可以直接将数据存储在一个文件中，无需单独安装数据库服务。SQLite支持标准的SQL语言，并提供了对事务、索引、触发器等数据库特性的支持，适用于小规模的数据存储和管理。

它包含在一个相对小的C库中。它是D.RichardHipp建立的公有领域项目。它的设计目标是嵌入式的，而且已经在很多嵌入式产品中使用了它，它占用资源非常的低，在嵌入式设备中，可能只需要几百K的内存就够了。它能够支持Windows/Linux/Unix等等主流的操作系统，同时能够跟很多程序语言相结合，比如 Tcl、C#、PHP、Java等，还有ODBC接口，同样比起Mysql、PostgreSQL这两款开源的世界知名数据库管理系统来讲，它的处理速度比他们都快。SQLite名列前茅个Alpha版本诞生于2000年5月。 至2021年已经接近有21个年头，SQLite也迎来了一个版本 SQLite 3已经发布。

### 工作原理

不像常见的客户—服务器范例，SQLite引擎不是个程序与之通信的独立进程，而是连接到程序中成为它的一个主要部分。所以主要的通信协议是在编程语言内的直接API调用。这在消耗总量、延迟时间和整体简单性上有积极的作用。整个数据库（定义、表、索引和数据本身）都在宿主主机上存储在一个单一的文件中。它的简单的设计是通过在开始一个事务的时候锁定整个数据文件而完成的。

### 功能特性

-   [Full-featured SQL](https://www.sqlite.org/about.htmlfullsql.html)
-   [Billions and billions of deployments](https://www.sqlite.org/about.htmlmostdeployed.html)
-   [Single-file database](https://www.sqlite.org/about.htmlonefile.html) 储存在单一磁盘文件中的一个完整的数据库
-   [Public domain source code ](https://www.sqlite.org/about.htmlcopyright.html)源码完全的开源，你可以用于任何用途，包括出售它
-   All source code in one file ([sqlite3.c](https://www.sqlite.org/about.htmlamalgamation.html)) 
-   [Small footprint](https://www.sqlite.org/about.htmlfootprint.html)
-   Max DB size: [281 terabytes](https://www.sqlite.org/about.htmllimits.html) (2<sup><small>48</small></sup> bytes)
-   Max row size: [1 gigabyte](https://www.sqlite.org/about.htmllimits.html)
-   [Faster than direct file I/O ](https://www.sqlite.org/about.htmlfasterthanfs.html)比一些流行的数据库在大部分普通数据库操作要快
-   [Aviation-grade quality and testing](https://www.sqlite.org/about.htmltesting.html)
-   [Zero-configuration](https://www.sqlite.org/about.htmlzeroconf.html) 零配置，无需安装和管理配置
-   [ACID transactions, even after power loss ](https://www.sqlite.org/about.htmltransactional.html)ACID事务
-   [Stable, enduring file format](https://www.sqlite.org/about.htmlfileformat.html)
-   [Extensive, detailed documentation](https://www.sqlite.org/about.htmldoclist.html) 良好注释的源代码，并且有着90%以上的测试覆盖率
-   [Long-term support](https://www.sqlite.org/about.htmllts.html)
-   数据库文件可以在不同字节顺序的机器间自由的共享
-   简单的API
-   独立，没有额外依赖
-   支持多种开发语言，C，C++，PHP，Perl，Java，C#，Python，Ruby等

同时它还支持事务处理功能等等。也有人说它象Microsoft的Access，有时候真的觉得有点象，但是事实上它们区别很大。比如SQLite 支持跨平台，操作简单，能够使用很多语言直接创建数据库，而不象Access一样需要Office的支持。如果你是个很小型的应用，或者你想做嵌入式开发，没有合适的数据库系统，那么你可以考虑使用SQLite。

### 常用函数

SQLite 有许多内置函数用于处理字符串或数字数据。下面列出了一些有用的 SQLite 内置函数，且所有函数都是大小写不敏感的：

-   **SQLite COUNT 函数**：SQLite COUNT 聚集函数是用来计算一个数据库表中的行数。
-   **SQLite MAX 函数**：SQLite MAX 聚合函数允许我们选择某列的最大值。
-   **SQLite MIN 函数**：SQLite MIN 聚合函数允许我们选择某列的最小值。
-   **SQLite AVG 函数**：SQLite AVG 聚合函数计算某列的平均值。
-   **SQLite SUM 函数**：SQLite SUM 聚合函数允许为一个数值列计算总和。
-   **SQLite RANDOM 函数**：SQLite RANDOM 函数返回一个介于 -9223372036854775808 和 +9223372036854775807 之间的伪随机整数。
-   **SQLite ABS 函数**：SQLite ABS 函数返回数值参数的绝对值。
-   **SQLite UPPER 函数**：SQLite UPPER 函数把字符串转换为大写字母。
-   **SQLite LOWER 函数**：SQLite LOWER 函数把字符串转换为小写字母。
-   **SQLite LENGTH 函数**：SQLite LENGTH 函数返回字符串的长度。
-   **SQLite sqlite\_version 函数**：SQLite sqlite\_version 函数返回 SQLite 库的版本。

## 2、键值对存储

键值对存储是一种轻量级的非关系型数据库，使用简单，并且支持高速读写，常见的键值存储有Redis、LevelDB等。但键值对存储不支持SQL，其功能比较简单，适用于小型应用，对于关联查询等复杂操作支持不太理想。

## 3、本地文件系统

将数据以文本或二进制格式的文件保存在计算机的硬盘上，可以使用常见的TXT、XML、JSON、CSV等格式。这种方式简单易用，适用于小规模数据存储，但数据访问效率相对较低，对大规模数据存储和处理不太适合。

## 4、内存存储

将数据存储在内存中，可以提供非常快速的读写速度和响应时间，适用于小规模的数据，如缓存和临时数据存储等。但由于内存大小限制，不能存储大规模数据且不支持永久存储。



# 二、SQLite

## **SQLite 数据类型**

SQLite 使用关系数据库结构。这意味着你将使用SQL添加、更新和访问数据库中的数据。数据存储在由列组成的表中，并且这些列中的数据必须是一致的类型。以下是 SQLite 支持的数据类型：

-   **NULL：**一个 NULL 值
-   **INTEGER：**一个整数值
-   **REAL：**浮点（十进制）值
-   **TEXT：**文本值
-   **BLOB：**大型二进制对象

如果你熟悉其他关系数据库，那么你可能会注意到 SQLite 中的数据类型是有限的。例如，没有 date 或 varchar 类型。这可能意味着 SQLite 无法满足某些数据库需求，但它对许多应用程序仍然有用。

## 内容查询与检索

这个查询将检索`students`表中年龄大于18岁的学生名字和年龄，连接到`classes`表以获取每个学生的班级信息，然后按照名字进行排序，返回从第6条开始的10条记录，并按班级分组，只返回每个班级中学生人数超过1的班级记录。

```sqlite
SELECT DISTINCT name, age
FROM students
JOIN classes ON students.class_id = classes.id
WHERE age > 18
ORDER BY name ASC
LIMIT 10 OFFSET 5
GROUP BY class_id
HAVING COUNT(*) > 1;
```

1.  **SELECT DISTINCT column\_list**:
    -   **SELECT**: 用于选择要从数据库中检索的数据列。
    -   **DISTINCT**: 可选关键字，用于返回唯一的、不重复的记录。如果不需要去重，可以省略。
2.  **FROM table\_list**:
    
    -   **FROM**: 指定要从中检索数据的表或视图。
    -   **table\_list**: 表示一个或多个表的名称，可以使用逗号分隔。
3.  **JOIN table ON join\_condition**:
    
    -   **JOIN**: 用于将两张表关联起来，根据指定的条件组合数据。
    -   **table**: 需要连接的另一个表。
    -   **ON join\_condition**: 指定连接条件，定义如何匹配表中的记录。
4.  **WHERE row\_filter**:
    
    -   **WHERE**: 用于过滤满足特定条件的行，只返回符合条件的记录。
    -   **row\_filter**: 指定的过滤条件。
5.  **ORDER BY column**:
    
    -   **ORDER BY**: 用于对结果集进行排序。
    -   **column**: 指定排序的列，可以指定升序（ASC）或降序（DESC），默认是升序。
6.  **LIMIT count OFFSET offset**:
    
    -   **LIMIT**: 用于限制返回的记录数量。
    -   **count**: 指定要返回的最大记录数。
    -   **OFFSET**: 可选关键字，指定开始返回记录之前要跳过的记录数。
7.  **GROUP BY column**:
    
    -   **GROUP BY**: 用于将结果集分组，根据一个或多个列进行分组，常用于聚合函数（如COUNT、SUM、AVG等）。
8.  **HAVING group\_filter**:
    
    -   **HAVING**: 用于过滤分组后的记录，类似于WHERE，但HAVING作用于分组后的记录。
    -   **group\_filter**: 指定的过滤条件。

一个完整的SQL查询可能包含这些子句中的部分或全部，以实现复杂的数据检索和处理。

# 三、Python SQLite 入门

 Python中内置了SQLite3 模块，即使如此，仍然需要将 sqlite3 模块导入到 Python 文件中，才可以正常使用：

```python
import sqlite3
```

## **创建并连接到 SQLite 数据库**

使用 Python SQLite，创建和连接数据库是一回事。如果数据库文件存在，Python 将连接到它。如果它不存在，那么它将创建数据库文件并连接到它：

```python
# 导入 sqlite3 模块
import sqlite3

# 创建数据库并使用 connect 函数连接它
# conn 变量将用于与数据库交互
conn = sqlite3.connect('sqlite3.db')

# 连接到指定文件夹的数据库
conn = sqlite3.connect('/some/other/folder/sqlite3.db')
```

我们将此连接对象设置为变量 `conn`，并在其余步骤中使用该变量。如果不需要静态数据库文件，也可以创建和使用只存在于内存中的SQL数据库，**一旦程序停止就会消失**。以下是创建和连接到内存中 SQLite 数据库的方法：

```python
conn = sqlite3.connect(":memory:")
```

## **使用 Python SQLite 创建游标对象**

完成创建连接对象后，你现在需要创建一个游标(`cursor`)对象以使用 SQL 查询数据库：

```python
# 创建游标
cursor = conn.cursor()
```

此时我们可以使用该`cursor`对象的 `execute` 方法在数据库上执行 SQL命令，SQL 查询命令应该用单引号或双引号括起来：

```python
cursor.execute("SELECT * FROM my_table;")
```

当然，上述数据库仍然是空的，因此执行上述命令会报错。

## **在 SQLite 数据库中创建表**

通过执行 SQL命令我们可以创建表来保存数据。本示例将创建两个表(`users` 和 `notes`)，以便稍后在 SQL 查询中使用`JOIN` ：

```python
# 创建用户表
cursor.execute('''CREATE TABLE IF NOT EXISTS users(
 id INTEGER PRIMARY KEY,
 firstname TEXT,
 lastname TEXT);
''')
# conn.commit()

# 创建笔记表
cursor.execute('''CREATE TABLE IF NOT EXISTS notes(
 id INTEGER PRIMARY KEY,
 userid INTEGER,
 note TEXT);
''')
conn.commit()

cursor.execute("SELECT * FROM users;")
cursor.execute("SELECT * FROM notes;")
```

关于上面的代码，应该注意以下几点：

-   我们**使用三引号将 SQL 查询括起来，因为 Python 中的三引号允许你创建跨越多行的字符串变量。这使你可以根据需要格式化 SQL**。
-   SQL 查询的 IF NOT EXISTS部分让我们在创建表之前检查表是否存在。如果表已经存在，则不会发生任何事情，脚本将进入下一步。
-   我们使用游标方法 `execute()` 来运行 SQL 查询。
-   连接对象的`conn.commit()`方法可将这个事务提交到数据库。在调用 commit 之前，更改不会显示在数据库中，**也可以在调用 commit 之前执行多个查询，最后只调用一次**。如果出现错误，你可以调用 `conn.rollback()`，它将所有更改回滚到最后一次提交之前。
-   我们在每个表上设置一个 `id` 列，作为表的主键。如果我们添加 `AUTOINCREMENT` 关键字，SQLite 还可以自动增加 ID，但它会影响性能，如果不需要，则不应使用。我们将手动设置这些 ID。
-   **将列定义为 INTEGER PRIMARY KEY 时，SQLite 不允许空值**，所以我们在创建记录时总是需要设置它，否则会抛出错误。
-   我们在 `notes` 表中添加了一个 `userid` 列来引用与特定笔记相关的用户。

## **将数据添加到 SQLite 表**

现在我们有一些表可以使用，我们可以将数据添加到表中。我们将继续使用游标对象的execute方法来执行SQL查询和连接对象的commit方法来提交对数据库的更改。如果我们只是想添加一个有一个注释的用户：

```python
# 添加单个用户
cursor.execute('''INSERT INTO users(id, firstname, lastname)
 VALUES(1, 'John', 'Doe');
''')
conn.commit()

# 添加单个笔记
cursor.execute('''INSERT INTO notes(id, userid, note)
 VALUES(1, 1, 'This is a note');
 ''')
conn.commit()
```

如果我们想一次添加多条记录，下面的方法更简单：

```python
# 多个用户
all_users = [(2, 'Bob', 'Doe'), (3, 'Jane', 'Doe'), (4, 'Jack', 'Doe')];

# 添加多个用户
cursor.executemany('''INSERT INTO users(id, firstname, lastname)
 VALUES(?, ?, ?);''', all_users)
conn.commit()

# 多个笔记
bobs_notes = [(2, 2, '这是第二个笔记'), (3, 2, '这是第三个笔记')];
janes_notes = [(4, 3, '这是第四个笔记'), (5, 3, '这是第五个笔记')];
jacks_notes = [(6, 4, '这是第六个笔记'), (7, 4, '这是第七个笔记')];
all_notes = bobs_notes + janes_notes + jacks_notes;

# 添加多个笔记
cursor.executemany('''INSERT INTO notes(id, userid, note)
 VALUES(?, ?, ?);''', all_notes)
conn.commit()
```

以下是你需要了解的有关上述代码的信息：

-   `executemany() `方法接受两个参数：一个带有占位符的 SQL 查询命令，其中将插入值，以及一个包含要插入的记录的元组列表。
-   为了创建每个人的笔记的单个列表，我们使用 + 运算符连接笔记列表。
-   这种运行 SQL 查询的方法可以避免 SQL 注入攻击。

还需要注意的是，你可以使用类似的方法通过` execute()` 方法添加记录，但它一次只会插入一条记录：

```python
# 添加单个用户
user = (1, 'John', 'Doe')
cursor.execute('''INSERT INTO users(id, firstname, lastname)
 VALUES(?, ?, ?);
''', user)
conn.commit()
```

从现在开始，我们将使用这种执行查询的方法，因为这是防止 SQL 注入的好习惯。当然，现在再执行上述代码会报错，因为已经存在用户id为1的数据了。

## **从 SQLite 读取数据**

你可以使用 Python SQLite 以几种不同的方式获取数据。

### **使用 `fetchall()`**

`fetchall() `方法将 SQL 查询产生的每条记录作为元组列表返回：

```python
# 获取所有用户
users = cursor.execute('''SELECT * FROM users''').fetchall()
print(users)
```

以下是这段代码的结果：

```
[( 1 , 'John', 'Doe'), ( 2 , 'Bob', 'Doe'), ( 3 , 'Jane', 'Doe'), ( 4 , 'Jack', 'Doe')]
```

我们还可以使用WHERE子句限制 SQL 查询，以仅获取 Jack 笔记的文本并返回所有这些记录：

```
# 获取杰克的笔记
jacks_notes = cursor.execute('''SELECT note FROM notes WHERE userid = ?''', (4,)).fetchall()
print(jacks_notes)
```

这是执行此代码的结果：

```
[('这是第六个笔记',), ('这是第七个笔记',)]
```

### **使用 `fetchmany()`**

`fetchmany() `方法类似于` fetchall() `方法，但接受一个整数参数，该参数指定要获取的记录数(LIMIT)：

```python
# 获取前两个用户
users = cursor.execute('''SELECT * FROM users''').fetchmany(2)
print(users)
```

以下是此代码返回的记录：

```
[( 1 , 'John', 'Doe'), ( 2 , 'Bob', 'Doe')]
```

### **使用 `fetchone()`**

Python SQLite `fetchone()` 方法类似于在 Microsoft SQL Server 中使用` SELECT TOP 1` 并返回查询的第一条记录：

```python
# 获取一个用户
user = cursor.execute('''SELECT * FROM users''').fetchone()
print(user)
```

这是此脚本打印的内容：

```
(1 , '约翰', 'Doe')
```

**使用` fetchone() `时，你会得到一个元组，而不是一个元组的列表。**

### **连接表**

你不必拘泥于 SQLite 中的简单 命令。毕竟，关系数据库的重点是将不同的数据集相互关联。我们创建了一个 `users` 表，然后使用 `notes` 表中的 `userid` 引用每个用户，以便将便笺与拥有它们的用户相关联。现在是时候让这种关系发挥作用了。**假设我们想要 notes 表中的每个便笺以及拥有该便笺的用户的名字和姓氏。以下是我们如何从数据库中检索该数据：**

```python
# 获取note中的用户名
notes = cursor.execute('''
 SELECT u.firstname, u.lastname, n.note FROM users AS u
 INNER JOIN notes AS n
 ON u.id = n.userid''').fetchall()
print(notes)
```

请注意，我们在查询中使用了 AS 关键字。这为 SQLite 中的表名称设置了一个别名，因此我们不必在使用它的任何地方输入整个名称。这是此查询的结果：

```
[('John', 'Doe', 'This is a note'), ('Bob', 'Doe', 'This is a second note'), ('Bob', 'Doe', 'This is a third note'), ('Jane', 'Doe', 'This is a Fourth note'), ('Jane', 'Doe', 'This is a Fifth note'), ('Jack', 'Doe', '这是第七个笔记')]
```

## **更新 SQLite 数据库中的数据**

你还可以通过在 SQL 查询中使用更新语句来更新SQLite 数据库中的数据。在此示例中，Jack 想要更改他的一个笔记的内容：

```python
# 更新 Jack 的笔记
updated_note = 'This is an updated note'
note_id = 6
cursor.execute('''UPDATE notes SET note = ? WHERE id = ?;''', (updated_note, note_id))
conn.commit()
```

如果我们查看数据库，我们可以看到注释已更新。

## **使用 Python SQLite 删除数据**

要删除数据库中的数据，过程是相同的。只需将删除语句添加到你的 SQL 查询、执行查询并提交更改：

```python
# 删除 Jack 的一条笔记
note_id = 6
cursor.execute('''DELETE FROM notes WHERE id = ?;''', (note_id, ))
conn.commit()
```

当我们查看数据库时，id 为 6 的便笺将消失。

## **关闭数据库连接**

你可能已经注意到，我们在代码中一遍又一遍地处理了两个 Python SQLite 对象：一个连接和一个游标。当我们不再需要它们时关闭这些对象是一种很好的做法，这是一种使用 `with` 语句和 Python `contextlib` 中的关闭方法自动完成的简单方法：

```python
from contextlib import closing

with closing(sqlite3.connect('db.sqlite3')) as conn:
    with closing(conn.cursor()) as cursor:
    # 这里是我们执行所有查询的地方
    users = cursor.execute(' SELECT * from users').fetchall()
    print(users)   
    # 一旦我们退出 with 块，游标和连接都将被关闭
```

# Reference

1. [一般的单机版软件（不联网）用什么存储数据呢 – PingCode](https://docs.pingcode.com/ask/33827.html)
2. [SQLite 官网](https://www.sqlite.org/)
3. [如何在 Python 中使用 SQLite 管理数据 - 疯狂的小黑](https://www.xiaoheiwoo.com/python-sqlite/)
4. [SQLite 教程 | 菜鸟教程](https://www.runoob.com/sqlite/sqlite-tutorial.html)