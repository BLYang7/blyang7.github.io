---
layout: post
title: Java线程之间的通信
categories: Java语言

---

>Java多线程中，不同线程之间有时候需要交换信息，这就需要线程之间相互通信


#### wait、notify、notifyAll 实现通信
传统的线程通信借助于Object类的三个方法，分别是wait()、notify()、notifyAll()。使用这三种方法来实现线程通信的一般是用synchronized实现线程同步的类。

* wait():让当前线程处于暂停状态，也就是阻塞当前线程，直到其他线程调用该同步监视器的notify()方法或者是notifyAll方法来唤醒该线程
* notify():唤醒该同步监视器上等待的单个线程
* notifyAll():唤醒该同步监视器上等待的所有线程

举个例子：

```
//简易替代取钱的方法，存钱和取钱交叉进行，flag为一个标记  
public synchronized void draw(double drawAmount){  
    try {  
        // 如果flag为假，表明账户中还没有人存钱进去，取钱方法阻塞  
        if (!flag){  
            wait();  
        }  
        else{  
            // 执行取钱  
            System.out.println(Thread.currentThread().getName()+"取钱:"+drawAmount);  
            balance -= drawAmount;  
            System.out.println("账户余额为：" + balance);  
            // 将标识账户是否已有存款的旗标设为false。  
            flag = false;  
            // 唤醒其他线程  
            notifyAll();  
        }  
    }  
    catch (InterruptedException ex){  
        ex.printStackTrace();  
    }  
}  
public synchronized void deposit(double depositAmount){  
    try{  
        // 如果flag为真，表明账户中已有人存钱进去，则存钱方法阻塞  
        if (flag){  
            wait();  
        }  
        else{  
            // 执行存款  
            System.out.println(Thread.currentThread().getName()  
                + " 存款:" +  depositAmount);  
            balance += depositAmount;  
            System.out.println("账户余额为：" + balance);  
            // 将表示账户是否已有存款的旗标设为true  
            flag = true;  
            // 唤醒其他线程  
            notifyAll();  
        }  
    }  
    catch (InterruptedException ex){  
        ex.printStackTrace();  
    }  
}  
```

<br/>

#### Condition类控制线程通信

当程序不使用synchronized实现同步，而是显式的使用Lock对象来保证同步时，系统中不存在隐式的同步监视器，所以不能使用wait()、notifyAll()等方法来进行线程通信。

针对这种情况，Java提供了Condition类，这个类可以让哪些已经得到Lock对象却无法继续执行的线程释放Lock对象，Condition对象也可以唤醒其他的处于等待的线程。

这种情况下，Lock替代了同步方法或者是同步代码块，而Condition替代了同步监视器的功能。Condition提供了下面三种方法：

* await()方法：类似于wait()方法
* signal()方法：类似于notify方法
* signalAll()方法：类似于notifyAll方法

当显式的使用Lock对象来充当同步监视器时，需要使用Condition对象来暂停、唤醒指定线程。具体的Condition类的创建方法，用下面的例子来说明，关键之处在于针对Lock类对象指定一个Condition对象：

```
public class Account  
{  
    // 显式定义Lock对象  
    private final Lock lock = new ReentrantLock();  
    // 获得指定Lock对象对应的Condition  
    private final Condition cond  = lock.newCondition();   
      
    // 封装账户编号、账户余额两个Field  
    private String accountNo;  
    private double balance;  
    //标识账户中是否已有存款的旗标  
    private boolean flag = false;  
  
    // ......省略了很多代码段，包括getter、setter和构造器等等  
      
    public void draw(double drawAmount)  
    {  
        // 加锁  
        lock.lock();  
        try{  
            // 如果flag为假，表明账户中还没有人存钱进去，取钱方法阻塞  
            if (!flag){  
                cond.wait();  
            }  
            else{  
                // 执行取钱  
                System.out.println(Thread.currentThread().getName()+"取钱:"+drawAmount);  
                balance -= drawAmount;  
                System.out.println("账户余额为：" + balance);  
                // 将标识账户是否已有存款的旗标设为false。  
                flag = false;  
                // 唤醒其他线程  
                cond.signalAll();  
            }  
        }  
        catch(InterruptedException ex){  
            ex.printStackTrace();  
        }  
        // 使用finally块来释放锁  
        finally{  
            lock.unlock();  
        }  
    }  
    public void deposit(double depositAmount){  
        lock.lock();  
        try{  
            // 如果flag为真，表明账户中已有人存钱进去，则存钱方法阻塞  
            if (flag){  
                cond.wait();  
            }  
            else{  
                // 执行存款  
                System.out.println(Thread.currentThread().getName()+"存款"+depositAmount);  
                balance += depositAmount;  
                System.out.println("账户余额为：" + balance);  
                // 将表示账户是否已有存款的旗标设为true  
                flag = true;  
                // 唤醒其他线程  
                cond.signalAll();  
            }  
        }  
        catch(InterruptedException ex){  
            ex.printStackTrace();  
        }  
        // 使用finally块来释放锁  
        finally{  
            lock.unlock();  
        }  
    }  
```

<br/>

#### 使用BlockingQueue控制线程通信

Java 5 提供了一个更方便的线程通信工具，BlockingQueue，它虽然是Queue的子接口，但是他的主要用途并不是作为容器，而是作为线程同步工具，这决定于BlockingQueue的特征：**当生产者线程视图想BlockingQueue中放入元素时，若该队列已满，则线程被阻塞。当消费者线程试图从BlockingQueue中获取元素时，若队列已空，则该线程被阻塞。**





