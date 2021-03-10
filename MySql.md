# 1 相关背景

## 1.1 数据库分类

- 关系型数据库（SQL）

  MySQL、Oracle、Sql Server、DB2、SQLlite

  有固定的关系模型（即一张二维表）

- 非关系型数据库（NoSQL）

  Redis，MongDB

  对象存储，通过对象的自身的属性来决定



## 1.2 DBMS（数据库管理系统）

管理操作数据库中的数据

## 1.3 MySQL简介

百度



安装建议：

1. 不要使用exe
2. 用压缩文件



## 1.4 安装MySQL 5.7.19

1. 下载解压

2. 将bin目录配置到系统环境变量的path中

3. 新建mysql配置文件my.ini，解压版没有my.ini

   ``` ini
   [mysqld]
   basedir=D:\Program Files\Pro\mysql-5.7.19\
   datadir=D:\Program Files\Pro\mysql-5.7.19\data\
   port=3306
   skip-grant-tables
   ```

4. 启动管理员模式下的CMD，进入bin目录

   - mysqld -install 安装mysql服务
   - mysqld --initialize-insecure --user=mysql 初始化数据库文件
   - net start mysql 启动mysql服务
   - mysql -u root -p（注意p后面不能有空格）通过CMD进入mysql
   - update mysql.user set authentication_string=password('admin') where user='root' and Host='localhost'; 修改密码
   - flush privileges 刷新权限

5. 注释掉my.ini的最后一句配置

   ``` ini
   [mysqld]
   basedir=D:\Program Files\Pro\mysql-5.7.19\
   datadir=D:\Program Files\Pro\mysql-5.7.19\data\
   port=3306
   #skip-grant-tables
   ```

6. 重启mysql即可正常使用

   net stop mysql

   net start mysql

7. 注意：-p后输入密码，不需要空格

8. 想要删除mysql服务的话使用 

   sc delete mysql



## 1.6 安装SQLyog

## 1.7 连接数据库

``` sql
mysql -u root -p123456 --要使用的数据库
```

## 1.8 常用命令

``` sql
show databases;--查看所有数据库
use school;--切换到school数据库
show tables;--查看school下所有的表
describe student;--查看school下student表的表结构 或DESC student

exit 退出连接

show create database school
show create table student
--查看创建数据库，表的语句

-- 单行注释
/*
多行注释
*/
```

## 1.9 数据表的类型

|              | MYISAM | INNODB                 |
| ------------ | ------ | ---------------------- |
| 事务支持     | 不支持 | 支持                   |
| 锁           | 表锁   | 表锁、行锁             |
| 外键约束     | 不支持 | 支持                   |
| 全文索引     | 支持   | 不支持                 |
| 表空间的大小 | 较小   | 较大，约为MYISAM的两倍 |

常规使用操作：

- MYISAM 节约空间，速度较快
- INNODB 安全性高，支持事务的处理，多表多用户操作

> 在物理空间存在的位置

所有数据库文件都存在data目录下

MySQL引擎在物理文件上的区别

- INNODB
  - frm,存储表的结构
  - idb,存放索引和数据
- MYISAM
  - frm,存储表的结构
  - MYD,数据文件
  - MYI,索引文件



# 2 联表查询

思路

1. 分析需求,分析查询的字段来自哪些表

2. 确定使用哪种连接查询,一共有7种

   ![image-20210224200649868](C:\Users\10645\Desktop\MarkDown\MySql.assets\image-20210224200649868.png)

3. 确定交叉点,即两张表都有的字段



# 3 事务 Transaction

## 3.1 什么是事务

事务是应用程序种一些列严密的操作,所有操作必须成功完成,否则在每个操作种所作的所有更改都会被取消.

## 3.2 事务的四大特性

> 事务有ACID原则,原子性,一致性,隔离性,持久性

隔离级别:

1. 读未提交:可以读取其他事务未提交的数据,造成脏读,还可能会出现不可重复读以及幻读
2. 读已提交:只能读取其他事务已经提交的数据,避免了脏读,但是会造成不可重复读,幻读
3. 可重复读:避免了不可重复读,但是会造成幻读
4. 串行化:一次执行一个事务,效率低,不会出现脏读,不可重复读,幻读



# 4 数据库锁

## 4.1 锁的分类

按照锁的粒度分:

1. 表锁

   开销大,加锁快;不会出现死锁;锁冲突概率高,并发低

2. 行锁

   开销小,加锁慢;会出现死锁;锁冲突概率低,并发高

## 4.2 行锁INNODB

行锁分为：

1. 共享锁S
2. 排他锁X

INNODB种还有两种内部使用的意向锁，这两种锁都是表锁：

1. 意向共享锁

   如果事务想要给数据行加共享锁，事务在给一个数据行加共享锁之前必须获取意向共享锁

2. 意向排他锁

   如果事务想要给数据行加排他锁，事务在给一个数据行加排他锁之前必须获取意向排他锁