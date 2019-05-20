---
title: Android 笔记
date: 2019-05-12 08:27:59
tags:
categories:
- 专业课
---





在最前面：
在AS中新建project 时，请勾上：

+ This project will support instant apps
+ use androidx.* artifacts 避免版本依赖的问题



##### Example 01 UI

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



###### ConstraintLayout

它的出现主要是为了解决布局嵌套过多的问题 [link](https://www.jianshu.com/p/17ec9bd6ca8a)

添加依赖 : 在app/build.gradle文件中添加ConstraintLayout的依赖

```
implementation 'com.android.support.constraint:constraint-layout:1.1.3'
```



----



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



-----



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

`View.OnLongClickListener: onLongClick()`
`View.OnFocusChangeListener: onFocusChange()`
`View.OnTouchListener: onTouch()`
`View.OnKeyListener: onKey()`



----



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



---



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

| 回调方法    | 特点                                                         |
| ----------- | ------------------------------------------------------------ |
| onCreate()  | 系统会在创建 Activity 时调用此方法. <br />实现内初始化 Activity 的数据 .<br />必须在 setContentView() 中定义 Activity 所使用的的layout文件.<br />onCreate() 完成后 下一步 就是 onStart(). |
| onStart()   | As `onCreate()` exits, the activity enters the Started state.<br />the activity becomes visible to the user.<br />This callback contains the activity’s final preparations for coming to the foreground and becoming interactive. |
| onPause()   | when the activity loses focus and enters a Paused state.<br />the activity will soon enter the **Stopped** or **Resumed** state.<br />may continue to update the UI if the user is expecting the UI to update (a media player playing). |
| onStop()    | when the activity is no longer visible to the user<br />may happen because the activity is being destroyedd, a new activity is starting, or an existing activity is entering a Resumed state and is covering the stopped activity<br />next callback that the system calls is either `onRestart()`  or  `onDestroy()` |
| onRestart() | when an activity in the Stopped state is about to restart<br />restores(恢复) the state of the activity from the time that it was stopped<br />This callback is always followed by `onStart()` |
| onDestroy() | invokes this callback before an activity is destroyed<br />ensure that all of an activity’s resources are released |



###### Common [Intents](https://developer.android.google.cn/guide/components/intents-common.html)

Intent在Android中的核心作用就是“跳转”,同时可以携带必要的信息，将Intent作为一个信息桥梁

1. explicit intent : 显示跳转到下一个活动

   ```java
   Intent intent = new Intent(this, SecondActivity.class);  //上下文环境； 目的Activity
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



----



##### Example 06  [RecyclerView](https://www.jianshu.com/p/4f9591291365)



> The views in the list are represented by *view holder*(视图持有者) objects. These objects are instances of a class you define by extending `RecyclerView.ViewHolder`. Each view holder is in charge of displaying a single item with a view. 
> The `RecyclerView`creates only as many view holders as are needed to display the on-screen portion of the dynamic content, plus a few extra. As the user scrolls through the list, the `RecyclerView` takes the off-screen views and rebinds them to the data which is scrolling onto the screen.

配置

在对应的 `build.gradle`   文件中dependencies加上   

```
implementation 'androidx.recyclerview:recyclerview:1.0.0'
```

布局

Activity布局文件 activity_main.xml
RecyclerView中item布局文件 item_1.xml
	news_pic; news_title; news_subtitle



创建实体类news封装数据  

> 实体类属性 public ?     :谷歌虚拟机没有使用内联，减少损耗   故不建议使用 get/set ， 直接public



创建自定义adapter

+ 在adapter中创建viewholder (在Adapter中创建一个**继承RecyclerView.ViewHolder**的静态内部类，记为VH)
+ adapter继承RecyclerView.Adapter，并声明VH泛型为自定义的VH类型
+ 在**Adapter中实现3个方法**
  
  1. **onCreateViewHolder()**
  
     > 这个方法主要**生成**为**每个Item [inflater](https://www.jianshu.com/p/cdc9d4c0826e?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)出一个View**，但是该方法**返回**的是一个**ViewHolder**。该方法把View直接封装在ViewHolder中，然后我们面向的是ViewHolder这个实例，当然这个ViewHolder需要我们自己去编写
  
  2. **onBindViewHolder()**
  
     > 这个方法主要用于**适配渲染数据到View**中。方法**提供**给你了一**viewHolder**而不是原来的convertView
  
  3. **getItemCount()**
  
     > 这个方法就**类似**于BaseAdapter的**getCount**方法了，即总共有多少个条目。



重写getItemCount()方法返回集合元素数量，即需要渲染的item数量  
重写onBindViewHolder()方法，当视图item滚动，绑定对应数据到item中的相应控件  
重写onCreateViewHolder()方法，声明item布局样式，并将view item对象，交由viewholder持有  
RecyclerView默认不包含点击事件及点击动画，需手动实现  

```java
public class MainAdapter extends RecyclerView.Adapter<MainAdapter.MyViewHolder> {
    private static final String TAG = "MainAdapter";
    private List<News> news;

    public MainAdapter(List<News> news) {
        this.news = news;
    }

    /**
     * 基于指定样式，渲染item，并将item对象绑定到viewholder
     * 当回收缓存区没有VH时，回调
     * 基于布局创建item对象，创建VH封装item对象，返回VH以复用
     * 因此，仅会回调有限次数。不同系统版本，不同屏幕尺寸等等均不同
     *
     * @param parent
     * @param viewType
     * @return
     */
    @NonNull
    @Override
    public MyViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        Log.i(TAG, "onCreateViewHolder");
        // 每一个item对象
		//将一个xml布局文件转换成一个View
        View itemView = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.recyclerview_news, parent, false);
        // 由VM持有
        return new MyViewHolder(itemView);
    }

    /**
     * 当渲染指定位置item时，从回收缓存区注入一个相同布局类型的VH，以及item的位置
     * 从vm中获取持有的控件对象，将指定集合位置的数据加载到控件，渲染
     *
     * @param holder
     * @param position
     */
    @Override
    public void onBindViewHolder(@NonNull MyViewHolder holder, int position) {
        Log.i(TAG, "onBindViewHolder: " + position + "/" + news.get(position).title);
        holder.title.setText(news.get(position).title);
        holder.suttitle.setText(news.get(position).subtitle);
        // 模拟图片
        holder.pic.setImageResource(R.mipmap.ic_launcher);
    }

    // 必须指定初始化时item数量，后期可动态改变
    @Override
    public int getItemCount() {
        return news.size();
    }

    /**
     * VH的作用是保持item view上的控件对象的引用
     * 避免在复用item上反复findViewById
     */
    static class MyViewHolder extends RecyclerView.ViewHolder {
        TextView title;
        TextView suttitle;
        ImageView pic;

        public MyViewHolder(@NonNull View itemView) {
            super(itemView);
            title = itemView.findViewById(R.id.news_title);
            suttitle = itemView.findViewById(R.id.news_subtitle);
            pic = itemView.findViewById(R.id.news_pic);
        }
    }
}

```



RecyclerView提供了**三种布局管理器**：
- **LinerLayoutManager** 以**垂直**或者**水平列表**方式展示Item
- **GridLayoutManager** 以**网格**方式展示Item
- **StaggeredGridLayoutManager** 以**瀑布流**方式展示Item

```java
public class MainActivity extends AppCompatActivity {
    private RecyclerView recyclerView;
    private MainAdapter adapter;
    private Button button;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        recyclerView = findViewById(R.id.act_main_recyclerview);
        // 指定一个默认的布局管理器
        RecyclerView.LayoutManager mLayoutManager = new LinearLayoutManager(this);
        recyclerView.setLayoutManager(mLayoutManager);
        // 指定item插入/移除动画
        // recyclerView.setItemAnimator(new DefaultItemAnimator());
        // 指定item分割线
        recyclerView.addItemDecoration(new DividerItemDecoration(this, LinearLayoutManager.VERTICAL));
        // 指定适配器
        adapter = new MainAdapter(listNews());
        recyclerView.setAdapter(adapter);

        button = findViewById(R.id.act_main_button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(MainActivity.this, SecActivity.class);
                startActivity(intent);
            }
        });
    }

    private List<News> listNews() {
        News n1 = new News(1, "阿根廷VS波黑", "小组赛F组 阿根廷VS波黑");
        News n2 = new News(2, "法国VS洪都拉斯", "小组赛E组 法国VS洪都拉斯");
        News n3 = new News(3, "瑞士VS厄瓜多尔", "小组赛E组 瑞士VS厄瓜多尔");
        News n4 = new News(4, "西班牙VS荷兰", "小组赛B组 西班牙VS荷兰");
        News n5 = new News(5, "俄罗斯VS丹麦", "小组赛A组 俄罗斯VS丹麦");
        News n6 = new News(6, "巴西VS意大利", "小组赛C组 巴西VS意大利");
        News n7 = new News(7, "日本VS伊朗", "小组赛D组 日本VS伊朗");
        List<News> news = new ArrayList<>();
        news.add(n1); news.add(n2); news.add(n3); news.add(n4);
        news.add(n5); news.add(n6); news.add(n7);
        news.add(n1); news.add(n2); news.add(n3); news.add(n4);
        return news;
    }
}

```



###### 需求OnItemClicklistener

+ 在item布局样式中添加点击波纹动画  
+ 在adapter onBindViewHolder()方法为item view设置点击监听  
+ 高级，自定义接口，实现在activity等的点击监听

```xml
//在RecyclerView.xml根元素加上属性：添加点击波纹动画  
android:background="?android:attr/selectableItemBackground"
android:clickable="true"
android:focusable="true"
```

```java
//在适配器中，自定义监听接口
public interface OnItemClickListener {
    void onItemClick(View view, int position, News news);
}

//onBindViewHolder()方法为item view设置点击监听 
@Override
public void onBindViewHolder(@NonNull ViewHolder holder, final int position) {
    holder.title.setText(news.get(position).title);
    holder.suttitle.setText(news.get(position).subtitle);
    holder.pic.setImageResource(R.mipmap.ic_launcher);
    holder.itemView.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            Toast.makeText(context, news.get(position).title, Toast.LENGTH_SHORT).show();
        }
    });
}
```

```java
//实现在activity等的点击监听
//在onCreate()中：
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Intent i = new Intent(SecActivity.this, ThirdActivity.class);
        startActivity(i);
    }
});
```



###### SwipeRefreshLayout
+ 添加下拉刷新功能，将RecyclerView嵌入SwipeRefreshLayout  
+ 通过Handler模拟耗时操作，添加元素  



```xml
//将RecyclerView嵌入SwipeRefreshLayout 
<androidx.swiperefreshlayout.widget.SwipeRefreshLayout
    android:id="@+id/act_third_swipe"
    android:layout_width="match_parent"
    android:layout_height="0dp"
    android:layout_weight="1">
    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/act_third_recyclerview"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</androidx.swiperefreshlayout.widget.SwipeRefreshLayout>
```



通过Handler模拟耗时操作，添加元素  

```java
swipe.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() {
    @Override
    public void onRefresh() {
        //模拟耗时操作，子线程禁止直接修改主线程控件属性
        new Handler().postDelayed(() -> {
            //取消刷新动画
            swipe.setRefreshing(false);
            news.add(0, new News(1, "阿根廷VS波黑" + news.size(), "小组赛F组 阿根廷VS波黑"));
                adapter.notifyDataSetChanged();
            }, 2000);
        }
});
```



----



##### Example 07 ItemTouchHelper

ItemTouchHelper，用来协助处理RecyclerView中item的移动/滑动/拖拽等操作  

> Callback内部类(非回调接口)，继承实现各种操作  
> 重写getMovementFlags()方法，决定拖拽滑动在哪个方向是被允许  
> 重写onSwiped()方法，Item横向滑动时回调，删除item  
> DefaultItemAnimator类定义recyclerView item操作动画  
> adapter添加/移除item必须通过notifyItemInserted()/notifyItemRemoved()方法，才有动画效果  
> 互交。重写onSwiped()方法可删除item，但无法删除数据，通过自定义回调接口实现  
> 依然通过SwipeRefreshLayout下拉刷新




ItemTouchHelper[使用步骤](https://blog.csdn.net/u014133119/article/details/80942932#commentBox)：

1. 创建 `MyCallback` 继承 `ItemTouchHelper.Callback` 

2. 把数据操作部分抽象成一个接口 `AdapterCallback`,这个接口可以写在 `MyCallback` 内

   > 因为`ItemTouchHelper`在完成触摸的各种动作后，就要对`Adapter`的数据进行操作，比如侧滑删除操作

3. 自定义 adapter 实现接口`MyCallback.AdapterCallback`, 重写相关方法

4. 在`MainActivity`中

   ```java
   ItemTouchHelper helper = new ItemTouchHelper(new MyCallback(adapter));
   helper.attachToRecyclerView(recyclerView);
   ```

   

具体实现：

MyCallback 及内部接口 AdapterCallback

```java
//MyCallback 及内部接口 AdapterCallback
public class MyCallback extends ItemTouchHelper.Callback {
    private static final String TAG = "MyCallback";
    private AdapterCallback adapterCallback;

    public interface AdapterCallback { //数据操作接口
        void remove(int position);
    }

    public MyCallback(AdapterCallback adapterCallback) {
        this.adapterCallback = adapterCallback;
    }

    /**
     * 该方法返回一个整数，用来指定拖拽和滑动在哪个方向是被允许的。
     * 在其中使用makeMovementFlags(int dragFlags, int swipeFlags)返回，
     * 该方法第一个参数用来指定拖动，第二个参数用来指定滑动。
     * ItemTouchHelper.UP  //滑动拖拽向上方向
     * ItemTouchHelper.DOWN//向下
     * ItemTouchHelper.LEFT//向左
     * ItemTouchHelper.RIGHT//向右
     * ItemTouchHelper.START//依赖布局方向的水平开始方向
     * ItemTouchHelper.END//依赖布局方向的水平结束方向
     *
     * @param recyclerView
     * @param viewHolder
     * @return
     */
    @Override
    public int getMovementFlags(@NonNull RecyclerView recyclerView, @NonNull RecyclerView.ViewHolder viewHolder) {
        // Log.i(TAG, "" + "getMovementFlags");
        //允许上下的拖动
        int dragFlags = ItemTouchHelper.UP | ItemTouchHelper.DOWN;
        //只允许从右向左侧滑
        int swipeFlags = ItemTouchHelper.LEFT;
        return makeMovementFlags(0, swipeFlags);
    }

    /**
     * onMove方法是拖拽的回调，参数viewHolder是拖动的Item，target是拖动的目标位置的Item,
     * 该方法如果返回true表示切换了位置，反之返回false。
     *
     * @param recyclerView
     * @param viewHolder
     * @param target
     * @return
     */
    @Override
    public boolean onMove(@NonNull RecyclerView recyclerView, @NonNull RecyclerView.ViewHolder viewHolder, @NonNull RecyclerView.ViewHolder target) {

        return false;
    }

    /**
     * onSwiped方法为Item滑动回调，viewHolder为滑动的item，direction为滑动的方向。
     *
     * @param viewHolder
     * @param direction
     */
    @Override
    public void onSwiped(@NonNull RecyclerView.ViewHolder viewHolder, int direction) {
        switch (direction) {
            case ItemTouchHelper.LEFT:
                adapterCallback.remove(viewHolder.getAdapterPosition());
        }

    }
}

```

自定义适配器并实现操作接口

```java
//自定义适配器并实现操作接口
public class MainAdapter extends RecyclerView.Adapter<MainAdapter.MyViewHolder> implements MyCallback.AdapterCallback {
    private static final String TAG = "MainAdapter";
    private List<News> news;
    public MainAdapter(List<News> news) {
        this.news = news;
    }

    @NonNull
    @Override
    public MyViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {

        View itemView = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.recyclerview_news, parent, false);
        return new MyViewHolder(itemView);
    }

    @Override
    public void onBindViewHolder(@NonNull MyViewHolder holder, int position) {
        holder.title.setText(news.get(position).title);
        holder.suttitle.setText(news.get(position).subtitle);
        holder.pic.setImageResource(R.mipmap.ic_launcher);
    }

    @Override
    public int getItemCount() {
        return news.size();
    }

    @Override
    public void remove(int position) {
        news.remove(position);
        // 指定通知可提高渲染效率，同时支持动画
        notifyItemRemoved(position);
    }

    static class MyViewHolder extends RecyclerView.ViewHolder {
        TextView title;
        TextView suttitle;
        ImageView pic;

        public MyViewHolder(@NonNull View itemView) {
            super(itemView);
            title = itemView.findViewById(R.id.news_title);
            suttitle = itemView.findViewById(R.id.news_subtitle);
            pic = itemView.findViewById(R.id.news_pic);
        }
    }
}

```

MainActivity配置

```java
public class MainActivity extends AppCompatActivity {
    private static final String TAG = "MainActivity";
    private RecyclerView recyclerView;
    private MainAdapter adapter;
    private SwipeRefreshLayout swipe;
    private List<News> news = listNews();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        recyclerView = findViewById(R.id.act_main_recyclerview);
        // 指定一个默认的布局管理器
        RecyclerView.LayoutManager mLayoutManager = new LinearLayoutManager(this);
        recyclerView.setLayoutManager(mLayoutManager);
        // 指定item插入/移除动画
        DefaultItemAnimator animator = new DefaultItemAnimator();
        // 包含默认的操作动画事件，也可自定义动画时间
        animator.setRemoveDuration(500);
        animator.setMoveDuration(500);
        recyclerView.setItemAnimator(animator);
        // 指定item分割线
        recyclerView.addItemDecoration(new DividerItemDecoration(this, LinearLayoutManager.VERTICAL));
        // 指定适配器
        adapter = new MainAdapter(news);
        recyclerView.setAdapter(adapter);

        ItemTouchHelper helper = new ItemTouchHelper(new MyCallback(adapter));
        helper.attachToRecyclerView(recyclerView);

        swipe = findViewById(R.id.act_third_swipe);
        swipe.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() {
            @Override
            public void onRefresh() {
                //模拟耗时操作，子线程禁止直接修改主线程控件属性
                new Handler().postDelayed(() -> {
                    //取消刷新动画
                    swipe.setRefreshing(false);
                    news.add(0, new News(1, "阿根廷VS波黑" + news.size(), "小组赛F组 阿根廷VS波黑"));
                    // 指定通知可提高渲染效率，同时支持动画
                    adapter.notifyItemInserted(0);
                    recyclerView.scrollToPosition(0);
                }, 500);
            }
        });
    }

    private List<News> listNews() {
        List<News> news = new ArrayList<>();
        News n1 = new News(1, "阿根廷VS波黑", "小组赛F组 阿根廷VS波黑");
        News n2 = new News(2, "法国VS洪都拉斯", "小组赛E组 法国VS洪都拉斯");
        News n3 = new News(3, "瑞士VS厄瓜多尔", "小组赛E组 瑞士VS厄瓜多尔");
        news.add(n1); news.add(n2); news.add(n3);
        return news;
    }
}
```



-----



##### Example 08 Appbar & Toolbar & Menu

Appbar & Toolbar

> 预使用功能丰富的appbar/toolbar，需先关闭android自带的title/actionbar  
> 自定义无title/actionbar样式的主题  
> 在AndroidManifest配置中引入自定义主题  
> 自定义独立的appbar布局文件，注意命名空间  
> 在activity layout中引入(类似JSP的include)  
> 在activity中获取toolbar对象，可动态修改各种属性  
> 动态置于ActionBar，开启左箭头(可选)等等  

res/values/styles.xml 中

```xml
 <!-- 自定义无title/actionbar样式 -->
<style name="AppTheme.NoActionBar">
    <item name="windowNoTitle">true</item>
    <item name="windowActionBar">false</item>
</style>
```

在AndroidManifest配置中引入自定义主题: 

`android:theme="@style/AppTheme.NoActionBar">`

自定义独立的appbar布局文件，注意命名空间

```xml
---->> appbar.xml
<?xml version="1.0" encoding="utf-8"?>
<com.google.android.material.appbar.AppBarLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <androidx.appcompat.widget.Toolbar

        android:id="@+id/my_toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
        app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"></androidx.appcompat.widget.Toolbar>

</com.google.android.material.appbar.AppBarLayout>
```

在activity layout中引入(类似JSP的include)

```xml
---->> activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <include layout="@layout/appbar"></include>

</LinearLayout>
```

在activity中获取toolbar对象，可动态修改各种属性  
动态置于ActionBar，开启左箭头(可选)等等  

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Toolbar toolbar = findViewById(R.id.my_toolbar);
        // 动态修改toolbar属性
        toolbar.setLogo(R.mipmap.ic_launcher);
        toolbar.setTitle("标题");
        setSupportActionBar(toolbar);
        // 显示左箭头
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
    }

    /**
     * 重写，加载menu布局
     * @param menu
     * @return
     */
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu, menu);
        return true;
    }

    /**
     * 重写，监听menu点击事件
     * @param item
     * @return
     */
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        String msg = "";
        switch (item.getItemId()) {
            case R.id.menu_add:
                msg = "add";
                break;
            // 点击左箭头，返回，即关闭当前activity
            case android.R.id.home:
                msg = "home";
                finish();
            case R.id.menu_send:
                msg = "send";
                break;
            case R.id.menu_edit:
                msg = "edit";
                break;
            case R.id.menu_del:
                msg = "delete";
                break;
        }
        Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
        return super.onOptionsItemSelected(item);
    }
}
```



-------



##### Example 09 Navigation & BottomNavigationView

###### Fragment

> 在module gradle配置引入navigation-fragment，navigation-ui依赖  
> (不直接声明依赖，创建导航视图文件时也可自动引入，但AS会卡住假死)  
> 创建多个Fragment及布局  
> 创建navigation资源目录，创建nav_graph导航视图文件  
> 在导航视图中引入fragment，声明导航规则  
> (也可设置fragment的独立global action)  
> 修改activity_main布局，添加NavHostFragment容器，，引用导航视图，声明必须属性   



引入依赖

```
def nav_version = "2.0.0"
implementation "androidx.navigation:navigation-fragment:$nav_version"
implementation "androidx.navigation:navigation-ui:$nav_version"
```

创建多个Fragment及布局

> new -> fragment -> 勾选 Create Layout XML; 下面的2个include 选项不要勾选 (在创建 fragment 类的同时 创建布局文件)

```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/foodFragment">
    <fragment
        android:id="@+id/foodFragment"
        android:name="com.example.example09.FoodFragment"
        android:label="fragment_food"
        tools:layout="@layout/fragment_food" >
        <action
            android:id="@+id/action_foodFragment_to_foodDetailFragment"
            app:destination="@id/foodDetailFragment" />
    </fragment>
    <fragment
        android:id="@+id/foodDetailFragment"
        android:name="com.example.example09.FoodDetailFragment"
        android:label="fragment_food_detail"
        tools:layout="@layout/fragment_food_detail" />
    ...
    ...
</navigation>
```

```java
public class FoodFragment extends Fragment {
    private static final String TAG = "FoodFragment";

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_food, container, false);
    }
    @Override
    public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);
        Button button = view.findViewById(R.id.frag_food_detail_button);
        button.setOnClickListener(v -> {
            Navigation.findNavController(v).navigate(R.id.action_foodFragment_to_foodDetailFragment);
        });
    }
}
```



创建navigation资源目录，创建nav_graph导航视图文件

> 在导航视图中引入fragment，声明导航规则



在导航视图中引入fragment，声明导航规则

```xml
<fragment
    android:id="@+id/my_nav_host_fragment"
    android:name="androidx.navigation.fragment.NavHostFragment"
    android:layout_width="match_parent"
    android:layout_height="0dp"
    android:layout_weight="1"
    app:defaultNavHost="true"
    app:navGraph="@navigation/nav_graph" />
```

###### BottomNavigationView

> com.google.android.material.bottomnavigation.BottomNavigationView  
> (自动包含选中状态样式：图标加亮+显示字体，区别未选中其他item)  
> 创建menu文件，声明Navigation所需item，图标及文字，绑定Navigation中的fragment  
> 在activity布局声明引入底部导航BottomNavigationView控件，引用menu  
> 在activity中获取NavController对象及并绑定BottomNavigationView对象



创建menu文件，声明Navigation所需item，图标及文字，绑定Navigation中的fragment

```xml
------->> menu_bottom_nav.xml

<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@id/foodFragment"
        android:title="Food"
        android:icon="@drawable/food_u" />
    <item android:id="@id/hotelFragment"
        android:title="Hotel"
        android:icon="@drawable/hotel_u" />
    <item android:id="@id/mapFragment"
        android:title="Map"
        android:icon="@drawable/ic_loc_in_map_u" />
    <item android:id="@+id/menu_bottom_list"
        android:title="List"
        android:icon="@drawable/main_index_my_pressed" />
</menu>
```

在activity布局声明引入底部导航BottomNavigationView控件，引用menu

```xml
<com.google.android.material.bottomnavigation.BottomNavigationView
    android:id="@+id/bottom_nav"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:menu="@menu/menu_bottom_nav" />
```

在Activity类中获取NavController对象及并绑定BottomNavigationView对象

```java
public class MainActivity extends AppCompatActivity implements NavController.OnDestinationChangedListener {
    private static final String TAG = "MainActivity";

    private NavController controller;
    private BottomNavigationView bottomNavigationView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        bottomNavigationView = findViewById(R.id.bottom_nav);
        controller = Navigation.findNavController(this, R.id.my_nav_host_fragment);
        // 监听导航切换事件
        controller.addOnDestinationChangedListener(this);
        NavigationUI.setupWithNavController(bottomNavigationView, controller);
    }
    @Override
    public void onDestinationChanged(@NonNull NavController controller, @NonNull NavDestination destination, @Nullable Bundle arguments) {
        switch (destination.getId()) {
            case R.id.foodDetailFragment:
                bottomNavigationView.setVisibility(View.GONE);
                break;
            default:
                bottomNavigationView.setVisibility(View.VISIBLE);
        }
    }
}
```



--------



##### Example 10 DrawerLayout & NavigationView

checkable 

> 基于DrawerLayout创建抽屉布局，从内向外逐层构建  
> 引入com.google.android.material:material:1.0.0依赖  
> 创建基本主内容布局，声明layout_behavior属性避免被appbar覆盖  
> 可声明使用showIn属性，增加预览效果  
> 创建appbar布局，声明toolbar，引入主内容布局(需声明使用noactionbar样式)  
> 基于menu创建抽屉导航选项  
> 基于DrawerLayout创建抽屉布局，引入appbar布局，NavigationView引用menu导航  
> 可选创建抽屉头部布局，并引入到NavigationView属性(自定义了头部背景样式)  
> Activity代码中添加actionbar  
> 监听navigationView OnNavigationItemSelectedListener  
> 任意item被点击，关闭抽屉  
> 抽屉与下导航，选一种作为app主布局即可  

引入依赖

```
implementation 'com.google.android.material:material:1.0.0'
```

创建基本主内容布局，声明layout_behavior属性避免被appbar覆盖

```xml
------->> content.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_behavior="com.google.android.material.appbar.AppBarLayout$ScrollingViewBehavior"
    tools:showIn="@layout/appbar">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="左拉抽屉；单activity开发，用navfragmenthost替换"
        android:textSize="40sp" />

</LinearLayout>
```

创建appbar布局，声明toolbar，引入主内容布局 (需在AndroidManifest.xml和style.xml声明使用AppTheme.NoActionBar样式)

```xml
------>> appbar.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    tools:showIn="@layout/activity_main">

    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <androidx.appcompat.widget.Toolbar
            android:id="@+id/my_toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
            app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar" />
    </com.google.android.material.appbar.AppBarLayout>

    <include layout="@layout/content" />
</LinearLayout>

```

基于menu创建抽屉导航选项

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <group android:checkableBehavior="single">
        <item
            android:id="@+id/nav_camera"
            android:icon="@android:drawable/ic_menu_camera"
            android:title="camera" />
        ...
        ...
    </group>

    <item android:title="Communicate">
        <menu>
            <item
                android:id="@+id/nav_share"
                android:icon="@android:drawable/ic_menu_share"
                android:title="share" />
            ...
            ...
        </menu>
    </item>

</menu>

```

基于DrawerLayout创建抽屉布局，引入appbar布局，NavigationView引用menu导航  

```xml
---->> menu_drawer.xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context=".MainActivity"
    tools:openDrawer="start">

    <include
        layout="@layout/appbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true"
        app:headerLayout="@layout/drawer_header"
        app:menu="@menu/menu_drawer" />

</androidx.drawerlayout.widget.DrawerLayout>

```

可选创建抽屉头部布局，并引入到NavigationView属性(自定义了头部背景样式)

```xml
---->> drawer_header.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="176dp"
    android:background="@drawable/side_nav_bar"
    android:gravity="bottom"
    android:orientation="vertical"
    android:padding="16dp">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingTop="8dp"
        app:srcCompat="@mipmap/ic_launcher_round" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginVertical="8dp"
        android:textSize="20sp"
        android:textStyle="bold"
        android:textColor="@color/colorAccent"
        android:text="主标题"/>

    <TextView
        android:id="@+id/textView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="副标题" />

</LinearLayout>
```

Activity代码中添加actionbar  
监听navigationView OnNavigationItemSelectedListener  
任意item被点击，关闭抽屉  

```java
public class MainActivity extends AppCompatActivity implements NavigationView.OnNavigationItemSelectedListener {
    private Toolbar toolbar;
    private DrawerLayout drawerLayout;
    private NavigationView navigationView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        toolbar = findViewById(R.id.my_toolbar);
        drawerLayout = findViewById(R.id.drawer_layout);
        navigationView = findViewById(R.id.nav_view);
        setSupportActionBar(toolbar);
        // 显示左箭头
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);

        navigationView.setNavigationItemSelectedListener(this);
    }

    @Override
    public boolean onNavigationItemSelected(@NonNull MenuItem menuItem) {
        switch (menuItem.getItemId()) {
            case R.id.nav_camera:
                Toast.makeText(this, "camera", Toast.LENGTH_SHORT).show();
                break;

        }
        drawerLayout.closeDrawer(GravityCompat.START); //任意item被点击，关闭抽屉  
        return true;
    }
}
```



---------



##### Example 11 SharedPreferences 接口

> Saving Key-Value Sets:  
> https://developer.android.google.cn/training/data-storage/shared-preferences  
> 基本的保存键值对数据的实现  
> 自定义application，暴露获取application对象的静态方法  
> 修改AndroidManifest配置启动自定义application  
> 创建SharedPreferences操作工具类  
> 编写输入输出布局  
> 在activity中将输入写入文件  
> 重新进入应用，文本输出控件，显式上次持久化的数据  

> SharedPreferences 需要上下文
> 使用： `getApplicationContext()` 拿



自定义application，暴露获取application对象的静态方法 

```java
public class MyApplication extends Application {
    private static MyApplication instance;
    public static MyApplication getInstance() {
        return instance;
    }
    @Override
    public void onCreate() {
        super.onCreate();
        instance = this;
    }
}
```

修改AndroidManifest配置启动自定义application  
`android:name=".util.MyApplication"`

创建SharedPreferences操作工具类

```java
/**
 * 有则直接使用，没有则创建名为为pre_my的XML配置文件
 * 位置data\data\packagename\shared_prefs
 * 声明使用范围
 */
public class SharedPreferencesUtils {
    private static SharedPreferences sf = create();
    private static final String PRE_FILE = "pre_my";
    private static final String MYEDIT = "myedit";

    /**
     * 封装SharedPreferences的构建过程，对外仅暴露允许修改的方法
     * @return
     */
    private static SharedPreferences create() {
        return MyApplication.getInstance()
                .getSharedPreferences(PRE_FILE, Context.MODE_PRIVATE);
    }

    public static void putMyedit (String myedit) {
        sf.edit().putString(MYEDIT, myedit).apply();
    }

    public static String getMyedit() {
        return sf.getString(MYEDIT, "");
    }

}

```

在activity中将输入写入文件

```java
// 在Main_Activity.java 中
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    editText = findViewById(R.id.act_main_edittext);
    textView = findViewById(R.id.act_main_textView);
    button = findViewById(R.id.act_main_button);
    // 获取值，没有默认值为空
    textView.setText(SharedPreferencesUtils.getMyedit());

    button.setOnClickListener(v -> {
        SharedPreferencesUtils.putMyedit(editText.getEditableText().toString());
    });
}
```

