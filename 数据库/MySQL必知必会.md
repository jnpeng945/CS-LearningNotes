> 《MySQL 必知必会》读书笔记

# 第 1 章 了解 SQL

SQL（结构化查询语言，Structured Query Language）：一种专用来与数据库通信的语言。其设计目的是提供一种从数据库中读写数据的简单有效的方法。

:bulb: SQL的优点：

- 兼容性：所有重要的DBMS都支持SQL，虽然可能不完全相同，不同 DBMS 的 SQL 语法不是完全可移植的；
- 简单易学：语句都是由少量的描述性很强的英语单词组成；
- 功能强：通过 SQL 可进行非常复杂和高级的数据库操作。

:bulb: SQL 语句基本特点：**(分号结尾，不区分大小写，空格忽略)**

- 每条 SQL 语句以分号 `;`  结尾
- SQL 语句不区分大小写，SQL 开发人员喜欢对 SQL 关键字使用大写，列和表名使用小写，以便于阅读和调试；注意： MySQL 4.1.1 版本之后 数据库名、表名和列名都不区分大小写；
- SQL 语句处理时所有的空格忽略；
- SQL 语句一般返回原始的、无格式的数据，数据的格式化是**表示问题**，而不是**检索问题**。表示一般在显示该数据的应用程序中规定。



## 1.1 数据库基础

:star:  **数据库 ( database )** ：保存**有组织**的数据的**容器**（通常是一个文件或一组文件）。

数据库软件应称为 DBMS（数据库管理系统），数据库是通过 DBMS 创建和操纵的容器。通常，我们使用 DBMS 来访问数据库。

**关系型数据库**：（SQL）

- MySQL、Oracle、SQLServer
- 通过表和表之间，行和列之间的关系进行数据的存储。

**非关系型数据库**：（NoSQL，Not Only SQL）

- Redis、MongDB
- 对象存储，通过对象自身的属性来确定。

:star:  **表（table）**：某种特定类型数据的结构化清单，存储在表中的数据是一种类型的数据或一个清单。

:star:  **表名**，数据库中的表用表名来标识自己，同一个数据库内表名唯一，不同数据库中的表名可以相同。

:star:  **模式（schema）**：关于数据库和表的布局及特性的信息。

:star:  **列（column）**：表中的一个字段。所有表都是由一个或多个列组成。

<!--将数据库表想象为一个网格，网格中每一列存储着一条特定的信息。-->

:bulb: 正确地将数据分解为多个列使得我们能利用特定的列对数据进行排序和过滤。

:star:  **数据类型（datatype）**：数据库中每个列都有相应的数据类型，其指示该列中可存储的数据的类型。数据类型可帮助正确排序数据，对于优化磁盘使用有帮助。

:star:  **行（row）**：表中的一个记录。表中的数据是按行存储。

<!--使用时，记录 == 行。技术上，行是正确术语。-->

:star:  **主键（primary key）**：一列（或一组列），其值能够唯一区分表中每个行。

<!--表中的每一行都需要一个能唯一标识自己的一列（或一组列）。表中的主键显然是便于之后的数据操纵和管理-->

主键值规则 ：

- 任意两行都不具有相同的主键值；（行与行之间主键值互不相同）
- 每个行都必须具有一个主键值（主键列**不允许NULL值**）。

通常，主键通常定义在表的一列上；进一步，可使用多个列作为主键，此时所有列值的组合必须是唯一的（但单个列的值可以不唯一）

主键设置建议：

- 不更新主键列中的值；
- 不重用主键列的值；
- 不在主键列中使用可能会更改的值。

<!--外键是什么？第15章 联结表-->



# 第 2 章 MySQL简介

## 2.1 什么是 MySQL

DBMS（数据库管理系统）：也称为数据库软件，用于进行数据的<u>存储、检索、管理和处理</u> 。

MySQL：是一种 DBMS，它的优点有：

- MySQL 是开源软件，社区版**免费**；
- MySQL 执行很快，**性能好**；
- 大公司都使用 MySQL 来处理自己的数据，**技术趋势**；
- 易于安装使用；

:zap: **DBMS 分为两类**：

1. 基于共享文件系统的 DBMS，如 Microsoft Access和FileMaker，常用于<u>桌面用途</u>；

2. 基于客户机—服务器的DBMS，如 MySQL、Oracle 和 Microsoft SQL Server，用于高端或更关键的应用；

   客户机—服务器分为两个不同的部分，其中**服务器部分**是<u>负责所有数据访问和处理</u>的一个软件，该软件运行在数据库服务器上。客服机软件发出相应数据操作请求，服务器软件完成数据添加、删除和数据更新。

<!--多个 MySQL 服务器可安装在单台机器上，但是每个服务器需要使用不同的端口。-->



## 2.2 MySQL 工具

:one: **mysql 命令行实用程序**

每个 MySQL 安装后都有一个名为 mysql 的简单命令行实用程序，通过 `cmd` 命令行即可打开使用。

```sql
mysql -u root -p -h localhost -P 3306		-- 进入mysql命令行
-- 参数说明：-u(用户名) -p(口令密码) -h(主机名) -P(端口号)
-- 命令以 ;或\g结束
-- help select 获得使用SELECT语句的帮助
-- quit 或 exit 退出命令行程序
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_1.png" width="700"/> </div><br>

:two: **MySQL Administrator**

MySQL Administrator（MySQL管理器）是一个图形交互客户机，用来简化MySQL服务器的管理。

> [mysql 下载](http://dev.mysql.com/downloads/ ) ，可用版本：Linux、Mac OS X和Windows ，源代码也可下载。

MySQL Administrator 安装后会提示输入服务器和登录信息，其中：

- Server Information（服务器信息）显示客户机和被连接的服务器的状态和版本信息；
- Service Control（服务控制）允许停止和启动 MySQL 以及指定服务器特性；
- User Administration（用户管理）用来定义 MySQL 用户、登录和权限；
- Catalogs（目录）列出可用的数据库并允许创建数据库和表。

MySQL Administrator 工具菜单包含有启动 mysql 命令行实用程序和MySQL Query Browser（MySQL查询浏览器）的选项，MySQL 查询浏览器也包含启动mysql命令行实用程序和 MySQL Administrator 的菜单选项。。

:three: **MySQL Query Browser**

MySQL Query Browser 是一个图形交互客户机，用来编写和执行 MySQL 命令。

> [mysql 下载](http://dev.mysql.com/downloads/ ) ，可用版本：Linux、Mac OS X和Windows ，源代码也可下载。

:bulb: 可用 MySQL Query Browser 执行保存的脚本，步骤：File —> Open Script —> Execute



# 第 3 章 使用 MySQL

## 3.1 连接

MySQL 使用前要连接到数据库，MySQL在内部保存自己的用户列表，并且把每个用户与各种权限关联起来。

如下所示为使用 SQLyog，查看 MySQL 内部的用户列表，右侧为每个用户相应的权限说明。

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_2.png" width="800"/> </div><br>

连接到 MySQL 所需信息：

- 主机名——如果连接到本地MySQL服务器，为localhost；
- 端口，默认端口为 3306，具体自己设置也可以；
- 合法的用户名以及用户口令密码；

## 3.2 数据库和表基本操作

```sql
SHOW DATABASES;			-- 显示当前 MySQL 中可用的数据库
USE school;			-- 使用 school 数据库
-- USE 语句并不返回任何结果，通常客服机会显示一个通知 `Database changed`。

SHOW TABLES;		-- 显示当前使用的数据库内可用的表
SHOW COLUMNS FROM student;	-- 展示 student 表的表头的属性设置
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_3.png" width="450"/> </div><br>

:star: **自动增量**：某些表列需要唯一值，如订单编号。在表中添加一行时，MySQL可以自动为每行分配下一个可用编号，而不用添加一行后手动分配唯一值，此功能就是自动增量。

<!--自动增量使用需要在 creat 语句创建表时将其作为表定义的组成部分，第 21 章。-->

```sql
DESCRIBE student;	-- SHOW COLUMNS FROM student; 等价语句
```

**其它 SHOW语句：**

```sql
SHOW STATUS;	-- 显示广泛的服务器状态信息
SHOW CREATE DATABASE;	-- 显示创建特定数据库
SHOW CREATE TABLE;		-- 显示创建特定表
SHOW GRANTS;	-- 显示授予用户（所有用户或特定用户）的安全权限；
SHOW ERRORS;	-- 显示服务器错误信息
SHOW WARNINGS;	-- 显示服务器警告消息
HELP SHOW;		-- 显示允许的 SHOW 语句
```

:zap: mysql 5 新增支持一个 INFORMA-TION_SCHEMA 命令，可用来获得和过滤模式信息。

# 第 4 章 检索数据

## 4.1 SELECT 语句使用

:memo: `选择什么 + 从哪选择`

`SELECT` 语句是最常用的 sql 语句，用于从一个或多个表中检索信息。

```sql
-- 1. 单列检索
SELECT name SELECT student;	-- 从 student 表中检索一个名为 name 的列 
```

上面的命令没有进行数据**过滤**和数据**排序**。

```sql
-- 2. 列检索
SELECT id, age FROM student;	-- 从 student 表中检索名为 id 和 age 的2列 
```

`SELECT` 可用于多列检索，此时需在列名之间加上逗号，但最后一个列名后不加。

```sql
-- 3. 所有列检索
 SELECT * FROM student;		-- 在实际列名的位置使用星号 * 通配符
```

:bulb: 通配符：通配符 ` * ` 的使用可让自己省事，缺点是：检索不需要的列会**降低检索和应用程序的性能**。优点是：可以检索名字未知的列（有时你可能忘记的列名）。

```sql
-- 4. 检索列的不同行
SELECT DISTINCT age FROM student;		-- 注意和 SELECT age FROM student; 的区别
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_4.png" width="290"/> </div><br>

分析：`SELECT DISTINCT age` 指示MySQL只返回唯一的 age 行。

:zap: 不能部分使用 `DISTINCT`，`DISTINCT` 关键字应用于所有列。例如：<u>仍然检索了所有的行。</u>

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_5.png" width="800"/> </div><br>

## 4.6 SELECT 限制结果

```sql
-- 5. 限制检索的行数
SELECT name FROM student LIMIT 3;
```

上述语句使用 `SELECT` 语句检索单个列，`LIMIT 3` 指示 MySQL 返回不多于 3行。

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_6.png" width="380"/> </div><br>

```sql
-- 6. 从某行开始限制检索的行数
SELECT name FROM student LIMIT 3,3;
SELECT name FROM student LIMIT 4 OFFSET 3;	-- 从行3开始取4行
```

`LIMIT 3,3` 指示 MySQL 返回从**第 3 行**开始的 3 行（最多给我返回 3 行），可以理解检索出来的**第 1 行为行 0**；

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_7.png" width="380"/> </div><br>

## 4.7 使用完全限定的表名

上面的是通过列名引用列，也可以同时使用表名和列字来引用列。

```sql
SELECT student.name FROM student;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_8.png" width="380"/> </div><br>

表名也可以是完全限定的，如下为查询数据库 school 中的 student 表。

```sql
SELECT student.name FROM school.student;	-- 结果与上面完全一致
```



# 第 5 章 排序检索数据

使用 `SELECT` 语句的 `ORDER BY` 子句，根据需要排序检索出的数据。

## 5.1 排序数据

如果不对数据进行排序，数据一般以它在底层表中出现的顺序显示（如添加到表中的顺序）。但是，如果数据后来进行过更新或删除，则此顺序将会受到 **MySQL 重用回收存储空间**的影响。

:zap: 依据关系数据库设计理论：如果不明确规定排序顺序，则假定检索出的数据的顺序无意义。

:star: 子句 ：SQL语句由子句构成，有些子句是必需的，而有的是可选的。如 `SELECT ` 语句的 `FROM` 子句。

`SELECT ` 语句结合 `ORDER BY` 子句进行排序，`ORDER BY` 子句取一个或多个列的名字。

```sql
-- 按照选择列进行排序
SELECT name FROM student ORDER BY name;	-- 对 name 列以字母顺序排序name 列数据
-- 按照非选择列进行排序
SELECT name FROM student ORDER BY age;	-- 优先按 age，age 相同则按表中原顺序
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_9" width="380"/> </div><br>

<center><font color = "blue">按照选择列进行排序</color></center>

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_10" width="380"/> </div><br>

<center><font color = "blue">按照非选择列进行排序</color></center>

## 5.2 按多个列排序

下面的 SQL 语句检索 3 个列，并按其中两个列对结果进行排序，首先按照 `age`，然后按照 `name ` 进行排序。

```sql
SELECT id, name, age FROM student ORDER BY age, name;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_11" width="500"/> </div><br>

<!--仅当具有相同的 age 时，才会对产品按照 name 进行排序。-->



## 5.3 指定排序方向

本章上述 SQL 语句是默认按照升序 （A-Z）进行排序，通过 `ORDER BY` 子句结合关键字 `DESC` 可以按照降序（Z-A）进行排序。

```sql
 SELECT id, name, age FROM student ORDER BY age DESC; -- 按照 age 降序排序
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_12" width="500"/> </div><br>



```sql
SELECT id, name, age FROM student ORDER BY age DESC, name; -- 先按 age 降序排序，然后相同 age 默认按照 name 升序排序
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_13" width="550"/> </div><br>

:zap: `DESC` 关键字只作用到位于其前面的列名（**一次性的**），若想在多个列上进行降序排序，必须对每个列指定 `DESC` 关键字。

:zap: `ASC` 关键字让指定列按照升序排序（没用，因为**默认就是升序**），使用方法与 `DESC` 一样。



:question: 区分大小写和排序顺序：在对文本性的数据进行排序时，A与 a相同吗？a位于B之前还是位于Z之后？这取决于**数据库如何设置**。MySQL 在进行字典排序时，默认 A 和 a 相等。实际如果需要，`ORDER BY` 子句无法解决，需要数据库管理员操作。

应用：使用 `ORDER BY` 和 `LIMIT` 组合，找出 `student ` 表中 `age` 的最大值。

```sql
SELECT age FROM student ORDER BY age DESC LIMIT 1;
```



# 第 6 章 过滤数据

使用 `SELECT` 语句的 `WHERE` 子句指定搜索条件，搜索条件（search criteria）也称为 过滤条件（filter condition）。

## 6.1 使用WHERE子句

```sql
-- 相等性测试：检查一个列是否具有指定的值，并据此进行过滤
-- 从student表中检索3个列，只返回 age = 22 的行
SELECT id, NAME, age FROM student WHERE age = 22;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_14" width="480"/> </div><br>

:star: SQL 过滤与应用过滤

数据可以在应用层过滤，SQL 的 `SELECT` 语句为客户机应用检索出超过实际所需的数据，然后客户机代码对返回数据进行循环，以提取出需要的行。在客户机上过滤数据，服务器要通过网络发送多余的数据，这会浪费网络带宽。

:warning: 同时使用 `WHERE 和 ORDER BY` 子句时，`WHERE` 子句需要在前面，否则会出错。



## 6.2 WHERE 子句操作符

<center><font color = "black">表6-1 WHERE子句操作符</color></center>

| 操作符  |        说明        |
| :-----: | :----------------: |
|    =    |        等于        |
|   <>    |       不等于       |
|   !=    |       不等于       |
|    <    |        小于        |
|   <=    |      小于等于      |
|    >    |        大于        |
|   >=    |      大于等于      |
| BETWEEN | 在指定的两个值之间 |



:one: 检查单个值

```sql
-- 返回 name 值为 Bob 的一行
SELECT id, name, age FROM student WHERE name = 'Bob';	-- 可以为bob，MySQL 执行匹配时默认不区分大小写
-- 返回 age ＜ 20 的所有人
SELECT id, name, age FROM student WHERE age < 20;
-- 语法类似，返回 age ≤ 20 的所有人
```

:two: 不匹配检查 `<> 或者 !=`

```sql
-- 查询 id 号不是 1 的学生
SELECT id, name, age FROM student WHERE id <> 1;			-- != 符号等价
-- 查询 name 不是 Rock 的学生
SELECT id, name, age FROM student WHERE name <> 'Rock';		-- != 符号等价
```



:bulb: **单引号用来限定字符串**，与数值列进行比较的值不用引号。



:three: 范围值检查 `BETWEEN`

 `BETWEEN` 操作符需要两个值，范围开始值和结束值。

```sql
SELECT id, name, age FROM student WHERE age BETWEEN 18 AND 20;
```



:four: 空值检查

:star: NULL 空值：列中不包含值，它与字段包含 0、空字符串或仅仅包含空格不同。

```sql
SELECT id, name, age FROM student WHERE name IS NULL;
```

:warning: NULL 与不匹配：在过滤数据时，要验证返回数据中被过滤列具有 NULL 的行。



# 第 7 章 数据过滤

`WHERE `子句组合进行高级搜索，`NOT` 和 `IN` 操作符。

## 7.1 组合 WHERE 子句

:one: `AND `操作符：用在 WHERE 子句中的关键字，对多个列进行数据过滤。

```sql
SELECT id, name, age FROM student WHERE id = 2 AND age <= 22;
-- 遵循的添加格式：过滤条件1 AND 过滤条件2 AND 过滤条件3
```

:two: `OR ` 操作符：指示 MySQL 检索匹配满足任一条件的行。

```sql
SELECT id, name, age FROM student WHERE id = 2 OR id = 3;
```

:three: `AND` 和 `OR` 操作符组合，`AND ` <u>优先级更高</u>。

```sql
SELECT id, name, age FROM student WHERE id = 1 OR id = 3 AND age >= 20;
SELECT id, name, age FROM student WHERE (id = 1 OR id = 3) AND age >= 20;	-- 建议的 SQL 语法
```

## 7.2 IN 操作符

`IN` 操作符：指定条件范围，范围内的每个条件都可以进行匹配。

```sql
SELECT id, name, age FROM student WHERE id IN (1, 2, 3) ORDER BY name;
-- OR 等价表达
SELECT id, name, age FROM student WHERE id = 1 OR id = 2 OR id = 3 ORDER BY name;
```

`IN` 和 `OR` 的区别：

- `IN` 操作符语法更清楚直观，易于管理；
- 相比于 `OR` ，**`IN` 操作符执行更快**；
- 最大优点：可以包含其他 `SELECT` 语句，使得能够动态建立 `WHERE` 子句。

<!--见第14 章-->

## 7.3 NOT 操作符

否定它之后所跟的任何条件。

```sql
-- 匹配 id = 1,2,3 之外的其它行
SELECT id, name, age FROM student WHERE id NOT IN(1, 2, 3) ORDER BY name;
```

:zap: MySQL 中的 `NOT` ：不同于多数 DBMS，MySQL 支持使用 `NOT` 对 `IN、BETWEEN 和 EXISTS`  子句取反。

 

# 第 8 章 用通配符进行过滤

通配符定义及其使用，如何使用 `LIKE` 操作符进行通配搜索。`LIKE` 指示 MySQL 后跟的搜索模式利用通配符匹配。

## 8.1 LIKE 操作符

:star: **通配符**：用来匹配值的一部分的特殊字符。

:star: 搜索模式：由字面值、通配符或两者组合构成的搜索条件。

:star: **谓词**：操作符作为谓词时就不是操作符。



:one: **百分号 `%` 通配符**（最常用）

`%`  **匹配多个任何字符（0，1，多个）**。

```sql
-- 检索以 o 开头的任意名字
SELECT id, name, age FROM student WHERE name LIKE 'o%';
-- 通配符可在搜索模式任意位置使用，且可使用多次
SELECT id, name, age FROM student WHERE name LIKE '%i%';
-- 通配符出现在字符中间
SELECT id, name, age FROM student WHERE name LIKE 'b%B';
```

:warning: 注意尾空格：尾空格会干扰通配符匹配，如果保存词 `Bob` 之后还有多个空格，那 `‘%Bob’` 就无法匹配它们，解决方法就是在搜索模式最后附加一个 `%` ，或者使用函数去掉首尾空格。

<!--第 11 章-->

:warning: 注意 `NULL` ：`WHERE name LIKE '%'` 不能匹配值为 `NULL` 作为产品名的行。



:two: 下划线 `_` 通配符

`_`  **匹配单个任何字符**。

```sql
 SELECT id, name, age FROM student WHERE name LIKE 'Bo_';
```



## 8.2 通配符

相比于前面讨论的其他搜索，通常**通配符搜索的处理耗时更长**。其使用技巧：

1. <u>通配符</u>和<u>其他操作符</u>都能做到的情况下，优先使用其他操作符；
2.  尽量避免将通配符置于搜索模式的开始处，因为这时搜索起来最慢；
3. 注意通配符的位置。



# 第 9 章 用正则表达式进行搜索

在MySQL `WHERE`子句内使用正则表达式来更好地控制数据过滤。

:star: 正则表达式的作用是：**匹配文本**，将一个正则表达式与一个文本串进行比较。正则表达式用正则表达式语言来建立。

<!--拓展知识：《正则表达式必知必会》-->

## 9.2 使用MySQL正则表达式

:one: 基本字符匹配

```sql
-- 检索列 name 包含文本 Bob 的所有行
SELECT id, name FROM student WHERE name REGEXP 'Bob' ORDER BY name;
```

分析：指示MySQL：`REGEXP` 后面所跟的东西作为一个正则表达式处理。

```sql
-- 进阶表达
SELECT id, name FROM student WHERE name REGEXP '.ob' ORDER BY name;
```

分析：`.`  **匹配任意一个字符**，当然也可用 `LIKE` 进行替换。(是正则表达式中的特殊字符)

:bulb: `LIKE ` 与 `REGEXP` 之间的差别：

- `LIKE` 匹配整个列。如果被匹配的文本在列值中出现（作为一部分），`LIKE` 就找不到它（<u>除非使用通配符</u> `^` 和 `$` 定位符）。
- `REGEXP` 在列值内进行匹配，如果被匹配的文本在列值中出现，`REGEXP` 能找到它。

```sql
SELECT id, name FROM student WHERE name LIKE '100' ORDER BY name;	-- LIKE
SELECT id, name FROM student WHERE name REGEXP '100' ORDER BY name;		-- REGEXP
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_15" width="600"/> </div><br>

:zap: 使得 `REGEXP` 和`LIKE ` 作用相同

```sql
-- 本章最后一节中，^开始每个表达式，$结束每个表达式
SELECT id, name FROM student WHERE name REGEXP '^100$' ORDER BY name;
```



:warning: MySQL中的**正则表达式匹配默认不区分大小写**，引入关键字 `BINARY` 之后就会区分大小写。

```sql
-- BINARY 关键字
SELECT name FROM student WHERE name REGEXP BINARY 'JetPack';
```



:two: 进行OR匹配 `|`

```sql
-- age 为 18 或者为 20 的行
SELECT id, name, age FROM student WHERE age REGEXP '18|20' ORDER BY name;
-- 两个以上 OR 条件，注意中间无空格
-- '18|20|30'
```

:three: 匹配几个字符之一

```sql
-- [123] 匹配特定的字符123
SELECT id, name, age FROM student WHERE name REGEXP 'SELECT id, name, age FROM student WHERE name REGEXP '[123] Bob' ORDER BY name;' ORDER BY name;
-- [] 是另一种形式的 OR语句，等价于[1|2|3] Bob
-- '1|2|3 Bob' 的意思是 '1'或者'2'或者'3 Bob'

--  '[^12] Bob' 匹配除这些字符外的任何东西，即返回 3 Bob
```

分析：正则表达式 `[123] Bob` （等价于`[1|2|3] Bob`）中 `[123]` 指示匹配 1 或 2 或 3。因此返回（没有 name 为 3 Bob）：

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_16" width="700"/> </div><br>

:four: 匹配范围 `-`

范围不限于完整的集合，范围也可以是字符。

```sql
-- 集合用来定义要匹配的一个或者多个字符
-- [123456] 等价于 [1-6]
-- [a-z]
SELECT id, name, age FROM student WHERE name REGEXP '[1-5] Bob' ORDER BY age;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_17" width="700"/> </div><br>

:five: 匹配特殊字符 `\\` 

```sql
-- 转义	\\-匹配-	\\.匹配.	\\\匹配\
SELECT id, name, age FROM student WHERE name REGEXP '\\.' ORDER BY id;
```

<center><font color = "black">表9-1 空白元字符</color></center>

| 元字符 |   说明   |
| :----: | :------: |
| `\\f`  |   换页   |
| `\\n`  |   换行   |
| `\\r`  |   回车   |
| `\\t`  |   制表   |
| `\\v`  | 纵向制表 |

:bulb: MySQL 使用两个反斜杠，MySQL 自己解释一个，正则表达式库解释另一个。

:six: 匹配字符类（预定义的字符集）

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_18" width="800"/> </div><br>

:seven: 匹配多个实例

之前介绍的正则表达式若存在一个匹配，检索该行；若不存在，无检索结果。

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_19" width="800"/> </div><br>

```sql
-- Bobs? 匹配 Bob和Bobs (s后的？使s可选,因为?匹配它前面的任何字符0次或1次)
SELECT id, name, age FROM student WHERE name REGEXP '\\([0-9] Bobs?\\)' ORDER BY id;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_20" width="750"/> </div><br>

```sql
-- 匹配连在一起的 4 个数字
-- [:digit:]匹配任意数字，{4} 要求它前面的字符出现4次。
SELECT id, name, age FROM student WHERE name REGEXP '[[:digit:]]{4}' ORDER BY id;

-- 等价表达
-- SELECT id, name, age FROM student WHERE name REGEXP '[0-9]{4}' ORDER BY id;
-- SELECT id, name, age FROM student WHERE name REGEXP '[0-9][0-9][0-9][0-9]' ORDER BY id;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_21" /> </div><br>

:eight: 定位符

上述例子都是匹配一个串中任意位置的文本，下列的定位符可以匹配特定位置的文本。

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_22" width="750"/> </div><br>

```sql
-- 检索 name 列中开头为数字或者. 的行  
SELECT id, name, age FROM student WHERE name REGEXP '^[0-9\\.]' ORDER BY id;
```

:zap: `^` 双重用途：

- 在集合中表示否定该集合，如`[^123]`
- 指示串的开始处

:zap: 使得 `REGEXP` 和`LIKE ` 作用相同

```sql
-- 本章最后一节中，^开始每个表达式，$结束每个表达式
SELECT id, name FROM student WHERE name REGEXP '^100$' ORDER BY name;
```

:bulb: 正则表达式测试

`REGEXP` 检查返回 0（没有匹配）或1（匹配）。

```sql
SELECT 'hello' REGEXP '[0-9]';		-- hello 中没有数字，返回0
```



# 第 10 章 创建计算字段

表中存储的数据不一定是应用程序所需的，我们需要从数据库中检索出转换、计算或格式化过的数据。

:star: **字段（field）**：字段和列（column）的意思基本相同。在数据库中，列一般称为列，而术语字段通常用在计算字段的连接上。

## 10.2 拼接字段

:bulb: 多数DBMS 使用 `+` 或 `||` 来实现拼接，MySQL 使用 `Concat()` 函数实现。

```sql
-- Concat()拼接串,各个串之间使用逗号分隔
SELECT Concat(name, '(', age, ')') FROM student ORDER BY name;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_23" width="650"/> </div><br>

```sql
-- 删除数据右侧多余的空格，RTrim() 函数
SELECT Concat(RTrim(name), '(', RTrim(age), ')') FROM student ORDER BY name;
-- 删除数据左侧多余的空格，LTrim() 函数
SELECT Concat(LTrim(name), '(', LTrim(age), ')') FROM student ORDER BY name;
```

**使用别名**

目的：`SELECT` 语句结合`Concat()` 函数拼接地址字段得到的 "列名字" 是一个值，这样未命名的列不能用于客户机应用。

:star: **别名**：一个字段或值的替换名，用 `AS` 关键字赋予，又称为导出列。

```sql
-- AS stu_title 指示SQL 创建一个名为 stu_title 的列，客户机应用可以按名引用这个列
SELECT Concat(RTrim(name), ' (', RTrim(age), ')  ') AS stu_title FROM student ORDER BY name;
```



## 10.3 执行算术计算

下表给出 MySQL 支持的算术操作符，圆括号可以用来区分优先顺序。

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_24" width="800"/> </div><br>

```sql
-- 使用举例
 SELECT id, quantity, item_price, quantity*item_price AS expanded_price FROM orderitems WHERE id = 1;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_25" width="980"/> </div><br>

# 第11章 使用数据处理函数

### 11.2.1 文本处理函数

```sql
-- Upper()函数：小写转换为大写
SELECT id, name, Upper(name) AS Name FROM student ORDER BY id;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_26" width="700"/> </div><br>

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_27" width="700"/> </div><br>

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_28" width="700"/> </div><br>

:star: 表中 `SOUNDEX` ：将任何文本串转换为描述其语音表示的字母数字模式的算法。`SOUNDEX` 考虑了类似的发音字符和音节，使得能对串进行发音比较而不是字母比较。虽然 `SOUNDEX` 不是 SQL 概念，但 MySQL（就像多数 DBMS 一样）都提供对 `SOUNDEX ` 的支持。

```sql
-- Soundex() 函数，匹配所有发音类似于***的联系名
```

### 11.2.2 日期和时间处理函数

- 用日期进行数据过滤；
  - 日期格式必须为 yyyy-mm-dd，如 2021-06-08；

:bulb: 如果需要日期，则使用 `Date()` 函数；需要使用时间，则使用 `Time()` 函数。

```sql
SELECT id, name, age FROM student WHERE Date(enroll_date) = '2021-06-08';
-- 检索出 2021年6月入学的所有学生
SELECT id, name, age FROM student WHERE Date(enroll_date) BETWEEN '2021-06-01' AND '2021-06-3O';
-- 检索出 2021年6月入学的所有学生，不需要记住每个月有多少天也不需要考虑闰年2月的方法
SELECT id, name, age FROM student WHERE Year(enroll_date) = 2021 AND Month(enroll_date) = 6;

```



<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_29" width="700"/> </div><br>



### 11.2.3 数值处理函数

数值处理函数仅仅处理**数值数据**，这些函数主要用于代数、三角或几何运算。

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_30" width="800"/> </div><br>



# 第 12 章 汇总数据

:star: 聚集函数（aggregate function）：运行在行组上，计算和返回单个值的函数。

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_31" width="800"/> </div><br>

### 12.1.1 AVG()函数

`AVG()` 对表中行数计数并计算特定列值之和，求得该列的平均值。`AVG()` 用来返回所有列的平均值，也可以用来返回特定列或行的平均值。

- `AVG()` 只能用来确定特定数值列的平均值，列名必须作为函数参数给出。为了获得多个列的平均值，必须使用多个 `AVG()` 函数。
- `AVG()` 函数忽略列值为 `NULL` 的行。

```sql
-- AVG() 返回student 表中所有学生的平均年龄
SELECT AVG(age) AS avg_age FROM student;	-- SELECT语句返回student 表中所有学生的平均年龄avg_age，avg_age 是别名

SELECT AVG(age) AS avg_age FROM student WHERE id BETWEEN 1 AND 8;
```



### 12.1.2 COUNT()函数

`COUNT()` 确定表中行的数目或符合特定条件的行的数目。

- 使用 `COUNT(*)` 对表中行的数目进行计数，且**不忽略 NULL 值**；
- 使用 `COUNT(column)` 对特定列中具有值的行进行计数，**忽略 `NULL` 值**；

```sql
-- 返回 student 表中学生的总数
SELECT COUNT(*) AS num_stud FROM student;
-- 对 student 表中，age 列中有值的行进行计数，忽略NULL 值
SELECT COUNT(age) AS num_stud FROM student;

```

### 12.1.3 MAX()函数

`MAX()` 函数：返回指定列中的最大值，如数值或者日期值。

- `MAX` 函数忽略列值为 `NULL` 的行；
- 对非数值数据使用 `MAX()`，当**用于文本数据时**，则 `MAX()` **返回（排序后数据列）最后一行**；

```sql
-- 返回 student 表中年龄最大的学生
SELECT MAX(age) AS max_age FROM student;
```

### 12.1.4 MIN()函数

`MIN()` 函数：返回指定列的最小值。

- `MIN` 函数忽略列值为 `NULL` 的行；
- 对非数值数据使用 `MIN()`，当**用于文本数据时**，则 `MIN()` **返回（排序后数据列）首行**；

```sql
-- 返回 student 表中年龄最小的学生
SELECT MIN(age) AS min_age FROM student;
```

### 12.1.5 SUM()函数

`SUM()` 函数：返回指定列的总和。

- `SUM()` 函数忽略列值为 `NULL` 的行；

```sql
-- 返回 orderitems 表中订单物品数量之和
SELECT SUM(quantity) AS items_ordered FROM orderitems;
-- 返回 orderitems 表中订单金额之和
SELECT SUM(quantity*item_price) AS total_price FROM orderitems;
```



:bulb: 利用标准的算术操作符，所有的聚集函数都可以用来执行多个列上的计算；



## 12.2 聚集不同值

以上 5 个聚集函数都可以按照如下规则使用：

- 对所有的行执行计算，指定 ALL 参数或者不给参数（ALL 是默认行为）；

- 只包含不同的值，指定 DISTINCT 参数；

:warning: DISTINCT 参数使用：

- 如果不指定 DISTINCT 参数，则默认为 ALL 参数；
- 如果指定列名，则 DISTINCT  只能用于 `COUNT()` ；
- DISTINCT 不能用于 `COUNT(*)` ，因此不允许使用 `COUNT(DISTINCT)`；
- DISTINCT 必须使用列名，不能用于计算或表达式；
- DISTINCT 用于 `MAX()` 或者 `MIN()` 语法合法，但是无实际意义；

 

```sql
-- AVG 函数返回 student 中学生平均年龄，但是使用了 DISTINCT 参数之后，平均值只考虑各个不同的年龄
SELECT AVG(DISTINCT age) AS avg_age FROM student;
```



## 12.3 组合聚集函数

:bulb:  在指定别名以包含某个聚集函数的结果时，不要使用表中实际列名。虽然这样做合法，但使用唯一的名字会可让你的 SQL 易于理解。

```sql
-- 单条 SELECT 语句执行了 4 个聚集计算
SELECT COUNT(*) AS num_items,
		MIN(item_price) AS price_min,
		MAX(item_price) AS price_max,
		AVG(item_price) AS price_avg FROM orderitems;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_32" width="475"/> </div><br>

# 第 13 章 分组数据

`GROUP BY` 子句和 `HAVING` 子句

## 13.2 创建分组

```sql
 -- GROUP BY 子句
 SELECT age, COUNT(*) AS people FROM student GROUP BY age;
```

分析：`SELECT` 语句指定了两个列，age 包含学生的年龄，people 为计算字段（ 用 `COUNT(*)` ） 函数建立。`GROUP BY` 子句指示 MySQL 按 age 排序并分组数据。

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_33" width="600"/> </div><br>

**GROUP BY子句：**

- `GROUP BY` 子句可包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。

- 若在 `GROUP BY` 子句中嵌套了分组，数据将在最后规定的分组上

  进行汇总。

- `GROUP BY` 子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）。如果在 `SELECT ` 中使用表达式，则必须在 `GROUP BY` 子句中指定相同的表达式。且不能使用别名。

- 除聚集计算语句外，`SELECT` 语句中的每个列都必须在 `GROUP BY` 子句中给出。
- 如果分组列中具有 `NULL` 值，则 `NULL` 将作为一个分组返回。如果列中有多行 `NULL` 值，它们将分为一组。

- `GROUP BY` 子句必须出现在 `WHERE` 子句之后，`ORDER BY` 子句之前。



:bulb: 使用 `ROLLUP` ：使用 `WITH ROLLUP` 关键字，可以得到每个分组以及每个分组汇总级别（针对每个分组）的值。

```sql
SELECT age, COUNT(*) AS people FROM student GROUP BY age WITH ROLLUP;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_34" width="800"/> </div><br>

## 13.3 过滤分组

`HAVING` 非常类似于 `WHERE` 。事实上，目前为止所学过的所有类型的 `WHERE` 子句都可以用 `HAVING` 来替代，句法相同，只是关键字有差别。**唯一的差别是 `WHERE` 过滤行，而 `HAVING` 过滤分组。**

- `WHERE` 在数据分组前进行过滤，`HAVING` 在数据分组后进行过滤。`WHERE` 排除的行不包括在分组中，这可能会改变计算值，从而影响 `HAVING` 子句中基于这些值过滤掉的分组。

```sql
-- 过滤 COUNT(*) >= 3 (分组中至少有3个以上年龄相同的组)
SELECT age, COUNT(*) AS people FROM student GROUP BY age HAVING COUNT(*) >= 3;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_35" width="870"/> </div><br>

```sql
-- 同时使用 WHERE 和 HAVING 子句,只对前 10 个 id 进行分组。
SELECT age, COUNT(*) AS people FROM student WHERE id <= 10 GROUP BY age HAVING COUNT(*) >= 3;
```



## 13.4 分组和排序

虽然 `GROUP BY` 和 `ORDER BY` 经常完成相同的工作，但它们之间的差异如下：

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_36" width="870"/> </div><br>

:bulb: 一般在使用  `GROUP BY` 子句时，应该也给出 `ORDER BY`  子句来保证数据正确排序。

```sql
-- GROUP BY 子句用来按年龄分组数据，HAVING 子句过滤数据，使得返回总计相同年龄的组内成员大于等于2，最后用 ORDER BY 子句排序输出。
SELECT age, COUNT(*) AS people FROM student GROUP BY age HAVING COUNT(*) >= 2 ORDER BY people;
```



## 13.5 SELECT语句中子句顺序

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_37" width="800"/> </div><br>

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_38" width="800"/> </div><br>

# 第 14 章 使用子查询

`SELECT` 语句是 SQL 的查询，上述学习的 `SELECT` 语句都是简单查询，即从单个数据库表中检索数据的单条语句。

:star: **查询**：任何 SQL 语句都是查询，但此术语一般指 `SELECT` 语句。

:star: **子查询**：嵌套在其他查询中的查询。

## 14.2 利用子查询进行过滤

在 `SELECT` 语句中，子查询总是从内向外处理。

```sql
-- 使用示例
SELECT cust_name, cust_contact
FROM customers
WHERE  cust_id IN (SELECT cust_id
                  FROM orders
                  WHERE order_num IN(SELECT order_num
                                    FROM orderitems
                                    WHERE prod_id = 'TNT2'));
```

在 `WHERE` 子句中使用子查询能写出功能很强且很灵活的 SQL 语句。对于能嵌套的子查询的数目没有限制，不过在实际使用时由于性能的限制，不能嵌套太多的子查询。

:zap: **列必须匹配**：在 `WHERE` 子句中使用子查询，应该保证 `SELECT` 语句具有与 `WHERE` 子句相同数目的列。通常，子查询将返回单个列并且与单个列匹配，但如果需要也可以使用多个列。

:zap: **子查询和性能**：使用子查询并不总是执行这种类型的数据检索的最有效的方法。

## 14.3 子查询中创建计算字段

```sql
-- SELECT 语句对 customers 表中每个客户返回 3 列：cust_name、cust_state和orders。orders 是一个计算字段，它是由圆括号中的子查询建立的。该子查询对检索出的每个客户执行一次。
-- 子查询中的 WHERE 子句，使用了完全限定列名（第4章）
SELECT	cust_name,
		cust_state,
		( SELECT COUNT(*)
        FROM orders
        WHERE orders.cust_id = customers.cust_id ) AS orders
FROM customers
ORDER BY cust_name;
```

:star: **相关子查询**：涉及外部查询的子查询。

# 第 15 章 联结表

## 15.1 联结

SQL 最强大的功能之一：利用 SQL 的 `SELECT` ，在数据检索查询的执行中联结（join）表。

联结是能执行的最重要的操作，很好地理解联结及其语法是学习SQL的一个极为重要的组成部分。



























