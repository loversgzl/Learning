# Python-面试问题

**参考链接**

[**100 道真实面试题**](https://blog.csdn.net/weixin_41666747/article/details/79942847)

[2016 年阿里校招 Python 面试](https://www.cnblogs.com/acvc/p/4714848.html)

[阿里 Python 工程师面试题](http://developer.51cto.com/art/201801/564790.htm)

[Python 36 问](https://baijiahao.baidu.com/s?id=1607651363840614527&wfr=spider&for=pc)

****

**面试题**

* **面：Python 中什么元素为假？**
* **答**：（0，空字符串，空列表、空字典、空元组、None, False）

* **面：Python 中查看某个关键字的属性？**
* **答**：dir ( key )，help() 函数返回帮助文档和参数说明，dir() 函数返回对象中的所有成员 (任何类型)

* **面：Python 全局变量 global 的使用？**
* **答**：[参考博客，超详细](https://blog.csdn.net/qw_sunny/article/details/80972357)

* **面：Python 中的 pass 语句有什么作用？**
* **答**：我们在写代码时，有时可能只写了函数声明而没想好函数怎么写，但为了保证语法检查的正确必须输入一些东西。在这种情况下，我们使用 pass 语句。

* **面：举例说明异常模块中 try except else finally 的相关意义？**
* **答**：try..except..else 没有捕获到异常，执行 else 语句；try..except..finally 不管是否捕获到异常，都执行 finally 语句。

* **面：with 方法打开处理文件帮我们做了什么？yield 的使用-生成器？**
* **答**：功能类似 try，except，finally 中 finally 的用法，with：除了有更优雅的语法，真正强大的地方：with 还可以很好的处理上下文环境产生的异常。with 后面的函数需要有 __ exit __ 方法，当程序出现异常的时候会帮助我们打印异常，清理资源，关闭文件。[with 参考博客](https://www.cnblogs.com/DswCnblog/p/6126588.html)，[yield 参考博客](https://blog.csdn.net/mieleizhi0522/article/details/82142856)

* **面：Python 中断言方法举例？**
* **答**：

* **面：？**
* **答**：

* **面：？**
* **答**：

* **面：？**
* **答**：

* **面：？**
* **答**：

* **面：？**
* **答**：

* **面：？**
* **答**：

* **面：？**
* **答**：