# 1 MySQL 安装

> [官网下载地址](https://www.mysql.com/downloads/)
>
> https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.19-winx64.zip

<!--压缩版安装快、方便。-->

1. 下载后得到zip压缩包，后放在自己的目录中，如 D:\Software\Code\mysql-5.7.19

2. 添加环境变量：我的电脑——>高级系统设置——>环境变量

3. 在 D:\Software\Code\mysql-5.7.19 下新建 MySQL 配置文件  `my.ini` 文件，注意替换路径位置

   ```ini
   [mysqld]
   basedir=D:\Software\Code\mysql-5.7.19\
   datadir=D:\Software\Code\mysql-5.7.19\data\
   port=3306
   skip-grant-tables
   ```

4. 启动管理员模式下的 cmd，并将路径切换至 MySQL 下的 bin 目录，然后输入mysqld –install (安装MySQL )

   ```basic
   cd /d D:\Software\Code\mysql-5.7.19\bin
   mysqld –install
   ```

5. 初始化数据文件

   ```sql
   mysqld --initialize-insecure --user=mysql
   ```

6. 启动 MySQL，然后用命令进入管理界面，密码可为空，这里直接键入回车

   ```sql
   net start mysql		-- 启动mysql
   mysql –u root –p	-- 进入管理界面
   ```

   ![image-20210603212331752](Figs/MySQL%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B_1.png)

7. 进入界面之后更改 root 密码

   ```sql
   update mysql.user set authentication_string=password('123456') where user='root' and Host = 'localhost';
   ```

8. 刷新权限

   ```sql
   flush privileges;
   ```

9. 修改 my.ini 文件，注释最后一句 `#skip-grant-tables` ，即注释掉跳过密码

10. 重启 mysql 就可以正常使用

    ```sql
    exit
    net stop mysql
    net start mysql
    ```

11. 连接上测试，出现以下结果表明安装成功

    ![image-20210603214852330](Figs/MySQL%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B_2.png)

    

# 2 SQLyog 安装

1. github 上搜索 SQLyog 社区版下载安装

2. 首次安装会要求新建一个连接，配置信息如下：

   ```te
   保存的连接名字：localhost		// 随便取
   MySQL Host Address：	localhost 或者 127.0.0.1
   用户名：root
   密码：安装 MySQL Server 时设置的密码，如123456
   端口：3306，安装 MySQL Server 时设置的端口
   // Ultimate 版本
   注册名：kuangshen
   注册码:8d8120df-a5c3-4989-8f47-5afc79c56e7c
   ```

   <img src="Figs/MySQL%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B_3.png" alt="image-20210604094622651" style="zoom:67%;" />

   

   安装好了之后可以在左侧看到有四个数据库，其中底下的三个为 `mysql、performance_schema、sys` 

   它们恰好就是 D:\Software\Code\mysql-5.7.19\data 目录下的 3 个数据库。

# 3 SQLyog 使用

1. 界面提示

   <img src="Figs/MySQL%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B_4.png" alt="image-20210604095956078" style="zoom: 50%;" />

2. 新建数据库 school

   <img src="Figs/MySQL%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B_5.png" alt="image-20210604100236701" style="zoom:50%;" />

   <!--每一个sqlyog 的执行操作，本质上就是对应的一个sql,可以在软件的历史记录中查看-->

3. 新建 student，字段 `id,name,age`，注意存储引擎选择 InnoDB

   ![image-20210604101350177](Figs/MySQL%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B_6.png)

4. 查看表，并往表中添加数据

   ![image-20210604101935120](Figs/MySQL%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B_7.png)

5. 命令解读

   ```sql
   net start mysql	-- 启动数据库
   net stop mysql	-- 关闭数据库
   mysql -uroot -p123456	-- 连接数据库
   update mysql.user set authentication_string=password('123456') where user='root' and Host = 'localhost';	-- 修改用户密码
   flush privileges;	-- 刷新权限
   ----------------------------------
   -- 所有的语句都使用 ; 结尾
   show databases;		-- 查看所有数据
   use school			-- 切换数据库 use 数据库名
   show tables;		-- 查看数据库中所有的表
   describe student;	-- 显示数据库中所有的表的信息
   create database westos;	-- 创建一个数据库
   
   ```

