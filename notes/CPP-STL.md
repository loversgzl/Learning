# CPP STL模板库

* **容器（Containers）**：容器是用来管理某一类对象的集合。C++ 提供了各种不同类型的容器，比如 deque、list、vector、map 等。

* **算法（Algorithms）**：相当于容器内置的方法，函数作用于容器。它们提供了执行各种操作的方式，包括对容器内容执行初始化、排序、搜索和转换等操作。

* **迭代器（iterator）**：迭代器用于遍历对象集合的元素。这些集合可能是容器，也可能是容器的子集。
****
* **向量 Vector**：类似数组，但是大小可变。

* **使用的函数**：size()、push_back()、begin()、end()。

  ```c++
  #include <iostream>
  #include <vector> //需要这个库文件
  using namespace std;
  int main(){
     vector<int> vec; // 创建一个向量存储 int
     int i;
     cout << "vector size = " << vec.size() << endl;// 显示 vec 的原始大小
     for(i = 0; i < 5; i++){// 推入 5 个值到向量中
        vec.push_back(i);
     }
     cout << "extended vector size = " << vec.size() << endl;// 显示 vec 扩展后的大小
     for(i = 0; i < 5; i++){// 访问向量中的 5 个值
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















