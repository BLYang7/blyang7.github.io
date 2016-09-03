---
layout: post
title: Activity生命周期中各个方法的调用
categories: Android开发

---

> Activity生命周期

![](/images/pages/android/activity.png)


关于Activity的生命周期，要仔细理解Activity生命周期状态图

* 1）活动状态：处在Activity栈顶，屏幕当前的界面，有焦点并且可见
* 2）暂停状态：失去焦点，还可见
* 3）停止状态：失去焦点、不可见
* 4）销毁状态：被系统或者进程结束它

<br/>

写了一个小程序，分别调用各个Activity的方法，在日志中打印出来的结果如下：

**第一个Activity启动：**

* 08-14 14:01:27.005: I/tag(1433): 1-create
* 08-14 14:01:27.005: I/tag(1433): 1-start
* 08-14 14:01:27.069: I/tag(1433): 1-resume

<br/>

**从第一个Activity中启动第二个Activity：**

* 08-14 14:04:12.965: I/tag(1504): 1-pause
* 08-14 14:04:13.013: I/tag(1504): 2-create
* 08-14 14:04:13.033: I/tag(1504): 2-start
* 08-14 14:04:13.033: I/tag(1504): 2-resume
* 08-14 14:04:13.569: I/tag(1504): 1-stop

<br/>

**点击返回键从第二个Activity返回第一个Activity：**

* 08-14 14:05:13.009: I/tag(1504): 2-pause
* 08-14 14:05:13.029: I/tag(1504): 1-start
* 08-14 14:05:13.029: I/tag(1504): 1-resume
* 08-14 14:05:13.481: I/tag(1504): 2-stop
* 08-14 14:05:13.481: I/tag(1504): 2-destory

<br/>

而在生命周期图中有两个循环，循环中的几个分类如下：

* 1）Activity完整的生命周期：从第一次调用onCreate()开始直到调用onDestroy()结束。
* 2）Activity的可视生命周期：从调用onStart()到相应的调用onStop()。在这两个方法之间，可以保持显示Activity所需要的资源。如在onStart()中注册一个广播接收者监听影响你的UI的改变，在onStop()中注销。注意这里的可视也包含了透明状态，重点是这个阶段数据是暂时保存在内存中的。
* 3）Activity的前台生命周期：从调用onResume()到相应的调用onPause()，这一阶段是显示在前端界面的，用户可见的。





 