# 数据库-MySQL-底层概念

<a href="#事务的特性">事务的特性</a>
<a href="#数据库引擎">数据库引擎</a>
<a href="#使用索引">使用索引</a>
<a href="#使用视图">使用视图</a>
<a href="#存储过程">存储过程</a>

## 数据库简介
RDBMS (Relational Database Management System，关系数据库管理系统)  
SQL (Structured Query Language，结构化查询语言)  
JDBC[ java数据库连接（java DataBase Connectivity）]：由一组使用java语言编写的类与接口组成，可以为多种关系数据库提供统一访问。  

**MySQL**： 是关系型数据库，目前属于 Oracle 旗下产品。MySQL 最流行的关系型数据库管理系统之一，选择如何存储和检索你的数据，这种灵活性是 MySQL 为什么如此受欢迎的主要原因，其中内置了多种存储引擎，是开源的，所以你不需要支付额外的费用；支持多线程，充分利用 CPU 资源；为多种编程语言提供了 API；可以处理拥有上千万条记录的大型数据库；  
**InnoDB** ：5.5 版本后 Mysql 的默认数据库引擎，事务型数据库的首选引擎，支持 ACID 事务，支持行级锁定，还有其他一些关系型数据库，最简单的是 Excel，如 SQLite 很小，可以随时移动。微软的 SQL Server,Oracle 出品的 MySQL。  

**NoSQL[非关系型数据库]**：MongoDB，Redis[内存存储]等都是非关系型数据库。  
随着互联网 web2.0 网站的兴起，传统的关系数据库在应付 web2.0 网站，特别是超大规模和高并发的 SNS 类型的 web2.0 纯动态网站已经显得力不从心，暴露了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL 数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题。  
**主要分四大类**：键值(Key-Value)存储数据库、列存储数据库、文档型数据库、图形(Graph)数据库  
****
* **问：MySQL 与 SQL Server 的区别？**  
[MySQL 与 SQL Server 的区别](https://www.cnblogs.com/hhx626/p/6010369.html)

* **问：drop、delete 与 truncate分别在什么场景之下使用？**
    这三个字段都和删除表有关，首先 drop 是删除整个表格，delete 是删除表中的数据，truncate 是还原表。
有这样一个场景，需要删除表中的说有数据，我们可以 delete from tableName; 效果是一条一条删除表中的数据，且 id 不会重置，不是我们想要的效果。那么我们就会想到使用 truncate tableName; 相当于 drop table，然后create table。但这时又有一种清空，truncate 不能使用，公司线上规定不能使用 drop 表相关的操作，包括 truncate，这个时候我们就只能使用 delete from tableName; 然后再加一条 alter table tableName auto_increment=1;

* **问：如何优化数据库、如何实现乐观锁、三个范式、主键 超键 候选键 外键、drop、delete 与 truncate分别在什么场景之下使用？**



# 事务的特性
<a name="事务的特性"></a>

* **问：数据库事务的四大特性（ACID）以及事务的隔离级别（高频）？**
* **答**：如果一个数据库声称支持事务的操作，那么该数据库必须要具备以下四个特性：  

**一、原子性（Atomicity）**  
　　原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚。  

**二、一致性（Consistency）**  
　　拿转账来说，假设用户 A 和用户 B 两者的钱加起来一共是 5000，那么不管 A 和 B 之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是 5000，这就是事务的一致性。

**三、隔离性（Isolation）**  
　　对于任意两个并发的事务 T1 和 T2，在事务 T1 看来，T2 要么在 T1 开始之前就已经结束，要么在 T1 结束之后才开始，这样每个事务都感觉不到有其他事务在并发地执行。  
　　关于事务的隔离性数据库提供了多种隔离级别，稍后会介绍到。这里需要注意的是，上面提到的是第一种隔离级别，隔离性最高，但同样效率最低，后面还会后三种隔离级别。  

**四、持久性（Durability）**  
　　持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。  

**隔离性是当多个用户（事务）并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。如果不考虑事务的隔离性，会发生哪几种问题：**

**1. 脏读[一事务在修改，二事务读，一事务回滚]**  
例如：用户 A 向用户 B 转账 100 元，对应 SQL 命令如下  
update account set money=money+100 where name=’B’; (此时A通知B)  
update account set money=money - 100 where name=’A’;  
　　当只执行第一条 SQL 时，A 通知 B 查看账户，B 发现确实钱已到账（此时即发生了脏读），而之后无论第二条 SQL 是否执行，只要该事务不提交，则所有操作都将回滚，那么当 B 以后再次查看账户时就会发现钱其实并没有转。脏读是指**在一个事务处理过程里读取了另一个未提交的事务中的数据**。当一个事务正在多次修改某个数据，而在这个事务中这多次的修改都还未提交，这时一个并发的事务来访问该数据，就会造成两个事务得到的数据不一致。所以 B 在 A 未完成事务的情况下不应该读到任何数据。  

**2.不可重复读【一事务在读，二事务修改并提交了数据，一事务在读，注意和脏读的区别】**  
　　例如事务 T1 在读取某一数据，而事务 T2 立马修改了这个数据并且提交事务给数据库，事务 T1再次读取该数据就得到了不同的结果，发生了不可重复读。  
　　不可重复读是指在对于数据库中的某个数据，一个事务范围内多次查询却返回了不同的数据值，这是由于在查询间隔，被另一个事务修改并提交了。  
　　在某些情况下，不可重复读并不是问题，比如我们多次查询某个数据当然以最后查询得到的结果为主。但在另一些情况下就有可能发生问题。  
　　**不可重复读和脏读的区别是：脏读是某一事务读取了另一个事务未提交的脏数据，而不可重复读则是读取了前一事务提交的数据。**  

**3.虚读(幻读)【对批量数据处理的过程中，另一事务做了修改，注意和不可重复读的区别】**  
　　幻读是事务非独立执行时发生的一种现象。例如事务 T1 对一个表中所有的行的某个数据项做了从“1”修改为“2”的操作，这时事务 T2 又对这个表中插入了一行数据项，而这个数据项的数值还是为“1”并且提交给数据库。而操作事务 T1 的用户如果再查看刚刚修改的数据，会发现还有一行没有修改，其实这行是从事务 T2 中添加的，就好像产生幻觉一样，这就是发生了幻读。  
　　**幻读和不可重复读都是读取了另一条已经提交的事务（这点就脏读不同），所不同的是不可重复读查询的都是同一个数据项，而幻读针对的是一批数据整体（比如数据的个数）。**  
**** 
**现在来看看 MySQL 数据库为我们提供的四种隔离级别**：  
[如何实现](https://blog.csdn.net/qq_34462387/article/details/82432593)  
**① Serializable (串行化)：可避免脏读、不可重复读、幻读的发生。**  
提供严格的事务隔离。它要求事务序列化执行，事务只能一个接着一个地执行，不能并发执行。此隔离级别可有效防止脏读、不可重复读和幻读。但这个级别可能导致大量的超时现象和锁竞争，在实际应用中很少使用。  

**② Repeatable read (可重复读)：可避免脏读、不可重复读的发生。**  
一个事务在执行过程中，可以访问其他事务成功提交的新插入的数据，但不可以访问成功修改的数据。读取数据的事务将会禁止写事务（但允许读事务），写事务则禁止任何其他事务。此隔离级别可有效防止不可重复读和脏读。  

**③ Read committed (读已提交)：可避免脏读的发生。**  
一个事务在执行过程中，既可以访问其他事务成功提交的新插入的数据，又可以访问成功修改的数据。读取数据的事务允许其他事务继续访问该行数据，但是未提交的写事务将会禁止其他事务访问该行。此隔离级别可有效防止脏读。

**④ Read uncommitted (读未提交)：最低级别，任何情况都无法保证。一个事务在执行过程中，既可以访问其他事务未提交的新插入的数据，又可以访问未提交的修改数据。如果一个事务已经开始写数据，则另外一个事务不允许同时进行写操作，但允许其他事务读此行数据。此隔离级别可防止丢失更新。**  
　　
　　以上四种隔离级别最高的是Serializable级别，最低的是Read uncommitted级别，当然级别越高，执行效率就越低。像Serializable这样的级别，就是以锁表的方式（类似于Java多线程中的锁）使得其他的线程只能在锁外等待，所以平时选用何种隔离级别应该根据实际情况。  

　　在 MySQL 数据库中，支持上面四种隔离级别，默认的为 Repeatable read (可重复读)；而在Oracle 数据库中，只支持 Serializable (串行化)级别和 Read committed (读已提交)这两种级别，其中默认的为 Read committed 级别。  
**记住：设置数据库的隔离级别一定要是在开启事务之前！**  
　　如果是使用 JDBC 对数据库的事务设置隔离级别的话，也应该是在调用 Connection 对象的setAutoCommit(false) 方法之前。调用 Connection 对象的 setTransactionIsolation(level) 即可设置当前链接的隔离级别，至于参数 level，可以使用 Connection 对象的字段：  
在JDBC中设置隔离级别的部分代码：

****
* **问：事务和数据库连接池有什么关系？**  
[参考博客](https://www.cnblogs.com/panxuejun/p/5920845.html)
将线程和连接绑定，保证事务能统一执行 

```java
Class.forName("com.mysql.jdbc.Driver");
/*
一次调用只能获得一个 connection，并不能满足高并发情况。
因为connection不是线程安全的，一个connection对应的是一个事务。
数据库每次连接都很浪费cpu和内存资源，因此诞生了数据库连接池(线程池)。
数据库连接池负责分配,管理和释放数据库连接,它允许应用程序重复使用一个现有的数据库连接,
而不是重新建立一个。
*/
java.sql.Connection conn = DriverManager.getConnection(jdbcUrl);

/*数据库连接池（注意这里是线程）
注意pool.getConnection()，都是先从threadlocal里面拿的，如果threadlocal里面有，
则用，保证线程里的多个dao操作，用的是同一个connection，以保证事务。
如果新线程，则将新的connection放在threadlocal里，再get给到线程。
*/
/*问：JTA事务和普通事务的区别？
答：简单的说 jta是多库的事务 jdbc是单库的事务
*/
```

# 数据库引擎 
<a name="数据库引擎"></a>
你能用的数据库引擎取决于 mysql 在安装的时候是如何被编译的。要添加一个新的引擎，就必须重新编译 MYSQL。  
在缺省情况下，MYSQL 支持三个引擎：ISAM、MYISAM和HEAP。另外两种类型INNODB和BERKLEY（BDB），也常常可以使用。[参考博客](https://www.cnblogs.com/0201zcr/p/5296843.html)  
**ISAM**  
ISAM 是一个定义明确且历经时间考验的数据表格管理方法，它在设计之时就考虑到数据库被查询的次数要远大于更新的次数。因此，ISAM 执行读取操作的速度很快，而且不占用大量的内存和存储资源。ISAM 的两个主要不足之处在于，**它不支持事务处理，也不能够容错：如果你的硬盘崩溃了，那么数据文件就无法恢复了**。如果你正在把 ISAM 用在关键任务应用程序里，那就必须经常备份你所有的实时数据，通过其复制特性，MYSQL 能够支持这样的备份应用程序。  

**INNODB 和 BERKLEYDB（BDB）**  
数据库引擎都是造就 MYSQL 灵活性的技术的直接产品，这项技术就是 MYSQL++ API。在使用MYSQL 的时候，你所面对的每一个挑战几乎都源于 ISAM 和 MYISAM 数据库引擎不支持事务处理也不支持外来键。尽管要比 ISAM 和 MYISAM 引擎慢很多，但是 INNODB 和 BDB 包括了对事务处理和外来键的支持，这两点都是前两个引擎所没有的。如前所述，如果你的设计需要这些特性中的一者或者两者，那你就要被迫使用后两个引擎中的一个了。  

**Innodb引擎**  
　　Innodb 引擎提供了对数据库 ACID 事务的支持，并且实现了 SQL 标准的四种隔离级别，关于数据库事务与其隔离级别的内容请见数据库事务与其隔离级别这篇文章。该引擎还提供了行级锁和外键约束，它的设计目标是处理大容量数据库系统，它本身其实就是基于 MySQL 后台的完整数据库系统，MySQL 运行时 Innodb 会在内存中建立缓冲池，用于缓冲数据和索引。但是该引擎不支持FULLTEXT 类型的索引，而且它没有保存表的行数，当 SELECT COUNT( * ) FROM TABLE 时需要扫描全表。当需要使用数据库事务时，该引擎当然是首选。由于锁的粒度更小，写操作不会锁定全表，所以在并发较高时，使用 Innodb 引擎会提升效率。但是使用行级锁也不是绝对的，如果在执行一个 SQL 语句时 MySQL 不能确定要扫描的范围，InnoDB 表同样会锁全表。  

**MyIASM引擎**
　　MyIASM 没有提供对数据库事务的支持，也不支持行级锁和外键，因此当 INSERT(插入)或UPDATE(更新)数据时即写操作需要锁定整个表，效率便会低一些。不过和 Innodb 不同，MyIASM 中存储了表的行数，于是 SELECT COUNT( * ) FROM TABLE 时只需要直接读取已经保存好的值而不需要进行全表扫描。如果表的读操作远远多于写操作且不需要数据库事务的支持，那么 MyIASM 也是很好的选择。
****
* **问：Innodb的索引实现？**
* **答**：B +树的形式保存。

* **问：数据库引擎（Innodb）的事务支持粒度？**
* **答**：行级锁。

* **问：MySQL 中 myisam 与 innodb 的区别，至少 5 点？**
* **答**：不支持事务，行级锁、外键、但返回表的行数更快、

# 索引
<a name="使用索引"></a>
* **问：索引是什么？有什么作用以及优缺点（高频）？**
1、在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法。这种数据结构，就是索引；  
2、索引用来快速地寻找那些具有特定值的记录，所有 MySQL 索引都以 B+ 树的形式保存；  
3、在 MySQL 中，当你建立主键时，索引同时也已建立起来了；  
4、MySQL 提供多种索引类型供选择：唯一性索引、主键索引、聚集索引、非聚集索引；  

**唯一索引**：索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。  
**聚集索引(Clustered)**：对于一张表来说，聚集索引只能有一个，因为数据真实的物理存储顺序就是按照聚集索引存储的。  
**非聚集索引(Non-clustered)**：B+Tree 的数据域存储的内容为实际数据的地址，也就是说它的索引和实际的数据是分开的，只不过是用索引指向了实际的数据，这种索引就是所谓的非聚集索引。  
****
* **问：主键就是聚集索引吗？**
* **问：主键 和 索引的区别？**  
1. 主键一定是唯一性索引，唯一性索引并不一定就是主键；  
2. 一个表中可以有多个唯一性索引，但只能有一个主键；  
3. 主键列不允许空值，而唯一性索引列允许空值；   
4. 索引可以提高查询的速度。 主键和索引都是键，不过主键是逻辑键，索引是物理键，意思就是主键不实际存在，而索引实际存在在数据库中；  

唯一索引能极大的提高查询速度，而且还有唯一约束的作用，一般索引，只能提高 30% 左右的速度，经常插入，修改，应在查询允许的情况下，尽量减少索引，因为添加索引，插入，修改等操作，需要更多的时间。如果在我们的查询条件使用了函数，那么索引就不可用了。可以用建立函数索引的方式，来解决这个问题。  

**优点**  
1、如果没有索引，执行查询时 MySQL 必须从第一个记录开始扫描整个表的所有记录，直至找到符合要求的记录。表里面的记录数量越多，这个操作的代价就越高。如果作为搜索条件的列上已经创建了索引，MySQL 无需扫描任何记录即可迅速得到目标记录所在的位置。如果表有 1000 个记录，通过索引查找记录至少要比顺序扫描记录快100倍。  

**缺点**  
1、为表设置索引要付出代价的：一是增加了数据库的存储空间，二是在插入和修改数据时要花费较多的时间(因为索引也要随之变动)。
****
* **问：什么时候【要】创建索引？**  
（1）表经常进行 SELECT 操作；  
（2）表很大(记录超多)，记录内容分布范围很广；  
（3）列名经常在 WHERE 子句或连接条件中出现；  

* **问：什么时候【不要】创建索引？**
（1）表经常进行 INSERT/UPDATE/DELETE 操作；  
（2）表很小(记录超少)；  
（3）列名不经常作为连接条件或出现在 WHERE 子句中；  

```mysql
/*创建表时，不能在同一个字段上建立两个索引(主键默认建立唯一索引)，
在需要经常查询的字段上建立索引。
*/
-- 显示这个表格的所有索引
show index from students;
select * from students where name like 'a%';

-- 这是最基本的索引，它没有任何限制。
CREATE INDEX nameIndex on students(NAME(8));

-- 这是唯一索引，索引列的值必须唯一，但允许有空值。
CREATE unique INDEX nameIndex on students(NAME(8));

-- 删除索引
drop index nameIndex on students;
```

# 视图
[参考博客](https://blog.csdn.net/coolwriter/article/details/79794309)
[参考博客](https://www.cnblogs.com/0201zcr/p/5296843.html)

<a name="使用视图"></a>

* **问：什么是视图？以及视图的使用场景有哪些？**
**视图**：是一种基于数据表的一种虚表，与包含数据的表不一样，视图只包含使用时动态检索数据的查询；不包含任何列或数据。使用视图可以简化复杂的 sql 操作，隐藏具体的细节，保护数据；视图创建后，可以使用与表相同的方式利用它们。  
**优点**：重用SQL语句，简化复杂的SQL操作，使用表的组成部分而不是整个表，保护数据-可以授予用户表部分的访问权限而不是整个表，更改数据格式-视图可以重新构成不同表不同部分的表示。  
视图不能被索引，也不能有关联的触发器或默认值，如果视图本身内有 order by 则对视图再次order by 将被覆盖。  
（1）视图是一种虚表；  
（2）视图建立在已有表的基础上, 视图赖以建立的这些表称为基表；  
（3）向视图提供数据内容的语句为 SELECT 语句,可以将视图理解为存储起来的 SELECT 语句；  
（4）视图向用户提供基表数据的另一种表现形式；  
（5）视图没有存储真正的数据，真正的数据还是存储在基表中；  
（6）程序员虽然操作的是视图，但最终视图还会转成操作基表；  
（7）一个基表可以有0个或多个视图；  
有的时候，我们可能只关系一张数据表中的某些字段，而另外的一些人只关系同一张数据表的某些字段,那么把全部的字段都都显示给他们看，这是不合理的。我们应该做到：他们想看到什么样的数据，我们就给他们什么样的数据，一方面就能够让他们只关注自己的数据，另一方面，我们也保证数据表一些保密的数据不会泄露出来，我们在查询数据的时候，常常需要编写非常长的SQL语句，几乎每次都要写很长很长，.上面已经说了，视图就是基于查询的一种虚表，也就是说，视图可以将查询出来的数据进行封装。那么我们在使用的时候就会变得非常方便。  
**值得注意的是**：使用视图可以让我们专注与逻辑，但不提高查询效率

### 视图操作
可以将视图的操作理解为预处理，将很多复杂的逻辑和表进行预处理，构建成一个简单的表，然后在此表的基础上进行操作。  
但记住很多视图是不可更新的，如有分组（group by,having），联结，子查询，并，函数（max,count,sum,distinct）等，好像限制很多，但其实视图主要就是用来检索数据的，术业有专攻

```mysql
# 查看某个视图
show create VIEW stuCla;
# 会显示所有的表，包括视图，因为视图也算一种表
show TABLES;

# 删除视图不存在会报错，下面这个不会
drop view gather;
drop view if EXISTS views;

# 创建视图，若存在则报错， 后者不存在-创建，存在-覆盖
create view stuCla AS
create or replace view stuCla As 
select stu.name stuName,cla.name claName from students stu, class cla
where stu.class = cla.classId;

# 使用视图
select * from stuCla;

```

# 存储过程
<a name="存储过程"></a>

### 存储过程
存储过程简单来说就是为以后的使用而保存的一条或多条SQL语句的集合，可视为批文件。但是在使用中，我觉得更像是函数，将一系列必须的操作封装在函数内调用，因此如果过短或者函数本身做的事情很少，那么这个存储过程就有点冗余了，可以按照函数的要求来进行编写。
```mysql
/* 1.存储过程只在创建时进行编译，以后每次执行存储过程都不需再重新编译，而一般 SQL 语句每执行一次就编译一次,所以使用存储过程可提高数据库执行速度。这一般不作为存储过程的主要优点，况且PreparedStatement有批处理，性能差别不大，针对业务逻辑，要对多个表进行多次进行增删改查操作，那么可以把这些操作汇聚成一个存储过程。
缺点：移植性很差，出错后不能准确定位错误。
2.当对数据库进行复杂操作时(如对多个表进行 Update,Insert,Query,Delete 时），可将此复杂操作用存储过程封装起来与数据库提供的事务处理结合一起使用。这些操作，如果用程序来完成，就变成了一条条的 SQL 语句，可能要多次连接数据库。而换成存储，只需要连接一次数据库就可以了。*/

create procedure proc1(out s int)  // 只有输出
create procedure proc2(in p_in bigint)  // 只有输入
create procedure proc15() // 没有输入与输出
create procedure demo_multi_param(
    in id bigint,
    in name varchar(32),
    out c int
) //多输入与输出

//显示创建的人，时间，等信息，如果没有like语句，则显示所有，系统的存储过程也会有。
show procedure STATUS like 'proc1'; 

drop procedure proc1; //如果没有会报错，下面的不会
drop procedure if EXISTS proc1;


/**编写输出存储过程，下面做的事情其实不用存储过程，这里只是示例，一个存储过程最好有逻辑判断等操作，即复杂的操作封装在存储过程中。*/
-- 先创建
create procedure proc1(
out max  int,
out min  int,
out avg  int
)
BEGIN
select max(age) into max from students;
select min(age) into min from students;
select avg(age) into avg from students;
END
-- 再调用
call proc1(@maxAge,@minAge,@avgAge);//执行存储过程
select @maxAge,@minAge,@avgAge;//输出

-- 编写输入存储过程
CREATE PROCEDURE proc2(in sid int)
BEGIN
SELECT *  from students where id=sid;
end
call proc1(1);-- 执行存储过程，返回一条记录,或者 set @pid=1; call proc1(@pid)

```
****
* **问：存储过程与触发器的区别?**  
触发器与存储过程非常相似，触发器也是SQL语句集，两者唯一的区别是触发器不能用EXECUTE语句调用，而是在用户执行Transact-SQL语句时自动触发（激活）执行。触发器是在一个修改了指定表中的数据时执行的存储过程。通常通过创建触发器来强制实现不同表中的逻辑相关数据的引用完整性和一致性。由于用户不能绕过触发器，所以可以用它来强制实施复杂的业务规则，以确保数据的完整性。触发器不同于存储过程，触发器主要是通过事件执行触发而被执行的，而存储过程可以通过存储过程名称名字而直接调用。当对某一表进行诸如 UPDATE、INSERT、DELETE 这些操作时，SQLSERVER就会自动执行触发器所定义的SQL语句，从而确保对数据的处理必须符合这些SQL语句所定义的规则。
****
**问：如何优化数据库？**  
1）应尽量避免在 where 子句中使用 != 或 <> 操作符，否则将引擎放弃使用索引而进行全表扫描。  
2）应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描，如：`select id from t where num is null`  
可以在 num 上设置默认值 0，确保表中 num 列没有 null 值，然后这样查询：  
`select id from t where num=0`  
3）很多时候用 exists 代替 in 是一个好的选择  
4）用Where子句替换HAVING 子句 因为HAVING 只会在检索出所有记录之后才对结果集进行过滤  
****
**问：什么是乐观锁和悲观锁？**  
**乐观锁（ Optimistic Locking ）**：乐观锁认为并发问题很少发生，所以读某个对象时不会加锁，只有在修改某个数据时，才会进行检查该数据是否被修改。  
**乐观锁实现**：使用数据版本（Version）记录机制实现，这是乐观锁最常用的一种实现方式。何谓数据版本？即为数据增加一个版本标识，一般是通过为数据库表增加一个数字类型的 “version” 字段来实现。当读取数据时，将version字段的值一同读出，数据每更新一次，对此version值加1。当我们提交更新的时候，判断数据库表对应记录的当前版本信息与第一次取出来的version值进行比对，如果数据库表当前版本号与第一次取出来的version值相等，则予以更新，否则认为是过期数据。  
**悲观锁**：悲观锁总是假设最坏的情况，每次取数据时都会加锁（读锁、写锁、行锁等），当其他线程想要访问数据时，都需要阻塞挂起。可以依靠数据库实现，如行锁、读锁和写锁等，都是在操作之前加锁，在Java中，synchronized的思想也是悲观锁。  
****
**问：三个范式是什么？你在建表时用到的范式？**  
首先要明确的是：满足着第三范式，那么就一定满足第二范式、满足着第二范式就一定满足第一范式；  
第一范式：字段是最小的的单元不可再分，学生信息组成学生信息表，有年龄、性别、学号等信息组成。这些字段都不可再分，所以它是满足第一范式；  
第二范式：满足第一范式,表中的字段必须完全依赖于全部主键而非部分主键；  
其他字段组成的这行记录和主键表示的是同一个东西，而主键是唯一的，它们只需要依赖于主键，也就成了唯一的，学号为1024的同学，姓名为Java3y，年龄是22岁。姓名和年龄字段都依赖着学号主键；  
第三范式：满足第二范式，非主键外的所有字段必须互不依赖；  
就是数据只在一个地方存储，不重复出现在多张表中，可以认为就是消除传递依赖  
比如，我们大学分了很多系（中文系、英语系、计算机系……），这个系别管理表信息有以下字段组成：系编号，系主任，系简介，系架构。那我们能不能在学生信息表添加系编号，系主任，系简介，系架构字段呢？不行的，因为这样就冗余了，非主键外的字段形成了依赖关系(依赖到学生信息表了)！正确的做法是：学生表就只能增加一个系编号字段。  

在设计一个商业项目的数据表时，离不开“业务需求”，第一，如果已知这个系统的数据量不会太大，那么肯定用第三范式，可以避免数据冗余带来的更新插入上需要同时多表里相同字段的麻烦。第二，如果数据量很大，那么我们可能就需要冗余数据，这样可以免去连接操作（加快执行速度），但在修改时，可能就需要修改多处了。  
****
**问：drop、delete与truncate分别在什么场景之下使用？**  
不再需要一张表的时候，用drop；  
想删除部分数据行时候，用 delete，并且带上 where 子句；  
保留表而删除所有数据的时候用 truncate  
****
**问：主键 超键 候选键 外键？**  
**主键**：一个数据列只能有一个主键，且主键的取值不能缺失，即不能为空值（Null）；  
**外 键**：在一个表中存在的另一个表的主键称此表的外键；  
**超 键**：在关系中能唯一标识元组的属性集称为关系模式的超键。一个属性可以为作为一个超键，多个属性组合在一起也可以作为一个超键。超键包含候选键和主键；  
**候选键**：是最小超键，即没有冗余元素的超键；  
****
**问：什么是临时表？**  
临时表只在当前连接可见，当关闭连接时，MySQL 会自动删除表并释放所有空间。因此在不同的连接中可以创建同名的临时表，并且操作属于本连接的临时表。

```mysql
#手动删除临时表
DROP TEMPORARY TABLE IF EXISTS temp_tb;
#创建临时表
CREATE TEMPORARY TABLE tmp_table (    NAME VARCHAR (10) NOT NULL,    time date NOT NULL);
select * from tmp_table;
```
****
**问：共享锁和排斥锁的含义以及用途？**  
  **共享锁又称读锁**：若事务T对数据对象A加上S锁，则事务T可以读A但不能修改A，其他事务只能再对A加S锁，而不能加X锁，直到T释放A上的S锁。  
  **排他锁又称写锁**：若事务T对数据对象A加上X锁，事务T可以读A也可以修改A，其他事务不能再对A加任何锁，直到T释放A上的锁。这保证了其他事务在T释放A上的锁之前不能再读取和修改A。排他锁很好理解，是自己独占资源。

