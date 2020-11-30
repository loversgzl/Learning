# JAVA-高级编程
以下是学习后的总结，[更具体的，参考这个学习网站，很好用](http://how2j.cn/stage/25.html)

**JAVA - 高级编程**

* <a href="#javaReflect">JAVA - 反射</a>
* <a href="#泛型">JAVA - 泛型</a>
* <a href="#javaIO">JAVA - I/O-NIO</a>
* <a href="#javaThreat">JAVA - 多线程编程</a>
* <a href="#javaJVM">JAVA - 虚拟机</a>
* <a href="#javaGC">JAVA - 垃圾回收，内存优化机制</a>
* <a href="#javaNet">JAVA - 网络编程</a>

### 基础知识
```java
/**序列化**
什么是序列化：https://www.cnblogs.com/douzi520/p/9497889.html
问：序列化是干什么的？
简单说就是为了保存在内存中的各种对象的状态，并且可以把保存的对象状态再读出来。虽然你可以用你自己的各种各样的
方法来保存Object States，但是Java给你提供一种应该比你自己好的保存对象状态的机制,那就是序列化。
  
问：什么情况下需要序列化？
当你想把内存中的对象保存到一个文件中或者数据库中时候，
当你想用序列化在网络上传送对象的时候；
当你想通过 RMI 传输对象的时候；

问：当对一个对象实现序列化时，究竟发生了什么？
Java 在序列化时不会实例化 static 变量和 transient 修饰的变量，因为 static 代表类的成员，transient 代表对象
的临时数据，被声明这两种类型的数据成员不能被序列化。序列化保存的是对象的状态，静态变量属于类的状态，因此，序列
化并不保存静态变量。 

问：java类中serialVersionUID的作用？
参考：https://www.cnblogs.com/duanxz/p/3511695.html
实现Serializable接口的目的是为类可持久化，比如在网络传输或本地存储，为系统的分布和异构部署提供先决条件。若没有
序列化，现在我们所熟悉的远程调用，对象数据库都不可能存在，serialVersionUID适用于java序列化机制。简单来说，
JAVA序列化的机制是通过 判断类的serialVersionUID来验证的版本一致的。在进行反序列化时，JVM会把传来的字节流中的
serialVersionUID于本地相应实体类的serialVersionUID进行比较。如果相同说明是一致的，可以进行反序列化，否则会出
现反序列化版本一致的异常，即是InvalidCastException。

具体序列化的过程是这样的：序列化操作时会把系统当前类的serialVersionUID写入到序列化文件中，当反序列化时系统会
自动检测文件中的serialVersionUID，判断它是否与当前类中的serialVersionUID一致。如果一致说明序列化文件的版本与
当前类的版本是一样的，可以反序列化成功，否则就失败；

serialVersionUID有两种显示的生成方式：
一是默认的1L，比如：private static final long serialVersionUID = 1L;        

二是根据包名，类名，继承关系，非私有的方法和属性，以及参数，返回值等诸多因子计算得出的，极度复杂生成的一个64位的哈希字段。基本上计算出来的这个值是唯一的。比如：private static final long  serialVersionUID = xxxxL;
注意：显示声明serialVersionUID可以避免对象不一致，

相关注意事项：
a）当一个父类实现序列化，子类自动实现序列化，不需要显式实现 Serializable 接口； 
b）当一个对象的实例变量引用其他对象，序列化该对象时也把引用对象进行序列化； 
c）并非所有的对象都可以序列化，,至于为什么不可以，有很多原因了　比如： 
1.安全方面的原因，比如一个对象拥有private，public等field，对于一个要传输的对象，比如写到文件，或者进行rmi传
输 等等，在序列化进行传输的过程中，这个对象的private等域是不受保护的。 
2. 资源分配方面的原因，比如socket，thread类，如果可以序列化，进行传输或者保存，
也无法对他们进行重新的资源分配，而且，也是没有必要这样实现把一个对象完全转成字节序列，方便传输。
就像你寄一箱饼干，因为体积太大，就全压成粉末紧紧地一包寄出去，这就是序列化的作用。
只不过JAVA的序列化是可以完全还原的。
*/

```


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
/**问：序列化有什么作用？java.io.Serializable,将此对象序列化为文件，并在另外一个 JVM 中读取文件，进行反序列
化，请问此时读出的 Data0bject 对象中的 word 和 i 的值分别为："123"，0
注意：序列化的是对象，不是类，类变量不会被序列化，Java 在序列化时不会实例化 static 变量和 transient 修饰的变
量，因为 static 代表类的成员，transient代表对象的临时数据，被声明这两种类型的数据成员不能被序列化。
*/
public class DataObject implements Serializable{
    private static int i=0;
    private String word=" ";
    public void setWord(String word){
        this.word=word;
    }
    public void setI(int i){
        Data0bject.i=I;
     }
}
DataObject object=new Data0bject ( );
object.setWord("123");
object.setI(2);

/*问：transient 关键字？
序列化与反序列化：https://blog.csdn.net/SDDDLLL/article/details/92583968
作用：将不需要序列化的属性前添加关键字transient，序列化对象的时候，这个属性就不会被序列化。
*/

/*代码一：一般说来操作系统都会安装在C盘，所以会有一个 C:\WINDOWS目录。遍历这个目录下所有的文件(不用遍历子目
录)，找出这些文件里，最大的和最小(非0)的那个文件，打印出他们的文件名。注: 最小的文件不能是 0 长度。
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
//代码二、输出电子书文件夹下所有的PDF文件，并统计有多少文件 。
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
/*实现的三种方法*/
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
通过调用Thread类的start方法来启动一个线程，这是此线程处于就绪状态，并没有运行。当得到cpu时间片后，开始执行
run方法。*/

/*启动线程是start()方法，run()并不能启动一个新的线程，但是调用run会执行这个方法，和普通函数调用相同*/
t1.start();
t.close();
Thread.sleep(1000); //线程阻塞 1 秒
t1.join();//在主线程中加入该线程，一般是 main 嘛，当 t1 执行完后，才会继续执行主函数
t1.yield(); //临时暂停，回到就绪状态，让位给同等优先级的其他线程。
t1.setPriority(Thread.MAX_PRIORITY);//为线程 t1 设置优先级，优先级越高，有越大的几率获得 CPU 资源。
t1.setDaemon(true); //如果一个进程只剩下守护线程，则该进程会自动结束。守护线程通常会被用来做日志，性能统计等工作。

/*问：多线程问题
多线程问题：sleep、wait、yield、join、volatile、并发、线程池、锁的实现

问：调用后释放锁的只有 wait+join。
答：刚看到一个大佬写的，给大家参考一下，所谓的释放锁资源实际是通知对象内置的monitor对象进行释放，而只有所有对
象都有内置的monitor对象才能实现任何对象的锁资源都可以释放。又因为所有类都继承自Object，所以wait(）就成了
Object方法，也就是通过wait()来通知对象内置的monitor对象释放，而且事实上因为这涉及对硬件底层的操作，所以
wait()方法是native方法，底层是用C写的。其他都是Thread所有，所以其他3个是没有资格释放资源的,而join()有资格释
放资源其实是通过调用wait()来实现的。

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


**问：sleep 方法 和 wait 方法区别？**
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
有关volatile的几个小面试题：https://blog.csdn.net/u011277123/article/details/72235927
参考博客2：https://blog.csdn.net/u012723673/article/details/80682208
面试官提问：https://blog.csdn.net/weixin_33726313/article/details/87976219

问：被 volatile 修饰的共享变量，具有以下两点特性？
1 . 保证了不同线程对该变量操作的内存可见性;
2 . 禁止指令重排序
JMM 主要就是围绕着如何在并发过程中如何处理原子性、可见性和有序性这3个特征来建立的，通过解决这三个问题，可以解除缓存不一致的问题。而 volatile 跟可见性和有序性都有关。

问：volatile 原理？
当写一个 volatile 变量时，JMM 会把该线程对应的本地内存中的共享变量刷新到主内存；
当读一个 volatile 变量时，JMM 会把该线程对应的本地内存置为无效，线程接下来将从主内存中读取共享变量；
但是 volatile 不能保证原子性，也就不能保证线程安全。

问：volatile 底层的实现机制？
如果把加入 volatile 关键字的代码和未加入 volatile 关键字的代码都生成汇编代码，会发现加入 volatile 关键字的
代码会多出一个 lock 前缀指令。lock 前缀指令实际相当于一个内存屏障，内存屏障提供了以下功能：
1 . 重排序时不能把后面的指令重排序到内存屏障之前的位置 2 . 使得本 CPU 的 Cache 写入内存 
3 . 写入动作也会引起别的 CPU 或者别的内核无效化其 Cache，相当于让新写入的值对别的线程可见。

问：Java 中能创建 volatile 数组吗？修饰一个数组能否保证内存可见性？
能，Java 中可以创建 volatile 类型数组，不过只是一个指向数组的引用，而不是整个数组。如果改变引用指向的数组，
将会受到 volatile 的保护，但是如果多个线程同时改变数组的元素，volatile 标示符就不能起到之前的保护作用了。

问：synchronized 关键字和 volatile 关键字比较？
1、volatile 关键字是线程同步的轻量级实现，所以 volatile 性能肯定比 synchronized 关键字要好。但是 volatile 
关键字只能用于变量而 synchronized 关键字可以修饰方法以及代码块。synchronized 关键字在 JavaSE1.6 之后进行了
主要包括为了减少获得锁和释放锁带来的性能消耗而引入的偏向锁和轻量级锁以及其它各种优化之后执行效率有了显著提升，
实际开发中使用 synchronized 关键字的场景还是更多一些。          
2、多线程访问 volatile 关键字不会发生阻塞，而 synchronized 关键字可能会发生阻塞；         
3、volatile 关键字能保证数据的可见性，但不能保证数据的原子性。synchronized 关键字两者都能保证。           
4、volatile 关键字主要用于解决变量在多个线程之间的可见性，而 synchronized 关键字解决的是多个线程之间访问资
源的同步性。

问：synchronized 关键字和 lock 关键字比较？

问：1、 HashMap和Hashtable的区别   
2、StringBuffer和StringBuilder的区别   
3、ArrayList和Vector的区别   
4、把非线程安全的集合转换为线程安全   
答：https://how2j.cn/k/thread/thread-thread-safe/703.html

问：ConcurrentHashMap如何保证线程安全？

问：JAVA中常见的锁以及其特性？
1、自旋锁
2、自旋锁的其他种类
3、阻塞锁
4、可重入锁
5、读写锁
6、互斥锁
7、悲观锁
8、乐观锁
9、公平锁
10、非公平锁
11、偏向锁
12、对象锁
13、线程锁
14、锁粗化
15、轻量级锁
16、锁消除
17、锁膨胀
18、信号量
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



### JAVA - 垃圾回收机制
<a name="javaGC"></a>

![多线程方法](../../pics/JVM结构.png)

```java
/**Java 虚拟机内存结构**
1.程序计数器是一个比较小的内存区域，用于指示当前线程下一条被执行指令的地址，因此也是也不存在OOM的情况，是线程
隔离的，每个线程都有。
2.虚拟机栈描述的是Java方法执行的内存模型，用于存储局部变量，操作数栈，动态链接，方法出口等信息，是线程隔离的，
每个线程都有自己的私有内存，这也是多线程并发时造成数据不一致的原因。
3.方法区用于存储JVM加载的类信息、常量、静态变量、以及编译器编译后的代码等数据，是线程之间共享的
4.原则上讲，所有的对象都在堆区上分配内存，是线程之间共享的

切入：综合来看，一个对象的创建需要存放多个区域。
金句：new 出来的对象存在于堆区，而这些new出来对象的引用则存在于栈区。
金句：类似 String a = "abc"; 之类的常量存在于常量池中，而常量池存在于方法区中。

**Java 虚拟机内存分配和回收的机制**
概括的说就是分代分配，分代回收，年轻代（Young Generation）、年老代（Old Generation）、永久代（Permanent Generation，也就是方法区）
每次调整分区都会调用 轻量级回收 Minor GC；重量级的 Full GC 以下情况会触发：
1、年老代或持久代被写满；2、程序员显示的调用 System.gc();方法；
注意：在执行 Full GC 时，会导致 Stop the World，虚拟机会终止所有 Java 程序，专门执行 Full GC，这可能会导致程序一直卡在那；

**判断对象可回收的依据**
标准：当某个对象上没有强引用时，该对象就可以被回收了。
早期版本采用“引用计数”，优点是简单，缺点是无法回收循环引用的对象，如 a=b; b=c; c = a;则无法回收；
后续版本采用了“根搜索算法”，这个算法将从一个根节点开始，寻找它所对应的引用节点，找到这个节点后，继续寻找该节点的引用节点。主要从方法区的常量和静态变量，两个栈，开始搜索引用；

**finalize 方法**
当通过上述介绍的根搜索算法回收某个对象时，先判断该对象是否重写了 Object 的 finalize 方法，没有则直接回收。

**虚拟机的类加载器**
JDK 中提供了三个 ClassLoader，根据层级从高到低为：           
启动类加载器：Bootstrap ClassLoader，主要加载JVM自身工作需要的类。          
扩展类加载器：Extension ClassLoader，主要加载%JAVA_HOME%\lib\ext目录下的库类。          
应用程序类加载器：Application ClassLoader，主要加载Classpath指定的库类，一般情况下这是程序中的默认类加载器，
也是ClassLoader.getSystemClassLoader() 的返回值。（这里的Classpath默认指的是环境变量中配置的Classpath，但
是可以在执行Java命令的时候使用-cp 参数来修改当前程序使用的Classpath）JVM加载类的实现方式，我们称为 双亲委托模
型：如果一个类加载器收到了类加载的请求，他首先不会自己去尝试加载这个类，而是把这个请求委托给自己的父加载器，每
一层的类加载器都是如此，因此所有的类加载请求最终都应该传送到顶层的Bootstrap ClassLoader中，只有当父加载器反馈
自己无法完成加载请求时，子加载器才会尝试自己加载。双亲委托模型的重要用途是为了解决类载入过程中的安全性问题。 
假设有一个开发者自己编写了一个名为Java.lang.Object的类，想借此欺骗JVM。现在他要使用自定义ClassLoader来加载自
己编写的java.lang.Object类。然而幸运的是，双亲委托模型不会让他成功。因为JVM会优先在Bootstrap ClassLoader的
路径下找到java.lang.Object类，并载入它

**强、弱、软、虚四种引用**
强引用：指向通过 new 得到的内存空间，如 String a = new String("123");
软引用（SoftReference）：如果一个对象只具有软引用，而当前虚拟机堆内存空间足够，则不会回收，反之回收；
应用场景：博客管理系统，将博客缓存到内存中，缩短响应时间，当内存不足时，将暂时每人看的博客缓存清除；
弱引用（WeakRefernce）：如果某块内存上只有弱引用，则一定会回收；
应用场景：优惠券，某个优惠券强引用小时，使用的用户自动更新；
虚引用（PhantomReference）：1.虚引用必须和引用队列（ReferenceQueue）一起使用；2.始终无法通过虚引用得到他所
指向的值；

**面试题**
问：简述 Java 虚拟机的内存结构？
1、回答五个分区；2、回答不同分区包含的数据；3、金句点题；
问：简述 Java 垃圾回收的大致流程？
1.回答三个分区；2.回答Minor GC和Full GC；3.垃圾回收的算法；



```

### 内存调优

```java
/**基本概念**
Stop-the-world意味着从应用中停下来并进入到GC执行过程中去。一旦Stop-the-world发生，除了GC所需的线程外，其他线
程都将停止工作，中断了的线程直到GC任务结束才继续它们的任务。GC调优通常就是为了改善stop-the-world的时间。



**下面太难了**
1、线上系统CPU，内存与磁盘IO暴增，你会如何调优？

问：你们JVM线上使的什么垃圾回收器？CMS还是G1？
https://blog.csdn.net/gui694278452/article/details/104781237/
CMS收集器是一种以获取最短回收停顿时间为目标的收集器，CMS是一款优秀的收集器，它的主要优点是：并发收集、低停顿，
就是与用户线程共同运行，但他有以下3个明显的缺点：1、CMS收集器对CPU资源非常敏感，降低运行效率；
G1(Garbage First)是一款面向服务端应用的垃圾收集器。

3、CMS的并发更新失败是怎么回事？如何优化？

4、JVM是任何时刻都可以STW吗？为什么？

5、线上系统GC问题如何快速定位与分析？阿里巴巴的Arthas用过吗？

6、单机几十万并发的系统JVM如何优化？

7、高并发系统为何建议选择G1垃圾收集器？

8、能说说Mysql索引底层B+树结构与算法吗？

9、聚集索引与覆盖索引与索引下推到底是什么？

10、能说说Mysql并发支撑底层Buffer Pool机制吗？

11、一线互联网公司的数据库架构是如何设计的?

12、对线上千万级大表加字段时，性能极慢问题如何处理？

*/

```


### 网络编程
<a name="javaNet"></a>
[参考W3C 网络编程](https://www.w3cschool.cn/java/java-networking.html)
NIO的Selector实例一般都是Socket网络通信方面，如用户可以在服务端通过一个选择器来管理来自客户端的多个通道，在一些高并发的系统中，会用到选择器来开发各业务模块之间的通信模块，但这是架构师或资深高级程序员要考虑的事情。对于初级程序员，知道一些基本概念即可。

```java
Selector selector = Selector.open(); // 创建一个Selector

```
