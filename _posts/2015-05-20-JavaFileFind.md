---
layout: post
title: 用嵌套递归的方法搜索指定目录下的所有文件
categories: Java语言

---

> 在File类的IO操作过程中，常常要查看指定目录下的文件，无论是文件夹还是文件名，这里我们希望能搜索指定目录下的所有文件。具体的方法是采用函数的嵌套调用。

搜索指定目录下的所有文件的方法分成下面四步来进行

* step 1：创建一个方法seeFile
* step 2：判断当前的文件不为空
* step 3：判断当前文件是否为文件夹，如果不是，直接打印。如果是文件夹，用一个FIle数据取出所有当前文件夹的所有文件File.listFile 
* step 4：对File数组中的每个文件名递归调用该方法(seeFile)方法

<br/>

具体的代码如下：

```
public class Test {  
    public static void main(String[] args) {  
        String fileName = "E:"+File.separator;  
        File f = new File(fileName);  
        print(f);  
    }  
  
    private static void print(File f) {  
        if(f != null){  
            if(f.isDirectory()){  
                File[] list = f.listFiles();  
                if(list != null){  
                    for(int i=0; i<list.length; i++){  
                        print(list[i]);  
                    }  
                }  
                  
            }  
            else{  
                System.out.println(f);  
            }  
        }  
    }  
}  
```





 