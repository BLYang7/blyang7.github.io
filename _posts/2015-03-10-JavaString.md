---
layout: post
title: String、StringBuffer和StringBuilder
categories: Java语言

---

>三种字符串变量操作的区别。对于字符串的操作是计算机程序中最常见的行为。而字符串中最常用的类是String类、StringBuffer类

<br/>
Java中定义了String和StringBuffer两个类来封装对字符串的各种操作。它们都被放置到java.lang包中，两者不同的是，String类是不可变字符序列类，而StringBuffer是可变的字符序列类。

<br/>

#### String

String的不可变是指String类对象实例化以后，所有的属性都是final的，当前的字符序列将不能被改变。需要注意的是，String str = “hello”；这里的str并不是真正的String对象，它只是指向String对象的一个引用类型变量而已。后面的“hello”才是真正的String对象，该String对象创建之后会一直存在于内存中，不会被JVM回收，这也是造成Java内存泄露的一个点。

String类的对象的内容一旦被初始化以后就不能再改变，String类中的每一个看起来会修改String值的方法，其源码的操作实际上都是创建了一个新的String对象，以包含修改后的字符内容。在面向对象程序设计中，最好是能重复运用已生成的对象，因为新的对象的生成需要内存空间与时间，不断地产生String实例，并且改变引用的指向是一个没有效率的行为。

<br/>

#### 三者区别

String类主要用于比较两个字符串，查找和抽取串中的字符或者子串，进行字符串与其他类型之间的相互转换。

StringBuffer类用于内容可变的字符串，可以将其他各种类型的数据增加、插入到字符串当中，任意修改字符串的值。每次对StringBuffer对象的修改都是对它本身的修改，并不涉及新的对象，这样就加快了处理速度。

在StringBuffer使用的过程中又出现了一些问题，于是在J2SE5.0以后引入了StringBuilder类。StringBuffer和StringBuilder是十分相似的两个类，基本能用StringBuffer实现的问题中，都能使用StringBuilder来完成。

StringBuffer类是线程安全的，可以应用于多线程的问题中，但是相应的也带来了效率低的问题。

StringBuilder类的效率更高，但是线程非安全，一般应用于单线程的问题中。

简单的来说：

* String：字符串常量
* StringBuffer：字符串变量、线程安全、效率低
* StringBuilder：字符串变量、线程非安全、效率高
 