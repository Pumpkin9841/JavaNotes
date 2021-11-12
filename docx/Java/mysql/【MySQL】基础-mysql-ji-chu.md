---
title: 【MySQL】基础
date: 2021-09-23 15:46:33.698
updated: 2021-09-25 14:37:14.432
url: https://pumpkn.xyz/archives/mysql-ji-chu
categories: 
tags: 学习 | mysql
---

# 数据库三层结构

- 所谓安装Mysql数据库，就是在主机安装一个数据库管理系统(DBMS)，这个管理程序可以管理多个数据库。```DBMS(database manage system)```
- 一个数据库中可以创建多个表，以保存数据(信息)
- 数据库管理系统(DBMS)、数据库和表的关系如图

![image.png](https://pumpkn.xyz/upload/2021/09/image-ed9024d79b8240ad91643d72240c5e5f.png)

# SQL语句分类

- ```DDL```: 数据定义语句[create 表、库]
- ```DML```:数据操作语句[insert , update , delete]
- ```DQL```: 数据查询语句[select]
- ```DCL```: 数据控制语句[比如用户权限 grant revoke]

# 创建数据库
```sql
CREATE DATABASE IF NOT EXISTS test 
    CHARACTER SET utf8 
    COLLATE utf8_bin # 校对规则 utf8_bin区分大小写，默认utf8_general_ci 不区分大小写
```

# 查看、删除数据库
```sql
# 显示数据库
SHOW DATABASES 

# 显示数据库创建语句
SHOW CREATE DATABASE test

# 删除数据库
DROP DATABASE IF EXISTS test 
```

# 备份数据库(在dos执行)
```sql
mysqldump -u 用户名 -p -B 数据库1 数据库2 数据库n > 文件名.sql
```

# 备份数据库的表
```sql
mysqldump -u 用户名 -p 密码 | 数据库 表1 表2 表n > 路径
```

# 恢复数据库(进入mysql命令行执行)

```sql
Source 文件名.sql
```

# 创建表

![image.png](https://pumpkn.xyz/upload/2021/09/image-9f9bbfee73874d4797cb927c0c44cf01.png)

```sql
CREATE TABLE user(
	id int ,
	name VARCHAR(255) ,
	pwd VARCHAR(255) ,
	birhday DATE
)CHARACTER SET utf8 COLLATE utf8_bin ENGINE INNODB
```

![image.png](https://pumpkn.xyz/upload/2021/09/image-17e7e6576af548aa9a1eeb97bfb5891f.png)

![image.png](https://pumpkn.xyz/upload/2021/09/image-e118e840a42842ae83c610f1b00279ff.png)

# Mysql常用数据类型

## 数值型（小数）

- ```float/double[unsigned]``` float 单精度, double双精度
- ```decimal[m,d][unsigned]```

	- 可以支持更加精确的小数位。m是小数位数的总数，d是小数点后面的位数
	- 如果d是0，则值没有小数点或分数部分。m最大65，d最大30.如果d被省略，默认是0.如果m被省略，默认是10
	- 建议：如果希望小数的精确度高，推荐使用decimal

## 字符串的基本使用

- ```CHAR(size)``` 固定长度字符串，最大255个字符
- ```VARCHAR(size)``` 0~65535，可变长度字符串，最大65535字节[utf8编码最大21844字符，1-3个字节用于记录大小]

## 字符串使用细节

- 细节1

	- ```char(4)``` 这个4表示字符数(最大255)，不是字节数，不管是中文还是字母都是放4个，按字符计算
	- ```varchar(4)``` 这个4表示字符数，不管是字母还是中文都以定义好的表的编码来存放数据
	- 不管是中文还是英文字母，都是最多存放4个，是按照字符来存放的

- 细节2

	- ```char(4)```是定长（固定大小），也就是说，即使你插入'aa'，也会占用分配的4个字符
	- ```varchar(4)```是变长的，就是说，如果你插入'aa'，实际上占用的空间大小并不是4个字符，而是按照实际占用空间来分配的(说明：varchar本身还需要占用1-3个字节来记录存放内容长度)

- 细节3（什么时候使用char，什么时候使用varchar）

	- 如果数据是定长，推荐使用char，比如md5的密码，邮编，手机号，身份证等
	- 如果一个字段的长度是不确定的，我们使用varcha，比如留言，文章等
	- **查询速度 char > varchar**

- 细节4
在存放文本时，也可以使用Text数据类型。可以将TEXT列视为VARCHAR列，**注意Text不能有默认值，大小0-2^16字节**，如果希望存放更多字符，可以选择```MEDIUMTEXT 0-2^24```或者```LONGTEXT 0-2^32```

## 日期类型的基本使用
```sql
CREATE TABLE birthday6(
		t1 DATE ,
		t2 DATETIME ,
		t3 TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP 
		ON UPDATE CURRENT_TIMESTAMP
)
```
TimeStamp在Insert和update时，自动更新


# 修改表

## 添加列


```sql
/*
 ALTER TABLE tableName 
	   ADD COLUMN datatype [DEFAULT expr] [, COLUMN datetype...]
*/
# 例如
ALTER TABLE birthday6 ADD COLUMN id int 
```

## 修改列的数据类型
```sql
/*
 ALTER TABLE tableName
	   MODIFY COLUMN datatype [DEFAULT expr]
*/

# 例如
ALTER TABLE birthday6 MODIFY  id  FLOAT
```

## 修改列名和列数据类型
```sql
ALTER TABLE birthday6 CHANGE id id4 DECIMAL(10,2)
```

## 删除列
```sql
/*
	ALTER TABLE tableName
	    DROP column
*/
# 例如
ALTER TABLE birthday6
     DROP id4
```

# ```Insert```语句

```sql
INSERT INTO tableName[(column...)]
     VALUES(value...)
```
## ```INSERT```细节说明
- 插入的数据应与字段的数据相同
- 数据的长度应在列的规定范围内
- 在values中列出的数据位置必须与被加入的列的排列位置相对应
- **字符和日期型数据应包含在单引号中**
- 列可以插入空置【前提时该字段允许为空】，```insert into table value(null)```
- ```insert into table_name(列名...) values(),(),()形式可以添加多条记录
- 如果是给表中的所有字段添加数据，可以不写前面的字段名称
- 默认值的使用，当不给某个字段值时，如果有默认值就会添加，否则报错。

# ```update```语句

```sql
UPDATE tableName SET column = expr...
       WHERE 条件
```

## 使用细节
- UPDATE语法可以用新值更新原有表行中的各列
- SET子句指示要修改哪些列和要给予什么值
- WHERE子句指定应更新那些行。如果没有WHERE子句，则更新所有的行
- 如果需要修改多个字段，可以通过set字段1=值1,字段2=值2

# ```delete```语句

```sql
delete from tableName where 条件
```

## 使用细节
- 如果不使用where子句，将删除表中所有数据
- delete语句不能删除某一列的值(可使用update设为null，或者'')
- 使用delete语句仅删除记录，不删除表本身。如果要删除表，使用drop table语句

# ```select```语句
```sql
SELECT [DISTINCT] *|{column1 , column2...}
      FROM tableName
```
- ```*``` 代表查询所有列
- ```DINSTINCT```可选，显示结果时，是否去掉重复数据

## 在select语句中可使用as语句
```sql
SELECT column_name as 别名 from table_name
```

## select语句的使用

```sql
SELECT name , (chinese + english + math) as '总分'
	FROM student
```

# 在where子句中经常使用的运算符

![image.png](https://pumpkn.xyz/upload/2021/09/image-5bf261ab7a4a4afeaaee7631c217a24a.png)


# 使用```order by``` 排序
```sql
SELECT column1 , column2...
	FROM table 
	ORDER BY column ASC|DESC
```

- ORDER BY 指定排序的列，排序的列既可以是表中的列明，也可以时select语句后指定的列名
- ASC升序（默认）、DESC降序
- ORDER BY 子句应位于SELECT语句的结尾

# 统计函数(count)
```sql
SELECT count(*) | count(列明)
     FROM tableName
     WHERE 条件
```

## count(列)和count(*)的区别

- ```count(*)```返回满足条件的记录的行数
- ```count(列名)```统计满足条件的某列有多少个，但是会排除null

```sql
CREATE TABLE T15(
	name varchar(20) 
)

INSERT INTO T15 VALUES('jack'),('Tom'),('mary'),(null)

SELECT COUNT(*) FROM T15 -- 4

SELECT COUNT(name) FROM T15 -- 3
```

# 合计函数(sum)
```sum```函数返回满足where条件的行的和
```sql
SELECT sum(列名) FROM tableName WHERE 条件语句
```

```sql
# 统计一个班语文，英语，数学总分
SELECT SUM(chinese) as '语文总分', SUM(english) as '英语总分' , SUM(math) as '数学总分' FROM student

# 统计一个班语文成绩平均分
SELECT SUM(chinese) / COUNT(*) FROM student
```

# 求平均值(avg)

```avg```函数返回满足where条件的一列平均值

```sql
# 语法
select avg(列名) from table
     where 条件语句

# 例子
## 一个班级总分的平均分
SELECT AVG( chinese + math + english ) as '平均分'
	FROM student 

```

# 求最大最小值(Max/Min)
Max/Min函数返回满足where条件的一列的最大/最小值
```sql
# 语法
select max(列名) from tableName
      where 条件语句
# 练习 求班级最高分和最低分（数值范围在统计中特别有用）
# 最高分
select max(math+english+chinese) as '班级最高分' from student

# 最低分
select min(math+english+chinese) as '班级最高分' from student
```

# ```select```语句深化

- 使用```group by```子句对列进行分组

	- ```sql
	     select column1 , column2 from table 
                   group by column
         ```

- 使用```having```子句对分组后的结果进行过滤

	- ```sql
            select column1,column2 
            from table
            group by column
            having ...
	  ```

- ```group by```用于对查询的结果分组统计
- ```having```子句用于限制分组显示结果，相当于分组结果的where条件语句

## 练习

```sql
# 如何显示每个部门的平均工资和最高工资
# 各个部门平均工资
select * from emp ;
SELECT avg(sal) , deptno 
	from emp 
	group by deptno
	
# 各个部门最高工资
select max(sal) , deptno
from emp 
group by deptno

# 显示每个部门的每种岗位的平均工资和最低工资
SELECT avg(sal) , min(sal), deptno , job
FROM emp
GROUP BY deptno , job

# 显示平均工资低于2000的部门号和它的平均工资

SELECT AVG(sal) as avg_sal , deptno
FROM emp 
GROUP BY deptno
HAVING avg_sal < 2000
```
# 字符串相关函数
![image.png](https://pumpkn.xyz/upload/2021/09/image-5e6bda1a017a48a59b7019ea9ee37a82.png)


## 演示
```sql
-- 字符串相关函数
# CHARSET(str) 返回字串字符集
SELECT CHARSET(ename) FROM emp ;

# CONCAT(string2 ....) 连接字串，将多个列拼接成1列
SELECT CONCAT(ename , '工作是' , job) FROM emp ;

# INSTR(string , substring) 返回substring在string中出现的位置，没有则返回0
# dual 亚元表，系统表，可以作为测试表使用
SELECT INSTR('zfhlx' , 'lx') FROM dual  -- 4

# UCASE(string2) 转换成大写
SELECT UCASE(ename) FROM emp ;

# LCASE(string2) 转换成小写
SELECT LCASE(ename) FROM emp ;

# LEFT(string2 , length) 从string2中的左边起取length个字符
# RIGHT(string2,length) 从string2中的右边起取length个字符

SELECT LCASE(RIGHT(ename , 2)) FROM emp ;

# length(string) string长度(按照字节)
SELECT ename , length(ename) from emp ;

# replace(str , search_str , replace_str) 
# 在str中用replace_str替换search_str
# 如果是manager 就替换成 经理
SELECT ename , replace(job , 'MANAGER' , '经理') FROM emp 

# STRCMP(string1,string2) 逐字符比较两字串大小
select strcmp('zf' , 'lx') from dual 

# substring(str , position , length)
# 从str的position开始[从1开始计算]，取length个字符
# 从ename列的第一个位置开始取出2个字符
select substring(ename , 1 , 2 ) from emp ;

# ltrim(string2)
# rtrim(string2)
# trim(string2)
# 去除前端空格或后端空格
select ltrim('    zf   ') from dual 
select rtrim('    zf   ') from dual 
select trim('    zf   ') from dual 

```

# 数学相关函数

![image.png](https://pumpkn.xyz/upload/2021/09/image-16968be14abb4244a9db118f1699cc29.png)

## 演示

```sql
-- 数字相关函数
# abs 绝对值
select abs(-10) from dual -- 10

# bin(number) 十进制转二进制
select bin(10) from dual  -- 1010

# ceiling(number) 向上取整，得到比number大的最小整数
SELECT ceiling(-2.1) from dual -- -2

# conv(number , from_base , to_base) 进制转换
# 将10进制的20 ，转换成2进制数
select conv(20 , 10 , 2) from dual -- 10100

# floor(number) 向下取整，得到比number小的最大整数
SELECT FLOOR(-2.1) FROM dual  -- -3

# FORMAT(number,decimal_places) 保留小数位数(四舍五入)
SELECT FORMAT(78.125458,2) FROM dual 

# hex(number) 转十六进制
SELECT HEX(22) FROM dual 

# least(number , number2 ...) 求最小值
SELECT least(0,1,-10,4) from dual  -- -10

# mod(number , denominator) 求余
SELECT mod(10 ,3) from dual 

# rand([seed]) 返回随机数 范围为0<= v <= 1.0
# 如果使用rand() 每次返回不同的随机数 在0 <= v <= 1.0
# 如果使用rand(seed) 返回随机数，范围 0 <= v <= 1.0 , 如果seed不变，随机数也不变

SELECT rand() from dual 
```

# 时间日期相关函数
![image.png](https://pumpkn.xyz/upload/2021/09/image-7630e64bd71a4d46bd8b125d290d792a.png)

## 演示
```sql
-- 日期时间相关函数

# CURRENT_DATE 当前日期
select CURRENT_DATE() from dual 

# CURRENT_TIME 当前时间
SELECT CURRENT_TIME() from dual 

# CURRENT_TIMESTAMP 当前时间戳
SELECT CURRENT_TIMESTAMP() FROM dual 

# 当前时间
SELECT NOW() from dual 

-- 创建测试表
create table mes(
	id int ,
	content varchar(30) ,
	sendtime datetime 
) ;

INSERT INTO mes VALUES(1 , '北京新闻' , CURRENT_TIMESTAMP());
INSERT INTO mes VALUES(2 , '上海新闻' , NOW());
INSERT INTO mes VALUES(3 , '广州新闻' , CURRENT_TIMESTAMP);
INSERT INTO mes VALUES(4 , '重庆新闻' , CURRENT_TIMESTAMP);

SELECT * from mes
# 显示日期，不用显示时间
select id , content , date(sendtime) from mes

# 查询在5分钟内发布的新闻
select * 
       from mes
			 WHERE DATE_ADD(sendtime,INTERVAL 5 MINUTE) >= NOW()
			 
# year|month|day|date(datetime)
select year(now()) from dual -- 2021
select month(now()) from dual -- 9
select day(now()) from dual  -- 24
select month('2013-11-10') from dual -- 11

# unix_timestamp() 返回的是1970-1-1到现在的秒数
select UNIX_TIMESTAMP() from dual 

# FROM_UNIXTIME() 可以把一个unix_timestamp秒数，转成指定格式的日期

# %Y-%m-%d 格式是规定好的，表示年月日
select FROM_UNIXTIME(1632468873 , '%Y-%m-%d') from dual
select FROM_UNIXTIME(1632468873 , '%Y-%m-%d %H:%i:%s') from dual

```

在实际开发中，我们经常使用int来保存一个unix时间戳，然后使用from_unixtime()进行转换，还是非常有使用价值的

# 加密和系统函数

- ```user()``` 查询用户
- ```database()``` 数据库名称
- ```md5(str)``` 为字符串算出一个md5 32的字符串，加密
- ```password(str)``` 从原文密码str计算并返回密码字符串，通常用于mysql数据库的用户密码加密

# 流程控制函数

![image.png](https://pumpkn.xyz/upload/2021/09/image-95eefce8b810482d948257add01d1621.png)

两个需求

- 查询emp表，如果comm是null，则显示0.0
- 如果emp表的job是clerk则显示职员，如果是manager则显示经理，如果是salesman则显示销售人员，其他正常显示


```sql
# 查询emp表，如果comm是null，则显示0.0
SELECT ename ,if(comm is null , 0.0 , comm)
FROM emp

# 如果emp表的job是clerk则显示职员，如果是manager则显示经理，如果是salesman则显示销售人员，其他正常显示
select ename , (select case
                when job = 'CLERK' then '职员'
				when job = 'MANAGER' then '经理'
				when job = 'SALESMAN' then '销售人员'
	        	else job
				end
				) as 'job'
from emp ;

```

# mysql表查询——加强
## 介绍
之前的查询都是对一张表进行的查询，这在实际开发中是远远不够的，下面都是关于多表查询的

## 案例

```sql
# 使用where子句
# 如何查找1992.1.1后入职的员工
-- 在mysql中，日期类型可以直接比较，需要注意格式
SELECT * from emp 
         where hiredate > '1992-01-01'
				 
# 如何使用like操作符
# %:表示0到多个字符 _:表示单个字符
# 如何显示首字符为S的员工姓名和工资
select ename , sal 
       from emp 
		   where ename like 'S%'
			 
# 如何显示第三个字符为大写O的所有员工的姓名和工资
select ename , sal
	     from emp 
			 where ename like '__O%'

# 如何显示没有上级的雇员情况
select * from emp 
				 where mgr is null 
				 
# 查询表结构
DESC emp 

# 使用order by子句
# 如何按照工资的升序，显示雇员信息
select * from emp
         order by sal 
				 
 # 按照部门号升序而雇员的工资降序排列，显示雇员信息
 select * from emp
					order by deptno , sal desc
```

## 分页查询
### 基本语法
```sql
select ... limit start , rows
# 表示从start+1 行开始取，取出rows行，start从0开始计算

# 推导公式
# 要显示第几页的内容
select * from table
         limit 每页显示记录数 * (第几页-1),每页显示记录数
```

## group by加强

```sql
-- 使用分组函数和分组子句group by
# 显示每种岗位的雇员总数、平均工资
select count(*) , avg(sal)
      from emp 
		  group by job
# 显示雇员总数，以及获得补助的雇员数
# 思路：获得补助的雇员数，就是comm列为非null,count(列)是不统计值为null的
select count(*) , count(comm)
		from emp 
# 显示雇员总数，以及没有补助的雇员数
select count(*) , count(if(comm is null , 1 , null))
		  from emp 

# 显示管理者的总人数
SELECT count(DISTINCT mgr)
	   from emp 
# 显示雇员工资的最大差额
SELECT max(sal) - min(sal) from emp 
```
## 分组总结
如果select语句同时包含有group by , having , limit ,order by那么他们的顺序是group by , having , order by</br>

```sql
# 语法顺序
select column...
       from table
       group by column
       having 条件
       order by column
       limit start , rows ;

# 案例
# 统计各个部门的平均工资，并且是大于1000的,并且按照平均工资从高到低排序，提取前两行记录
select avg(sal) as avg_sal , deptno from emp 
       group by deptno
		having avg_sal > 1000
		order by avg_sal
		limit 0 ,2
```

# mysql多表查询
多表查询是指基于两个和两个以上的表查询，在实际应用中，查询单个表可能不能满足你的需求。

## 多表查询练习
```sql
 -- 多表查询练习
 # 显示雇员名，雇员工资及所在部门名字[笛卡儿积]
 select * from emp 
 select * from dept
 select  ename ,sal ,dname 
       from emp , dept
			 where emp.deptno = dept.deptno
			 
 -- 技巧：多表查询的条件不能少于表的个数-1，否则会出现笛卡儿积
 # 如何显示部门号为10的部门名、员工名和工资
 select ename , sal , dname
       from emp , dept
			 where emp.deptno = dept.deptno and dept.deptno = 10
			 
 # 显示各个员工的姓名、工资，及其工资级别
 select * from salgrade
 select ename , sal , grade 
       from emp , salgrade
			 where emp.sal between salgrade.losal and salgrade.hisal
```

## 自连接
自连接是指在同一张表的连接查询（同一张表看做两张表）

```sql
 # 显示公司员工名字和他的上级的名字
 select e1.ename as '员工', e2.ename as '上级'
      from emp e1 , emp e2
		where e1.mgr = e2.empno
```

# mysql表子查询
## 什么是子查询
子查询就是嵌入在其他sql语句中的select语句，也叫嵌套查询

- 单行子查询
单行子查询是指只返回一行数据的子查询语句

- 多行子查询
- 多行子查询指返回多行数据的子查询，使用关键字```in```

## 练习
```sql
-- 子查询
-- 单行子查询
# 如何显示与SMITH同一部门的所有员工
select * from emp
	where deptno = (
	select deptno
    from emp
	where ename = 'SMITH'
	)
	
-- 多行子查询
	# 如何查询和部门10的工作相同的雇员的信息、但是不含10号部门自己的雇员
select * from emp
     where job in (
		select distinct job
	    from emp 
		where deptno = 10	
		 ) and deptno != 10

```

** 子查询可以当作临时表使用**

## ```all```和```any```的使用
```sql
 # all和any的使用
 # 显示工资比部门30的所有员工的工资高的员工的姓名、工资和部门号
 select ename , sal , deptno
		  from emp 
			where sal > all(
						 select sal
						 from emp
						 where deptno = 30
			       )
	
	# 如何显示工资比部门30的其中一个员工的工资高的员工的姓名、工资和部门号
	select ename , sal ,deptno
	     from emp
			 where sal > any(
							select sal
							from emp
							where deptno = 30
						  )
```

## 多列子查询
多列子查询是指查询返回多个列数据的子查询语句

```sql
# 多列子查询
# 查询与smith的部门和岗位完全相同的所有雇员（并且不含smith本人）

select *
     from emp
		 where (deptno , job) = (
		        select deptno , job
						from emp
						where ename = 'SMITH'
		 ) and ename != 'SMITH'
```

## 在from子句中使用子查询
```sql
# 查找每个部门工资高于本部门平均工资的人的资料
select * 
	   from (
			  select avg(sal) as avg_sal, deptno
               from emp
			  group by deptno
			) temp , emp 
	where temp.deptno = emp.deptno and emp.sal > temp.avg_sal
			
# 查找每个部门工资最高的人
select * 
		 from (
		    select max(sal) as max_sal, deptno
            from emp
		    group by deptno
		 ) temp , emp
		 where temp.deptno = emp.deptno and emp.sal = temp.max_sal

# 查询每个部门的信息（包括部门名，编号，地址），各部门人数
select dept.* , temp.total
		  from (
				select count(*) as total, deptno
        		from emp 
			  GROUP BY deptno
			) temp , dept
			where temp.deptno = dept.deptno
```

# 表复制

## 自我复制数据(蠕虫复制)
有时，为了对某个sql语句进行效率测试，我们需要海量数据时，可以使用此法为表创建海量数据


```sql
-- 表复制
create table my_tab01(
			id int ,
			`name` varchar(32) ,
			sal double ,
			job varchar(32),
			deptno int
) ;

-- 演示自我复制
# 1.先把emp表的记录复制到my_tab01
insert into my_tab01 (id,`name`,sal,job,deptno) 
			select empno,ename,sal,job,deptno from emp 
# 2.自我复制
insert into my_tab01
		 select * from my_tab01

select count(*) from my_tab01

-- 如何删除一张表重复记录
# 1.先创建一张表my_tab02
# 2.让my_tab02有重复的记录
create table my_tab02 like emp  -- 把emp表的结构复制到my_tab02
insert into my_tab02
      select * from my_tab02
desc my_tab02
select count(*) from my_tab02
# 3.考虑去重my_tab02记录
#    思路
#    (1) 先创建一张临时表my_tmp,该表的结构和my_tab02一样
      create table my_tmp like my_tab02
#    (2) 把my_tab02的记录通过distinct关键字 处理后把记录复制到my_tmp
     insert into my_tmp
		       select distinct * from my_tab02
#    (3) 消除掉my_tab02记录
      delete from my_tab02
#    (4) 把my_tmp表的记录复制到my_tab02
      insert into my_tab02
			      select * from my_tmp
#    (5) drop掉临时表my_rmp
			drop table my_tmp

```

# 合并查询
有时在实际应用中，为了合并多个select语句的结果，可以使用**集合操作符号```union```，```union all``` **

## ```union all```
该操作符用于取得两个结果集的并集。当时用该操作符时，不会取消重复的行

```sql
# union all  就是两个结果求并集，不会去重
select ename,sal,job from emp where sal > 2500
union all 
select ename ,sal ,job from emp where job = 'MANAGER'
```

## ```union```
该操作符与```union all```相似，但是会**自动去掉结果集中重复行**

# mysql表外连接

- **左外连接**  （如果**左侧的表完全显示**我们就说左外连接）
- **右外连接**  （如果**右侧的表完全显示**我们就说右外连接）

```sql
-- 外连接演示
# 创建测试表
create table stu(
	id int ,
	`name` varchar(30)
)

insert into stu values(1 ,'Jack') , (2,'Tom') , (3 ,'Kity') , (4,'nono')
select * from stu

create table exam(
	id int ,
	grade int
)

insert into exam values(1,56),(2,76),(11,8)
select * from exam

# 使用左外连接（显示所有人的成绩，如果没有成绩，也要显示该人的姓名和id，成绩显示为空）
# select ... from 表1 left join 表2 on 条件
# 表1：就是左表  表2：就是右表
select `name` , stu.id , grade
      from stu left join exam
			on stu.id = exam.id
			
# 使用右外连接（显示所有成绩，如果没有名字匹配，则显示空）
# select ... from 表1 right join 表2 on 条件
# 表1：就是左表 表2：就是右表
select `name` , stu.id , grade
     from stu right join exam
		 on stu.id = exam.id

```

# mysql约束
**约束**用于确保数据库的数据满足特定的规则</br>
在mysql中，约束包括```not null```、```unique```、```primary key```、```foreign key```和```check```五种

## ```primary key``` 主键

### 基本使用

```字段名 字段类型 primary key```
用于唯一的标示表行的数据，当定义主键约束后，该列不能右重复值

```sql
-- 主键
create table t17(
		id int primary key , -- 表示id列是主键
		`name` varchar(32) ,
		email varchar(32)
)
```

## ```primary key```细节说明
- primary key不能重复而且不能为null
- 一张表最多只能有一个主键，但可以是复合主键
- 主键的指定方式有两种

	- 直接在字段名后指定 ```字段名 prikary key```
	- 在表定义最后写 ```primary key(列名)```
- 使用desc表名，可以看到primary key的情况

```sql
-- 演示复合主键（id和name做成复合主键）
create table t18(
		id int ,
		`name` varchar(32),
		email varchar(32),
		primary key(id , `name`) -- 这里就是复合主键
)
```

## ```not null```非空
如果在列上定义了not null，那么当插入数据时，必须为列提供数据</br>
基本语法```字段名 字段类型 not null```

## ```unique```唯一
当定义了唯一约束后，该列值是不能重复的</br>
基本语法```字段名 字段类型 unique```

### ```unique```细节
- 如果没有指定not null，则unique字段可以有多个null
- 一张表可以有多个unique字段

## ```foreign key```外键
用于定义主表和从表之间的关系：外键约束要定义在从表上，主表则必须具有主键约束或是unique约束，当定义外键约束后，要求外键列数据必须在主表的主键列存在或是为null
</br>
```foreign key(本表字段名) references 主表名(主键名或unique字段名)```

```sql
-- 演示外键
# 创建主表my_class
create table my_class(
		id int primary key , -- 班级编号
		`name` varchar(32) not null default ''
);

# 创建从表my_stu
create table my_stu(
	  id int primary key ,
		`name` varchar(32) not null default '' ,
		class_id int , -- 学生所在班级编号
		# 指定外键
		foreign key (class_id) references my_class(id)
)
```

### ```foreign key```细节
- 外键指向的表的字段，要求是primary key 或者是 unique
- 表的类型是```innodb``` ，这样的表才支持外键
- 外键字段的类型要和主键字段的类型一致（长度可以不同）
- 外键字段的值，必须在主键字段中出现过，或者为null（前提是外键字段允许为null）
- 一旦建立主外键关系，数据就不能随意删除了

## ```check```
用于强制行数据必须满足的条件，假定在sal列上定义了check约束，并要求sal列值在1000~2000之间，如果不再就会提示出错
</br>
```列名 类型 check(条件)```

```sql
-- 演示check
create table t23(
	id int primary key ,
	`name` varchar(32),
	sex varchar(6) check(sex in ('man','woman')),
	sal double check(sal>1000 and sal<2000)
)
```

# 自增长

```字段名 整型 primary key auto_increment```

## 自增长细节
- 一般来说自增长是和primary key配合使用的
- 自增长也可以单独使用[但是需要配合一个unique]
- 自增长修饰的字段为整数型（虽然小数也可以但是非常非常少使用）
- 自增长默认从1开始，也可以通过命令修改
	
	- ```alter table 表名 auto_increment = xxx``` 

# mysql索引
说起提高数据库性能，索引是最物美价廉的东西。不用加内存，不用改程序，不用调sql，查询速度就可能提高百倍千倍。

## 索引原理

- 没有索引为什么会变慢
> 因为需要全表扫描

- 使用索引为什么会变快
> 形成一个索引的数据结构，比如二叉树

- 索引的代价
> - 磁盘占用
> - 对dml(update delete insert) 语句的效率影响较大（在项目中 select占90%）

![image.png](https://pumpkn.xyz/upload/2021/09/image-5e74887bbd2242c68d58dfb131de341b.png)

## 索引的类型
- 主键索引 主键自动为主索引
- 唯一索引(uqinue)
- 普通索引(index)
- 全文索引(fulltext) 一般开发不适用mysql自带的全文索引。

```sql
-- 索引
create table t1(
	id int primary key , -- 主键 同时也是索引 称为主键索引
	name varchar(32) ,
)

create table t2(
	id int unique , -- id是唯一的，同时也是索引，称为unique索引
)
```

## 索引的使用

- 添加索引

	- ```create [unique] index index_name on table_name (col_name)```
	- ```alter table table_name add index[index_name] (col_name)```

- 添加主键索引
	
	- ```alter table 表名 add primary key(列名)```

- 删除索引

	- ```drop index index_name on table_name```
	- ```alter table table_name drop index index_name```

- 删除主键索引

	- ```alter table t_b drop primary key```


## 演示索引
```sql
-- 演示索引
# 创建索引
create table t25(
	id int ,
	`name` varchar(32)
)

# 查询表是否有索引
show indexes from t25

# 添加索引——方式1
create unique index id_index on t25(id)

# 添加索引——方式2
alter table t25 add index id_index (id)

# 删除索引
drop index id_index on t25

# 添加主键索引——设置主键，自动建立主键索引
alter table t25 add primary key(id)

# 删除主键索引
alter table t25 drop primary key
```

## 索引总结
- 较频繁的作为查询条件字段应该创建索引
- 唯一性太差的字段不适合单独创建索引，即使频繁作为查询条件
- 更新非常频繁的字段不适合创建索引
- 不会出现在where子句中的字段不应该创建索引


# mysql事务

## 什么是事务
事务用于保证数据的一致性，它由**一组相关的dml语句组成**，改组的dml语句要么全部成功，要么全部失败。如：转账就要用事务来处理，用以保证数据的一致性

## 事务和锁
当执行事务操作时(dml语句)，mysql会在表上加锁，防止其他用户该表的数据。这对用户来说是非常重要的。

## mysql控制事务的几个重要操作
- ```start transaction``` 开始一个事务
- ```savepoint 保存点名``` 设置保存点
- ```rollback to 保存点名``` 回退事务
- ```rollback``` 回退全部事务
- ```commit``` 提交事务，所有操作生效，不能回退

```sql
-- 演示事务
# 开始事务
start transaction
# 设置保存点
savepoint a 
# 执行dml操作
insert into t27 values(100 , 'tom') ;

savepoint b
# 执行dml
insert into t27 values(200 , 'jack') ;
# 回退到b
rollback to b
# 继续回退到a
rollback to a 
# 这样表示回退到事务开始的状态
rollback
```

## 提交事务
使用commit语句可以提交事务。当执行了commit语句后，会确认事务的变化、结束事务、删除保存点、释放锁、数据生效。当使用commit语句结束事务后，其他会话将可以查看到事务变化后的新数据。

## 事务细节
- 如果不开始事务，默认情况下，dml操作是自动提交的，不能回滚
- 如果开始一个事务，你没有创建保存点，你可以执行rollback，默认回到事务开始的状态
- 你也可以在这个事务中(还没有提交时)，创建多个保存点。
- 你可以在事务没有提交前，选择回退到任意一个保存点
- mysql的事务机制需要innodb的存储引擎，还可以使用myisam。
- 开始一个事务start transaction, set autocommit = off

## 事务隔离级别

- 多个连接开启各自事务操作数据库中数据时，数据库系统要负责隔离操作，以保证各个连接在获取数据时的准确性
- 如果不考虑隔离性，可能会引发如下问题

	- 脏读
	- 不可重复读
	- 幻读
- **脏读(dirty read)** 当一个事务读取另一个事务尚未提交的修改时，产生脏读
- **不可重复度(nonrepearable read)**: 同一查询在同一事务中多次进行，由于其他提交事务所做的<font color="red">修改或删除</font>，每次返回不同的结果集，此时发生不可重复读
- **幻读(phantom read)**: 同一查询在同一事务中多次进行，由于其他提交事务所做的<font color="red">插入操作</font>，每次返回不同的结果集，此时发生幻读

|隔离级别|脏读|不可重复读|幻读|加锁读|
|-------|-------|-------|-------|-------|
|读未提交(read uncommited)|√|√|√|不加锁|
|读已提交(read committed)|×|√|√|不加锁|
|可重复读(repeatable read)|×|×|×|不加锁|
|可串行化(serializable)|×|×|×|加锁|

mysql默认的事务隔离级别时可重复读，一般情况下，没有特殊要求没有必要修改

## 事务的```acid```特性
- 原子性(atomicity)
> 原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生

- 一致性(consistency)
> 事务必须使数据库从一个一致性状态变换到另外一个一致性状态

- 隔离性(isolation)
> 事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据锁干扰，多个并发事务之间要相互隔离

- 持久性(durability)
> 持久性是指一个事务一旦被提交，他对数据库中的数据的改变是永久性的。

## mysql表类型和存储引擎
- mysql的表类型由存储引擎决定，主要包括MyISAM,innoDB,Mymory等
- mysql数据表主要支持六种类型，分别是CSV、Memory、ARCHIVE、MRG_MYISAM、MYISAM、innoDB
- 这六种又分为两类，一类是"事务安全型"，比如innoDB;其余都属于第二类，称为“非事务安全型”

![image.png](https://pumpkn.xyz/upload/2021/09/image-affabd916fcc466093aa97cfb346278e.png)


## 存储引擎细节说明
- ```MyISAM```不支持事务、也不支持外键，但访问速度快，岁事务完整性没有要求
- ```InoDB```存储引擎提供了具有提交、回滚和崩溃回复能力的事务安全。但是比起MyISAM存储引擎，InnoDB写的处理效率差一些并且会占用更多的磁盘空间以保留数据和索引
- ```Memory```存储引擎使用存在内存中的内容来创建表。每个memory表实际对应一个磁盘文件。memory类型的表访问非常的快，因为它的数据是放在内存中的，并且默认是hash索引。但是一旦服务器，表中的数据就会丢失掉，表的结构还在
- 修改存储引擎 ```alter table 表名 engine = 存储引擎```

# 视图(view)

看一个需求</br>
emp表的列信息很多，有些信息是个人重要信息(比如sal,comm,mgr,hiredate)，如果我们希望某个用户只能查询emp表的(empno、ename、job和deptno)信息，有什么办法？

## 视图基本概念
- 视图是一个虚拟表，其内容由查询定义。同真实的表一样，试图包含列，其数据来自对应的真是表（基表）
- 视图和基表的关系

	- ![image.png](https://pumpkn.xyz/upload/2021/09/image-6b79d365005442da95ead3549c247e1b.png)

## 视图的基本使用
- ```create view 视图名 as select 语句```
- ```alter view 视图名 as select 语句```
- ```show create view 视图名```
- ```drop view 视图名1 , 视图名2```


```sql
# 创建一个视图emp_view01，只能查询emp表的(empno、ename、job和deptno)
create view emp_view01 as select empno , ename , job , deptno from emp
select * from emp_view01
```

## 视图细节
- 创建视图后，在数据库中，对应视图的只有一个视图结构文件(视图名.frm)
- 视图的数据变化会影响到基表，基表的数据变化也会影响到视图
- 视图中可以再使用视图