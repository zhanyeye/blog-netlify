---
title: Android 笔记
date: 2019-05-12 08:27:59
tags:
categories:
- 专业课
---



#####  example01 UI

**Dimension:**

+ dp，像素密度，设备屏幕尺寸无关的，描述控件间距离等
+ sp，描述字体大小
+ px，像素，相对的绝对单元，与CSS相似等，图片等

**Layouts**

+ **Relative Layout & Linear Layout**
  + 理解Linear与Relative布局的区别；主要属性；相同属性与不同属性
  + android:orientation，LinearLayout 排列方向   vertical，horizontal
  + android:layout_gravity，控件本身的显示位置。仅在LinearLayout内有效，受android:orientation属性影响
    + center，bottom ...
  + android:gravity，控件内内容的显示位置。
  + android:layout_weight，比重，android:orientation相应方向的值需设为0dp。



+ **ConstraintLayout**

  + 与Relative的区别：仅对相同位置/方向进行相对的约束
  <https://developer.android.google.cn/training/constraint-layout/index.html>
  
  

**Tutorial**

<https://developer.android.com/guide/topics/ui>
<https://developer.android.google.cn/guide/topics/ui/declaring-layout.html>



##### example02 Common Widgets

**Input Controls:**
android:id属性，声明组件ID。后端可以通过ID值获取组件对象
@+id，创建一个新ID 
@id，引用一个ID
@，引用资源
@android:，引用android下自带资源

```xml
<ImageView
    android:id="@+id/imageView"
    android:layout_width="wrap_content"
    android:layout_gravity="center"
    android:layout_height="wrap_content"
    app:srcCompat="@android:drawable/sym_def_app_icon" />
```



基本输入组件： Buttons；Text Fields；CheckBoxes；Radio Buttons；Toggle Buttons；ImageView；EditText



Others:

imageview; progressBar; Seekbar; RatingBar; etc





##### example03 UI-Events

**IDs**

> 后端仅基于ID名称获取组件，无法基于不同布局文件区分组件ID，也无法区分组件类型。调用到不是当前布局上组件，运行时才能发现错误。 因此，ID的名称必须能够体现具体的完整的信息，从而避免出错。 

较好的命名方法，布局类型 _ 布局名称 _ 组件类型 _ 组件名称
findViewById()

> 当 Android 应用程序被编译，会自动生成一个 R 类，其中包含了所有 res/ 目录下资源的 ID，如布局文件，资源文件，图片（values下所有文件）的ID等。在写java代码需要用这些资源的时候，你可以使用 R 类，通过子类+资源名或者直接使用资源 ID 来访问资源。

**Events**

理解Callback模式，理解Android中Callback的设计与使用
理解掌握2种实现监听的方法
Input Events & Event Listeners

+ View.OnClickListener: `onClick()`

+ EditText: addTextChangedListener 

  > 用于监听edittext的输入文本的变化，他都用于密码框，或者那种检测用户输入过程中的变化。

+ View.OnLongClickListener: onLongClick()

+ View.OnFocusChangeListener: onFocusChange()

  > OnFocusChangeListener接口用来处理控件焦点发生改变的事件，如果注册了该接口，当某个控件失去焦点或者获得焦点的时候都会出发该接口中的回调方法.
  > 该接口对应的回调方法：public void onFocusChange(View v,Boolean hasFocus)

+ View.OnTouchListener: onTouch()

+ View.OnKeyListener: onKey()

**Examples**

内容：控件对象的获取，控件的相应事件监听器。
要求：接收EditText的输入，点击button后将输入结果显示在TextView，并使Button变为不可用状态。

```java
public class MainActivity extends AppCompatActivity {
    private EditText editTextUserName;
    private Button buttonSubmit;
    private TextView textViewUserName;
    private EditText editTextNameChange;
    private TextView textViewNamechange;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        editTextUserName = findViewById(R.id.act_main_editText_username);
        buttonSubmit = findViewById(R.id.act_main_button_submit);
        textViewUserName = findViewById(R.id.act_main_textView_username);
        editTextNameChange  =findViewById(R.id.act_main_editText_change);
        textViewNamechange = findViewById(R.id.act_main_textView_change);

        //为buttonSubmit设置一个onclick 监听器
        buttonSubmit.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String string = editTextUserName.getText().toString();
                textViewUserName.setText(string);
                buttonSubmit.setEnabled(false); //禁用button
            }
        });

        textViewUserName.setOnClickListener(v -> {
            Toast.makeText(MainActivity.this, "你好呀" + textViewUserName.getText().toString(), Toast.LENGTH_SHORT).show();
        });


        //监听editText的输入文本的变化
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

    }
}

```



##### example04 App Resources

内容：
Android工程结构，各文件目录放置的资源；
在相应资源文件中声明静态数据信息；
R.java的作用与意义；
自定义资源文件，创建字符串数组资源，添加ListView，引入自定义字符串数组至ListView显示
定义字符串数组资源，并在代码中通过R调用。
Resources Overview
<https://developer.android.google.cn/guide/topics/resources/overview.html>

Providing Resources
<https://developer.android.google.cn/guide/topics/resources/providing-resources.html>

Accessing Resources
<https://developer.android.google.cn/guide/topics/resources/accessing-resources.html>

Resource Types
<https://developer.android.google.cn/guide/topics/resources/available-resources.html>
Layout Resource
Menu Resource
String Resources
More Resource Types

   