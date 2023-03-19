## SQL概述
**数据库（DataBase）**：通常是一个或一组文件，按照一定的格式储存一些文件的组合，是存数据的仓库  
**数据库管理系统（DataBase Management System）**：专门管理数据库中数据的软件，对数据库中的数据进行增删改查。如Oracle、SQL Server、MySql、Sybase  
**SQL**：结构化查询语言，程序员需要学习SQL语言，通过编写SQL语句，DBMS执行SQL语句，最终完成增删改查。它是一个标准，也是一门编程语言，在各种DBMS中都能使用  
### mysql常用命令
#### 查看mysql服务
计算机右键-->管理-->服务和应用程序  
mysql服务在启动之后才能使用，默认情况下为自动，即下一次重启系统时自动启动，默认端口3306  
#### MySQL服务启停
net start/stop + 服务名称 区分大小写
### 一些命令(必须使用分号结尾才能执行，换行之后再写可以接着打分号)
- 查看有哪些数据库    show databases;
- 查看当前数据库下有哪些表 show tables;
- 查看其他库中的表 show tables from + 库名;
- 选择使用某个数据库  use + 数据库名字;
- 创建数据库         create database + 数据库名字;
- 向数据库中导入表    source + 绝对路径;（注意路径中不要有中文）
- 导入表的全过程  连接数据库-选择数据库（use）-导入数据库（source）
- 展示建表时所用数据库语句 show create table + 表名;
- 查看数据库版本号     select version();
- 查看当前在用的数据库 select database();
- 删除数据库          drop database + 数据库名字;
- 查看表的结构 desc + 表名;
- 查询支持的存储引擎  show engines \G
- 终止命令输入        \c
- 退出 exit quit
- 数据库登录         Mysql -uroot -p12345 / mysql -uroot -p   (显示/不显示密码)
### 表的理解
表(table)是数据库最基本的单元,是一种结构化的文件，每张表都有特定的名称且不能重复。  
表的行被称作数据/记录（一条数据/一条记录），列叫做字段。每个字段都有字段名称、字段数据类型、字段约束、字段长度  
表还有主键/外键的概念，后面会讲  
### SQL的分类
- 数据查询语言(**DQL**-Data Query Language)  
select  
凡是带select关键字的都是查询语言
- 数据操作语言(**DML**-Data Manipulation Language)  
insert;delete;update  
凡是对表中数据进行增删改的都是这几个
- 数据定义语言(**DDL**-Data Definition Language)  
create;drop;alter  
主要操作的是表的结构不是数据
- 事务控制语言(**TCL**-Transactional Control Language)  
commit;rollback  提交，回滚
- 数据控制语言(**DCL**-Data Control Language)  
grant;revoke  授权，撤销授权  
## 查询语言
### 简单的查询语言
- 查询一个字段  
select 字段名 from 表名;
- 查询多个字段  
使用逗号隔开
- 查询所有字段  
使用星号
- 给查询的字段起别名  
select 字段 as 别名 from dept;  
只是查询时显示的名字改变了，原列表名不变。select语句不进行修改操作，只负责查询  
as可以省略为空格。如果别名中有空格？会报错，这是不符合语法的（可以用单引号括起来,可以用这种方式起中文名）  
- 列参与数学运算  
select ename,sal\*12 as yearsal from emp  
字段可以参与数学运算，直接使用数学表达式
### 条件查询
| 运算符 | 说明 |
|-------|-------|
|=|等于|
