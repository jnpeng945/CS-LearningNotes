### 1 安装以及配置信息

```mysql
// 数据库服务端软件安装
sudo apt-get install mysql-server
// 客户端软件安装
sudo apt-get install mysql-client
// 查询mysql服务状态
sudo service mysql status
// 停止MySQL服务
sudo service mysql stop
// 启动MySQL服务
sudo service mysql start
// 重启mysql服务
sudo service mysql restart
 
```

### 2 Mysql 配置

1. MySQL配置文件的路径

   ![image-20210426213623205](Figs/MySQL_1)

   ```mysql
   // 查看配置文件
   vim mysql.cnf
   // 翻页
   ctrl + f
   // 清屏
   ctrl + l
   // 退出
   :q
   // 数据库中蓝色的表示文件
   // 查看日志文件
   cat error.log
   
   ```

   

2. 配置项介绍

   - `port` 表示端口号，默认为 `3306`
   - `bind-address` 表示服务器绑定的ip，默认为 `127.0.0.1`
   - `datadir`表示数据库保存路径，默认为 `/var/lib/mysql`
   - `log_error` 表示错误日志，默认为 `/var/log/mysql/error.log`

### 3 Navicat安装以及使用

```mysql
 // 解压Navicat安装包
tar -zxvf navicat112_mysql_cs_x64.tar.gz
// cd 到Navicat安装包文件夹目录
cd navicat112_mysql_cs_x64
// 运行 navicat
./start_navicat

```

**Navicat使用**

```
// Navicat连接Mysql服务端
菜单栏新建连接——左侧栏打开新建的数据库
// 数据库的操作

```



### 4 Mysql终端指令操作

#### 4.1 登录客户端操作

```mysql
// 连接mysql服务端指令
mysql -uroot -p
// 显示当前时间
select now();
// 退出连接
exit/quit/contrl+d
```

#### 4.2 mysql数据库操作

```
// 系统原有的数据库
information_schema
mysql
performance_schema
sys

// 查看所有数据库
show databases;
// 创建数据库
create database 数据库名 charset=utf8;
// 使用数据库
use 数据库名
// 查看当前使用的数据库
select database();
// 删除数据库
drop database 数据库名
```

#### 4.3 mysql表操作

```mysql
// 查看所有当前库中所有表
show tables;
// 创建表
create table 表名(字段名字 数据类型 可选的约束条件，column1 datatype contrai,...);
// 修改表字段类型
alter table 表名 modify 列名 类型 约束；
// 删除表
drop table 表名
// 查看表结构
desc 表名；
```

**创建表的例子**：

![image-20210427110753670](Figs/MySQL_2)

**修改表**：

![image-20210427110753670](Figs/MySQL_3)

#### 4.4 mysql-CRUD操作

```sql
// 查询数据
1. 查询所有列
select * from 表名;

2. 查询指定列
select 列1,列2,.. from 表名;\

3. 增加数据
 1）全列插入：值的顺序必须和字段顺序完全一致
 insert into 表名 values(...);
 2)部分列插入：值的顺序和给出的列的顺序对应
 insert into 表名(列1...) values(值1...);
 3)部分多行插入：
 insert into 表名 values(...),(...),(...);
 4)部分列多行插入
 insert into 表名(列1...) values(值1...),(值1...),(值1...);
 
 4. 修改数据
 update 表名 set 列1=值1，列2=值2... where 条件
 update students set age = 18, gender = '女' where id = 6;
 
 5. 删除数据
 delete from 表名 where 条件
 delete from students where id=5;
```

#### 4.5 Mysql 数据-备份导出

```sql
// 备份导出-语法
mysqldump -u用户名 -p密码 数据库名字 表名字 > data.sql
// 恢复导入-语法
cd 到数据文件路径下
mysql -u用户名 -p密码
use 数据库
source data.sql
```

### 5 Python交互Mysql数据库

#### 5.1 PyMysql安装以及使用

```sql
// 安装pymysql
sudo pip3 install pymysql
// 查看安装情况
pip3 show pymysql
// 卸载pymysql
sudo pip3 uninstall pymysql

// 导包
import pymysql
// 创建和pymysql服务端的连接对象
pymysql.connect(参数列表)
// 获取游标对象
cursor = conn.cursor()
// 执行sql语句
row_count = cursor.execute(sql)
// 查询结果集
result = cursor.fetchall()
// 将增加和修改操作提交到数据库
conn.commit()
// 回滚数据
conn.rollback()
// 关闭游标对象
cursor.close()
// 关闭连接
conn.close()
```



### 其他

Navicat是一套快速的、可靠并且价格合适的数据库管理工具，适用于Linux以及Windows。可以用于对本机以及远程的Mysql等数据库进行管理。



> 黑马Mysql教程
