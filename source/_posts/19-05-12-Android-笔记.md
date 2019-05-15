---
title: Android 笔记
date: 2019-05-12 08:27:59
tags:
categories:
- 专业课
---





##### Example 01 UI

> dp，像素密度，设备屏幕尺寸无关的，描述控件间距离等
> sp，描述字体大小
> px，像素，相对的绝对单元，与CSS相似等，图片等



RelativeLayout: 相对布局
LinearLayout: 线性布局
ConstraintLayout: 约束布局



###### LinearLayout 主要属性

+ android:orientation，LinearLayout方向
+ android:layout_gravity，控件本身的显示位置。仅在LinearLayout内有效，受android:orientation属性影响
+ android:gravity，控件内内容的显示位置
+ android:layout_weight，比重，android:orientation相应方向的值需设为0dp



###### ConstraintLayout

它的出现主要是为了解决布局嵌套过多的问题 [link](https://www.jianshu.com/p/17ec9bd6ca8a)

添加依赖 : 在app/build.gradle文件中添加ConstraintLayout的依赖

```
implementation 'com.android.support.constraint:constraint-layout:1.1.3'
```





##### Example 02 Common Widgets

android:id属性，声明组件ID; 后端可以通过ID值获取组件对象

+ @+id，创建一个新ID
+ @id，引用一个ID
+ @，引用资源
+ @android:，引用android下自带资源

```xml
<TextView
    android:id="@+id/textView1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="基本文本显示" />

<ImageView
    android:id="@+id/imageView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:srcCompat="@android:drawable/ic_input_add" />
```

基本输入组件： Buttons；Text Fields；CheckBoxes；Radio Buttons；Toggle Buttons；ImageView；EditText

other: imageview; progressBar; Seekbar; RatingBar;





##### Example 03 UI-Events

> 后端仅基于ID名称获取组件，无法基于不同布局文件区分组件ID，也无法区分组件类型。
> 调用到不是当前布局上组件，运行时才能发现错误。 
> 因此，ID的名称必须能够体现具体的完整的信息，从而避免出错。 
> 较好的命名方法 : 布局类型 _ 布局名称 _ 组件类型 _ 组件名称

`findViewById()` : 基于ID名称获取组件



Android中Callback的设计与使用 ?
理解掌握2种实现监听的方法?

+ 匿名内部类、 lambda表达式 (set language level to 8)



`View.OnClickListener: onClick()`

```java
//匿名内部类
buttonSubmit.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        String string = editTextUserName.getText().toString();
        textViewUserName.setText(string);
    }
});

//lambda表达式
buttonSubmit.setOnClickListener(v -> {
    String string = editTextUserName.getText().toString();
    textViewUserName.setText(string);
});
```

`EditText: TextChangedListener `

```java
editTextNameChange.addTextChangedListener(new TextWatcher() {
    @Override
    public void beforeTextChanged(CharSequence s, int start, int count, int after) {

    }

    @Override
    public void onTextChanged(CharSequence s, int start, int before, int count) {
        textViewNamechange.setText(s);
    }

    @Override
    public void afterTextChanged(Editable s) {

    }
});
```

```java
View.OnLongClickListener: onLongClick()
View.OnFocusChangeListener: onFocusChange()
View.OnTouchListener: onTouch()
View.OnKeyListener: onKey()
```



