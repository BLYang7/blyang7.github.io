---
layout: post
title: Java内存管理的Tips
categories: Java虚拟机

---

>整理了一些Java使用过程中的内存管理的一些小技巧

**1、尽量使用直接量**

在创建的时候使用直接量，减少new的过程，比如说 String string = "--string--"; 而不是使用 String string = new String("--string--");

<br/>

**2、养成使用StringBuilder和StringBuffer的习惯**

对不断需要更改的字符串使用StringBuilder或者是StringBuffer，而不是String。这是因为String创建的都是固定的字符序列，这些字符序列放在内存中是不会被销毁的，容易造成内存泄露。

<br/>

**3、尽早释放无用对象的引用**

当对象的引用使用结束后，直接将对象赋值为NULL，释放对象的引用。

<br/>

**4、尽量少用静态变量**

使用static修饰的变量，JVM内存回收机制是不会销毁它的，那么它就要占用常驻内存，造成资源浪费。

<br/>

**5、减少Java对象的创建**

在经常调用的方法中，或者是循环语句中，要避免Java对象的创建，尽管这些变量是局部变量，在对象的不断的创建、销毁回收的过程中，程序的性能将受到巨大的影响。

<br/>

**6、缓存经常使用的对象**

<br/>

**7、尽量不要使用finalize()方法**
