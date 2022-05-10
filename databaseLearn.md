## #1.sql的分类

/*
DDL 数据定义语言 create\alter 修改结构\drop 删除\rename\truncate:清空表，但是表的结构还在 表的结构的增删改查
DML 数据操作语言 insert\delete\update 更新\select 查询\       表的数据的增删查改
DQL 数据查询语言 select
DCL 数据控制语言 grant revoke
TCL 事务控制语言 commit rollback	

-- 导入数据库的方式

/*方式一：source 文件的全路径
mysql>	source D:\... 引入sql源文件
要用命令行执行
要在某一个database中导入
  方式二：基于具体的图形化界面的工具可以导入数据 

## -- 列的别名

-- as:全称：alias（别名），可以省略 不是英语中的as（作为）
-- 列的别名可以使用双引号引起来。
-- select 是永远不会修改数据的

## 条件格式 

mysql> select * from emp where sal >= 1250 and sal <= 3000; 显示工资在1250 到 3000之间的 与下面的方法同

mysql> select * from emp where sal between 1250 and 3000;

 select * from emp where comm is not null; comm是空

 select * from emp where comm is null;somm不是空

select * from emp where sal>=2500 and job = 'manager'; 工作岗位是管理 工资大于2500的员工；

select * from emp where sal>=2500 or job = 'manager'; 找到岗位是管理或者工资大于2500的员工；

select * from emp where sal>=2500 and (deptno = 10 or deptno = 20);找出部门编号为10或者20的且工资大于2500的员工；

说明：and和or同时出现，and优先级比较高 如果and和or同时出现or的内容加小括号；

select empno,job,ename from emp where job in('manager','salesman'); 工作是管理和销售的人员部分信息；

注意：in不是表示一个区间 而是一个一一对应的关系

select empno,job,ename from emp where sal not in(5000,3000)；工资不是3000和5000的员工

## 14 模糊查询：

like '' 的使用

%是一个特殊的符号 。_下划线也是一特殊的字符

%匹配任意多的字符

_任意一个字符

 select ename from emp where ename like '%O%'; 找到名字里面含有O的员工

select * from emp where ename like '_M%'; 找到名字第二个字母是m的员工

转义字符

_的转义\ \_

## 15 排序：

order by 默认是升序

sac指定升序 desc降序

多个字段排序：

按照多个字段排序；

有主有次 要求按照薪资升序 再按名字的升序排列

select

​	\*

from 

​	emp 

order by 

​	sal asc,ename asc; 在前的是起到主导作用的；

根据字段的位置也可以排序

select ename, sal from emp order by 2; 用第二列排序 

了解一下，不建议开发中这样使用，应为不健壮 删除一列会导致排序逻辑变化

select * from emp where sal between 1250 and 3000 order by sal desc;

## 16 条件加排序的综合处理：

严格按照一个顺序

select 

...

from

...

where

....

order by

 ...

先from

在where

在select

在order by



排序总是在最后执行



## 17 数据处理函数

数据处理函数又称为单行处理函数

单行处理函数的特点是一个输入对应一个输出 (单参数函数)

多行处理函数是多个输入对应一个输出(多参数函数)

单行处理函数常见的有哪些

![1651320477124](C:\Users\ADMINI~1\AppData\Local\Temp\1651320477124.png)

lower函数 select lower(ename) 'lowername' from emp; 14个输入对应14个输出 这是单处理函数的特点

 select upper(ename) 'lowername' from emp;大写

取子串 substr();

select substr(ename,1,1) 'kk' from emp; 其实位置以一为起始；

select substr(ename,1,2),job,sal from emp where substr(ename,1,1) = 'S';截取子串

concat 函数进行字符串的拼接；

length()取字符串长度

trim()去除前后空白；select * from emp where ename = trim('king');

str_to_data;

data_fromat: 格式化日期

format 设置千分位

round 四舍五入

ifnull 可以将null转化为一个具体的值

select round(1234.567,2) as result from emp; 保留两位小数 = 1234.57

select round(1234.567,1) as result from emp；保留一位小数 =1234.6

select round(1234.567,-1) as result from emp；十位1240

select round(1234.567,-2) as result from emp；百位 1200；

 select round(rand()*100) as result  from emp;得到一百以内的随机数

凡是空参加的运算最终结果都是null；

select ename,job,sal,(sal+ifnull(comm,0)) as anusal_comout from emp;  ifnull(object,result)函数前面写判断对象 后面写是null的时候对象当作什么？

case...when...then....when...than....else.....when.....; 当什么什么如何当什么什么如何如何此外当如何如何

不修改数据只是将查询结果显示为工资上调

实例：

select ename,job,sal oldersal,(case job when 'manager'then sal*1.1 when 'salesman' then sal*1.5 else sal end) as newsal from emp；

当员工为管理的时候工资提高百分之十 当员工为销售人员时工作提高百分之五十 其他的不变；

## 18 分组函数

多行处理函数 一共有五个；

count() 计数

sum（）求和

avg（）求平均值

max（）求最大值

min（）求最小值

mysql> select sum(sal),ename  from emp;
+----------+----------+
| sum(sal) | ename    |
+----------+----------+
| 39025.00 | SeaOcean |
+----------+----------+


mysql> select avg(sal) from emp;
+-------------+
| avg(sal)    |
+-------------+
| 2601.666667 |
+-------------+


mysql> select min(sal) from emp;
+----------+
| min(sal) |
+----------+
|   800.00 |
+----------+


mysql> select count(sal) from emp;
+------------+
| count(sal) |
+------------+
|         15 |
+------------+

分组函数使用时要先分组 如果没有分组默认是整张表是一个组

分组函数在使用的时候要只注意哪些问题；

一、分组函数自动处理null，不需要提前对null进行处理；

二、分组函数中count(*)和count具体字段有什么区别

count(具体字段)：表示统计字段下所有不为null的字段

count(*)：统计表中的总行数

三、分组函数不能直接使用在where子句中；

# 19 分组查询

什么是分组查询：

先分组 再分组操作

select

​	...

from 

​	...

where

​	....

group by

​	...

执行顺序：

1 from 2where 3group 4select 5order by

计算每个部门的工资和

计算每个岗位的平均工资

mysql> select job,sum(sal) from emp  group by job order by sal;
+-----------+----------+
| job       | sum(sal) |
+-----------+----------+
| CLERK     |  4150.00 |
| SALESMAN  |  5600.00 |
| ANALYST   |  6000.00 |
| PRESIDENT |  5000.00 |
| MANAGER   | 18275.00 |
+-----------+----------+

### 重点结论：

在一条select语句中，如果有group by 语句的话，select后面只能跟：参加分组的字段及其分组函数。其他一律不行

mysql> select deptno,max(sal) from emp group by deptno; 找到各个部门最大新资

+--------+----------+
| deptno | max(sal) |
+--------+----------+
|     20 | 10000.00 |
|     30 |  2850.00 |
|     10 |  5000.00 |
+--------+----------+

如何用两个关键字分组？

+-------+----------+-----------+------+------------+----------+---------+--------+
| EMPNO | ENAME    | JOB       | MGR  | HIREDATE   | SAL      | COMM    | DEPTNO |
+-------+----------+-----------+------+------------+----------+---------+--------+
|  1001 | SeaOcean | MANAGER   | 8848 | 1998-09-26 | 10000.00 |    NULL |     20 |
|  7369 | SMITH    | CLERK     | 7902 | 1980-12-17 |   800.00 |    NULL |     20 |
|  7499 | ALLEN    | SALESMAN  | 7698 | 1981-02-20 |  1600.00 |  300.00 |     30 |
|  7521 | WARD     | SALESMAN  | 7698 | 1981-02-22 |  1250.00 |  500.00 |     30 |
|  7566 | JONES    | MANAGER   | 7839 | 1981-04-02 |  2975.00 |    NULL |     20 |
|  7654 | MARTIN   | SALESMAN  | 7698 | 1981-09-28 |  1250.00 | 1400.00 |     30 |
|  7698 | BLAKE    | MANAGER   | 7839 | 1981-05-01 |  2850.00 |    NULL |     30 |
|  7782 | CLARK    | MANAGER   | 7839 | 1981-06-09 |  2450.00 |    NULL |     10 |
|  7788 | SCOTT    | ANALYST   | 7566 | 1987-04-19 |  3000.00 |    NULL |     20 |
|  7839 | KING     | PRESIDENT | NULL | 1981-11-17 |  5000.00 |    NULL |     10 |
|  7844 | TURNER   | SALESMAN  | 7698 | 1981-09-08 |  1500.00 |    0.00 |     30 |
|  7876 | ADAMS    | CLERK     | 7788 | 1987-05-23 |  1100.00 |    NULL |     20 |
|  7900 | JAMES    | CLERK     | 7698 | 1981-12-03 |   950.00 |    NULL |     30 |
|  7902 | FORD     | ANALYST   | 7566 | 1981-12-03 |  3000.00 |    NULL |     20 |
|  7934 | MILLER   | CLERK     | 7782 | 1982-01-23 |  1300.00 |    NULL |     10 |
+-------+----------+-----------+------+------------+----------+---------+--------+

select job,deptno,max(sal) from emp group by deptno,job order by deptno;
+-----------+--------+----------+
| job       | deptno | max(sal) |
+-----------+--------+----------+
| CLERK     |     10 |  1300.00 |
| MANAGER   |     10 |  2450.00 |
| PRESIDENT |     10 |  5000.00 |
| ANALYST   |     20 |  3000.00 |
| CLERK     |     20 |  1100.00 |
| MANAGER   |     20 |  2975.00 |
| CLERK     |     30 |   950.00 |
| MANAGER   |     30 |  2850.00 |
| SALESMAN  |     30 |  1600.00 |
+-----------+--------+----------+

select job,deptno,max(sal) from emp group by deptno,job having max(sal) > 3000;
+-----------+--------+----------+
| job       | deptno | max(sal) |
+-----------+--------+----------+
| PRESIDENT |     10 |  5000.00 |
+-----------+--------+----------+
1 row in set (0.00 sec)

以上sql语句执行效率较低

可以考虑先将大于3000的找出来 在分组

mysql> select deptno,job from emp where sal > 3000 group by deptno,job;
+--------+-----------+
| deptno | job       |
+--------+-----------+
|     10 | PRESIDENT |
+--------+-----------+

找出每个部门平均新资，要求显示平均新资高于2500的；

还没分组时不能使用分组函数

mysql> select avg(sal),deptno from emp group by deptno having avg(sal)>2500;
+-------------+--------+
| avg(sal)    | deptno |
+-------------+--------+
| 2916.666667 |     10 |
+-------------+--------+

能用where用where 没有where用having；

总结：

select

​	.........

from

​	...........

where 

​	......

group by

​	........

having

​	.......

order by

​	..........

## 20 distinct

distinct只能写在所有字段的最前方

## 21 连接查询

2.1 什么时连接查询？

冲一张表中单独查询，称为单独查询

emp表和dept表联合起来查询，从emp表中取出员工名字，从dept表中查出部门名称。

这种跨表查询，多张表联合起来查询数据，被称为连接查询。

2.2 连接查询的分类？

  根据语法的年代分类：

SQL92：上世纪92年出现的语法

SQL99：上世纪99年出现的语法

我们在这里重点学习SQL99 

根据表连接的方式分类：

内连接：

等值连接 非等值连接 自联结

外连接：

左连接 右连接

全连接（不讲）：几乎不用

2.3

当两张表进行连接查询时，没有任何条件限制会发生什么情况？

案例：查询每个员工的所在的部门名称？

mysql> select ename,dname from emp,dept;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | OPERATIONS |
| SMITH  | SALES      |
| SMITH  | RESEARCH   |
| SMITH  | ACCOUNTING |
| ALLEN  | OPERATIONS |
| ALLEN  | SALES      |
| ALLEN  | RESEARCH   |
| ALLEN  | ACCOUNTING |
| WARD   | OPERATIONS |
| WARD   | SALES      |
| WARD   | RESEARCH   |
| WARD   | ACCOUNTING |
| JONES  | OPERATIONS |
| JONES  | SALES      |
| JONES  | RESEARCH   |
| JONES  | ACCOUNTING |
| MARTIN | OPERATIONS |
| MARTIN | SALES      |
| MARTIN | RESEARCH   |
| MARTIN | ACCOUNTING |
| BLAKE  | OPERATIONS |
| BLAKE  | SALES      |
| BLAKE  | RESEARCH   |
| BLAKE  | ACCOUNTING |
| CLARK  | OPERATIONS |
| CLARK  | SALES      |
| CLARK  | RESEARCH   |
| CLARK  | ACCOUNTING |
| SCOTT  | OPERATIONS |
| SCOTT  | SALES      |
| SCOTT  | RESEARCH   |
| SCOTT  | ACCOUNTING |
| KING   | OPERATIONS |
| KING   | SALES      |
| KING   | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | OPERATIONS |
| TURNER | SALES      |
| TURNER | RESEARCH   |
| TURNER | ACCOUNTING |
| ADAMS  | OPERATIONS |
| ADAMS  | SALES      |
| ADAMS  | RESEARCH   |
| ADAMS  | ACCOUNTING |
| JAMES  | OPERATIONS |
| JAMES  | SALES      |
| JAMES  | RESEARCH   |
| JAMES  | ACCOUNTING |
| FORD   | OPERATIONS |
| FORD   | SALES      |
| FORD   | RESEARCH   |
| FORD   | ACCOUNTING |
| MILLER | OPERATIONS |
| MILLER | SALES      |
| MILLER | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+

没有任何条件限制 14 *4 = 56

当两张表进行连接查询，没有任何条件限制时候，最终的查询条数，是两张表条数的乘积，这种现象

叫笛卡尔积现象；

如何避免笛卡尔积现象：那就是连接时加条件

满足条件的记录筛选出来

mysql> select ename,dname from emp,dept where emp.deptno = dept.deptno;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+

思考：最终查询的结果是14条 但是匹配的过程中匹配的次数没有减少

给表起别名 同时加条件

mysql> select e.ename,d.dname from emp e,dept d  where e.deptno = d.deptno;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)

上面的是92语法

注意：通过笛卡尔积现象得出，表的连接次数越多，效率越低

2.5

内连接之等值连接

查询内个员工所在的部门名称显示员工名和部门名

将emp表和dept表进行连接，条件是e.deptno = d.deptno

SQL99语法：

select

e.ename,d.dname 

from 

emp e

join

dept d

on

e.deptno = d.deptno;

join...on...



mysql> select * from salgrade;
+-------+-------+-------+
| GRADE | LOSAL | HISAL |
+-------+-------+-------+
|     1 |   700 |  1200 |
|     2 |  1201 |  1400 |
|     3 |  1401 |  2000 |
|     4 |  2001 |  3000 |
|     5 |  3001 |  9999 |
+-------+-------+-------+
5 rows in set (0.00 sec)

mysql> select * from emp;
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
14 rows in set (0.00 sec)

## Innerjoin

mysql> select e.sal,e.ename,s.grade from emp e join salgrade s on e.sal between s.losal and s.hisal;
+---------+--------+-------+
| sal     | ename  | grade |
+---------+--------+-------+
|  800.00 | SMITH  |     1 |
| 1600.00 | ALLEN  |     3 |
| 1250.00 | WARD   |     2 |
| 2975.00 | JONES  |     4 |
| 1250.00 | MARTIN |     2 |
| 2850.00 | BLAKE  |     4 |
| 2450.00 | CLARK  |     4 |
| 3000.00 | SCOTT  |     4 |
| 5000.00 | KING   |     5 |
| 1500.00 | TURNER |     3 |
| 1100.00 | ADAMS  |     1 |
|  950.00 | JAMES  |     1 |
| 3000.00 | FORD   |     4 |
| 1300.00 | MILLER |     2 |
+---------+--------+-------+

条件不是一个等量关系 成为非等值连接；

2.7 内连接之自联结

案例：查询员工的上级领导，要求显示员工名对应的领导名

mysql> select a.ename worker,b.ename boss from emp a join emp b on a.mgr = b.empno;
+--------+-------+
| worker | boss  |
+--------+-------+
| SMITH  | FORD  |
| ALLEN  | BLAKE |
| WARD   | BLAKE |
| JONES  | KING  |
| MARTIN | BLAKE |
| BLAKE  | KING  |
| CLARK  | KING  |
| SCOTT  | JONES |
| TURNER | BLAKE |
| ADAMS  | SCOTT |
| JAMES  | BLAKE |
| FORD   | JONES |
| MILLER | CLARK |
+--------+-------+

一张表 一个别名叫A 一个别名叫B

以上就是内连接中的自联结 

## 外连接：

内连接的特点是完全能够匹配上这个条件的数据查询出来。

select

e.ename,d.dname 

from 

emp e

rightjoin

dept d

on

e.deptno = d.deptno;

right代表什么：表是将join关键字右边的整张表看成主要

表，主要是为了将这张表的数据显示出来，捎带

关联右边的表；

在外连接中表与表中存在主次关系 内连接中的表示平等的关系

带有right的是右外连接，又叫右连接

带有right的是左外连接，又叫左连接

左右连接是可以相互转化的；

外连接的查询结果条数一定是大于内连接查询结果条数的；

案例：查询每个员工的上级领导，要显示所有员工的名字和领导名字

mysql> select a.ename,b.ename from emp a left join emp b on a.mgr = b.empno;
+--------+-------+
| ename  | ename |
+--------+-------+
| SMITH  | FORD  |
| ALLEN  | BLAKE |
| WARD   | BLAKE |
| JONES  | KING  |
| MARTIN | BLAKE |
| BLAKE  | KING  |
| CLARK  | KING  |
| SCOTT  | JONES |
| KING   | NULL  |
| TURNER | BLAKE |
| ADAMS  | SCOTT |
| JAMES  | BLAKE |
| FORD   | JONES |
| MILLER | CLARK |
+--------+-------+

## 2.9 多表如何连接

语法一：

select 

​	...

from

​	A

join

  	B

on

​	a与b连接的条件

join

​	C

on

​	a和c的连接条件

join

​	d

on 

​	a和d连接的条件

案例：找出每个部门的员工名称，同时找出每个员工的工资等级；

要求显示员工名 部门名 薪资等级

mysql> select e.ename,d.dname,s.grade from emp e left join dept d on e.deptno = d.deptno join salgrade s on e.sal between s.losal and s.hisal ;
+--------+------------+-------+
| ename  | dname      | grade |
+--------+------------+-------+
| SMITH  | RESEARCH   |     1 |
| ALLEN  | SALES      |     3 |
| WARD   | SALES      |     2 |
| JONES  | RESEARCH   |     4 |
| MARTIN | SALES      |     2 |
| BLAKE  | SALES      |     4 |
| CLARK  | ACCOUNTING |     4 |
| SCOTT  | RESEARCH   |     4 |
| KING   | ACCOUNTING |     5 |
| TURNER | SALES      |     3 |
| ADAMS  | RESEARCH   |     1 |
| JAMES  | SALES      |     1 |
| FORD   | RESEARCH   |     4 |
| MILLER | ACCOUNTING |     2 |
+--------+------------+-------+

案例：找出每个员工的部门名称以及工资等级还有上级领导

mysql> select e.ename,d.dname,s.grade,e2.ename from emp e join dept d on e.deptno = d.deptno join salgrade s on e.sal between s.losal and s.hisal left join emp e2 on e.mgr = e2.empno;
+--------+------------+-------+-------+
| ename  | dname      | grade | ename |
+--------+------------+-------+-------+
| SMITH  | RESEARCH   |     1 | FORD  |
| ALLEN  | SALES      |     3 | BLAKE |
| WARD   | SALES      |     2 | BLAKE |
| JONES  | RESEARCH   |     4 | KING  |
| MARTIN | SALES      |     2 | BLAKE |
| BLAKE  | SALES      |     4 | KING  |
| CLARK  | ACCOUNTING |     4 | KING  |
| SCOTT  | RESEARCH   |     4 | JONES |
| KING   | ACCOUNTING |     5 | NULL  |
| TURNER | SALES      |     3 | BLAKE |
| ADAMS  | RESEARCH   |     1 | SCOTT |
| JAMES  | SALES      |     1 | BLAKE |
| FORD   | RESEARCH   |     4 | JONES |
| MILLER | ACCOUNTING |     2 | CLARK |
+--------+------------+-------+-------+

## 22子查询

什么是子查询？

select 语句中嵌套select语句，被嵌套的语句被称为子查询。

子查询可以出现在哪里？

select

.....(select)

from 

.....(select)

where

........(select)

3.3where子句中出现子查询

案例 ：找出比最低工资高的员工姓名和工资

select ename,sal from emp where sal > (select min(sal) from emp);

3.4 from子句中的查询

注意：from后面的子查询可以将子查询的查询结果当一张临时表来看待；

案例：找出每个岗位的平均工资的工资等级

mysql> select t.*,s.grade from (select job,avg(sal) as avgsal from emp group by job) t join salgrade s on t.avgsal between s.losal and s.hisal;
+-----------+-------------+-------+
| job       | avgsal      | grade |
+-----------+-------------+-------+
| CLERK     | 1037.500000 |     1 |
| SALESMAN  | 1400.000000 |     2 |
| MANAGER   | 2758.333333 |     4 |
| ANALYST   | 3000.000000 |     4 |
| PRESIDENT | 5000.000000 |     5 |
+-----------+-------------+-------+

非常值得注意的是 arg() 要重命名才能在no里面使用

3.5 select后面的子查询

注意：对于select后面的子查询来说，这个只能一次返回一个结果，多于一条，就会报错；

## 23 合并查询结果集

案例：查询工作岗位是manager和salesman的员工

mysql> select ename,job from emp where job in ('manager','salesman')
    -> ;
+--------+----------+
| ename  | job      |
+--------+----------+
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| JONES  | MANAGER  |
| MARTIN | SALESMAN |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
| TURNER | SALESMAN |
+--------+----------+
7 rows in set (0.00 sec)

mysql> select ename,job from emp where job = 'manager'
    -> union
    -> select ename,job from emp where job = 'salesman'
    -> ;
+--------+----------+
| ename  | job      |
+--------+----------+
| JONES  | MANAGER  |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| MARTIN | SALESMAN |
| TURNER | SALESMAN |
+--------+----------+

其中union的效率高一些，对于表连接来说，每次连接一张新表，则匹配的次数满足笛卡尔积

但是union可以减少匹配的次数。在减少匹配次数的情况下，还能完成两个结果集的拼接

注意事项：union要求在进行结果集合并时要求两个结果集的列数相同。数据类型也要一样

### 5、limit

将查询结果集的一部分取出来，通常在分页查询中；

按照薪资降序，取排名前五的员工

mysql> select sal from emp order by sal desc limit 0,5;
+---------+
| sal     |
+---------+
| 5000.00 |
| 3000.00 |
| 3000.00 |
| 2975.00 |
| 2850.00 |
+---------+
5 rows in set (0.00 sec)

## limit start, length;

注意的是limit是在order by 之后执行的

取出工作排在5-9的员工

每页显示3条记录

第一页 limit0，3

第二页 limit 3，3

第三页 limit 6，3

每页显示pageSize条数记录：

第pageNo页：limit （pageNo - 1)*pageSize ,PageSize

from where group_by having select order_by limit

## 23 建表的语法

name varchar(15) default ‘m’;  设置默认值。

str_to_date('01-09-1998','%d-%m-%Y');

date_format(date,'%Y/%m/%d')

date 和datetime 的区别

date:年月日

datetime：年月日时分秒

now()：系统当前时间 并且时间带有时分秒信息

### 修改：update

## &update&  emp  &set&  ename = 'wanghaoyang'  &where& ename = 'king';

mysql> update emp set ename='seaocean' where ename = 'king';
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from emp;
+-------+----------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME    | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+----------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH    | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN    | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD     | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES    | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN   | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE    | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK    | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT    | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | seaocean | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER   | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS    | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES    | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD     | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER   | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+----------+-----------+------+------------+---------+---------+--------+
14 rows in set (0.00 sec)

## 24 删除的语法

delete from emp where name ename = 'seaocean';

insert语句可以一次插入多条记录？

insert into emp value (...),(.....),(.......);

快速复制表

create table emp2 as select * from emp; 可以完成表的快速赋值

很少用 学个乐 //记录时可以重复的

快速删除表内的数据：

delete语句的原理：只是删除记录的指针但是不会清空磁盘中的数据，其实还是可以回滚的。

start transaction；开启事务

rollback ；回滚

删除效率比较低。

快速删除的方法：truncate

这种删除效率比较好，表被一次截断，物理删除。

缺点：不支持回滚

优点：高效

使用truncate之前一定要警告客户

## 25对表结构的增删查改 可以使用使用sqlyog

实际开发中很少使用表结构的修改

开放中修改表结构的代价很高

动表的结构就需要动大量的java代码 这样的责任应该由设计人员承担

如果真的要修改 可以使用程序

修改表结构不需要java 不是java的范畴

## 26 创建表的时候加入约束：

什么是约束？

约束对应的英语单词是constraint

在创建表的时候 我们可以在表中的一个字段加上一些约束，来保证这个表中的数据完整性，有效性！！！

约束作用就是为了保证：表中的数据有效；

约束的一些形式：

​	not null;	非空

​	unique;	唯一

​	primary key;主键

​	foreign key;外键

实例：

create table students(

studentno int not null,

score int

)

### 小插曲

xxx.sql这种文件被成为sql脚本文件

sql脚本文件中的编写的大量的sql语句。我们执行的时候，该文件中的sql语句会全部执行！ 

批量的执行的sql，可以使用sql脚本文件。在mysql中怎么使用脚本文件呢？

在你实际工作中，第一天到了公司，项目经理会给你一个sql文件，source一下在自己的数据库中就会出现这些数据

联合唯一性：要求name和email两个字段联合起来具有唯一性

create table emp(

id int,

name varchar(15),

email varchar(30);

unique(name,email)

);

此时是name和email联合起来唯一；

注意：约束直接加到列后面的叫列级约束 如果没有放在列后面的叫表级约束

什么时候使用表级约束？

需要给多个字段联合起来进行约束的叫标记约束。

not null只有列级约束，没有标记约束的语法。

在mysql中not noll unique 和primary是等价的； 在甲骨文中是不能这样的

# 27 主键约束

主键约束的相关术语：

主键字段：添加了主键约束的字段 

主键值：主键的值 

主键约束：主键的约束

什么是主键 有什么用处？

主键值是每一行记录的唯一标识

主键值是每一行记录的身份证

注意：任何一张表都应该有i一个主键，没有主键表无效

主键的特征：主键具有唯一性，不可重复性

create students(

name varchar(15)，

stdno bigint primary key,

score int

);

主键可以使用标记约束

create students(

name varchar(15)，

stdno bigint ,

score int,

primary(stdno,score)

);

## 28 外键

create t_class(

classno int primary key;

classname varchar(20)

);

create students(

name varchar(20),

classno int,

foreign key(classno) reference t_class(classno)

)

## 29 存储引擎

innoDB

myisam

momery

## 30 事务：

事务开：start transaction

事务结束：rollback        commit

事务具有四条性质：

A 原子性

B 一致性

I 隔离性

D 持久性  

重点研究一下事务的隔离性：

隔离涉及到隔离级别 一共有四个隔离级别

​	1 读未提交 read uncommitted

事务a可以读到事务b未提交的数据，这种隔离可能读到脏数据。这种隔离级别一般是理论上的。

​	2 读已提交 read committed 

*oracle*的默认隔离级别就是读已提交

不可重复读 一个事务里面可能两次读到的数据不同

解决了脏读但是不可重复读取数据，

​	3 可重复读 repeatable read 提交之后也读不到 读的都是开启事务时的数据

可重复读解决了不可重复读的问题，但是可能出现幻影读 每次读的数据都是幻象，不真实

​	4 序列化 serializable

最高的隔离级别，效率最低。解决了所有的问题。种族隔离级别表示事务排队，不能并行。

## 索引

 什么是索引？

索引是在数据库的字段上添加的，是为了提高查询效率存在的一种机制。一张表的一个字段可以添加一个索引，当然，多个字段联合起来也可以添加索引。索引相当于一本书的目录，是为了缩小扫描范围而存在的一种机制。

加索引可以提高效率 但是要视情况而定。

在mysql数据库中索引也是需要排序的，并且这个索引的排序和TREESET数据结构相同。TreeSet底层是一个自平衡的二叉树！在mysql当中是一个B-Tree数据结构。

索引的实现原理：

提示：在任何一个数据库中主键会自动添加索引对象，id字段上有索引，以为id是pk的，另外mysql当中一个字段如果有unique约束的话，也会自动创建索引对象。在任何数据库中，任何一张表的任何一个记录都在硬盘上存储都有物理存储编号。在mysql当中，索引是一个单独的对象，不同的存储引擎以不同的形式存在，在myisam存储引擎中索引存在于一个.MYI文件中。在innodb存储引擎中索引存储在一个逻辑名称叫做tablespace的当中。在memory中存储在内存中，不管索引存储在哪里，索引在mysql中是一种树的形式纯在

在什么情况下添加索引：

​	条件一：数据量庞大 和硬件情况有关系

​	条件二：这个字段经常出现在where后面 常常在条件子句中出现

​	条件三：该字段很少的DML操作(insert delete update)

​	不建议随意添加索引，建议通过unique键来查 效率高一些

索引怎么创建？索引怎么删除？语法是什么？

create index emp_ename_index on emp(ename);

drop index emp_ename_index on emp;

索引有时候会失效，什么时候会失效呢？

#### 失效一

select * from emp where name like '%T';

ename 上即使添加了索引，也不会查索引

原因是模糊查询当中以%开头了

尽量避免模糊查询的时候以‘%’开始

这是一种优化的手段

#### 失效二

使用or会失效 如果使用or要求两边都要有索引 才会使用索引 不然会失效

这是玩什么不建议使用or的原因

####  失效三

使用符合索引的时候，没有使用左侧的列的时候 索引会失效

#### 失效四

在where中索引列参加了运算，索引失效。

#### 失效五

在where中索引使用了函数；单列函数

### 索引的分类：

索引是mysql数据库进行优化的重要手段

# 视图(view)

视图是什么？

view站在不同的角度看同一份数据

#### 创建视图对象的方法？

create view emp_view as select * from emp; 和复制有些相似

#### 删除视图对象的方法？

drop view emp_view;

注意：只有DQL语句可以view的形式创建

#### 有了视图可以做什么？

对视图进行增删改查会导致原表被操作，视图是用来简化sql的

将常用的sql语句，用view保存起来。retrive（检索） CRUD

DBA 授权语句

导入导出命令

​	导出Dos下使用： mysqldump mybase >C:\mybase.sql -uroot -pwhy123456

​	导入：source   要在库内

三范式

第一范式：每个表都要有主键，并且每一个字段都要有原子性 不可再分； 比如电话号码和邮箱地址要分为两个

第二范式：建立在第一范式的基础上，要求所有非主键字段完全依赖主键 不要依赖部分主键。一个表只写一个对象的内容，要解耦合。使用*三张表表示多对多的关系 如：教师表 学生表 和教师学生关系表*   单一主键满足完全依赖

## 多对多 三张表 关系表两个外键

第三范式：建立在第二范式的基础上，要求使用的非主键字段必须直接依赖主键，不能传递依赖

## 一对多 两张表  多的表加外键

一对一有时候要拆分表 一对一 外键唯一

## for updata; 行级锁 又叫 悲观锁 不许别的线程修改select的行记录 

乐观锁支持多线程 悲观锁不支持多线程  打完空间的的那款我









