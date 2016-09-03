---
layout: post
title: 文本编辑距离计算
categories: 数据结构和算法

---

>所谓文本编辑距离，是指从一个文本编程另一个文本所需要的最小操作数。这些操作一般包括字符的插入、删除和替换

文本编辑距离的概念是俄罗斯科学家在1965年提出来的

编辑距离的算法可以概括如下：定义一个编辑距离的函数editText(i,j)表示从长度为i的字符串变到长度为j的字符串所需的最小操作数。这个问题可以用动态规划的方法来求解，概括为以下几点：

* if (i == 0 && j == 0)  edit(i, j) = 0;
* if (i == 0 && j > 0)   edit(i, j) = j;
* if (i > 0 && j == 0)  edit(i, j) = i;
* if (i >= 1 && j >= 1)  edit(i, j) == min( edit(i - 1, j) + 1, edit(i, j - 1) + 1, edit(i - 1, j-1) + f(i,j) ); 其中当字符串A的第i个字符和字符串B的第j个字符相同时，f(i,j) 为零，否则为一。

具体的计算过程如下图所示。在一个字符串中，每个char是一个单元，底标编号从0开始。而矩阵中的每一个元素，都是一个函数 edit( i, j ),。整个矩阵的第一行和第一列的数值是很容易得到的。然后函数 f(i,j)的数值只需要对比纵向第i个char和横向第j个char是否相同即可。然后递归的向上，动态得到矩阵内的所有值。最后，矩阵的右下角的值即为两个字符串之间的最小编辑距离。需要注意的是，这里的编辑包涵插入、删除和替换。

![](/images/pages/datastructure/textMargin.jpg)

具体的算法如下：

```
///   
/// 计算两个文本的编辑距离  
///   
private int edit(string strA, string strB)  
{  
    int maxA = strA.Length;  
    int maxB = strB.Length;  
  
    //定义编辑矩阵并赋初值  
    int[ , ] matrix = new int[maxA + 1,maxB+1];  
    for (int i = 0; i < maxA + 1; i++ )  
    {  
        matrix[i , 0] = i;  
    }  
    for (int i = 0; i < maxB + 1; i++)  
    {  
        matrix[0, i] = i;  
    }  
  
    //计算矩阵的各个值  
    for (int i = 1; i < maxA + 1; i++)  
    {  
        for (int j = 1; j < maxB + 1; j++)  
        {  
            int d;  
            int temp = Math.Min(matrix[i-1,j]+1, matrix[i,j-1]+1);  
            if (strA.Substring(i - 1, 1) == strB.Substring(j - 1, 1))  
            {  
                d = 0;  
            }  
            else  
            {  
                d = 1;  
            }  
            matrix[i , j] = Math.Min(temp, matrix[i - 1,j - 1] + d);  
            Console.Write("{0}\t", matrix[i, j]);  
        }  
        Console.WriteLine("");  
    }  
  
    return matrix[maxA, maxB];  
}  
```

也就说说，矩阵右下角的数字为两个字符串的编辑距离，编辑距离越小，两个字符串相似的程度也就越高