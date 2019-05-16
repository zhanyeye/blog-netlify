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
> 较好的命名方法 : 布局类型 _ 布局名称 _ 组件类型 _ 组件名称: `act_main_editText_username`

`findViewById()` : 基于ID名称获取组件



**Android中Callback的设计与使用 ? 理解掌握2种实现监听的方法?**

+ 匿名内部类、 lambda表达式 (set language level to 8)

  

`View.OnClickListener: onClick()`

```java
buttonSubmit = findViewById(R.id.act_main_button_submit);
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
editTextNameChange = findViewById(R.id.act_main_editText_change);

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





##### Example 04 App Resources

**各文件目录放置的资源?**

+ drawable : 图片相关的xml文件 (若图标有固定的尺寸，不需要更改，那么drawable更适合)
+ minmap :  图片相关的xml文件   (如果需要变大变小的，有动画的，放在mipmap中能有更高的质量)
+ Layout : 布局文件
+ value : 用于存放显示相的配置数据的定义文件，如strings.xml, style.xml, dimens.xml, arrays.xml, ids.xml等



**R.java的作用与意义 ?**

+ R.java文件自动生成，用来定义Android程序中所有各类型的资源的索引
+ 在java程序中引用资源 `R.resource_type.resource_name`
+ 在XML文件中引用资源 `@[package:]type/name`
  @drawable/icon
  @ 代表的是R.java类
  `drawable` 代表的是`R.java`中的静态内部类 `drawable`
  `/icon`代表静态内部类 `drawable` 中的静态属性 `icon`
  如果访问的是Android系统中自带的文件，则要添加包名“Android:” `android:textColor="@android:color/red"`
+ 往R.java文件中添加一条资源记录
  为组件添加Id属性作为标识:`@id+/name`



自定义资源文件，创建字符串数组资源，添加ListView，引入自定义字符串数组至ListView显示

```xml
自定义资源文件 mysource.xml

<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="courses">
        <item>C语言</item>
        <item>Java语言</item>
        <item>数据库原理</item>
        <item>计算机网络</item>
    </string-array>
</resources>

布局文件 activity_main.xml
<ListView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:entries="@array/courses"></ListView>
```

定义字符串数组资源，并在代码中通过R调用

```java
button = findViewById(R.id.act_main_button);
```



[Resources Overview](https://developer.android.google.cn/guide/topics/resources/overview.html)  

[Providing Resources](https://developer.android.google.cn/guide/topics/resources/providing-resources.html)  

[Accessing Resources](https://developer.android.google.cn/guide/topics/resources/accessing-resources.html)  

[Resource Types](https://developer.android.google.cn/guide/topics/resources/available-resources.html)





##### Example 05 Activities

###### Activities

> [`Activity`](https://developer.android.google.cn/guide/components/activities.html)是一个应用组件，用户可与其提供的屏幕进行交互（相当于一个页面）
> 每个 Activity 都会获得一个用于绘制其用户界面的窗口。
> 窗口通常会充满屏幕，但也可小于屏幕并浮动在其他窗口之上

> 一个应用通常由多个彼此松散联系的 Activity 组成。 一般会指定应用中的某个 Activity 为“主”Activity，即首次启动应用时呈现给用户的那个 Activity。 而且每个 Activity 均可启动另一个 Activity，以便执行不同的操作。 每次新 Activity 启动时，前一 Activity 便会停止，但系统会在堆栈（“返回栈”）中保留该 Activity。 当新 Activity 启动时，系统会将其推送到返回栈上，并取得用户焦点。 返回栈遵循基本的“后进先出”堆栈机制，因此，当用户完成当前 Activity 并按“返回”按钮时，系统会从堆栈中将其弹出（并销毁），然后恢复前一 Activity。



**Declare Activities**

使用activity 需要在 `manifest` 配置中声明: 在 `<application>` 添加1个 `<activity>` 元素

```
<manifest ... >
  <application ... >
      <activity android:name=".ExampleActivity" />
      ...
  </application ... >
  ...
</manifest >
```

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [onCreate()](https://developer.android.google.cn/reference/android/app/Activity.html#onCreate(android.os.Bundle)) | 系统会在创建 Activity 时调用此方法. <br />实现内初始化 Activity 的数据 .<br />必须在 [setContentView()](https://developer.android.google.cn/reference/android/app/Activity.html#setContentView(android.view.View)) 中定义 Activity 所使用的的layout文件.<br />onCreate() 完成后 下一步 就是 onStart(). |
| [onStart()](https://developer.android.google.cn/reference/android/app/Activity.html#onStart()) | As `onCreate()` exits, the activity enters the Started state.<br />the activity becomes visible to the user.<br />This callback contains the activity’s final preparations for coming to the foreground and becoming interactive. |
| [onPause()](https://developer.android.google.cn/reference/android/app/Activity.html#onPause()) | when the activity loses focus and enters a Paused state.<br />the activity will soon enter the **Stopped** or **Resumed** state.<br />may continue to update the UI if the user is expecting the UI to update (a media player playing). |
| [onStop()](https://developer.android.google.cn/reference/android/app/Activity.html#onStop()) | when the activity is no longer visible to the user<br />may happen because the activity is being destroyedd, a new activity is starting, or an existing activity is entering a Resumed state and is covering the stopped activity<br />next callback that the system calls is either `onRestart()`  or  `onDestroy()` |
| [onRestart()](https://developer.android.google.cn/reference/android/app/Activity.html#onRestart()) | when an activity in the Stopped state is about to restart<br />restores(恢复) the state of the activity from the time that it was stopped<br />This callback is always followed by `onStart()` |
| [onDestroy()](https://developer.android.google.cn/reference/android/app/Activity.html#onDestroy()) | invokes this callback before an activity is destroyed<br />ensure that all of an activity’s resources are released |



###### Common [Intents]((https://developer.android.google.cn/guide/components/intents-common.html) )

Intent在Android中的核心作用就是“跳转”,同时可以携带必要的信息，将Intent作为一个信息桥梁

1. explicit intent : 显示跳转到下一个活动

   ```java
   Intent intent = new Intent(this, SecondActivity.class);
   startActivity(intent);   //startActivity方法
   ```

2. implicit intent : specifies an action that can invoke any app on the device able to perform the action

   ```java
   // Create the text message with a string
   Intent sendIntent = new Intent();
   sendIntent.setAction(Intent.ACTION_SEND);
   sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
   sendIntent.setType("text/plain");
   
   // Verify that the intent will resolve to an activity
   if (sendIntent.resolveActivity(getPackageManager()) != null) {
       startActivity(sendIntent);
   }
   ```

3. 传递数据 : `intent.putExtra(key, value)` 上一个活动向下一个活动传递数据

   ```java
   //在第一个页面将数据装进Intent
   intent.putExtra("extra_data", "dafadfadfa");   //键值对
//在第二个页面拿到数据
   getIntent().getStringExtra("extra_data");
   ```
   
   

**Starting an Activity**
内容：Activity的切换方法；切换时前后Activity经历的生命周期过程；通过Intent在跳转切换时传递参数；Bundle；
要求：实现全部生命周期回调函数，在跳转时观察activity的状态，传递并接收参数。

**知识点1**：[实现`OnClickListener`接口的三种方法](https://blog.csdn.net/markshz/article/details/80762818)

1. 创建内部类
   
   > 创建一个内部类实现OnClickListener接口并重写onClick方法
2. 主类中实现OnClickListener接口: 
   
   > 在主类中实现OnClickListener接口并重写onClick方法
3. 匿名内部类:
   
   > 当按钮较少或只有一个按钮时，可以直接创建OnClickListener的匿名内部类传入按钮的setOnClickListener参数中
   
   

**知识点2**：[super.onCreate(savedInstanceState)](https://www.cnblogs.com/dazuihou/p/3565647.html)

1. onCreate()方法其实是覆写了基类（Activity类）的onCreate方法，super.onCreate()是在调用基类中的onCreate方法。

   > 而在子类的onCreate方法中，不能直接调用onCreate(),因为这样做会产生递归，为了解决这一问题，java用super关键字表示超类的意思，当前类就是从超类继承而来的

2. savedInstanceState是保存当前Activity的状态信息

   > 如果一个非running的Activity因为资源紧张而被系统销毁，当再次启动这个Activity时，可以通过这个保存下来的状态实例，即通过saveInstanceState获取之前的信息，然后使用这些信息，让用户感觉和之前的界面一模一样，提升用户体验。




```java
//MainActivity类中实现OnClickListener接口，并重写 OnClick() 方法
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private static final String TAG = "MainActivity";

    private Button button;
    private EditText editText;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState); //保存当前Activity的状态信息
        Log.i(TAG, "onCreate()");
        setContentView(R.layout.activity_main);  //放置UI布局
        button = findViewById(R.id.act_main_button);
        editText = findViewById(R.id.act_main_edittext);

        button.setOnClickListener(this); //为什么用this???
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.act_main_button:
                Intent intent = new Intent(this, SecActivity.class);
                intent.putExtra("value", editText.getText().toString()); //以键值对传值
                startActivity(intent);
        }
    }
}

```

```java
public class SecActivity extends AppCompatActivity {
    private static final String TAG = "SecActivity";

    private TextView textView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sec);
        Log.i(TAG, "SecActivity onCreate()");
        String value = getIntent().getStringExtra("value");
        textView = findViewById(R.id.act_sec_textview);
        textView.setText(value);
    }

    @Override
    protected void onStart() {
        super.onStart();
        Log.i(TAG, "SecActivity onStart()");
        // The activity is about to become visible.
    }
    @Override
    protected void onResume() {
        super.onResume();
        Log.i(TAG, "SecActivity onResume()");
        // The activity has become visible (it is now "resumed").
    }
    @Override
    protected void onPause() {
        Log.i(TAG, "SecActivity onPause()");
        super.onPause();
        // Another activity is taking focus (this activity is about to be "paused").
    }
    @Override
    protected void onStop() {
        Log.i(TAG, "SecActivity onStop()");
        super.onStop();
        // The activity is no longer visible (it is now "stopped")
    }
    @Override
    protected void onDestroy() {
        Log.i(TAG, "SecActivity onDestroy()");
        super.onDestroy();
        // The activity is about to be destroyed.
    }

}

```





##### Example 06  RecyclerView

 