# 控件1 TextView

1. 基本布局

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical">
       <!--
   match_parent与linerLayout容器一样大
   warp_content与控件内部内容一致不大于LinearLayout容器
   #FF00FF00前两位表示透明度，后面依次表示红、绿、蓝三原色
   id/background/textColor可以去values文件夹下的xml中设置，再在这个文件中指向
   -->
       <TextView
           android:id="@+id/tv_one"
           android:layout_width="200dp"
           android:layout_height="200dp"
           android:textColor="@color/blue"
           android:textSize="30sp"
           android:gravity="center"
           android:text="@string/tv_one"
           android:background="@color/red"
           android:textStyle="bold" />
   
   </LinearLayout>
   ```

2. 阴影

   ```xml
   <!--
   		android:shadowColor="#FFFF0000" 颜色
           android:shadowRadius="3.0" 模糊度
           android:shadowDx="10.0" 向右偏移
           android:shadowDy="10.0" 向下偏移
   -->
   ```

3. 跑马灯效果

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical">
       <!--
           android:singleLine="true" 单行显示
           android:ellipsize="marquee" 在哪里少略文本
           android:focusable="true" 是否可以获取焦点
           android:focusableInTouchMode="true" 用于控制视图在触摸模式下是否可以聚集
           android:marqueeRepeatLimit="marquee_forever" 字幕动画重复次数
           <requestFocus /> 请求焦点
   -->
       <TextView
           android:id="@+id/tv_one"
           android:layout_width="match_parent"
           android:layout_height="200dp"
           android:ellipsize="marquee"
           android:focusable="true"
           android:focusableInTouchMode="true"
           android:gravity="center"
           android:marqueeRepeatLimit="marquee_forever"
           android:shadowColor="#FFFF0000"
           android:shadowDx="10.0"
           android:shadowDy="10.0"
           android:shadowRadius="3.0"
           android:singleLine="true"
           android:text="@string/tv_one"
           android:textColor="#FF000000"
           android:textSize="30sp"
           android:textStyle="italic">
   
           <requestFocus />
       </TextView>
   </LinearLayout>
   ```










```
先编写xml格式的界面布局，再编辑相应的Java文件
1. 用setContentView(R.layout.filename.xml)调用xml文件
2. res目录在存放了Android应用的全部资源，给其生成了清单R.java

```



## xml文件

例：strings.xml文件

```xml
<resources>
    <string name="app_name">Star</string>
</resources>
```

### 使用

```
1. @资源对应的内部类类名/资源项的名称 
	@string/app_name
2. @+id/name
```

### 在Java代码中获取该组件

```java
/**
  * 寻找我们需要的组件
  */
editUserName = findViewById(R.id.edit_username);
editPassword = findViewById(R.id.edit_password);
btnRegist = findViewById(R.id.btn_regist);
btnWJMM = findViewById(R.id.btn_updatepassword);
btnLogin = findViewById(R.id.btn_login);
```

### 在xml文件中获取该组件

```xml
@id/标识符号代号
```

在onCreate()方法外面输入logt然后按下Tab键，会自动以当前类名作为值自动生成一个TAG常量。

ctrl + R替换

# 1. 了解Android Studio的使用

四层架构：Linux内核层，系统运行层，应用框架层和应用层

四大组件：活动(Activity)，服务(Service)，广播接收器(Broadcast Receiver)和内容提供器(Content Provider)

## 新建项目

## 项目结构

左边栏切换到Project目录显示：

> * Project
>   * .grdle
>   * .idea
>   * app
>   * build
>   * gradle
>   * ...

主要关注app目录中的main中的文件（夹）

> * app
>   * build
>   * ...
>   * main
>     * java(Java代码)
>     * res(资源文件)
>     * AndroidManifest.xml(配置文件)

### res 目录

> * drawable(放图片)
> * layout(放布局文件)
> * mipmap*(放应用图片)
>   * xx*hdpi(不同分辨率)
> * values*(字符串，颜色，样式的配置)

1. 如何使用？

   例：

   > Java代码中通过R.string(mipmap)/_name
   >
   > xml代码中通过@string(mipmap)/_name

## 日志的使用

工具类：Log(android.util.Log)

5个方法打印：

* Log.v() ------verbose冗长的，琐碎日志
* Log.d() ------debug调试日志
* Log.i()  ------info信息日志
* Log.w() -----warn警告日志
* Log.e()  -----error错误日志

以上，从上到下，级别变高。

```java
public class SplashActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_splash);
        /*第一个参数tag，一般传当前类名，第二个参数msg，传入打印内容*/
        Log.e("SplashActivity", "onCreate execute");
    }
    /*按下logt会自动生成一个TAG常量*/
    private static final String TAG = "SplashActivity";
}
```

运行后可以在下面locat中看到打印信息。可以添加过滤器，以tag为过滤条件。也可以设置级别上限打印，比上限低的不能显示。

# 2. 基本活动的使用

大体来说活动主要是用户能直接看见的内容与操作，包括：

* xml布局文件(界面)

* Java控制文件(操作)
* AndroidManifest注册

## xml文件使用

`activity_first.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FirstActivity">
    <!--定义一个Button，取名button1-->
    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/button_1"
        android:text="button1"/>

</LinearLayout>
```



## Java控制文件使用

### 主要内容

```java
public class SplashActivity extends AppCompatActivity {

    /*所有活动都应该重写Activity的onCreate()方法，并调用父类中的方法*/
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        /*调用setContentView()方法来给当前的活动加载一个布局，项目中添加的何资源都会在R文件中生成一个相应的资源id*/
        setContentView(R.layout.activity_splash);
    }
}
```

### 点击事件与Toast

`FirstActivity.java`文件中

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_first);
        Log.e("SplashActivity", "onCreate execute");
        super.onCreate(savedInstanceState);
        /*调用setContentView()方法来给当前的活动加载一个布局，项目中添加的何资源都会在R文件中生成一个相应的资源id*/
        setContentView(R.layout.activity_first);
        /*在activity_first.xml中找到button_1*/
        Button button1 = (Button)findViewById(R.id.button_1);
        /*为其设置监听，如果按下就打印一个消息*/
        button1.setOnClickListener(new View.OnClickListener(){
            @Override
            public void onClick(View v){
                Toast.makeText(FirstActivity.this, "You clicked Button 1", Toast.LENGTH_SHORT).show();
            }
        });
    }
```

### 加右上角菜单

res--->menu--->文件下main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/add_item"
        android:title="Add" />
    <item
        android:id="@+id/remove_item"
        android:title="Remove" />
</menu>
```



`FirstActivity.java`文件中

```java
    /**
     *
     * @param menu 为当前活动在界面右上角创建菜单
     * @return true
     */
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }

    /**
     *
     * @param item 菜单响应事件
     * @return true
     */
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId())
        {
            /*点击打印消息*/
            case R.id.add_item:
                Toast.makeText(this, "You clicked Add", Toast.LENGTH_SHORT).show();
                finish();
                break;
            case R.id.remove_item:
                Toast.makeText(this, "You clicked Remove", Toast.LENGTH_SHORT).show();
                break;
            default:
                break;
        }
        return true;
    }
```

## AndroidManifest文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.chinabird.star">

    <!--icon指定应用图标，label指定应用名称与标题栏内容-->
    <application
        android:allowBackup="true"
        android:icon="@mipmap/logo"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <!--主活动-->
        <activity android:name=".SplashActivity"
            android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

## Intent跳转

### 显式Intent

```java
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);//从FirtstActivity跳到SecondActivity
startActivity(intent);//启动活动
```

跳转到的Activity要求在AndroidManifest.xml文件中注册。

### 隐式Intent

> 不指出是哪个活动，而用AndroidManifest文件中配置的内容来指定是否调用。主要由action 和category来决定。

在AndroidManifest.xml的Activity中设置

```xml
        <!--隐式intent-->
        <activity android:name=".SecondActivity">
            <intent-filter>
                <action android:name="com.chinabird.ACTION_START"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <category android:name="com.chinabird.MY_CATEGORY"/>
            </intent-filter>
        </activity>
```

在FirstActivity文件中设置

```java
Intent intent = new Intent("com.chinabird.ACTION_START");
/*自定义category*/
intent.addCategory("com.chinabird.MY_CATEGORY");
/*android.intent.category.DEFAULT会自动添加进去*/
startActivity(intent);
```

可以看到的时，

1. 配置好的action要传入Intent
2. category
   1. 是android.intent.category.DEFAULT时不需要add
   2. 不是默认的就要在startActivity前调用intent.addCategory("com.chinabird.MY_CATEGORY");

#### 更多隐式用法

> 可以实现其他程序的活动

### 向下一个活动传递数据

* 传递FirstActivity

```java
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);//从FirtstActivity跳到SecondActivity
                /*传递数据*/
String data = "Hello SecondActivity";
intent.putExtra("extra_data", data); //第一个参数是键，第二个才是数据
startActivity(intent);//启动活动
```

* 取出SecondActivity

```java
public class SecondActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);
        /*接收到传递的数据*/
        Intent intent = this.getIntent();
        String data = intent.getStringExtra("extra_data");
        Log.d("SecondActivity", data);
    }
}
```

### 返回数据给上一个活动

* 启动FirstActivity

```java
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);

/*启动有返回数据的，在活动销毁时返回给上一个结果*/
startActivityForResult(intent, 1);
```

* SecondActivity返回数据给FirstActivity

```java
public class SecondActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);
        Button button2 = (Button)findViewById(R.id.button_2);
        button2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                /*用于传递数据*/
                Intent intent = new Intent();
                intent.putExtra("data_return", "Hello FirstActivity");
                /*第一个参数返回处理结果，第二个参数把带有数据的Intent传递回去*/
                setResult(RESULT_OK, intent);
                finish();
                /*销毁后调用的是onActivityResult()方法*/
            }
        });
    }
}
```

在FirstActivit活动中重写onActivityResult()方法，即可得到返回的结果。

```java
    /**
     * 由startActivityForResult()方法来启动的活动finish()后调用此方法
     * @param requestCode starActivityForResult()启动时的请求码
     * @param resultCode 处理结果
     * @param data 带有数据的intent
     */
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        switch (requestCode) {
            case 1:
                if (resultCode == RESULT_OK) {
                    String returnedData = data.getStringExtra("data_return");
                    Log.d("FirstActivity", returnedData);
                }
                break;
            default:
                break;
        }
    }
```

## 活动生命周期

> Android是使用任力(Task)来管理活动的，一个任务就是一组存放在栈里的活动的集合，这个栈也被称为返回栈(Back Stack)
>
> 活动中栈的弹出是通过调用返回键或者finish()方法做到的。

### 活动状态

> 活动在生命周期中最多可能会有4种状态

1. 运行状态：活动位于栈顶
2. 暂停状态：活动不位于栈顶，但可见
3. 停止状态：活动不位于栈顶，且不可见
4. 销毁状态：活动弹出栈

### 活动的生存期

1. onCreate()：创建(完整生存期开始)
2. onStart()：开始(可见生存期开始)
3. onResume()：启动(前台生存期开始)
4. onPause()：暂停(前台生存期结束)
5. onStop()：停止(可见生存期结束)
6. onDestroy()：销毁(完整生存期结束)
7. onRestart()：重新开始(可见生存期重新开始)

![image-20210705092132351](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210705092132351.png)



### 对话框式活动

`AndroidManifest.xml`

```xml
        <activity android:name=".NormalActivity"></activity>
        <activity
            android:name=".DialogActivity"
            android:theme="@android:style/Theme.Dialog">
        </activity>
```

把DialogActivity设置为对话框式的。

`DialogActivity.java`

```java
public class DialogActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_dialog);
    }
}
```

对话框类在新的SDK中继承于Activitiy，不会报错。

### 防止活动被回收

==活动进行停止状态是有可能被系统回收的，再启动这个活动时数据就已丢失了。==

* 保存

使用`onSaveInstanceState()`方法。

```java
    /**
     * 保证在活动被回收之前一定会被调用，可以用来保存数据，防止其被回收
     * @param outState 提供一系列方法保存整型数据
     */
    @Override
    public void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        String tempData = "Something you just typed";
        outState.putString("data_key", tempData);
    }
```

* 恢复

`onCreate(Bundle savedInstanceState)`方法中savedInstanceState带有刚才保存的数据。

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if (savedInstanceState != null) {
            String tempData = savedInstanceState.getString("data_key");
            Toast.makeText(this, tempData, Toast.LENGTH_SHORT).show();
        }
    }
```

## 活动四种启动模式

> 根据需求为每个特定的活动指定恰当的启动模式

1. standard
2. singleTop
3. singleTask
4. singleInstance

在`AndroidManifest.xml`中通过给<activity>标签指定android:launchMode属性来选择启动模式。

### standard

默认启动模式，启动即入栈

### singleTop

如果多次进入同一个活动，`standard`只会一直入栈，使用`singleTop`模式，发现活动已在栈顶，就不再入栈。

`AndroidManifest.xml`

```xml
        <activity
            android:name=".FirstActivity"
            android:launchMode="singleTop">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```



### singleTask

如果活动不在栈顶，那边还是会将活动入栈，栈中还是没会有多个活动，使用`singleTask`模式，发现活动已在栈中，就不再入栈。

`AndroidManifest.xml`

```xml
        <activity
            android:name=".FirstActivity"
            android:launchMode="singleTask">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```

### singleInstance

`AndroidManifest.xml`

```xml
        <!-- 隐式intent -->
        <activity
            android:name=".SecondActivity"
            android:launchMode="singleInstance">
            <intent-filter>
                <action android:name="com.chinabird.ACTION_START" />

                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="com.chinabird.MY_CATEGORY" />
            </intent-filter>
        </activity>
```

把SecondeActivity设置为singleInstance。这样他就单独成栈。

![image-20210705100246782](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210705100246782.png)

## 活动的最佳实践

### 知晓当前在哪一个活动

创建一个`BaseActivity`

```java
public class BaseActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        /*获取类名*/
        Log.d("BaseActivity", getClass().getSimpleName());
    }
}
```

让你的所有活动都继承自它，每次进入就可以打印信息了。

### 随时退出

写个类，用List来保存所有的activity，用一个静态方法将其清除。

```java
public class ActivityCollector {
    public static List<Activity> activities = new ArrayList<>();

    public static void addActivity(Activity activity) {
        activities.add(activity);
    }
    public static void removeActivity(Activity activity){
        activities.remove(activity);
    }
    public static void finishAll(){
        for (Activity activity : activities) {
            if (!activity.isFinishing()){
                activity.finish();
            }
        }
        /*杀死当前程序的进程*/
        android.os.Process.killProcess(android.os.Process.myPid());
    }
}
```

```java
public class BaseActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        /*获取类名*/
        Log.d("BaseActivity", getClass().getSimpleName());
        ActivityCollector.addActivity(this);
    }

    /**
     * 重写onDestroy()方法，在销毁时清除这个
     */
    @Override
    protected void onDestroy() {
        super.onDestroy();
        ActivityCollector.removeActivity(this);
    }
}
```

### 启动活动的最佳写法

传递多个参数启动时会有点麻烦：

```java
    public static void actionStart(Context context, String data1, String data2){
        Intent intent = new Intent(context, SecondActivity.class);
        intent.putExtra("param1", data1);
        intent.putExtra("param2", data2);
        context.startActivity(intent);
    }
```

这样写就好了。

# 3. UI

## 控件

### TextView

> 字体大小用sp作为单位

### Button

1. 匿名类写法
2. 实现接口写法

### EditText

### ImageView

1. 动态更改界面

### ProgressBar进度条

默认圆形进度条

1. 控件的可见属性xml文件中可改

   ```xml
   android:visibility = "visible"可见
   android:visibility = "invisible"不可见透明
   android:visibility = "gnoe"不可见
   ```

2. java文件

   ```java
   progressBar.getVisibility()
   progressBar.setVisibility()
   ```

水平进度条

```xml
    <ProgressBar
        android:id="@+id/progress_bar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        style="?android:attr/progressBarStyleHorizontal"
        android:max="100" />
```

### AlertDialog对话框

点击事件后弹出：

```java
AlertDialog.Builder dialog = new AlertDialog.Builder(MainActivity.this);
dialog.setTitle("This is a Dialog");
dialog.setMessage("Something important");
dialog.setCancelable(false);

dialog.setCancelable(false);
/*在此基础上设置两个点击事件*/
dialog.setPositiveButton("OK", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialogInterface, int i) {

    }
});
dialog.setNegativeButton("Cancel", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialogInterface, int i) {

    }
});
dialog.show();
```

## 4种基本布局

### 线性LinearLayout

```xml
android:layout_gravity 控件在布局中的对齐方式，布局重心
android:gravity 文字在控件中的对齐方式，重心
```

dp控件大小，sp文字大小。

#### 等分宽度

```xml
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        
        <ImageView
            android:id="@+id/star_off"
            android:layout_width="0dp"
            android:layout_weight="1"
            android:layout_height="wrap_content"
            android:background="@mipmap/ic_map_star_nomal_off"/>
        <ImageView
            android:id="@+id/star_on"
            android:layout_width="0dp"
            android:layout_weight="1"
            android:layout_height="wrap_content"
            android:background="@mipmap/ic_map_star_nomal_on"/>
    </LinearLayout>
```

### 相对布局RelativeLayout

1. 控件相对父布局进行定位
2. 控件相对控件进行定位

### 帧布局FrameLayout

默认左上角

### 百分比布局android.support.percent.PercentFrameLayout

需要更改app/build.gradle文件

## 自定义控件

![image-20210706095529302](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210706095529302.png)

### 引入布局

编写好xml文件，在需要引入xml文件的地方加入：

```xml
<include layout="@layout/title"/>
```

隐藏自带标题栏，在onCreate()方法中：

```java
        android.support.v7.app.ActionBar actionBar = getSupportActionBar();
        if (actionBar != null) {
            actionBar.hide();
        }
```

### 自定义控件

多个活动中都有的按键功能可以使用自定义控件。

创建一个类TitleLayout继承自LinearLayout。

```java
public class TitleLayout extends LinearLayout{
    public TitleLayout(Context context, AttributeSet attrs){
        super(context, arrts);   
        LayoutInflater.from(context).inflate(R.layout.title, this);
    }
}
```

#### 引入控件

```xml
    <com.chinabird.uiwidgettest.TitleLayout 
        android:layout_height="wrap_content"
        android:layout_width="match_parent">
    </com.chinabird.uiwidgettest.TitleLayout>
```

最后在TitleLayout中注册点击事件。

## ListView

用RecyclerView代替

## RecyclerView

> recycle 循环回收再利用。

通过一个水果列表来学习RecyclerView

1. 准备工作，在app/build.gradle文件中导入依赖

   ```gradle
   dependencies {
       implementation 'androidx.appcompat:appcompat:1.2.0'
       implementation 'com.google.android.material:material:1.2.1'
       implementation 'androidx.constraintlayout:constraintlayout:2.0.1'
       implementation 'androidx.recyclerview:recyclerview:1.0.0'
       testImplementation 'junit:junit:4.+'
       androidTestImplementation 'androidx.test.ext:junit:1.1.2'
       androidTestImplementation 'androidx.test.espresso:espresso-core:3.3.0'
   }
   ```

   可以看到recyclerview在androidx里。

2. 首先构建一个水果类，其中包括了要显示的水果内容——名称与图片

   `Fruit.java`

   ```java
   package com.chinabird.recycler;
   
   public class Fruit {
       private String name;
       private int imageId;
   
       public Fruit(String name, int imageId) {
           this.name = name;
           this.imageId = imageId;
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
   
       public int getImageId() {
           return imageId;
       }
   
       public void setImageId(int imageId) {
           this.imageId = imageId;
       }
   }
   
   ```

3. 在xml文件中加入Recyclerview

   `activity_main.xml`

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
   
       <!--写入完整包名-->
       <androidx.recyclerview.widget.RecyclerView
           android:id="@+id/recycler_view"
           android:layout_width="match_parent"
           android:layout_height="match_parent" />
   
   </LinearLayout>
   ```

4. 写出fruit的xml布局

   `fruit_item.xml`

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="match_parent"
       android:layout_height="wrap_content">
       <!--水果图片-->
       <ImageView
           android:id="@+id/fruit_image"
           android:layout_width="wrap_content"
           android:layout_height="wrap_content" />
       <!--水果名称-->
       <TextView
           android:id="@+id/fruit_name"
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:layout_gravity="center_vertical"
           android:layout_marginTop="10dp"/>
   </LinearLayout>
   ```

5. 为RecyclerVirew准备一个适配器，适配的是fruit_item

   ```java
   package com.example.recycler;
   
   
   import android.view.LayoutInflater;
   import android.view.View;
   import android.view.ViewGroup;
   import android.widget.ImageView;
   import android.widget.TextView;
   
   import java.util.List;
   
   import androidx.annotation.NonNull;
   import androidx.recyclerview.widget.RecyclerView;
   
   /**
    * 继承自RecyclerView.Adapter实现类，泛型是其内部类
    */
   public class FruitAdapter extends RecyclerView.Adapter<FruitAdapter.ViewHolder> {
       /*全局Friut的List数组*/
       private List<Fruit> mFruitList;
   
       /**
        * 构造器
        */
       public FruitAdapter(List<Fruit> mFruitList) {
           this.mFruitList = mFruitList;
       }
   
       /**
        * 内部类也继承其内部类，并重写构造器
        */
       static class ViewHolder extends RecyclerView.ViewHolder {
           ImageView fruitImage;
           TextView fruitName;
           /**
            * 内部类构造器
            *
            * @param view Recycler子项的最外层布局
            */
           public ViewHolder(View view) {
               super(view);
               fruitImage = view.findViewById(R.id.fruit_image);
               fruitName = view.findViewById(R.id.fruit_name);
           }
       }
   
       @NonNull
       @Override
       public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
           /*加载布局*/
           View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.fruit_item, parent, false);
           /*布局传递到内部类的构造函数中*/
           ViewHolder holder = new ViewHolder(view);
           /*返回内部类对象*/
           return holder;
       }
   
       /**
        * 子项数据赋值，通过这个方法实现滑动添加
        *
        * @param holder
        * @param position
        */
       @Override
       public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
           /*得到当前fruit*/
           Fruit fruit = mFruitList.get(position);
           /*更改图片*/
           holder.fruitImage.setImageResource(fruit.getImageId());
           /*更改文字*/
           holder.fruitName.setText(fruit.getName());
       }
   
       /**
        * @return 子项长度
        */
       @Overrde
       public int getItemCount() {
           return mFruitList.size();
       }
   }
   ```

6. 在MainActivity中进行Recycler的显示，并调用适配器填充内容

   ```java
   package com.example.recycler;
   
   import androidx.appcompat.app.AppCompatActivity;
   import androidx.recyclerview.widget.LinearLayoutManager;
   import androidx.recyclerview.widget.RecyclerView;
   
   import android.os.Bundle;
   
   import java.util.ArrayList;
   import java.util.List;
   
   public class MainActivity extends AppCompatActivity {
       private List<Fruit> mFruitList = new ArrayList<Fruit>();
   
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_main);
           initFruits();
           /*找到activity_main.xml中的recyclerView*/
           RecyclerView recyclerView = findViewById(R.id.recycler_view);
           /*指定RecyclerView布局方式*/
           LinearLayoutManager layoutManager = new LinearLayoutManager(this);
           /*指定为LinearLayout布局*/
           recyclerView.setLayoutManager(layoutManager);
           /*适配去管理滑动显示*/
           FruitAdapter adapter = new FruitAdapter(mFruitList);
           /*设置适配*/
           recyclerView.setAdapter(adapter);
       }
   
       /**
        * 初始化40个苹果数组
        */
       private void initFruits(){
           for (int i = 0; i < 40; i++) {
               Fruit apple = new Fruit("Apple" + i, R.mipmap.ic_launcher);
               mFruitList.add(apple);
           }
       }
   }
   ```

# 4. 碎片

> 碎片是可以嵌入在活动中的UI片段，其不能独立存在，必须由Activity或另一个Fragment托管。Fragment的视图层次结构会成为宿主的视图层次结构的一部分，或附加到宿主的视图层次结构。

最小实践：

![image-20210726162840210](https://i.loli.net/2021/07/26/oiVTcMg5Esy3xjY.png)

```java
package com.example.criminalintent;

import android.os.Bundle;

import androidx.fragment.app.Fragment;
import androidx.fragment.app.FragmentActivity;
import androidx.fragment.app.FragmentManager;

/**
 * 抽象类
 */
public abstract class SingleFragmentActivity extends FragmentActivity {
    /*创建一个Fragment的子类返回*/
    protected abstract Fragment createFragment();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        /*显示fragment视图*/
        setContentView(R.layout.activity_fragment);
        /*管理fragment并将视图添加到activity的视图层级结构中*/
        FragmentManager fm = getSupportFragmentManager();
        /*找到布局中的fragment*/
        Fragment fragment = fm.findFragmentById(R.id.fragment_container);
        /*如果容器中什么都没有*/
        if (fragment == null) {
            /*得到一个Fragment子类*/
            fragment = createFragment();
            fm.beginTransaction()
                /*把fragment添加到容器里，显然这个容器应该是Fragment的*/
                .add(R.id.fragment_container, fragment)
                .commit();
                /*回调Fragment子类的生命周期方法*/
        }
    }
}
```

```java
package com.example.criminalintent;

import androidx.fragment.app.Fragment;

public class CrimeActivity extends SingleFragmentActivity{

    @Override
    protected Fragment createFragment() {
        return new CrimeFragment();
    }
}
```

`R.layout.activity_fragment`这个文件的作用就是添加一个空容器，对应上图中的`activity_crime`

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/fragment_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
</FrameLayout>
```

中间的`crimeFragment.java`与`fragment_crime.xml`是一样的与普通的Activity与xml相似，也就是说Fragment只是对原来Activity的多一层封装，也是一层细化。



![image-20210726163021271](https://i.loli.net/2021/07/26/9WIPQaN83UERObd.png)

`left_fragment.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:text="Button"/>
</LinearLayout>
```

`right_fragment.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:background="#00ff00"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:textSize="20sp"
        android:text="This is right fragment"/>

</LinearLayout>
```

`LeftFragment`

```java
package com.example.fragmenttest;

import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

import androidx.fragment.app.Fragment;

public class LeftFragment extends Fragment {
    @Override
    public View onCreateView( LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.left_fragment, container, false);
        return view;
    }
}

```

`RightFragment`

```java
package com.example.fragmenttest;

import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

import androidx.fragment.app.Fragment;

public class RightFragment extends Fragment {
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.right_fragment, container, false);
        return view;
    }
}

```

`activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <fragment
        android:id="@+id/left_fragment"
        android:name="com.example.fragmenttest.LeftFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"/>
    <fragment
        android:id="@+id/Right_fragment"
        android:name="com.example.fragmenttest.RightFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"/>

</LinearLayout>
```

## Fragment与RecyclerView的混用

![image-20210727111054879](https://i.loli.net/2021/07/27/NbKIosvx8eMgGUR.png)

### 模型层

`Cirme.java`

```java
package com.example.criminalintent;

import java.text.SimpleDateFormat;
import java.util.Locale;
import java.util.UUID;
import java.util.Date;

public class Crime {
    /*标识ID*/
    private UUID mId;
    /*标题*/
    private String mTitle;
    private Date mDate;
    private boolean mSolved;
    public Crime(){
        mId = UUID.randomUUID();
        mDate = new Date();
    }

    public String getData(){
//        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        SimpleDateFormat format = new SimpleDateFormat("EEEE,MMM dd,yyyy", Locale.ENGLISH);
        return format.format(mDate);
//        DateFormat dateFormat = DateFormat.getDateInstance(DateFormat.LONG, Locale.ENGLISH);
//        return dateFormat.format(mDate);
//        DateFormat dateFormat = DateFormat.getDateInstance(DateFormat.LONG, Locale.ENGLISH);
//        return dateFormat.format(mDate);
    }

    public void setData(Date data) {
        mDate = data;
    }

    public boolean isSolved() {
        return mSolved;
    }

    public void setSolved(boolean solved) {
        mSolved = solved;
    }

    public UUID getId() {
        return mId;
    }

    public void setId(UUID id) {
        mId = id;
    }

    public String getTitle() {
        return mTitle;
    }

    public void setTitle(String title) {
        mTitle = title;
    }

}
```

`CrimeLab.java`

```java
package com.example.criminalintent;

import android.content.Context;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

/**
 * 创建数据模型：一个单例，一个包含100个Crime的List数组
 */
public class CrimeLab {
    private static CrimeLab sCrimeLab;
    private List<Crime> mCrimes;

    public static CrimeLab get(Context context){
        if (sCrimeLab == null)
            return new CrimeLab(context);
        return sCrimeLab;
    }

    private CrimeLab(Context context){
        mCrimes = new ArrayList<>();
        /*生成100个crime*/
        for (int i = 0; i < 100; i++) {
            Crime crime = new Crime();
            crime.setTitle("Crime #" + i);
            crime.setSolved(i % 2 == 0);
            mCrimes.add(crime);
        }
    }
    public List<Crime> getCrimes(){
        return mCrimes;
    }
    public Crime getCrime(UUID id){
        for (Crime crime : mCrimes){
            if (crime.getId().equals(id)){
                return crime;
            }
        }
        return null;
    }
}
```

### 控制层

`CrimeListFragment.java`显示RecyclerView在碎片中

```java
package com.example.criminalintent;

import android.annotation.SuppressLint;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.LinearLayout;
import android.widget.TextView;

import org.jetbrains.annotations.NotNull;

import java.util.List;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

public class CrimeListFragment extends Fragment {
    private RecyclerView mRecyclerView;
    private CrimeAdapter mAdapter;

    private void updateUI(){
        /*指定布局方式为LinearLayout*/
        LinearLayoutManager layoutManager = new LinearLayoutManager(getContext());
        mRecyclerView.setLayoutManager(layoutManager);
        /*获得单例对象*/
        CrimeLab crimeLab = CrimeLab.get(getActivity());
        /*获得一个泛型List*/
        List<Crime> crimes = crimeLab.getCrimes();
        /*适配器*/
        mAdapter = new CrimeAdapter(crimes);
        mRecyclerView.setAdapter(mAdapter);
    }
    @Nullable
    @org.jetbrains.annotations.Nullable
    @Override
    public View onCreateView(@NonNull @NotNull LayoutInflater inflater, @Nullable @org.jetbrains.annotations.Nullable ViewGroup container, @Nullable @org.jetbrains.annotations.Nullable Bundle savedInstanceState) {
        /*加载一个视图，注意这个视图不是碎片，而是碎片中的内容*/
        View view = inflater.inflate(R.layout.fragment_crime_list, container, false);
        /*找到RecyclerView*/
        mRecyclerView = view.findViewById(R.id.crime_recycler_view);
        /*给RecyclerView填充内容*/
        updateUI();
        return view;
    }
    /*容纳View布局，RecyclerView本身不会创建视图，它创建的是ViewHolder，ViewHolder引用着一个个itemView*/
    private class CrimeHolder extends RecyclerView.ViewHolder{
        public TextView mTitleTextView;

        @SuppressLint("ResourceType")
        public CrimeHolder(@NonNull @NotNull View itemView) {
            super(itemView);
            mTitleTextView = (TextView) itemView;
        }
    }
    private class CrimeAdapter extends RecyclerView.Adapter<CrimeHolder>{
        private List<Crime> mCrimes;

        /**
         * 从模型层获取数据
         * @param crimes
         */
        public CrimeAdapter(List<Crime> crimes){
            mCrimes = crimes;
        }

        /**
         * 接着RecyclerView调用此方法创建ViewHolder及ViewHolder要显示的视图，
         * 一旦创建了够用的ViewHolder，RecyclerView就会停止调用此方法，通过回收利用旧的ViewHolder节约时间和内存
         * @param parent
         * @param viewType
         * @return
         */
        @NonNull
        @NotNull
        @Override
        public CrimeHolder onCreateViewHolder(@NonNull @NotNull ViewGroup parent, int viewType) {
            /*获得当前的打气筒*/
            LayoutInflater layoutInflater = LayoutInflater.from(getActivity());
//            LayoutInflater layoutInflater = LayoutInflater.from(parent.getContext());就是其activity类
            View view = layoutInflater.inflate(android.R.layout.simple_list_item_1, parent, false);
            return new CrimeHolder(view);
        }

        /**
         * 最后，Recycler传入ViewHolder及其位置，adapter会找到目标位置数据并绑定到ViewHolder视图，也就是用模型数据填充视图
         * @param holder
         * @param position
         */
        @Override
        public void onBindViewHolder(@NonNull @NotNull CrimeListFragment.CrimeHolder holder, int position) {
            Crime crime = mCrimes.get(position);
            holder.mTitleTextView.setText(crime.getTitle());
        }

        /**
         * 首先，调用adapt的getItemCount()方法，RecyclerView询问数组列表包含多少个对象
         * @return
         */
        @Override
        public int getItemCount() {
            return mCrimes.size();
        }
    }
}
```

`CrimeListActivity.java`把这个包含RecyclerView的碎片放到碎片容器里

```java
package com.example.criminalintent;

import androidx.fragment.app.Fragment;

public class CrimeListActivity extends SingleFragmentActivity{
    @Override
    protected Fragment createFragment() {
        return new CrimeListFragment();
    }
}

```

`SingleFragmentActivity.java`

```java
package com.example.criminalintent;

import android.os.Bundle;

import androidx.fragment.app.Fragment;
import androidx.fragment.app.FragmentActivity;
import androidx.fragment.app.FragmentManager;

/**
 * 抽象类
 */
public abstract class SingleFragmentActivity extends FragmentActivity {
    /*创建一个Fragment的子类返回*/
    protected abstract Fragment createFragment();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        /*显示fragment视图*/
        setContentView(R.layout.activity_fragment);
        /*管理fragment并将视图添加到activity的视图层级结构中*/
        FragmentManager fm = getSupportFragmentManager();
        /*找到布局中的fragment*/
        Fragment fragment = fm.findFragmentById(R.id.fragment_container);
        /*如果容器中什么都没有*/
        if (fragment == null) {
            /*得到一个Fragment子类*/
            fragment = createFragment();
            fm.beginTransaction()
                /*把fragment添加到容器里，显然这个容器应该是Fragment的*/
                .add(R.id.fragment_container, fragment)
                .commit();
                /*回调Fragment子类的生命周期方法*/
        }
    }
}
```

### 视图层

`activity_fragment.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/fragment_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
</FrameLayout>
```

`fragment_crime_list.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.recyclerview.widget.RecyclerView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/crime_recycler_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</androidx.recyclerview.widget.RecyclerView>
```

Fragment类的作用是把自定义布局变成碎片，再搞一个碎片容器，通过Activity把这个碎片加进去就好。在上例自定义布局是一个RecyclerView。

## 静态添加碎片

![image-20210731163332853](https://i.loli.net/2021/07/31/NLYgqxJGaRpKs3A.png)

可以看到多了两个文件：

![image-20210731164546375](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210731164546375.png)

删除TestFragment_1的其它内容，只保留onCreateView方法：

```java
package com.example.fragmenttest_1;

import android.os.Bundle;

import androidx.fragment.app.Fragment;

import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

public class TestFragment_1 extends Fragment {

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_test_1, container, false);
    }
}
```

fragment_test_1随便写个自己喜欢的布局：

`fragment_test_1.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    >
    <Button
        android:id="@+id/button1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="button1"
        />
    <TextView
        android:id="@+id/text1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button1"
        android:gravity="center_horizontal"
        />

</LinearLayout>
```

把activity_main改一下，加上fragment，这一步就是静态添加的意思，把自己写的fragment布局加到一个main布局里去。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >
    <fragment
        android:id="@+id/fragment"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:name="com.example.fragmenttest_1.TestFragment_1"
        />

</LinearLayout>
```

运行一下：

![image-20210731165247719](https://i.loli.net/2021/07/31/n9ZN2qY46eoWD8l.png)

运行过程应该是这样：`MainActivity.java`加载`activity_main.xml`，发现`fragment`，回调`TestFragment_1.java`中的onCreateView方法把`fragment_test_1.xml`布局加载进来。

这种程序发现`fragment`就自己去找的方式就是静态的，写死在`activity_main.xml`中，程序运行后不可以控制什么时候去找。

## 动态添加碎片

和静态一样，创建两个文件，修改方法也一样。唯一不同的，先把`activity_main.xml`的内容改成下面的：

`activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >
    <!--作为容器用-->
    <FrameLayout
        android:id="@+id/fragemnt"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        ></FrameLayout>

</LinearLayout>
```

这里把`fragment`换成了`FrameLayout`程序就不会找了，运行一下看看：

![image-20210731171915433](https://i.loli.net/2021/07/31/Sef8Kg3dGkBPaO6.png)

现在就剩下让程序怎么去找的问题了，在`MainActivity.java`中去写：

```java
package com.example.fragmenttest_2;

import androidx.appcompat.app.AppCompatActivity;
import androidx.fragment.app.Fragment;
import androidx.fragment.app.FragmentManager;
import androidx.fragment.app.FragmentTransaction;

import android.os.Bundle;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        /*管理fragment并将视图添加到activity的视图层级结构中*/
        FragmentManager fm = getSupportFragmentManager();
        /*找到布局中的fragment*/
        Fragment fragment = fm.findFragmentById(R.id.fragemnt);
        /*如果容器中什么都没有*/
        if (fragment == null) {
            /*得到一个Fragment子类*/
            fragment = new FragmentTest_2();
            fm.beginTransaction()
                    /*把fragment添加到容器里，显然这个容器应该是Fragment的*/
                    .add(R.id.fragemnt, fragment)
                    .commit();
            /*然后就会自动回调Fragment子类的生命周期方法*/
        }
      	//也可以这样写
        /*获取FragmentManager*/
        //FragmentManager fragmentManager = getSupportFragmentManager();
        /*处理，开启一个事务*/
        //FragmentTransaction transaction = fragmentManager.beginTransaction();
        /*替换碎片*/
        //transaction.replace(R.id.right_layout, fragment);
		/*提交事务*/
        //transaction.commit();
    }
}
```



```java
package com.example.fragmenttest;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import androidx.appcompat.app.AppCompatActivity;
import androidx.fragment.app.Fragment;
import androidx.fragment.app.FragmentManager;
import androidx.fragment.app.FragmentTransaction;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button = findViewById(R.id.button);
        button.setOnClickListener(this);
        replaceFragment(new RightFragment());
    }

    @Override
    public void onClick(View view) {
        switch (view.getId()){
            case R.id.button:
                replaceFragment(new AnotherRightFragment());
                break;
            default:
                break;
        }
    }
    private void replaceFragment(Fragment fragment){
        /*获取FragmentManager*/
        FragmentManager fragmentManager = getSupportFragmentManager();
        /*处理，开启一个事务*/
        FragmentTransaction transaction = fragmentManager.beginTransaction();
        /*替换碎片*/
        transaction.replace(R.id.right_layout, fragment);
		/*提交事务*/
        transaction.commit();
    }
}
```

![image-20210803132851705](https://i.loli.net/2021/08/03/Im2oFSnMphj9QJw.png)

## 如何在碎片之间传递数据

### Intent传输

碎片不是活动，所以会有一点点区别。

Intent跳转还是根据活动来的，所以还是用Intent传递。每一个Activity对应一个Fragment，Fragment也是通过Activity调用的Fragment的add得到的，可以在Activity中加一个静态方法，传递数据进来，返回带数据的Intent。

```java
public static final String EXTRA_CRIME_ID = "com.example.criminalintent.crime_id";

public static Intent newIntent(Context packageContext, UUID crimeId){
    Intent intent = new Intent(packageContext, CrimeActivity.class);
    intent.putExtra(EXTRA_CRIME_ID, crimeId);
    return intent;
}
```

在Activity中写上：

```java
Intent intent = new Intent(getActivity(), CrimeActivity.class);
startActivity(intent);
```

最后在启动的Activity对应的Fragment中得到数据。

```java
    @Override
    public void onCreate(@Nullable @org.jetbrains.annotations.Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        /*geIntent()方法是在Activity中调用的*/
        UUID crimeId = (UUID)getActivity().getIntent().getSerializableExtra(CrimeActivity.EXTRA_CRIME_ID);
        /*获得单例*/
        CrimeLab crimeLab = CrimeLab.get(getActivity());
        /*找到*/
        mCrime = crimeLab.getCrime(crimeId);
    }
```

### fragment argument

每个fragment实例都可以带一个Bundle对象。这个对象有一个键值对可以存储。1，2是在要调用的Activity中，3，4在要调用的Fragment中。Intent中取出送入Fragment中，再从Fragment中取出。

1. 同样也是调用newIntent

   ```java
   private static final String EXTRA_CRIME_ID = "com.example.criminalintent.crime_id";
   
   /*1*/
   /*先加载Activity*/
   public static Intent newIntent(Context packageContext, UUID crimeId){
       Intent intent = new Intent(packageContext, CrimeActivity.class);
       /*传入*/
       intent.putExtra(EXTRA_CRIME_ID, crimeId);
       return intent;
   }
   ```

2. 取出存入

   ```java
       /*2*/
       @Override
       protected Fragment createFragment() {
   //        return new CrimeFragment();
           /*取出*/
           UUID crimeId = (UUID)getIntent().getSerializableExtra(EXTRA_CRIME_ID);
           /*存到CrimeFragment中*/
           return CrimeFragment.newInstance(crimeId);
       }
   ```

3. 存

   ```java
   /*3*/
   public static CrimeFragment newInstance(UUID crimeId){
       Bundle args = new Bundle();
       args.putSerializable(ARG_CRIME_ID, crimeId);
   
       CrimeFragment fragment = new CrimeFragment();
       /*在fragment创建后，添加给activity前完成set*/
       fragment.setArguments(args);
       return fragment;
   }
   ```

4. 取出

   ```java
       /**
        * 创建
        * @param savedInstanceState
        */
       @Override
       public void onCreate(@Nullable @org.jetbrains.annotations.Nullable Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
   
           /*取出*/
           UUID crimeId = (UUID) getArguments().getSerializable(ARG_CRIME_ID);
           CrimeLab crimeLab = CrimeLab.get(getActivity());
           /*最后想要的结果*/
           mCrime = crimeLab.getCrime(crimeId);
       }
   ```

## 碎片与活动之间的通信

1. 活动中获得碎片

```java
RightFragment rightFragment = (RightFragment)getSupportFragmentManager().findFragmentById(R.id.right_fragment);
```

2. 碎片中获得活动

```java
MainActivity activity = (MainActivity)getActivity();
```

## 简易版新闻的理解

<img src="C:\Users\31787\OneDrive\图片\UML\out\第一行\news\news.png" alt="news"  />

MainActivity调用根据屏幕大小调用布局：

1. 单页模式：RecyclerView显示新闻标题，有点击传递数据进入NewContentActivity，其布局中是静态Fragment
2. 双页模式：
   1. 左侧：RecyclerView显示新闻标题
   2. 右侧：根据左侧的点击，获得NewContentFragment对象刷新

`News.java`

```java
package com.example.fragmentbestpractice;

public class News {
    private String title;
    private String content;

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }
}
```

`news_item.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/news_title"
    android:maxLines="1"
    android:ellipsize="end"
    android:textSize="18sp"
    android:paddingLeft="10dp"
    android:paddingRight="10dp"
    android:paddingTop="15dp"
    android:paddingBottom="15dp"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

</TextView>
```

`news_title_frag.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.recyclerview.widget.RecyclerView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/news_title_recycler_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</androidx.recyclerview.widget.RecyclerView>ja
```

`NewsContentFragment.java`

```java
package com.example.fragmentbestpractice;

import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import androidx.fragment.app.Fragment;

/*新闻内容碎片*/
public class NewsContentFragment extends Fragment {
    private View view;

    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        /*新闻内容布局*/
        view = inflater.inflate(R.layout.news_content_frag, container, false);
        return view;
    }

    /**
     *
     * @param newsTitle 新闻标题
     * @param newsContent 内容控件
     */
    public void refresh(String newsTitle, String newsContent)
    {
        View visibilityLayout = view.findViewById(R.id.visibility_layout);
        visibilityLayout.setVisibility(View.VISIBLE);

        TextView newsTitleText = view.findViewById(R.id.news_title);
        TextView newsContentText = view.findViewById(R.id.news_content);
        newsTitleText.setText(newsTitle);
        newsContentText.setText(newsContent);
    }
}
```

`news_content_frag.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--新闻内容-->
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:id="@+id/visibility_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:visibility="invisible">
        <!--标题-->
        <TextView
            android:id="@+id/news_title"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:padding="10dp"
            android:textSize="20sp" />
        <!--分隔线-->
        <View
            android:layout_width="match_parent"
            android:layout_height="1dp"
            android:background="#000" />
        <!--内容-->
        <TextView
            android:id="@+id/news_content"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            android:padding="15dp"
            android:textSize="18sp" />

    </LinearLayout>
    <View
        android:layout_width="1dp"
        android:layout_height="match_parent"
        android:layout_alignParentLeft="true"
        android:background="#000"/>
</RelativeLayout>
```

`news_content.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <!--静态加载-->
    <fragment
        android:id="@+id/news_content_fragment"
        android:name="com.example.fragmentbestpractice.NewsContentFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</LinearLayout>
```

`NewsContentActivity.java`

```java
package com.example.fragmentbestpractice;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Context;
import android.content.Intent;
import android.os.Bundle;

public class NewsContentActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.news_content);
        String newsTitle = getIntent().getStringExtra("news_title");
        String newsContent = getIntent().getStringExtra("news_content");
        NewsContentFragment newsContentFragment = (NewsContentFragment) getSupportFragmentManager().findFragmentById(R.id.news_content_fragment);
        newsContentFragment.refresh(newsTitle, newsContent);
    }

    /**
     * 写在跳转到的Activity中
     * @param context 开始跳转的Activity
     * @param newsTitle 标题
     * @param newsContent 内容
     */
    public static void actionStar(Context context, String newsTitle, String newsContent){
        Intent intent = new Intent(context, NewsContentActivity.class);
        intent.putExtra("news_title", newsTitle);
        intent.putExtra("news_content", newsContent);
        context.startActivity(intent);
    }
}
```

`MainActivity.java`

```java
package com.example.fragmentbestpractice;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        /*引用的是一个FrameLayout，其中有一个静态加载的fragment*/
        setContentView(R.layout.activity_main);
    }
}
```

`activity_main.xml`单页

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <!--main_activity加载这个碎片会去调用他的onCreateView之类的方法，静态调用-->
    <fragment
        android:id="@+id/news_title_fragment"
        android:name="com.example.fragmentbestpractice.NewsTitleFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</FrameLayout>
```

双页

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!--静态加载左边的标题布局-->
    <fragment
        android:id="@+id/news_title_fragment"
        android:name="com.example.fragmentbestpractice.NewsTitleFragment"
        android:layout_width="0dp"
        android:layout_weight="1"
        android:layout_height="match_parent"/>
    <!--帧布局，用来显示右边的内容-->
    <FrameLayout
        android:id="@+id/news_content_layout"
        android:layout_width="0dp"
        android:layout_weight="3"
        android:layout_height="match_parent">
        <!--静态加载右边的内容碎片-->
        <fragment
            android:id="@+id/news_content_fragment"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:name="com.example.fragmentbestpractice.NewsContentFragment"/>
    </FrameLayout>
</LinearLayout>
```



# 5. 广播机制

> Android应用与系统，应用与应用都可以互相收发广播，可以理解为它们之间的交流方式

> Android8.0后广播改动较大

## 系统广播

> 系统会在发生各种系统事件时自动发送广播。home键返回键，网络的开关等等，指的也就是第一个Android系统手机都有的功能。

广播消息本身会被封装在一个Intent对象中，该对象的操作字符串会标识所发生的事件(android.intent.action.AIRPLANE_MODE)，这个intent可能还包含绑定到其extra字段中的附加信息，例如，飞行模式包含布尔值extra来指示是否已开启飞行模式。

## 动态注册监听网络变化（上下文注册的接收器）

> 动态注册就是在Java代码中进行注册

```java
package com.chinabird.broadcastbrstpractice;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.content.IntentFilter;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.support.annotation.Nullable;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    private IntentFilter mIntentFilter;
    private NetworkChangeReceiver mNetworkChangeReceiver;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mIntentFilter = new IntentFilter();
        /*网络变化*/
        mIntentFilter.addAction(ConnectivityManager.CONNECTIVITY_ACTION);
        /*飞行模式*/
        mIntentFilter.addAction(Intent.ACTION_AIRPLANE_MODE_CHANGED);
        mNetworkChangeReceiver = new NetworkChangeReceiver();
        /*注册*/
        registerReceiver(mNetworkChangeReceiver, mIntentFilter);


        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                new Handler().postDelayed(new Runnable() {
                    @Override
                    public void run() {
                        Intent intent1 = new Intent("MY_BROADCAST");
                        intent1.setComponent(new ComponentName("com.chinabird.broadcastbrstpractice", "com.chinabird.broadcastbrstpractice.MyBroadcastReceiver2"));
                        sendBroadcast(intent1);
                    }
                }, 3000);

//                intent.setPackage("com.example.broadcasttest2");
//                sendBroadcast(intent);
//                sendOrderedBroadcast(intent, null);
                Intent intent = new Intent("MY_BROADCAST");
                /*在Android 8.0版本后，必须添加这一行，完整的广播类包名与类名*/
                intent.setComponent(new ComponentName("com.chinabird.broadcastbrstpractice", "com.chinabird.broadcastbrstpractice.MyBroadcastReceiver"));
//                intent.setPackage("com.chinabird.broadcastbrstpractice");
//                sendBroadcast(intent);
                sendOrderedBroadcast(intent, null);
            }
        });
    }

    /**
     * 专门监听网络的类
     */
    class NetworkChangeReceiver extends BroadcastReceiver {

        /**
         * 如果网络变化，这个方法就会被执行，加了个飞行模式，飞行模式开的时候也会
         *
         * @param context
         * @param intent
         */
        @RequiresApi(api = Build.VERSION_CODES.JELLY_BEAN_MR1)
        @Override
        public void onReceive(Context context, Intent intent) {
            /*系统服务类*/
            ConnectivityManager connectivityManager = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
            if (networkInfo != null && networkInfo.isAvailable()) {
                Toast.makeText(context, "network is available", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(context, "network is unavailable", Toast.LENGTH_SHORT).show();
            }
            if (isAirPlaneModeOn()) {
                Toast.makeText(context, "airplne is available", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(context, "airplne is unavailable", Toast.LENGTH_SHORT).show();
            }
        }
    }

    /*判断是否是飞行模式*/
    @RequiresApi(api = Build.VERSION_CODES.JELLY_BEAN_MR1)
    private boolean isAirPlaneModeOn() {
        int mode = 0;
        try {
            mode = Settings.Global.getInt(getContentResolver(), Settings.Global.AIRPLANE_MODE_ON);
        } catch (Settings.SettingNotFoundException e) {
            e.printStackTrace();
        }
        return mode == 1;
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        unregisterReceiver(mNetworkChangeReceiver);
    }
}
```

在配置文件中声明权限：

```xml
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
   <!-- <uses-permission android:name="android.permission.INTERNET"/> -->
```

## 静态注册开机启动（清单声明的接收器）

> 动态注册必须在程序启动后才能接收广播，静态注册是为了在程序未启动的情况下接收广播。

![image-20210721105210711](https://i.loli.net/2021/07/21/cuo1Rs9dEOIr68F.png)

Export和Enable全部勾选。

`BootCompleteReceiver`

```java
package com.chinabird.broadcastbrstpractice;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.widget.Toast;

public class BootCompleteReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        // TODO: This method is called when the BroadcastReceiver is receiving
        // an Intent broadcast.
//        throw new UnsupportedOperationException("Not yet implemented");
        Toast.makeText(context, "Boot Complete", Toast.LENGTH_LONG).show();
    }
}
```

`AndroidManifest`

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.chinabird.broadcastbrstpractice">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity android:name=".Main2Activity" />
        <!--静态广播的注册-->
        <!--Export：是否允许接收本程序以外的广播-->
        <!--Enable: 是否启用这个广播-->
        <receiver
            android:name=".BootCompleteReceiver"
            android:enabled="true"
            android:exported="true">
            <!--静态广播的监听动作-->
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
            </intent-filter>

        </receiver>

    </application>

    <!--系统网络权限-->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <!--系统开机权限-->
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>

</manifest>
```

注意不要在onReceive()方法添加过多逻辑，广播接收器是不允话开启线程的，长时间没有结束就会报错。

## 自定义广播

### 发送标准广播 

同样新建一个MyBroadcastReceiver

```java
package com.chinabird.broadcastbrstpractice;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.widget.Toast;

public class MyBroadcastReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
//        // TODO: This method is called when the BroadcastReceiver is receiving
//        // an Intent broadcast.
//        throw new UnsupportedOperationException("Not yet implemented");
        Toast.makeText(context, "received in MyBroadcastReceiver", Toast.LENGTH_SHORT).show();
    }
}
```

在`AndroidManifest.xml`文件中设置

```xml
        <receiver
            android:name=".MyBroadcastReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
                <action android:name="MY_BROADCAST"/>
            </intent-filter>
        </receiver>
```

Activity中设置发送广播

```java
Button button = findViewById(R.id.button);
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Intent intent = new Intent("MY_BROADCAST");
        /*在Android 8.0版本后，必须添加这一行，完整的广播类包名与类名*/
        intent.setComponent(new ComponentName("com.chinabird.broadcastbrstpractice","com.chinabird.broadcastbrstpractice.MyBroadcastReceiver"));
        /*也可以这样写*/
        intent.setPackage("com.chinabird.broadcastbrstpractice");
        
        sendBroadcast(intent, null);
//      sendBroadcast(intent);
    }
});
```

由于8.0后广播的要求改动较大，有序广播，静态注册基本没用了。



# 6. 存储

## 文件存储

```java
    private void save(String inputText) {
        FileOutputStream out = null;
        BufferedWriter writer = null;
        try {
            /*得到一个流对象*/
            out = openFileOutput("data", Context.MODE_PRIVATE);
            /*OutputStreamWriter对象*/
            writer = new BufferedWriter(new OutputStreamWriter(out));
            /*写入*/
            writer.write(inputText);
            Log.e("MainActivity", "Write");
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            try{
                if (writer != null){
                    writer.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

## 文件读取

```java
    public String load() {
        FileInputStream in = null;
        BufferedReader reader = null;
        StringBuilder content = new StringBuilder();
        try {
            in = openFileInput("data");
            reader = new BufferedReader(new InputStreamReader(in));
            String line = "";
            while ((line = reader.readLine()) != null) {
                content.append(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (reader != null) {
                try {
                    reader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return content.toString();
    }
```

## SharedPreferences

### 存储

```java
        Button saveData= findViewById(R.id.save_data);
        saveData.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                /*得到SharedPreferences.Editor对象，指定文件名为data*/
                SharedPreferences.Editor editor = getSharedPreferences("data", MODE_PRIVATE).edit();
                /*将数据以键值对的方式写入*/
                editor.putString("name", "Tom");
                editor.putInt("age", 28);
                editor.putBoolean("married", false);
                /*提交*/
                editor.apply();
                Log.d("MainActivity", "Write");
            }
        });
```

### 读取

```java
        Button restoreData = findViewById(R.id.restore_data);
        restoreData.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                /*得到SharePreferences对象*/
                SharedPreferences pref= getSharedPreferences("data", MODE_PRIVATE);
                /*获得存入的键值对，要加一个默认数据*/
                String name = pref.getString("name", "");
                int age = pref.getInt("age", 0);
                boolean married = pref.getBoolean("married", false);
                Log.d("MainActivity", "name is " + name);
                Log.d("MainActivity", "age is " + age);
                Log.d("MainActivity", "married is " + married);
            }
        });
```

### 密码

```java
package com.chinabird.broadcastbrstpractice;

import android.content.Intent;
import android.content.SharedPreferences;
import android.preference.PreferenceManager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    private SharedPreferences pref;
    private SharedPreferences.Editor editor;

    private EditText accountEdit;
    private EditText passwordEdit;
    private Button login;
    private CheckBox rememberPass;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        /*得到SharePreferences对象*/
        pref = PreferenceManager.getDefaultSharedPreferences(this);

        /*得到所有按键*/
        accountEdit = findViewById(R.id.account);
        passwordEdit = findViewById(R.id.password);
        login = findViewById(R.id.login);
        rememberPass = findViewById(R.id.remember_pass);

        /*获得remember的数据，默认是false*/
        boolean isRemember = pref.getBoolean("remember_password", false);
        /*如果是true，就获得写入*/
        if (isRemember) {
            String account = pref.getString("account", "");
            String password = pref.getString("password", "");
            accountEdit.setText(account);
            passwordEdit.setText(password);
            rememberPass.setChecked(true);
        }
        /*登录时确定是否写入*/
        login.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                String account = accountEdit.getText().toString();
                String password = passwordEdit.getText().toString();
                if (account.equals("admin") && password.equals("12345")) {
                    editor = pref.edit();
                    /*如果rember是点击好的*/
                    if (rememberPass.isChecked()) {
                        editor.putBoolean("remember_password", true);
                        editor.putString("account", account);
                        editor.putString("password", password);
                    } else {
                        editor.clear();
                    }
                    editor.apply();
                    Intent intent = new Intent(MainActivity.this, Main2Activity.class);
                    startActivity(intent);
//                    finish();
                } else {
                    Toast.makeText(MainActivity.this, "account or password is invalid", Toast.LENGTH_SHORT).show();
                }

            }
        });
    }
}
```

xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Account:" />

        <EditText
            android:id="@+id/account"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1" />

    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content" 
            android:layout_height="wrap_content" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Remember password"
            android:textSize="18sp" />

    </LinearLayout>

    <Button
        android:id="@+id/login"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="login" />

</LinearLayout>
```

## SQLite数据库存储

数据库建表：

`MyDatabaseHelper.java`

```java
package com.example.databasetest;

import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;
import android.widget.Toast;
import androidx.annotation.Nullable;

public class MyDatabaseHelper extends SQLiteOpenHelper {
    private Context mContext;
    public static final String CREATE_BOOK = "create table Book("
            + "id integer primary key autoincrement,"
            + "author test,"
            + "price real, "
            + "pages integer, "
            + "name text)";
    public static final String CREATE_CATEGORY = "create table Category("
            + "id integer primary key autoincrement,"
            + "category_name text,"
            + "category_code integer)";

    /*只要版本号更大就可以实现升级*/
    public MyDatabaseHelper(@Nullable Context context, @Nullable String name, @Nullable SQLiteDatabase.CursorFactory factory, int version) {
        super(context, name, factory, version);
        mContext = context;
    }

    @Override
    public void onCreate(SQLiteDatabase sqLiteDatabase) {
        /*execSQL用来执行这个表建表语句*/
        sqLiteDatabase.execSQL(CREATE_BOOK);
        sqLiteDatabase.execSQL(CREATE_CATEGORY);
        Toast.makeText(mContext, "Create succeeded", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onUpgrade(SQLiteDatabase sqLiteDatabase, int i, int i1) {
        /*如果发现已经存在Book表就删除，引号内是一个DROP语句*/
        sqLiteDatabase.execSQL("drop table if exists Book");
        sqLiteDatabase.execSQL("drop table if exists Category");
        /*删除完了再去生成*/
        onCreate(sqLiteDatabase);
    }
}
```

`MainActivity.java`

```java
package com.example.databasetest;

import androidx.appcompat.app.AppCompatActivity;
import android.content.ContentValues;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {
    private MyDatabaseHelper mDatabaseHelper;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mDatabaseHelper = new MyDatabaseHelper(this, "BookStore.db", null, 2);
        /*建表*/
        Button createDatabase = findViewById(R.id.create_database);
        createDatabase.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                /*打开或创建一个现有的数据库*/
                /*如果是创建就会去执行onCreate方法，否则返回那个打开的*/
                mDatabaseHelper.getWritableDatabase();
            }
        });
        /*添加数据*/
        Button addData = findViewById(R.id.add_data);
        addData.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                SQLiteDatabase db = mDatabaseHelper.getWritableDatabase();

                ContentValues values = new ContentValues();
                /*组装第一条数据*/
                values.put("name", "The Da Vinci Code");
                values.put("author", "Dan Brown");
                values.put("pages", 454);
                values.put("price", 16.96);
                /*插入第一条数据*/
                db.insert("Book", null, values);
                values.clear();

                values.put("name", "The Lost Symbol");
                values.put("author", "Dan Brown");
                values.put("pages", 510);
                values.put("price", 19.95);
                /*插入第二条数据*/
                db.insert("Book", null, values);
            }
        });
        /*更新数据*/
        Button updateData = findViewById(R.id.update_data);
        updateData.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                SQLiteDatabase db = mDatabaseHelper.getWritableDatabase();
                ContentValues values = new ContentValues();
                values.put("price", 10.99);
                /*更新Book表中，name = The Da Vinci Code行，price行为10.99*/
                db.update("Book", values, "name = ?", new String[]{"The Da Vinci Code"});
            }
        });
        /*删除数据*/
        Button deleteButton = findViewById(R.id.delete_data);
        deleteButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                SQLiteDatabase db = mDatabaseHelper.getWritableDatabase();
                db.delete("Book", "pages > ?", new String[]{"500"});
            }
        });
        /*查询数据*/
        Button queryButton = findViewById(R.id.query_data);
        queryButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                SQLiteDatabase db = mDatabaseHelper.getWritableDatabase();
                /*查询Book表中所有的数据*/
                Cursor cursor = db.query("Book", null, null, null ,null, null, null);
                /*遍历*/
                if (cursor.moveToFirst()){
                    do{
                        int id = cursor.getInt(cursor.getColumnIndex("id"));
                        String name = cursor.getString(cursor.getColumnIndex("name"));
                        String author = cursor.getString(cursor.getColumnIndex("author"));
                        int pages = cursor.getInt(cursor.getColumnIndex("pages"));
                        double price = cursor.getDouble(cursor.getColumnIndex("price"));
                        Log.d(TAG, "book id is " + id);
                        Log.d(TAG, "book name is " + name);
                        Log.d(TAG, "book author is " + author);
                        Log.d(TAG, "book pages is " + pages);
                        Log.d(TAG, "book price is " + price);
                    }while (cursor.moveToNext());
                }
                /*关闭*/
                cursor.close();
            }
        });
    }

    private static final String TAG = "MainActivity";
}
```

`activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    >
    <Button
        android:id="@+id/create_database"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Create database"
        />
    <Button
        android:id="@+id/add_data"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Add data"
        />
    <Button
        android:id="@+id/update_data"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Update data"
        />
    <Button
        android:id="@+id/delete_data"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Delete data"
        />
    <Button
        android:id="@+id/query_data"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Query data"
        />
</LinearLayout>
```

## LitePal

> 需要时再去书上查吧

# 7.  跨程序共享数据——探究内容提供者

> 内容提供器(Content Provider)用于在不同程序之间实现数据共享的功能。

## 1. 运行时权限

> 6.0后不在安装时一次获取全部权限，而在软件使用过程中再对某一项权限进行授权

`xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:id="@+id/make_call"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/app_name"
        android:onClick="clickView"/>

</LinearLayout>
```

`activity`

```java
package com.example.runtimepermissiontest;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

import android.Manifest;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    Button makeCall;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        makeCall = findViewById(R.id.make_call);
//        makeCall.setOnClickListener(new View.OnClickListener() {
//            @Override
//            public void onClick(View view) {
//                /*判断用户是否已经给我们授过权了，相等表示已经授权了*/
//                if (ContextCompat.checkSelfPermission(MainActivity.this, Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED) {
//                    /*向用户申请权限*/
//                    /*Activity实例
//                      要申请的权限数组
//                      请求码
//                     */
//                    ActivityCompat.requestPermissions(MainActivity.this, new String[]{Manifest.permission.CALL_PHONE}, 1);
//                } else {
//                    call();
//                }
//            }
//        });
    }

    public void clickView(View v) {
        if (v == makeCall) {
            /*判断用户是否已经给我们授过权了，相等表示已经授权了*/
            if (ContextCompat.checkSelfPermission(MainActivity.this, Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED) {
                /*向用户申请权限*/
                    /*Activity实例
                      要申请的权限数组
                      请求码
                     */
                ActivityCompat.requestPermissions(MainActivity.this, new String[]{Manifest.permission.CALL_PHONE}, 1);
            } else {
                call();
            }
        }
    }

    private void call() {
        Intent intent = new Intent(Intent.ACTION_CALL);
        intent.setData(Uri.parse("tel:15270436239"));
        startActivity(intent);
    }

    /**
     * @param requestCode
     * @param permissions
     * @param grantResults 用户授权的结果
     */
    @Override
    public void onRequestPermissionsResult(int requestCode,
                                           @NonNull @org.jetbrains.annotations.NotNull String[] permissions,
                                           @NonNull @org.jetbrains.annotations.NotNull int[] grantResults) {
        switch (requestCode) {
            case 1:
                if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    call();
                } else {
                    Toast.makeText(this, "You denied the permission", Toast.LENGTH_SHORT).show();
                }
                break;
            default:
        }
    }
}
```

`manifest`

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.runtimepermissiontest">
    <uses-permission android:name="android.permission.CALL_PHONE"/>

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.RuntimePermissionTest">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

## 2. 读取手机通讯录

```java
private void readContacts() {
    Cursor cursor = null;
    cursor = getContentResolver().query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI, null, null, null, null);
    if (cursor != null) {
        while (cursor.moveToNext()) {
            String displayName = cursor.getString(cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME));
            String number = cursor.getString(cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER));
            mContentsList.add(new Contents(displayName, number));
        }
    }
    ContentsAdapter contentsAdapter = new ContentsAdapter(mContentsList);
    recyclerView.setAdapter(contentsAdapter);
    cursor.close();
}

@Override
public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    switch (requestCode) {
        case 1:
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                readContacts();
            }else {
                Toast.makeText(this, "You denied the permission", Toast.LENGTH_SHORT).show();
            }
            break;
        default:
            break;
    }
}
```



# 8. 多媒体

## 1. 使用通知

> Notification

`xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <Button
        android:id="@+id/send_notice"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Send notice"/>

</LinearLayout>
```

`activity`

```java
package com.example.notification;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.NotificationCompat;

import android.app.Notification;
import android.app.NotificationChannel;
import android.app.NotificationManager;
import android.graphics.BitmapFactory;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button sendNotice = findViewById(R.id.send_notice);
        sendNotice.setOnClickListener(this);
    }

    @Override
    public void onClick(View view) {
        switch (view.getId()){
            case R.id.send_notice:
                /*先通过getSystemService获得一个通知管理器*/
                NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
                //高版本需要渠道
                if(Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.O){
                    //只在Android O之上需要渠道，这里的第一个参数要和下面的channelId一样
                    NotificationChannel notificationChannel = new NotificationChannel("1","name",NotificationManager.IMPORTANCE_HIGH);
                    //如果这里用IMPORTANCE_NOENE就需要在系统的设置里面开启渠道，通知才能正常弹出
                    manager.createNotificationChannel(notificationChannel);
                }
                /*创建一个Notification对象并设置其中的内容*/
                Notification notification = new NotificationCompat.Builder(this, "1")
                        .setContentTitle("This is content title")
                        .setContentText("This is content text")
                        /*通知被创建的时间*/
                        .setWhen(System.currentTimeMillis())
                        .setSmallIcon(R.mipmap.ic_launcher)
                        .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher))
                        .build();
                /*保证每条通知的id不同*/
                manager.notify(1, notification);
                break;
        }
    }
}
```

通知无法点击：可以使用PendingIntent。

```java
@Override
public void onClick(View view) {
    switch (view.getId()){
        case R.id.send_notice:
            /*设置跳转意图*/
            Intent intent = new Intent(this, NotificationActivity.class);
            /*创建PendingIntent对象：
                context
                传入0
                Intent对象
                传入0
             */
            PendingIntent pi = PendingIntent.getActivity(this, 0, intent, 0);
            
            /*先通过getSystemService获得一个通知管理器*/
            NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
            //高版本需要渠道
            if(Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.O){
                //只在Android O之上需要渠道，这里的第一个参数要和下面的channelId一样
                NotificationChannel notificationChannel = new NotificationChannel("1","name",NotificationManager.IMPORTANCE_HIGH);
                //如果这里用IMPORTANCE_NOENE就需要在系统的设置里面开启渠道，通知才能正常弹出
                manager.createNotificationChannel(notificationChannel);
            }
            /*创建一个Notification对象并设置其中的内容*/
            Notification notification = new NotificationCompat.Builder(this, "1")
                    .setContentTitle("This is content title")
                    .setContentText("This is content text")
                    /*通知被创建的时间*/
                    .setWhen(System.currentTimeMillis())
                    .setSmallIcon(R.mipmap.ic_launcher)
                
                    .setContentIntent(pi)
                
                    .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher))
                    .build();
            /*保证每条通知的id不同*/
            manager.notify(1, notification);
            break;
    }
}
```

点击通知跳转到需要的地方。不过这时候通知还没有被关闭。

1. 直接设置

   ```xml
   /*设置点击后关闭通知*/
   .setAutoCancel(true)
   ```

2. 在跳转到的界面上设置

   ```java
   public class NotificationActivity extends AppCompatActivity {
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_notification);
           NotificationManager manager = (NotificationManager) getSystemService((NOTIFICATION_SERVICE));
           /*关闭id为1的通知*/
           manager.cancel(1);
       }
   }
   ```

## Android8.0后的通知管理 

> Notification通知在8.0后新加入了通知渠道(NotificationChannel)和通知渠道组(NotificationChannelGroup)
>
> [ Android 8.0 后通知、通知渠道、通知渠道组使用简介_WindGrin_的博客-CSDN博客](https://blog.csdn.net/WindGrin_/article/details/97143395)

## 2. 摄像与相册

## 2.  音乐

```java
        /*基于本地音频加载创建mediaPlayer对象*/
        mediaPlayer = MediaPlayer.create(BarrierActivity.this, R.raw.bgm);

        /*播放音乐*/
        if (!mediaPlayer.isPlaying()) {
            mediaPlayer.start();
            /*循环播放*/
            mediaPlayer.setLooping(true);
        }
        
```

# 通过一个demo来理解Android的MVC架构

安卓基于MVC架构设计——Model-View-Controller

模型：用户的数据

视图：用户能看到的界面

控制器：控制这些界面与数据交互的中间件

**GeoQuiz项目：**

> 界面给出一些问题，由用户进行选择对或者错，再给出用户的选择是否正确。

**AndroidManifest.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.geoquiz">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.GeoQuiz">
        <activity android:name=".QuizActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

**strings.xml**

```xml
<resources>
    <string name="app_name">GeoQuiz</string>
    <string name="question_0">hhh</string>
    <string name="question_1">iii</string>
    <string name="question_2">jjj</string>
    <string name="question_3">kkk</string>
    <string name="question_4">lll</string>
    <string name="question_5">mmmm</string>
    <string name="ture_button">TRUE</string>
    <string name="false_button">FALSE</string>
    <string name="next_button">NEXT</string>
    <string name="correct_toast">CORRECT</string>
    <string name="incorrect_toast">INCORRECT</string>
</resources>
```

## View层

**activity_quiz.xml**

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    tools:context=".QuizActivity"
    android:orientation="vertical">

    <TextView
        android:id="@+id/question_text_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="24dp"
        android:text="@null"/>
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center">
        <Button
            android:id="@+id/true_button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/ture_button"/>
        <Button
            android:id="@+id/false_button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/false_button"/>

    </LinearLayout>
    <Button
        android:id="@+id/next_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/next_button"
        />

</LinearLayout>
```

## Model层

**Question**类

```java
package com.example.geoquiz;

public class Question {
    private int mTextResId;
    private boolean mAnswerTrue;

    public int getTextResId() {
        return mTextResId;
    }

    public void setTextResId(int textResId) {
        mTextResId = textResId;
    }

    public boolean isAnswerTrue() {
        return mAnswerTrue;
    }

    public void setAnswerTrue(boolean answerTrue) {
        mAnswerTrue = answerTrue;
    }

    public Question(int mTextResId, boolean mAnswerTrue) {
        this.mTextResId = mTextResId;
        this.mAnswerTrue = mAnswerTrue;
    }

}
```

## Controller层

```java
package com.example.geoquiz;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

public class QuizActivity extends AppCompatActivity {
    private Button mTrueButton;
    private Button mFalseButton;
    private Button mNextButton;
    private TextView mQuestionTextView;
    private Question[] mQuestionBank = new Question[]{
            new Question(R.string.question_0, true),
            new Question(R.string.question_1, false),
            new Question(R.string.question_2, false),
            new Question(R.string.question_3, true),
            new Question(R.string.question_4, false),
            new Question(R.string.question_5, true),
    };
    private int mCurrentIndex = 0;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_quiz);
        mTrueButton = (Button) findViewById(R.id.true_button);
        mTrueButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                checkAnswer(true);
            }
        });
        mFalseButton = (Button) findViewById(R.id.false_button);
        mFalseButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                checkAnswer(false);
            }
        });
        mQuestionTextView = (TextView) findViewById(R.id.question_text_view);
        mNextButton = (Button) findViewById(R.id.next_button);
        mNextButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                mCurrentIndex = (mCurrentIndex + 1) % mQuestionBank.length;
                /*更新显示*/
                updateQuestion();
            }
        });
        /*初始显示*/
        updateQuestion();
    }

    private void checkAnswer(boolean userPressedTrue) {
        /*获得问题的答案*/
        boolean answerIsTrue = mQuestionBank[mCurrentIndex].isAnswerTrue();
        int messageResId = 0;
        if (userPressedTrue == answerIsTrue) {
            messageResId = R.string.correct_toast;
        }else {
            messageResId = R.string.incorrect_toast;
        }
        Toast.makeText(QuizActivity.this, messageResId, Toast.LENGTH_SHORT).show();
    }

    private void updateQuestion() {
        int question = mQuestionBank[mCurrentIndex].getTextResId();
        mQuestionTextView.setText(question);
    }
}
```

可以看出：Controller层是最复杂的，其通过操作数据层类创建的对象数组与用户点击事件进行对用户进行反馈。

# 生命周期的理解

1. onCreate()：创建(完整生存期开始)
2. onStart()：开始(可见生存期开始)
3. onResume()：启动(前台生存期开始)
4. onPause()：暂停(前台生存期结束)
5. onStop()：停止(可见生存期结束)
6. onDestroy()：销毁(完整生存期结束)
7. onRestart()：重新开始(可见生存期重新开始)

列一个表格来说明各方法调用

| 操作前状态 | 操作                 | 被执行的方法(依次) | 操作后状态        |
| ---------- | -------------------- | ------------------ | ----------------- |
| 无         | 进入应用             | 1，2，3            | 3显示             |
| 3          | 单击后退键           | 4，5，6            | 6被销毁           |
| 无(已销毁) | 重新进入             | 1，2，3            | 3显示             |
| 3          | 单击home键           | 4，5               | 5停止             |
| 3          | 进入任务管理器       | 4，5               | 5停止(可能被回收) |
| 3          | 进入下一个界面       | 4，5               | 5停止             |
| 别的界面   | 进入刚才停止的原界面 | 7，2，3            | 3显示             |

# 7. 服务

> 服务是Android中实现程序后台运行的解决方案

## 1. 线程的两种创建方式

* 继承

  ```java
  class MyThread extened Thread{
      @Override
      public void run(){
          //具体实现逻辑
      }
  }
  new MyThread().start();
  ```

* 实现

  ```java
  Class MyThread implements Runnable{
      @Override
      public void run(){
          //具体实现逻辑
      }
  }
  MyThread myThread = new MyThread();
  new Thread(myThread).start();
  new Thread(new Runnable(){
      @Override
      public void run(){
          //具体实现逻辑
      }    
  }).start();
  ```

## 2. 异步消息处理机制

* Message：用于在线程之间传递消息
* Handler：处理者，用于发送sendMessage与处理消息handleMessage
* MessageQueue：消息队列，存放用Handler发送的消息，每个线程只会有一个MessageQueue对象
* Looper：取出MessageQueue的消息

异步流程：

1. 创建一个Handler对象，重写handleMessage()方法
2. 子线程需要操作主线程的一些东西时，创建一个Message对象，通过Handler将这条消息发送出去
3. 消息进入MessageQueue队列等待被处理
4. Loop一直尝试从MessageQueue中读取，分发给Handler的handleMessage()方法， 因为其是在主线程中的，所以可以在这里操作

```java
package com.example.androidthreadtest;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private Button mButton;
    private TextView mTextView;
    /*自己定义一个what*/
    public static final int UPDATE_TEXT = 1;
    /*1. 在主线程中创建Handler*/
    private Handler mHandler = new Handler(){
        /*接收返回的消息*/
        /*4. 分发给Handler的handleMessage()方法*/
        @Override
        public void handleMessage(Message msg){
            switch (msg.what){
                case UPDATE_TEXT:
                    mTextView.setText("hkkkk");
                    break;
                default:
                    break;
            }
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mButton = findViewById(R.id.change_text);
        mTextView = findViewById(R.id.text);
        mButton.setOnClickListener(this);
    }

    @Override
    public void onClick(View view) {
        switch (view.getId())
        {
            case R.id.change_text:
                /*创建一个线程，发送消息*/
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        /*2. 创建一个Message对象，通过Handler将这条消息发送出去*/
                        /*消息处理*/
                        Message message = new Message();
                        message.what = UPDATE_TEXT;
                        /*发送消息*/
                        mHandler.sendMessage(message);
                    }
                }).start();
                break;
        }
    }
}
```

服务有两种设置方法：

1. 启动与停止(单纯的用Intent启动，然后就不能通过Activity去控制它了)

   ```java
               case R.id.start_service:
                   Intent startIntent = new Intent(this, MediaService.class);
                   startService(startIntent);
                   break;
               case R.id.stop_service:
                   Intent stopIntent = new Intent(this, MediaService.class);
                   stopService(stopIntent);
                   break;
   ```

   在Service类里的任何一个地方调用stopSelf()方法也可以停止。

2. 绑定与解绑(可以通过Activity去控制)

采用绑定与解绑的方法更可控，大概思路如下：

通过Activity里绑定服务——>调用onServiceConnected创建mMediaBinder对象——>这个对象就是服务类onBind返回的值，向下转型即可得到内部类对象，这就可以实现内部类方法。

![Service](https://i.loli.net/2021/07/23/2i78xQBywgj1pPL.png)

`xml`设置三个按键

`DownloadService`

```java
package com.example.downloadservice;

import android.app.Notification;
import android.app.NotificationChannel;
import android.app.NotificationManager;
import android.app.PendingIntent;
import android.app.Service;
import android.content.Intent;
import android.graphics.BitmapFactory;
import android.os.Binder;
import android.os.Build;
import android.os.IBinder;
import android.util.Log;

import androidx.annotation.Nullable;
import androidx.annotation.RequiresApi;
import androidx.core.app.NotificationCompat;

public class DownloadService extends Service {
    private DownloadBinder mBinder = new DownloadBinder();

    class DownloadBinder extends Binder {
        public void startDownload() {

        }

        public int getProgress() {
            return 0;
        }
    }

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return mBinder;
    }

    @RequiresApi(api = Build.VERSION_CODES.O)
    @Override
    public void onCreate() {
        super.onCreate();
        
        /*以下创建了前台服务*/
        Intent intent = new Intent(this, MainActivity.class);
        PendingIntent pi = PendingIntent.getActivity(this, 0, intent, 0);
        /*先通过getSystemService获得一个通知管理器*/
        NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
        //高版本需要渠道
        //只在Android O之上需要渠道，这里的第一个参数要和下面的channelId一样
        NotificationChannel notificationChannel = new NotificationChannel("1", "name", NotificationManager.IMPORTANCE_HIGH);
        //如果这里用IMPORTANCE_NOENE就需要在系统的设置里面开启渠道，通知才能正常弹出
        manager.createNotificationChannel(notificationChannel);
        /*创建一个Notification对象并设置其中的内容*/
        Notification notification = new NotificationCompat.Builder(this, "1")
                .setContentTitle("This is content title")
                .setContentText("This is content text")
                /*通知被创建的时间*/
                .setWhen(System.currentTimeMillis())
                .setSmallIcon(R.mipmap.ic_launcher)
                .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher))
                .setContentIntent(pi)
                .build();
        startForeground(1, notification);
    }

    private static final String TAG = "DownloadService";
    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.d(TAG, "onDestroy()");
    }
}
```

`Activity`

```java
package com.example.downloadservice;

import androidx.appcompat.app.AppCompatActivity;

import android.content.ComponentName;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.util.Log;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private DownloadService.DownloadBinder mDownloadBinder;
    private ServiceConnection mConnection = new ServiceConnection() {
        /*绑定后调用*/
        @Override
        public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
            mDownloadBinder = (DownloadService.DownloadBinder) iBinder;
            mDownloadBinder.startDownload();
            mDownloadBinder.getProgress();
        }

        @Override
        public void onServiceDisconnected(ComponentName componentName) {

        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button1 = findViewById(R.id.button1);
        Button button2 = findViewById(R.id.button2);
        Button button3 = findViewById(R.id.button3);
        button1.setOnClickListener(this);
        button2.setOnClickListener(this);
        button3.setOnClickListener(this);
    }

    private static final String TAG = "MainActivity";

    @Override
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.button1:
                Intent bindIntent = new Intent(MainActivity.this, DownloadService.class);
                /*会去调用onServiceConnected*/
                bindService(bindIntent, mConnection, BIND_AUTO_CREATE);
                break;
            case R.id.button2:
                unbindService(mConnection);
                break;
            case R.id.button3:
                Log.d(TAG, "Thread id is " + Thread.currentThread().getId());
                Intent intentService = new Intent(this, MyIntentService.class);
                startService(intentService);
                break;
        }
    }
}
```

## 服务生命周期

* onCreate() 服务第一次创建时(绑定也算)
* onStartCommand() 服务创建时
* onBind() 绑定时
* onDestroy() 销毁时

每个服务只有一个实例，无论startService()多少次都只需要调用一次stopServie()或stopSelf()方法

## 使用IntentService

搞了半天，服务的代码都是主线程中运行的。应该把耗时的逻辑放入线程中去执行

```java
@Override
public int onStartCommand(Intent intent, int flags, int startId) {
    new Thread(new Runnable() {
        @Override
        public void run() {
            /*具体实现逻辑*/
            
            /*停掉*/
            stopSelf();
        }
    }).start();
    return super.onStartCommand(intent, flags, startId);
}
```

使用IntentService可以自动实现异步与停止的效果。

```java
package com.example.downloadservice;

import android.app.IntentService;
import android.content.Intent;
import android.util.Log;

import androidx.annotation.Nullable;

public class MyIntentService extends IntentService {
    public MyIntentService(){
        super("MyIntentService");
    }
    /**
     * @param name
     * @deprecated
     */
    public MyIntentService(String name) {
        super(name);
    }

    private static final String TAG = "MyIntentService";

    /*这个方法就默认是在子线程中执行的*/
    @Override
    protected void onHandleIntent(@Nullable Intent intent) {
        /*打印当前线程的id*/
        Log.d(TAG, "Thread id is " + Thread.currentThread().getId());
    }

    /*运行结束时会自动停止*/
    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.d(TAG, "onDestroy executed");
    }
}
```

不要忘记在`AndroidManifest.xml`中去注册

# 8. 动画

![Android动画](https://i.loli.net/2021/07/28/9WSpjZYBCIFg6Ev.png)

动画应文件也是以xml为后缀的，放在res/anim目录下。

从Animation继承的属性：

>* android:duration            动画持续时间，以毫秒为单位
>* android:fillAfter              如果设置为true，控件动画结束时，保持动画最后时的状态
>* android:fillBefore           如果设置为true，控件动画结束时，还原到动画开始前的状态
>* android:fillEnabled        与android:fillBefore 效果相同
>* android:repeatCount     重复次数 如果设置为infinite，则表示无限次重复动画
>* android:repeatMode     重复类型，有reverse和restart两个值，reverse表示倒叙回放，restart表示重新播放一遍，必须与android:repeatCount一起使用才有效果，因为这里的意义是重复类型，即回放的动作   
>* android:interpolator      插值器，指定动作的效果，比如弹跳，快速等

| Interpolator class               | Resource   ID                                    |                                              |
| -------------------------------- | ------------------------------------------------ | -------------------------------------------- |
| AccelerateDecelerateInterpolator | @android:anim/accelerate_decelerate_interpolator | 在动画开始和结束的时候比较慢，中间比较快     |
| AccelerateInterpolator           | @android:anim/accelerate_interpolator            | 动画开始时比较慢，然后加速                   |
| AnticipateInterpolator           | @android:anim/anticipate_interpolator            | 开始的时候向后然后向前甩                     |
| AnticipateOvershootInterpolator  | @android:anim/anticipate_overshoot_interpolator  | 开始的时候向后然后向前甩一定值后返回最后的值 |
| BounceInterpolator               | @android:anim/bounce_interpolator                | 动画结束的时候弹起                           |
| CycleInterpolator                | @android:anim/cycle_interpolator                 | 动画循环播放特定的次数，速率改变沿着正弦曲线 |
| DecelerateInterpolator           | @android:anim/decelerate_interpolator            | 在动画开始的地方快然后慢                     |
| LinearInterpolator               | @android:anim/linear_interpolator                | 以常量速率改变                               |
| OvershootInterpolator            | @android:anim/overshoot_interpolator             | 向前甩一定值后再回到原来位置                 |

## 补间动画Tween

> 补间的意思是电脑帮你补上中间的，也就是你只用给定起始与终止的效果即可

### 透明度(AlphaAnimation)

> * android:fromAlpha 开始透明度0.0~1.0
> * android:toAlpha 结束透明度

### 缩放(ScaleAnimation)

> * android:fromXScale           起始X方向上相对自身的缩放比例，浮点值，比如1.0自身无变化，0.5表示缩小一倍，2.0表示放大2倍；
> * android:toXScale               结尾X方向上相对自身的缩放比例，浮点值；
> * android:fromYScale           起始Y方向上相对自身的缩放比例，浮点值；
> * android:toYScale               结尾Y方向上相对自身的缩放比例，浮点值；
> * android:pivotX                  缩放起点X坐标，可以是数值，百分数，百分数P，三种样式，比如50，50%，50%P；当为数值时：表示在view的左上角，即原点处加上50px作为起始缩放原点，当为百分数时：如果是50%则表示，当前控件的左上角加上自身宽度的50%作为缩放的起点（图片横向中点），当为百分数P时：如果是50%P，则表示，当前左上角加上父控件宽度的50%作为起始点X轴坐标；（后面演示）
> * android:pivotY                  缩放起点Y轴坐标，取值及意义和android:pivotX 一致；

### 位移(TranslateAnimation)

> * android:fromXDelta            起始点X轴坐标，可以是数值，百分数，百分数p三种样式，比如50，50%，50%p；具体意义已经在scale标签中讲述，这里就不作解释了 100%p表示看不到的最右边，-100%p看不到的最左边
> * android:fromYDelta            起始点Y轴坐标，意义同上100%表示看不到的最下边
> * android:toXDelta                结束点X轴坐标
>
> * android:toYDelta                结束点Y轴坐标

### 旋转(RotateAnimation)

> * android:fromDegrees      开始旋转的角度位置，正值表示顺时针方向的度数，负值表示逆时针方向的度数
> * android:toDegrees          结束旋转的角度位置，正值表示顺时针方向的度数，负值表示逆时针方向的度数
> * android:pivotX                 旋转中心点X轴坐标，可以是数值，百分数，百分数p三种样式，比如50，50%，50%p；具体意义已经在scale标签中讲述，这里就不一 一作解释了
> * android:pivotY                旋转中心点Y轴坐标，意义同上

### set定义动作合集

多个动画一起使用，要用set，set属全部继承于Animation

* 在Set标签中设置android:repeatCount是无效的，只能在子动画中重复次数才有效果。如果没有设置则默认为1次。
* 对于android:duration和android:repeatMode则优先使用Set标签中设置的属性，如果Set标签中没有设置就使用子控件中的（Set没有设置动画时长，子动画一定要设置时长）。
* android:fillAfter、android:fillBefore、android:fillEnabled属性只能在Set标签中设置，如果没有设置则使用默认的android:fillBefore="true"，在子动画中设置是无效的。

## 属性动画

