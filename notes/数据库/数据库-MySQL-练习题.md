# 数据库-MySQL-编程基本概念

**有些比较难理解的命令，还是需要动手操作，比如 group by、inner join、 MAX 、MIN、 COUNT、 avg 等等。**

* 书籍推荐《MySQL 里的 36 条军规》

注释，增、删、改表和创建数据库
```mysql
#注释
-- 三种注释之一，两杠后加一空格
#三种注释之二，python语言风格
/*三种注释之三，C语言风格*/

#语法：不区分大小写，句末记得添加分号

#字段可选类型：int、char、datetime、smallint、varchar
#注意字段上的点，叫间隔号,建删表时不可省略，查询等操作可省略。
create database `school`;
use `school`;

create table `students`(
    `id` int not null,
    `name` varchar(20) not null,
    `sex` smallint not null,
    `age` int not null,
    `in_time` datetime not null,
    primary key(`id`)
) default charset 'UTF8';

#删除表格，数据库，表格的某一列
drop table `students`; 
drop database `school`;
ALTER TABLE students DROP column nickname;

#修改表名，数据库名好像不好修改，重建吧
#为已有表格增加一列
ALTER TABLE `student` RENAME TO `students`; 
ALTER TABLE `students` add column `age` int not null after `sex`;

```

### 在一个表中查询
四种基本操作：INSERT、DELETE、UPDATE、SELECT（主要考察 select，所有后面着重介绍）
```mysql
###INSERT
INSERT INTO students(name,sex,age,in_time) VALUE ('张四',1,25,NOW());


```

### SELECT

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
* **进阶：统计不同年龄的人生，显示人生大于等 2 的年龄**
```mysql
#关键字：count，group by，having
SELECT sex,count(*) num FROM students GROUP BY sex;
SELECT age,count(*) as num FROM students GROUP BY age HAVING num>=2;
```

* **问：将学生按照年龄倒序排，按照 id 升序排？**
```mysql
#关键字 order by, DESC,AESC
SELECT * FROM students ORDER BY age DESC, id ASC;
```

* **问：查找姓名以 a 开头 或者以 b 开头的学生？**
```mysql
#关键字：regexp
SELECT * FROM students WHERE name REGEXP '^a|^b';
```

* **问：查找所有年龄，不同年龄只显示一次？**
```mysql
#关键字：distinct
SELECT DISTINCT age FROM students;
```

* **问：查找最晚入职员工的所有信息？**
```mysql

```


* **关键字：limit**
查询某些数据，如前三条，后三条，中间某些等等
```mysql
#问：查找年龄第三小的员工所有信息？
SELECT * from students ORDER BY age LIMIT 2,1;
#limit start num;
SELECT * from students LIMIT 5;#只要前五条数据，0 省略。
SELECT * from students LIMIT 5,10;#从第 6 条开始的 10 条数据。
SELECT * from students LIMIT -1,5;#最后 5 条数据不要了，其余全要。
```

### 涉及到两个或多个表的操作

* **关键字：join **
即表格的交并补操作，有左连接，右连接，内部连接，外部连接
左连接：求两个表的交集外加左表剩下的数据；
右连接：求两个表的交集外加右表剩下的数据；
内部连接：求两个表的交集；
外部连接：就是求两个集合的并集，但是 MySQL 不支持 OUTER JOIN，但是我们可以对左右连接做 UNION 操作实现。
```mysql

```

* **问：在两个表中，分别查找一个字段？**
*select salaries.salary,dept_manager.dept_no
from salaries inner join dept_manager
on dept_manager.emp_no = salaries.emp_no
and dept_manager.to_date = '9999-01-01'
and salaries.to_date = '9999-01-01';*

参考博客：https://www.cnblogs.com/fudashi/p/7491039.html

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



**count 函数 having**
一般使用 where 时，count 是在查询完成后进行聚合。如：查询每个部门成绩大于89的员工数
SELECT dept,COUNT(user_name) FROM ec_uses WHERE score>89 GROUP BY dept;

如果要在聚合之后删选记录，就是删选聚合出来的数据，那么要用到 having 了。
参考博客：https://www.cnblogs.com/yunfeioliver/p/7887463.html




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



不懂！
1-对所有员工的当前(to_date='9999-01-01')薪水按照salary进行按照1-N的排名，相同salary并列且按照emp_no升序排列？

