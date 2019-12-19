# JAVA-EE
### J2EE 主要技术
[参考博客](https://blog.csdn.net/Neuf_Soleil/article/details/80962686)
JavaEE 号称有十三种核心技术。它们分别是：**JDBC、Servlet、JSP**、JNDI、EJB、RMI、XML、JMS、Java IDL、JTS、JTA、JavaMail 和 JAF。一般来讲，初学者应该遵循路径
JDBC -> Servlet -> JSP -> Spring -> 组合框架。

JDBC：JAVA 操作数据库
Tomcat：常见的免费 web 服务器
Servlet：用于处理用户提交的数据
JSP：可以写 java 代码的 html

### JDBC
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
public class test {
	public static void main(String[] args) {
		try{
			Class.forName("com.mysql.jdbc.Driver");//加载数据库驱动类，在引入的 jar 包中
		}catch(ClassNotFoundException e) {//此异常无需引包
			e.printStackTrace();
		}
		//建立与数据库的连接，获取 statement 对象，执行 SQL 语句
		//放在try 里面，是关闭流，执行完自动关闭，否则需要手动关闭
		try (Connection con = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/school?characterEncoding=UTF-8",	"root","1234");	Statement s = con.createStatement();){
			//执行 SQL 语句，字符串用单引号。
			String insert = "insert into students value(5,'王五',1,26,1,curdate());";
             String delete = "delete from students where age = 25;";
			String update = "update students set age = 24 where id = 1; ";
			s.execute(update);
             for(int i=2; i<100; i++) {//for循环插入多条语句，注意 '%s'，
				insert = String.format("insert into hero value(%d,'%s',%d,%d);", 				                    i,"hero"+i,100+i,50+i);
				s.execute(insert);
				System.out.println("执行完"+i);
			}
		} catch(SQLException e) {
			e.printStackTrace();
		} 
	}
}
//SELECT	由于 select 语句会有返回值，所以单独列出
import java.sql.ResultSet;
ResultSet rs = s.executeQuery(select);
while(rs.next()){
    //可以根据字段名称，或者序号从 1 开始，获得返回值。
    int id = rs.getInt("id"); 
    String name = rs.getString(2);
    System.out.printf("id: %d, name: %s\n",id,name);
}

//SQL语句判断账号密码是否正确：错误的做法是把所有数据加载进内存，应该去数据库中查找
if(rs.next())
    System.out.println("账号密码正确");

/*
Statement 和 PreparedStatement 比较
Statement 需要进行字符串拼接，可读性和维护性比较差，
改进 PreparedStatement，1可读性更好，2有预编译机制，速度更快。3防止SQL注入攻击。
*/
Statement s = con.createStatement();
s.execute(sql);//这个sql语句需要拼接 sql = 'a'+i;
PreparedStatement ps = con.prepareStatement(sql);
//这里sql = insert into hero values(?,?,?)，会进行预编译，之后除了参数其他语句都无效
ps.setString(1,"张三");
ps.setInt(2,25);
ps.execute();


/*
execute 与 executeUpdate 的区别
不同1：
execute可以执行查询语句
然后通过getResultSet，把结果集取出来
executeUpdate不能执行查询语句
不同2:
execute返回boolean类型，true表示执行的是查询语句，false表示执行的是insert,delete,update等等
executeUpdate返回的是int，表示有多少条数据受到了影响
*/
s.execute(sqlSelect);
ResultSet rs = s.getResultSet();
while (rs.next()) 
    System.out.println(rs.getInt("id"));

/*
事务	只有所有操作都正确执行，事务才发生
MYSQL 表的类型必须是INNODB才支持事务
相关操作主要有四种：
*/

c.setAutoCommit(false); //自动提交关闭
s.execute(sql1);
s.execute(sql2);
c.commit();// 手动提交,提交一个事务
begin();//开始一个事务
rollback();//回滚一个事务
prepare();//准备提交一个事务



/*
ORM=Object Relationship Database Mapping 
对象和关系数据库的映射 ,简单说，一个对象，对应数据库里的一条记录
对象的属性，就是数据库中不同的字段
DAO = DataAccess Object，把数据库相关的操作都封装在这个类里面，其他地方看不到JDBC的代码。
*/

/*
数据库连接池
当有多个线程，每个线程都需要连接数据库执行SQL语句的话，那么每个线程都会创建一个连接，并且在使用完毕后，关闭连接。创建数据库连接和关闭数据库连接的过程也是特别消耗系统资源的，当多线程并发的时候，系统就会变得很卡顿。
同时，一个数据库同时支持的连接总数也是有限的，如果多线程并发量很大，那么数据库连接的总数就会被消耗光，后续线程发起的数据库连接就会失败。

与传统方式不同，连接池在使用之前，就会创建好一定数量的连接。如果有任何线程需要使用连接，那么就从连接池里面借用，而不是自己重新创建.使用完毕后，又把这个连接归还给连接池供下一次或者其他线程使用。倘若发生多线程并发情况，连接池里的连接被借用光了，那么其他线程就会临时等待，直到有连接被归还回来，再继续使用。整个过程，这些连接都不会被关闭，而是不断的被循环使用，从而节约了启动和关闭连接的时间。
*/

```

问：JDBC事务与JTA事务的区别？
JDBC事务缺点：事务的范围局限于一个数据库连接。一个 JDBC 事务不能跨越多个数据库。JTA 事务提供了跨数据库连接（或其他JTA资源）的事务管理能力。这一点是与JDBC Transaction最大的差异。JDBC事务由Connnection管理，也就是说，事务管理实际上是在JDBC Connection中实现。事务周期限于Connection的生命周期。同样，对于基于JDBC的ibatis事务管理机制而言，事务管理在SqlMapClient所依托的JDBC Connection中实现，事务周期限于SqlMapClient 的生命周期。

JTA事务管理则由 JTA容器实现，JTA容器对当前加入事务的众多Connection进行调度，实现其事务性要求。JTA的事务周期可横跨多个JDBC Connection生命周期。




### TomCat
[安装 TomCat](https://www.cnblogs.com/limn/p/9358657.html)
简介：常见的免费 web 服务器。
启动：F:\JavaWorkSpace\apache-tomcat-9.0.26\bin\startup.bat
注意：每次修改源文件或者其他文件都要重新启动。
修改：F:\JavaWorkSpace\apache-tomcat-9.0.26\conf\server.xml
里面的 Context 会将访问的 ip 地址指向一个项目的 web 文件夹下，然后通过里面的 web.xml 找到对应 classes/Servlet.class 文件进行相应的处理。记住不是.java 文件，服务器会找到.class文件进行处理，开始创建这个项目更改了 .class 文件的路径，所以需要重新编译这个项目，重新生成 .class文件（找了老半天的错误，总是404，后来才发现是没有 .class 文件）：
右击工程 -> properties -> java compiler -> Enable project specific settings[打钩] -> compiler compliance level 改成11。之后，eclipse会问你是否重新编译，当然选是，要得就是这个问题。


### Servlet
<img src="../../pics/servlet.png" align="center">
[ 参考博客](https://learner.blog.csdn.net/article/details/81091580)

简介：Servlet用于处理用户提交的数据，熟悉 request 和 response 两种方法，了解不同的跳转方式。
熟悉 servlet 对数据库的基本操作。
Servlet 本身不能独立运行，需要在一个 web 应用中运行，而一个 web 应用是部署在 tomcat 中的，所以开发一个 servlet 需要如下几个步骤：
1.创建 web 应用项目；2.编写 servlet 代码；3.部署到 tomcat 中
浏览器输入 ip 地址，通过 TomCat 免费服务器（里面的 cof/server.xml 文件设置访问的 classes  文件的路径，即通过浏览器可以访问到的文件夹），在 java 项目的.classes 文件夹中包含 .xml 文件，分析 浏览器的 ip 地址访问的是哪个页面，然后根据不同的页面，映射对应的 Servlet。

一、开发 Servlet，打开 TomCat，访问地址为：http://127.0.0.1:8080/hello
对应的servlet中有doGet方法，即执行 web 浏览器的请求的get方法。
二、获取参数，访问地址为：http://127.0.0.1:8080/login.html
通过html文件继续访问，点击提交后会执行对应的servlet中doPost方法。
三、如果浏览器是 GET 请求，那么服务端是 Response 给予响应；
如果浏览器是 POST 表单，那么服务端是 Request 获取信息；
四、继承 HttpServlet 的同时，也继承了一个 service 方法，判断是执行 doPost 还是 doGet，可以直接重写该方法，那么就不需要判断了。三者参数相同。
五、获取中文，添加：request.setCharacterEncoding("UTF-8"); 
响应中文，添加：response.setContentType("text/html; charset=UTF-8");

六、生命周期：
servlet接口定义了servlet的生命周期方法：init（）、service（）、destory（）三个方法
实例化：在路径找到对应的servlet后，如发现没有实例（即第一次调用），则会调用该类的默认构造方法，且只会执行一次，因为是单例的。
初始化：init 方法是一个实例方法，所以会在构造方法执行后执行。Servlet是单例的，所以只会初始化一次。
提供服务：在service()中编写业务代码。
销毁destroy()：如下情况会调用 1该Servlet所在的web应用重新启动，2关闭tomcat的时候 destroy()方法会被调用
被回收：被销毁后就会很快被回收

七、服务端跳转页面，并返回给客户端：（路径不变，只是将内容传递过去）
request.getRequestDispatcher("success.html").forward(request, response);
或者直接通知客户端自己进行跳转：response.sendRedirect("fail.html");
八、Servlet 自启动

**问：Java Servlet 与使用 CGI（Common Gateway Interface，公共网关接口）有什么优势**：
1. 性能明显更好。
2. Servlet 在 Web 服务器的地址空间内执行。这样它就没有必要再创建一个单独的进程来处理每个客户端请求。
3. Servlet 是独立于平台的，因为它们是用 Java 编写的。
4. 服务器上的 Java 安全管理器执行了一系列限制，以保护服务器计算机上的资源。因此，Servlet 是可信的。
5. Java 类库的全部功能对 Servlet 来说都是可用的。它可以通过 sockets 和 RMI 机制与 applets、数据库或其他软件进行交互。

**问：当多个客户请求一个Servlet时，服务器为每一个客户启动一个进程？**
1.不一定， Servlet 可以是单线程的，也可以是多线程的。
3.当多个浏览器终端请求web服务器的时候，服务器为每个客户启动一个线程，不是进程。(选择中喜欢偷换概念，在这个上面做文章。)

**问：在开发servlet继承HttpServlet时如何处理父类的service方法？**
答：一般我们都是不对service方法进行重载(没有特殊需求的话)，而只是重载doGet()之类的doXxx()方法，减少了开发工作量。但如果重载了service方法，doXXX()方法也是要重载的。即不论是否重载service方法，doXXX()方法都是需要重载的。
[额外知识补充](https://my.oschina.net/dtkking/blog/89443)

```java
//
import java.io.IOException;
import java.io.PrintWriter;//response返回html的类

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;//需要继承的类
import javax.servlet.http.HttpServletRequest;//处理POST
import javax.servlet.http.HttpServletResponse;//处理GET
public class HelloServlet extends HttpServlet{

/*import javax.servlet.http.httpservletrequest，你看这个包说明servlet是一个特殊的Java类， java和javax都是Java的API包，java是核心包，javax的x是extension的意思，也就是扩展包。
服务端初始化，肯定在构造函数之后执行
*/
public void init(ServletConfig config) {//服务器启动初始化
		for(int i=0; i<10; i++)
			System.out.println("init of Hello Servlet");
}

/*
实际上，在执行doGet()或者doPost()之前，都会先执行service()
由service()方法进行判断，到底该调用doGet()还是doPost()
可以发现，service(), doGet(), doPost() 三种方式的参数列表都是一样的。
所以，有时候也会直接重写service()方法，在其中提供相应的服务，就不用区分到底是get还是post了。
*/
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {}
	
	
/*哪些是 get 的方式？
form 默认的提交方式，通过 ip 地址访问，处理请求
*/
public void doGet(HttpServletRequest request, HttpServletResponse response)throw ServletException,IOException{//返回响应
		//响应构造 html 编码,如果有中文第一句不能省。
		response.setContentType("text/html; charset=UTF-8"); 
  	    response.getWriter().println("<h1>three test!</h1>");
    }
//处理发送的表单，一般在 form 上显示设置 method="post" 的时候
protected void doPost(HttpServletRequest request, HttpServletResponse response)throws IOException,ServletException{//处理表单
    //如果 post 里面有中文，要进行中文编码
    request.setCharacterEncoding("UTF-8");
	String name = request.getParameter("name");//获取表单里对应属性的值
    System.out.println(name);//打印在服务器端

    String html = null;//设置html返回给客户端
    html = "<div style='color:green'>登录成功</div>";
    //如果html编码里有中文，需要进行中文编码
    response.setContentType("text/html; charset=UTF-8"); 
    PrintWriter pw = response.getWriter();
    pw.println(html);
	}
	
/*
进行页面间的跳转
服务端跳转：浏览器地址不会变，
发命令让客户端跳转：浏览器地址会改变，
*/
request.getRequestDispatcher("success.html").forward(request, response);
response.sendRedirect("fail.html");//客户端跳转
}

/*Servlet 自启动
在 web.xml 中，在某个servlet方法 <servlet-class> 下增加一句
<load-on-startup>10</load-on-startup>
取值范围是1-99，即表明该Servlet会随着Tomcat的启动而初始化，即启动服务器时会自动初始化该servlet方法，调用该类的构造方法和init方法。
<load-on-startup>10</load-on-startup> 中的 10 表示启动顺序
如果有多个Servlet都配置了自动启动，数字越小，启动的优先级越高
*/


/*
Request的方法
*/
request.setCharacterEncoding("UTF-8");//设置表单值的中文编码
request.getParameter("name");//获取表单属性值
//服务端跳转网页
request.getRequestDispatcher("success.html").forward(request, response);
request.getRequestURL();//浏览器发出请求时的完整URL，包括协议 主机名 端口
request.getRequestURI();//浏览器发出请求的资源名部分，去掉了协议和主机名
request.getQueryString();//请求行中的参数部分
request.getRemoteAddr();//浏览器所处于的客户机的IP地址
request.getRemoteHost();//浏览器所处于的客户机的主机名
request.getRemotePort();//浏览器所处于的客户机使用的网络端口
request.getLocalAddr();//服务器的IP地址
request.getLocalName();//服务器的主机名 
request.getMethod();//得到客户机请求方式

/*
Response的方法，返回html元素
*/
response.setContentType("text/html; charset=UTF-8");//设置html的中文编码
response.getWriter().println(html);//获取返回html的对象
response.sendRedirect("fail.html");//客户端跳转网页
```

### web.xml 
浏览器通过服务器会访问到此文件，根据里面对应的路径进行
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app>
    <servlet>
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>HelloServlet</servlet-class>
    </servlet>
 
    <servlet-mapping>
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
 
</web-app>
```

### Servlet 的 CRUD操作
```java
/*
ORM=Object Relationship Database Mapping 
对象和关系数据库的映射 ,简单说，一个对象，对应数据库里的一条记录
对象的属性，就是数据库中不同的字段
DAO = DataAccess Object，把数据库相关的操作都封装在这个类里面，其他地方看不到JDBC的代码。
*/

```

### JSP
到这里，大家对使用 Servlet 进行CRUD开发就有比较全面感性认识了。 其中一个比较明显的弊端就是在 Servlet 编写 html 代码很痛苦，效率不高，可读性差，难以维护。最好可以在 html 文件里面写html 代码，同时又能在里面调用 java 的变量，那么这样就需要学习 JSP 了。
  全称（Java Server Pages）是一种动态网页开发技术。它使用 JSP 标签在 HTML 网页中插入 Java 代码。标签通常以 <%开头以%> 结束。与 JavaScript 相比：虽然 JavaScript 可以在客户端动态生成 HTML，但是很难与服务器交互，因此不能提供复杂的服务，比如访问数据库和图像处理等等。 **执行过程**：把 hello.jsp 转译为 hello_jsp.java 文件，这个文件继承了 HttpServlet，所以它就是一个Servlet，之后的处理就都是一样的了，编译为 class 文件，处理响应等等。

**JSP 和 Servlet 的区别**
从网络三层结构的角度看 JSP 和 Servlet 的区别，一个网络项目最少分三层：data layer(数据层)，business layer(业务层)，presentation layer(表现层)。当然也可以更复杂。Servlet 用来写 business layer 是很强大的，但是对于写  presentation layer  就很不方便。JSP 则主要是为了方便写 presentation layer 而设计的。综上所述，Servlet 是一个早期的不完善的产品，写 business layer 很好，写 presentation layer 就很臭，并且两层混杂。
所以，推出 JSP+BEAN，用 JSP 写 presentation layer，用 BEAN 写 business layer。SUN 自己的意思也是将来用 JSP 替代 Servlet。这是技术更新方面 JSP 和 Servlet 的区别。可是，这不是说，学了 Servlet 没用，实际上，你还是应该从 Servlet 入门，再上 JSP，再上 JSP+BEAN。
强调的是：学了JSP，不会用 Java BEAN 并进行整合，等于没学。大家多花点力气在 JSP+BEAN 上。
**常见容器**：Tomcat, Jetty, resin, Oracle Application server, WebLogic Server, Glassfish, Websphere, JBoss 等等。（提供了 Servlet 功能的服务器，叫做 Servlet 容器。对 web 程序来说，Servlet 容器的作用就相当于桌面程序里操作系统的作用，都是提供一些编程基础设施）

![JSP语法 ](../../pics/jsp语法.png)

```jsp
<%@page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" import="java.util.*" %>

<%-- 日期，相当于 response.getWriter()，前面带 = 的属于需要输出的内容，不用分号结尾
如果没有则表示正常的 java 代码，需要用分号结尾 --%>
<%= new Date().toLocaleString() %>


<%-- java 代码需要加分号 --%>
<%
List<String> words = new ArrayList<>();
words.add("I"); 
words.add("am");
words.add("a");
words.add("good");
words.add("boy");
%>

<%-- 输出一个表格 --%>
<table width="200px" align="center" border="1" cellspacing="0">
<% for (String word : words) { %>
<tr>
    <td><%=word%></td>
</tr>
<% } %>
</table>


<%-- include 使用模板页 ，footer.jsp，并传递参数 --%>
<%@include file="footer.jsp" %> <%-- 指令，编译像宏，且变量模板页可以任意访问 --%>
<jsp:include page="footer.jsp"> <%-- 动作，编译像函数，会调用 --%>
	<jsp:param name="year" value="2017"/> <%-- 这里最后的斜杠总是忘记，模板页需要获取参数才可使用 --%>
</jsp:include>
<p style="text-align:center">copyright@<%=request.getParameter("year")%></p>

<%-- 跳转页面，可以使用 servlet 的代码，下面是 jsp 的服务端跳转--%>
<jsp:forward page="hello.jsp" />
<%-- 客户端跳转和在原servlet中不变--%>
<%    response.sendRedirect("hello.jsp"); %>

<%@page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" import="javax.servlet.http.Cookie">
<%-- 设置 (属性，值)，生存时间，保存路径 --%>
<%
Cookie c = new Cookie("name", "gareen");
c.setMaxAge(24*60*60); 
c.setPath('/');
response.addCookie(c);
%>

<%-- 获取 Cookie --%>
<%
Cookie[] cookies = request.getCookies();
if(cookies != null)
	for(int i=0; i<cookies.length; i++)
        out.print(cookies[i].getName()+":"+cookies[i].getValue());
%>

<%-- 引入 javax.servlet.http.Cookie 就包含session --%>
<%
session.setAttribute("name", "gareen");
String name = (String)getAttribute("name");
%>

<%-- 九种隐式对象：不需要显示定义，直接就可以使用的对象
out：输出 <% out.println("hello jsp");%>
page：表示当前对象
request：请求
response：响应
session：会话作用域

pageContext：当前页面作用域
application：全局作用域

config：可以获取一些在 web.xml 中初始化的参数。
exception：只有当前页面的<%@page 指令设置为isErrorPage="true"的时候才可以使用。

JSP有4个作用域，分别是
pageContext 当前页面的jsp页面，跳转就访问不到了
request 一次请求，结束数据即被回收。注意：客户端跳转，浏览器会发生一次新的访问，新的访问会产生一个新的request对象。所以页面间客户端跳转的情况下，是无法通过request传递数据的。（可以使用服务器跳转）
session 当前会话可以任意访问，但是不能共享不同用户的数据。
application 全局，所有用户共享
--%>


<%-- JSTL JSP Standard Tag Library 标准标签库 --%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<c:set var="name" value="${'gareen'}" scope="request" />
通过标签获取name: <c:out value="${name}" /> <br>
<c:remove var="name" scope="request" /> <br>

<c:if test="${hp<5}">
    <p>这个英雄要挂了</p>
</c:if>
<c:if test="${!(hp<5)}">
    <p>这个英雄觉得自己还可以再抢救抢救</p>
</c:if>

<c:forEach items="${heros}" var="hero" varStatus="st"  >
    <tr>
        <td><c:out value="${st.count}" /></td>
        <td><c:out value="${hero}" /></td>
    </tr>
</c:forEach>




<%-- EL 表达式--%>
通过标签获取name: <c:out value="${name}" /> <br>
通过 EL 获取name: ${name}
英雄名字 ： ${hero.name} <%-- 会自动调用 get,set 方法--%>
英雄血量 ： ${hero.hp}


```

**Cookie**：Cookie 是一种浏览器和服务器交互数据的方式。Cookie 是由服务器端创建，但是不会保存在服务器。创建好之后，发送给浏览器。浏览器保存在用户本地。下一次访问网站的时候，就会把该 Cookie 发送给服务器。

**Session**：Session对应的中文翻译是会话。会话指的是从用户打开浏览器访问一个网站开始，无论在这个网站中访问了多少页面，点击了多少链接，都属于同一个会话。 直到该用户关闭浏览器为止，都属于同一个会话。

盒子对应服务器上的 Session，十五分钟或者半小时没有访问过可能会清除，钥匙（SessionID）对应保存在浏览器上的 Cookie。通过钥匙可以找到盒子，会在第一次访问创建一个sessionID，返回给你保存在 Cookie 里，等再次访问时，服务端取出 ID。如果没有 Cookie 那么每次只能重新生成 session。

### MVC
Servlet 相当于在 Java 代码里面写 html，肯定很繁琐，所有html都是字符串拼接起来的。
JSP 相当于在 HTML 里面写代码，用两个%括起来，所有的变量也要括起来，也很麻烦。
所以单独用一个做很繁琐，就将两个的优势结合，就是MVC的思想。
Modle 模型-数据（DAO,Bean），View 视图-网页（JSP），Controller 控制器（Servlet）。
其中可以看出，是视图是.jsp，控制器和模型都是 .java文件，除了web.xml。接下来学习JSP+Bean
```java
/*
Servlet 只用来从数据库中查询 Hero 对象，然后跳转到 JSP 页面，如果保存在request，需要服务器跳转，因为它的有效期是一次会话。
*/
<%-- controlerHero.java --%>
int id = Integer.parseInt(request.getParameter("id"));
Hero hero = new HeroDAO().get(id);
request.setAttribute("hero", hero);
request.getRequestDispatcher("editHero.jsp").forward(request, response);

```

```jsp
<%-- viewHero.jsp --%>
<form action='updateHero' method='post'>
    名字 ： <input type='text' name='name' value='${hero.name}'> <br>
    血量 ：<input type='text' name='hp' value='${hero.hp}'> <br>
    伤害： <input type='text' name='damage' value='${hero.damage}'> <br>
    <input type='hidden' name='id' value='${hero.id}'>
    <input type='submit' value='更新'>
</form>
```


### Filter





### GUI（有兴趣可以自己了解）
**Swing**
Swing 是一个为Java设计的GUI工具包。包括了图形用户界面（GUI）器件如：文本框，按钮，分隔窗格和表。
**五子棋**：1.掌握 JavaGUI 界面设计、2.掌握鼠标事件的监听（MouseListener，MouseMotionListener）


### 框架
**SSH**：Structs + Spring + Hibernate
**SSM**：Spring + SpringMVC + MyBatis（先学习这种）


### Spring
Spring是一个基于 IOC 和 AOP 结构的 J2EE 系统框架。
**pojo**(Plain Old Java Object) 简单的Java对象


** IOC 是反转控制 (Inversion Of Control) **
Spring 框架是一个开源的 Java 平台，它为容易而快速的开发出耐用的 Java 应用程序提供了全面的基础设施。
**传统的方式**：通过 new  关键字主动创建一个对象
**IOC 方式**：对象的生命周期由 Spring 来管理，直接从 Spring 那里去获取一个对象，就像控制权从本来在自己手里，交给了 Spring。在主程序调用 xml 文件时，就开始对实例进行初始化构造了，不管是直接写的 bean 还是用注解方式（自动生成一个bean），效果都一样。
**IoC的实现原理**：就是工厂模式加反射机制。

**Spring 注解方式**
@Autowired：默认按类装配
@Resource(name="one")：默认先按名称，找不到按类
@Resource注解属性名表示按照属性名来查找类，找到匹配的类后，自动创建一个bean来存放对象，并注入属性，找不到或者找到多个，都会抛出异常。
@Autowired时先按照出行的类型进行查找类，如果有多个再找属性名，属性名还是有多个就报错。
@Component：对整个 Bean 进行注解。

**AOP 即 Aspect Oriented Program 面向切面编程**
首先，在面向切面编程的思想里面，把功能分为核心业务功能和周边功能。
所谓的核心业务：比如登陆，增加数据，删除数据都叫核心业务
所谓的周边功能（切面）：比如性能统计，日志，事务管理等等
在面向切面编程 AOP 的思想里面，核心业务功能和切面功能分别独立进行开发，然后把切面功能和核心业务功能 "编织" 在一起（在xml文件中编织），这就叫 AOP。在编写切面功能时，有一个核心功能接口 joinpoin（可以调用核心功能），默认在切面方法中将所有功能完成后（如在执行核心功能前后打印开始结束日志），再在 xml 中进行编织（就我理解，这里感觉只是创建两个bean，然后调用，真正的编织感觉在编写切面功能的时候已经做了，xml中负责调用）。

**Spring Scope（作用域）的范围**
![springScope作用域](../../pics/springScope作用域.jpg)

**问：AOP技术优势在于？**
答：将核心关注点与横切关注点完全隔离，面相切面编程，与传统oop相比，传统oop编程是自顶向下的编写主业务逻辑，但往往需要参杂着一些与主业务逻辑无关或关系不大的逻辑，这就产生了横切性问题。Aop能很好的隔离和管理这些与主业务逻辑关联不大的业务代码，使得代码的可读性和可维护性大大提高。

**问：在spring中，singleton属性默认是false，每次指定别名取得的Bean时都会产生一个新的实例？**
答：Bean的创建时会提到Spring的单例模式，就是说默认情况下Spring中定义的Bean是以单例模式创建的。如果以前了解设计模式中的单例模式的话很容易对这种说法产生先入为主的印象。事实上，Spring中的单例模式还有许多需要注意的地方。在GoF中的单例模式是指一个ClassLoader中只存在类一个实例。而在Spring中的单例实际上更确切的说应该是：
1.每个Spring Container中定义的Bean只存在一个实例
2.每个Bean定义只存在一个实例。

```xml
<!-- spring 反转控制，在xml中产生实例，在主函数中通过 getBean("c") 获取这个实例。 -->
<bean name="c" class="package.Category">
	<property name="name" value="CategoryOne" />
</bean>

<!-- spring 注入对象（DI）,将上一个实例注入到另一个实例中 -->
<bean name="p" class="package.Product">
	<property name="name" value="ProductOne" />
	<property name="category" ref="c" />
</bean>

<!-- spring 使用注解的方式注入对象 -->
<!-- 在主配置文件 .xml 中 bean 的前面添加 <content:annotation-config/>  -->
<!-- 在 product 类中的 Category 属性上面添加 @AutoWired 会自动匹配bean，
或者 set 方法上面，会自动调用该方法进行匹配。
而在属性上添加 @Resource(name="c") 则是主动的方式，更容易理解。
-->

<!--   -->

<!-- 更进一步，将 bean 对象本身也通过注解，重要，因为很方便，后面项目经常用到 -->
<!-- 在主配置文件 .xml 中删除所有的 bean，添加 <content:component-scan base-package="spring"/>  -->
<!-- 在Product类上添加 @Component("p") 即表明此类是 bean,需要注入对象的属性上面添加@AutoWired-->
<!-- 同时属性要在类中初始化了 -->


<!-- 将核心业务功能与切面功能整合：
核心业务类编写照常，切面类需要通过一个核心业务类接口，提前将需要做的任务完成，如在核心业务类前后打印日志等。
接下来在xml中通过两个类构造两个bean，xml中的调用肯定是通过bean来实现的，只要调用核心类中任意的方法，就会触发切面类的bean，具体看以下实现。
-->
<!-- 切面类实例化一个bean -->
<bean id="loggerAspect" class="spring.LoggerAspect"/>

<!-- 具体的编织过程 -->
<aop:config>
<!-- 核心编程的id，自定义的核心bean，调用任意此类的方法就调用切面类 -->
<aop:pointcut id="loggerCutpoint" 
expression = "execution(* spring.ProductService.*(..)) "/>

<!-- 切面编程的id，绑定的切面bean -->
<aop:aspect id="logAspect" ref="loggerAspect">
<!-- 只要触发了核心bean，那么就调用 log 方法 -->
<aop:around pointcut-ref="loggerCutpoint" method="log"/>
</aop:aspect>
</aop:config>   

<!-- 其实了解了上述构造过程，可以看到，这些只是重复了切面类的功能，用bean来做，
所以我们可以使用注解的方式，重要，因为较为方便快捷。
1在xml文件中添加两句
<context:component-scan base-package="spring"/>
<aop:aspectj-autoproxy/> 
2在切面类添加 @Aspect,@Component,@Around即可
-->
```

### SpringMVC
入口是Servlet。
springmvc的请求流程，我们以一次用户的数据查询为例: 1，用户通过浏览器发送http请求，web容器接收到相关请求调用springmvc的核心控制器DispatcherServlet ，任何路径的映射都是如此；2，DispatcherServlet请求处理器映射器（HandlerMapping），处理器映射器根据 路径 对应的 配置或注解，找到最终要执行的Handler，并返回处理器执行链（HandlerExecutionChain）给DispatcherServlet。 3，DispatcherServlet接收到处理器执行链后请求处理器适配器,（HandlerAdapter）处理器适配器根据Handler规则执行不同的Handler，即我们编写的Controller，执行完成后返回一个ModelAndView对象给DispatcherServlet 5，DispatcherServlet接收数据并调用视图解析器（ViewResolver），视图解析器将逻辑视图解析成真正的物理视图，并返回View对象 6，DispatcherServlet接收到对应的View对象,对视图进行渲染，将model中的数据转为response响应。 7，DispatcherServlet响应用户的请求。
```java
//使用 Bean 跳转，将模型与视图整合
public class IndexController implements Controller {
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
//如果要进行客户端跳转，则修改为：ModelAndView("redirect:/index.jsp");
        ModelAndView mav = new ModelAndView("index.jsp");
        mav.addObject("message", "Hello Spring MVC");
        return mav;
    }
}

/*
使用注解方式：重点，因为很简洁，用的也很多。

*/
package springmvc;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;
 
@Controller
public class IndexController {
    @RequestMapping("/index")
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView mav = new ModelAndView("index");
        mav.addObject("message", "Hello Spring MVC");
        return mav;
    }
 
    @RequestMapping("/jump")//客户端跳转
    public ModelAndView jump() {
        ModelAndView mav = new ModelAndView("redirect:/index");
        return mav;
    }
 
    @RequestMapping("/check")//session的访问次数
    public ModelAndView check(HttpSession session) {
        Integer i = (Integer) session.getAttribute("count");
        if (i == null)
            i = 0;
        i++;
        session.setAttribute("count", i);
        ModelAndView mav = new ModelAndView("check");
        return mav;
    }
 
}
```




### MyBatis
**基础**
平时我们都用JDBC访问数据库，除了需要自己写SQL之外，还必须操作Connection, Statement, ResultSet 这些其实只是手段的辅助类。 不仅如此，访问不同的表，还会写很多雷同的代码，显得繁琐和枯燥。那么用了Mybatis之后，只需要自己提供SQL语句，其他的工作，诸如建立连接，Statement， JDBC相关异常处理等等都交给Mybatis去做了，那些重复性的工作Mybatis也给做掉了，我们只需要关注在增删改查等操作层面上，而把技术细节都封装在了我们看不见的地方。
```xml
<!-- 配置mybatis-config.xml 连接数据库  -->

<typeAliases> <!-- 配置前缀  -->
   <package name="com.how2java.pojo"/>
</typeAliases>
 <!-- 连接数据库  -->
<property name="driver" value="com.mysql.jdbc.Driver"/>
<property name="url" value="jdbc:mysql://localhost:3306/school?characterEncoding=UTF-8"/>
<property name="username" value="root"/>
<property name="password" value="admin"/>
 <!-- 映射到执行文件  -->
<mappers>
	<mapper resource="mybatis/Category.xml"/>
</mappers>
</configuration>
<!-- 配置 Category.xml  执行sql语句  -->
<mapper namespace="mybatis">
<insert id="addCategory" parameterType="Category" >
insert into category_ ( name ) values (#{name})   
</insert>

<delete id="deleteCategory" parameterType="Category" >
delete from category_ where id= #{id}  
</delete>

<update id="updateCategory" parameterType="Category" >
update category_ set name=#{name} where id=#{id}   
</update>

<select id="getCategory" parameterType="_int" resultType="Category">
select * from   category_  where id= #{id}   
</select>

<select id="listCategory" resultType="Category">
select * from   category_     
</select>    
</mapper>
```


**动态 SQL **
if 标签，可以对 SQL 语句进行控制。
where 标签，可以自动删除多余的 and 和 or。

```xml
<mapper namespace="mybatis">
    <select id="One" resultType="Product">
    select * from product
    <if test="name != null">
    where name like concat('%',#{name},'%')
    </if>        
    </select>

    <select id="Two" resultType="Product">
    select * from product
    <where>
    <if test="name!=null">
    and name like concat('%',#{name},'%')
    </if>        
    <if test="price!=null and price!=0">
    and price > #{price}
    </if>
    </where>     
    </select>
    
    <update id="updateProduct" parameterType="Product" >
        update product
        <set>
            <if test="name != null">name=#{name},</if>
            <if test="price != null">price=#{price}</if>
        </set>
         where id=#{id}   
    </update>
</mapper>
```

**注解 **
将原来配置在 XML 中的数据库命令，转移到新建的接口类文件中，并以装饰器的方式，插入 sql 语句，方法名就是原来 xml 指令的 id，参数就是原来的参数。


### 错误集锦
**错误提示：java.lang.NoClassDefFoundError？**
解决：搜索了一下，意思是某个类明明有，但是运行的时候找不到了，即这个类不在 classpath 中，一直不知道这个 classpath 在哪里，真是眼瞎，在导入外部包的时候啊！！有两个选项，Moudlepath、Classpath，我以为都一样，随便点了第一个！，不要这么随意好么？？

### SSM整合
**Servlet**: WEB-INF/web.xml  通过这个配置文件将浏览器中的 路径 映射到 类，servlet.java类会根据浏览器的不同请求执行 doGet doPost方法。浏览器访问
功能：接受浏览器的访问提供映射。

**Spring**:src/applicationContext.xml [Spring 的核心配置文件]通过构建不同的 bean 来提供实例，
注解方式：<context:annotation-config/> @Autowired @Resource(name="c")    .java文件测试
功能：主要做 IOC 反转控制，通过注解自动生成 bean。

**SpringMVC**: WEB-INF/web.xml 将浏览器中所有的请求 路径 映射指定的 Servlet 和对应的 mvc 配置文件 WEB-INF/springMVC-servlet.xml 。该文件将浏览器不同的 路径 映射到 不同的Controller
注解方式：<context:component-scan base-package="package">	 @Controller @RequestMapping("/index")

**Mybatis**: src/mybatis-config.xml 连接数据的配置信息并映射到配置SQL语句的文件，
src/Category.xml 封装SQL语句（DAO）

**ssm**：

1. 首先浏览器上访问路径 /listCategory
2. tomcat根据web.xml上的配置信息，拦截到了/listCategory，并将其交由DispatcherServlet处理。
3. DispatcherServlet 根据springMVC的配置，将这次请求交由CategoryController类进行处理，所以需要进行这个类的实例化
4. 在实例化CategoryController的时候，注入CategoryServiceImpl。 (自动装配实现了CategoryService接口的的实例，只有CategoryServiceImpl实现了CategoryService接口，所以就会注入CategoryServiceImpl)
5. 在实例化CategoryServiceImpl的时候，又注入CategoryMapper
6. 根据ApplicationContext.xml中的配置信息，将CategoryMapper和Category.xml关联起来了。
7. 这样拿到了实例化好了的CategoryController,并调用 list 方法
8. 在list方法中，访问CategoryService,并获取数据，并把数据放在"cs"上，接着服务端跳转到listCategory.jsp去
9. 最后在listCategory.jsp 中显示数据

比较难理解的三个文件，三个配置文件：web.xml、applicationContext.xml、springMVC.xml 。

WEB-INF/web.xml：两个作用
1. 通过ContextLoaderListener在web app启动的时候，获取contextConfigLocation配置文件的文件名applicationContext.xml，并进行Spring相关初始化工作。
2. 有任何访问，都被DispatcherServlet所拦截，这就是Spring MVC那套工作机制了。
1映射到applicationContext.xml，2将浏览器中所有的请求 路径 映射指定的 Servlet 和对于的 mvc 配置文件 src/springMVC.xml 。

src/springMVC.xml：主要任务获取数据然后和jsp页面整合返回。
1. 扫描Controller,并将其生命周期纳入Spring管理
2. 注解驱动，以使得访问路径与方法的匹配可以通过注解配置
3. 静态页面，如html,css,js,images可以访问
4. 视图定位到/WEB/INF/jsp 这个目录下

src/applicationContext.xml：主要任务连接数据库，从数据库获取数据。
构造一个<bean>连接数据库，并注入到另一个<bean>，连接Category.java类构造ORM，连接Category.xml让其定义的SQL语句方法可以调用数据库。

大致配置过程是从下到上的三个过程，从mybatis数据库，到SpringMVC，到Spring。使用作者的源文件，修改数据库部分即可。为什么我的项目不行？我唯一的修改即是将包都去掉，整合在一个包下，可能在注解的时候会出现问题吧。