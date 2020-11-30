# SpringBoot框架
简介

遇到的问题；
1、添加 jar 包：新建文件夹，在F盘里看不见，也赋值不进去文件。只能在IDEA里面新建，然后拖进去。然后右键 add as Library 添加 jar 包。


# 注解
```java
/*
问：@SuppressWarnings(“deprecation”)的功能是什么？
Java三大注解分别是@Override @Deprecated @Suppresswarnings
*Deprecated 注解*
可以修饰类、方法、变量，在java源码中被@Deprecated修饰的类、方法、变量等表示不建议使用的，可能会出现错误的，可能以后会被删除的类、方法等，如果现在使用，则在以后使用了这些类、方法的程序在更新新的JDK、jar包等就会出错，不再提供支持。     个人程序中的类、方法、变量用@Deprecated修饰同样是不希望自己和别人在以后的时间再次使用此类、方法。  当编译器编译时遇到了使用@Deprecated修饰的类、方法、变量时会提示相应的警告信息。

*Suppresswarnings 注解*
可以达到抑制编译器编译时产生警告的目的，但是很不建议使用@SuppressWarnings注解，使用此注解，编码人员看不到编译时编译器提示的相应的警告，不能选择更好、更新的类、方法或者不能编写更规范的编码。同时后期更新JDK、jar包等源码时，使用@SuppressWarnings注解的代码可能受新的JDK、jar包代码的支持，出现错误，仍然需要修改。
*/

```