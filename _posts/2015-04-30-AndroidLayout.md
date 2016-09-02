---
layout: post
title: Android界面
categories: Android开发

---

>android常见的一些操作概述

### 消息对话框的创建

```
final AlertDialog.Builder builder = new AlertDialog.Builder(this);  
          
        myButton.setOnClickListener(new OnClickListener() {  
        public void onClick(View v) {  
            builder.setMessage("真的要删除该记录吗？")  
            .setPositiveButton("是", new DialogInterface.OnClickListener() {  
                public void onClick(DialogInterface dialog, int which) {  
                    myTV.setText("删除成功！");  
                }  
            })  
            .setNegativeButton("否", new DialogInterface.OnClickListener() {  
                public void onClick(DialogInterface dialog, int which) {  
                    myTV.setText("取消删除！");  
                }  
            });  
              
            AlertDialog ad = builder.create();  
            ad.show();  
        }  
    });  
```

<br/>

#### 菜单栏

Android提供了三种类型的菜单，分别是选项菜单、上下文菜单和子菜单。
        
* 选项菜单(Option Menu)：单击设备的Menu按键时弹出
* 上下文菜单(Context Menu)：在设备的界面上长时间按住不放时，弹出上下文菜单
* 子菜单(Sub Menu)：功能相同的分组多级显式的菜单

<br/>

#### 提示信息Toast

调用makeText方法添加显式文本和时长，调用show方法显示出来

<br/>

#### 事件监听

* 单击事件：OnClickListener
* 焦点事件：OnFocusChangeListener
* 按键事件：OnKeyListener
* 触碰事件：OnTouchListener
* 菜单选择事件：onOptionsItemSelected
* etc......

<br/>

#### 常见组件

文本框（TextView）、编辑框（EditText）、单选按钮（RadioButton）、复选按钮（CheckBox）、开关按钮（ToggleButton）、下拉列表（Spinner）、自动完成文本框（AutoCompleteTextView）、选项卡（Tab）、进度条（ProgressBar）、列表视图（ListView）、网格视图（GridView）、画廊视图（Gallery）、地图视图（MapView）、网络视图（WebView）





