# CPP 基础语法

[参考W3Cschool](https://www.w3cschool.cn/cpp/cpp-files-streams.html)
**面试考点：指针、引用、运算符重载**

****
### 命名空间（namespace）

您可能会写一个名为 xyz() 的函数，在另一个可用的库中也存在一个相同的函数 xyz()。这样，编译器就无法判断您所使用的是哪一个 xyz() 函数。因此，引入了 命名空间 这个概念，专门用于解决上面的问题，它可作为附加信息来区分不同库中相同名称的函数、类、变量等。

```c++
//<iostream> 这是我们要用到的库，作者也没有很好的理解这个文件和 std 命名空间的具体关系，之后会找找书，了解一下底层构造。
#include <iostream> 
using namespace std;//像例子那样调用会很麻烦，所以直接把前缀写出来，直接用就可以了。
// using std::cout 只为一个函数声明，只有这一个函数可以直接用。
//cout << "申明部分函数的命名空间"<< std::endl;

namespace first_space{// 自定义的第一个命名空间
   void func(){
      cout << "Inside first_space" << endl;
   }
}

namespace second_space{// 自定义的第二个命名空间
   void func(){
      cout << "Inside second_space" << endl;
   }
}
int main (){
   first_space::func();// 调用第一个命名空间中的函数
   second_space::func(); // 调用第二个命名空间中的函数
   return 0;
}
```

* **问：在 C 语言中，语句 int a = 016，那么十进制 a 是多少？**

* **答**：0 开头表示八进制，a 为 14。
****
### 运算符的使用
* **问：C 语言中的三目运算符的返回值？**

* **答**：**count  <<  ( x > y )  ?  x : y;** 返回 0 或 1，即真和假，只有 **z = ( x > y )  ?  x : y;** 才返回较大值。应该是运算符优先级的问题，**<<**  优先级高于 **?**
  [参考博客](https://blog.csdn.net/arbel/article/details/7294247)

  ```c++
  //问：输出是多少？
  int main()
  {
      int a = 1,b = 2,m = 0,n = 0,k;
      k = ( n = b < a ) && ( m = a ) ;
      printf("%d,%d\n",k,m);
      return 0;
  }
  //答：0，0 运算符的运行方式，&& 运算符，左边是 0 ，直接返回 0 ，左边是 1 则直接返回右边的布尔值。
  ```
****
### 宏的使用

  ```c++
  #include<stdio.h>
  #define SUM(x) 3*x*x+1 //关键字 define
  int main()
  {
      int i=5,j=8;
      printf("%d\n",SUM(i+j));
  }
  //输出64，宏的使用，代码编译的时候，宏会替换为表达式，即 （3 * i + j * i + j + 1）= 64。
  ```
****
### 字符串

* **第一种**：C++ 风格的字符串起源于 C 语言，并在 C++ 中继续得到支持。字符串实际上是使用 null 字符 '\0' 终止的一维字符数组。因此，一个以 null 结尾的字符串，包含了组成字符串的字符。下面示例长度为 6；

  ```c++
  char greeting[] = "Hello";//字符串长度为6，以'\0'结尾。
  ```

  赋值可以使用函数  **strcpy(S1,S2)** 将 S2 赋值到 S1。还有其他很多函数 **strcat、strlen、strcmp、strchr、strstr等等**

* **第二种**：C++ 标准库提供了 string 类类型，推荐此种，比较容易理解。

  ```c++
  #include <iostream>
  #include <string>
  using namespace std;
  int main (){
     string str1 = "Hello";
     string str2 = "World";
     string str3;
     int  len ;
      
     str3 = str1;// 复制 str1 到 str3
     cout << "str3 : " << str3 << endl;
  
     str3 = str1 + str2;// 连接 str1 和 str2
     cout << "str1 + str2 : " << str3 << endl;
  
     len = str3.size();// 连接后，str3 的总长度
     cout << "str3.size() :  " << len << endl;
  
     return 0;
  } 
  ```
****
### 指针和引用

* **指针 [ * ]**：是一个变量，其值为另一个变量的地址，即内存的地址。

* **(&)[取地址运算符]**：获取某个变量的地址；( * )[地址访问运算符]:访问某个地址的值；
  在变量声明的时候，如果没有确切的地址可以赋值，为指针变量赋一个 NULL 值是一个良好的编程习惯。赋为 NULL 值的指针被称为空指针。NULL 指针是一个定义在标准库中值为零的常量。

  ```c++
  int var = 100;
  int *pt = NULL; //没有 null 也可以，但养成好习惯嘛。
  pt = &var;
  int *ppt = &pt
  **pt;//二级指针，地址里包含的值还是一个地址,这样可以找到变量 var 的值。
  ```
  指向指针的指针是一种多级间接寻址的形式，或者说是一个指针链。通常，一个指针包含一个变量的地址。当我们定义一个指向指针的指针时，第一个指针包含了第二个指针的地址，第二个指针指向包含实际值的位置。

***

* **引用 [ & ]**：引用变量是一个别名，也就是说，它是某个已存在变量的另一个名字。一旦把引用初始化为某个变量，就可以使用该引用名称或变量名称来指向变量。通过使用引用来替代指针，会使 C++ 程序更容易阅读和维护。

  ```c++
  int a;
  int& b = a;//b 是 a 的引用，此时 a，b 同值。
  //一、最常见的 引用作为参数 用法
  void swap(int& x, int& y)
  {
     int temp;
     temp = x; /* 保存地址 x 的值 */
     x = y;    /* 把 y 赋值给 x */
     y = temp; /* 把 x 赋值给 y  */
     return;
  }
  //二、引用作为返回值 用法
  double vals[] = {10.1, 12.6, 33.1, 24.1, 50.0};
  double& setValues( int i )
  {
    return vals[i];// 返回第 i 个元素的引用，注意作用域，不要返回局部变量的引用。
  }
  setValues(0) = 100;//函数放在左边，重新赋值。
  ```

* **问：在 C++ 中，引用和指针的区别？**
* **答**：1. 引用总是指向一个对象，指针可能不指向对象。
2. 引用不能用 const 修饰，而指针可以。

3. 引用创建时必须初始化，而指针则可以在任何时候被初始化。

4. 引用和指针都可以被重新赋值，错误：指针可以被重新赋值，一旦引用被初始化为一个对象，就不能被指向到另一个对象。

****
* **输出函数 cout，cerr，clog**
  **cout**：经过缓冲后输出，默认情况下是显示器。缓冲区的目的，就是减少刷屏的次数——比如，你的程序输出圣经中的一篇文章。不带缓冲的话，就会每写一个字母，就输出一个字母，然后刷屏。有了缓冲，你将看到若干句子“同时”就出现在了屏幕上（由内存翻新到显存，然后刷新屏幕）。
  **cerr**：不经过缓冲而直接输出，一般用于迅速输出出错信息。因为有时候怕内存满了这种错误，不直接输出就没地方放了。
  **clog**：也是输出标准错误流（这点儿和 cerr 是一样的），貌似平时很少用到这个啊。
  [参考博客](https://blog.csdn.net/bsmmaoshenbo/article/details/50778068cout)
****
- **运算符和函数重载**
- **函数重载**：在同一个作用域内，可以声明几个功能类似的同名函数，但是这些同名函数的形式参数（指参数的个数、类型或者顺序）必须不同。您不能仅通过返回类型的不同来重载函数。
- **运算符重载[operator]**：与其他函数一样，重载运算符有一个返回类型和一个参数列表。大多数的重载运算符可被定义为普通的非成员函数或这被定义为类成员函数。如果我们定义上面的函数为类的非成员函数，那么我们需要为每次操作传递两个参数

### 高级教程

* **C++ 文件流**：我们已经使用了 **iostream** 标准库，它提供了 **cin** 和 **cout** 方法分别用于从标准输入读取流和向标准输出写入流。下面介绍如何从文件读取流和向文件写入流。这就需要用到 C++ 中另一个标准库 **fstream**。
* **.dat**：硬盘中的 .dat文件，这个是某个程序组的 数据库文件（data），是用来存放数据参数用的，比如说 3D游戏中的 3D 模型，皮肤文件等。这个打开也没用，因为都是编码。

```c++
//具体参看W3C里的介绍，open（文件位置，打开方式）函数用于打开文件。
void open(const char *filename, ios::openmode1 | ios::openmode2);
void close(); //关闭打开的文件

#include <fstream>//文件流操作库文件。
#include <iostream>
using namespace std;
int main (){
   char data[100];
   // 以写模式打开文件
   ofstream outfile;//该数据类型表示输出文件流，用于创建文件并向文件写入信息。
   outfile.open("afile.dat");

   cout << "Enter your name: "; 
   cin.getline(data, 100);//用户的输入保存在 data 中，输入一行数据
   outfile << data << endl;//向文件写入一行用户输入的数据

   cout << "Enter your age: "; 
   cin >> data;//再输入一行数据
   cin.ignore();
   outfile << data << endl;

   outfile.close();// 关闭打开的文件

   ifstream infile;// 以读模式打开文件
   infile.open("afile.dat"); 
 
   cout << "Reading from the file" << endl; 
   infile >> data; //读取一行
   cout << data << endl;// 在屏幕上显示
   
   infile >> data; // 再读取一行
   cout << data << endl; 

   infile.close();//关闭打开的文件

   return 0;
}
```

* **cin、cin.getline()、getline() 用法**

```c++
int a,b;
cin>>a>>b; //遇“空格”、“TAB”、“回车”就结束第一个输入

char str[100];
cin.getline(str,10);//接收十个字符到 str，可以接收空格

#include<string>//需包含头文件 string
string str;
getline(cin, str);
```

**当一个程序同时出现 cin>> 和 getline() 时，需要注意的是，在 cin>> 输入流完成之后，需要通过**

```c++
str="\n"; getline(cin,str);
```

**的方式将回车符作为输入流 cin 以清除缓存，如果不这样做的话，在控制台上就不会出现 getline() 的输入提示，而直接跳过，因为程序默认地将之前的变量作为输入流。**

```c++
#include<iostream>
#include<string>
using namespace std;

int main(){
    int age;
    //standard input(cin)
    cout<<"Please enter an integer value as your age: ";
    cin>>age;
    cout<<"Your ager is: "<<age<<".\n";
    
    string mystr;
    cout<<"What's your name:";
    //mystr="\n";//如果不加这两句，会直接跳过。
    //getline(cin,mystr);
    getline(cin,mystr);
    cout<<"Hello,"<<mystr<<".\n";

    system("pause");
    return 0;
}
```

### 动态内存

- **栈：**在函数内部声明的所有变量都将占用栈内存。
- **堆：**这是程序中未使用的内存，在程序运行时可用于动态分配内存。

很多时候，我们无法提前预知需要多少内存来存储某个变量中的信息，所需内存的大小需要在运行时才能确定。

在 C++ 中，您可以使用特殊的运算符为给定类型的变量在运行时分配堆内的内存，这会返回所分配的空间地址。这种运算符即 **new** 运算符。如果不需要动态分配内存，可以使用 **delete** 运算符，删除之前由 new 运算符分配的内存。

```c++
double* pvalue  = NULL; // 初始化为 null 的指针
pvalue  = new double;   // 为变量请求内存
```

**malloc()** 函数在 C 语言中就出现了，在 C++ 中仍然存在，但建议尽量不要使用 malloc() 函数。new 与 malloc() 函数相比，其主要的优点是，new 不只是分配了内存，它还创建了对象。

### 模板（Template）有点不好理解

模板是泛型编程的基础，泛型编程即以一种独立于任何特定类型的方式编写代码。模板是创建泛型类或函数的蓝图或公式。

```c++
#include <iostream>
#include <string>
using namespace std;

template <typename T>
inline T const& Max (T const& a, T const& b) 
{ 
    return a < b ? b:a; 
} //是不是感觉参数有点多，是的，template，typename 当做关键字好了，T 是我们定义的类型，先当做 int 
//理解，inline 是内联函数关键字，可以不加，后面是三个 （T const&）一个返回值，两个参数类型。

int main ()
{
 
    int i = 39;
    int j = 20;
    cout << "Max(i, j): " << Max(i, j) << endl; 

    double f1 = 13.5; 
    double f2 = 20.7; 
    cout << "Max(f1, f2): " << Max(f1, f2) << endl; 

    string s1 = "Hello"; 
    string s2 = "World"; 
    cout << "Max(s1, s2): " << Max(s1, s2) << endl; 

   return 0;
}
```









