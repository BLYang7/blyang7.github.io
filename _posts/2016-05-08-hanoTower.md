---
layout: post
title: 递归调用（汉诺塔）
categories: 数据结构和算法

---

>递归调用的两个准则是：一是保持原有问题的形式，逐步降低复杂度。二是最简单的情况可以直接解决。

汉诺塔是递归调用的最经典的例子，汉诺塔的算法原理是：

* step1：把前面的N-1个圆盘从original移动到temp
* step2：把第N个圆盘从original移动到destination
* step3：把之前放在temp的N-1个圆盘从temp移动到destination

具体代码如下：

```
public class Main {  
      
    private void hano(int num, char original, char destination, char temp){  
          
        //问题最简单的形式  
        if(num == 1){  
            move(original, destination);  
        }  
        //具体的算法,注意保持问题的原始形式  
        else{  
            hano(num-1, original, temp, destination);//step1  
            move(original, destination);//step2  
            hano(num-1, temp, destination, original);//step3  
        }  
    }  
      
    private void move(char original, char destination) {  
        System.out.println("direction: " + original + " --> " + destination);  
    }  
      
    public static void main(String[] args) {  
        new Main().hano(3, 'A', 'B', 'C');  
    }  
}  
```



