# Vue 常见报错
**报错：Vue 启动问题（You may use special comments to disable some warnings）**  
[原因：Eslint的检测机制](https://blog.csdn.net/ThisEqualThis/article/details/100550761)

****
**'报错：webpack-dev-server' 不是内部或外部命令，也不是可运行的程序 或批处理文件。**  
试错一：先不删除 node_modules，直接 cnpm install，这样更快，看看是否可行，不行再试试下一条。
试错二：1、删除根目录下的 node_modules  
2、cnpm install 重新安装  
3、直接运行，简单粗暴  

****
**报错：Unexpected token '<'**  
[引入 jquery 文件错误](https://www.jianshu.com/p/6c3c33c43509)  
1、这种错误一般都是文件路径错误导致，找不到对应的文件，你自己点开文件夹可以找到，但是系统获取不到，找不到或者打不开。

****
**问题：vuex 传递参数，无法传递两个及以上？**  
[将参数以对象的形式传入](https://www.cnblogs.com/shidawang/p/12090971.html)

****
**报错：从另一个页面获取的变量会动态变化，但是 item 不实时显示变化，无法动态更新 data 数据？**  
[state 六种传值方式](https://www.cnblogs.com/ming1025/p/13073753.html)


# 使用 Element 组件报错记录
[Element 官方文档](https://element.eleme.cn/#/zh-CN)

**报错：Vue 中清空 Form 表单的 resetFields 方法不起作用？**  
答：如果要使用此方法，那么表单中的所有 item 都要加 prop 属性，该方法会根据 prop 属性中的值找到对应的 formdata，然后清除。（注意：如果使用了 vuex，则必须先清空 vuex，此方法无效）

