---
layout: post
title: LinkedList 和 ArrayList的区别
categories: Java语言

---

>Java中两种常见的链表结构分别是用数组实现的ArrayList数据结构和用链表实现的LinkedList数据结构。其中用ArrayList访问速度快，而LinkedList插入删除的效果好

### 两者对比
* ArrayList基于动态数组来实现，LinkedList基于链表来实现
* 两种数据结构在尾部添加一个元素的时间开销是固定的（ArrayList偶尔的数组长度增大可忽略）
* ArrayList中间插入一个元素会导致它右边的元素都要移位，开销变大。而LinkedList中间插入一个元素的开销则相对固定。这是由于ArrayList内存连续，而链表的内存中占用分散
* LinkedList不支持高效的元素随机访问，每个元素的访问都要从head或者其它节点去遍历
* ArrayList使用数组来保存对象，LinkedList的每一个节点都是一个单独的对象，每个对象都放在一个独立的空间。
* ArrayList的空间浪费体现在List尾部的预留空间。LinkedList的空间浪费体现在它的每一个节点都是独立的，都要消耗固定的空间
* ArrayList默认构造函数的默认容量是10
* 两者都是线程不安全的

### 总结
* ArrayList使用数组实现，随机访问效率高，随机插入、删除的效率低
* LinkedList使用随机访问效率低，随机插入删除的效率高




