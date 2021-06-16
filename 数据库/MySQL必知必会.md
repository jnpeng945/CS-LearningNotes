> _《MySQL 必知必会》读书笔记

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
-- 相等性测试：检查一个列是否具有指定的值，并据此进行过滤-- 从student表中检索3个列，只返回 age = 22 的行SELECT id, NAME, age FROM student WHERE age = 22;
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
-- 返回 name 值为 Bob 的一行SELECT id, name, age FROM student WHERE name = 'Bob';	-- 可以为bob，MySQL 执行匹配时默认不区分大小写-- 返回 age ＜ 20 的所有人SELECT id, name, age FROM student WHERE age < 20;-- 语法类似，返回 age ≤ 20 的所有人
```

:two: 不匹配检查 `<> 或者 !=`

```sql
-- 查询 id 号不是 1 的学生SELECT id, name, age FROM student WHERE id <> 1;			-- != 符号等价-- 查询 name 不是 Rock 的学生SELECT id, name, age FROM student WHERE name <> 'Rock';		-- != 符号等价
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
SELECT id, name, age FROM student WHERE id = 2 AND age <= 22;-- 遵循的添加格式：过滤条件1 AND 过滤条件2 AND 过滤条件3
```

:two: `OR ` 操作符：指示 MySQL 检索匹配满足任一条件的行。

```sql
SELECT id, name, age FROM student WHERE id = 2 OR id = 3;
```

:three: `AND` 和 `OR` 操作符组合，`AND ` <u>优先级更高</u>。

```sql
SELECT id, name, age FROM student WHERE id = 1 OR id = 3 AND age >= 20;SELECT id, name, age FROM student WHERE (id = 1 OR id = 3) AND age >= 20;	-- 建议的 SQL 语法
```

## 7.2 IN 操作符

`IN` 操作符：指定条件范围，范围内的每个条件都可以进行匹配。

```sql
SELECT id, name, age FROM student WHERE id IN (1, 2, 3) ORDER BY name;-- OR 等价表达SELECT id, name, age FROM student WHERE id = 1 OR id = 2 OR id = 3 ORDER BY name;
```

`IN` 和 `OR` 的区别：

- `IN` 操作符语法更清楚直观，易于管理；
- 相比于 `OR` ，**`IN` 操作符执行更快**；
- 最大优点：可以包含其他 `SELECT` 语句，使得能够动态建立 `WHERE` 子句。

<!--见第14 章-->

## 7.3 NOT 操作符

否定它之后所跟的任何条件。

```sql
-- 匹配 id = 1,2,3 之外的其它行SELECT id, name, age FROM student WHERE id NOT IN(1, 2, 3) ORDER BY name;
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
-- 检索以 o 开头的任意名字SELECT id, name, age FROM student WHERE name LIKE 'o%';-- 通配符可在搜索模式任意位置使用，且可使用多次SELECT id, name, age FROM student WHERE name LIKE '%i%';-- 通配符出现在字符中间SELECT id, name, age FROM student WHERE name LIKE 'b%B';
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
2. 尽量避免将通配符置于搜索模式的开始处，因为这时搜索起来最慢；
3. 注意通配符的位置。



# 第 9 章 用正则表达式进行搜索

在MySQL `WHERE`子句内使用正则表达式来更好地控制数据过滤。

:star: 正则表达式的作用是：**匹配文本**，将一个正则表达式与一个文本串进行比较。正则表达式用正则表达式语言来建立。

<!--拓展知识：《正则表达式必知必会》-->

## 9.2 使用MySQL正则表达式

:one: 基本字符匹配

```sql
-- 检索列 name 包含文本 Bob 的所有行SELECT id, name FROM student WHERE name REGEXP 'Bob' ORDER BY name;
```

分析：指示MySQL：`REGEXP` 后面所跟的东西作为一个正则表达式处理。

```sql
-- 进阶表达SELECT id, name FROM student WHERE name REGEXP '.ob' ORDER BY name;
```

分析：`.`  **匹配任意一个字符**，当然也可用 `LIKE` 进行替换。(是正则表达式中的特殊字符)

:bulb: `LIKE ` 与 `REGEXP` 之间的差别：

- `LIKE` 匹配整个列。如果被匹配的文本在列值中出现（作为一部分），`LIKE` 就找不到它（<u>除非使用通配符</u> `^` 和 `$` 定位符）。
- `REGEXP` 在列值内进行匹配，如果被匹配的文本在列值中出现，`REGEXP` 能找到它。

```sql
SELECT id, name FROM student WHERE name LIKE '100' ORDER BY name;	-- LIKESELECT id, name FROM student WHERE name REGEXP '100' ORDER BY name;		-- REGEXP
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_15" width="600"/> </div><br>

:zap: 使得 `REGEXP` 和`LIKE ` 作用相同

```sql
-- 本章最后一节中，^开始每个表达式，$结束每个表达式SELECT id, name FROM student WHERE name REGEXP '^100$' ORDER BY name;
```



:warning: MySQL中的**正则表达式匹配默认不区分大小写**，引入关键字 `BINARY` 之后就会区分大小写。

```sql
-- BINARY 关键字SELECT name FROM student WHERE name REGEXP BINARY 'JetPack';
```



:two: 进行OR匹配 `|`

```sql
-- age 为 18 或者为 20 的行SELECT id, name, age FROM student WHERE age REGEXP '18|20' ORDER BY name;-- 两个以上 OR 条件，注意中间无空格-- '18|20|30'
```

:three: 匹配几个字符之一

```sql
-- [123] 匹配特定的字符123SELECT id, name, age FROM student WHERE name REGEXP 'SELECT id, name, age FROM student WHERE name REGEXP '[123] Bob' ORDER BY name;' ORDER BY name;-- [] 是另一种形式的 OR语句，等价于[1|2|3] Bob-- '1|2|3 Bob' 的意思是 '1'或者'2'或者'3 Bob'--  '[^12] Bob' 匹配除这些字符外的任何东西，即返回 3 Bob
```

分析：正则表达式 `[123] Bob` （等价于`[1|2|3] Bob`）中 `[123]` 指示匹配 1 或 2 或 3。因此返回（没有 name 为 3 Bob）：

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_16" width="700"/> </div><br>

:four: 匹配范围 `-`

范围不限于完整的集合，范围也可以是字符。

```sql
-- 集合用来定义要匹配的一个或者多个字符-- [123456] 等价于 [1-6]-- [a-z]SELECT id, name, age FROM student WHERE name REGEXP '[1-5] Bob' ORDER BY age;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_17" width="700"/> </div><br>

:five: 匹配特殊字符 `\\` 

```sql
-- 转义	\\-匹配-	\\.匹配.	\\\匹配\SELECT id, name, age FROM student WHERE name REGEXP '\\.' ORDER BY id;
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
-- Bobs? 匹配 Bob和Bobs (s后的？使s可选,因为?匹配它前面的任何字符0次或1次)SELECT id, name, age FROM student WHERE name REGEXP '\\([0-9] Bobs?\\)' ORDER BY id;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_20" width="750"/> </div><br>

```sql
-- 匹配连在一起的 4 个数字-- [:digit:]匹配任意数字，{4} 要求它前面的字符出现4次。SELECT id, name, age FROM student WHERE name REGEXP '[[:digit:]]{4}' ORDER BY id;-- 等价表达-- SELECT id, name, age FROM student WHERE name REGEXP '[0-9]{4}' ORDER BY id;-- SELECT id, name, age FROM student WHERE name REGEXP '[0-9][0-9][0-9][0-9]' ORDER BY id;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_21" /> </div><br>

:eight: 定位符

上述例子都是匹配一个串中任意位置的文本，下列的定位符可以匹配特定位置的文本。

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_22" width="750"/> </div><br>

```sql
-- 检索 name 列中开头为数字或者. 的行  SELECT id, name, age FROM student WHERE name REGEXP '^[0-9\\.]' ORDER BY id;
```

:zap: `^` 双重用途：

- 在集合中表示否定该集合，如`[^123]`
- 指示串的开始处

:zap: 使得 `REGEXP` 和`LIKE ` 作用相同

```sql
-- 本章最后一节中，^开始每个表达式，$结束每个表达式SELECT id, name FROM student WHERE name REGEXP '^100$' ORDER BY name;
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
-- Concat()拼接串,各个串之间使用逗号分隔SELECT Concat(name, '(', age, ')') FROM student ORDER BY name;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_23" width="650"/> </div><br>

```sql
-- 删除数据右侧多余的空格，RTrim() 函数SELECT Concat(RTrim(name), '(', RTrim(age), ')') FROM student ORDER BY name;-- 删除数据左侧多余的空格，LTrim() 函数SELECT Concat(LTrim(name), '(', LTrim(age), ')') FROM student ORDER BY name;
```

**使用别名**

目的：`SELECT` 语句结合`Concat()` 函数拼接地址字段得到的 "列名字" 是一个值，这样未命名的列不能用于客户机应用。

:star: **别名**：一个字段或值的替换名，用 `AS` 关键字赋予，又称为导出列。

```sql
-- AS stu_title 指示SQL 创建一个名为 stu_title 的列，客户机应用可以按名引用这个列SELECT Concat(RTrim(name), ' (', RTrim(age), ')  ') AS stu_title FROM student ORDER BY name;
```



## 10.3 执行算术计算

下表给出 MySQL 支持的算术操作符，圆括号可以用来区分优先顺序。

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_24" width="800"/> </div><br>

```sql
-- 使用举例 SELECT id, quantity, item_price, quantity*item_price AS expanded_price FROM orderitems WHERE id = 1;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_25" width="980"/> </div><br>

# 第11章 使用数据处理函数

### 11.2.1 文本处理函数

```sql
-- Upper()函数：小写转换为大写SELECT id, name, Upper(name) AS Name FROM student ORDER BY id;
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
SELECT id, name, age FROM student WHERE Date(enroll_date) = '2021-06-08';-- 检索出 2021年6月入学的所有学生SELECT id, name, age FROM student WHERE Date(enroll_date) BETWEEN '2021-06-01' AND '2021-06-3O';-- 检索出 2021年6月入学的所有学生，不需要记住每个月有多少天也不需要考虑闰年2月的方法SELECT id, name, age FROM student WHERE Year(enroll_date) = 2021 AND Month(enroll_date) = 6;
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
-- AVG() 返回student 表中所有学生的平均年龄SELECT AVG(age) AS avg_age FROM student;	-- SELECT语句返回student 表中所有学生的平均年龄avg_age，avg_age 是别名SELECT AVG(age) AS avg_age FROM student WHERE id BETWEEN 1 AND 8;
```



### 12.1.2 COUNT()函数

`COUNT()` 确定表中行的数目或符合特定条件的行的数目。

- 使用 `COUNT(*)` 对表中行的数目进行计数，且**不忽略 NULL 值**；
- 使用 `COUNT(column)` 对特定列中具有值的行进行计数，**忽略 `NULL` 值**；

```sql
-- 返回 student 表中学生的总数SELECT COUNT(*) AS num_stud FROM student;-- 对 student 表中，age 列中有值的行进行计数，忽略NULL 值SELECT COUNT(age) AS num_stud FROM student;
```

### 12.1.3 MAX()函数

`MAX()` 函数：返回指定列中的最大值，如数值或者日期值。

- `MAX` 函数忽略列值为 `NULL` 的行；
- 对非数值数据使用 `MAX()`，当**用于文本数据时**，则 `MAX()` **返回（排序后数据列）最后一行**；

```sql
-- 返回 student 表中年龄最大的学生SELECT MAX(age) AS max_age FROM student;
```

### 12.1.4 MIN()函数

`MIN()` 函数：返回指定列的最小值。

- `MIN` 函数忽略列值为 `NULL` 的行；
- 对非数值数据使用 `MIN()`，当**用于文本数据时**，则 `MIN()` **返回（排序后数据列）首行**；

```sql
-- 返回 student 表中年龄最小的学生SELECT MIN(age) AS min_age FROM student;
```

### 12.1.5 SUM()函数

`SUM()` 函数：返回指定列的总和。

- `SUM()` 函数忽略列值为 `NULL` 的行；

```sql
-- 返回 orderitems 表中订单物品数量之和SELECT SUM(quantity) AS items_ordered FROM orderitems;-- 返回 orderitems 表中订单金额之和SELECT SUM(quantity*item_price) AS total_price FROM orderitems;
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
-- AVG 函数返回 student 中学生平均年龄，但是使用了 DISTINCT 参数之后，平均值只考虑各个不同的年龄SELECT AVG(DISTINCT age) AS avg_age FROM student;
```



## 12.3 组合聚集函数

:bulb:  在指定别名以包含某个聚集函数的结果时，不要使用表中实际列名。虽然这样做合法，但使用唯一的名字会可让你的 SQL 易于理解。

```sql
-- 单条 SELECT 语句执行了 4 个聚集计算SELECT COUNT(*) AS num_items,		MIN(item_price) AS price_min,		MAX(item_price) AS price_max,		AVG(item_price) AS price_avg FROM orderitems;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_32" width="475"/> </div><br>

# 第 13 章 分组数据

`GROUP BY` 子句和 `HAVING` 子句

## 13.2 创建分组

```sql
 -- GROUP BY 子句 SELECT age, COUNT(*) AS people FROM student GROUP BY age;
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
-- 过滤 COUNT(*) >= 3 (分组中至少有3个以上年龄相同的组)SELECT age, COUNT(*) AS people FROM student GROUP BY age HAVING COUNT(*) >= 3;
```

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_35" width="870"/> </div><br>

```sql
-- 同时使用 WHERE 和 HAVING 子句,只对前 10 个 id 进行分组。SELECT age, COUNT(*) AS people FROM student WHERE id <= 10 GROUP BY age HAVING COUNT(*) >= 3;
```



## 13.4 分组和排序

虽然 `GROUP BY` 和 `ORDER BY` 经常完成相同的工作，但它们之间的差异如下：

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_36" width="870"/> </div><br>

:bulb: 一般在使用  `GROUP BY` 子句时，应该也给出 `ORDER BY`  子句来保证数据正确排序。

```sql
-- GROUP BY 子句用来按年龄分组数据，HAVING 子句过滤数据，使得返回总计相同年龄的组内成员大于等于2，最后用 ORDER BY 子句排序输出。SELECT age, COUNT(*) AS people FROM student GROUP BY age HAVING COUNT(*) >= 2 ORDER BY people;
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
-- 使用示例SELECT cust_name, cust_contactFROM customersWHERE  cust_id IN (SELECT cust_id                  FROM orders                  WHERE order_num IN(SELECT order_num                                    FROM orderitems                                    WHERE prod_id = 'TNT2'));
```

在 `WHERE` 子句中使用子查询能写出功能很强且很灵活的 SQL 语句。对于能嵌套的子查询的数目没有限制，不过在实际使用时由于性能的限制，不能嵌套太多的子查询。

:zap: **列必须匹配**：在 `WHERE` 子句中使用子查询，应该保证 `SELECT` 语句具有与 `WHERE` 子句相同数目的列。通常，子查询将返回单个列并且与单个列匹配，但如果需要也可以使用多个列。

:zap: **子查询和性能**：使用子查询并不总是执行这种类型的数据检索的最有效的方法。

## 14.3 子查询中创建计算字段

```sql
-- SELECT 语句对 customers 表中每个客户返回 3 列：cust_name、cust_state和orders。orders 是一个计算字段，它是由圆括号中的子查询建立的。该子查询对检索出的每个客户执行一次。-- 子查询中的 WHERE 子句，使用了完全限定列名（第4章）SELECT	cust_name,		cust_state,		( SELECT COUNT(*)        FROM orders        WHERE orders.cust_id = customers.cust_id ) AS ordersFROM customersORDER BY cust_name;
```

:star: **相关子查询**：涉及外部查询的子查询。

# 第 15 章 联结表

## 15.1 联结

SQL 最强大的功能之一：利用 SQL 的 `SELECT` ，在数据检索查询的执行中联结（join）表。

联结是一种机制，用来在一条 `SELECT` 语句中关联表，因此称之为联结。

:bulb: **外键**：外键为某个表中的一列，它包含另一个表的主键值，定义了两个表之间的关系。

关系数据可以有效地存储和方便地处理。因此，关系数据库的可伸缩性远比非关系数据库要好。

:bulb: **可伸缩性**：能够适应不断增加的工作量而不失败。设计良好的数据库或应用程序称之为可伸缩性好。



## 15.2 创建联结

规定要联结的所有表 + 表如何关联

```sql
-- vend_name在一个表中，prod_name 和 prod_price在另外一个表中-- WHERE 子句指示MySQL匹配vendors表中的vend_id和products表中的vend_id-- 完全限定列名，如vendors.vend_idSELECT vend_name, prod_name, prod_priceFROM vendors, productsWHERE vendors.vend_id = products.vend_idORDER BY vend_name, prod_name;
```

:bulb: **完全限定列名**：防止引用的列出现二义性错误。



联结两个表时，实际上是将第一个表中的每一行与第二个表中的每一行配对。`WHERE` 子句为过滤条件，它只包含那些匹配给定条件（联结条件）的行。没有`WHERE` 子句，第一个表中的每个行将与第二个表中的每个行配对，而不管它们逻辑上是否可以配在一起。

:bulb: **笛卡儿积**（cartesian product） ：<u>没有联结条件的表关系返回的结果</u>为笛卡儿积。检索出的行的数目将是第一个表中的行数乘以第二个表中的行数。

### 15.2.2 内部联结

等值联结（equijoin）：基于两个表之间的相等测试，也称为内部联结。

```sql
-- 15.2 创建联结 中的示例SELECT vend_name, prod_name, prod_priceFROM vendors, productsWHERE vendors.vend_id = products.vend_idORDER BY vend_name, prod_name;-- 等价表达SELECT vend_name, prod_name, prod_priceFROM vendors INNER JOIN productsON vendors.vend_id = products.vend_id;
```

<u>分析</u>：上述等价表达语句中 `SELECT` 语句中 `FROM` 子句不同，`FROM` 子句中的 `INNER JOIN` 用于指定两个表之间的关系 ，联结条件用特定的 `ON` 子句而不是 `WHERE` 子句指定。 

:spiral_notepad: ANSI SQL 规范首选 `INNER JOIN` 语法。



### 15.2.3 联结多个表

SQL 不对一条 `SELECT` 语句可以联结的表的数目进行限制。创建联结只需要首先列出所有表，然后定义表之间的关系。

```sql
-- 例：联结多个表SELECT prod_name, vend_name, prod_price, quantityFROM orderitems, products, vendorsWHERE products.vend_id = vendors.vend_id	AND orderitems.prod_id = product.prod_id	AND order_num = 20005;
```

<u>分析</u>：显示编号为 20005 的订单中的物品。订单物品存储在 orderitems 表中。每个产品按其产品 ID 存储，它引用 products 表中的产品。这些产品通过供应商 ID 联结到 vendors 表中相应的供应商，供应商 ID 存储在每个产品的记录中。这里的 `FROM ` 子句列出了3个表，而 `WHERE ` 子句定义了这两个联结条件，而第三个联结条件用来过滤出订单 20005中的物品。



:warning: **性能考虑**： MySQL 在运行时关联指定的每个表以处理联结。这种处理可能非常消耗资源，因此不要联结不必要的表。



```sql
-- 例：SELECT 语句返回订购产品 TNT2 的客户列表SELECT cust_name, cust_contactFROM customersWHERE cust_id IN (SELECT cust_id                 FROM orders                 WHERE order_num IN (SELECT order_num                                     FROM orderitems                                    WHERE prod_id = 'TNT2'));-- 等价表达：使用联结查询, 效率更高 !!!-- 使用两个联结, 3个WHERE子句条件。-- 前两个关联联结中的表，后一个过滤产品TNT2的数据。SELECT cust_name, cust_contactFROM customers, orders, orderitemsWHERE customers.cust_id = orders.cust_id	AND orderitems.order_num = orders.order_num	AND prod_id = 'TNT2';
```



# 第 16 章 创建高级联结

另外一些联结类型，并介绍如何对被联结的表使用表别名和聚集函数。

## 16.1 使用表别名

```sql
-- 给列起别名SELECT Concat(RTrim(vend_name), '(', RTrim(vend_country), ')') AS vend_titleFROM vendorsORDER BY vend_name;
```



别名：1）用于列名和计算字段；2）给表名起别名，旨在缩短 SQL 语句，并允许在单条 `SELECT` 语句中多次使用相同的表。

```sql
SELECT cust_name, cust_contactFROM customers AS c, orders AS o, orderitems AS oiWHERE c.cust_id = o.cust_id	AND oi.order_num = o.order_num	AND prod_id = 'TNT2';
```

表别名不仅能用于 `WHERE` 子句，还可以用于 `SELECT` 的列表、`ORDER BY` 子句以及语句的其他部分。表别名只在查询执行中使用，且其不返回到客户机。



## 16.2 使用不同类型的联结

自联结、自然联结和外部联结。

### 16.2.1 自联结

使用表别名的主要原因之一是：能在单条 `SELECT` 语句中不止一次引用相同的表。

```sql
-- 例如：当发现某物品（其ID为DTNTR）存在问题，因此想知道生产该物品的供应商生产的其他物品是否也存在这些问题。此查询要求首先找到生产ID为DTNTR的物品的供应商，然后找出这个供应商生产的其他物品。-- 方法1：内部 SELECT 语句返回生产ID为 DTNTR 的物品供应商的 vend_id，该ID用于外部查询的 WHERE 子句中，以便检索出这个供应商生产的所有物品。SELECT prod_id, prod_nameFROM productsWHERE vend_id = (SELECT vend_id                 FROM products                 WHERE prod_id = 'DTNTR');-- 联结的相同查询SELECT p1.prod_id, p1.prod_nameFROM products AS p1, products AS p2WHERE p1.vend_id = p2.vend_id	AND p2.prod_id = 'DTNTR';
```

**分析**：此查询中需要的两个表实际上是相同的表，因此 products 表在 `FROM` 子句中出现了两次。虽然这是完全合法的，但对 products 的引用具有二义性，因为MySQL不知道你引用的是 products 表中的哪个实例。

products 的第一次出现为别名 p1，第二次出现为别名 p2。现在可以将这些别名用作表名。例如，`SELECT` 语句使用 p1 前缀明确地给出所需列的全名。如果不这样，MySQL 将返回错误，因为分别存在两个名为prod_id、prod_name的列。MySQL不知道想要的是哪一个列（即使它们事实上是同一个列）。`WHERE`（通过匹配 p1中 的 vend_id 和p2中的 vend_id）首先联结两个表，然后按第二个表中的prod_id 过滤数据，返回所需的数据。



:bulb: **使用自联结而不用子查询**：自联结通常作为外部语句用来替代从相同表中检索数据时使用的子查询语句。虽然最终的结果是相同的，但有时候处理联结远比处理子查询快得多。

### 16.2.2 自然联结

<u>自然联结</u>：你只能选择那些唯一的列，通常是通过对表使用通配符`SELECT *`，对所有其他表的列使用明确的子集来完成。

```sql
SELECT c.*, o.order_num, o.order_date,		oi.prod_id, oi.quantity, oi.item_priceFROM customers AS c, orders AS o, orderitems AS oiWHERE c.cust_id = o.cust_id	AND oi.order_num = o.order_num	AND prod_id = 'FB';
```

**分析**：本例中，通配符只对第一个表使用，所有其他列明确列出，所有没有重复的列被检索出。

### 16.2.3 外部联结

联结包含了那些在相关表中没有关联行的行。

```sql
-- 内部联结：检索所有客户以及所有订单SELECT customers.cust_id, orders.order_numFROM customers INNER JOIN ordersON customers.cust_id = orders.cust_id;-- 外部联结：检索所有的客户，以及那些没有订单的客户-- 使用LEFT OUTER JOIN从FROM子句的左边表（customers表）中选择所有行SELECT customers.cust_id, orders.order_numFROM customers LEFT OUTER JOIN ordersON customers.cust_id = orders.cust_id;-- 为了从右边的表中选择所有行，应该使用 RIGHT OUTER JOINSELECT customers.cust_id, orders.order_numFROM customers RIGHT OUTER JOIN ordersON orders.cust_id = customers.cust_id;
```

**分析**：本条 `SELECT` 语句使用了关键字 `OUTER JOIN` 指定联结的类型（而不是在`WHERE` 子句中进行指定）。使用 `OUTER JOIN` 语法时，需要使用 `RIGHT` 或 `LEFT` 关键字指定包括其所有行的表（`RIGHT` 指出的是 `OUTER JOIN` 右边的表，而 `LEFT` 指出的是 `OUTER JOIN` 左边的表 ）。



:spiral_notepad: **没有 `*= ` 操作符**：MySQL 不支持简化字符 `*=` 和 `=*` 的使用，这两种操作符在其他 DBMS 中很流行。

:bulb: **两种基本的外部联结形式**：左外部联结和右外部联结。它们之间的唯一差别是<u>所关联的表的顺序不同</u>，也即左外部联结可通过颠倒 `FROM` 或 `WHERE` 子句中表的顺序转换为右外部联结。因此，两种类型的外部联结可互换使用。



## 16.3 使用带聚集函数的联结

```sql
-- 检索所有客户以及每个客户所下的订单数SELECT customers.cust_name,       customers.cust_id,       COUNT(orders.order_num) AS num_ordFROM customers INNER JOIN ordersON customers.cust_id = orders.cust_idGROUP BY customers.cust_id;
```

**分析**：`INNER JOIN` 关联 customers 和 orders 表。`GROUP BY` 子句按客户分组数据，因此函数调用 `COUNT (orders.order_num)` 对每个客户的订单计数为 `num_ord`。

```sql
-- 聚集函数和其他联结一起使用SELECT customers.cust_name,       customers.cust_id,       COUNT(orders.order_num) AS num_ordFROM customers LEFT OUTER JOIN ordersON customers.cust_id = orders.cust_idGROUP BY customers.cust_id;
```



## 16.4 使用联结和联结条件

联结及其使用的某些要点：

- 注意所使用的联结类型，一般我们使用内部联结；
- 注意使用正确的联结条件；
- 注意要提供联结条件；
- 在一个联结中可以包含多个表，甚至对于每个联结可以采用不同的联结类型。



# 第 17 章 组合查询

`UNION` 操作符将多条 `SELECT` 语句组合成一个结果集。

## 17.1 组合查询

多数 SQL 查询都只包含从一个或多个表中返回数据的单条SELECT语句。

MySQL 也允许执行多个查询（多条 SELECT 语句），并将结果作为单个查询结果集返回。这些组合查询通常称为**并（union）或复合查询（compound query）**。

**使用组合查询的两种基本场景**：

- 在单个查询中从不同的表返回类似结构的数据；
- 对单个表执行多个查询，按单个查询返回数据。

## 17.2 创建组合查询

`UNION` 操作符来组合数条 SQL 查询，通过`UNION` 操作符可给出多条 `SELECT` 语句，将它们的结果组合成单个结果集。

### 17.2.1 使用UNION

给出每条 `SELECT` 语句，在各条语句之间放上关键字 `UNION`。

```sql
-- UNION 指示MySQL 执行两条 SELECT 语句，并将输出组合成单个查询结果集 SELECT vend_id, prod_id, prod_priceFROM productsWHERE prod_price <= 5UNIONSELECT vend_id, prod_id, prod_priceFROM productsWHERE vend_id IN(1001, 1002);-- 等价表达SELECT vend_id, prod_id, prod_priceFROM productsWHERE prod_price <= 5	OR vend_id IN (1001, 1002);
```

上述例子中，使用 `UNION` 可能比使用 `WHERE` 子句更加复杂，但对于更复杂的过滤条件，或者从多个表中检索数据的情形，使用 `UNION` 更简单。

### 17.2.2 UNION规则

1. UNION 必须由两条或两条以上的 SELECT 语句组成，语句之间用关键字 UNION 分隔（因此，如果组合4条 SELECT 语句，将要使用3个UNION关键字）。
2. UNION 中的每个查询必须包含相同的列、表达式或聚集函数（不过各个列不需要以相同的次序列出）。
3. 列数据类型必须兼容：类型不必完全相同，但必须是 DBMS 可以隐含地转换的类型（例如，不同的数值类型或不同的日期类型）。

### 17.2.3 包含或取消重复的行

- UNION 从查询结果集中自动去除了重复的行（换句话说，它的行为与单条SELECT 语句中使用多个 WHERE 子句条件一样）。
- 如果想返回所有匹配行，可使用 UNION ALL 而不是 UNION。

:bulb: **UNION 与 WHERE**：UNION 可以完成与多个 WHERE 条件相同的工作。UNION ALL 为 UNION 的一种形式，当需要每个条件的匹配行全部出现（包括重复行），则必须使用 UNION ALL。

### 17.2.4 对组合查询结果排序

<u>SELECT 语句的输出用 ORDER BY 子句排序。在用 UNION 组合查询时，只能使用一条 ORDER BY 子句，且必须放在最后一条 SELECT 语句后。</u>对于结果集，不存在用一种方式排序一部分，而又用另一种方式排序另一部分的情况，因此不允许使用多条 ORDER BY 子句。



# 第 18 章 全文本搜索

## 18.1 理解全文本搜索

:zap: **并非所有引擎都支持全文本搜索**

- MyISAM 引擎支持全文本搜索；
- InnoDB 引擎不支持全文本搜索。

使用全文本搜索时，MySQL 不需要分别查看每个行，不需要分别分析和处理每个词。MySQL 创建指定列中各词的一个索引，搜索可以针对这些词进行。这样，MySQL 可以快速有效地决定哪些词匹配（哪些行包含它们），哪些词不匹配，它们匹配的频率，等等。

## 18.2 使用全文本搜索

为进行全文本搜索，必须索引被搜索的列，而且要随着数据的改变不断地重新索引。MySQL 会自动进行所有的索引和重新索引。

索引之后，SELECT 可与 Match() 和 Against() 一起使用以实际执行搜索。

### 18.2.1 启用全文本搜索支持

一般在**创建表时启用全文本搜索**。CREATE TABLE 语句（第21章中介绍）接受FULLTEXT 子句，它给出被索引列的一个逗号分隔的列表。

```sql
-- CREATE TABLE语句定义表productnotes并列出它所包含的列-- 这些列中有一个名为note_text的列，为了进行全文本搜索，MySQL根据子句FULLTEXT(note_text)的指示对它进行索引。CREATE TABLE productnotes(    note_id		int			NOT NULL AUTO_INCREMENT,    prod_id		char(10)	NOT NULL,    note_date	datetime	NOT NULL,    note_text	text		NULL,    PRIMARY KEY(note_id),    FULLTEXT(note_text)) ENGINE = MyISAM;
```

定义之后，MySQL 自动维护该索引。在增加、更新或者删除行时，<u>索引自动更新</u>。

:bulb: **不要在导入数据时使用 FULLTEXT** 。如果正在导入数据到一个新表，此时不应该启用 FULLTEXT 索引。应该首先导入所有数据，然后再修改表，定义FULLTEXT。<u>这样有助于更快地导入数据</u>。

### 18.2.2 进行全文本搜索

索引之后，使用两个函数 Match() 和 Against() 执行全文本搜索，其中 Match() 指定被搜索的列，Against() 指定要使用的搜索表达式。

```sql
-- SELECT语句检索单个列note_text。WHERE子句执行全文本搜索Match(note_text) 指示MySQL针对指定的列进行搜索，Against('rabbit')指定词rabbit作为搜索文本。由于有两行包含词rabbit，这两个行被返回。SELECT note_textFROM productnotesWHERE Match(note_text) Against('rabbit');
```



:bulb: **使用完整的 Match() 说明** 。传递给 Match() 的值必须与 FULLTEXT() 定义中的相同。如果指定多个列，则必须列出它们（而且次序正确）。

:bulb: ​**搜索不区分大小写**。除非使用BINARY方式（本章中没有介绍），否则全文本搜索不区分大小写。

```sql
-- 上述搜索的等价表达,但是次序不同SELECT note_textFROM productnotesWHERE note_text LIKE '%rabbit%';
```

**分析**：上述两条 `SELECT` 语句都不包含 `ORDER BY` 子句。后者（使用 `LIKE` ）返回数据未指定顺序。前者（使用全文本搜索）返回以文本匹配的良好程度排序的数据。

**文本中词靠前的行的等级值比词靠后的行的等级值高。**

**全文本搜索如何排除行（排除等级为0的行），如何排序结果（按等级以降序排序）。**

:bulb: **排序多个搜索项**：如果指定多个搜索项，则包含多数匹配的那些行将具有比包含较少词（或仅有一个匹配）的那些行高的等级值。

**全文本搜索提供了 `LIKE` 搜索不能提供的功能。而且，由于数据是索引的，全文本搜索更快。**

### 18.2.3 使用查询扩展

**查询扩展能找出可能相关的结果，即使它们并不精确包含所查找的词。**

使用查询扩展时，MySQL 对<u>数据和索引</u>进行<u>两遍扫描</u>来完成搜索：

- 首先，进行一个基本的全文本搜索，找出与搜索条件匹配的所有行；
- 其次，MySQL 检查这些匹配行并选择所有有用的词；
- 最后，MySQL 再次进行全文本搜索，这次不仅使用原来的条件，而且还使用所有有用的词。

```sql
-- 简单的全文本搜索，没有查询扩展-- 只有一行包含词anvils，因此只返回一行。SELECT note_textFROM productnotesWHERE Match(note_text) Against('anvils');-- 相同的搜索，并使用查询扩展-- 这次返回7行。第一行包含词anvils，因此等级最高。第二行与anvils无关，但因为它包含第一行中的两个词（customer和recommend），所以也被检索出来。第3行也包含这两个相同的词，但它们在文本中的位置更靠后且分开得更远，因此也包含这一行，但等级为第三。第三行确实也没有涉及anvils（按它们的产品名）。SELECT note_textFROM productnotesWHERE Match(note_text) Against('anvils' WITH QUERY EXPANSION);
```

**查询扩展增加了返回的行数，但也增加了你实际上并不想要的行数**。

:bulb: **行越多越好**，表中的行越多（这些行中的文本就越多），使用查询扩展返回的结果越好。

### 18.2.4 布尔文本搜索

**布尔方式**：MySQL 支持全文本搜索的另外一种形式，以布尔方式可以提供关于如下内容的细节：

- 要匹配的词；
- 要排斥的词（如果某行包含这个词，则不返回该行，即使它包含其他指定的词也是如此）；
- 排列提示（指定某些词比其他词更重要，更重要的词等级更高）； 
- 表达式分组；



:bulb: **即使没有 `FULLTEXT` 索引也可以使用**。这一点是布尔方式和迄今为止使用的全文本搜索语法的不同之处，但这种操作非常慢，数据量增加性能会降低。

```sql
-- IN BOOLEAN MODE的作用演示SELECT note_textFROM productnotesWHERE Match(note_text) Against('heavy' IN BOOLEAN MODE);
```

**分析**：利用关键字 `IN BOOLEAN MODE` 检索包含词 heavy 的所有行（有两行）。其中，没有指定布尔操作符，因此其结果与没有指定布尔方式的结果相同。

```sql
-- 匹配包含 heavy 但不包含任意以rope开始的词的行SELECT note_textFROM productnotesWHERE Match(note_text) Against('heavy -rope*' IN BOOLEAN MODE);
```

**分析**：上例中匹配词 heavy，但是 -rope* 明确地指示 MySQL 排除包括 rope* 的行。

如上，使用了全文本搜索布尔操作符 `-` 和 `*` ，前者排除一个词，后者是截断操作符（类似于词尾的一个通配符），下表列出了支持的所有布尔操作符。

<div align="center"> <img src="Figs/MySQL%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A_39" width="800"/> </div><br>

```sql
-- 搜索匹配包含词rabbit和bait的行SELECT note_textFROM productnotesWHERE Match(note_text) Against('+rabbit +bait' IN BOOLEAN MODE);-- 没有指定操作符，搜索匹配包含rabbit和bait中的至少一个词的行SELECT note_textFROM productnotesWHERE Match(note_text) Against('rabbit bait' IN BOOLEAN MODE);-- 匹配短语 rabbit baitSELECT note_textFROM productnotesWHERE Match(note_text) Against('"rabbit bait"' IN BOOLEAN MODE);-- 匹配rabbit 和carrot，增加前者的等级，降低后者的等级SELECT note_textFROM productnotesWHERE Match(note_text) Against('>rabbit <carrot' IN BOOLEAN MODE);-- 匹配词safe和combination，降低后者的等级SELECT note_textFROM productnotesWHERE Match(note_text) Against('+safe +(<combination)' IN BOOLEAN MODE);
```

:bulb: **排列而不排序**：在布尔方式中，不按等级值降序排序返回的行。

### 18.2.5 全文本搜索的使用说明

<u>全文本搜索的重要说明：</u>

- **在索引全文本数据时，短词被忽略且从索引中排除**。短词定义为那些具有3个或3个以下字符的词（如果需要，这个数目可以更改）。
- MySQL 带有一个内建的非用词（stopword）列表，这些词在索引全文本数据时总是被忽略。如果需要，可以覆盖这个列表。
- 许多词出现的频率很高，搜索它们没有用处（返回太多的结果）。因此，MySQL 规定了一条 50% 规则，**如果一个词出现在 50% 以上的行中，则将它作为一个非用词忽略。50% 规则不用于 IN BOOLEAN MODE。**
- **如果表中的行数少于 3 行，则全文本搜索不返回结果**（因为每个词或者不出现，或者至少出现在 50% 的行中）。
- **忽略词中的单引号**。例如 don't 索引为 dont。
- 不具有词分隔符（包括日语和汉语）的语言不能恰当地返回全文本搜索结果。
- **仅在 MyISAM 数据库引擎中支持全文本搜索**。 

:bulb: 邻近搜索也就是搜索相邻的词，它是许多全文本搜索支持的一个特性。

# 第 19 章 插入数据

`INSERT` 用来插入（或添加）行到数据库表中，且 `INSERT` 语句一般不产生输出，插入方式有如下几种：

- 插入完整的行；
- 插入行的一部分；
- 插入多行；
- 插入某些查询的结果。

:bulb: **插入及系统安全**：可以针对每个表或者每个用户，利用 MySQL 的安全机制禁止使用 `INSERT` 语句。<!--第28章-->

## 19.2 插入完整的行

```sql
-- 插入一个新客户到customers表，存储到每个表列中的数据在VALUES子句中给出，对每个列必须提供一个值。如果某列中没有值，应该使用 NULL 值。各个列必须以它们在表定义中出现的次序填充。INSERT INTO CustomersVALUES(NULL,      'Pep E. LaPew',      '100 Main Street',      'Los Angeles',      'CA',      '90046',      'USA',      NULL,      NULL);
```

**分析**：上述 SQL 语句依赖表中列的定义次序和该次序获得的信息。即使我们能得到这种次序信息，也不能保证下次表结构变动后各个列保持完全相同的次序。因此，编写依赖于特定列次序的 SQL 语句不安全。

```sql
-- 等价语法表达，更加安全的 INSERT语句INSERT INTO Customers(cust_name,                     cust_city,                     cust_state,                     cust_zip,                     cust_country,                     cust_contact,                     cust_email)            VALUES('Pep E. LaPew',                  '100 Main Street',                  'Los Angeles',                  'CA',                  '90046',                  'USA',                  NULL,                  NULL);
```

**分析**：上例在表名后的括号里明确地给出了列名。在插入行时，MySQL 将用VALUES 列表中的相应值填入列表中的对应项。

:bulb: **我们要使用明确给出列的列表的 `INSERT ` 语句。**

:bulb: 使用 `INSERT` 语法时必须给出 VALUES 的正确数目。如果不提供列名，则必须给每个表列提供一个值。**如果提供列名，则必须对每个列出的列给出一个值，但是可以省略某些列**。

:bulb: **省略列**：如果表的定义允许，可以在  `INSERT`  操作中省略某些列。省略的列需要满足以下某个条件：

- 该列定义为允许NULL值（无值或空值）；

- 在表定义中给出默认值。这表示如果不给出值，将使用默认值。

  在 `INSERT` 和 `INTO` 之间添加关键字 `LOW_PRIORITY` ，指示 MySQL 降低 `INSERT` 语句的优先级。如 `INSERT LOW_PRIORITY INTO`  ，这种技巧也可用于 `UPDATE` 和 `DELETE` 语句。

## 19.3 插入多个行

使用多条 `INSERT ` 语句，甚至一次提交，每条语句用一个分号结束。

```sql
INSERT INTO Customers(cust_name,                     cust_address,                     cust_city,                     cust_state,                     cust_zip,                     cust_country)            VALUES('Pep E. LaPew',                  '100 Main Street',                  'Los Angeles',                  'CA',                  '90046',                  'USA');INSERT INTO Customers(cust_name,                     cust_address,                     cust_city,                     cust_state,                     cust_zip,                     cust_country)            VALUES('M. Martian',                  '42 Galaxy Way',                  'New York',                  'NY',                  '11213',                  'USA');                  -- 等价表达，语句组合,单条INSERT语句有多组值，每组值用一对圆括号括起来并用逗号分隔INSERT INTO Customers(cust_name,                     cust_address,                     cust_city,                     cust_state,                     cust_zip,                     cust_country)            VALUES('Pep E. LaPew',                  '100 Main Street',                  'Los Angeles',                  'CA',                  '90046',                  'USA'),            	  ('M. Martian',                  '42 Galaxy Way',                  'New York',                  'NY',                  '11213',                  'USA');
```

上述第二种处理方式组合多条 `INSERT` 语句，可以**提高数据库处理的性能**，因为MySQL <u>用单条 INSERT 语句处理多个插入比使用多条 INSERT 语句快</u>。

## 19.4 插入检索出的数据

`INSERT ` 语句将一条 `SELECT` 语句的结果插入表中，也就是 `INSERT SELECT`，由**一条 `INSERT ` 语句和一条 `SELECT` 语句组成**。

```sql
INSERT INTO customers(cust_id,                     cust_contact,                     cust_email,                     cust_name,                     cust_address,                     cust_city,                     cust_state,                     cust_zip,                     cust_country)SELECT cust_id,	   cust_contact,	   cust_email,	   cust_name,	   cust_address,	   cust_city,	   cust_state,	   cust_zip,	   cust_countryFROM custnew;
```

**分析**：上述例子中使用 `INSERT SELECT` 从 custnew 中将所有数据导入customers。`SELECT` 语句从 custnew 检索出要插入的值，而不是列出它们。`SELECT` 中列出的每个列对应于 customers 表名后所跟的列表中的每个列。这条语句将插入多少行有赖于 custnew 表中有多少行。如果这个表为空，则没有行被插入（也不产生错误，因为操作仍然是合法的）。如果这个表确实含有数据，则所有数据将被插入到 customers。

:bulb:  **`INSERT SELECT` 中的别名**。本例中  `INSERT ` 语句和 `SELECT` 语句使用了相同相同的列名。但是，<u>不一定要求列名匹配</u>。实际上，MySQL 不必关心 `SELECT` 返回的列名。<u>它使用的是列的位置</u>，因此  `SELECT`  中的第一列用来填充表列中指定的第一个列，第二列用来填充表列中指定的第二个列，如此等等。

**`INSERT SELECT` 中 `SELECT` 语句可包含 `WHERE` 子句以过滤插入的数据。**



# 第 20 章 更新和删除数据

`UPDATE` 和 `DELETE` 语句

:warning: 不要省略 `WHERE` 子句，否则会更新表中所有行，或者删除表中所有的行。

## 20.1 更新数据

两种方式使用 `UPDATE`：

- 更新表中特定行；
- 更新表中所有行。

`UPDATE` 语句由3部分组成：1）要更新的表；2）列名和它们的新值；3）确定要更新行的过滤条件。

```sql
-- 更新客户 10005的电子邮件地址UPDATE customersSET cust_email = '13453542220@qq.com'	-- 设置cust_email 为指定值WHERE cust_id = 10005;
```

**分析**：`UPDATE` 语句以要更新的表的名字（customers）开始，`SET` 命令用来将新值赋给被更新的列，`UPDATE` 语句以 `WHERE` 子句为结束（指示 MySQL 更新哪一行）。

```sql
-- 更新客户 10005的cust_name列和cust_email列UPDATE customersSET cust_name = 'Tom',	cust_email = '13453542220@qq.com'WHERE cust_id = 10005;
```

**分析**：更新多个列时，只需要使用单个 `SET` 命令，每个 列-值 对之间用逗号分隔（最后一列之后不用逗号）。

:bulb: **在 `UPDATE` 语句中使用子查询**，使得其可以用 `SELECT` 语句检索出的数据更新列数据。

:bulb: **`IGNORE` 关键字**。如果用 `UPDATE` 语句更新多行，假设在更新这些行中的一行或多行时出一个错误，<u>则整个 `UPDATE` 操作被取消</u>（错误发生前更新的所有行被恢复到它们原来的值）。为了在即便是**发生错误的情况下也继续进行更新列数据**，可使用 `IGNORE` 关键字，如下所示： 

```sql
UPDATE IGNORE customers… 
```



**如果要删除某个列的值，可以将它设置为 `NULL` （前提是表定义允许 NULL 值）**。

```sql
-- NULL 用于去除cust_email列中的值UPDATE customersSET cust_email = NULLWHERE cust_id = 10005;
```



## 20.2 删除数据

两种方式使用 `DELETE`：

- 从表中删除特定行；
- 从表中删除所有行。

```sql
-- 从customers表中删除一行DELETE FROM customersWHERE cust_id = 10006;
```

**分析**：`DELETE FROM` 要求指定从中删除数据的表名。`WHERE` 子句过滤要删除的行。`DELETE` 不需要列名或通配符。`DELETE` 删除整行而不是删除列。若要删除指定的列，需使用 `UPDATE` 语句。



:bulb: **删除表的内容而不是表**。`DELETE` 语句从表中删除行，甚至是删除表中所有行。但是，**`DELETE` 不删除表本身**。

:bulb: 如果想从表中删除所有行，可使用 **`TRUNCATE TABLE` 语句，相比于 `DELETE` 语句它的速度更快**（TRUNCATE 实际是删除原来的表并重新创建一个表，而不是逐行删除表中的数据）。



## 20.3 更新和删除的指导原则

如果省略了 `UPDATE` 和 `DELETE` 语句中的 `WHERE` 子句，则 `UPDATE` 或 `DELETE` 将应用到表中所有的行。如下是使用 `UPDATE` 或 `DELETE` 时所遵循的 SQL 语法规则：

- 除非确实打算更新和删除每一行，否则绝对**不要使用不带 WHERE 子句的 UPDATE 或 DELETE 语句**；
- 保证**每个表都有主键**（如果忘记这个内容，请参阅第15章），尽可能像 WHERE 子句那样使用它（可以指定各主键、多个值或值的范围）；
- 在**对 UPDATE 或 DELETE 语句使用 WHERE 子句前，应该先用 SELECT 进行测试**，保证它过滤的是正确的记录，以防编写的 WHERE 子句不正确；
- **使用强制实施引用完整性的数据库**（关于这个内容，请参阅第15章），这样 MySQL 将不允许删除具有与其他表相关联的数据的行。



# 第 21 章 创建和操纵表

## 21.1 创建表

用程序创建表，也即使用 `CREATE TABLE` 语句；用交互式工具创建表，界面工具会自动生成并执行相应的 MySQL 语句。

### 21.1.1 表创建基础

为利用 `CREATE TABLE` 创建表，必须给出下列信息：

- 新表的名字，在关键字 CREATE TABLE 之后给出；

- 表列的名字和定义，用逗号分隔。

```sql
-- 创建customers表CREATE TABLE customers (    cust_id			int			NOT NULL AUTO_INCREMENT,    cust_name		char(50)	NOT NULL ,    cust_address	char(50)	NULL ,    cust_city		char(50)	NULL ,    cust_state		char(5)		NULL ,    cust_zip		char(10)	NULL ,    cust_country	char(50)	NULL ,    cust_contact	char(50)	NULL ,    cust_email		char(255)	NULL ,    PRIMARY KEY (cust_id)) ENGINE=InnoDB;
```

**分析**：`CREATE TABLE` 关键字之后为表名，表定义（所有列）括在圆括号之中，各列之间用逗号分隔。表的主键可以在创建表时用 `PRIMARY KEY` 关键字指定。整条语句由右圆括号后的分号结束。

:bulb: **语句格式化**。MySQL 语句中忽略空格，语句可以在一个长行上输入，也可以分成许多行。

**在创建新表时，指定的表名必须不存在。**如果要防止意外覆盖已有的表，SQL 要求首先手工删除该表，然后再重建它。**如果仅想在一个表不存在时创建它，应该在表名后给出 `IF NOT EXISTS` 。**这样做不检查已有表的模式是否与你打算创建的表模式相匹配。它只是查看表名是否存在，并且仅在表名不存在时创建它。

### 21.1.2 使用NULL值

**NULL 值就是没有值或缺值**。允许 NULL 值的列允许在插入行时不给出该列的值。不允许 NULL 值的列在插入或更新行时，该列必须有值。

```sql
-- 表的定义决定该表列的属性，NULL 或者 NOT NULL-- 创建 orders 表，每个列的定义都含有关键字NOT NULLCREATE TABLE orders (    order_num		int			NOT NULL AUTO_INCREMENT,    order_date		datetime	NOT NULL ,	cust_id			int			NOT NULL ,    PRIMARY KEY (order_num))ENGINE=InnoDB;-- 创建vendors 表，混合了NULL和NOT NULL列的表CREATE TABLE vendors (    vend_id			int			NOT NULL AUTO_INCREMENT,    vend_name		char(50)	NOT NULL ,    vend_address	char(50)	NULL ,    vend_city		char(50)	NULL ,    vend_state		char(5)		NULL ,    vend_zip		char(10)	NULL ,    vend_country	char(50)	NULL ,    PRIMARY	KEY (vend_id))ENGINE=InnoDB;
```



:bulb: **NULL的理解​**。<u>NULL 值是没有值，它不是空串</u>。如果指定 ' '（两个单引号，其间没有字符），这在 NOT NULL 列中是允许的。<u>空串是一个有效的值，它不是无值</u>。<u>NULL 值用关键字 NULL 而不是空串指定</u>。

### 21.1.3 主键再介绍

**主键值是唯一的**。如果主键使用单个列，则它的值必须唯一。<u>如果使用多个列，则这些列的组合值必须唯一</u>。

```sql
-- 创建多个列组成的主键，以逗号分隔的列表给出各列名CREATE TABLE orderitems (	order_num		int				NOT NULL ,    order_item		int				NOT NULL ,    prod_id			char(10)		NOT NULL ,    quantity		int				NOT NULL ,    item_price		decimal(8,2)	NOT NULL ,    PRIMARY KEY (order_num, order_item))ENGINE=InnoDB;
```



**主键可以在创建表时定义（<u>如上所示</u>），或者在创建表之后定义**。



### 21.1.4 使用AUTO_INCREMENT

```sql
-- 取自本章开始处，创建customers表cust_id			int			NOT NULL AUTO_INCREMENT,
```

**分析**：`AUTO_INCREMENT` **告诉 MySQL，本列每增加一行时会自动增量**。每次
执行一个 INSERT 操作时，MySQL 自动对该列增量，给该列赋予下一个可用的值。这样给每个行分配一个唯一的 cust_id，从而可以用作主键值。

**每个表只允许一个 `AUTO_INCREMENT` 列，而且它必须被索引**（如让它成为主键）。

```sql
-- 返回最后一个 AUTO_INCREMENT 值，然后将它用于后续的 MySQL 语句SELECT last_insert_id()
```



### 21.1.5 指定默认值

如果在插入行时没有给出值，MySQL 允许指定此时使用的默认值。**默认值用`CREATE TABLE` 语句的列定义中的 `DEFAULT` 关键字指定。**

```sql
-- quantity,订单中每项物品的数量默认为1CREATE TABLE orderitems (	order_num		int				NOT NULL ,    order_item		int				NOT NULL ,    prod_id			char(10)		NOT NULL ,    quantity		int				NOT NULL DEFAULT 1,    item_price		decimal(8,2)	NOT NULL ,    PRIMARY KEY (order_num, order_item)) ENGINE=InnoDB;
```



**MySQL 仅支持常量作为默认值**。



### 21.1.6 引擎类型

MySQL 有一个具体管理和处理数据的内部引擎，但 MySQL 与其他 DBMS 不一样，它具有多种引擎。它打包多个引擎，**这些引擎都隐藏在 MySQL 服务器内**，全都能执行  `CREATE TABLE` 和 `SELECT` 等命令。

使用 `CREATE TABLE` 语句时，该引擎具体创建表，使用 `SELECT` 语句或进行其他数据库处理时，该引擎在内部处理你的请求。

:bulb: **发行多种引擎的意义**：它们具有各自不同的功能和特性，为不同的任务选择正确的引擎能获得良好的功能和灵活性。

- **InnoDB** 是一个可靠的事务处理引擎（参见第26章），它不支持全文本搜索；
- **MEMORY** 在功能等同于 **MyISAM**，但由于数据存储在内存（不是磁盘）中，速度很快（<u>特别适合于临时表</u>）；
- **MyISAM** 是一个性能极高的引擎，它支持全文本搜索（参见第18章），但不支持事务处理。



:warning: **外键不能跨引擎**。外键（用于强制实施引用完整性，如第1章所述）不能跨引擎，即使用一个引擎的表不能引用具有使用不同引擎的表的外键。



## 21.2 更新表

使用 `ALTER TABLE` 语句**更新表定义**。

理想状态下，当表中存储数据以后，该表就不应该再被更新。

为使用 `ALTER TABLE` 更改表结构，必须给出下面的信息：

- 在 `ALTER TABLE` 之后给出要更改的表名（该表必须存在，否则将出错）；
- 所做更改的列表。

```sql
-- 给vendors表增加一个 vend_phone 列ALTER TABLE vendorsADD vend_phone CHAR(20);-- 删除刚刚添加的列ALTER TABLE vendorsDROP COLUMN vend_phone;
```



`ALTER TABLE` 的一种常见用途是**定义外键**。下面是用来定义本书中的表所用的外键的代码：

```sql
-- 更改2不同的表，使用2条ALTER TABLE语句ALTER TABLE orderitemsADD CONSTRAINT fk_orderitems_ordersFOREIGN KEY (order_num) REFERENCES orders (order_num);ALTER TABLE orderitemsADD CONSTRAINT fk_orderitems_productsFOREIGN KEY (prod_id) REFERENCES products (prod_id);...
```



**复杂的表结构更改一般需要手动删除过程**，步骤如下：

- 用新的列布局创建一个新表；
- 使用 INSERT SELECT 语句（第19章）从旧表复制数据到新表。如果有必要，可使用转换函数和计算字段；
- 检验包含所需数据的新表；
- 重命名旧表（如果确定，可以删除它）；
- 用旧表原来的名字重命名新表；
- 根据需要，重新创建触发器、存储过程、索引和外键。



:warning: **使用 ALTER TABLE 时，应该在进行改动前做一个完整的备份**（模式和数据的备份）。数据库表的更改不能撤销，如果删除了列则会丢失该列中的所有数据。



## 21.3 删除表

`DROP TABLE` 语句，删除表没有确认，也不能撤销，执行这条语句将永久删除该表。

```sql
-- 删除customers表（假设它存在）。DROP TABLE customers;
```



## 21.4 重命名表

`RENAME TABLE` 语句

```sql
-- 将customers表重新命名为customers表RENAME TABLE customers TO customers;-- 对多个表重命名RENAME TABLE backup_customers TO customers;			 backup_vendors TO vendors;			 backup_products TO products;
```



# 第 22 章 使用视图

## 22.1 视图

**视图是虚拟的表**。不同于包含数据的表，视图只包含<u>使用时动态检索数据的查询</u>。

```sql
-- SELECT语句从3个表中检索数据SELECT cust_name, cust_contactFROM customers, orders, orderitemsWHERE customers.cust_id = orders.cust_id  AND orderitems.order_num = orders.order_num  AND prod_id = 'TNT2';  -- 把整个查询包装成一个名为productcustomers的虚拟表，等价表达为SELECT cust_name, cust_contactFROM productcustomersWHERE prod_id = 'TNT2';
```

上述 productcustomers 就是一个视图，它**不包含表中应该有的任何列或数据，它包含的是一个 SQL 查询**。



### 22.1.1 为什么使用视图

- **重用SQL语句**；
- **简化SQL操作**。在编写查询后，可以方便地重用它而不必知道它的基本查询细节。
- **使用表的组成部分而不是整个表**。
- **保护数据**。可以给用户授予表的特定部分的访问权限而不是整个表的访问权限。
- **更改数据格式和表示**。视图可返回与底层表的表示和格式不同的数据。



创建视图之后，可以用与表基本相同的方式利用它们。可以对视图执行 SELECT 操作，过滤和排序数据，将视图联结到其他视图或表，甚至能添加和更新数据。



视图仅仅是用来查看存储在别处的数据的一种设施。视图本身不包含数据，因此它们返回的数据是从其他表中检索出来的。在添加或更改这些表中的数据时，视图将返回改变过的数据。



:bulb: **视图的性能** 。视图不包含数据，每次使用视图时都必须处理查询执行时所需的任一个检索。如果你用多个联结和过滤创建了复杂的视图或者嵌套了视图，性能会下降得很厉害。



### 22.1.2 视图的规则和限制

- 与表一样，**视图必须唯一命名**（不能给视图取与别的视图或表相同的名字）。
- 对于可以**创建的视图数目没有限制**。
- 为了**创建视图，必须具有足够的访问权限**。这些限制通常由数据库管理人员授予。
- **视图可以嵌套**，即可以利用从其他视图中检索数据的查询来构造一个视图。
- **`ORDER BY` 可以用在视图中**，但如果从该视图检索数据 SELECT 中也含有 ORDER BY，那么该视图中的 ORDER BY 将被覆盖。
- **视图不能索引，也不能有关联的触发器或默认值**。
- **视图可以和表一起使用**。例如，编写一条联结表和视图的 SELECT 语句。

## 22.2 使用视图

- 视图用 `CREATE VIEW` 语句来创建。
- 使用 `SHOW CREATE VIEW viewname` ；来查看创建视图的语句。
- 用 `DROP` 删除视图，其语法为 `DROP VIEW viewname`；
- 更新视图时，可以先用 `DROP` 再用 `CREATE`，也可以直接用 `CREATE OR REPLACE VIEW`。<!--如果要更新的视图不存在，则上述第2条更新语句会创建一个视图；如果要更新的视图存在，则第2条更新语句会替换原有视图。-->

### 22.2.1 利用视图简化复杂的联结

```sql
-- 创建productcustomers视图，联结三个表，返回已订购了任意产品的所有客户的列表。
-- 如果执行SELECT * FROM productcustomers，将列出订购了任意产品的客户。
CREATE VIEW productcustomers AS
SELECT cust_name, cust_contact, prod_id
FROM customers, orders, orderitems
WHERE customers.cust_id = orders.cust_id
  AND orderitems.order_num = orders.order_num;
```

为检索订购了 TNT2 产品的客户，如下：

```sql
SELECT cust_name, cust_contact
FROM productcustomers
WHERE prod_id = 'TNT2';
```

**分析**：WHERE 子句从视图中检索特定数据。在 MySQL 处理此查询时，它将指定的WHERE 子句添加到视图查询中的已有 WHERE 子句中，以便正确过滤数据。

**利用视图，可一次性编写基础的SQL，然后根据需要多次使用**。



### 22.2.2 用视图重新格式化检索出的数据







