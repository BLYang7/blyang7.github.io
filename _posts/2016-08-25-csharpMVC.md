---
layout: post
title: C#下的MVC框架
categories: 框架

---

>MVC框架的经典性是一致认同的，它的优点在与降低了逻辑、展示、数据三者之间的耦合性，也提高了代码的可用性，同时对搜索引擎更友好，也降低了服务器访问和处理的复杂度。这里给个MVC框架的例子，设计接口实现、Model定义、序列化和反序列化、Redis缓存使用、MongoDB使用。链接如下：[Github.C#.MVC](https://github.com/BLYang7/MVCTest)

ASP.NET的MVC框架和其他框架一致，也是包含Model、View和Controller三部分组成。在网站访问的时候，首先用户能获取到控制器，控制器根据用户行为创建Model对象，在数据填充到Model之后，再传递到View显示层。如下如示
![](/images/pages/framework/csharpmvc.png)

#### Controller
Controller是用来处理用户交互逻辑的，页面上用户的每个动作都是以http请求的形式出现在服务端（注意web编程中是没有事件的概念的，交互都是http请求），而控制器就是处理这些请求，并给出响应的。Model规定了响应的数据格式，而View则集中处理了数据展示形式。

Controller的定义需要注意一下，一定是定义一个控制器，并且这个控制器的命名要以controller结束，一定不要写成了cs文件。Controller处理用户请求并作出响应之后，需要显示在页面上，也就是View中。

#### View
View就是用户视图了。这里可以是aspx文件，或者是cshtml文件，甚至是简单的html文件，只要能获取到数据就可以。View获取的是Controller中的Model数据，然后将这些数据展示出来。

#### Model
Model 只是规定了数据格式，预先定义数据字段。而实际的数据填充，则是在Controller实现的。

#### 一个例子
讲完了这三者的定义，这里给一个例子。C# MVC框架的实现。这个实现中设计Model定义，对象序列化和反序列化，Redis缓存的使用，MongoDB的使用。

* 在VS中新建一个ＭＶＣ架构的ASP.NET Web应用程序
* 新建一个TestController控制器
* 在TestController下新建方法，比如说GetStr()，于是在浏览器中的访问路径为http://localhost:XXXX/Test/GetStr
* 新建一个Model，用于存取数据
* 在Common中新建几个帮助类，分别是JsonUtil.cs RedisHelper.cs MongoDBHelper.cs，分别用于JSON序列化和反序列化，Redis缓存，MongoDB数据库读写

这里不给代码了，有需要的请直接下载使用
