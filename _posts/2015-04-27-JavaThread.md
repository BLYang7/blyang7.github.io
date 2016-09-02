---
layout: post
title: Java多线程的三种实现方式
categories: Java语言

---

>Java多线程的几种实现


#### 继承Thread类来实现多线程

在继承Thread类之后，一定要重写类的run方法，在run方法中的就是线程执行体，在run方法中，直接使用this可以获取当前线程，直接调用getName()方法可以获得当前线程的名称，多线程的名称一般为Thread-n。而在主方法中，需要调用Thread.currentThread()方法来获取当前线程。需要注意的是，主线程（从主方法main开始执行的线程）也是一个线程。main方法中的内容代表的是主线程的线程执行体。

在Thread对象创建完成之后，要调用start方法来启动线程对象。例子如下：

```
public class ThreadName extends Thread{

	private int i;
	
	public void run(){
		for(; i<100; i++){
			System.out.println(getName() + i);
		}
	}
	
	public static void main(String[] args) {
		for(int i=0; i<100; i++){
			System.out.println(Thread.currentThread().getName() + i);
			
			if(i==20){
				new ThreadName().start();
				new ThreadName().start();
			}
		}
	}
}

```

<br/>

#### 通过实现Runnable接口来实现多线程

同样的，当实现了Runnable接口之后，要重写接口中的run方法，否则会报错。run方法中的代码块儿就是线程执行体。但是，此时在run方法中想要获得当前线程，必须使用Threa.currentThread ()方法。在主线程中如果需要启动新的线程，其创建的方法也与直接继承Thread类有区别。**需要首先创建Runnable实现类的实例，然后左右Thread的Target来创建Thread对象，**这个对象才是真正的线程对象，实际的线程对象依然是Thread实例，只是这个Thread线程负责执行target对象的run()方法。完成之后再调用start方法来启动线程对象。

```
public class ThreadName implements Runnable  
{  
    private int i ;  
  
    public void run()  
    {  
        for ( ; i < 100 ; i++ )  
        {  
            System.out.println(Thread.currentThread().getName() + i);  
        }  
    }  
          
    public static void main(String[] args)   
    {  
        for (int i = 0; i < 100;  i++)  
        {  
            System.out.println(Thread.currentThread().getName()  
                + "  " + i);  
            if (i == 20)  
            {  
                ThreadName st = new ThreadName();     
                // 通过new Thread(target , name)方法创建新线程  
                new Thread(st , "新线程1").start();  
                new Thread(st , "新线程2").start();  
            }  
        }  
    }  
}  
```

<br/>

#### 通过实现Callable接口实现多线程

当实现Callable接口之后，要重写接口中的call方法，call方法中的代码作为线程执行体，此时的call方法可以有返回值。

Callable接口是Java5中新增的接口，不是Runnable接口的子接口，所以Callable对象不能直接作为Thread对象的Target，于是Java5中提供了Future接口来代表Callable接口里call方法的返回值，并为Future接口提供了一个FutureTask实现类，它实现了Future接口和Runnable接口，可以作为Thread类的Target。所以在创建Callable
接口实现类之后，要用FutureTask来包装Callable对象（实现手动装箱）。然后用FutureTask对象作为Target。

```
public class ThreadName implements Callable<Integer>  
{  
    public Integer call()  
    {  
        int i = 0;  
        for ( ; i < 100 ; i++ )  
        {  
            System.out.println(Thread.currentThread().getName() + "的循环变量i的值：" + i);  
        }  
        // call()方法的返回值  
        return i;  
    }  
  
    public static void main(String[] args)   
    {  
        // 创建Callable对象  
        ThreadName rt = new ThreadName();  
        // 使用FutureTask来包装Callable对象  
        FutureTask<Integer> task = new FutureTask<Integer>(rt);  
        for (int i = 0 ; i < 100 ; i++)  
        {  
            System.out.println(Thread.currentThread().getName() + "的循环变量i的值：" + i);  
            if (i == 20)  
            {  
                // 实质还是以Callable对象来创建、并启动线程  
                new Thread(task , "有返回值的线程").start();  
            }  
        }  
        try  
        {  
            // 获取线程返回值  
            System.out.println("子线程的返回值：" + task.get());  
        }  
        catch (Exception ex)  
        {  
            ex.printStackTrace();  
        }  
    }  
}  
```

**上述几种创建方式都能够实现多线程，后面两种很相似，只不过Callable接口中的Call方法可以有返回值，可以抛出异常。一般在实际的代码中，推荐使用实现Runnable接口或者是Callable接口的方式来实现多线程**
