# CPP STL模板库

* **容器（Containers）**：容器是用来管理某一类对象的集合。C++ 提供了各种不同类型的容器，**比如 deque、list、vector、map 等。**
* **算法（Algorithm）**：相当于容器内置的方法，函数作用于容器。它们提供了执行各种操作的方式，包括对容器内容执行初始化、排序、搜索和转换等操作。
* **迭代器（iterator）**：迭代器用于遍历对象集合的元素。这些集合可能是容器，也可能是容器的子集。
* **函数对象、分配器等**
****
### 向量 （Vector）

* **类似数组，但是大小可变，使用的函数：size()、begin()、end()、添加：push_back()、insert()、删除：erase()、clear()，遍历输出向量的两种方式。**

  ```c++
  #include <iostream>
  #include <vector> //需要使用库文件
  using namespace std;
  int main(){
     vector<int> vec; //创建一个向量存储一维数组
     //vector<vector<int>> vec; //创建二维数组
     //vector<int*> vec; //创建多维数组
     
     //添加元素的方法，push_back（num）、insert(index,num)
     cout << "vector size = " << vec.size() << endl;// 显示 vec 的原始大小
     for(int i = 0; i < 5; i++){// 推入 5 个值到向量中
        vec.push_back(i);
     }
     vec.insert(vec.begin(),100);
      
     //删除元素的方法,erase(index),erase(start,end),clear()
     vec.erase(vec.begin()+1);
      
     //打印 vector 的两种方法
     cout << "extended vector size = " << vec.size() << endl;// 显示 vec 扩展后的大小
     for(int i = 0; i < 5; i++){// 访问向量中的 5 个值
        cout << "value of vec [" << i << "] = " << vec[i] << endl;
     }
     vector<int>::iterator v = vec.begin();// 使用迭代器 iterator 访问值
     while( v != vec.end()) {
        cout << "value of v = " << *v << endl;
        v++;
     }
     return 0;
  }
  ```
  **算法 algorithm 库的使用，reverse（）函数**
  
  ```c++
  #include <iostream>
  #include <vector>
  #include <algorithm>
  using namespace std;
  int main(){
      vector<int> vec;
      vec.push_back(1);
      vec.push_back(2);
      vec.push_back(3);
      reverse(vec.begin(),vec.end());//倒转向量
      for(int i=0; i<vec.size(); i++){
          cout<<vec[i]<<endl;
      }
      return 0;
  }
  ```
**输入输出**：连续输入一串数后，遇到回车退出。
  
  ```c++
  vector<int> a;
  int n=0,b;
  while(cin>>b){
  	a.push_back(b);
  	n++;
  	if (cin.get() == '\n') 
  		break;
  }
  ```

****

### map

* 功能类似平常使用的字典，[参考W3C](https://www.w3cschool.cn/cpp/cpp-fu8l2ppt.html)，**map 的使用，也有很多函数，insert、erase、count、empty等等。**
```c++
#include<map>//需要使用库文件
//定义一个 map
map<int ,string> maplive;//map 的定义方式，键为 int，值为 string。
maplive.insert(pair<int,string>(102,"aclive"));//方式一
maplive.insert(map<int,string>::value_type(321,"hai"));//方式二
maplive[112]="April";//map中最简单最常用的插入添加！

//map 中查找元素
map<int ,string >::iterator l_it;
l_it=maplive.find(112);
if(l_it==maplive.end())
    cout<<"we do not find 112"<<endl;
else 
    cout<<"wo find 112"<<endl;

//map 中删除元素
map<int ,string >::iterator l_it;;   
l_it=maplive.find(112);
if(l_it==maplive.end())
    cout<<"we do not find 112"<<endl;
else  
    maplive.erase(l_it);  //delete 112;

//map 中 swap 的用法，交换两个 map 的引用。
#include <map> 
#include <iostream>
using namespace std;
int main( ){
    map <int, int> m1, m2, m3;
    map <int, int>::iterator m1_Iter;
    m1.insert ( pair <int, int>  ( 1, 10 ) );
    m1.insert ( pair <int, int>  ( 2, 20 ) );
    m2.insert ( pair <int, int>  ( 10, 100 ) );
    cout << "The original map m1 is:";
    for ( m1_Iter = m1.begin( ); m1_Iter != m1.end( ); m1_Iter++ )
        cout << " " << m1_Iter->second;
    cout   << "." << endl;
    
    m1.swap( m2 );//将两个 map 的引用交换。
    cout << "After swapping with m2, map m1 is:";
    for ( m1_Iter = m1.begin( ); m1_Iter != m1.end( ); m1_Iter++ )
        cout << " " << m1_Iter -> second;
    cout  << "." << endl;
    return 0;
}
```







