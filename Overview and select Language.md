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
| 运算符 | 说明 | 补充|
|-------|-------|-------|
|=|等于|等于接字符串时需要用双引号/单引号括上|
|<>或!=|不等于| |
|between..and..|两个值之间，相当于>= and <=|  |
|is null|为空 is not null|在数据库中null不能使用等号进行衡量，它不是一个值|
|and or |和，或 同时出现时and的优先级更高|  |
|in，not in| 包含，不包含 |相当于多个or，接的不是一个区间而是几个具体的值|
|not|可以取非，主要用在is或in中|  |
### 条件查询举例
|运算符|要求|实现语句|
|------|-----|-------|
|=|查询薪水为5000的员工|select ename from emp where sal=5000;|
|<>|查询薪水不等于5000的员工|select ename,sal from emp where sal<>5000;|
|between..and..|查询工资在1600到3000之间的员工|select ename,sal from emp where sal>=1600 and sal<=3000;|
|is null|查询津贴为空的员工|select ename from emp where comm is null;|不能写comm = null
|and|查询工作岗位为manager，薪水大于2500的员工|select ename,sal from emp where job="manager" and sal>2500;|
|in|查询工作岗位为manager或salesman的员工|select * from emp where job in ("manager","salesman");|
|表达式的优先级|查询薪水大于 1800，并且部门代码为20 或 30 的员工|select * from emp where sal>1800 and (deptno = 20 or deptno = 30);|
|not|查询薪水不包含1600和不包含3000的员工|select * from emp where sal<>1600 and sal<>3000;|
| | |select * from emp where not {sal = 1600 or sal = 3000};|  
| | |select * from emp where sal not in(1600,3000);|
### 模糊查询like
使用like和%、\_进行模糊查询，Like中的表达式必须放在单引号或双引号中  
%匹配任意字符出现的个数，\_匹配一个字符  
如果是名字中含有下划线的，在前面加上反斜杠
|举例|实现语句|
|----|----|
|查询姓名以M开头的所有员工|select ename from emp where ename like 'M%';|
|查询姓名以N结尾的所有员工|select ename from emp where ename like '%N';|
|查询姓名中包含o的员工|select ename from emp where ename like '%O%';|
|查询姓名中第二个字母为A的员工|select ename from emp where ename like '\_A%';|

## 数据排序
排序采用order by子句，order by+排序字段，排序字段可以放多个，多个字段之间用逗号连接。  
order by默认采用升序，如果存在where子句，那么order by必须放在where后面
### 单一字段排序
默认升序
|举例|实现语句|
|----|-------|
|按照薪水由小到大排序|select * from emp order by sal;|
|工作岗位为manager的员工，薪水由小到大排序|select * from emp where job="manager" order by sal;|
|先按照job排序，再按照sal排序|select * from emp order by job,sal;|
### 手动指定排序顺序
|举例|实现语句|
|----|--------|
|手动指定薪水由小到大排序|select * from emp order by sal asc;|
|手动指定薪水由大到小排序|select * from emp order by sal desc;|
### 多个字段排序
采用多个字段排序时，第一个字段重复后会按照第二个字段排序(前一个字段占主导地位)
|举例|实现语句|
|----|--------|
|按照job和薪水倒序|select * from emp order by job desc, sal desc;|
### 使用字段的位置排序
不建议这样写，健壮性不够
|举例|实现语句|
|----|--------|
|按照薪水排序|select * from emp order by 6|

## 数据处理函数
### 单行处理函数
是一行一行处理的，特点是一个输入对应一个输出。
|关键字|举例|实现语句|补充|
|------|----|-------|------|
|Lower|查询员工并将员工姓名改为小写|select lower(ename) from emp;|
|upper|查询job为manager的员工|select * from emp where job=upper("manager");|表中的岗位字段是大写的|
|substr|查询姓名以M开头的员工|select * from where substr(ename,1,1)=upper('M');|被截取的字符串，起始下标，截取的长度，起始下标从1 开始|
|length|取得员工姓名长度为5的|select * from emp where length(ename)=5;|
|trim|取得工作岗位为manager的员工|select * from emp where job=upper(' manager ');|去首位空格，不去中间空格,避免一些误打空格的情况|
|str_to_date|查询1981-02-20入职的员工|select * from emp where HIREDATE='1981-02-20';|必须严格按照标准输出|
|||select * from emp where HIREDATE=str_to_date('1981-02-20','%Y-%m-%d');|('字符串','匹配格式')|
|||select * from emp where HIREDATE=str_to_date('02-20-1981','%m-%d-%Y');|将字符串转换为日期|
|format|查询员工薪水加入千分位|select empno,ename,Format(sal,0) from emp;|结果：1,800|
||查询员工薪水加入千分位和保留两位小数|select empno,ename,Format(sal,2) from emp;|结果：1,800.00|
|round|四舍五入|select round(123.56);|结果：123.6|
|||select round(1234.567,0)|结果：0表示保留到整数位，-1表示保留到十位|
|||直接输入字面值会借助表的结构把一堆一样的数放进去|select后可以跟某个表的字段名（变量名）或字面值（数据）|
|rand|生成随机数|select rand()||
|ifnull|将null转换为一个具体值|select ifnull(comm,0) from emp;|在SQL语句中，有null计算结果一定是Null,为了防止出现null，建议先用ifnull进行预处理|
|concat|字符串拼接|||  

case..when..then..else..end
如果job为MANAGER薪水上涨10%,是SALESMAN上涨50%->  
select empno,ename,job,sal,case job when 'MANAGER' then sal\*1.1 when 'SALESMAN' then sal\*1.5 end as new sal from emp;
select empno,ename,job,sal,case job when 'MANAGER' then sal\*1.1 when 'SALESMAN' then sal\*1.5 else sal end as newsal from emp;
### SQL中的日期
使用now()获取当前时间  
以下为日期格式的说明
|符号|含义|
|----|----|
|%Y|代表4位的年份|
|%y|代表2位的年份|
|%m|代表月，格式为{01,02,...,12}|
|%c|代表月，格式为{1,2,...,12}|
|%H|代表小时，格式为{00,01,...,23}|
|%h|代表小时，格式为{01,02,...,12}|
|%i|代表分钟，格式为{00,...,59}|
|%r|代表时间，格式为12小时(hh,mm,ss [AP]M)|
|%T|代表时间，格式为24小时(hh,mm,ss)|
|%S|代表秒|
|%s|代表秒|
|date_format|select empno,ename,date_format(hiredate,'%Y-%m-%d %H:%i:%s') as hiredate from emp|  
### 分组函数
特点是输入多行但是只输出一行  
分组函数自动忽略空值，不用手动的where排除  
分组函数不能直接使用在where关键字后面  
主要注意count(\*)（统计总行数，有一行就加1）和count(字段)（统计该字段下所有不为null元素的总数，所有列都是null的行是不存在的）
分组函数的组合使用：  
select count(\*),sum(sal),avg(sal),max(sal),min(sal) from emp;
|关键字|举例|实现语句|补充|
|------|---|--------|---|
|count|取得所有的员工数|select count(\*) from emp;|取得所有记录，忽略是不是null，为null的值也会取得|
||取得津贴不为null的员工数|select count(comm) from emp;|采用count(字段名称)，不会取得null的记录|
||取得工作岗位的个数|select count(distinct job) from emp;|distinct为只取不一样的|
|\sum|取得薪水的合计|select sum(sal) from emp;|取得某一列的和，null会被忽略|
||取得薪水的合计（sal+comm）|select sum(sal+ifnull(comm,0)) from emp;|sum会忽略Null值，应该先处理一下| 
|avg|取得平均薪水|select avg(sal) from emp;|取得某一列的平均值|
|max|取得最高薪水|select max(sal) from emp;|取得某一列的最大值|

### 分组查询（单表分组查询，特别重要）
分组查询主要涉及到两个子句：group by 和 having  
在SQL语句中如果有group by 语句，那么select后只能跟分组函数+参与分组的字段
先分组然后对每一组的数据进行操作
- 取得每个岗位的工资的合计，要求显示岗位名称和工资合计
&emsp; select job,sum(sal) from emp group by job;  
(如果使用了order by,必须放到group by后面)
&emsp; selecr job,sum(sal) from emp group by job order by job;
  
- 取得每个部门不同工作岗位的最高薪资（两个字段联合成一个字段看，联合分组）
&emsp; select deptno,job,max(sal) from emp group by deptno,job;
  
- 找出每个部门最高薪资，要求显示最高薪资大于3000的  
&emsp; 使用having子句，可以对查询结果进行进一步的过滤，不能单独使用，不能代替where，必须和group by联合使用，效率较低，使用where更好
&emsp; select deptno,max(sal) from emp group by deptno having max(sal)>3000;  
&emsp; select deptno,max(sal) from emp where sal > 3000 group by deptno;  
  
- 查询结果集的去重
&emsp; 在字段前加distinct关键字，它只能出现在所有字段的前面，表示后面所有的字段联合起来去重。前面不能加字段，外面可以再套分组函数
  
**语句顺序：**  
**&emsp; select from where group by  having  order by**  
**执行顺序：**  
**&emsp; from where group by having select order by;**  

## 连接查询
### 连接查询的概念  

实际开发中大部分情况都是多张表联合查询取出最后结果，数据都放在一张表中容易造成冗余（SQL92、SQL99）
#### 连接查询的分类（根据连接方式分类）
- 内连接（等值连接，非等值连接，自连接）
- 外连接（左外连接，右外连接）
- 全连接（用的非常少）
#### 笛卡尔积现象
两张表在连接时没有任何条件限制。连接时加上筛选条件可以避免。（详见PPT）

### 内连接
特点：完成能够匹配where条件的数据查询出来，两张表之间无主次关系
内连接也称等同连接，返回的结果集是两个表中相匹配的数据，而舍弃不匹配的数据。查询结果集必须满足on子句中的搜索条件
#### 等值连接
- 查询每个员工所在的部门名称，显示员工名和部门名。emp e和dept d进行连接。条件是e.deptno = d.deptno  
select e.ename,d.dname from emp e **inner join** dept d **on** e.deptno = d.deptno;
#### 非等值连接
- 查询每个员工的薪资等级，要求显示员工名，薪资，薪资等级  
select e.ename,e.sal,s.grade from emp e **inner join** salgrade s **on** e.sal between s.losal and s.hisal;  
#### 自连接（把一张表看成两张表）
- 查询员工的上级领导，要求显示员工名和对应领导名  
select a.ename as '员工' ,b.ename as '领导' from emp a **inner join** emp b **on** a.mgr = b.empno;  
(mgr是领导号码，empno是员工号码)
### 外连接
两张表之间形成了主次关系。任何一个右连接都有左连接的写法，任何一个左连接都有右连接的写法。取决于你把哪张表当作主表。  
外连接查询结果条数一定大于内连接查询结果条数
#### 右外连接
将右边的表看作主表。右表除了满足条件的数据，剩下的也显示。  
也就是右表数据全部查询，连带着左表有关系的也拿出来  
#### 左外连接
- 查询每个员工的上级领导，要求显示所有的员工名和领导名  
&emsp; select a.ename,b.ename from emp a left join emp b on a.mgr=b.empno;
#### 内外混合连接
select from a join b on **ab** join c on **ac** join d on **ad**;  
- 找出每个员工的部门名称以及工资等级，显示员工名，领导名，工资，部门名，薪资等级  
&emsp; select e.ename,m.ename,e.sak,d.dname,s.grade from emp e left join emp m e.mgr=m.deptno join dept d on e.deptno=d.deptno join salgrade s on e.sal between s.losal and s.hisal;

### 子查询
子查询就是嵌套的select语句，可以理解为子查询就是一张表，也就是将子查询的查询结果当作一张临时表  
#### where子句中的子查询
- 找出比最低工资高的员工姓名和工资  
&emsp;select ename,sal from emp where sal>(select min(sal) from emp);
- 查询管理者信息，要求显示其员工编号和姓名  
&emsp; select empno,ename from emp where empno in (select mgr from emp where mgr is not null);
- 查询哪些人薪水高于员工平均薪水，要求显示员工编号，员工姓名，薪水  
&emsp; select empno,ename,sal from emp where sal>(select avg(sal) from emp);
#### from语句中的子查询
-查询员工信息，查询哪些人是管理者，要求显示出其员工编号和员工姓名  
&emsp;首先获得管理者的编号，去除重复的.然后将查询结果当作一张表，放到from 语句的后面  
&emsp;&emsp; select distinct mgr from emp where mgr is not null;  
&emsp;&emsp; select e.empno,e.ename from emp e join (select distinct mgr from emp where mgr is not null) m on e.empno=m.mgr;  
- 查询各个部门的平均薪水所属等级，需要显示部门编号，平均薪水，等级编号  
&emsp;首先取得各个部门的平均薪水,然后将部门的平均薪水作为一张表与薪水等级表建立连接，取得等级  
&emsp;&emsp; select deptno,avg(sal) as avg_sal from emp group by deptno;  
&emsp;&emsp; select a.deptno,a.avg_sal,g.grade from (select deptno,avg(sal) as avg_sal from emp group by deptno) a join salgrade g on a.avg_sal = between g.losal and hisal;
#### 在select子句中使用子查询
- 查询员工信息，并显示员工所属的部门名称  
&emsp; 在select中再次嵌套select语句完成部分名称的查询  
&emsp;&emsp; select e.ename,(select d.dname from dept d where e.deptno = d.deptno) as dname from emp e;
这个子查询只能一次返回一条结果，多余一条就会报错  

### UNION
union可以合并集合（相加）  
合并结果集的时候，需要查询字段对应个数相同，在oracle中更严格，不但要求个数相同，而且还要求类型对应相同  
- 查询工作岗位是manager和salesman的员工  
- 1.select ename,job from emp where job in('manager','salesman');
- 2.select ename,job from emp where job = 'manager'  
- union
- select ename,job from emp where job = 'salesman';

### limit的使用
mysql提供了limit，主要用于提取前几条或中间某几行数据，也就是将查询结果集的一部分取出来，通常使用在分页查询中。  
**select * from table limit m,n**  
其中m是指记录开始的index，从0开始表示第一条记录。n是指从第m+1条开始，取n条。  
select * from table limit 2,4;  即取出第3至第6条，共4条记录
- 按照薪资降序，取出排名在前5的员工(取出薪水最高的5名员工)
- 1.select ename,sal from emp order by sal desc limit 5;
- 2.select ename,sal from emp ordre by sal desc limit 0,5;
### 通用分页
每页显示pagesize条记录，第No.page页？  
符合我们需求的分页sql格式是：select * from table limit (start-1)\*pageSize,pageSize; 其中start是页码，pageSize是每页显示的条数。  
```
public static void main (String agrs[]){
int pageNo = 5;
int pageSize = 10;

int startIndex = {pageNo - 1} * pageSize;
String sql = "select ... limit" + startIndex + "," + pageSize;
}
```
