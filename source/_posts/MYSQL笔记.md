---
title: MYSQL笔记
date: 2023-10-19
tags:
- mysql
categories:
- mysql
aside: false
---

1.打开mysql
cmd->mysql -u root -p

## 一、数据库/表的操作（DDL）:
2.查询所有数据库：
show databases;
3.创建：
create database (if not exists可加看自己需求) XXX(数据库名) (default charset utf8mb4字符集，看需求);
4.删除
drop database XXX;
5.使用/切换数据库
use XXX;
6.查看当前使用哪个数据库
select database();
——将mydata2数据库的编码修改为utf-8；
alter database mydata2 character set utf8;
——查看数据的定义信息：
show create database mydata;
7.显示数据库中的表
show tables;
8.查看表的字段信息：
desc student;
9.在上面员工表的基本上增加一个address列：（表中加列）
alter table student add address varchar(1000);
10.修改name列，使其长度为30：
alter table student modify name varchar(30);
11.删除address列,一次只能删一列：
alter table student drop address;
12.表名改为user：
rename table student to user;
13.查看表格的创建细节：
show create table user;
14.修改表的字符集为gbk：
alter table user character set gbk;
15.列名name修改为username：
alter table user change name username varchar(20)；
16.删除表：
drop table user;
17.数据类型：数值型、字符型、时间
18.创建表
create table student(
	id int comment '编号',
	name varchar(50) comment '姓名') comment '学生信息表';

DDL总结：
show databases;
create/drop database 数据库名;
use 数据库名;
select database();
show tables;
create/drop table 表名（字段 字段类型 ,字段 字段类型）;
desc 表名;
show create table 表名;
alter table 表名 add/modify/change/drop/rename to ...;

## 二、DML:对数据库中表的数据记录进行增删改查
增：insert into 表名(字段1， 字段2) values(值1， 值2);
改：update 表名 set 字段1 = ‘修改值', 字段2 = ‘修改值'  where ... （where后面为查询条件）;
删：delete from 表名 where ...;

## 三、DQL:查询数据库中表的记录
基本查询：select ... from ... 
条件查询：where 
聚合函数：count、max、min、avg、sum
分组查询：group by         having是聚合函数里的条件，在group by后面进行筛选
排序查询：order by
分页查询：limit

编写顺序：select ... from ... where ... group by ... having ... order by ... limit ...
执行顺序：from ... where ... group by ... having ... order by ... limit ...

## 四、DCL：管理数据库用户，控制数据库访问权限
管理用户：'%'任意主机，'localhost'当前主机
1.查询用户
use mysql;
select * from user;
2.创建用户
create user '用户名'@'主机名' identified by '密码';
3.修改用户名密码
alter user '用户名'@'主机名' identified with mysql_native_password by '密码';
4.删除用户
drop  user '用户名'@'主机名';
权限：all代表所有权限
查询权限：show grants for '用户名'@'主机名';
授予权限：grant 权限列表 on 数据库.表名 to '用户名'@'主机名';  eg:grant all on 数据库.* to '用户名'@'主机名';->显示：grant all privieges on 数据库.* to '用户名'@'主机名'表示数据库所有表的权限授予用户
撤销权限：revoke 权限列表 on 数据库.表名 from '用户名'@'主机名';

函数：
1.字符串：concat、lower、upper、lpad、rpad、trim、substring
2.数值：ceil、floor、mod、rand、round
3.日期：curdate、curtime、now、year、month、day、date_add、datediff
4.流程：if、ifnull、case[...] when...then...else...end
约束：
not null、unique、primary key、default、check、foreign key
eg:alter table 表名 add constraint 外键名 foreign key （外键字段名） references 主表(主表列名);

多表查询：
显式内联：select 字段列表 from 表1 inner join 表2 on 连接条件;
隐式：elect 字段列表 from 表1,表2 where 条件;
左外联：select 字段列表 from 表1 left join 表2 on 连接条件;
右：select 字段列表 from 表1 right join 表2 on 连接条件;
自：select 字段列表 from 表1join 表2 on 连接条件;
联合：union all、union(去掉查询后的重复值)
eg:查询所有工资小于5000和年龄大于50的员工都查出来select * from 表 where s < 5000 union all select * from 表 where age > 50;
子查询：select * from t where column = (select column from t1);
select * from t where (字段1，字段2) =/in (select 字段1，字段2 from t1 where ...);


事务：一组操作集合，把所有操作作为一个整体向系统提交/撤销请求
eg:a转1000给b
select * from account where name = "a";
update account set money = money - 1000 where name = 'a';
update account set money = money + 1000 where name = 'b';

开启：start transaction、begin
提交：commit;
回滚：rollvack;

