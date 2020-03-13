# 数据库-MySQL-编程基本概念

基本命令
```mysql
#查看数据库引擎，语法不区分大小写
show engines;
/*
问：查询语句不同元素（where、jion、on、limit、group by、having 等等）执行先后顺序？
查询中用到的关键词主要包含六个，并且他们的顺序依次为 select--from--where--group by--having--order by
其中 select 和 from 是必须的，其他关键词是可选的，这六个关键词的执行顺序 与 sql 语句的书写顺序并不是一样的，而是按照下面的顺序来执行:
from:需要从哪个数据表检索数据
where:过滤表中数据的条件
group by:如何将上面过滤出的数据分组
having:对上面已经分组的数据进行过滤的条件
select:查看结果集中的哪个列，或列的计算结果
order by :按照什么样的顺序来查看返回的数据
from 后面的表关联，是自右向左解析 而 where 条件的解析顺序是自左而右的。
也就是说，在写 SQL 文的时候，尽量把数据量小的表放在最右边来进行关联（用小表去匹配大表），而把能筛选出小量数据的条件放在 where 语句的最左边 （用小表去匹配大表）
*/
```

创建数据库 和 注释，增、删、改表
```mysql
-- 三种注释之一，两杠后加一空格
#三种注释之二，python语言风格
/*三种注释之三，C语言风格*/
#语法：不区分大小写，句末记得添加分号

#创建数据库，如果存在则先删除, #数据库名一旦创建好像不好修改，删除重建吧
drop database if exists `school`;
create database `school` default character set utf8;
use `school`;

#创建表格，字段可选类型：int、char、varchar、datetime、smallint、
auto_increment #自增
primary key(column) #主键
engine=InnoDB default charset=utf8; #设置引擎和编码
#设置外键，将产品 cid，绑定到 Category 的主键 id上
constraint fk_product_category foreign key (cid) references category (id);

#注意字段上的点，叫间隔号,建删表时不可省略，查询等操作可省略。
drop table if exists `students`; 
create table `students`(
    `id` int not null auto_increment,
    `name` varchar(20) not null,
    `sex` smallint not null,
    `age` int not null,
    `in_time` datetime not null,
    primary key(`id`)
) default charset 'UTF8';

#修改表格
ALTER TABLE `student` RENAME TO `students`; #修改表名
ALTER TABLE `students` add column `age` int not null after `sex`; #为已有表格增加一列
ALTER TABLE students DROP column nickname; #删除表格格的某一列

#查看创建表格的SQL语句
show CREATE table table_name;

```

### 在一个表中查询
四种基本操作：INSERT、DELETE、UPDATE、SELECT（主要考察 select，所有后面着重介绍）
```mysql
#VALUES (),();可以一次性插入多条记录，提升性能，但不是每种数据库都支持这种操作。
INSERT INTO students(name,sex,age,in_time) VALUES ('张四',1,25,curdate());

#where age in (20,21,22); 可以一次删除多条记录，提升性能。
DELETE FROM students where age = 20;

UPDATE students set sex = 0 where id = 1;
#一般都用update更新，但有时项目需求为“无则插入，有则更新”，需要用到merge。
#如何使用还需要好好搜索一下
merge into 图书表 a
using 图书进货表 b
on(a.ISBN = b.ISBN)
when matched then 
	update set a.name = b.name
when not matched then 
	insert into a(ISBN,name) values(b.ISBN,b.name)
```

### 基本查询语句
```mysql
#基本关键字和函数包括
函数：max、min、sum、avg、count、distinct、
关键字：group by、having、order by[DESC、ASC]、limit、regexp、like
赋予权限的关键词：GRANT
select length(filename) from tablename; #查看某个字段的数据长度，字节记

#日期+时间，日期，时间
now();curdate();curtime();

```

* **问：查找年龄最大的学生的所有信息？**
```mysql
SELECT MAX(age) from students;
SELECT * FROM students WHERE age = (SELECT MAX(age) from students);
```

* **问：查找年龄在 20-23 岁的学生？**
```mysql
SELECT * FROM students WHERE age>=20 and age<=23;
```

* **问：统计表中男女学生的人数？**
* **进阶：统计不同年龄的人数，人数大于等与 2 **
```mysql
#关键字：count，group by，having
SELECT sex,count(*) num FROM students GROUP BY sex;
SELECT age,count(id) as num FROM students GROUP BY age HAVING num >= 2;
```

* **问：将学生按照年龄倒序排，按照 id 升序排？**
```mysql
#关键字 order by, DESC,ASC
SELECT * FROM students ORDER BY age DESC, id ASC;
```

* **问：查找姓名以 a 开头 或者以 b 开头的学生？**
```mysql
#关键字：regexp
SELECT * FROM students WHERE name REGEXP '^a|^b';
SELECT * from students where name like 'a%' or name like 'b%';
/*
LIKE子句
在 where like 的条件查询中，SQL 提供了四种匹配方式。
%：表示任意 0 个或多个字符。可匹配任意类型和长度的字符，有些情况下若是中文，请使用两个百分号（%%）表示。
_：表示任意单个字符。匹配单个任意字符，它常用来限制表达式的字符长度语句。
[]：表示括号内所列字符中的一个（类似正则表达式）。指定一个字符、字符串或范围，要求所匹配对象为它们中的任一个。
[^] ：表示不在括号所列之内的单个字符。其取值和 [] 相同，但它要求所匹配对象为指定字符以外的任一个字符。
查询内容包含通配符时,由于通配符的缘故，导致我们查询特殊字符 “%”、“_”、“[” 的语句无法正常实现，而把特殊字符用 “[ ]” 括起便可正常查询。

'%a'     //以a结尾的数据
'a%'     //以a开头的数据
'%a%'    //含有a的数据
'_a_'    //三位且中间字母是a的
'_a'     //两位且结尾字母是a的
'a_'     //两位且开头字母是a的
*/

/*
REGEXP ：正则表达式
'a$'     //以a结尾的数据
'^a'     //以a开头的数据
'a'    //含有a的数据
'^[a-z]|end$'    //以字母开头或者以end结尾
*/

```

* **问：查找所有年龄，不同年龄只显示一次？**
```mysql
#关键字：distinct
SELECT DISTINCT age FROM students;
#或者使用group by，并且效率会更高
SELECT age from students GROUP BY age;
#下面介绍了计算'2019-03-11'这一天访问过页面的所有用户数
SELECT count(user_id) FROM (SELECT user_id FROM visit WHERE date = '2019-03-11'  GROUP BY user_id) f
SELECT count(user_id) FROM (SELECT DISTINCT user_id FROM visit WHERE date = '2019-03-11') f
SELECT count(DISTINCT user_id) FROM visit WHERE date = '2019-03-11'

```

**问：一道写 SQL 语句的题，计算学生的平均成绩和总成绩？**

```mysql
#关键字：AVG,sum
select stuNum, avg(score) average from course GROUP BY stuNum;
select stuNum, sum(score) total from course GROUP BY stuNum ORDER BY total DESC;
```

**问：给定一个表，找出每门成绩（grade）都大于等于80分的学生姓名？**
[参考博客](https://www.cnblogs.com/chenlin/p/7412253.html)

```mysql
#exists
SELECT * from (select DISTINCT stuNum from course) course1
where not EXISTS 
(select * from (select DISTINCT stuNum from course where score < 80) course2 
WHERE course1.stuNum=course2.stuNum);
#解释一：为什么套两层，因为两个语句处理的是一个表，需要重新命名
#解释二：where语句不能少，这里是一个等号！解析从左到右，如果表2中存在，则返回true，否则返回false，具体参考下面exists的用法。

#in
select DISTINCT stuNum from course
where stuNum not in (select DISTINCT stuNum from course where score < 80);

/*not exists 参考博客：https://www.cnblogs.com/cjm123/p/8177017.html
把最重要的给忘记了，where表的解析是从左到右。
exists : 强调的是是否返回结果集，不要求知道返回什么，而 exists 与 in 最大的区别在于 in 引导的子句只能返回一个字段。
*/

```

**关键字：limit**
查询某些数据，如前三条，后三条，中间某些等等
```mysql
#问：查找年龄第三小的员工所有信息？
SELECT * from students ORDER BY age LIMIT 2,1;
#limit start num;
SELECT * from students LIMIT 5;#只要前五条数据，0 省略，表示为0，5。
SELECT * from students LIMIT 5,10;#从第 6 条开始的 10 条数据。
SELECT * from students LIMIT -1,5;#最后 5 条数据不要了，其余全要。
```

### 涉及到两个或多个表的操作
* **关键字：union **
MySQL UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中。多个 SELECT 语句会删除重复的数据。使用 union 的前后两个查询语句的结构必须是一样的，可以采用字段组合或是不足补空行的方式把前后两个结构调整为一样的。
```mysql
#将两个表的性别汇总，相同性别只记录一次（不包含重复数据），如果要记录所以则使用 union all（包含重复数据）。
select sex from table1 #两条语句的字段不一定相同，但数量一定要一样，不够的用空格代替。
union 
select sex from table2
```

* **关键字：join **  [参考博客](https://www.cnblogs.com/fudashi/p/7491039.html)
即表格的交并补操作，有左连接，右连接，内部连接，外部连接，注意：解析连接的表是从右往左进行
左连接（left join）：求两个表的交集外加左表剩下的数据；
右连接（right join）：求两个表的交集外加右表剩下的数据；
内部连接（inner join）：求两个表的交集；
外部连接：就是求两个集合的并集，但是 MySQL 不支持 OUTER JOIN，但是我们可以对左右连接做 UNION 操作实现。
交叉连接：笛卡尔积

```mysql
table1 （inner）join table1 on foreign1Key = foreign2Key;
#默认是内连接，因为用的比较多，相当于将外键补充出来，说成交集并不是很准确。
left join; #如果理解了内连接，那么左右连接也好理解了，因为有些字段没有外键，因此左连接就将左表中无外键的数据补充出来，外键设置为空，右连接则补充右表。大部分情况下on后面是外键，不然也不会用到两张表嘛，但也可能是其他字段作为条件，这里只是举例。
right join;

```

* **问：查找学生的姓名，年龄和所对应的班级全称（另一个表的信息）？**
```mysql
SELECT stu.name, stu.age, cla.name 
from students stu inner join class cla 
ON stu.class = cla.classId;
#另一个关键字 using(classID)：用法相当于 on，但是 using 里面的字段不是可选的且两张表都要有。
#注意，如果两边的表如果某个记录为空，则是不会显示的，因为不符合 on 的条件。
#如果想显示左边空的记录，则选择左连接即可。
```

**left join 如果有多张表，关键字可以使用多次**
可以使用别名，using 的用法相当于 on，但是 using 里面的字段不是可选的（就是 select 里面你选不了），且两张表都要有。
*select a.last_name,a.first_name,b.dept_no 
from employees a 
left join dept_emp b 
using(emp_no)*

**差集表示：使用 where is null**
*select e.emp_no 
from employees e left join dept_manager d on e.emp_no = d.emp_no
where d.dept_no is null*

* **问：获取所有部门中当前员工薪水最高的相关信息？有点难理解的，因为东西比较多，我们拆开来看**
*SELECT d.dept_no, s.emp_no, MAX(s.salary) AS salary
FROM salaries AS s INNER JOIN dept_emp As d
ON d.emp_no = s.emp_no 
WHERE d.to_date = '9999-01-01' AND s.to_date = '9999-01-01'
GROUP BY d.dept_no*

* 1-如果没有 group by 和 MAX 函数，那么显示所有 部门-员工-薪水
* 2-如果只加上 MAX 函数，那么只会显示一条记录，就是最高薪水的那条记录 部门-员工-薪水（最大）
* 3-如果不要 MAX 函数，只加上 Group by，那么只会显示分组的种类数，每个部门选第一条数据展示。
* 4-如果都要，可以这样理解，先用 Group by 进行分组，没有结束前是分组状态，不会只抽取第一条记录，当使用  MAX 函数过滤出每组的最大值后，每个分组只剩一条记录。



