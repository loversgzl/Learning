# JAVA-高级编程
以下是学习后的总结，[更具体的，参考这个学习网站，很好用](http://how2j.cn/stage/25.html)

**JAVA - 高级编程**

* JAVA - 数据结构
* JAVA - 网络编程
* JAVA - 多线程编程
* JAVA - 分布式编程
* JAVA - 框架

### JAVA - 反射
在对象方法前，加上修饰符 synchronized ，同步对象是当前实例。
那么如果在类方法前，加上修饰符 synchronized，同步对象是这个类的反射。

使用反射方式，首先准备一个配置文件，就叫做 spring.txt 吧, 放在 src 目录下。 里面存放的是类的名称，和要调用的方法名。在测试类Test中，首先取出类名称和方法名，然后通过反射去调用这个方法。当需要从调用第一个业务方法，切换到调用第二个业务方法的时候，不需要修改一行代码，也不需要重新编译，只需要修改配置文件 spring.txt，再运行即可。
这也是 Spring 框架的最基本的原理，只是它做的更丰富，安全，健壮。

**获取类对象的三种方式**
Class.forName（“Hero”），Hero.class，new Hero().getClass()，（Hero 是一个类）在一个JVM中，一种类，只会有一个类对象存在。所以以上三种方式取出来的类对象，都是一样的。反射其实就是获取类对象。

### JAVA - I/O

** I/O 文件对象**
**例题**：一般说来操作系统都会安装在C盘，所以会有一个 C:\WINDOWS目录。遍历这个目录下所有的文件(不用遍历子目录)，找出这些文件里，最大的和最小(非0)的那个文件，打印出他们的名。注: 最小的文件不能是 0 长度。
知识点：1如何创建文件对象，2返回文件夹下所有子文件的函数，3一个文件的长度
```java
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

练习：对 d 盘的一个文件进行读取。
知识点：1文件输入输出流的创建，
```java
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
			byte[] all = new byte[(int)f.length()];
			fis.read(all);
			for(byte b : all){
				System.out.println(b); //输出asc码
			}
			fis.close(); //每次使用完需要关闭
		} catch(IOException e){
			e.printStackTrace();
		}
	}
}
```

### JAVA - 多线程编程

**一、创建线程 和 线程常用方法**
创建多线程有 3 种方式，分别是 继承线程类，实现 Runnable 接口，匿名类。
继承线程类（需要重写 run 方法）：创建一个对象就是一个线程。
实现 Runnable 接口（需要重写 run 方法）：和上面相似，只是在实例化的时候有区别。
匿名类（不需要额外增加一个类）：和匿名函数相似，在用的时候才写，而且直接写在主函数里面。
注： 启动线程是start()方法，run()并不能启动一个新的线程

```java
Thread t1 = new Thread(){
	@Override
	public void run(){}//匿名函数修改 run 函数
}

/*
常用方法：
*/
t1.start(); //启动线程 t1
Thread.yield(); //临时暂停，回到就绪状态。
t1.sleep(1000); //t1 线程暂停 1 秒
t1.join();
//在主线程中加入该线程，一般是 main 嘛，当 t1 执行完后，才会继续执行主函数
t1.setPriority(Thread.MAX_PRIORITY);
//为线程 t1 设置优先级，优先级越高，有越大的几率获得 CPU 资源。
t1.setDaemon(true); //如果一个进程只剩下守护线程，则该进程会自动结束。守护线程通常会被用来做日志，性能统计等工作。
```

**二、线程同步问题**
问题：多个线程同时修改一个数据的时候，可能导致问题。
1. 线程同步的关键字
2. 同步主线程和子线程的三种方法
2.1 构造一个 object 对象，独占此对象才可执行该线程。
2.2 将此对象设定为 类的实例，则独占此实例才可调用此实例的方法（被关键字 synchronized  修饰过的多个方法，只有独占此实例，才可调用其中的任意一个），
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

/*
3.线程交互 wait() 和 notify()
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
问：可以用线程实现生产者消费者模型么？

问：lock 和 synchronized 的区别？
答：与  synchronized (someObject)  类似的，lock() 方法，表示当前线程占用 lock 对象，一旦占用，其他线程就不能占用了。
与 synchronized 不同的是，一旦 synchronized 块结束，就会自动释放对 someObject 的占用。 lock却必须调用 unlock 方法进行手动释放，为了保证释放的执行，往往会把 unlock() 放在 finally 中进行。

疑问区：
疑问：一个线程运行结束后不会立马销毁么？？？会发生什么？？
疑问：脏数据？？？
疑问：主线程和子线程的运行顺序如何？？
java：equals 和 == 的区别？？
疑问：wait 和sleep 的区别？？
this.wait()表示 让占有 this 的线程等待，并临时释放占有


### 网络编程
[参考W3C 网络编程](https://www.w3cschool.cn/java/java-networking.html)
能够实现：如何创建服务器套接字、如何创建客户端套接字、
知识点：获取本机 IP 地址，使用 Java 执行 ping 命令，

```java
/*
练习：实现查看本网段哪些 IP 地址已被占用，单线程超级慢，要十几分钟。
进阶：可考虑使用多线程实现。
*/
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
public class test{
	public static void main(String[] args) throws IOException{
	//可执行 exe 程序
		Process p = Runtime.getRuntime().exec("ping " + "www.baidu.com");
	//获取 流 到缓存中
		BufferedReader br = new BufferedReader(new InputStreamReader(p.getInputStream()));
		StringBuilder sb = new StringBuilder();
		String line = null;
		while((line = br.readLine()) != null){
			if(line.length() != 0)
				sb.append(line + "\r\n");
		}
		System.out.println(sb.toString());
	}
}

```

实例：编写客户端和服务器端双向通信，多线程实现。
```java
// 客户端代码
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.net.UnknownHostException;
import java.util.Scanner;
 
public class ClientThread {
 
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        try{
            Socket socket = new Socket("127.0.0.1",9612);
            SendServer(socket);
            ReceiveServer(socket);
             
        } catch (UnknownHostException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
 
    private static void SendServer(Socket socket) {
        new Thread() {
 
            @Override
            public void run() {
                //synchronized (socket) {
                    try (OutputStream os = socket.getOutputStream();
                            DataOutputStream dos = new DataOutputStream(os);
                            Scanner scann = new Scanner(System.in);) {
                        while (true) {
                            String msg = scann.nextLine();
                            dos.writeUTF(msg);
                        }
 
                    } catch (IOException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
            //}
             
        }.start();
    }
 
    private static void ReceiveServer(Socket socket) {
        new Thread() {
 
            @Override
            public void run() {
                synchronized (socket) {
                    try (InputStream ins = socket.getInputStream(); DataInputStream dins = new DataInputStream(ins);) {
                        while (true) {
                            //System.out.println("从服务器收到的信息：");
                            String msg = dins.readUTF();
                            System.out.println("从服务器收到的信息："+msg);
                        }
 
                    } catch (IOException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
            }
             
        }.start();
    }
 
}
 
// 服务端代码
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;
 
public class ServerThread {
 
    public static void main(String[] args) {
        System.out.println("运行中……");
        try{
            ServerSocket serversocket = new ServerSocket(9612);
            System.out.println("服务器等待连接");
            Socket socket = serversocket.accept();
            // 设置服务器监听的端口
            ReceiveClient(socket);
            SendClient(socket);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
 
    private static void ReceiveClient(Socket socket) {
        new Thread() {
 
            @Override
            public void run() {
                try (
                        InputStream ins = socket.getInputStream();
                        DataInputStream dins = new DataInputStream(ins);
                ){
                    while (true) {
                        String msg = dins.readUTF();
                        System.out.println("接受客户端的信息："+msg);
                    }
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }
             
        }.start();
    }
 
    private static void SendClient(Socket socket) {
        new Thread() {
 
            @Override
            public void run() {
                //synchronized (socket) {
                    try (OutputStream os = socket.getOutputStream();
                            DataOutputStream dos = new DataOutputStream(os);
                            Scanner scann = new Scanner(System.in);) {
                        while (true) {
                            String msg = scann.nextLine();
                            dos.writeUTF(msg);
                        }
 
                    } catch (IOException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                //}
            }
             
        }.start();
    } 
}
```


