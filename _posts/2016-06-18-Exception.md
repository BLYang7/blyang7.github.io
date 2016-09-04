---
layout: post
title: Exception和RuntimeException的区别
categories: Java语言

---

几种异常情况要做个说明

* error：编译时错误，语法有问题，这就不作赘述了
* Exception：编译时异常，这是指编译是通不过，必须要做处理的一场，比如说IO块儿，必须要用try catch或者throw处理的
* RuntimeException：运行时异常，这是指编译时可以通过，但是程序运行时可能会出错的错误，此事出现异常，会有JVM作处理，一般会中断程序进程，报错。

<br/>

同样一段代码，有时候会发现没有出现RuntimeException，而另外一些时候，又出现了RuntimeExcepton。

其实，本质上是这段代码真的有问题，没有报错只是因为还没有跑到该报错的位置。比如说数组越界的问题ArrayIndexOutOfBoundsException，空指针异常NullPointerException。对于RuntimeException，简单的try catch是解决不了问题的，要想想代码设计的逻辑，或者是处理问题的思路，用一种合理的方式来解决这种Bug。

更直观的说吧，Exception编译时异常，是说代码本身的逻辑没有问题，出问题的是语言本身潜在的风险，比如说IO时无法打开文件流。此时用try catch是可以完美处理的。

而RuntimeException，则是代码本身的逻辑问题了，是有bug的，用try catch块儿是没法完美处理这个bug的，这就是程序员的问题了。