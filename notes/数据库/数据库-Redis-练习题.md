# Redis 数据库底层原理

# 基本函数
```mysql

```


# 基本类型
```mysql
空：nil
成功：返回 1，或者 'ok'
```


# String 的基本操作
```mysql
'''增'''
set animal Cat #赋值一个变量，没有则创建，有则覆盖
get animal #获取变量的值
mset name Tom age 11 #赋值多个变量
mget name age #获取多个变量

expire key 5 #定时5秒
set animal 'cat' EX 5 #也是定时5秒
set user:tom:age:45 'abcde' #比较好的编码方式

'''删'''
del animal #删除

'''改'''
set animal 'Cat' #赋值一个变量
append animal 'Dog'#追加

set count 100
incr count #101增加1
decr count #100减少1

'''查'''
exits count #查询是否存在返回0,1
type count #查询类型

get animal #获取
mget user1 user2 animal #多个赋值

```

# Hash 的基本操作
```mysql

'''增'''
hmset user username tom birth 1977 sex 1 # 给表格 user 字段赋值
hmget user username birth sex #获取表格字段的值
hset,mget #操作user 表的某个字段 

'''删'''
hdel user sex#删除某个字段
'''改'''
hsetnx user sex #如果存在，则不设置
hincrby user birth 10#给出生年加10
'''查'''
hkeys/hvals user #获取user 表所有的键/值
hlen user #返回user 表字段的数量
hexists #是否存在
```

# List 的基本操作
```mysql

'''增'''
rpush mylist A B C #在右边插入值，相当于入栈
lpush mylist E F G #在左边插入值

'''删'''
rpop mylist #出栈
lpop mylist #出队列

'''改'''

'''查'''
lrange mylist 0 -1 #就是显示全部元素，[0,10]显示前十一个元素。
llen mylist #显示长度

```


# Set 的基本操作
```mysql
/*
问：为什么能存这么多还很快？
集合中最大的成员数为 232 - 1(4294967295, 每个集合可存储40多亿个成员)。?????


问：zset 如何实现排序？
Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。zset的成员是唯一的,但分数(score)却可以重复。



*/
'''增'''
sadd myset 1 2 3 #赋值
'''删'''
srem myset 1 #删除元素
'''改'''

'''查'''
sismember myset 1#判断元素是否在集合内
smembers myset #返回所有成员，每次返回顺序可能不同
sdiff set1 set2 #返回两个结合的差集set1 - set2
sinter #返回几个集合的交集
sunion #返回几个集合的并集
```

