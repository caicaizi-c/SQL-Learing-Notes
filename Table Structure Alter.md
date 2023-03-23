### 表的创建
#### 建表的语法格式
创建表时表中有字段，每一个字段有字段名、字段数据类型、字段长度限制、字段约束
```
create table 表名(
  字段名1 数据类型，
  字段名2 数据类型
  );
```
表名建议以t_或tbl_开始，可读性较强。所有标识符都应该小写，单词和单词之间使用下划线连接。
#### 常见数据类型
|符号|意义|
|----|----|
|varchar|255,可变长度字符串，根据传过来的实际的数据长度动态分配空间，节省空间但速度慢|
|char|255定长字符串，分配固定长度的空间，使用不恰当时会导致空间的浪费，速度较快|
|int|整数型|
|bigint|长整型，相当于long|
|float||
|double||
|date|短日期|
|datetime|长日期|
|clob|字符大对象，最多可以存储4个G的字符串，文章说明书等超过255的都采用clob|
|blob|二进制大对象，存储图片、声音视频等流媒体数据。往blog类型的字段上插入数据时需要使用io流才行|
#### 创建一个学生表
drop table t_student 当表不存在时会报错
drop table if exists t_student;这个不会
- 创建一个学生信息表，字段包括：学号、姓名、性别、出生日期、email、班级标识
```
create table t_student(
  student_id int(10),
  student_name varchar(20),
  sex char(1) default 'm',
  birthday date,
  email varchar(30),
  classes_id int(3)
);
```
- 向表中加入数据（必须使用客户端软件，cmd默认GBK编码，数据中设置的编码是UTF-8）  
**DML语句** insert语句只要执行成功就会多一条记录。字段名可以省略，相当于都写上了，所以值也要都写上，不能颠倒顺序  
&emsp; insert into t_student(student_id,student_name,sex,birthday,email,classes_id) values(1001,'zhangsan','1988-01-01','qqq@163.com',10)  
性别处不填值得话就使用了默认值。指定默认值时为default值，不指定则为Null.  
#### insert插入日期
数字格式化：format（数字，‘格式’）  
select ename,format(sal,’$999,999’) as sal from emp;加入千分位  
str_to_date:将字符串varchar类型转换成date类型  Str_to_date(‘字符串日期’， ‘日期格式’)  
此函数通常使用在insert上，因为插入的时候需要一个日期类型的数据，需要通过该函数将字符串转换为date。当日期字符串格式为年-月-日时，这个函数可以省略，会做自动类型转换  
date_format:将data类型转换成具有一定格式的varchar字符串类型  
将日期类型转换成特定格式的字符串	select id,name,data_format(birth,’%Y-%m-%d’) from t_user;  
此函数通常使用在查询日期方面。设置展示的日期格式。  
不指定的话也会有默认格式化，将日期类型转换成varchar类型。  
Java中的日期格式：yyyy-MM-dd  HH:mm:ss SSS  


