---
layout: post
title: 寻找第K大数的方法
categories: 数据结构和算法

---

>寻找一堆数中第K大的数，第一感觉是排序，然后将排序之后的值取第K个。但是实际上，这种方式最少的时间复杂度是O(nlogn)。有更简单的方式可以实现线性的时间复杂度

**算法总是有穷尽的，而思想无穷尽，而使用算法的本质是用空间去换取时间**

这里给的方案是

* 1）假设所有的数都是正数，并且事先知道最大数的范围
* 2）开辟一个数组numArr，长度至少要为最大数加一，初始化数组中每个元素都为0
* 3）对于某个数T，在对应的数组 numArr[T] 的位置上加一（将所有的数都进行当前操作）
* 4）从数组的最顶端开始向下累加，sum+=numArr[i]，当sum的值大于等于K时，当前的i即为这组数中第K大的值

如下图示：

![](/images/pages/datastructure/firstK.jpg)

在这个例子中，当我们取第5大的数的时候，从顶部向下累加，发现加到第三个数的时候，sum就为5了，所以此时第5大的数应该就是对应的i值为7.

给下面的代码：


```
/** 
 * 寻找第K大大数值 
 * @author blyang 
 */  
public class KBig {  
  
    public static void main(String[] args) {  
          
        int K = 5;  
        int[] nums = new int[]{ 0, 3, 2, 7, 8, 9, 3, 4, 6, 1, 9, 3, 7};  
        int max = 10;  
        int[] numArr = new int[max];  
        int sum = 0;  
          
        for( int i=0; i<numArr.length; i++ ){  
            numArr[i] = 0;  
        }  
          
        for(int num : nums){  
            numArr[num]++;  
        }  
          
        int i=0;  
        for( i=numArr.length-1; i>=0; i--){  
            sum += numArr[i];  
            if(sum >= K){  
                break;  
            }  
        }  
          
        System.out.println(i);  
    }  
}  
```

