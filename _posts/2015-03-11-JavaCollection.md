---
layout: post
title: Java中的Collection理解
categories: Java语言

---

>Java中的Collection，是指容纳其他对象数据结构，理解的时候可以参考数组，数组本身也是一个容器

Collection的结构图如下： 

![](/images/pages/java/collection.png)

<br/>

#### Collection 容器接口

<br/>

**Set** ：

* 无序、不可重复的对象（通过equals和hashCode方法来判断对象是否可以重复）
* HashSet是Set中重要的实现，内部使用HashMap来存储数据
* 遍历set的两种方法：增强for循环、使用迭代器iterator

<br/>

**List** :

* 有序，可重复的对象
* ArrayList：线程不安全，底层用数组实现，查询效率高，增删效率低
* LinkedList：线程不安全，底层使用双向链表实现，查询效率低，增删效率高
* Vector：线程安全，底层使用数组实现

遍历容器的三种方式：

```
       for(inti;i<list.size();i++){
              String str = list.get(i);
              System.out.println(str);
       }
      
       for(String str:list){//增强for循环
              System.out.println(str);
       }
      
       Iterator<String> ite =list.iterator();//迭代器
       while(ite.hasNext()){
              Stringstr = ite.next();
              System.out.println(str);
       }
      
       for(Iteratorite = list.iterator();ite.hasNext();){
              Stringstr = (STring)ite.next();
              System.out.println(str);
       }
```

<br/>

**Map** : 使用“key-value”方式来存储信息，key不能重复

* HashTable：线程安全，效率低
* HashMap：线程不安全，效率高
* properties：用来读取资源文件，放置在src目录下的文件
* 遍历Map的两种方式：entrySet方法、keySet、Values方法

其中Properties读取文件的方法：

```

Propertiesps = new Properties();  
FileReaderread = new FileReader(new File("…"));//路径  
ps.load(read);  
String p1= ps.getProperty("name");//从key来寻找value值  
System.out.println(p1);  
```

<br/>

**Iterator接口** :

* hasNext判断时候到末尾
* next返回对象
* remove移除元素（如果在遍历时删除集合中的元素，建议采用迭代器中的这个方法，万无一失）



