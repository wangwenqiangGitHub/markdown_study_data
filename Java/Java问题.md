# 什么是值传递？什么是引用传递？举例说明java中是什么传递？

==值传递和引用传递，属于函数调用时参数的<font color = red>求值策略</font>，这是对调用函数时，求值和传值方式的描述，而非传递的内容的类型。==

这里的内容，指的是值类型还是引用类型，是值还是指针。值类型/引用类型，是用于区分两种内存<font color =  red>分配方式</font>，值类型在调用栈上分配，引用类型在堆上分配。

> 值传递：是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。C语言中只有值传递，即使是传递指针，也是将地址值传递过去，本质上还是值传递。

> 引用传递(Pass By Reference)：指的是方法传参时，传递的是参数本身，因此对参数进行任意修改都会影响原内容。

作者：沉默王二
链接：https://www.zhihu.com/question/385114001/answer/1393887646
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



七年前，我从温和湿润的苏州回到古色古香的洛阳，抱着一幅“天下我有”的心态“约谈”了几位面试官。其中有一位叫老马，让我印象深刻。因为他当时扔了一个面试题把我砸懵了：**说说 Java 到底是值传递还是引用传递吧**。

我当时年轻气盛，自认为所有的面试题都能对答如流，没想到被老马“刁难”了——原来洛阳这块互联网的荒漠也有技术专家啊。现在回想起来，脸上不自觉地就泛起了羞愧的红晕：当时真菜！刚好题主也在问这个问题，那我就来好好回答一下。

将参数传递给方法有两种常见的方式，一种是“值传递”，一种是“引用传递”。C 语言本身只支持值传递，它的衍生品 C++ 既支持值传递，也支持引用传递，而 Java 只支持值传递。

### **01、值传递 VS 引用传递**

首先，我们必须要搞清楚，到底什么是值传递，什么是引用传递，否则，讨论 Java 到底是值传递还是引用传递就显得毫无意义。

<font color = red>当一个参数按照值的方式在两个方法之间传递时，调用者和被调用者其实是用的两个不同的变量——被调用者中的变量（原始值）是调用者中变量的一份拷贝，对它们当中的任何一个变量修改都不会影响到另外一个变量。</font>

<font color = red>而当一个参数按照引用传递的方式在两个方法之间传递时，调用者和被调用者其实用的是同一个变量，当该变量被修改时，双方都是可见的。</font>

Java 程序员之所以容易搞混值传递和引用传递，主要是因为 Java 有两种数据类型，一种是基本类型，比如说 int，另外一种是引用类型，比如说 String。

基本类型的变量存储的都是实际的值，而引用类型的变量存储的是对象的引用——指向了对象在内存中的地址（从这点上来看，java中的引用更像是C语言中的指针）。值和引用存储在 stack（栈）中，而对象存储在 heap（堆）中。

![img](https://pic4.zhimg.com/v2-d4b94901d057d8f7027bb826b9c4f6cb_b.jpg)![img](https://pic4.zhimg.com/80/v2-d4b94901d057d8f7027bb826b9c4f6cb_720w.jpg)

之所以有这个区别，是因为：

- 栈的优势是，存取速度比堆要快，仅次于直接位于 CPU 中的寄存器。但缺点是，栈中的数据大小与生存周期必须是确定的。
- 堆的优势是可以动态地分配内存大小，生存周期也不必事先告诉编译器，Java 的垃圾回收器会自动收走那些不再使用的数据。但由于要在运行时动态分配内存，存取速度较慢。

### **02、基本类型的参数传递**

众所周知，Java 有 8 种基本数据类型，分别是 int、long、byte、short、float、double 、char 和 boolean。它们的值直接存储在栈中，每当作为参数传递时，都会将原始值（实参）复制一份新的出来，给形参用。形参将会在被调用方法结束时从栈中清除。

来看下面这段代码：

```text
public class PrimitiveTypeDemo {
    public static void main(String[] args) {
        int age = 18;
        modify(age);
        System.out.println(age);
    }

    private static void modify(int age1) {
        age1 = 30;
    }
}
```

1）main 方法中的 age 是基本类型，所以它的值 18 直接存储在栈中。

2）调用 `modify()` 方法的时候，将为实参 age 创建一个副本（形参 age1），它的值也为 18，不过是在栈中的其他位置。

3）对形参 age 的任何修改都只会影响它自身而不会影响实参。

![img](https://pic2.zhimg.com/v2-e2d8ba2a29989c33319d9034f6575609_b.jpg)![img](https://pic2.zhimg.com/80/v2-e2d8ba2a29989c33319d9034f6575609_720w.jpg)

### **03、引用类型的参数传递**

来看一段创建引用类型变量的代码：

```text
Writer writer = new Writer(18, "沉默王二");
```

writer 是对象吗？还是对象的引用？为了搞清楚这个问题，我们可以把上面的代码拆分为两行代码：

```text
Writer writer;
writer = new Writer(18, "沉默王二");
```

假如 writer 是对象的话，就不需要通过 new 关键字创建对象了，对吧？那也就是说，writer 并不是对象，在“=”操作符执行之前，它仅仅是一个变量。那谁是对象呢？`new Writer(18, "沉默王二")`，它是对象，存储于堆中；然后，“=”操作符将对象的引用赋值给了 writer 变量，于是 writer 此时应该叫对象引用，它存储在栈中，保存了对象在堆中的地址。

每当引用类型作为参数传递时，都会创建一个对象引用（实参）的副本（形参），该形参保存的地址和实参一样。

来看下面这段代码：

```text
public class ReferenceTypeDemo {
    public static void main(String[] args) {
        Writer a = new Writer(18);
        Writer b = new Writer(18);
        modify(a, b);

        System.out.println(a.getAge());
        System.out.println(b.getAge());
    }

    private static void modify(Writer a1, Writer b1) {
        a1.setAge(30);

        b1 = new Writer(18);
        b1.setAge(30);
    }
}
```

1）在调用 `modify()` 方法之前，实参 a 和 b 指向的对象是不一样的，尽管 age 都为 18。

![img](https://pic1.zhimg.com/v2-b6718be0260c00bbfc44584f847126fc_b.jpg)![img](https://pic1.zhimg.com/80/v2-b6718be0260c00bbfc44584f847126fc_720w.jpg)

2）在调用 `modify()` 方法时，实参 a 和 b 都在栈中创建了一个新的副本，分别是 a1 和 b1，但指向的对象是一致的（a 和 a1 指向对象 a，b 和 b1 指向对象 b）。

![img](https://pic1.zhimg.com/v2-86cd249ce610240a0135bed73b23c114_b.jpg)![img](https://pic1.zhimg.com/80/v2-86cd249ce610240a0135bed73b23c114_720w.jpg)

3）在 `modify()` 方法中，修改了形参 a1 的 age 为 30，意味着对象 a 的 age 从 18 变成了 30，而实参 a 指向的也是对象 a，所以 a 的 age 也变成了 30；形参 b1 指向了一个新的对象，随后 b1 的 age 被修改为 30。

![img](https://pic2.zhimg.com/v2-3f5133cd310f2e2061886502b92ae731_b.jpg)![img](https://pic2.zhimg.com/80/v2-3f5133cd310f2e2061886502b92ae731_720w.jpg)

修改 a1 的 age，意味着同时修改了 a 的 age，因为它们指向的对象是一个；修改 b1 的 age，对 b 却没有影响，因为它们指向的对象是两个。

程序输出的结果如下所示：

```text
30
18
```

果然和我们的分析是吻合的。

Java中只有值传递，无论是变量，还是对象的引用，都是使用值传递。

```java
@Test
public void Test1(){
    int a = 3;
    changeValue(a);
    System.out.println(a);
    String b = "bbb";
    changeValue(b);
    System.out.println(b);
    int[] array = new int[2];
    changeValue(array);
    System.out.println(array[0]);
    Person p = new Person("Tom", 12);
    changeValue(p);
    System.out.println(p);
}
public void changeValue(int a){
    a = 1;
}
public void changeValue(String a){
    a = "aaa";
}
public void changeValue(int[] a){
//        int[] b = new int[2];
//        a = b;
    a[0] = 1;
}
public void changeValue(Person p){
    Person k = new Person("Tom", 12);
    System.out.println("k = " + k);
    p = k;
    System.out.println("p = " + p);
}
/*
3
bbb
1 
k = io.github.zjj.annotate.cases.Person@1d371b2d
p = io.github.zjj.annotate.cases.Person@1d371b2d
io.github.zjj.annotate.cases.Person@543c6f6d
*/
```

显而易见，变量和String类型一定是值传递，可是为什么数组会被改变值？因为在拷贝数组时，他引用的还是原来堆中的那个地址，a与array是相同数组的别名，因此在a中做的修改array中可以看到。举个反例，如上，a = b， 如果是引用传递，array肯定会改变，而它没有。同理可得，自定义类型也是如此。

# IDEA 

```vmoptions
-Xms128m 16G内存的机器可以尝试设置为-Xms512m(设置初始的内存数，增加该值可以提高Java程序的启动速度)
-Xmx750m 16G内存的机器可尝试设置为 -Xms1500m(设置最大内存数，提高该值，可以减少内存Garage收集的频率，提高程序性能)
-XX:ReservedCodeCacheSize=240m 16G内存的机器可以尝试设置为-XX:ReservedCodeCacheSize=500m (保留代码占用的内存容量)
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-XX:CICompilerCount=2
-Dsun.io.useCanonPrefixCache=false
-Djdk.http.auth.tunneling.disabledSchemes=""
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
-Djdk.attach.allowAttachSelf=true
-Dkotlinx.coroutines.debug=off
-Djdk.module.illegalAccess.silent=true
-Deditable.java.tet.console=true
```

#  注释

文档注释：

注释内容可以被`javadoc`解析，生成一套以网页文件形式体现的该程序的说明文档。

```java
/**
@author 
@version
*/
```

命令行：

> `javadoc -d mydoc -author -version HelloWorld.java`

`mydoc`是文件夹的名字。

# Scanner输入问题

> java中提供这样一个文本扫描器

```java
Scanner in = new Scanner(System.in);
```

通过上面这个对象的一些方法，可以使用户从System.in也就是控制台输入一个数读入

## 方法

1. 判断

```java
boolean hasNextInt();
boolean hasNextFloat();
boolean hasNextLine();//扫描器中存在另一行，返回true，可以读空行，返回true
```

常用上面这些方法去判断输入是否正确，而这些方法本身也有输入的作用，它们都可以读入回车，把其当作空行，返回true。

输入完的数据就留在了缓冲区中，要清掉。

```java
String nextLine();//执行当前行，并返回路过的输入信息，可以读空行
String next(); //不能读空行
```

```java
import java.util.Scanner;

public class Change {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        while (!in.hasNextInt())//如果输入的不是int型，那输入就会留在缓冲区，它可以读入回车
        {
            System.out.print("a");            
            String k = in.nextLine();//这句有清除缓冲区的作用，清掉一行   
            System.out.println(k.length());
        }
    }
}
输入； 下面是三个空格



a
输出
a0
a0
a0
a1
```

可以看到，它可以读空格，但是只能是一输入就是空格的情况，先输入了别的东西就不行了。

# 设计模式

## MVC设计模式

将项目分成三个层次：视图模型层，控制器层，数据模型层

### 模型层 model主要处理数据

* 数据对象封装：model.bean/domain
* 数据库操作类：model.dao
* 数据库：moddel.db

### 控制层 contrtroller处理业务逻辑

* 应用界面相关：controller.activity
* 存放fragment：controller.fragment
* 显示列表的适配器：controller.adapter
* 服务相关：controller.service
* 抽取的基类：controller.base

### 视图层 view显示数据

* 相关工具类：view.utils
* 自定义：view view.ui

## 单例设计模式

> 某个类只能存在一个对象实例，构造器为private，用静态方法返回对象。

```java
public class SingletonTest {
    public static void main(String[] args) {
        Bank bank = Bank.getInstance();
    }
}
/**
 * 饿汉式：坏处：对象加载时间过长
 		 好处：线程安全
 */
class Bank
{
    private Bank() {
    }
    private static Bank instance = new Bank();

    static public Bank getInstance(){
        return instance;
    }
}

/**
 * 懒汉式：好处：延迟对象的创建
 		  目前坏处：线程不安全---->到多线程内容，再修改
 */
class Order
{
    private Order() {
    }
    private static Order instance = null;
    public static Order getInstance()
    {
        if (instance == null)
        {
            sychronized(Bank.class){
                if (instance == null)
                    instance = new Order();
            }
        }
        return instance;
    }
/*
    public static Order getInstance()
    {
        sychronized(Bank.class){
        	if (instance == null)
            	instance = new Order();
            return instance;
       	}
    }
*/
    public static sychronized Order getInstance()
    {
        if (instance == null)
            instance = new Order();
        return instance;
    }
}
```

## 模板方法设计模式

> 用抽象类提供一个通用的模板

```java
public abstract class Template {
    public void spendTime() {
        long start = System.currentTimeMillis();
        code();
        long end = System.currentTimeMillis();
    }

    public abstract void code();
}

class SubTemplate extends Template {
    @Override
    public void code() {
        int k = 0;
        for (int i = 2; i < 100; i++) {
            boolean flag = true;
            for (int j = 2; j <= Math.sqrt(i); j++) {
                if (i % j == 0) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                k++;
                System.out.printf("%3d", i);
                if (k % 5 == 0)
                    System.out.println();
            }
        }
        boolean flag = 2 < Math.sqrt(5);
        System.out.println(flag);

    }
}

class Main{
    public static void main(String[] args) {
        new SubTemplate().spendTime();
    }
}
```



# JDK,JRE,JVM

> JDK = JRE + Java 的开发工具（javac.exe, java.exe, javadoc.exe)
>
> JRE = JVM + Java核心类库



# JVM内存结构

编译完源程序 以后，生成一个或多个字节码文件。

我们使用jvm中的类加载器和解释器对生成的字节码文件进行解释运行。意味着需要将字节码文件对应的类加载到内存中，涉及到内存解析。

![image-20200911100311504](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20200911100311504.png)

# java内存为成5个部分：

1. 栈（Stack）：存放的都是方法中的局部变量。方法的运行都是在栈中。

2. 堆（heap）：new，这个内存里的东西都有一个地址值，堆内存里的数据都有一个默认值。

3. 方法区（Method Area）：存储.class相关信息，包含方法的信息。

4. 本地方法栈（Native Method Stack）：与操作系统相关。

5. 寄存器（pc Register）：与CPU相关。

   

   ![image-20200701154138986](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20200701154138986.png) 

<img src="C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20200702221051680.png" alt="image-20200702221051680"  />



## 成员变量与局部变量

1. 位置
   成员变量：在类中，方法外
   局部变量：在方法中

2. 作用范围不一样

   成员变量：整个类

   局部变量：方法中使用

3. 默认值
   成员变量：有默认值，与数组一样
   局部变量：没有默认值，没有赋值的不能用

4. 内存的位置不同

   成员变量：堆

   局部变量：栈

5. 生命周期

   成员变量：对象创建诞生，垃圾回收而消失

   局部变量：方法进栈诞生，出栈消失