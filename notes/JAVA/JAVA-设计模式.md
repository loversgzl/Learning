# JAVA-设计模式

**简介**
设计模式（Design pattern）代表了最佳的实践，通常被有经验的面向对象的软件开发人员所采用。设计模式是软件开发人员在软件开发过程中面临的一般问题的解决方案。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。使用设计模式是为了重用代码、让代码更容易被他人理解、保证代码可靠性。以下代码示例均引用 [ W3C 设计模式 ](https://www.w3cschool.cn/shejimoshi/design-pattern-tutorial.html)

* 对接口编程而不是对实现编程。
* 优先使用对象组合而不是继承。
* 开发人员的共同平台：设计模式提供了一个标准的术语系统，当你说单例模式时，别人都能理解使用了单个对象。

**24 种设计模式**
三大类：创建型模式（Creational Patterns）、结构型模式（Structural Patterns）、行为型模式（Behavioral Patterns）。

### 一、创建型模式
这些设计模式提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是使用新的运算符直接实例化对象。这使得程序在判断针对某个给定实例需要创建哪些对象时更加灵活。

1. <a href="#工厂模式">工厂模式（Factory Pattern）</a>：定义了一个创建对象的接口，但由子类决定要实例化的类是哪一个，工厂方法让类把实例化推迟到子类。即一个工厂类，管理其余的所有子类，要想实例化子类，需通过工厂类的方法。

2. <a href="#抽象工厂模式">抽象工厂模式（Abstract Factory Pattern）</a>：可以简单理解为如果有多个工厂模式，那么需要用抽象工厂来管理。

3. <a href="#单例模式">单例模式（Singleton Pattern）</a>： 确保一个类只有一个实例，可以直接访问，不需要实例化该类的对象。

4. <a href="#建造者模式">建造者模式（Builder Pattern）</a>：使用多个简单的对象一步一步构建成一个复杂的对象（有点复杂）。

5. <a href="#原型模式">原型模式（Prototype Pattern）</a>：当需要创建重复的对象时，为了保证性能和效率，它提供了一种创建对象的最佳方式。这种模式是实现了一个原型接口，该接口用于创建当前对象的克隆。当直接创建对象的代价比较大时，则采用这种模式。例如，一个对象需要在一个高代价的数据库操作之后被创建。我们可以缓存该对象，在下一个请求时返回它的克隆，在需要的时候更新数据库，以此来减少数据库调用。

6. <a href="#生成器模式">生成器模式（Builder pattern）</a>： 使用生成器模式封装一个产品的构造过程, 并允许按步骤构造. 将一个复杂对象的构建与它的表示分离, 使得同样的构建过程可以创建不同的表示。

7. <a href="#简单工场模式">简单工场模式</a>

8. <a href="#多例模式">多例模式（Multition pattern）</a>： 在一个解决方案中结合两个或多个模式，以解决一般或重复发生的问题。


### 二、结构型模式
这些设计模式关注类和对象的组合。继承的概念被用来组合接口和定义组合对象获得新功能的方式。

1、<a href="#适配器模式">适配器模式（Adapter Pattern）</a>：将一个类的接口, 转换成客户期望的另一个接口. 适配器让原本接口不兼容的类可以合作无间. 对象适配器使用组合, 类适配器使用多重继承.

2、<a href="#桥接模式">桥接模式（Bridge Pattern）</a>： 使用桥接模式通过将实现和抽象放在两个不同的类层次中而使它们可以独立改变.

3、<a href="#组合模式">组合模式（Composite Pattern）</a>：允许你将对象组合成树形结构来表现”整体/部分”层次结构. 组合能让客户以一致的方式处理个别对象以及对象组合.

4、<a href="#装饰器模式">装饰器模式（Decorator Pattern）</a>：动态地将责任附加到对象上, 若要扩展功能, 装饰者提供了比继承更有弹性的替代方案.

5、<a href="#外观模式">外观模式（Facade Pattern）</a>：提供了一个统一的接口, 用来访问子系统中的一群接口. 外观定义了一个高层接口, 让子系统更容易使用.

6、<a href="#享元模式">享元模式（Flyweight Pattern）</a>：如想让某个类的一个实例能用来提供许多”虚拟实例”, 就使用蝇量模式.

7、<a href="#代理模式">代理模式（Proxy Pattern）</a>：为另一个对象提供一个替身或占位符以控制对这个对象的访问.

### 三、行为型模式
这些设计模式特别关注对象之间的通信。

1、<a href="#责任链模式">责任链模式（Chain of Responsibility Pattern）</a>： 通过责任链模式, 你可以为某个请求创建一个对象链. 每个对象依序检查此请求并对其进行处理或者将它传给链中的下一个对象.

2、<a href="#命令模式">命令模式（Command Pattern）</a>： 将”请求”封闭成对象, 以便使用不同的请求,队列或者日志来参数化其他对象. 命令模式也支持可撤销的操作.

3、<a href="#解释器模式">解释器模式（Interpreter Pattern）</a>：使用解释器模式为语言创建解释器.

4、<a href="#迭代器模式">迭代器模式（Iterator Pattern）</a>：提供一种方法顺序访问一个聚合对象中的各个元素, 而又不暴露其内部的表示.

5、<a href="#中介者模式">中介者模式（Mediator Pattern）</a>：使用中介者模式来集中相关对象之间复杂的沟通和控制方式.

6、<a href="#备忘录模式">备忘录模式（Memento Pattern）</a>：当你需要让对象返回之前的状态时(例如, 你的用户请求”撤销”), 你使用备忘录模式.？？

7、<a href="#观察者模式">观察者模式（Observer Pattern）</a>： 在对象之间定义一对多的依赖, 这样一来, 当一个对象改变状态, 依赖它的对象都会收到通知, 并自动更新.

8、<a href="#状态模式">状态模式（State Pattern）</a>：允许对象在内部状态改变时改变它的行为, 对象看起来好象改了它的类.

9、<a href="#策略模式">策略模式（Strategy Pattern）</a>：定义了算法族, 分别封闭起来, 让它们之间可以互相替换, 此模式让算法的变化独立于使用算法的客户.

10、<a href="#模板模式">模板模式（Template Pattern）</a>： 在一个方法中定义一个算法的骨架, 而将一些步骤延迟到子类中. 模板方法使得子类可以在不改变算法结构的情况下, 重新定义算法中的某些步骤.

11、<a href="#访问者模式">访问者模式（Visitor Pattern）</a>：当你想要为一个对象的组合增加新的能力, 且封装并不重要时, 就使用访问者模式.

* 学习的过程中可能会遇到 UML 类图，所以还是有点必要学习一下的。[参考 W3C UML 类图](https://www.w3cschool.cn/uml_tutorial/uml_tutorial-5y1i28pl.html)

****

**七大设计原则**：
1、单一职责原则【Single Responsibility Principle】：一个类负责一项职责

.2、里氏替换原则【Liskov Substitution Prnciple】：继承与派生的规则.（子类可替换父类）

3、依赖倒转原则【Dependence Inversion Principle】：高层模块不应该依赖低层模块，二者都应该依赖其抽象；抽象不应该依赖细节；细节应该依赖抽象。即针对接口编程，不要针对实现编程。

4、接口隔离原则【Interface Segregation Principle】：建立单一接口，不要建立庞大臃肿的接口，尽量细化接口，接口中的方法尽量少。

5、迪米特法则【Low Of Demeter】：高内聚 低耦合 – high cohesion low coupling

6、开闭原则【Open Close Principle】：对扩展开放，对修改关闭。一个软件实体如类、模块和函数

7、组合/聚合复用原则【Composition/Aggregation Reuse Principle(CARP) 】：尽量使用组合和聚合少使用继承的关系来达到复用的原则。

**设计模式的六大原则**

1、开闭原则（Open Close Principle）
开闭原则的意思是：对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简言之，是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类，后面的具体设计中我们会提到这点。

2、里氏代换原则（Liskov Substitution Principle）
里氏代换原则是面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。LSP 是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。

3、依赖倒转原则（Dependence Inversion Principle）
这个原则是开闭原则的基础，具体内容：针对对接口编程，依赖于抽象而不依赖于具体。

4、接口隔离原则（Interface Segregation Principle）
这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。

5、迪米特法则，又称最少知道原则（Demeter Principle）
最少知道原则是指：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。

6、合成复用原则（Composite Reuse Principle）
合成复用原则是指：尽量使用合成/聚合的方式，而不是使用继承。
****

* <a name="工厂模式">工厂模式（Factory Pattern）</a>：定义了一个创建对象的接口，但由子类决定要实例化的类是哪一个，工厂方法让类把实例化推迟到子类。即一个工厂类，管理其余的所有子类，要想实例化子类，需通过工厂类的方法。

```java
//工厂模式
public interface Shape{//定义一个接口
	public void draw();
}

class Circle implements Shape{
    @Override //防止下面的函数写错，确定这是重写的函数
	public void draw(){
		System.out.println("Circle method()!");
	}
}

class Square implements Shape{
    @Override
	public void draw(){
		System.out.println("Square method()!");
	}
}

class Rectangle implements Shape{
    @Override
	public void draw(){
		System.out.println("Rectangle method()!");
	}
}

class ShapeFactory{ //工厂用于管理形状的生产
	public Shape getShape(String shape){
		if(shape == "CIRCLE"){
			return new Circle();
		}else if(shape == "SQUARE"){
			return new Square();
		}else if(shape == "RECTANGLE"){
			return new Rectangle();
		}
		return null;
	}
}

public class test{
	public static void main(String args[]){
		ShapeFactory factory = new ShapeFactory();//生成一个图形工厂，开始生成形状
		Shape shape1 = factory.getShape("CIRCLE");
		Shape shape2 = factory.getShape("SQUARE");
		Shape shape3 = factory.getShape("RECTANGLE");
		shape1.draw();
		shape2.draw();
		shape3.draw();
	}
}
```

 <a name="抽象工厂模式">抽象工厂模式（Abstract Factory Pattern）</a>：可以简单理解为管理多个工厂的制造。
```java
//抽象工厂模式
interface Shape{
	public void draw();
}
interface Color{
	public void fill();
}

//形状可以有多个，这里只用了 Circle 测试一下
class Circle implements Shape{
	@Override
	public void draw(){
		System.out.println("Circle method()!");
	}
}
//颜色可以有多个，这里只用了 Red 测试一下
class Red implements Color{
	@Override
	public void fill(){
		System.out.println("this shape filled with red!");
	}
}

abstract class AbstractFactory{ //管理所有的 工厂模式 里工厂的所有方法
	//如果是抽象方法，需要加 abstract 关键字，接口可不写
	public abstract Shape getShape(String shape); 
	public abstract Color getColor(String color);
}

class ShapeFactory extends AbstractFactory{//不同的工厂只实现自己的方法
	@Override
	public Shape getShape(String shape){
		if(shape == "CIRCLE"){
			return new Circle();
		}
		return null;
	}
	@Override
	public Color getColor(String color){
		return null;
	}
}

class ColorFactory extends AbstractFactory{//不同的工厂只实现自己的方法
	@Override
	public Shape getShape(String shape){
		return null;
	}
	@Override
	public Color getColor(String color){
		if(color == "RED"){
			return new Red();
		}
		return null;
	}
}

class FactoryProducer{//工厂制造者，为不同的人建造不同的工厂
	public AbstractFactory getFactory(String factory){
		if(factory == "shape"){
			return new ShapeFactory();
		}else if(factory == "color"){
			return new ColorFactory();
		}
		return null;
	}
}

public class test{
	public static void main(String args[]){
		FactoryProducer producer = new FactoryProducer();
		AbstractFactory factory1 = producer.getFactory("shape");
		AbstractFactory factory2 = producer.getFactory("color");
		Shape shape1 = factory1.getShape("CIRCLE");
		Color color1 = factory2.getColor("RED");
		shape1.draw();
		color1.fill();
	}
}
```

<a name="单例模式">单例模式（Singleton Pattern）</a>：单例类只能有一个实例、单例类必须自己创建自己的唯一实例、单例类必须给所有其他对象提供这一实例。

```java
//单例模式，普通的类就可以调用内部的静态函数。
class SingleObject{
	private static SingleObject instance = new SingleObject();
    //构造器私有化
	private SingleObject(){}
	public static SingleObject getInstance(){
		return instance;
	}
	public void showMessage(){
		System.out.println("普通函数");
	}
}

public class test{
	public static void main(String args[]){
	//唯一的实例，不用创建，直接用。是不是想知道，那么这个实例啥时候创建的呢？
	SingleObject single = SingleObject.getInstance();
	single.showMessage();
	}
}
/*
因为有上面的问题，所以单例模式有多种，其中我写的这种叫做 饿汉式，instance 在类装载时就实例化。
还有 private static Singleton instance; 这种叫 懒汉式，但是在多线程下不能正常工作，其他形式请参考 W3C
*/

```

<a name="建造者模式">建造者模式（Builder Pattern）</a>：使用多个简单的对象一步一步构建成一个复杂的对象。

```java
//建造者模式
import java.util.ArrayList;
import java.util.List;

interface Item{ //有了接口关键字，就不需要类了
	public String name();
	public Packing packing();//返回一个接口类型而已，继承了这个接口的都可以
	public int price();
}
interface Packing{
	public String pack();
}

class Wrapper implements Packing{
	@Override
	public String pack(){
		return "Wrapper";
	}
}
class Bottle implements Packing{
	@Override
	public String pack(){
		return "Bottle";
	}
}

//抽象的类可以实现接口的部分功能，抽象 类 abstract 关键字在前
abstract class Burger implements Item{
	@Override
	public Packing packing(){
		return new Wrapper();
	}
}
abstract class ColdDrink implements Item{
	@Override
	public Packing packing(){
		return new Bottle();
	}
}

//抽象类中已经实现的类，不用重复实现，比如打包盒
class VegBurger extends Burger{
	@Override
	public String name(){
		return "VegBurger";
	}
	@Override
	public int price(){
		return 25;
	}
}
class Pepsi extends ColdDrink{
	@Override
	public String name(){
		return "Pepsi";
	}
	@Override
	public int price(){
		return 8;
	}
}

class Meal{
	private ArrayList<Item> items = new ArrayList<Item>();
	public void addItem(Item item){
		items.add(item);
	}
	public int getCost(){
		int cost = 0;
		for(Item item : items){
			cost += item.price();
		}
		return cost;
	}
	public void showItems(){
		for(Item item : items){
         System.out.print("Item : "+item.name());
         System.out.print(", Packing : "+item.packing().pack());
         System.out.println(", Price : "+item.price());
		}
	}
}
class MealBuilder{
	public Meal prepareVegMeal(){//这是素食，还可以构造荤食类
		Meal meal = new Meal();
		meal.addItem(new VegBurger());
		meal.addItem(new Pepsi());
		return meal;
	}
}

public class test{
	public static void main(String args[]){
		MealBuilder mealBuilder = new MealBuilder();
		Meal vegMeal = mealBuilder.prepareVegMeal();
		vegMeal.showItems();
		System.out.println("Total Cost: " +vegMeal.getCost());
	}
}
```
<a name="原型模式">原型模式（Prototype Pattern）</a>：当需要创建重复的对象时，为了保证性能和效率，它提供了一种创建对象的最佳方式。

```java
import java.util.Hashtable;

abstract class Shape implements Cloneable{
	private String id;
	protected String type;

	abstract void draw();

	public String getType(){
		return type;
	}
	public String getId(){
		return id;
	}
	public void setId(String id){
		this.id = id;
	}
	public Object clone(){
		Object clone = null;
		try{
			clone = super.clone();
		}catch(CloneNotSupportedException e){
			e.printStackTrace();
		}
		return clone;
	}
}

class Rectangle extends Shape{
	public Rectangle(){
		type = "Rectangle";
	}
	@Override
	public void draw(){
		System.out.println("Rectangle method!");
	}
}
class Circle extends Shape{
	public Circle(){
		type = "Circle";
	}
	@Override
	public void draw(){
		System.out.println("Circle method!");
	}
}

class ShapeCache{//保存所有创建的实例，需要时克隆一个返回。
	private static Hashtable<String, Shape> shapeMap = new Hashtable<String, Shape>();
	public static Shape getShape(String shapeId){
		Shape cachedShape = shapeMap.get(shapeId);
		return (Shape) cachedShape.clone();
	}

	public static void loadCache(){
		Circle circle = new Circle();
		circle.setId("1");
		shapeMap.put(circle.getId(), circle);

		Rectangle rectangle = new Rectangle();
		rectangle.setId("2");
		shapeMap.put(rectangle.getId(), rectangle);
	}
}

public class test{
	public static void main(String args[]){
		ShapeCache.loadCache();//先存两个图形到 map 中

        //从 map 中克隆一个实体返回
		Shape clonedShape1 = (Shape) ShapeCache.getShape("1");
		System.out.println("Shape : " + clonedShape1.getType());

		Shape clonedShape2 = (Shape) ShapeCache.getShape("2");
		System.out.println("Shape : " + clonedShape2.getType());
	}
}
```

### 二、结构型模式
1、<a name="适配器模式">适配器模式（Adapter Pattern）</a>：将一个类的接口，转换成客户期望的另一个接口。适配器让原本接口不兼容的类可以合作无间。 对象适配器使用组合，类适配器使用多重继承。

```java
interface MediaPlayer{
	public void play(String audioType, String filename);
}
interface AdvancedMediaPlayer{
	public void playVlc(String filename);
	public void playMp4(String filename);
}

class VlcPlayer implements AdvancedMediaPlayer {
	@Override
	public void playVlc(String filename){
		System.out.println("Vic play music " + filename);
	}	
	@Override
	public void playMp4(String filename){}
}
class Mp4Play implements AdvancedMediaPlayer {
	@Override
	public void playVlc(String filename){}	
	@Override
	public void playMp4(String filename){
		System.out.println("Mp4 play music " + filename);
	}
}

class MediaAdapter implements MediaPlayer{
	private AdvancedMediaPlayer advancedMediaPlayer;
	public MediaAdapter(String typename){//构造函数初始化一个适配器，方便播放
		if(typename == "vlc"){
			advancedMediaPlayer = new VlcPlayer();
		}else if (typename == "mp4"){
			advancedMediaPlayer = new Mp4Play();
		}else{
			advancedMediaPlayer = null;
		}
	}
	//根据 CD 类型播放音乐，如果类型和适配器不符，有可能播放不成功，
	public void play(String audioType, String filename){
		if(audioType == "vlc"){
			advancedMediaPlayer.playVlc(filename);
		}else if(audioType == "mp4"){
			advancedMediaPlayer.playMp4(filename);
		}
	}
}

class AudioPlayer implements MediaPlayer{
	private MediaAdapter mediaAdapter;
	public void play(String audioType, String filename){
		if(audioType == "mp3"){
			System.out.println("mp3 play music " + filename);
		}else if(audioType == "mp4" || audioType == "vlc"){
			mediaAdapter = new MediaAdapter(audioType);
			mediaAdapter.play(audioType, filename);
		}else{
			System.out.println("play error, no such type.");
		}
	}
}

public class test{
	public static void main(String args[]){
		AudioPlayer audioPlayer = new AudioPlayer();
		audioPlayer.play("mp3","盗将行");
		audioPlayer.play("vlc","遇见");
		audioPlayer.play("mp4","花粥");
		audioPlayer.play("zip","aaa");
	}
}

```

2、<a name="桥接模式">桥接模式（Bridge Pattern）</a>： 使用桥接模式通过将实现和抽象放在两个不同的类层次中而使它们可以独立改变.

```java
interface DrawAPI{
	public void drawCircle(int radius, int x, int y);
}

class RedCircle implements DrawAPI{
	@Override
	public void drawCircle(int radius, int x, int y){
		System.out.println(String.format("draw circle color is red. x:%d,y:%d,radius:%d",x,y,radius));
	}
}

class GreenCircle implements DrawAPI{
	@Override
	public void drawCircle(int radius, int x, int y){
		System.out.println(String.format("draw circle color is green. x:%d,y:%d,radius:%d",x,y,radius));
	}
}

abstract class Shape{
	protected DrawAPI drawAPI;
	protected Shape(DrawAPI drawAPI){
		this.drawAPI = drawAPI;
	}
	public abstract void draw();//方法是抽象的需要申明
}

class Circle extends Shape{
	private int x,y,radius;
	public Circle(int x, int y, int radius, DrawAPI drawAPI){
		super(drawAPI);
		this.x = x;
		this.y = y;
		this.radius = radius;
	}
	@Override
	public void draw(){
		drawAPI.drawCircle(radius,x,y);
	}
}

public class test{
	public static void main(String args[]){
		Shape shape1 = new Circle(1,2,3,new RedCircle());
		Shape shape2 = new Circle(4,5,6,new GreenCircle());
		shape1.draw();
		shape2.draw();
	}
}
```

3、<a name="过滤器模式">过滤器模式（Composite Pattern）</a>：允许你将对象组合成树形结构来表现”整体/部分”层次结构. 组合能让客户以一致的方式处理个别对象以及对象组合.

```java
 import java.util.List;
import java.util.ArrayList;

/*
设计模式是真的强大，写出来你可能没感觉，但是当你写错的时候，或者要修改的时候，你就会发现，
节省我们大量大量的时间，而且原本框架几乎不动，比如 Person 类里的字段我想改变，其他地方几乎不影响。
*/
class Person{
	private String name;
	private String gender;
	private String state;
	public Person(String name, String gender, String state){
		this.name = name;
		this.gender = gender;
		this.state = state;
	}
	public String getName(){return name;}
	public String getGender(){return gender;}
	public String getState(){return state;}
}

interface Criteria{
	public List<Person> meetCriteria(List<Person> persons);
}

class CriteriaMale implements Criteria{
	@Override
	public List<Person> meetCriteria(List<Person> persons){
		List<Person> items = new ArrayList<Person>();
		for(Person person : persons){
			if(person.getGender() == "Male"){
				items.add(person);
			}
		}
		return items;
	}
}

class CriteriaSingle implements Criteria{
	@Override
	public List<Person> meetCriteria(List<Person> persons){
		List<Person> items = new ArrayList<Person>();
		for(Person person : persons){
			if(person.getState() == "Single"){
				items.add(person);
			}
		}
		return items;
	}
}

class OrCriteria implements Criteria{
	private Criteria firstCriteria, secondCriteria;

	public OrCriteria(Criteria firstCriteria, Criteria secondCriteria){
		this.firstCriteria = firstCriteria;
		this.secondCriteria = secondCriteria;
	}

	@Override
	public List<Person> meetCriteria(List<Person> persons){
		List<Person> firstItems = firstCriteria.meetCriteria(persons);
		List<Person> secondItems = secondCriteria.meetCriteria(persons);
		for(Person person : firstItems){
			if(!secondItems.contains(person)){
				secondItems.add(person);
			}
		}
		return secondItems;
	}
}

class AndCriteria implements Criteria{
	private Criteria firstCriteria, secondCriteria;
	public AndCriteria(Criteria firstCriteria, Criteria secondCriteria){
		this.firstCriteria = firstCriteria;
		this.secondCriteria = secondCriteria;
	}

	@Override
	public List<Person> meetCriteria(List<Person> persons){
		List<Person> Items = firstCriteria.meetCriteria(persons);
		return secondCriteria.meetCriteria(Items);
	}
}

public class test{
	public static void main(String args[]){
	  List<Person> persons = new ArrayList<Person>();
      persons.add(new Person("Robert","Male", "Single"));
      persons.add(new Person("John","Male", "Married"));
      persons.add(new Person("Laura","Female", "Married"));
      persons.add(new Person("Diana","Female", "Single"));
      persons.add(new Person("Mike","Male", "Single"));
      persons.add(new Person("Bobby","Male", "Single"));

      Criteria male = new CriteriaMale();
      Criteria single = new CriteriaSingle();
      Criteria singleMale = new AndCriteria(single, male);
      Criteria singleOrMale = new OrCriteria(single, male);

      System.out.println("Males: ");
      printPersons(male.meetCriteria(persons));

      System.out.println("\nSingle: ");
      printPersons(single.meetCriteria(persons));

      System.out.println("\nSingle Males: ");
      printPersons(singleMale.meetCriteria(persons));

      System.out.println("\nSingle Or Females: ");
      printPersons(singleOrMale.meetCriteria(persons));
	}

	public static void printPersons(List<Person> persons){
      for (Person person : persons) {
         System.out.println("Person : [ Name : " + person.getName() 
            +", Gender : " + person.getGender() 
            +", Marital Status : " + person.getState()
            +" ]");
      }
  }
}
```

3、<a name="组合模式">组合模式（Composite Pattern）</a>：允许你将对象组合成树形结构来表现”整体/部分”层次结构. 组合能让客户以一致的方式处理个别对象以及对象组合.
```java
import java.util.ArrayList;
import java.util.List;

public class Employee {
   private String name;
   private String dept;
   private int salary;
   private List<Employee> subordinates;

   //构造函数
   public Employee(String name,String dept, int sal) {
      this.name = name;
      this.dept = dept;
      this.salary = sal;
      subordinates = new ArrayList<Employee>();
   }

   public void add(Employee e) {//添加下属
      subordinates.add(e);
   }

   public void remove(Employee e) {
      subordinates.remove(e);
   }

   public List<Employee> getSubordinates(){
     return subordinates;
   }

   public String toString(){
      return ("Employee :[ Name : "+ name 
      +", dept : "+ dept + ", salary :"
      + salary+" ]");
   }   
}

public class CompositePatternDemo {
   public static void main(String[] args) {
      Employee CEO = new Employee("John","CEO", 30000);

      Employee headSales = new Employee("Robert","Head Sales", 20000);

      Employee headMarketing = new Employee("Michel","Head Marketing", 20000);

      Employee clerk1 = new Employee("Laura","Marketing", 10000);
      Employee clerk2 = new Employee("Bob","Marketing", 10000);

      Employee salesExecutive1 = new Employee("Richard","Sales", 10000);
      Employee salesExecutive2 = new Employee("Rob","Sales", 10000);

      CEO.add(headSales);
      CEO.add(headMarketing);

      headSales.add(salesExecutive1);
      headSales.add(salesExecutive2);

      headMarketing.add(clerk1);
      headMarketing.add(clerk2);

      //打印该组织的所有员工
      System.out.println(CEO); 
      for (Employee headEmployee : CEO.getSubordinates()) {
         System.out.println(headEmployee);
         for (Employee employee : headEmployee.getSubordinates()) {
            System.out.println(employee);
         }
      }        
   }
}
```

4、<a name="装饰器模式">装饰器模式（Decorator Pattern）</a>：动态地将责任附加到对象上, 若要扩展功能, 装饰者提供了比继承更有弹性的替代方案.

```java
//刚开始我在想，装饰器其实中间也许不用加这个抽象类的，直接继承接口就好了，
//但是也许是想将父类的方法和装饰器的方法分开，后面容易扩展考虑吧。
interface Shape{
	public void draw();
}
class Circle implements Shape{
	@Override
	public void draw(){
		System.out.println("this is a circle.");
	}
}
class Rectangle implements Shape{
	@Override
	public void draw(){
		System.out.println("this is a Rectangle.");
	}
}

abstract class ShapeDecorator implements Shape{
	protected Shape shape;
	public ShapeDecorator(Shape shape){
		this.shape = shape;
	}
	@Override
	public void draw(){
		shape.draw();
	}
}

class RedShapeDecorator extends ShapeDecorator{
	public RedShapeDecorator(Shape shape){
		super(shape);
	}

	@Override
	public void draw(){
		shape.draw();
		setRedBorder(shape);
	}
	private void setRedBorder(Shape shape){
		System.out.println("this border color is red.");
	}
}

public class test{
	public static void main(String args[]){
		Shape redCircle = new RedShapeDecorator(new Circle());
		Shape redRectangle = new RedShapeDecorator(new Rectangle());
		redCircle.draw();
		redRectangle.draw();
	}
}
```

5、<a name="外观模式">外观模式（Facade Pattern）</a>：提供了一个统一的接口, 用来访问子系统中的一群接口. 外观定义了一个高层接口, 让子系统更容易使用.

```java
//和工程模式的异同？？
```

6、<a name="享元模式">享元模式（Flyweight Pattern）</a>：如想让某个类的一个实例能用来提供许多”虚拟实例”, 就使用蝇量模式.

```java

```

7、<a name="代理模式">代理模式（Proxy Pattern）</a>：为另一个对象提供一个替身或占位符以控制对这个对象的访问.

```java

```
### 三、行为型模式
这些设计模式特别关注对象之间的通信。

1、<a name="责任链模式">责任链模式（Chain of Responsibility Pattern）</a>
原本一个类对出错等级进行判断，现在细分，不同出错等级由不同的类管理，然而这些类会串联起来，像一个类一样，即实现了原来的功能，又解耦合，易扩展。
```java
abstract class AbstractLogger{ #为不同出错等级的类提供模板
	public static int INFO = 1;
	public static int DEBUG = 2;
	public static int ERROR = 3;
	protected int level;
	private AbstractLogger nextLogger = null;

	public void setNextLogger(AbstractLogger nextLogger){
		this.nextLogger = nextLogger;
	}
	public void logMessage(int level, String message){
		if(this.level <= level){
			write(message);
		}
		if(nextLogger != null){
			nextLogger.logMessage(level, message);
		}
	}
	protected abstract void write(String message);
}

class ConsoleLogger extends AbstractLogger{#输入出错
	public ConsoleLogger(int level){
		this.level = level;
	}
	@Override
	protected void write(String message){
		System.out.println("this is a console message! " + message);
	}
}

class FileLogger extends AbstractLogger{#debug
	public FileLogger(int level){
		this.level = level;
	}
	@Override
	protected void write(String message){
		System.out.println("this is a file message! " + message);
	}
}

class ErrorLogger extends AbstractLogger{#程序错误
	public ErrorLogger(int level){
		this.level = level;
	}
	@Override
	protected void write(String message){
		System.out.println("this is a Error message! " + message);
	}
}

public class test{
	public static AbstractLogger abstractLogger(){#构造责任链
		AbstractLogger consoleLogger = new ConsoleLogger(AbstractLogger.INFO);
		AbstractLogger fileLogger = new FileLogger(AbstractLogger.DEBUG);
		AbstractLogger errorLogger = new ErrorLogger(AbstractLogger.ERROR);

		consoleLogger.setNextLogger(fileLogger);
		fileLogger.setNextLogger(errorLogger);
		return consoleLogger;
	}

	public static void main(String args[]){#测试
		AbstractLogger log = abstractLogger();
		log.logMessage(1, "this is a message!");
		log.logMessage(2, "this is a debug message!");
		log.logMessage(3, "this is a error message!");
	}
}
```

2、<a name="命令模式">命令模式（Command Pattern）</a>
通过将一个大的命令分开执行，分别封装到小的类中，执行时看不出差别。
```java
import java.util.ArrayList;
import java.util.List;

interface Order{//不同小命令需要执行的函数
	//public Stock absStock = null; 除非是静态的成员，否则不能写在接口内
	public void execute();
}

class Stock{//股票需要的操作函数
	private String name = "abcStock";
	private int quantity = 10;
	public void buy(){
		System.out.println("buy " + name + " " + quantity); //不要空格夹在 + 号中间。
	}
	public void sell(){
		System.out.println(name + " sell " + quantity);
	}
}

class BuyStock implements Order{//买股票的命令
	private Stock abcStock;
	public BuyStock(Stock abcStock){
		this.abcStock = abcStock;
	}
	@Override
	public void execute(){
		abcStock.buy();
	}
}

class SellStock implements Order{//卖股票的命令
	private Stock abcStock;
	public SellStock(Stock abcStock){
		this.abcStock = abcStock;
	}
	@Override
	public void execute(){
		abcStock.sell();
	}
}

class Broker{//中间的代理人，对所有命令进行处理
	List<Order> orders = new ArrayList<Order>();
	public void takeOrder(Order order){
		orders.add(order);
	}
	public void placeOrder(){
		for(Order order : orders){
			order.execute();
		}
		orders.clear();
	}
}

public class test{//测试
	public static void main(String args[]){
		Stock abcStock = new Stock();
		Order order1 = new BuyStock(abcStock);
		Order order2 = new SellStock(abcStock);

		Broker broker = new Broker();
		broker.takeOrder(order1);
		broker.takeOrder(order2);
		broker.placeOrder();
	}
}
```

3、<a name="解释器模式">解释器模式（Interpreter Pattern）</a>

```java
interface Expression{
	public boolean interpret(String content);
}

class TerminalExpression implements Expression{
	private String data;
	public TerminalExpression(String data){
		this.data = data;
	}
	@Override
	public boolean interpret(String content){
		if(content.contains(data)){
			return true;
		}else{
			return false;
		}
	}
}

class AndExpression implements Expression{
	private Expression expr1 = null;
	private Expression expr2 = null;

	public AndExpression(Expression expr1, Expression expr2){
		this.expr1 = expr1;
		this.expr2 = expr2;
	}

	@Override
	public boolean interpret(String content){
	//利用细分接口组合与条件
		return expr1.interpret(content) && expr2.interpret(content);
	}
}

class OrExpression implements Expression{
	private Expression expr1 = null;
	private Expression expr2 = null;

	public OrExpression(Expression expr1, Expression expr2){
		this.expr1 = expr1;
		this.expr2 = expr2;
	}

	@Override
	public boolean interpret(String content){
	//利用细分接口组合 或条件，这里需要体会一下
		return expr1.interpret(content) || expr2.interpret(content);
	}
}

public class test{
	public static Expression getMaleExpression(){
	//构造标准，符合以下条件的为男性
		Expression robert = new TerminalExpression("Robert");
		Expression john = new TerminalExpression("John");
		return new OrExpression(robert, john);
	}

	public static Expression getMarriedWomanExpression(){
	//构造标准，符合以下条件的为结婚女性
		Expression julie = new TerminalExpression("Julie");
		Expression married = new TerminalExpression("Married");
		return new AndExpression(julie, married);
	}

	public static void main(String args[]){
		Expression isMale = getMaleExpression();
		Expression isMarriedWoman = getMarriedWomanExpression();

		System.out.println("John is male? " + isMale.interpret("John"));
		System.out.println("Julie is a married women?" + isMarriedWoman.interpret("Married Julie"));
	}
}

```

4、<a href="#迭代器模式">迭代器模式（Iterator Pattern）</a>：提供一种方法顺序访问一个聚合对象中的各个元素, 而又不暴露其内部的表示.
```java
interface Iterator{
	public boolean hasNext();
	public Object next();
}

interface Container{
	public Iterator getIterator();
}

class NameRepository implements Container{
	public String name[] = {"Robert" , "John" ,"Julie" , "Lora"};

	@Override
	public Iterator getIterator(){
		return new NameIterator();
	}

	private class NameIterator implements Iterator{
	//这个类写在上面类的内部
		int index = 0;
		@Override
		public boolean hasNext(){//判断是否是最后一个
			if(index < name.length){
				return true;
			}
			return false;
		}
		@Override
		public Object next(){//返回下一个值
			if(this.hasNext()){
				return name[index++];
			}
			return null;
		}
	}
}

public class test{
	public static void main(String args[]){
		NameRepository nameRepository = new NameRepository();

		for(Iterator iterator = nameRepository.getIterator(); iterator.hasNext();){
			String name = (String)iterator.next();
			System.out.println("name: "+name);
		}
	}
}
```

5、<a href="#中介者模式">中介者模式（Mediator Pattern）</a>：使用中介者模式来集中相关对象之间复杂的沟通和控制方式.
```java
import java.util.Date;
class ChatRoom{
	public static void showMessage(User user, String message){
		System.out.println(new Date().toString() + user.getName() + message);
	}
}
class User{//使用上面创建的静态方法进行聊天
	private String name;
	public User(String name){
		this.name = name;
	}
	public String getName(){
		return name;
	}
	public void setName(String name){
		this.name = name;
	}
	public void sendMessage(String message){
		ChatRoom.showMessage(this, message);
	} 
}

public class test{
	public static void main(String args[]){
		User john = new User("john");
		User marry = new User("marry");
		john.sendMessage("hello!");
		marry.sendMessage("hello!");
	}
}
```
6、<a href="#备忘录模式">备忘录模式（Memento Pattern）</a>：当你需要让对象返回之前的状态时(例如, 你的用户请求”撤销”), 你使用备忘录模式.？？

7、<a href="#观察者模式">观察者模式（Observer Pattern）</a>： 在对象之间定义一对多的依赖, 这样一来, 当一个对象改变状态, 依赖它的对象都会收到通知, 并自动更新.
```python
import java.util.ArrayList;
import java.util.List;

class Subject{
	private List<Observer> observers = new ArrayList<Observer>();
	private int state;

	public int getState(){
		return state;
	}
	public void setState(int state){
		this.state = state;
		notifyAllObservers();
	}
	public void attach(Observer observer){
		observers.add(observer);
	}
	public void notifyAllObservers(){
		for(Observer observer : observers){
			observer.update();
		}
	}
}

abstract class Observer{
//观察者都会有一个观察的物体，并把自己加入这个物体的观察者队列中
	protected Subject subject;
	public abstract void update();
}

class BinaryObserver extends Observer{
	public BinaryObserver(Subject subject){
		this.subject = subject;
		this.subject.attach(this);
	}
	@Override
	public void update(){
		System.out.println("BinaryOperator update!" + subject.getState());
	}
}

class OctalObserver extends Observer{
	public OctalObserver(Subject subject){
		this.subject = subject;
		this.subject.attach(this);
	}
	@Override
	public void update(){
		System.out.println("OctalObserver update!" + subject.getState());
	}
}

public class test{
	public static void main(String args[]){
		Subject subject = new Subject();

		//将物体加入到不同的观察者中
		new BinaryObserver(subject);
		new OctalObserver(subject);

		//当物品的状态发生改变时
		subject.setState(15);
		subject.setState(10);
	}
}
```
8、<a href="#状态模式">状态模式（State Pattern）</a>：允许对象在内部状态改变时改变它的行为, 对象看起来好象改了它的类.
```java
interface State{
	public void doAction(Context context);
}

class StartState implements State{
	@Override
	public void doAction(Context context){
		System.out.println("this state is start!");
		context.setState(this);
	}
	public String toString(){
		return "Start state";
	}
}

class StopState implements State{
	@Override
	public void doAction(Context context){
		System.out.println("this state is stop!");
		context.setState(this);
	}
	public String toString(){
		return "Stop state";
	}
}

class Context{
	private State state;
	public Context(){
		this.state = null;
	}
	public State getState(){
		return state;
	}
	public void setState(State state){
		this.state= state;
	}
}

public class test{
	public static void main(String args[]){
		Context context = new Context();

		StartState start = new StartState();
		start.doAction(context);
		System.out.println(context.getState().toString());

		StopState stop = new StopState();
		stop.doAction(context);
		System.out.println(context.getState().toString());
	}
}
```
9、<a href="#策略模式">策略模式（Strategy Pattern）</a>：定义了算法族, 分别封闭起来, 让它们之间可以互相替换, 此模式让算法的变化独立于使用算法的客户.

10、<a href="#模板模式">模板模式（Template Pattern）</a>： 在一个方法中定义一个算法的骨架, 而将一些步骤延迟到子类中. 模板方法使得子类可以在不改变算法结构的情况下, 重新定义算法中的某些步骤.

11、<a href="#访问者模式">访问者模式（Visitor Pattern）</a>：当你想要为一个对象的组合增加新的能力, 且封装并不重要时, 就使用访问者模式.


### 经典例题
参考：https://www.jianshu.com/p/4588e2698ab3

**问：什么是设计模式？你是否在你的代码里面使用过任何设计模式？**
设计模式是软件开发人员在软件开发过程中面临的一般问题的解决方案。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。使用设计模式是为了重用代码、让代码更容易被他人理解、保证代码可靠性。
肯定要说用过，曾经遇到过？问题，用了？设计模式解决。

**问：请列举出在JDK中几个常用的设计模式？**
单例模式：用于Runtime，Calendar 和其他的一些类中。
工厂模式：被用于各种不可变的类如 Boolean，像 Boolean.valueOf。 
观察者模式：被用于 Swing 和很多的事件监听中。 
装饰器设计模式（Decorator design pattern）被用于多个 Java IO 类中。

