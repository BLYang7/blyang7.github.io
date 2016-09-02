---
layout: post
title: Android资源访问
categories: Android开发

---

>在代码中访问资源对象使用的是Context的getResources()方法，得到Resources对象。引用的值是通过使用R资源类中定义的资源文件类型和资源文件名称组成

**在其他资源中引用资源文件的格式是：@[包名]资源类型/资源名称**

<br/>

#### 颜色资源（Color）

颜色的定义格式是通过RGB三原色和一个alpha值来确定的。#RGB、#ARGB、#RRGGBB、#AARRGGBB

在代码中的引用格式为：R.color.color_name，调用方法为：Resources.getColor() 
        
颜色资源的XML定义格式如下：

```
<?xml version="1.0" encoding="UTF-8"?>  
<resources>  
      
    <!-- 颜色资源子元素 -->  
    <color name="red">#ff0000</color>  
      
</resources>  
```

在代码中使用颜色资源的例子如下：

```
@Override  
public void onClick(View v) {  
    Resources res = getResources();  
    textview.setBackgroundColor(res.getColor(R.color.red));  
    Toast.makeText(getApplicationContext(), "这是第一个Activity的按键响应", Toast.LENGTH_LONG).show();  
}  
```

<br/>

#### 字符串资源（String）

代码中的引用格式是：R.string.string_name。获得字符串资源的方法是：Resources.getString() 

字符串资源的定义格式如下：

```
<?xml version="1.0" encoding="utf-8"?>  
<resources>  
  
    <!-- 字符串子元素 -->  
    <string name="app_name">Demo</string>  
  
</resources> 
```

在代码中调用字符串资源的格式如下：

```
String str = getString(R.string.hello_world).toString();  
textview.setText(str);  
```

<br/>

#### 尺寸资源（dimen）

一般尺寸资源的尺寸单位设置为dp，这是一个和密度无关的像素，是相对屏幕物理密度的抽象单位。
        
在代码中获取尺寸资源的方法是：getResources().getDimension() 在代码中的引用尺寸资源的格式是：R.dimen.dimen_name
        
尺寸资源的定义格式如下：

```
<?xml version="1.0" encoding="utf-8"?>  
<resources>  
  
    <!-- 尺寸资源子元素 -->  
    <dimen name="activity_horizontal_margin">16dp</dimen>  
  
</resources>  
```

在代码中获取资源文件的方式如下：

```
Resources res = getResources();  
float length = res.getDimension(R.dimen.activity_horizontal_margin);  
```

<br/>

#### 绘图资源（drawables）

drawable资源是一些图片或者颜色资源，主要用来绘制屏幕。drawable资源分为三类，分别是：位图文件（Bitmap File）、颜色（Color Drawable）、九片图片（Nine-Patch Image）

在代码中获得位图资源的方法是：Resources.getDrawable 。引用格式是R.drawable.file_name。
        
假设在布局文件中定义了一个ImageView视图，并且在drawable文件下放入了一个文件graph。则在代码中的引用方式如下：

```
Resources res = getResources();  
Drawable draw = res.getDrawable(R.drawable.graph);  
imageview.setImageDrawable(draw);
```

<br/>

#### 布局文件（layout）

定义方式如下：

```
<?xml version="1.0" encoding="UTF-8"?>  
  
<!-- 布局类 -->  
<!-- 布局属性 -->  
<!-- 视图组件或者是其他嵌套布局类 -->  
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    tools:context="com.example.demo.MainActivity" >  
  
    <TextView  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="@string/hello_world" />  
  
</RelativeLayout>  
```

<br/>

#### 菜单资源

Android菜单分为选项菜单，上下文菜单和子菜单，都可以在XML文件中声明定义，在代码中通过MenuInflater类使用。

具体的创建方式是定义<menu>根元素，在根元素里面嵌套<item>和<group>子元素。其中<item>是菜单项，包含在<menu>或者<group>中的有效属性。<group>是一个菜单组，相同的菜单组可以一起设置其属性。

先来说一下定义方式，如下代码中所示，定义一个菜单项，这个根菜单中包含三个项目，分别是File、Edit和Help菜单项，每个项目下又包含一个子菜单，File菜单有New、Open和Save三个子菜单项。Edit菜单有Cut、Copy和Paste菜单项，Help菜单有About和Exit菜单项。

```
<?xml version="1.0" encoding="utf-8"?>  
<!-- 创建一个menu根元素，它没有属性值，在内部包含item和group选项 -->  
<menu xmlns:android="http://schemas.android.com/apk/res/android">  
    <item   
        android:title="File"  
        android:icon="@drawable/file">  
        <menu>  
            <group   
                android:id="@+id/noncheckable_group"  
                android:checkableBehavior="none">  
                  
                <item   
                android:id="@+id/newFile"  
                android:title="New"   
                android:alphabeticShortcut="n"/>  
                  
                <item   
                android:id="@+id/openFile"  
                android:title="Open"   
                android:alphabeticShortcut="o"/>  
                  
                <item   
                android:id="@+id/saveFile"  
                android:title="Save"   
                android:alphabeticShortcut="s"/>  
            </group>  
        </menu>  
    </item>  
  
    <item android:title="Edit" android:icon="@drawable/edit">  
        <menu>  
            <group android:id="@+id/edit_group"  
                    android:checkableBehavior="single">  
                      
                <item android:id="@+id/cut"  
                      android:title="Cut" />  
                        
                <item android:id="@+id/copy"  
                      android:title="Copy"/>  
                        
                <item android:id="@+id/past"  
                      android:title="Past"/>  
            </group>  
        </menu>  
    </item>  
  
    <item android:title="Help" android:icon="@drawable/help">  
        <menu>  
            <group android:id="@+id/help_group">  
                <item android:id="@+id/about"  
                        android:title="About" />  
                <item android:id="@+id/exit"  
                        android:title="Exit" />  
            </group>  
        </menu>  
    </item>  
      
</menu>  
```

而菜单项在使用的时候，首先要在Activity类的顶部声明一个MenuInflater类，然后在onCreat()方法中实例化该对象，用来加载菜单XML文件。

接着覆盖Activity类的onCreateOptionsMenu()方法，调用MenuInflater的inflate方法，通过配置文件创建菜单。

第三步是覆盖onOptionsItemSelected()方法，响应菜单单击事件，作为例子，只写about选项和exit选项的事件响应。

```
public class TestMenuActivity extends Activity {  
    //声明MenuInflater对象  
    private MenuInflater mi;  
      
    //覆写onCreate()方法  
    @Override  
    public void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.test_menu);  
        mi = new MenuInflater(this);  
    }  
      
      
    // 创建菜单，这里创建的是选项菜单OptionMenu  
    @Override  
    public boolean onCreateOptionsMenu(Menu menu) {  
        mi.inflate(R.menu.file_menu, menu);  
        return true;  
    }  
      
    //覆写菜单选择响应器事件  
    @Override  
    public boolean onOptionsItemSelected(MenuItem item) {  
        switch (item.getItemId()) {  
        case R.id.about:  
            aboutAlert("本实例演示的是如何使用XML菜单资源来定义菜单！");  
            break;  
        case R.id.exit:  
            exitAlert("真的要退出吗？");  
            break;  
        }  
        return true;  
    }  
      
    // 显示对话框  
    private void exitAlert(String msg){  
    AlertDialog.Builder builder = new AlertDialog.Builder(this);  
    builder.setMessage(msg)  
           .setCancelable(false)  
           .setPositiveButton("确定", new DialogInterface.OnClickListener() {  
               public void onClick(DialogInterface dialog, int id) {  
                   finish();  
               }  
            })  
           .setNegativeButton("取消", new DialogInterface.OnClickListener() {  
               public void onClick(DialogInterface dialog, int id) {  
                   return;  
               }  
           });  
        AlertDialog alert = builder.create();  
        alert.show();  
    }  
      
    // 显示对话框  
    private void aboutAlert(String msg){  
    AlertDialog.Builder builder = new AlertDialog.Builder(this);  
    builder.setMessage(msg)  
           .setCancelable(false)  
           .setPositiveButton("确定", new DialogInterface.OnClickListener() {  
               public void onClick(DialogInterface dialog, int id) {  
               }  
           });  
    AlertDialog alert = builder.create();  
    alert.show();  
    }  
}  
```

