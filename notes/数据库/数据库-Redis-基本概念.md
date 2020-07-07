# Redis 缓存数据库

# 数据库简介
1、出现的原因：NoSQL[非关系型数据库]，Redis[内存存储]是非关系型数据库，属于开源产品。随着互联网 web2.0 网站的兴起，传统的关系数据库在应付 web2.0 网站，特别是超大规模和高并发的 SNS 类型的 web2.0 纯动态网站已经显得力不从心，暴露了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL 数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题。
2、分四大类：键值(Key-Value)存储数据库、列存储数据库、文档型数据库、图形(Graph)数据库
3、用途：数据库、缓存和消息中间件。
4、类型：字符串(strings)、散列(hashes)、列表(lists)、集合(sets)、有序集合(sorted sets)
5、命令不区分大小写
6、Redis 数据库里面的每个键值对都是由对象（Object）组成的，其中：1、数据库 键 总是一个字符串，2、数据库的 值 可以是五种对象中的一种。
7、键可以包含任何数据（二进制安全）,比如 jpg 图片或者序列化的对象,一个键最大能存储512M

# 优势
1、性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
2、丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
3、原子 – Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
4、丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

# 对象（Object）
1、Redis 并没有直接使用下面介绍的数据结构来实现键值对数据库，而是基于这些数据结构创建了一个对象系统，这个系统包含这五种类型的对象。
2、Redis 对象系统还实现了基于引用计数的内存回收机制。
3、Redis 使用对象来表示数据库中的 键 和 值，每次新建一个键值对时，至少会创建两个对象。
4、键总是一个字符串对象，而值可以有五种对象，因此当我们称 “列表键” 时，指的是“这个数据库键所对应的值为列表对象”。

```C
/*
type：命令返回的结果为数据库键对应值的 对象类型，不是键的，键只有String。
encoding：决定底层的数据结构，long 类型的整数、简单动态字符串、字典、双端链表、压缩列表、整数集合、跳跃表和字典等等，每种对象都至少使用了两种编码。
*/
typedef struct redisObject{
    unsigned type:4; //类型[五个中的一个 STRING,HASH,LIST,SET,ZSET]
    unsigned encoding:4; //编码
    void *ptr; //指向底层实现数据结构的指针
}robj;
```



# String(字符串)

## 简介


## 1、简单动态字符串
```C
/*没有直接使用 C 语言传统的字符串表示（以空字符结尾的字符数组：C 字符串），而是自己构建一种名为动态字符串（simple dynamic string, SDS）的抽象类型作为默认类型。Redis 中，一些无须对字符串值进行修改的地方如打印日志，会用到 C 字符串。如果需要修改，则会使用 SDS。下面介绍SDS的基本使用规则。
1、数据结构
2、空间预分配：在free不够时进行添加操作，会自动分配与len相同的空间（小于1M，大于1M的话，最多分配 1M）
3、惰性空间释放：其实就是不释放，如果删除字符串的某些字符，则 free 增加即可。
4、二进制安全：buf 叫做字节数组，因为 SDS 的API 都是二进制安全的，都会以处理二进制的方式来处理 SDS 存放在 buf 中的数据。就算有很多\0也没事，因为有 len。

问：SDS 与 C 相比有哪些优点？
1、常数复杂度获取字符串长度
2、杜绝缓冲区溢出
3、减少了修改字符串长度时所需的内存重分配次数
4、二进制安全
5、兼容部分 C 字符串函数

*/
struct sdshdr{
/*记录 buf 数组中未使用字节的数量，不够会自动分配，防止内存溢出与泄露*/
    int free; 
/*记录 buf 数组中已使用字节的数量，而 C 字符串需要遍历统计*/
    int len; 
/*字节数组，用于保存字符串（末尾自动添加\0，这样可以方便调用 C 的函数库，所以数组的长度 = len + free + 1）*/
    char buf[]; 
}

```


# Hash(字典)
1、又称为符号表（symbol table）、关联数组（associative array）、映射（map）键值对集合
2、适合存储对象，并且可以像数据库中 update 一个属性一样只修改某一项属性值(Memcached中需要取出整个字符串反序列化成对象修改完再序列化存回去)	存储、读取、修改用户属性

应用：Redis 数据库底层实现就是采用字典，对数据库的增、删、改、查都是构建在对字典的操作上的。
```C
/*
1、Redis 中的字典使用哈希表作为底层实现，每个字典带有两个哈希表，一个平时使用，一个rehash 时使用。
2、哈希表使用链地址法来解决冲突（头插法）
3、对哈希表进行扩展或者收缩操作时（采用渐进式rehash），程序需要将现有哈希表包含的所有键值对rehash到新哈希表里面。

*/

//字典
typedef struct dict{
    dictType *type; //类型特定函数
    void *privdata; //私有数据
    dictht ht[2]; //哈希表，两个，默认使用第一个
    in trehashidx; //rehash索引，不再进行时为 -1.
}dict;

//哈希表
typedef struct dictht{
    dictEntry **table; //哈希表数组
    unsigned long size; //哈希表大小
    unsigned long sizemask; //哈希表大小掩码，用于计算索引值，总是等于 size - 1
    unsigned long used; //哈希表已有节点的数量
}dictht;

//数组节点的结构 dictEntry
typedef struct dictEntry{
    void *key;
    union{
        void *val;
        uint64_tu64;
        int64_ts64;
    }v;
    struct dictEntry *next; //指向下一个哈希表节点，形成链表
}dictEntry;

```


# List(列表)
## 链表(双向链表)特点
1、双端：有前后指针
2、无环：表头 prev，表尾 tail 的 next 都指向 null
3、有表头指针和表尾指针
4、带链表长度
5、多态

功能：发布与订阅、慢查询、监视器、保存多个客户端的状态信息，构建输出缓冲区，最新消息排行等功能(比如朋友圈的时间线)、消息队列（Message Queue，MQ）

```C
//节点
typedef struct listNode{
    struct listNode *prev;
    struct listNode *next;
    void *value;
}

//虽然使用多个 listNode 结构就可以组成链表，但使用下面的 list 操作起来更方便。
typedef struct list{
    listNode *head; //头结点
    listNode *tail; //尾节点
    unsigned long len; //长度
    void *(*dup)(void *ptr); //复制链表节点所保存的值
    void (*free)(void *ptr); //释放链表节点所保存的值
    int (*match)(void *ptr, void *key); //比较是否相等
}list;
```
## 压缩列表（ziplist）
1、压缩列表是列表键 和 哈希键的底层实现之一。
2、当一个列表键只包含少量列表项，且要么是小整数值，要么就是比较短的字符串，则 Redis 使用压缩列表来做列表键的底层实现。
3、压缩列表是 Redis 为了节约内存而开发的，是由一系列特殊编码的连续内存块组成的顺序型数据结构。
4、压缩列表可以包含多个节点，每个节点可以保存一个字节数组或者整数值
5、添加新节点到压缩列表，或者从压缩列表中删除节点，可能会引发连锁更新操作，但这种操作出现几率不高。
```C
/*
压缩列表结构：[zlbytes、zltail、zllen、entry1、entry2、。。。、entryN、zlend]
zlbytes：4字节，记录整个压缩列表占用的内存字节数
zltail：4字节，记录压缩列表表尾节点距离 起始节点有多少字节
zllen：2字节，记录压缩列表包含的节点数量
entryX：压缩列表包含的各个节点，节点的长度由节点保存的内容决定
zlend：特殊值 0xFF，用于标记压缩列表的末端

压缩列表节点组成结构：previous_entry_length、encoding、content 三个部分
previous_entry_length：
encoding：
content：
*/

```



# Set(集合)	
1、哈希表实现,元素不重复	
2、添加、删除,查找的复杂度都是O(1) 
3、为集合提供了求交集、并集、差集等操作	
4、应用：共同好友 ，利用唯一性,统计访问网站的所有独立ip ，好友推荐时,根据tag求交集,大于某个阈值就可以推荐

```C
/*
1、整数集合是集合键的底层实现之一
2、整数集合的底层实现是数组，以有序，无重复的方式保存集合元素，在有需要时，根据新添加的元素，自动升级，改变数组的类型。
3、升级操作为整数集合带来了操作上的灵活性，并且尽可能的节约了内存。
4、整数集合只支持升级操作，不支持降级操作。
*/

typedef struct intset{
    uint32_t encoding; //编码方式，确定数组的类型
    uint32_t length; //集合包含的元素数量
    int8_t contents[]; //保存元素的数组，类型以最大范围的为准
}intset;
```


# Sorted Set(有序集合)	
1、将Set中的元素增加一个权重参数score,元素按score有序排列	
2、数据插入集合时,已经进行天然排序	
3、应用：排行榜 ，带权重的消


## 跳跃表
跳跃表（skiplist）是一种有序数据结构，它通过在每个节点中维持多个指向其他节点的指针，从而达到快速访问节点的目的。大部分情况下，跳跃表的效率可以和平衡树相媲美，并且因为跳跃表实现更为简单，所以可以使用前者代替后者。

应用只有两处：有序集合、集群节点

```C
/*
1、层： level 数组包含多个元素，每个元素都包含一个指向其它节点的指针，可以加快访问其它节点的速度，一般说来，层越多访问其它节点的速度越快。
*/
typedef struct zskiplistNode{
    struct zskiplistLevel{ //层
        struct zskiplistNode *forward; //前进指针
        unsigned int span; //跨度
    }level[]
        
    struct zskiplistNode *backward; //后退指针
    
    double score; //分值
    robj *obj; //成员对象
}zskiplistNode;

//仅靠多个跳跃表节点就可以组成一个跳跃表
typedef struct zskiplist{
    struct zskiplistNode *header, *tail; //表头节点和表尾节点
    unsigned long length; //表中节点的数量
    int level; //表示最大的层数
}
```

# 单机数据库的实现

# 数据库
本章将对 Redis 服务器的数据库实现进行详细介绍，
1、说明服务器保存数据库的方法
默认创建 16 个数据库
2、通过 SELECT 命令来切换数据库，实现原理（修改redisClient 的 db 指针）

2、客户端切换数据库的方法
3、数据库保存键值对的方法
4、以及针对数据库的添加、删除、查看，更新操作的实现方法
5、说明服务器保存键的过期时间的方法
6、服务器自动删除过期键的方法。


# 持久化

# 事件

# 客户端

# 服务器