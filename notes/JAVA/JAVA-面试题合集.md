# JAVA-面试题合集

**问：Java 中的值传递和引用传递？**
错误理解一：值传递和引用传递，区分的条件是传递的内容，如果是个值，就是值传递。如果是个引用，就是引用传递。
错误理解二：Java是引用传递。
错误理解三：传递的参数如果是普通类型，那就是值传递，如果是对象，那就是引用传递。
其实我觉得和 Python 中可变类型与不可变类型的概念类似，具体可参考[Python 五种数据类型](https://blog.csdn.net/qq_29611345/article/details/100736961)
我的理解就是不可变类型是值传递，如（Integer、String）
可变类型是引用传递：如（int[]，ArrayList）

**问：.equals 和  ==  的区别？**
.equals：这是一个方法，比较两个对象的内容是否相同，基本类型不能用的。
==：比较两个值的引用是否相同，比前者快，因为只比较引用，如果是基本数据类型，值相同，那么肯定引用相同。[参考博客](https://www.cnblogs.com/Eason-S/p/5524837.html)

**错误：在循环内进行删除。**

```java
ArrayList<String> list = new ArrayList<String>();  
list.add("a");  
list.add("bb");  
list.add("bb");  
list.add("ccc");  
list.add("ccc");  
list.add("ccc");
//第一种，可能会没有删干净
for (int i = 0; i < list.size(); i++) {  
	String s = list.get(i);  
	if (s.equals("bb")) 
		list.remove(s);  
}
//第二种：出现异常 并发修改异常java.util.ConcurrentModificationException
for (String s : list) {  
	if (s.equals("bb")) 
		list.remove(s);  
}  
```

### 多线程问题
多线程问题：sleep、wait、yield、join、volatile、并发、线程池、锁的实现

**问：Java 中的 sleep 方法？**
Thread.sleep(100);
作用：让当前正在运行的占用 cpu 的线程，挂起 100 ms，其余线程自己竞争获取 cpu，如果被停止，会抛出 InterruptedException，需要的注意的是就算线程的睡眠时间到了，他也不是立即会被运行，只是从睡眠状态变为了可运行状态，是不会由睡眠状态直接变为运行状态的。

**问：Java 中的 wait 方法？**
this.wait()
作用：属于Object类，自动释放对象锁，挂起，等待this.notify()唤醒，进入就绪状态。

**比较：sleep 方法 和 wait 方法**
对于sleep()方法，我们首先要知道该方法是属于Thread类中的。而wait()方法，则是属于Object类中的。sleep()方法导致了程序暂停执行指定的时间，让出cpu给其他线程，但是他的监控状态依然保持者，当指定的时间到了又会自动恢复运行状态。
在调用sleep()方法的过程中，线程不会释放对象锁。而当调用wait()方法的时候，线程会放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象调用notify()方法后本线程才进入对象锁定池准备获取对象锁进入运行状态。

**问：Java 中的 yield 方法？**
Thread.yield();
作用：暂停当前正在执行的线程对象，并执行其他线程，其实也不一定执行其他线程。yield()应该做的是让当前运行线程回到可运行状态，以允许具有相同优先级的其他线程获得运行机会。因此，使用yield()的目的是让相同优先级的线程之间能适当的轮转执行。但是，实际中无法保证yield()达到让步目的，因为让步的线程还有可能被线程调度程序再次选中。

**问：Java 中的 join方法？**
thread.Join把指定的线程加入到当前线程，可以将两个交替执行的线程合并为顺序执行的线程。比如在线程B中调用了线程A的Join()方法，直到线程A执行完毕后，才会继续执行线程B。

**问：关于 volatile 的一些面试问题？？？**
[参考博客1](https://blog.csdn.net/u011277123/article/details/72235927)[参考博客2](https://blog.csdn.net/u012723673/article/details/80682208)
* * volatile原理？
volatile 是一种简单的同步的处理机制，被 volatile 修饰的变量遵循以下规则：
•变量的值在使用之前总会从主内存中再读取出来。
•对变量值的修改总会在完成之后写回到主内存中。

* volatile 能使得一个非原子操作变成原子操作吗？volatile 修饰 long 变量？
一个典型的例子是在类中有一个 long 类型的成员变量。如果你知道该成员变量会被多个线程访问，如计数器、价格等，你最好是将其设置为 volatile。为什么？因为 Java 中读取 long 类型变量不是原子的，需要分成两步，如果一个线程正在修改该 long 变量的值，另一个线程可能只能看到该值的一半（前 32 位）。但是对一个 volatile 型的 long 或 double 变量的读写是原子。

* Java 中能创建 volatile 数组吗？修饰一个数组能否保证内存可见性？
能，Java 中可以创建 volatile 类型数组，不过只是一个指向数组的引用，而不是整个数组。如果改变引用指向的数组，将会受到 volatile 的保护，但是如果多个线程同时改变数组的元素，volatile 标示符就不能起到之前的保护作用了。


**问：java并发相关的包？**
[参考博客](https://www.jianshu.com/p/46728d6bc6b2)
java.util.concurrent（JUC）包，继续发问：用过里面哪些工具类呢？
报告面试官，JUC中有非常多的类，将部分类按功能进行分类，分别是：
1. 原子atomic包
2. 比synchronized功能更强大的lock包
3. 线程池
AQS（AbstractQueuedSynchronizer），即队列同步器。它是构建锁或者其他同步组件的基础框架，它是JUC并发包中的核心基础组件。

**问：线程池？**

**问：锁的实现？Synchronized 和 Lock 的区别？**
[参考博客](https://blog.csdn.net/youyou1543724847/article/details/52735510)
关于锁有两个关键字，Synchronized（重量级）和Lock（轻量级），synchronized 是 java中的一个关键字，也就是说是 Java 语言内置的特性。那么为什么会出现Lock呢？如果一个代码块被synchronized 修饰了，当一个线程获取了对应的锁，并执行该代码块时，其他线程便只能一直等待，等待获取锁的线程释放锁，而这里获取锁的线程释放锁只会有两种情况：
　　1）获取锁的线程执行完了该代码块，然后线程释放对锁的占有；
　　2）线程执行发生异常，此时JVM会让线程自动释放锁。
那么如果这个获取锁的线程由于要等待 IO 或者其他原因（比如调用 sleep 方法）被阻塞了，但是又没有释放锁，其他线程便只能干巴巴地等待，试想一下，这多么影响程序执行效率。因此就需要有一种机制可以不让等待的线程一直无期限地等待下去（比如只等待一定的时间或者能够响应中断），通过 Lock 就可以办到。再举个例子：当有多个线程读写文件时，读操作和写操作会发生冲突现象，写操作和写操作会发生冲突现象，但是读操作和读操作不会发生冲突现象。但是采用synchronized 关键字来实现同步的话，就会导致一个问题：
如果多个线程都只是进行读操作，所以当一个线程在进行读操作时，其他线程只能等待无法进行读操作。因此就需要一种机制来使得多个线程都只是进行读操作时，线程之间不会发生冲突，通过Lock 就可以办到。另外，通过 Lock 可以知道线程有没有成功获取到锁。这个是 synchronized 无法办到的。总结一下，也就是说 Lock 提供了比 synchronized 更多的功能。但是要注意以下几点：
1）Lock 不是 Java 语言内置的，synchronized 是 Java 语言的关键字，因此是内置特性。Lock 是一个类，通过这个类可以实现同步访问；
2）Lock 和 synchronized 有一点非常大的不同，采用 synchronized 不需要用户去手动释放锁，当synchronized 方法或者 synchronized 代码块执行完之后，系统会自动让线程释放对锁的占用；而Lock则必须要用户去手动释放锁，如果没有主动释放锁，就有可能导致出现死锁现象。
锁的相关类别：
可重入锁的机制：方法一内部调用方法二，且都是syn修饰，则第二次可不用申请锁。
可中断锁：syn不可中断，lock可中断。
公平锁；按照请求顺序给锁，syn不是公平锁，
读写锁、偏向锁、乐观锁、悲观锁

### 集合框架的问题
**问：ArrayList 与 LinkedList 有什么区别，分别有什么适用场景（高频考点）**
答：类似于动态数组和链表，前者定位，修改快，后者插入，删除快。

**Map 接口类**
java 为数据结构中的映射定义了一个接口 java.util.Map; 它有四个实现类,分别是HashMap Hashtable LinkedHashMap 和 TreeMap。
**问：HashMap实现原理（get、put、resize）？？？**
[参考博客](https://www.imooc.com/article/24118)
需要阅读源码深入理解。

**问：HashMap 与 HashSet 有什么区别，两者在查询某个值的效率上是否有区别？**
答：类似字典和集合，HashSet 自身并没有独立的实现，而是在里面封装了一个 Map，向 HashSet中增加元素，其实就是把该元素作为key，增加到Map中，其他也是类似的 map 操作。

**问：HashMap 和 Hashtable 的区别？**
答：相同：HashMap 和 Hashtable都实现了Map接口，都是键值对保存数据的方式；
区别1：HashMap 可以存放 null、Hashtable不能存放null
区别2：HashMap 不是线程安全的类、Hashtable 是线程安全的类
HashMap 不支持线程的同步，即任一时刻可以有多个线程同时写 HashMap;可能会导致数据的不一致。如果需要同步，可以用 Collections 的 synchronizedMap 方法使 HashMap具有同步的能力，或者使用ConcurrentHashMap。

**问：HashMap 和 Linked HashMap 的区别？**
LinkedHashMap; 数据会有序的存入字典中和 HashMap 不同，后者是无序的。TreeMap 能够把它保存的记录根据键排序,默认是按键值的升序排序。

```java
import java.util.LinkedHashMap;
import java.util.Map;
import java.util.Iterator;
Map<Integer, String> map = new LinkedHashMap<>();
map.put(1,"one");
//用 Iterator 遍历时会按照插入顺序输出
for(Iterator x = map.keySet().iterator(); x.hasNext();)
	Object key = x.next();
```

### 字符串问题
**问：字符串拼接 StringBuffer，StringBuilder，concat 和 + 的区别？**
答：[参考博客](https://www.cnblogs.com/lojun/articles/9664794.html)
StringBuffer 是线程安全的，StringBuilder 不是线程安全的。其他两者基本相同，因为操作字符串很多时候不需要线程安全，所以在拼接字符串时 StringBuilder 使用较多。
1、在字符串不经常发生变化的业务场景优先使用String(代码更清晰简洁)。如常量的声明，少量的字符串操作(拼接，删除等)。
2、在单线程情况下，如有大量的字符串操作情况，应该使用StringBuilder来操作字符串。不能使用String"+"来拼接，避免产生大量无用的中间对象，耗费空间且执行效率低下（新建对象、回收对象花费大量时间）。如JSON的封装等。
3、在多线程情况下，如有大量的字符串操作情况，应该使用StringBuffer。如HTTP参数解析和封装等。且如果在 for 循环中进行拼接操作，建议使用这两者，整个逻辑都只做字符数组的加长，拷贝，到最后也不会创建新的String对象，所以速度很快。

**问：equals 和 == 的区别？ **
答：前者：不能用于基本数据类型比较，只能用于引用类型，比较内存地址。 后者：基本数据类型比较值，引用类型比较内存地址。

