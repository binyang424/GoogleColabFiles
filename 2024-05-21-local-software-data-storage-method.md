# 一般的单机版软件（不联网）用什么存储数据

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

# Reference

1. [一般的单机版软件（不联网）用什么存储数据呢 – PingCode](https://docs.pingcode.com/ask/33827.html)
2. [SQLite 官网](https://www.sqlite.org/)