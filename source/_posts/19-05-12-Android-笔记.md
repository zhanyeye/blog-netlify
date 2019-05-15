---
title: Android 笔记
date: 2019-05-12 08:27:59
tags:
categories:
- 专业课
---





###### Example 01 UI

> dp，像素密度，设备屏幕尺寸无关的，描述控件间距离等
> sp，描述字体大小
> px，像素，相对的绝对单元，与CSS相似等，图片等



RelativeLayout: 相对布局

LinearLayout: 线性布局

ConstraintLayout: 约束布局



LinearLayout 主要属性

+ android:orientation，LinearLayout方向
+ android:layout_gravity，控件本身的显示位置。仅在LinearLayout内有效，受android:orientation属性影响
+ android:gravity，控件内内容的显示位置
+ android:layout_weight，比重，android:orientation相应方向的值需设为0dp



ConstraintLayout

它的出现主要是为了解决布局嵌套过多的问题 [link](https://www.jianshu.com/p/17ec9bd6ca8a)

添加依赖 : 在app/build.gradle文件中添加ConstraintLayout的依赖

```
implementation 'com.android.support.constraint:constraint-layout:1.1.3'
```





###### Example 02 Common Widgets