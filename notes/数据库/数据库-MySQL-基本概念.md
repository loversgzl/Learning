# 数据库-MySQL-基本概念

* 书籍推荐《MySQL 里的 36 条军规》

* **注释，建表，四种基本操作：INSERT、DELETE、UPDATE、SELECT；**

```mysql
#注释
-- 三种注释之一，两杠后加一空格
#三种注释之二，python语言风格
/*三种注释之三，C语言风格*/

#建表
#int、char、datetime、smallint：可以表示性别，比 int 内存更少，varchar：可变长度。
create table `students`(
    `id` int not null,
    `name` varchar(20) not null,
    `sex` smallint not null,
    `in_time` datetime not null,
    primary key(`id`)
);#注意字段上的点，叫间隔号。
#删表操作：drop table `name`;


```

















* **问：查找最晚入职员工的所有信息？**
```mysql
select * from employees where hire_date = (select max(hire_date) from employees)
#max 获取一条日期最大的记录，因为这个记录只选了这一个字段，所以后面的 select 语句就只返回一个日期。
```



*  

* **问：查找入职员工时间排名倒数第三的员工所有信息？**

* *select * from employees order by hire_date desc limit 2,1*
解释：[ order by ]排序 [ desc ]倒序 [ asc ]增序，默认增序

**limit**
*select * from table limit 5; 只要前五条数据
*select * from table limit 5,10; 从第6条开始的十条数据。
*select * from table limit 5,-1; 从第6条开始后的所有数据
*select * from table limit -1,10; 最后十条数据不要了，其余全要。


* **问：在两个表中，分别查找一个字段？**
*select salaries.salary,dept_manager.dept_no
from salaries inner join dept_manager
on dept_manager.emp_no = salaries.emp_no
and dept_manager.to_date = '9999-01-01'
and salaries.to_date = '9999-01-01';*

**inner join**
就是交并补，
有左连接：求两个表的交集外加左表剩下的数据；
右连接：求两个表的交集外加右表剩下的数据；
内部连接：求两个表的交集
外部连接：就是求两个集合的并集，但是 MySQL 不支持 OUTER JOIN，但是我们可以对左右连接做 UNION 操作实现。
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

* **问：统计一个表中出现超过 15 次的 emp_no 员工编号？**
*select emp_no,count(emp_no) as t 
from salaries
group by emp_no having t > 15*
* 如果不加 group by，那么 count 就会统计全部的 emp_no，且第一条记录的 emp_no 作为员工编号，后面 count 为记录总数。
* 因此加了 group by，那么就会以 emp_no 进行分组，得到的记录数就是种类数，即员工数量，然后对每个分组统计数量，即每个员工编号出现的次数。

**count 函数 having**
一般使用 where 时，count 是在查询完成后进行聚合。如：查询每个部门成绩大于89的员工数
SELECT dept,COUNT(user_name) FROM ec_uses WHERE score>89 GROUP BY dept;

如果要在聚合之后删选记录，就是删选聚合出来的数据，那么要用到 having 了。
参考博客：https://www.cnblogs.com/yunfeioliver/p/7887463.html

* **问：搜索所有员工的薪水，相同的薪水只显示一次？**
*select salary from salaries 
where to_date = '9999-01-01'
group by salary
order by salary desc*
**group by 解决重复问题-有点没有理解到位**
使用 distinct 也可以，不过效率不高 SELECT DISTINCT salary FROM salaries。一般推荐使用 group by。


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