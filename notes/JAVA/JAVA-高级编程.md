# JAVA-高级编程
以下是学习后的总结，[更具体的，参考这个学习网站，很好用](http://how2j.cn/stage/25.html)

**JAVA - 高级编程**

* <a href="#javaReflect">JAVA - 反射</a>
* <a href="#javaIO">JAVA - I/O-NIO</a>
* <a href="#javaThreat">JAVA - 多线程编程</a>
* <a href="#javaJVM">JAVA - 虚拟机</a>
* <a href="#javaGC">JAVA - 垃圾回收，内存优化机制</a>
* <a href="#javaNet">JAVA - 网络编程</a>

### JAVA - 反射
<a name="javaReflect"></a>
**有啥用**：使用反射方式，首先准备一个配置文件，就叫做 spring.txt 吧, 放在 src 目录下。 里面存放的是类的名称，和要调用的方法名。在测试类Test中，首先取出类名称和方法名，然后通过反射去调用这个方法。当需要从调用第一个业务方法，切换到调用第二个业务方法的时候，不需要修改一行代码，也不需要重新编译，只需要修改配置文件 spring.txt，再运行即可。这也是 Spring 框架的最基本的原理，只是它做的更丰富，安全，健壮。

**反射的常见用法**；有三类，一是 查看（Hero.class），某个类的属性方法等信息；二是 装载 如装载指定的类到内存中；三是 调用，如通过输入参数调用指定的方法。

**获取类对象的三种方式**
1. Class.forName（“Hero”） 最常用，通过字符串获取
2.  Hero.class 通过类来获取类
3. new Hero().getClass() 通过实例获取该类
Hero 是一个类，在一个JVM中，一种类，只会有一个类对象存在。所以以上三种方式取出来的类对象，都是一样的。反射其实就是获取类对象。反射指的是在运行时能够分析类的能力的程序。其中1，3会导致类的静态属性被初始化，而且只会执行一次，后面在调用不会初始化了，2不会。
```java
/*问：你知不知道反射？或者你有没有用过反射？
答：有，用的最多的应该就是JDBC装载数据库驱动的时候，通过Class.forName("com.mysql.jdbc.Driver");

问：Class类有什么作用？你用过其中的什么方法？
答：反射的实现基础是Class类，可以查看某个类的属性和方法。当一个类或接口被装入Java虚拟机时，变会产生一个与它相关联的java.lang.Class对象。
通过Class类的forName方法，我们能得到一个指定类型的Class对象，通过newInstance方法，可以加载指定的类。

问：如果我要看一个class文件中的属性和方法，该怎么看？
上面一行，加上，可以通过Field类，得到类中的属性。通过Method类，调用类中的方法，通过Constructor类，得到类的构造函数。
*/


import java.lang.reflect.Field;
import java.util.Stack;
class Student{
	int age;
	String name;
	public Student() {
		System.out.println("构造函数");
	}
}

public class Main {
    public static void main(String[] args){
    	try {
    		Class<?> clazz = Class.forName("Student");	
    		Field[] fields = clazz.getDeclaredFields();//获取类的字段
    		for(Field field : fields) 
    			System.out.println(field.getName());
    	}catch(Exception e){
    		e.printStackTrace();
    	}
    }
}


/*知识点：getClass() ？
SubClass 和 SuperClass 的 getClass() 都没有重写，他们都是调用Object的getClass，而Object的getClass作用是返回的是运行时的类的名字。这个运行时的类就是当前类，所以下面的代码并不是获取父类的名字，而是当前类的名字。*/
super.getClass().getName(); //当前类的名称
super.getClass().getSuperclass(); //真正获取父类的名称的方法

```


反射的特点：
1. 在运行时判断任意一个对象所属的类；在运行时构造任意一个类的对象；在运行时判断任意一个类所具有的成员变量和方法；在运行时调用任意一个对象的方法；生成动态。
2. 通过反射可以动态的实现一个接口，形成一个新的类，并可以用这个类创建对象，调用对象方法
3. 通过反射，可以突破Java语言提供的对象成员、类成员的保护机制，访问一般方式不能访问的成员
4. Java的反射机制会给内存带来额外的开销。例如对永生堆的要求比不通过反射要求的更多

问题：
1. Java反射主要涉及的类如Class, Method, Filed,等，他们都在java.lang.reflet包下？
答：在运行时分析类的能力--检查类的结构--所用到的就是 java.lang.reflect 包中的 Field、Method、Constructor，分别用于描述类的域、方法和构造器。Class类在java.lang中。

2. Java反射机制提供了字节码修改的技术，可以动态的修剪一个类？
答：不能修改。

3. Java反射机制一般会带来效率问题，效率问题主要发生在查找类的方法和字段对象，因此通过缓存需要反射类的字段和方法就能达到与之间调用类的方法和访问类的字段一样的效率？
答：不能，反射会降低效率，禁止安全检查，可以提高反射的运行速度。

手撕代码：
1、通过反射创建一个对象。
2、通过反射获取属性，再修改属性值
问：getField 和 getDeclaredField的区别？
答：这两个方法都是用于获取字段，getField 只能获取 public 的，包括从父类继承来的字段。
getDeclaredField 可以获取本类所有的字段，包括 private 的，但是不能获取继承来的字段。 (注： 这里只能获取到private的字段，不能访问该private字段的值,除非加上setAccessible(true))
3、通过反射调用一个对象方法

```java
 String className = "charactor.Hero"; //使用反射的方式创建对象
 Class pClass=Class.forName(className); //类对象
 Constructor c= pClass.getConstructor(); //构造器
 Hero h2= (Hero) c.newInstance(); //通过构造器实例化
 
 //获取类Hero的名字叫做name的字段
 Field f1= h.getClass().getDeclaredField("name");
 f1.set(h, "teemo");  //修改这个字段的值
 
 // 获取这个名字叫做setName，参数类型是String的方法
 Method m = h.getClass().getMethod("setName", String.class);
 m.invoke(h, "盖伦"); // 对h对象，调用这个方法

/*
错误：运用java反射机制获取实体方法报错，java.lang.NoSuchMethodException: int.<init>(java.lang.String)？
解决：构造Hero类时使用了基本数据类型，public int damage; 要换成包装类 Integer;
还有其他错误：如 xxx Hero<init>xxx?
解决：是找不到该类，因为我只写了类名，没有写包名.类名。
*/

```

### 泛型
<a name="泛型"></a>
```java
ArrayList heros = new ArrayList(); //如果不使用泛型约束，heros里可以存放任意类型，都以Obeject存储，造成记忆和使用困难。
ArrayList<APHero> heros = new ArrayList<>();//加入泛型约束，那么限制只能某一类存入heros。

//构造一个支持存入多种类型的类，使用泛型T，Java的源代码可以看见很多。
public class MyStack<T>{
LinkedList<T> values = new LinkedList<T>();
    public void push(T t) {
        values.addLast(t);
    }
}

//extends Hero 表示这是一个Hero泛型的子类泛型，只要是其子类，就认为是同一种类型，如List和ArrayList。
ArrayList<? extends Hero> heroList = apHeroList;

//? super Hero 表示 heroList的泛型是Hero或者其父类泛型
ArrayList<? super Hero> heroList = new ArrayList<Object>();
          
/*如果希望只取出，不插入，就使用? extends Hero
如果希望只插入，不取出，就使用? super Hero
如果希望，又能插入，又能取出，就不要用通配符？*/
```


### JAVA - I/O-NIO
<a name="javaIO"></a>
**I/O 文件对象**
按照流是否直接与特定的地方（如磁盘、内存、设备等）相连，分为 节点流 和 处理流 两类。
**节点流**：可以从或向一个特定的地方（节点）读写数据。
文 件 FileInputStream FileOutputStrean FileReader FileWriter 文件进行处理的节点流。

```java
/*代码一：一般说来操作系统都会安装在C盘，所以会有一个 C:\WINDOWS目录。遍历这个目录下所有的文件(不用遍历子目录)，找出这些文件里，最大的和最小(非0)的那个文件，打印出他们的名。注: 最小的文件不能是 0 长度。
知识点：1如何创建文件对象，2返回文件夹下所有子文件的函数，3一个文件的长度
*/

import java.io.File; //引入必要的包
public class test{
	public static void main(String args[]){
		File fs = new File("C:/WINDOWS"); //创建一个文件对象
		long max=0, min=Integer.MAX_VALUE;
		File maxName=null, minName=null; //文件对象要初始化。
//该函数以文件数组的形式，返回当前文件夹下的所有文件（不包含子文件及子文件夹）
		for(File f : fs.listFiles()){ 
			if(f.length() > max){
				max = f.length();
				maxName = f;
			}
			if(f.length() < min && f.length() > 0){
				min = f.length();
				minName = f;
			}
		}
		System.out.println("最大的文件为： " + maxName + " 大小为：" + max);
		System.out.println("最小的文件为： " + minName + " 大小为：" + min);
		//输出文件名 maxName.getAbsoluteFile() 也可以。
	}
}
```

```java
//代码二、输出电子书文件夹下所有的PDF文件，并统计有多少文件。
import java.io.File;
public class Main{
	public static int num = 0;
	static void getCSVInFolder(String filePath) {
		File folderName = new File(filePath);
		File[] flist = folderName.listFiles();
		if(flist==null || flist.length==0) return;
		String fileName = null;
		for(File f : flist) {
			if(f.isDirectory())
				getCSVInFolder(f.getAbsolutePath());
			else {
				fileName = f.getName();
                //这里如果用 == "pdf"比较，则无法匹配成功，因为会产生新的对象，地址不相等的。
				if(fileName.substring(fileName.lastIndexOf('.')+1).equals("pdf")) {
					System.out.println(f.getName());
					num++;
				}
			}
		}
	}
	public static void main(String[] args) {
		getCSVInFolder("F:\\办公文档\\计算机工作文档\\生活-阅读");
		System.out.println(num);
		System.out.println("我一共有"+num+"本电子书");
	}
}

//代码三、练习：对 d 盘的一个文件进行读取。
知识点：文件输入输出流的创建，
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class test{
	public static void main(String[] args) {
		try{ //必须对其进行保护，否则会报错
		//File f = new File("F:/aa//bb//cc");
		//f.mkdir(); 防止文件路径不存在，可以创建递归目录。
			File f = new File("F:/test.txt");
			FileInputStream fis = new FileInputStream(f);
			byte[] buffer = new byte[(int)f.length()];
			fis.read(buffer);
			for(byte b : buffer){
				System.out.println(b); //输出asc码
			}
		} catch(IOException e){
			e.printStackTrace();
		}
        finally{
            fis.close(); //每次使用完需要关闭
        }
	}
}

//需要记住的常用函数
File folderName = new File(filePath); //创建一个文件对象
File[] flist = folderName.listFiles(); //返回文件夹下所有的文件
flist[0].isDirectory(); //判断是否是文件夹

```

**非阻塞性IO**
```java
/*
前面介绍的IO是（BIO,阻塞性IO），JDK1.4后增加了NIO。在高并发的情况下很有用，与性能优化和架构设计密切相关。
与传统的IO相比，NIO有 面向缓存 和 非阻塞 两大特点，还可以通过选择器管理多个读写通道。
Channel通道、Buffer缓冲器、Selector选择器，是NIO的三大核心组件。
*/
public class NIOBufferDemo {
	public static void main(String[] args) throws IOException {
		int bufferSize = 10;//字节
		FileChannel src = new FileInputStream("F:\\EFI\\src.txt").getChannel();
		FileChannel dest = new FileOutputStream("F:\\EFI\\dest.txt").getChannel();
		ByteBuffer buffer = ByteBuffer.allocate(bufferSize);
		int times = 0;//看读了几次
		while(src.read(buffer) != -1) {//一次读满缓存区，10个字节
			buffer.flip();
			dest.write(buffer);
			buffer.clear();
			System.out.println(++times);
		}
		System.out.println("ok");
	}
}


```


### JAVA - 多线程编程
<a name="javaThreat"></a>
**一、创建线程 和 线程常用方法**
创建多线程有 3 种方式，分别是 继承线程类，实现 Runnable 接口，匿名类。
1、继承线程类（需要重写 run 方法）：创建一个对象就是一个线程。
2、实现 Runnable 接口（需要重写 run 方法）：一般会问如果已经继承了一个类，该如何实现多线程。
3、匿名类（不需要额外增加一个类）：和匿名函数相似，在用的时候才写，而且直接写在主函数里面。
注： 创建线程是start()方法，run()并不能创建一个新的线程
![多线程方法](../../pics/java多线程.png)

```java
/*
实现的三种方法
*/
//方法一：继承Thread类
public class SimpleThread extends Thread{
    SimpleThread t = new SimpleThread();
    t.start();
}

//方法二：实现Runnable方法
public class SimpleThread implements Runnable{
    Thread t = new Thread(new SimpleThread());
    t.start();
}

//方法三：匿名内部类
public class SimpleThread{
    Thread t = new Thread(){
        public void run(){}
    };
    t.start();
}

/*常用方法：
通过调用Thread类的start方法来启动一个线程，这是此线程处于就绪状态，并没有运行。当得到cpu时间片后，开始执行run方法。*/
t1.start(); //启动线程是start()方法，run()并不能启动一个新的线程
Thread.sleep(1000); //线程阻塞 1 秒
t1.join();//在主线程中加入该线程，一般是 main 嘛，当 t1 执行完后，才会继续执行主函数
t1.yield(); //临时暂停，回到就绪状态，让位给同等优先级的其他线程。
t1.setPriority(Thread.MAX_PRIORITY);//为线程 t1 设置优先级，优先级越高，有越大的几率获得 CPU 资源。
t1.setDaemon(true); //如果一个进程只剩下守护线程，则该进程会自动结束。守护线程通常会被用来做日志，性能统计等工作。

/*问：多线程问题
多线程问题：sleep、wait、yield、join、volatile、并发、线程池、锁的实现

问：调用后释放锁的只有wait+join。
答：刚看到一个大佬写的，给大家参考一下，所谓的释放锁资源实际是通知对象内置的monitor对象进行释放，而只有所有对象都有内置的monitor对象才能实现任何对象的锁资源都可以释放。又因为所有类都继承自Object，所以wait(）就成了Object方法，也就是通过wait()来通知对象内置的monitor对象释放，而且事实上因为这涉及对硬件底层的操作，所以wait()方法是native方法，底层是用C写的。     其他都是Thread所有，所以其他3个是没有资格释放资源的    而join()有资格释放资源其实是通过调用wait()来实现的    

Thread.sleep(100);
作用：让当前正在运行的占用 cpu 的线程，阻塞 100 ms，其余线程自己竞争获取 cpu，如果被停止，会抛出 InterruptedException，需要的注意的是就算线程的睡眠时间到了，他也不是立即会被运行，只是从阻塞状态变为了就绪状态，是不会由阻塞状态直接变为运行状态的。

this.wait()
自动释放对象锁，挂起，等待this.notify()唤醒，进入就绪状态。注意 wait和notify 两个方法都属于Object类，不是Thread线程上的，生产者-消费者问题。

问：ThreadLocal？
1、ThreadLocal 存放的值是线程封闭，线程间互斥的，主要用于线程内共享一些数据，避免通过参数来传递。
2、线程的角度看，每个线程都保持一个对其线程局部变量副本的隐式引用，只要线程是活动的并且ThreadLocal实例是可访问的；在线程消失之后，其线程实例的所有副本都会被垃圾回收。
3、Thread 类中有一个Map，用于存储每一个线程的变量的副本。
4、对于多线程资源共享的问题，同步机制采用了“以时间换空间”的方式，而ThreadLocal 采用了以“空间换时间”的方式。
ThreadLocal类用来提供线程内部的局部变量。这种变量在多线程环境下访问(通过get或set方法访问)时能保证各个线程里的变量相对独立于其他线程内的变量。ThreadLocal实例通常来说都是private static类型的，用于关联线程和线程的上下文。  可以总结为一句话：ThreadLocal的作用是提供线程内的局部变量，这种变量在线程的生命周期内起作用，减少同一个线程内多个函数或者组件之间一些公共变量的传递的复杂度。  举个例子，我出门需要先坐公交再做地铁，这里的坐公交和坐地铁就好比是同一个线程内的两个函数，我就是一个线程，我要完成这两个函数都需要同一个东西：公交卡（北京公交和地铁都使用公交卡），那么我为了不向这两个函数都传递公交卡这个变量（相当于不是一直带着公交卡上路），我可以这么做：将公交卡事先交给一个机构，当我需要刷卡的时候再向这个机构要公交卡（当然每次拿的都是同一张公交卡）。这样就能达到只要是我(同一个线程)需要公交卡，何时何地都能向这个机构要的目的。  有人要说了：你可以将公交卡设置为全局变量啊，这样不是也能何时何地都能取公交卡吗？但是如果有很多个人（很多个线程）呢？大家可不能都使用同一张公交卡吧(我们假设公交卡是实名认证的)，这样不就乱套了嘛。现在明白了吧？这就是ThreadLocal设计的初衷：提供线程内部的局部变量，在本线程内随时随地可取，隔离其他线程。


**比较：sleep 方法 和 wait 方法**
对于sleep()方法，我们首先要知道该方法是属于Thread类中的，而wait()方法，则是属于Object类中的。sleep()方法导致了程序暂停执行指定的时间，让出cpu给其他线程，但是他的监控状态依然保持着，当指定的时间到了又会自动恢复运行状态。
在调用sleep()方法的过程中，线程不会释放对象锁。而当调用wait()方法的时候，线程会放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象调用notify()方法后本线程才进入对象锁定池准备获取对象锁进入运行状态。

Thread.yield();
作用：暂停当前正在执行的线程对象，并执行其他线程，其实也不一定执行其他线程。yield()应该做的是让当前运行线程回到就绪状态，以允许具有相同优先级的其他线程获得运行机会。因此，使用yield()的目的是让相同优先级的线程之间能适当的轮转执行。但是，实际中无法保证yield()达到让步目的，因为让步的线程还有可能被线程调度程序再次选中。

t1.Join()
把线程t1加入到当前线程，最主要的功能其实是防止主线程执行结束了，子线程还在没执行完成。如果加入这一句，则一定等到t1执行完成后才继续往后（主线程中）执行。如果前面有多个线程在执行了，则也会加入到执行队伍中（顺序不定），并且也是只有到t1结束后，程序才会继续往后执行。

问：Synchronized关键字的作用？
答：synchronized，可以作用在方法上，代码块上，类上（即静态方法）。synchronized关键字不能继承，虽然可以使用synchronized来定义方法，但synchronized并不属于方法定义的一部分，因此，synchronized关键字不能被继承。如果在父类中的某个方法使用了synchronized关键字，而在子类中覆盖了这个方法，在子类中的这个方法默认情况下并不是同步的，而必须显式地在子类的这个方法中加上synchronized关键字才可以。当然，还可以在子类方法中调用父类中相应的方法，这样虽然子类中的方法不是同步的，但子类调用了父类的同步方法，因此，子类的方法也就相当于同步了。
注意：
静态方法是属于类的而不属于对象的。同样的，synchronized修饰的静态方法锁定的是这个类的所有对象。
synchronized关键字不能继承，所以在定义接口方法时不能使用synchronized关键字。
构造方法不能使用synchronized关键字，但可以使用synchronized代码块来进行同步。 
缺点：然后引入与lock的区别。


**问：关于 volatile 的一些面试问题**
参考博客1：https://blog.csdn.net/u011277123/article/details/72235927
参考博客2：https://blog.csdn.net/u012723673/article/details/80682208

问：volatile 关键字的作用？
答：1、使得一个非原子操作变成原子操作，多线程访问中用 volatile 修饰 long和double 类型的变量。
2、volatile 关键字用在多线程同步中，可保证读取的可见性
3、JVM 保证从主存加载到线程工作内存的值是最新的
4、volatile 能禁止进行指令重排序


问：volatile原理？
出于运行速率的考虑，java编译器会把经常经常访问的变量放到缓存（严格讲应该是工作内存）中，读取变量则从缓存中读。但是在多线程编程中,内存中的值和缓存中的值可能会出现不一致。volatile用于限定变量只能从内存中读取，保证对所有线程而言，值都是一致的。但是volatile不能保证原子性，也就不能保证线程安全。
变量的值在使用之前总会从主内存中再读取出来。
对变量值的修改总会在完成之后写回到主内存中。

问：Java 中能创建 volatile 数组吗？修饰一个数组能否保证内存可见性？
能，Java 中可以创建 volatile 类型数组，不过只是一个指向数组的引用，而不是整个数组。如果改变引用指向的数组，将会受到 volatile 的保护，但是如果多个线程同时改变数组的元素，volatile 标示符就不能起到之前的保护作用了。

问：synchronized关键字和volatile关键字比较？
   volatile关键字是线程同步的轻量级实现，所以volatile性能肯定比synchronized关键字要好。但是volatile关键字只能用于变量而synchronized关键字可以修饰方法以及代码块。synchronized关键字在JavaSE1.6之后进行了主要包括为了减少获得锁和释放锁带来的性能消耗而引入的偏向锁和轻量级锁以及其它各种优化之后执行效率有了显著提升，实际开发中使用 synchronized 关键字的场景还是更多一些。          
   多线程访问volatile关键字不会发生阻塞，而synchronized关键字可能会发生阻塞          
   volatile关键字能保证数据的可见性，但不能保证数据的原子性。synchronized关键字两者都能保证。                  volatile关键字主要用于解决变量在多个线程之间的可见性，而 synchronized关键字解决的是多个线程之间访问资源的同步性。

问：synchronized 关键字和 lock 关键字比较？d


问：1、 HashMap和Hashtable的区别   
2、StringBuffer和StringBuilder的区别   
3、ArrayList和Vector的区别   
4、把非线程安全的集合转换为线程安全   
答：https://how2j.cn/k/thread/thread-thread-safe/703.html

问：ConcurrentHashMap如何保证线程安全？



*/

```

**二、线程同步问题**
问题：多个线程同时修改一个数据的时候，可能导致问题。

1. 线程同步的关键字
2. 同步主线程和子线程的三种方法
2.1 构造一个 object 对象，独占此对象才可执行该线程。
2.2 将此对象设定为 类的实例，则独占此实例才可调用此实例的方法（被关键字 synchronized  修饰过的多个方法，只有独占此实例，才可调用其中的任意一个）。
2.3 如果在类方法前（静态方法），加上修饰符 synchronized，同步对象是这个类的反射（即这个类独占，才可以调用 被关键字 synchronized  修饰过的多个方法中的一个）。一般是在方法前加 关键字，类前面是不加的。
3. 线程交互-生产者消费者模型。
```java
/*
1. 定义一个全局 object，谁占用谁有话语权
2. 可以用实例代替 object
3. 用关键字 synchronized 修饰方法
*/

Object someObject =new Object();
synchronized (someObject){}//此处的代码只有占有了someObject后才可以执行

Hero gareen = new Hero();
synchronized (gareen){}

public void hurt()
	synchronized(this){ //this 就代表了这个实例
		hp = hp - 1;
	}

public synchronized void hurt()//效果相同
	hp = hp - 1;
public static synchronized void method(){}//防止多个线程同时访问这个类中的synchronized方法。也就是说此种修饰，可以对此类的所有对象实例起作用。

/*
线程交互 wait() 和 notify()
这里需要强调的是，wait方法和notify方法，并不是Thread线程上的方法，它们是Object上的方法。 因为所有的Object都可以被用来作为同步对象，所以准确的讲，wait和notify是同步对象上的方法。wait()的意思是： 让占用了这个同步对象的线程，临时释放当前的占用，并且等待。 所以调用wait是有前提条件的，一定是在synchronized块里，否则就会出错。
*/
    public synchronized void recover() {
    	hp += 1;
    	System.out.printf("%s 血量增加到 %.0f \n", name, hp);
    	this.notify();
    }
    
    public synchronized void hurt() {
    	if(hp == 1) {
    		try {
    			this.wait();
    		}catch(InterruptedException e) {
    			e.printStackTrace();
    		}
    	}
    	hp -= 1;
    	System.out.printf("%s 血量减少为 %.0f \n", name, hp);	
    }

/*
问：可以用线程实现生产者消费者模型么？

问：lock 和 synchronized 的区别？ 
答：与  synchronized (someObject)  类似的 lock() 方法，表示当前线程占用 lock 对象，一旦占用，其他线程就不能占用了。与 synchronized 不同的是，一旦 synchronized 块结束，就会自动释放对 someObject 的占用。 lock却必须调用 unlock 方法进行手动释放，为了保证释放的执行，往往会把 unlock() 放在 finally 中进行。

synchronized:
1、一个对象有多个synchronized方法，只要一个线程访问了其中的一个synchronized方法，其它线程不能同时访问这个对象中任何一个synchronized方法。
2、在对象方法前，加上修饰符 synchronized ，同步对象是当前实例。
3、在类方法（静态方法）前，加上修饰符 synchronized，同步对象是这个类。

问：synchronized 底层实现，如何实现 Lock？
堆中实例对象的对象头中的重量级锁指针，因为这个锁对象存放在对象本身，也就是为什么Java中任意对象可以作为锁的原因。
参考博客：https://baijiahao.baidu.com/s?id=1612142459503895416&wfr=spider&for=pc


问：一个线程运行结束后不会立马销毁么？会发生什么？
答：结束后等待系统回收资源，线程池内的线程资源应该暂时不会回收。

问：两线程对变量i进行加1操作，结果如何？为什么？怎么解决？


*/
```

**三、 java 线程池**
问：如果一个线程出现异常，那么这个线程会被如何处理？
参考：https://blog.csdn.net/u011635492/article/details/80328815
未理解？？

问：java 实现多个子线程执行完毕后，再执行主线程
知识点：join 和 countDownLatch 进行阻塞

```java
//下面的代码没有实现此功能哦，主线程会先结束，子线程还未执行完毕。
//以下两种做法的区别，参考：https://blog.csdn.net/zhang199416/article/details/70846958
/*
1 final CountDownLatch latch = new CountDownLatch(3); 需要阻塞的线程数量
子线程中添加：latch.countDown();

2 子线程.join();
*/
public class test {
	public static void main(String[] args) {
		System.out.println("主线程开始执行....");
		for (int i = 0; i < 3; i++) {
			new Thread(){
				@Override
				public void run() {
					try {
						System.out.println(Thread.currentThread().getName()+"  开始执行存储过程..");						
						Thread.sleep(1000);
						System.out.println(Thread.currentThread().getName()+"  存储过程执行完毕...");						
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				};
			}.start();
		}
		System.out.println("主线程执行完毕....");
	}
}

```

### JAVA - 虚拟机
<a name="javaJVM"></a>
JAVA虚拟机简介
参考链接：https://blog.csdn.net/qq_41701956/article/details/81664921

JDK各个版本发布时间和版本名称
参考链接：https://blog.csdn.net/J080624/article/details/85259041

JDK1.5，1.6，1.7，1.8，1.9，1.10，1.11的新特性整理
参考链接：https://blog.csdn.net/J080624/article/details/85092655

```java
/*1.程序计数器是一个比较小的内存区域，用于指示当前线程所执行的字节码执行到了第几行，是线程隔离的.
2.虚拟机栈描述的是Java方法执行的内存模型，用于存储局部变量，操作数栈，动态链接，方法出口等信息，是线程隔离的
3.方法区用于存储JVM加载的类信息、常量、静态变量、以及编译器编译后的代码等数据，是线程之间贡献的
4.原则上讲，所有的对象都在堆区上分配内存，是线程之间共享的
*/
```



### JAVA - 垃圾回收机制
<a name="javaGC"></a>

```java
/*JDK中提供了三个ClassLoader，根据层级从高到低为：           
Bootstrap ClassLoader，主要加载JVM自身工作需要的类。          
Extension ClassLoader，主要加载%JAVA_HOME%\lib\ext目录下的库类。          
Application ClassLoader，主要加载Classpath指定的库类，一般情况下这是程序中的默认类加载器，也是ClassLoader.getSystemClassLoader() 的返回值。（这里的Classpath默认指的是环境变量中配置的Classpath，但是可以在执行Java命令的时候使用-cp 参数来修改当前程序使用的Classpath）              JVM加载类的实现方式，我们称为 双亲委托模型：          如果一个类加载器收到了类加载的请求，他首先不会自己去尝试加载这个类，而是把这个请求委托给自己的父加载器，每一层的类加载器都是如此，因此所有的类加载请求最终都应该传送到顶层的Bootstrap ClassLoader中，只有当父加载器反馈自己无法完成加载请求时，子加载器才会尝试自己加载。            双亲委托模型的重要用途是为了解决类载入过程中的安全性问题。       假设有一个开发者自己编写了一个名为Java.lang.Object的类，想借此欺骗JVM。现在他要使用自定义ClassLoader来加载自己编写的java.lang.Object类。然而幸运的是，双亲委托模型不会让他成功。因为JVM会优先在Bootstrap ClassLoader的路径下找到java.lang.Object类，并载入它*/

```


### 网络编程
<a name="javaNet"></a>
[参考W3C 网络编程](https://www.w3cschool.cn/java/java-networking.html)
NIO的Selector实例一般都是Socket网络通信方面，如用户可以在服务端通过一个选择器来管理来自客户端的多个通道，在一些高并发的系统中，会用到选择器来开发各业务模块之间的通信模块，但这是架构师或资深高级程序员要考虑的事情。对于初级程序员，知道一些基本概念即可。

```java
Selector selector = Selector.open(); // 创建一个Selector

```
