---
title: MySql
date: 2020-11-04 00:00:00
cover: https://ydesign.oss-cn-beijing.aliyuncs.com/MySql.png
tags:  
    - 技术
    - MySql
---

# MySql

### 建库

```mysql
create database 库名;
```

 删库

```mysql
drop database 表名;
```

判断库是否存在 存在就删除

```mysql
drop DATABASE if EXISTS 表名;
```

使用数据库

```mysql
use 库名;
```

建表

```mysql
create table 表名(
    表结构
) CHARSET=utf8;
```

删除表

```mysql
drop table users;
```

彻底删除数据

```mysql
RUNCATE  TABLE 表名; 
```



### 表结构

主键 PRIMARY KEY
自增长 auto_increment

```mysql
uid int PRIMARY KEY auto_increment
```

非空约束 not null 
唯一约束 UNIQUE

```mysql
upass varchar(255) not null 
```

check约束 自定义约束 ( 注：不能使用可视化的方式设置 )
age int CHECK(age>0)

外键约束 语法： FOREIGN key（从表字段） REFERENCES（主表字段);

```mysql
CONSTRAINT fk_uid FOREIGN KEY(uid) REFERENCES users(uid);
```



### 修改表结构

添加字段 

语法：alter 表名 add 字段名 数据类型 约束

```mysql
alter table users add sex VARCHAR(10) CHECK(sex='男' or sex='女') 
```

只能为男或者女（默认男）

```mysql
enum('男','女') DEFAULT '男'  
```

修改字段名 和 约束 
alter table  表名 change 字段名 新字段名 数据类型 约束

更新 语法：alter table  表名 MODIFY  字段名 数据类型 约束

```mysql
alter table users MODIFY  sex varchar(255) DEFAULT '女'
alter table users add country varchar(255) DEFAULT '中国'
```

删除字段

```mysql
alter table users drop country
show CREATE table users
```

添加外键

```mysql
alter table orders add CONSTRAINT fk_pid FOREIGN key(pid) REFERENCES product(pid)
```

删除外键

```mysql
alter table orders drop FOREIGN key fk_pid
```

重命名

```mysql
rename table orders to orders_shop
```

### 增

增加一条信息

```mysql
INSERT INTO dept VALUES(9,'保卫部','负责公司日常安保工作')
```

一次增加多条信息

```mysql
INSERT INTO NewsType VALUES
(null,'军事'),
(null,'社会'),
(null,'国际'),
(null,'体育'),
(null,'科技'),
(null,'财经'); 
```

### 删

```mysql
DELETE from dept WHERE dname='市场部'
```

### 改

```mysql
UPDATE dept set dname='产品体验部' WHERE dname='体验部' 
```

### 查

```mysql
SELECT * FROM emp WHERE address='广西'   
```

#### 表连接 

```mysql
SELECT *  from emp,dept WHERE empty.did=dept.did salary>5000
```

查询员工信息 要求显示员工姓名 地址 薪水 职位

```mysql
SELECT  字段  from emp,dept WHERE emp.did=dept.did
```

连接三张表

```mysql
SELECT * from emp,dept,rank WHERE emp.did = dept.did and emp.rid = rank.rid 
```

三张表中查询姓名 地址 薪水 职位名称

```mysql
SELECT emp.uname 
as 员工姓名,emp.address 地址,emp.salary 薪水,rank.rankName 
as 职位名称 
from emp,dept,rank 
WHERE emp.did=dept.did and emp.rid=rank.rid 
```

### 运算符 

#### 算术运算符

 ( + - * / % )

给每个salary+500

```mysql
Update emp set salary=salary+500
```

#### 比较运算符

( > < >= <= != <>  is not null  is null )

```mysql
SELECT * from emp WHERE salary>5000
```

#### 逻辑运算符 

add 和 并且( 一般用于多个条件的连接 多个条件同时满足 )

```mysql
SELECT * from emp WHERE salary >=5000 and salary <=8000
```

BETWEEN and 在...之间

```mysql
SELECT * from emp WHERE salary BETWEEN 5000 and 8000
```

判断

```mysql
if(homeinfo.statu=0,'未出租','已出租') '出租状态'
```

Or (或者 满足一个条件就行)

```mysql
SELECT * from emp WHERE salary>=5000 or address=’温州’
```

In( 在...里面 )

```mysql
SELECT * from emp WHERE address in(‘北京’,’上海’,’广州’,’深圳’)
```

As( 给别名 )

```mysql
SELECT uname as 用户名,salary as 薪水,address 地址 from emp
```

DISTINC(过滤重复的值)

```mysql
SELECT DISTINC 字段名 from 表名
```

模糊查询

Like( %不管后面有多少字符都显示)
通配符(%_%)

```mysql
SELECT * from 表名 WHERE 字段名 like ‘张%’
SELECT * from 表名 WHERE 字段名 like ‘%张’
SELECT * from 表名 WHERE 字段名 like ‘%张%’
```

占位符

```mysql
SELECT * from 表名 WHERE 字段名 like ‘张_’
```

ORDER BY(排序 默认是升序 降序：后面加desc)
查找唐山地区的人以工资排名

```mysql
SELECT * from emp WHERE address=‘唐山’ ORDER BY salary 
```

查找唐山地区的人以工资排名 降序排序

```mysql
SELECT * from emp WHERE address=‘唐山’ ORDER BY salary desc
```

多字段排序(第一个条件同时满足的情况下 以第二个条件进行排序)
字段：salary  birthday
以salary 降序排序 后以 birthday 升序

```mysql
SELECT * from emp ORDER BY salary desc,birthday
```

Limit
参数一：从第几条数据开始统计 0 代表第一条数据   
参数二：统计多少条数据连接查询(内连接第一种查询多张表的数据 连接条件一般为外键 )
SELECT *  from emp limit 参数一,参数二 

JOIN( 内连接第二种写法 ) 语法：SELECT * from 表名1 INNER JOIN 表2 on 连接条件

```mysql
SELECT * from emp INNER JOIN dept on emp.did=dept.did
```

LEFT JOIN( 左连接 先将左表的数据全部查询出来 再连接查询右表 如果右表没有对应连接的数据 显示null 值)
RIGHT JOIN( 右连接 先将右表的数据全部查询出来 再连接查询左表 如果左表没有对应连接的数据 显示null 值)

聚合函数
(Max：最大值  Min：最小值  Avg：平均值  Count：计数  Sum：求和)
语法：SELECT max(字段) FROM 表名 
SELECT max(salary) 最高薪水,min(salary) 最低薪水 FROM emp

GROUP BY （分组 一般配合聚合函数来使用 )
语法：GROUP BY 字段名

```mysql
SELECT MAX(salary),address FROM emp GROUP BY address
```

HAVING (分组之后再加条件 条件一般为聚合函数)

```mysql
SELECT MAX(salary),address FROM emp GROUP BY address HAVING max(salary)>8000
```

子查询

```mysql
delete * from TransInfo 
 where CardNo=(select CardNo from Account,TransInfo 
where CustomerName='猪一群' ) 
```

### MySQL视图

```mysql
-- 1.进入mysql数据库
use mysql；
-- 2.查看user表
select * from user；
-- 3.创建用户
create user 'NameUser'@'localhost' identified by '123';
-- 4.刷新权限
flush privileges;
-- 5.授权
grant select,update,delete,insert on school.books to 'NameUser'@'localhost';
-- 授权用户所有数据库和表的所有权限
-- grant all privileges on *.* to 'NameUser'@'localhost';
-- 6.回收权限 
revoke ；
```

### 定义存储过程的步骤

```mysql
-- 1.如果存在就删除
drop procedure pro_borrow;
-- 2.定义结束符
delimiter $$
-- 3.创建存储过程
create procedure pro_borrow()
begin
select * from borrow;
end
-- 4.结束
$$
-- 5.还原结束符
delimiter ;
```

#### 定义一个无参有返回值的存储过程(输出参数)

```mysql
-- 查询所有图书的平均价格。
-- 删除
drop procedure if exists pro_avg_price;
-- 定义结束符
delimiter $$
-- 创建存储过程
create procedure pro_avg_price()
begin
-- 执行业务
select avg(price) into @price from books;
end $$
-- 还原结束符
delimiter ;
-- 定义用户变量，用来存储返回值
set @price = 0.0;
-- 调用存储过程
call pro_avg_price();
-- 查看结果
select @price;
```

#### 定义一个有参无返回值的存储过程(输入参数)

```mysql
-- 查询某个价格范围内的图书信息 20，50
-- 1.删除
drop procedure if exists pro_select_books;
-- 2.定义结束符
delimiter $$
-- 3.创建存储过程
create procedure pro_select_books(in price1 int,in price2 int)
begin
select * from books where price between price1 and price2;
end $$
-- 5.还原结束符
delimiter ;
-- 6.调用存储过程
call pro_select_books(20,50);
```

### 变量

-- 1.全局变量：不能自定义，都是@@开头
-- 2.用户变量：可以自定义，在当前数据库使用，一般都是@开头
-- 2.1使用方式

```mysql
 set @name = 'tom';
```

-- 2.2

```mysql
select @name := bname from books where bno = 1001;
```

-- 2.3

```mysql
select bname into @name from books where bno = 1001;
```

-- 3.局部变量：可以自定义，只能作用于begin-end之间，一般不用@开头

分支结构
if 条件  then
语句块
end  if；

循环结构
while 条件  do
语句块
end  while；

### 触发器

```mysql
-- 触发器
-- drop trigger if exists 触发器名称
-- create trigger 触发器名称 {before|after} {insert|update|delete} on 表名 for each row
-- begin
-- 触发器执行的语句块
-- End

drop trigger if exists trigger_borrow
create trigger trigger_borrow before insert on borrow for each row
begin
insert StuExam_log value(default,'insert',CURRENT_USER,NOW());
end
```





sql

-- 新建用户

CREATE USER budget_wsfy IDENTIFIED BY budget_wsfy; 

-- 为用户授权

GRANT connect,dba to budget_wsfy;

 

-- 删除用户

drop user budget_wsfy cascade;

 

-- 流程

ORA-01439: 要更改数据类型则要修改的列必须为空

1、修改原字段名name为临时字段name_new；

alter table ACT_HI_VARINST rename column TEXT_ to TEXT_1 ;

2、添加一个新字段名称和原来字段名相同，name，类型为要修改的新类型；

alter table ACT_HI_VARINST add(TEXT_ NVARCHAR2(2000));

3、把临时字段name_new的数据更新到新添加的字段name中；

update ACT_HI_VARINST set TEXT_ = trim(TEXT_1);

4、删除临时字段name_new;

alter table ACT_HI_VARINST drop column TEXT_1 ;



-- 回滚

alter table PD_BASE_SUBJECTINFO enable row movement;

flashback table PD_BASE_SUBJECTINFO to timestamp TO_TIMESTAMP('2023-07-22 20:50:38', 'yyyy-mm-dd hh24:mi:ss');
