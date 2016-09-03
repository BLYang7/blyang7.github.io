---
layout: post
title: 静态分派和动态分派
categories: Java虚拟机

---

>区分JVM中的静态分派和动态分派

#### 静态分派
**在Java中所有以静态类型来确定执行版本的分派动作，称为静态分派**

<br/>

#### 动态分派
**在运行期根据实际类型来确定方法执行版本的分派动作，称为动态分派**

<br/>

```
Human man = new Man();
Human women = new Women();
```

针对上面一段代码，Human是这两个变量的静态类型，而Man和Women则是二者的动态类型。静态类型和动态类型的区别在于，静态类型在编译时可知，而动态类型则需要在运行时才能确定。

而重载的方法在编译时，只能通过方法的静态类型来确定方法执行版本。举个例子来区分：

```
class Human{
	public void sayHello(){};
}

class Man extends Human{
	public void sayHello(){
		System.out.println("hello, Man");
	}
}

class Women extends Human{
	public void sayHello(){
		System.out.println("hello, Women");
	}
}

public class Main{

	Human man = new Man();
	Human women = new Women();
	
	public void sayHello(Human guy){
		System.out.println("hello, guy");
	}
	
	public void sayHello(Man guy){
		System.out.println("hello, man");
	}
	
	public void sayHello(Women guy){
		System.out.println("hello, women");
	}
	
	public static void main(String[] args) {
		Main main = new Main();
		
		//静态分派
		main.sayHello(main.man); //guy
		main.sayHello(main.women); //guy
		
		//动态分派
		main.man.sayHello(); //man
		main.women.sayHello(); //women
		
	}
	
}

```
