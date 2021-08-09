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

> 某个只能存在一个对象实例，构造器为private，用静态方法类内部 返回对象。

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

# 继承 

通过扩展一个类的过程称为**继承(inheritance)**

## 定义：

```java
public class Manager extends Employee
{
    added methods and fields
}
```

关键字`extend`表明正在构造的新类派生于一个已存在的类。已存在的类叫做超类（superclass)，基类(base class)，父类（parent class)，新类就叫子类(subclass)，派生类(derived class)，或者孩子类（child class）。叫做超类的原因我想应该是它可以扩展的方面更多，有更多的发展空间吧，但是究二者功能来说，子类应该是更多功能的。

也可以用**超集**与**子集** 来描述两者之间的关系，因为经理本身也是员工，所有员工的集合包含所有经理组成的集合。

## 覆盖方法

既然已经搞出来一个子类，必定是让它做与超类不同的事情，所以超类中的一些方法对子类不适用，但是它们有的时候是做同一件事，也是同名字的，比如下面员工类与经理类的薪水问题。

```java
public class Employee{
    private double salary;
    public double getSalary()
    {
        return salary;
    }
}
public class Manager extends Employee{
    private double bonus;
    public double getSalary()
    {
        double baseSalary = super.getSalary();
        return baseSalary + bonus;
    }
} 
```

因为`Manager`类的方法不能访问`Employee`的私有字段，在这里也就是不能访问`salary`，所以我们只能通去方法的返回来获得`salary`，但是这里还用了一个`super`，因为没有这个它就变成了调用自身了，我们在这里要用到超类，所以用`super`来表示这是用超类中方法。

**覆盖（重写）与重载方法的不同**

**重载是在同一个类中的，它的方法名相同，参数列表不同，无关返回值。**

**重写则是在不同类中，且重写方法在子类中，参数列表，方法名与返回值都要相同。在javaSE5后，覆盖重写方法可以返回基类方法返回值类型的某种导出类型**

## 子类构造器

子类构造器调用前，必须要调用超类构造器，而且要放在第一个位置。与原先一样，编译器会默认给你一个无参构造器，但是这个构造器超类中如果没有的话就会报错。你自己也可以写一个调用父类重载构造。

```java
public Manager (String name, double salary, int year, int month, int day)
{
    super(name, salary, year, month, day);
    bonus = 0;
}
```

这里的`super`指的是调用超类中的构造器的简写。**使用`super`调用构造器的语句必须是子类构造器的第一条语句。**

### super与this关键字

`this`有两个含义：

1. 在本类的成员方法中，访问本类的成员变量
2. 在本类的成员方法中，访问本类的成员方法
3. 在本类的构造方法中，访问本类的另一个构造方法，this也要放在第一个，不能与super同时使用

`super`也有两个含义

1. 在子类的成员方法中，访问父类的成员方法
2. 在子类的构造方法中，访问父类的构造方法
3. 在子类的成员方法中，访问父类的成员变量

但是`super`并不是一个对象的引用，不能将值赋给另一个对象变量，它只是编译器调用超类方法的特殊关键字。

## 多态与动态绑定

> 一个对象可以指示多种实际类型的现象称为**多态（polymorphism)**，在远行时能够自动的选择适当的方法，称为**动态绑定（dynamic binding)**

超类可以引用子类，而子类不能引用超类。子类是超类的一部分，所有的经理也是员工。但所有的员工并不一定是经理。如果要实现超类引用赋给子类变量，要进行强制类型转换。转换前要先检查能否转换。

```java
if (staff[1] instanceof Manager)
{
    boss = (Manager) staff[1];
    ...
}
```

> `instanceof` 用来测试它左边的对象是否是它右边类的实例

虚拟机知道变量实际的引用类型。

## 理解方法调用

假设要调用`x.f(args)`，隐式参数`x`声明为类`C`的一个对象。

1. 编译器查看对象的声明类型和方法名，找到所有可能会被调用的方法。

2. 确定方法调用中提供的参数类型，如果同名方法中存在一个完全匹配的方法，就选择这个方法，这就叫**重载解析（overloading resolution）**，如果没有找到或者有多个匹配，就会报错。

3. 如果是`private,static final`方法或构造器，那编译器可以准确的知道应该调用哪个方法，这就叫**静态绑定(static binding)**。与此同时， 如果要调用的方法依赖于隐式参数的实际类型， 那么必须在运行时使用**动态绑定**

4. 程序运行并用采用动态绑定调用方法时，虚拟机必须调用与x所引用对象实际类型相同的那个方法，假设`x`的实际类型是`D`，是`C`的子类，如果`D`中定义了`f（args）`方法，那就用`D`中的。否则就在`D`的超类`C`中寻找。

   为了实现这个搜索，虚拟机预先为每个类计算了一个**方法表(method table)**，列出了所有方法的签名和要调用的实际方法。真正调用的时候只需要查表就可以了。如查调用的有`super`那么编译器将对隐式参数超类的方法表进行搜索。

总的来说，就是：

1. 找
2. 比
3. 有无动态
4. 确定

## 阻止继承：final类和方法

> 如果希望阻止人们利用某个类定义子类。不允许扩展的类被称为`final`类。

```java
public final class Executive extends Manager
{
……
}
```

阻止派生`Executive`的子类，这里`Executive`是`Manager`的子类。

也可以对方法使用`final`， 这样子类就不能覆盖这个方法。

```java
public class Employee
{
    pbulic final String getName()
    {
        return name;
    }
}
```

## 抽象类

> 在类的继承层次中，上层的更具有一般性，更抽象，而其可能具有其所有子类的共同性质，但它们又可能总是相同，又没有必要总是去用子类覆盖，比如描述学生与员工是不同的，但他们都可以抽象为人，都可以有一个描述的方法。

包含一个或多个的类本身必须被声明为抽象的。

```java
public abstract class Person
{
    bpulic abstract String getDescription();
}
```

**抽象方法充当占位的角色，在子类中才具体实现。**

抽象不能实例化，如果一个类声明为`abstract`就不能创建这个类的对象，但可以创建一个具体子类的对象，根据多态抽象类可以引用其非抽象子类的对象。

```java
Person p = new Student("Vince Vu", "Economics");
```

```java
public abstract class Person
{
    private String name;
    
    public Person(String name)
    {
        this.name = name;
    }
    
    public abstract String getDescription();
    
    public String getName()
    {
        return name;
    }
}
```

```java
public class Student extends Person
{
    private String major;
    
    public Student(String name, String major)
    {
        super(name);
        this.major = major;
    }
    
    public String getDercription()
    {
        return "a studentmajoring in " + major;
    }
}
```

## 受保护访问

最好将类中的字段标记为`private`，而方法标记为`public`，私有内容在其它类中是不可见的，即使子类也不能访问超类的字段。

为了解决这个问题，我们可以将这些方法与字段声明设置为受保护`protected`，在java中保护字段只能由同一个包中的类访问。 

1. 对本类可见——private
2. 对外部完全可见——public
3. 对本包和所有子类可见——protected
4. 对本包可见——默认，不需要修饰符

# 枚举类

> 1. 类的对象只有有限个， 确定的，所以会放在类中。 
> 2. 需要一组常量时，用枚举类。  
> 3. 如果枚举类中只有一个对象， 则可以作为单例模式的实现方式 。

Java5之前

```java
public class Test {
    public static void main(String[] args) {
        Season spring = Season.SPRING;

        System.out.println(spring);
    }
}
class Season
{
    private final String seasonName;
    private final String seasonDesc;

    //私有化类的构造器
    private Season(String seasonName, String seasonDesc){
        this.seasonDesc = seasonDesc;
        this.seasonName = seasonName;
    }

    public static final Season SPRING = new Season("春天", "春暖花开");//这些对象是唯一的
    public static final Season SUMMER = new Season("夏天", "酷热难耐");
    public static final Season AUTUMN = new Season("秋天", "秋高气爽");
    public static final Season WINTER = new Season("冬天", "天寒地冻");

    @Override
    public String toString() {
        return "Season{" +
                "seasonName='" + seasonName + '\'' +
                ", seasonDesc='" + seasonDesc + '\'' +
                '}';
    }
}
```

```java
private final String NAME;
public Status(String name)
{
    this.NAME = name;
}
public static final Status FREE = new Status("FREE");
public static final Status VOCATION = new Status("VOCATION");
public static final Status BUSY = new Status("BUSY");
public String getNAME(){
    return NAME;
}
```

这就是一个不用`enum`的枚举类，jdk5.0之前是这样定义的。

1. 普通用法：直接写

   ```java
   public enum Number{ONE, TWO, THREE, FOUR, FIVE, SIX, SEVEN, EIGHT, NINE, TEN};
   ```

   这个类可当作特殊的普通类来用。第一种用法不需要new，有点像String可以直接用，但是要用`enum`里的值来初始化。

   ```java
   Number a = Number.ONE;
   ```

2. 有构造器

   这个类比较特殊，不能从外部来改变，所以有初始化作用的构造器是私有的

   ```java
   public enum Number{
       ONE("one"), TWO("two"), THREE("three"), FOUR("four")...;
       String a;
       private Number(String a)
       {
           this.a = a;
       }
       public String getA() 
       {
           return a;
       }
   }
   ```

   因为其构造器是私有的，所以不能调用，也就不能从外部传参进来，只能在这个类本身去修改ONE("one")。最后，可以通过`getA`得到这个参数。

## 访问及几个方法

可以直接通过对象来访问，这里对象.name()的简写

```java
public class Test {
    public static void main(String[] args) {
        Money a = Money.ONE;
        System.out.println(a);//ONE
        System.out.println(a.name());//ONE
    }
}
```

**values()与ordinal()**方法

value()可以将这个类的内容按顺序转换成数组形式，以上就是ONE TWO THREE FOUR 的形式

ordinal()相当于数组下标。

```java
public class Test {
    public static void main(String[] args) {
        Money[] s = Money.values();
        for (Money a : s)
        {
            System.out.print(a.ordinal() + " ");//0 1 2 3
        }
        System.out.println();
        for (Money a : Money.values())
        {
            System.out.print(a + " " + a.ordinal() + " ");//ONE 0 TWO 1 THREE 2 FOUR 3
        }
        System.out.println();
    }
}
```

## valueOf(String name)

> Returns the enum constant of the specified **enum type** with the specified name

写错就会抛异常。

## 分别实现接口

```java
public enum Season implements Info{
    SPRING("春天"){
        @Override
        public void show(){
            System.out.println("春天在哪里？");
        }
    }, SUMMER("夏天") {
        @Override
        public void show(){
            System.out.println("夏天在哪里？");
        }
    }, AUTUMN("秋天"){
        @Override
        public void show(){
            System.out.println("秋天在哪里？");
        }
    }, WINTER("冬天"){
        @Override
        public void show(){
            System.out.println("冬天在哪里？");
        }
    };

    private String description;

    private Season(String description){
        this.description = description;
    }

    @Override
    public void show() {
        System.out.println("四季快乐");
    }
}
interface Info{
    void show();
}
```

这样展示的时候就会展示自己的方法。

# 代码块

> 代码块是用来初始化的，加载类时的初始化顺序为，子类------->父类到最终类时按静态代码块（只在类加载进执行一次）或静态字段（静态变量）------>非静态字段------>非静态代码块------>构造器执行
>
> 静态大于类，父类静态完了不执行父类其它动作先执行子类静态。

```java
public class Test {
    public static void main(String[] args) {
        //1
        System.out.println("main方法执行");
        Dog dog = new Dog();
    }
}

class Animals {
    //2
    static int age = 5;

    //3
    static {
        System.out.println("父类静态代码块执行" + age);
    }

    //8
    public Animals() {
        System.out.println("父类构造器执行");
    }

    //6
    private String name = "a";

    //7
    {
        System.out.println("父类非静态代码块执行" + name);
    }
}

class Dog extends Animals {
    //4
    static int age = 5;

    //5
    static {
        System.out.println("子类静态代码块执行" + age);
    }

    //11
    public Dog() {
        System.out.println("子类构造器执行");
    }

    //9
    private String name = "a";

    //10
    {
        System.out.println("子类非静态代码块执行" + name);
    }

}
/*
main方法执行
父类静态代码块执行5
子类静态代码块执行5
父类非静态代码块执行a
父类构造器执行
子类非静态代码块执行a
子类构造器执行
*/
```

### 静态代码块：见上图

### 非静态代码块：见上图

# JavaBean

> JavaBean是一种Java语言写成的可重用组件

所谓`javaBean`，是指符合如下标准的Java类。

1. 类是公共的
2. 有一个无参的公共构造器
3. 有属性，且有对应的get， set方法

# 字符串

## 获取有关方法

### 长度

```java
public int length();
```

### 拼接

```java
public String concat(String str);//返回的是新的字符串
```

### 索引

```java
public char charAt (int index);
```

### 查找子串位置

```java
public int indexOf(String str);//从零开始 
```

## 截取有关方法

```java
public String Substring(int index);截取从参数位置一直到字符串末尾，返回新字符串
public String Substring(int begin, int end);截取两段中间的字符串，左闭右开
```

## 转换有关方法

```java
public char[] toCharArray();//变成字符数组
public byte[] getBytes();//字节数组
public  String replace(CharSequence oldString, CharSequence newString);//将所有出现的老字符串，替换成新的字符串，返回替换之后的结果新字符串。
```

## 分割字符串 

```java
public String[] split(String regex);//按参数的规则，将字符串分成为若干部分
```

这个方法其实是一个正则表达式，如果要以英文`.`句点来分割，那其参数`regex`就应该为`\\.`

### toString方法

```java
public String toString()
```

返回该对象的字符串表示。通常，`toString `方法会返回一个“以文本方式表示”此对象的字符串。结果应是一个简明但易于读懂的信息表达式。建议所有子类都重写此方法。 `String，Date，File，包装类等都重写了Object中的toString方法`
Object 类的 `toString` 方法返回一个字符串，该字符串由类名（对象是该类的一个实例）、at 标记符“@”和此对象哈希码的无符号十六进制表示组成。换句话说，该方法返回一个字符串，它的值等于： 

```java
getClass().getName() + '@' + Integer.toHexString(hashCode())
```

返回：
		该对象的字符串表示形式。

当你需要用的`String`类型时，而你只有对象时，这时聪明的编译器会得知你想要的是一个Sting。它就会调用了toString()方法，返回一个字符串，这个方法常常用于输出时。你自己有必要重写这个方法。

```java
public class A {
    public static void main(String[] args) {
        S s = new S();
        System.out.println(s);
    }
}
class S
{
    @Override
    public String toString()
    {
        return "haha";
    }
}
```

# 包装类

针对八种基本数据类型定义相应的引用类型----包装类（封装类）

| 基本数据类型 | 包装类    |
| ------------ | --------- |
| byte         | Byte      |
| short        | Short     |
| int          | Integer   |
| long         | Long      |
| float        | Float     |
| double       | Double    |
| boolean      | Boolean   |
| char         | Character |

前面6种包装类的父类是Number。

## 基本数据类型，包装类，String类三者的转换

1. 基本数据类型到包装类

> 调用包装类的构造器
>
> 自动装箱

```java
Integer int1 = new Interger(num);
Integer int2 = num;
```

2. 包装类到基本数据类型

> 调用包装类的xxxValue()
>
> 自动拆箱

```java
Integer int1 = new Interger(num);
int i1 = int1.intValue();//变成基本可以进行加减乘除运算
int i2 = int1;//自动拆箱
```

3. 上两者-->Sting

> 直接连一个空字符串

```java
String str = num + "";
```

> `String.valueOf()`

```java
String str = String.valueOf(num);
```

4. String--->上面两者

> 包装类parseXxx()方法

```java
int num = Integer.parseInt("1");
```

这之中转到`boolean`类型的字符串可以加别的东西，不会报错。

```java
String str = "true231";
boolean b = Boolean.parseBoolean(Str);
```

# 抽象

在继承关系里，可能同一类东西有相同的方法，但是这些方法实现不同，比如，动物都要吃东西，但是吃东西的方法不同，猫吃鱼，狗吃肉。

会在超类里放一些“哑”方法，它们没有方法体，也就没有具体实现，而让这些具体实现在子类中实现。就是因为超类的目的是为它的所有导出类创建一个通用接口。

建立这个通用接口，不同的子类可以用不同方式表示此接口，通用接口建立起一种基本形式，以此表示所有导出类的共同部分。这个类就是**抽象类**。

## 关键点：

> 有抽象方法的类就是抽象类， 抽象方法要由子类中的方法去具体实现。抽象方法不可以创建对象，也就是new。

抽象类要在普通类的基础上，加上`abstract`，有抽象方法的类必须限定为抽象类，否则编译器会报错。

```java
public abstract class A{
	//....
}
```

总结：

1. 抽象类不可创建对象
2. 抽象类可以有构造方法，因为子类中有默认的`super`，会调用超类的构造器
3. 抽象类不一定包含抽象方法，有抽象方法的一定是抽象类
4. 抽象类的子类必须要覆盖重写超类的抽象方法，要不然它分默认继承这些抽象方法，显然不重写，它也成了抽象类

# 多态

> extends 继承或者implements实现， 是多态性的前提，一个对象，有其父类，与自己本身类的多种形态。

## 原因

> 将一个方法调用同一个方法体关联起来被称作**绑定**，在程序执行前进行绑定，就是前期绑定，它是面向过程语言所默认的绑定方式。而多态是由于在面前对象语言用的是后期绑定（动态绑定，运行时绑定）。
>
> 后期绑定在运行时判断对象的类型，从而运用正确的方法，编译器一直都不知道对象的类型，但是方法调用机制能找到正确的方法体。

**所以只有方法才有后期绑定，也就是只有方法才有多态，而方法中static与`fianl`（private)之外，其它所有的方法都是后期绑定。**

## 向上转型

> 在继承图上，导出类（子类）转型成父类，代码层面就是父类引用，指向子类对象。

向上转型是安全的，但是无法调用导出类的特有内容。

## 向下转型

> 其实是一个【还原】动作。

格式：子类名称 对象名 = （子类名称）父类对象

本来是用父类引用指向子类，对这个父类进行向下转型成这个子类是可以的。别的子类却不行。

```java
Animal animal = new Cat();//Animal是Cat与Dog的父类
Cat cat = (Cat)animal;//这样可以
Dog dog = (Dog)animal;//不可以
```

对象本来创建时是猫，向上转型成动物可以，再向下转型只能成为猫了。

那么问题来了，我有一个父类的引用，你怎么知道它new的哪一个子类。

## `instanceof`

> 判断左边的对象是否是new的右边的子类。
>
> 对象 instanceof 子类 返回一个boolean值。

```java
public class Test {
    public static void main(String[] args) {
        Animal a = new Cat();
        if (a instanceof Cat) {
            System.out.println("是猫");
        }
    }
}
class Animal {
    public void out() {
        System.out.println("父");
    }
}

class Cat extends Animal {
    @Override
    public void out() {
        System.out.println("子");
    }
}

```

## 体现

在代码中多态的体现，就是父类引用可以指向子类的对象，而子类的引用不可以指向父类的对象。

而在调用时，new了谁就先调用谁的方法，没有则向上找。 

```java
public class Test {
    public static void main(String[] args) {
        Fu a = new Zi();
        a.out();//调用了子类方法
    }
}
class Fu
{
    public void out() {
        System.out.println("调用了父类方法");
    }
}
class Zi extends Fu
{
    public void out() {
        System.out.println("调用了子类方法");
    }
}
```

## 多态性编译时行为还是运行时行为？

> 运行时行为

什么时编译时行为？就是你还没有运行就可以知道它的结果的。

```java
import java.util.Random;

public class Test2 {
    public static Animals getRandom(int random) {
        switch (random) {
            case 1:
                System.out.println("1");
                return new Dog();
            case 2:
                System.out.println("2");
                return new Cat();
            default:
                System.out.println("3");
                return new Fish();
        }
    }

    public static void main(String[] args) {
        getRandom(new Random().nextInt(3) + 1).who();
    }
}

class Animals {
    public void who() {
        System.out.println();
    }
}

class Dog extends Animals {
    @Override
    public void who() {
        System.out.println("I am a dog.");
    }
}

class Cat extends Animals {
    @Override
    public void who() {
        System.out.println("I am a cat.");
    }
}

class Fish extends Animals {
    @Override
    public void who() {
        System.out.println("I am a fish.");
    }
}

```

我们不知道程序到底会输出什么。

## 缺陷

> 只有方法的调用是多态的，域（成员变量）不是。

任何域访问操作都将由编译器解析，因此不是多态的。从代码层面上来看，就是左边的引用是谁就访问谁的域，而不去管new，没有就向上找，不会向下找。

```java
public class Test {
    public static void main(String[] args) {
        Fu num = new Zi();
        System.out.println(num.a);//20
    }
}
class Fu
{
    public int a = 20;
}
class Zi extends Fu
{
    public int a = 200;
}
===============================================================
public class Test {
    public static void main(String[] args) {
        Zi num = new Zi();
        System.out.println(num.a);//20
    }
}
class Fu
{
    public int a = 20;
}
class Zi extends Fu {}

```

值得注意的是，平时很少会将域设置成`public`，所以由这种情况产生的错误很少会发生。

而静态方法和静态成员与对象没有关系，只能通过类名调用。所以它们也没有多态性了。

总结：

> 编译看左边，运行看右边。在IDE写时，它即时编译，有红线就是编译错误。

# 接口

> 接口就是多个类的公共规范。其是一种引用数据类型，最重要的内容就是其中的：抽象方法。

注意：

1. 接口没有静态代码块或者构造方法。

2. 一个类的直接父类是唯一的，但一个类可以同时实现多个接口。

   ```java
   public class MyInterfaceImpl implements MyIneterfaceA, MyInterfaceB{
       //覆盖重写所有方法
   }
   ```

3. 如果实现类所实现的多个接口当中， 存在重复的抽象方法，那么只需要覆盖重写一次

4. 如果实现类的没有覆盖重写所有接口当中的所有抽象方法，那么实现类就必须是一个抽象类

5. 如果实现所实现的多个接口当中，存在重复的默认方法，那实现类要对冲突的默认方法覆盖重写

6. 一个类使用的方法，父类与接口中的默认方法产生了冲突，优先使用父类中的方法

## 定义：

```java
public interface 接口名称{
   	//接口内容
}
```

## 接口内容

`Java7`接口中可以包含的内容有：

1. 常量 

   ```java
   [public] [static] [final] 数据类型 变量名称 = 数据值;
   ```

   接口中的成员变量要在普通的成员变量的基础上加上三个关键字， 因为它不可以改变，还是静态的，所以它成了常量，必须初始化，也不可以被类对象进行调用。这三个关键字可以省略。

2. 抽象方法

   ```java
   [public] [abstract] 返回值类型 方法名称（参数列表）;
   ```
   
   

`Java8`可以包含

3. 默认方法

   ```java
   [public] default 返回值类型 方法名称（参数列表）{
   	方法体
   }
   ```

   public可以省略，因为它是有方法体的，你也可以自己覆盖重写，可以通过接口实现类对象直接调用。

4. 静态方法

   ```java
   [public] static 返回值类型 方法名称 （参数列表）{
       方法体
   }
   ```

   同样的静态方法只能用名称调用，而不是对象进行调用。只能用接口名称调用。

`Java9`可以包含

5. 私有方法（）

>原先的几个方法中有重复的内容，我们就需要将这个公共内容写在一个新方法中，我们要保证这个方法，只能为原先的几个方法使用，而不是有这个实现类对象就能用。

* 普通私有：解决默认方法之间代码重复问题

  ```java
  private 返回值类型 方法名称 （参数列表）{
      方法体
  }
  ```

  

* 静态私有：解决静态方法之间代码重复问题

  ```java
  private static 返回值类型 方法名称 （参数列表）{
      方法体
  }
  ```

  

## 接口的使用步骤

1. 接口是进一步的抽象，所有接口也不可以直接使用，而必须要有一个**实现类**来**实现**该接口。

其格式是：

```java
public class 实现类名称 implements 接口名称{
    //...
}
```

2. 与抽象相同，接口的实现类必须覆盖重写接口中所有的抽象方法。

3. 创建实类的对象，使用。

## 类与接口的关系

1. 类与类之间是单继承的。直接父类就一个
2. 类与接口之间是多实现的。一个类可以实现多个接口
3. 接口与接口之间是多继承的

## 代理模式

```java
/*
*静态代理
*/
public class NetWrokTest{
    public static void main(String[] args)
    {
        Server server = new Server();
        ProxyServer proxyServer = new ProxyServer(server);
        
        proxyServer.browse();
    }
}
interface NetWork{
    public void browse();
}
//被代理类
class Server implements NetWork{
    @override
    public void browse(){
        System.out.println("真实的服务器访问网络");
    }
}
//代理类
class ProxyServer implements Network{
    private NetWork work; 
    
    public ProxyServer(Network work){
        this.work = work;
    }
    
    public void check(){
        System.out.println("联网前的检查工作")
    }
    @override
    public void browse(){
        chaeck();
        
        work.browse();//代理类代理调用
    }
}
```

## 工厂模式



# final关键字

## 常用的四种用法

1. 修饰一个类

2. 修饰一个方法

3. 修饰一个局部变量

4. 修饰一个成员变量：

   > 1. 由于成员变量有默认值，所以用了final之后必须手动赋值，不会再给默认值
   > 2. 对于final的成员变量，要么使用直接赋值，要么通过构造方法赋值，二者选其一

构造器中可以赋值。

# java中的四种权限修饰符

|              | public | protected             | （default) | private |
| ------------ | ------ | --------------------- | ---------- | ------- |
| 同一个类     | √      | √                     | √          | √       |
| 同一个包     | √      | √                     | √          | ×       |
| 不同包子类   | √      | √（可以访问初始父类） | ×          | ×       |
| 不同包非子类 | √      | ×                     | ×          | ×       |

# 内部类

> 一个类的内部又有一个类，那这个就叫内部类
>
> 例如：身体与心脏的关系，汽车与发动机的关系

## 成员内部类

### 定义

```java
修饰符 class 外部类名称{
	修饰符 class 内部类名称{
        //...
    }
    //...
}
```

内部类用外部类，随意访问

外部类用内部类，要内部类对象

### 使用

1. 直接：外部类名称.内部类名称 对象名 = new 外部类名称().new 内部类名称();
2. 间接：在外部类的方法中，使用内部类，然后在main只是调用外部类的方法

### 内部类同名变量的访问

> 原先学继承时，学到要访问重名的父类成员， 要加上super。而内部类与外部类也有重名的话就要用：
>
> 外部类名称. this.外部类变量名

## 局部内部类（匿名内部类）

### 定义

> 如果一个类是定义在一个方法内部的，那么这就是一个局部内部类。
>
> “局部”：只有当前所属的方法才能使用它，出了这个方法就不能用了。

```java
修饰符 class 外部类名称{
    修饰符 返回值类型 外部类方法名称（参数列表）{
		class 局部类名称{
        	//...
        }
    }
    //...
}
```

### 局部内部类访问所在方法的局部变量

> 如果局部内部类要访问所在方法的局部变量，那就要保证这个变量不可变，可以是final（java7之前必须)，也可以是从来就没变过。

原因：

1. new 的对象在堆内存中
2. 局部变量是在方法中，在栈内存中
3. 方法运行结束，立刻出栈，局部变量会消失
4. 但局部内部类还在堆中，要等到垃圾回收消失。

只要保证它不变，局部内部类就会将它拷贝下来，如果总是变，那就不合理了。

## 匿名内部类

### 定义

接口名称 对象名 = new 接口名称(); 

直接new接口补上个大括号。

```java
public class Zii{
    public static void main(String[] args) {
        Fu fu = new Fu() {
            @Override
            public void method() {
                System.out.println("匿名内部类方法");
            }
        };
        fu.method();//匿名内部类方法
    }
}
abstract class Fu
{
    public abstract void method();
}
```

本来是要实现这个接口的，但是只用一次太麻烦了，所以就在内部匿名创建一个类，

## 权限修饰符

1. 外部类：public / (default)
2. 成员内部类：都可以
3. 局部内部类：什么都不写

# `equals`方法

## ==符号

> ==是存储地址的比较

* 因为有常量池，所以在比较八大类型与String类型时，它们的地址值都是一样的。也就与比较值没有什么不同了

* 引用类型比较的是地址值，常常不同
* 基本包装类在-128~127用一个数组来存，所以地址也是一样的

编译器可以帮你重写。

`equals`方法默认比较值是不是相等

1. 是变量：就比较它们的值是不是相等
2. 是对象：就比较它们的地址是不是相等

**Object类中的`equals`方法也是实现了第二点，也就是比较两者的引用是否相等。这无可厚非，但是平时去比较两个对象是否相等，多数是比较其中的内容是不是相等，所以像String，Date，File，包装类等都重写了Object中的equals方法，去比较对象的“实体内容”，我们也可以去重写equals方法。**

## 重写equals方法

```java
@Override
public boolean equals(Object obj)
{
    if (obj == this){
        return ture;
    }
    if (obj == null){
        return false;
    }
    if (obj instanceof Person){
        Person p = (Person)obj;
        return this.name.equals(p.name) && this.age == p.age;
    }
    return false;
}
```

当然编译器也会帮你去直接生成。

### Objects的equals方法

```java
public static boolean equals(Object a, Object b){
    return (a == b)||(a != null && a.equal(b));
}
```

这个方法是将两个要比较的方法都作为参数的，这样有什么好处呢？看源码可知，这个检测了null空指针， 所以就**不会有空指针异常**。

equals 方法在非空对象引用上实现相等关系： 

* 自反性：对于任何非空引用值 x，x.equals(x) 都应返回 true。 

* 对称性：对于任何非空引用值 x 和 y，当且仅当 y.equals(x) 返回 true 时，x.equals(y) 才应返回 true。 

* 传递性：对于任何非空引用值 x、y 和 z，如果 x.equals(y) 返回 true，并且 y.equals(z) 返回 true，那么 x.equals(z) 应返回 true。 
* 一致性：对于任何非空引用值 x 和 y，多次调用 x.equals(y) 始终返回 true 或始终返回 false，前提是对象上 equals 比较中所用的信息没有被修改。 
* 对于任何非空引用值 x，x.equals(null) 都应返回 false。

# 异常处理

**开发异常处理的初衷是为了方便程序员处理错误，只有在你知道如何处理的情况下才捕获异常。**

> 异常处理是java中唯一正式的错误报告机制

> `Error`：java虚拟机无法解决的严重问题。JVM内部错误。一般不编写针对性代码进行处理。堆栈溢出

> Exception：其它因编程错误或偶然的外在因素导致的一般性问题，可以用针对性的代码处理。

我的理解为代码没有产生预期的效果，而需要返回一种安全状态，而不是放之任之，有错了，要提醒我，怎么做到这一点，就是未雨绸缪，提前找到可能出异常的地方。

`throw`就是抛出的意思，如果代码出现了错误就产生（自动与手动） 一个异常对象，程序员要对这个异常做出处理，分析它是什么异常，然后给出提示，有的异常可以解决，有的则不可以。有异常就要处理，`try-catch-finally`就是做这件事的，另外也可以当时不处理，而是向上抛，就是`throws`，最后再在最上层的代码块中去处理问题。

**异常处理的一个重要特点：把“描述在正常执程中做什么事的代码”和“出了问题怎么办的代码相分离。”，如果发生问题，它们将不允许程序沿着其正常的路径继续走下去。**

## 异常的体系结构

* `java.lang.Throwable`
  
  *  `java.lang.Error `不作处理，报错时的后缀都是Error
  *  `java.lang.Exception`; 可以进行异常处理
    * 编译时异常(checked)
      * `IOException`I/O异常
        * `FileNotFoundException`文件未发现异常
      * `ClassNotFoundException`类未发现异常
    * 运行时异常(unchecked)
      * `NullPointerException`空指针异常
      * `ArrayIndexOutOfBoundsException`数组指针越界异常
      * `ClassCastException`类计算异常
      * `NumberFormatException`数字格式异常
      * `InputMismatchException`输入不匹配异常
      * `ArithmeticException`数字计算异常
  
  ![image-20200904094014374](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20200904094014374.png) 

**如果出现RuntimeException异常，那是一定是你的问题。**

> 不需要在异常说明方法中声明方法将抛出RuntimeException类型的异常，它们也被称为**不受检查异常**。这种异常属于错误，会被自动捕获。

```java
package io.github.little.chapter12;


/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-20 11:20
 */
public class NeverCaught {
    private static void f(){
        throw new RuntimeException("throw from f()");
    }
    private static void g(){
        f();
    }
    public static void main(String[] args) {
        g();
    }
}
Exception in thread "main" java.lang.RuntimeException: throw from f()
	at io.github.little.chapter12.NeverCaught.f(NeverCaught.java:11)
	at io.github.little.chapter12.NeverCaught.g(NeverCaught.java:14)
	at io.github.little.chapter12.NeverCaught.main(NeverCaught.java:17)
```

**如果RuntimeException没有被捕获而直达main()，那么在程序退出前将调用异常的printStackTrace()方法**

只能在代码中忽略RuntimeException，因为它代表的是编程错误。

## 自定义异常类

所有的标准异常类都有两个构造器：默认构造器和以字符串为参数，以便能把相关信息放入异常对象的构造器。

> 使用try-catch-finally处理编译时异常， 使得程序在编译时就不再报错，但是运行时仍可能出错。相当于使用try-catch-finally将一个编译时可能出现的异常，延迟到运行出现。
>
> 开发中由于运行时异常比较常见，所以通常不针对运行时异常编写try-catch-finally了。针对编译时异常，一定要考虑异常的处理。

```java
import java.util.Date;
import java.util.Scanner;

public class Out {
    /*
    java.lang.ArithmeticException: / by zero
    运算异常
     */
    @Test
    public void test6()
    {
        int a = 10;
        int b = 0;
        System.out.println(a / b);
    }
    /*
    java.util.InputMismatchException
    输入不匹配
     */
    @Test
    public void test5()
    {
        Scanner scanner = new Scanner(System.in);
        int score = scanner.nextInt();//如是输入入的不是一个Int类型
        System.out.println(score);
    }
    /*
    java.lang.NumberFormatException: For input string: "abc"
    数的格式异常，abc不能转成Int型
	at java.base/java.lang.NumberFormatException.forInputString
     */
    @Test
    public void test4()
    {
        String string = "123";
        string = "abc";
        int num = Integer.parseInt(string);
    }
    /*
    java.lang.ClassCastException: class java.util.Date cannot be cast to class java.lang.String (java.util.Date and java.lang.String are in module java.base of loader 'bootstrap')
    类型计算异常
	at io.github.big.chapter.Out.test3(Out.java:25)
     */
    @Test
    public void test3()
    {
        Object object = new Date();
        String str = (String)object;
    }
    /*
    java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3
    数组越界
	at io.github.big.chapter.Out.test2(Out.java:13)
     */
    @Test
    public void test2()
    {
        int[] array = new int[3];
        System.out.println(array[3]);
    }
    /*
    java.lang.NullPointerException
	at io.github.big.chapter.Out.test1(Out.java:10)
	空指针异常
     */
    @Test
    public void test1()
    {
        int[] array = null;
        System.out.println(array[4]);
    }
}
```

自定义异常类时，只需要继承Exception异常类型，写好两个构造器即可。

## 异常处理机制一（编译时异常处理）

### 监控区域

try中包含了可能出现异常的代码，后面跟着处理这些异常的代码，这就是监控区域。

### 异常处理程序

异常处理程序紧跟在try块之后，以关键字catch表示。

### 终止与恢复

1. 终止模型：这种模型将假设错误非常关键，以至于程序无法返回到异常发生的地方继续执行，一旦异常被抛出，就表明错误已无法挽回，也不能回来继续执行。
2. 恢复模型：就是多次循达到正确的方法，这种方法用去输入错误到是可行，但其增加了代码编写和维护的难度。

```java
try{
    //可能出现异常的代码
}catch(异常类型1 变量名1){
    //处理异常的方式1
}catch(异常类型2 变量名2){
    //处理异常的方式2
}catch(异常类型3 变量名3){
    //处理异常的方式3
}catch(异常类型4 变量名4){
    //处理异常的方式4
}
......
finally{
    //一定会执行的代码
}
public void test1()
{
    String str = "123";
    str = "abc";
    try {
        int num = Integer.parseInt(str);
    }catch(NumberFormatException e){
        System.out.println("NumFormatException");
    }
}
```

1. finally 是可选的
2. 用try将可能出现异常的代码包起来，程序执行try中的所有代码直到其抛出异常为止，此时将跳过try中的剩余代码，转去执行与该异常匹配的catch子句中的代码。
   1. 如果catch子句没有抛出异常，那么执行完finally后就继续执行其后的语句。
   2. 如果catch子句抛出异常，那么执行完finally后，再回到catch这个方法的调用者。
3. 如果代码抛出了异常却没有任何catch子句去捕获这个异常，程序将执行所有的try语句直到抛出异常，跳过剩余代码，然后执行finally语句，并将异常抛回给这个方法的调用者。
4. catch中的异常类型中也有子父类的关系，要把子类写在前面，要不然就会报错
5. 常用的异常处理方式有：
   1. `String getMessage() `获取异常信息
   2. `printStackTrace()` 输出追踪堆栈信息

> 将一个编译时可能会出现的异常处理掉，延迟直到运行时才会出现。

## 异常处理机制二

异常说明：告知客户端程序员某个方法可能会抛出的异常类型，客户端程序员就可以进行处理。

1. throws + 异常类型写在方法声明处，指明此方法执行时，可能会抛出的异常类型。

   一旦当方法体执行时，出现异常，仍会在异常代码处生成一个异常类的对象，此对象满足throws后异常类型时，就会抛出。异常后的代码就不再执行。最后还是要用`try-catch-fanilly`来处理

```java
package io.github.big.chapter7;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class ExceptionTest2 {
    public static void main(String[] args) {
        try {
            method2();
        }catch (IOException e)
        {
            e.printStackTrace();
        }
    }

    public static void method2() throws IOException, FileNotFoundException{
        method1();
    }
    public static void  method1() throws FileNotFoundException, IOException {
        File file = new File("hello.txt");
        FileInputStream fis = new FileInputStream(file);

        int data = fis.read();
        while (data != -1){
            System.out.println((char)data);
            data = fis.read();
        }
        fis.close();
    }
}
```

## 方法重写的规则

> 子类重写的方法抛出的异常类型不大于父类被重写的方法抛出的异常类型 

## 分析堆栈的轨迹元素

```java
package io.github.big.chapter7;

import java.util.Scanner;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-17 8:48
 */
public class StackTraceTest {
    public static int factorial(int n)
    {
        System.out.println("factorial(" + n + "):");
        StackWalker walker = StackWalker.getInstance();//获得堆栈运行实例
        walker.forEach(System.out::println);//在每个栈帧上完成指定的动作，从最近的调用方法开始
        int r;
        if (n <= 1)
            r = 1;
        else
            r = n * factorial(n -1);
        System.out.println("return" + r);
        return r;
    }

    public static void main(String[] args) {
        try
        {
            System.out.println("Enter n:");
            var in = new Scanner(System.in);
            int n = in.nextInt();
            factorial(n);
        }
        finally{

        }
    }
}

Enter n:
3
factorial(3):
io.github.big.chapter7.StackTraceTest.factorial(StackTraceTest.java:15)
io.github.big.chapter7.StackTraceTest.main(StackTraceTest.java:31)
factorial(2):
io.github.big.chapter7.StackTraceTest.factorial(StackTraceTest.java:15)
io.github.big.chapter7.StackTraceTest.factorial(StackTraceTest.java:20)
io.github.big.chapter7.StackTraceTest.main(StackTraceTest.java:31)
factorial(1):
io.github.big.chapter7.StackTraceTest.factorial(StackTraceTest.java:15)
io.github.big.chapter7.StackTraceTest.factorial(StackTraceTest.java:20)
io.github.big.chapter7.StackTraceTest.factorial(StackTraceTest.java:20)
io.github.big.chapter7.StackTraceTest.main(StackTraceTest.java:31)
return1
return2
return6
```

从输出语句可以看出一个完整的压栈与弹出的过程。

## 重新抛出异常

> 如果希望把刚捕获的异常抛出，尤其是使用Exception捕获所有异常的时候。catch得到了当前对象的引用，可以将它重新抛出。

```java
catch(Exception e){
    throw e;
}
```

重抛会把异常抛给上一级环境中的异常处理程序，后面的catch子句会忽略。

如果只是把**当前的**(没有new新对象)异常对象重新抛出，那么printfStackTrace()方法显示的就是原来异常点的堆栈信息，而不是这个重新抛出点的异常信息。

**如果想要更新这个异常信息，也就是忘掉原来的异常信息，只从这个抛出点开始，可以调用fillInStackTrace()方法。**它会把当前调用栈的信息填入原来那个异常对象。

```java
package io.github.little.chapter12;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-20 10:25
 */
public class ExceptionLink {
    public static void main(String[] args) {
        try{
            try {
                Test.f();
            } catch (MyException e) {
                System.out.println(e.getMessage());
                e.printStackTrace(System.out);
                e.fillInStackTrace();//有这行就没有下面的f()调用栈
                throw e;
            }
        }catch (MyException e) {
            System.out.println(e.getMessage());
            e.printStackTrace(System.out);
        }
    }
}
class Test
{
    public static void f() throws MyException {
        throw  new MyException("throw from MyException");
    }
}
throw from MyException
io.github.little.chapter12.MyException: throw from MyException
	at io.github.little.chapter12.Test.f(ExceptionLink.java:28)
	at io.github.little.chapter12.ExceptionLink.main(ExceptionLink.java:12)
throw from MyException
io.github.little.chapter12.MyException: throw from MyException
   	//at io.github.little.chapter12.Test.f(ExceptionLink.java:28)有上面那行就没有这一行
	at io.github.little.chapter12.ExceptionLink.main(ExceptionLink.java:16)
```

同时，有时可能会在捕获异常后抛出另一个异常(new了就是新的)，得到的效果类似于使用fillStackTrace()，有关原来异常发生点的信息会丢失，剩下的就是与新抛出点有关的信息。

## 异常链

> 如果在catch子句中抛出一个异常，那么原来的异常就会丢失，通常，希望改变异常的类型时会这样做。如果想要知道原先的异常的类型，可以将原来的异常设置成现在异常的原因，就好像串起来了一样。叫做异常链。

```java
package io.github.little.chapter12;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-20 10:25
 */
public class ExceptionLink {
    public static void main(String[] args) {
        try{
            try {
                Test.f();
            } catch (MyException e) {
                System.out.println(e.getMessage());
                e.printStackTrace(System.out);
                throw new Exception("throw from Exception");
            }
        }catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace(System.out);
        }
    }
}
class Test
{
    public static void f() throws MyException {
        throw  new MyException("throw from MyException");
    }
}
throw from MyException
io.github.little.chapter12.MyException: throw from MyException
	at io.github.little.chapter12.Test.f(ExceptionLink.java:27)
	at io.github.little.chapter12.ExceptionLink.main(ExceptionLink.java:12)
throw from Exception
java.lang.Exception: throw from Exception
	at io.github.little.chapter12.ExceptionLink.main(ExceptionLink.java:16)
```

可以看出第二次抛出已经丢失了前面的f()（MyException）异常。

```java
package io.github.little.chapter12;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-20 10:25
 */
public class ExceptionLink {
    public static void main(String[] args) {
        try{
            try {
                Test.f();
            } catch (MyException e) {
                System.out.println(e.getMessage());
                e.printStackTrace(System.out);
                var a = new Exception("throw from Exception");
                a.initCause(e);//将e设置为a的原因
                throw a;//抛出a
            }
        }catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace(System.out);
        }
    }
}
class Test
{
    public static void f() throws MyException {
        throw  new MyException("throw from MyException");
    }
}
throw from MyException
io.github.little.chapter12.MyException: throw from MyException
	at io.github.little.chapter12.Test.f(ExceptionLink.java:29)
	at io.github.little.chapter12.ExceptionLink.main(ExceptionLink.java:12)
throw from Exception
java.lang.Exception: throw from Exception
	at io.github.little.chapter12.ExceptionLink.main(ExceptionLink.java:16)
Caused by: io.github.little.chapter12.MyException: throw from MyException//这里告知了原因
	at io.github.little.chapter12.Test.f(ExceptionLink.java:29)
	at io.github.little.chapter12.ExceptionLink.main(ExceptionLink.java:12)
```

总结：如果是同一个异常，也就是没有new的，那么它的堆栈抛迹会保留，不想保留可以用fillStrackTrace方法。否则，再次抛出，前面的异常会丢失，如果不想丢失，那可以用异常链，用initCause把原先的异常设置为现在异常的原因。

```java
ThrowableC initCause(Throwable cause);//为这个对象设置原因，如果这个对象已经有原因，则抛出一个异常。
public Throwable fillInStackTrace();//在异常堆栈跟踪中填充。此方法在 Throwable 对象信息中记录有关当前线程堆栈帧的当前状态。当前try的忘掉了。
throw (Exception)e.fillInStackTrace();
```

## 异常丢失

> 如果在嵌套的使用try时，没有catch异常处理程序，而是直接用finally里再调用可能出现异常的子句，那么原先的异常就会丢失。

```java
package io.github.little.chapter12;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-20 16:23
 */
public class LostMessage {
    private void f() throws VeryImportantException {
        throw new VeryImportantException();
    }

    private void dispose() throws HoHumException {
        throw new HoHumException();
    }

    private void three() throws MyException {
        throw new MyException();
    }

    public static void main(String[] args) {
        try {
            LostMessage lm = new LostMessage();
            try {
                lm.f();//throw from VeryImportantException
            } finally {
                lm.dispose();//throw form HoHumException
            }
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
class VeryImportantException extends Exception {
    @Override
    public String toString() {
        return "VeryImportantException{}";
    }
}

class HoHumException extends Exception {
    @Override
    public String toString() {
        return "HoHumException{}";
    }
}
HoHumException{}
```

VeryImportantException异常不见了，它被finally子句中的异常所取代。

# 多线程

 ## 程序、进程、线程

> programmer：为完成特定任务、用某种语言编写的一组指令的集合。即指一段静态的代码，静态对象。
>
> process：是程序一次执行的过程，或是正在运行的一个程序，是一个动态的过程：有它自身的产生、存在和消亡的过程。——生命周期。
>
> thread：进程可进一步细化为线程，是**一个程序内部的一条执行路径**。
>
> * 若一个进程同一时间并行执行多个线程，就是支持多线程的
> * 线程作为调度和执行的单位，每个线程拥有独立的运行栈和程序计数器（pc），线程切换的开销小
> * 一个进程中的多个线程共享相同的内存单元/ 内存地址空间（堆和方法区） ->它们从同一堆中分配对象，可以访问机同的变量和对象。这就例得线程间通信更简便、高效。但多个线程操作共享的系统资源可能就会带来安全的隐患。
> * 单核与多核：
>   * 单核：假的多线程，一个时间单元内，只能执行一个线程。多车道，但是只有一个收费员。java至少有main()，gc()垃圾回收，异常处理三个线程。
> * 并行与并发：
>   * 并行：多个cpu同时执行多个任务
>   * 并发：一个cpu”同时“执行多个任务，双11秒杀

 ## 线程创建的四种方式

```java
static Thread currentThread() 返回对当前正在执行的线程对象的引用。
void start() 使该线程开始执行；Java 虚拟机调用该线程的 run 方法。 
void run() 如果该线程是使用独立的 Runnable 运行对象构造的，则调用该 Runnable 对象的 run 方法；否则，该方法不执行任何操作并返回。 
```

1.  方式一：继承于Thread类
   1. 创建一个继承于Tread类的子类
   2. 重写Thread类的run()--> 将此线程执行的操作声明在run()中
    3. 通过此对象调用start()

```java
public class ThreadTest {
    public static void main(String[] args) {
        //这个是主线程造的
        MyTread myTread = new MyTread();
        Thread.currentThread().setName("主线程");
        //myTread开始独立的运行
        //1.启动当前线程2.调用当前线程的run()方法
        myTread.setName("线程1");
        myTread.start();
        //main线程执行的
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ": main");
            }
            if (i == 20) {
                try {
                    myTread.join();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

class MyTread extends Thread {
    @Override
    public void run() {
        for (int i = 0, j = 0; i < 100; i++) {
            if (i % 2 == 0) {
                try {
                    sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.print(getName() + ": ");
                System.out.printf("%3d", i);
                j++;
                if (j % 5 == 0 && i != 0)
                    System.out.println();
            }
        }
    }
}
```

2. 方式二：实现Runnable接口

   1. 创建一个实现了Runnable的接口的类
    2. 实现run()
    3. 创建对象
    4. 将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
     5. 通过Thread类的对象调用start()

   ```java
   public class ThreadTest1 {
       public static void main(String[] args) {
           MThread mThread = new MThread();
           Thread thread1 = new Thread(mThread);
           thread1.start();
           Thread thread2 = new Thread(mThread);
           thread2.start();
       }
   }
   class MThread implements Runnable{
       @Override
       public void run() {
           for (int i = 0; i < 100; i++) {
               System.out.println(Thread.currentThread().getName() + ":" + i);
           }
       }
   }
   //在Thread类中，有这样两段源码
       public Thread(Runnable target) {
           this(null, target, "Thread-" + nextThreadNum(), 0);
       }
       @Override
       public void run() {
           if (target != null) {
               target.run();
           }
       }
   //如果构造时，有添加Runnable对象，那就调用这个对象的run()方法
   ```

   优先选择实现方法：

   1. 单继承的局限性
   2. 数据共享的局限性（如果是实现，那么只创建一个对象，这个对象作为参数传递进去，这一个对象是共享的）

```java
class Thread implements Runnable 
```

Thread是实现Runnable的。

3. 方式3：实现Callable接口

```java
package in.github.zhengjj.NewThread;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-22 10:34
 * 实现Callable接口
 */
public class New {
    public static void main(String[] args) {
        //3.创建接口实现类对象
        MyThread myThread = new MyThread();
        //4.接口实现类对象作为参数，创建FutureTask对象
        FutureTask futureTask = new FutureTask(myThread);
		//5.以FutureTask对象作为参数，新建Thread对象
        new Thread(futureTask).start();

        try {
            //get()返回值即为FutureTask构造器参数Callable实现类重写的call()的返回值
            Object sum = futureTask.get();
            System.out.println(sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
//1.实现接口
class MyThread implements Callable
{
    //2. 重写方法
    @Override
    public Object call() throws Exception {
        return null;
    }
}
```

实现Callable接口比实现Runnable接口方式强大

* call()有返回值
* call()可以抛出异常，被外面的操作捕获，获取异常的信息
* Callable支持泛型

4. 方式4：线程池

```java
package in.github.zhengjj.NewThread;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-22 10:34
 * 使用线程池
 */
public class New {
    public static void main(String[] args) {
        //1.提供指定线程数量的线程池
        ExecutorService executorService = Executors.newFixedThreadPool(10);
        //2. 执行指定的线程操作，提供实现Callable/Runnable接口的类
        executorService.execute(new MyThread());
        executorService.execute(new MyThread1());
//        executorService.submit();
        executorService.shutdown();
    }
}

class MyThread implements Runnable
{
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 1) {
                System.out.println(Thread.currentThread().getName() + "："+ i);
            }
        }
    }
}

class MyThread1 implements Runnable
{
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + "："+ i);
            }
        }
    }
}
```

使用线程池的好处：

1. 提高响应速度(减少了创建新线程的时间)

2. 降低资源消耗(重复利用线程池中线程，不需要每次都创建)

3. 便于线程管理

   ​		corePoolSize：核心池的大小

   ​		maximumPoolSize：最大线程数

   ​		keepAliveTime：线程没有任务时最多保持多长时间后会终止

## 线程的生命周期

![image-20200914111942783](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20200914111942783.png)

## 线程的同步

> 共享的数据被多个线程同时访问，导致了重复。java中通过同步机制来解决线程安全问题。

共享数据：也就是共有的数据。

新建线程时，要么都使用实现方法，要么都实现继承方法。如果这两个线程都创建方法都不同，那它们的run()方法也会不同，自然同步也没有什么意义了。

也就是说，同样的run()方法操作相同的数据时的被它们创建的多个线程同时访问出现的问题，这就是线程安全问题。

1. 同步代码块

   ```java
   synchronized(同步监视器){
       //需要被同步的代码
   }
   ```

   操作共享数据的代码，即为需要被同步的代码。

   同步监视器——锁，任何一个类的对象都可以充当锁。多个线程必须要共用同一把锁。 

   实现Runnable接口创建多线程的方式中，可以考虑使用this充当同步监视器。因为这个this是实现Runnable接口类。创建其一个对象作为形参放到Thread构造器中。

   而继承Thread方法，可以使用类名.class作为sychronized的同步监视器。

   ```java
   class Window extends Thread
   {
       private static int ticket = 100;
       private static Object object = new Object();
       private int counts = 0;
   
       public int getCounts() {
           return counts;
       }
   
       @Override
       public void run() {
           while(true)
           {
               synchronized (object)
   //            synchronized(Window.class)
               {
                   if (ticket > 0) {
                       try {
                           Thread.sleep(100);
                       } catch (InterruptedException e) {
                           e.printStackTrace();
                       }
                       if ("窗口1".equals(getName()) || "窗口2".equals(getName()) || "窗口3".equals(getName()))
                       {
                           counts++;
                       }
                       System.out.println(getName() + ":卖票，票号为："
                               + ticket + " " + counts);
                       ticket--;
                   } else
                       break;
               }
           }
       }
   }
   ```

   ```java
   class Windows implements Runnable {
       private int ticket = 100;
       Object object = new Object();
   
       @Override
       public void run() {
           while (true) {
   //            synchronized (object)
               synchronized (this)
               {
                   if (ticket > 0) {
                       try {
                           Thread.sleep(100);
                       } catch (InterruptedException e) {
                           e.printStackTrace();
                       }
                       System.out.println(Thread.currentThread().getName()
                               + "：卖票，票号为：" + ticket);
                       ticket--;
                   } else
                       break;
               }
           }
       }
   }
   ```

   

2. 同步方法

   操作共享数据的代码完整的声明在一个方法中，不妨将此方法声明为同步的。

   继承方式方法用静态。当前类.class是锁(static没有this)

   实现方式方法不用变。this是锁
   
   **以上两点都是为了保证是同一把锁**

> 同步方法没有显式的说明锁是谁。

3. Lock锁(JDK5.0后新增)

```java
package in.github.zhengjj.danli;

import java.util.concurrent.locks.ReentrantLock;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-22 10:18
 */
public class Testd {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        Thread thread = new Thread(myThread);
        Thread thread1 = new Thread(myThread);
        Thread thread2 = new Thread(myThread);
        thread.setName("线程0：");
        thread1.setName("线程1：");
        thread2.setName("线程2：");

        thread.start();
        thread1.start();
        thread2.start();
    }
}

class MyThread implements Runnable {
    private int ticket = 100;
    private ReentrantLock lock = new ReentrantLock(true);

    @Override
    public void run() {
        while (true) {
            try {
                lock.lock();

                if (ticket > 0) {
                    System.out.println(Thread.currentThread().getName() + ticket);
                    ticket--;
                } else
                    break;
            }finally {
                lock.unlock();
            }
        }
    }
}
```

与synchronized相比，它要手动实现启动与释放监视器。

优先使用顺序->lock—>同步代码块->同步方法

## 死锁问题

> 不同的线程分别占用对方需要的同步资源不放弃，能在等待对方放弃自己需要的同步资源，就形成了线程的死锁。
>
> 所有线程都处于阻塞状态。

```java
package in.github.zhengjj.danli;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-22 10:18
 */
public class Testd {
    public static void main(String[] args) {
        StringBuffer s1 = new StringBuffer();
        StringBuffer s2 = new StringBuffer();

        new Thread() {
            @Override
            public void run() {
                synchronized (s1) {
                    s1.append("a");
                    s2.append("1");
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (s2) {
                        s1.append("b");
                        s2.append("2");

                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }.start();
        new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (s2) {
                    s1.append("c");
                    s2.append("3");
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (s1) {
                        s1.append("d");
                        s2.append("4");

                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }).start();
    }
}
```

## 线程之间的通信

```java
wait() 在其他线程调用此对象的 notify() 方法或 notifyAll() 方法前，导致当前线程等待。
void notify()唤醒在此对象监视器上等待的单个线程。 
```

```java
package in.github.zhengjj.danli;

import javax.swing.*;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-22 10:18
 */
public class Testd {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        Thread thread = new Thread(myThread);
        Thread thread1 = new Thread(myThread);

        thread.start();
        thread1.start();
    }
}
class MyThread implements Runnable
{
    private int number = 100;
    @Override
    public void run() {
        while (true)
        {
            synchronized (this) {
                notify();
                if (number > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + "：" + number--);
                    try { 
                        wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            else
                break;
            }
        }
    }
}
//当一个任务在方法里遇到了对wait()的调用时，线程的执行被挂起，对象上的锁被释放。因为wait()将释放锁，这就意味着另一个任务可以获得这个锁，因此在该对象(现在是未锁定的)中的其他synchronized方法可以在wait()期间被调用。
```

sleep()和wait()的异同

1. 相同：一旦执行，都可以使当前线程进入阻塞

2. 不同点：1. 声明位置不同：Thread中sleep(),Object中声明wait()

   ​				2.wait()要在同步代码块和同步方法中

   ​				3.在同步代码块和同步方法中使用，sleep()不释放同步监视器，wait()会释放。

```java
package in.github.zhengjj.danli;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-09-22 10:18
 */
public class Testd {
    public static void main(String[] args) {
        Clerk clerk = new Clerk(0);
        Producer producer = new Producer(clerk);
        Customer customer = new Customer(clerk);

        Thread thread = new Thread(producer);
        Thread thread1 = new Thread(customer);
        Thread thread2 = new Thread(customer);

        thread.setName("生产者：");
        thread1.setName("消费者1：");
        thread2.setName("消费者2：");

        thread.start();
        thread1.start();
        thread2.start();
    }
}

class Clerk {
    private int clerk = 0;//共享的数据

    public Clerk(int clerk) {
        this.clerk = clerk;
    }
	//同一个锁就是this——Clerk对象，只造了一个
    public synchronized void productClerk() {
        if (clerk < 20) {
            clerk++;
            notify();//生产了一个就可以消费
            System.out.println(Thread.currentThread().getName() + "开始生产第" + clerk + "个产品");
        } else {
            try {
                wait();//如果生产停，只可能是满了
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public synchronized void makeClerk() {
        if (clerk > 0) {
            System.out.println(Thread.currentThread().getName() + "开始消费第" + clerk + "个产品");
            clerk--;
            notify();//消费了一个就可以生产
        } else {
            try {
               	System.out.println(Thread.currentThread().getName() + "拿了");//锁有可能被任何一个线程拿到
                wait();//消费停，只可能是没了
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

}

class Producer implements Runnable {
    Clerk clerk;

    public Producer(Clerk clerk) {
        this.clerk = clerk;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "开始生产产品...");
        while (true) {
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            clerk.productClerk();
        }
    }
}

class Customer implements Runnable {
    Clerk clerk;

    public Customer(Clerk clerk) {
        this.clerk = clerk;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "开始消费产品...");
        while (true) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            clerk.makeClerk();
        }
    }
}
```

> 明确用法，synchronized是不让多个线程同时操作共享数据，wait是当前线程等待，并释放锁，这个锁有可能被其他没有等待的线程拿到，notify唤醒等待的线程。

![image-20200923083217318](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20200923083217318.png)

# Java常用类

## String

1. 声明为final，不可被继承
2. 实现了Serializable接口：表示字符串是支持序列化的，也实现了Comparable接口，所以它可以比较大小。
3. 内部定义了final char[] value用于存储字符串数据
4. 通过字面量方式，与new不同，其值声明在字符串常量池中
5. 字符串常量池中是不会存储相同内容的字符串的

> String——> char[]：调用String的toCharArray()
>
> char[]——>String：调用String的构造器



> String ——> byte[]：调用String的getBytes() 
>
> byte[]——>String：调用String构造器

常量和常量的拼接在常量池。 

## String、StringBuffer、StringBuilder的异同？

String：不可变字符序列：底层用char[]存储

StringBuffer：可变的字符序列：线程安全的，效率低：底层用char[]存储

StringBuilder：可变的字符序列：jdk5.0新增，线程不安全，效率高：底层用char[]存储

增：append()

删：delete(int ,int )

改：setChar(int , char)/replace(int , int , String)

查：charAt(int)

插：insert(int , xxx)

长度：length()

遍历：for() +charAt / toString

## StringBuffer

> StringBuffer sb1 = new StringBuffer();//char[] value = new char[16];
>
> StringBuffer sb1 = new StringBuffer("abc");//char[] value = new char["abc".length() + 16];

底层数组有16个，如果多了，那就要扩容。默认扩容为原来容量的2倍 + 2，同时将原有数组中的元素复制到新的数组中。

### 面试题

1. 反转确定位置字符串

```java
public class Test {
    public static void main(String[] args) {
        String string = "abcdefg";
        System.out.println(reverse1(string, 2, 5));
        System.out.println(reverse2(string, 2, 5));
        System.out.println(reverse3(string, 2, 5));
    }
    public static String reverse1(String string, int start, int last)
    {
        if (string != null)
        {
            char[] array = string.toCharArray();
            for (int i = start, j = last; i < j;i++, j--)
            {
                char temp = array[i];
                array[i] = array[j];
                array[j] = temp;
            }
            return new String(array);
        }
        return null;
    }
    public static String reverse2(String string, int start, int last)
    {
        String str = string.substring(0, start);
        for (int i = last; i >= start; i--)
        {
            str += string.charAt(i);
        }
        str += string.substring(last + 1);
        return str;
    }
    public static String reverse3(String string, int start, int last)
    {
        StringBuffer stringBuffer = new StringBuffer(string.length());
        stringBuffer.append(string.substring(0, start));
        for (int i = last; i >= start; i--)
        {
            stringBuffer.append(string.charAt(i));
        }
        stringBuffer.append(string.substring(last + 1));
        return new String(stringBuffer);
    }

}

```

2. 找字符串中的子串

```java
public class Test {
    public static void main(String[] args) {
        String string = "abcdefgab";
        System.out.println(stringNumber("ab", string));
        System.out.println(stringNumber2("ab", string));
    }
    public static int stringNumber(String find, String string)
    {
        int num = 0;
        int index = 0;
        while((index = string.indexOf(find) )!= -1)
        {
            num++;
            string = string.substring(index + find.length());
        }
        return num;

    }
    public static int stringNumber2(String find, String string)
    {
        int num = 0;
        int l = find.length();
        int index = 0;
        while ((index = string.indexOf(find, index) ) != -1)
        {
            num++;
            index += l;
        }
        return num;
    }
}
```

3. 获取两个字符串中最大相同子串

```java
public class Test {
    public static void main(String[] args) {
        String string = "abcdefgab";
        String string2 = "abcdefsaadf";
        System.out.println(getMaxSameString(string, string2));
    }

    public static String getMaxSameString(String str1, String str2) {
        String stringMin = str1.length() <= str2.length() ? str1 : str2;
        String stringMax = str1.length() > str2.length() ? str1 : str2;
        int length = stringMin.length();
        for (int i = 0; i < length; i++) {
            for (int x = 0, y = length - i; y <= length; y++, x++) {
                String string = stringMin.substring(x, y);
                if (stringMax.contains(string)) {
                    return string;
                }
            }
        }
        return null;
    }
}
```

思路：从最小串中，每次减一个找大串中是否有相同的。所以每一次减一要用一个循环。一个字串少一个字符有2种可能，少两个字符有3种可能，这里用位移的方法，从右边少n个开始整体向右移字符串。

![image-20200925213421940](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20200925213421940.png)

此题`            for (int x = 0, y = length - i; y <= length; y++, x++) `就是实现了上图的效果。

如果有多个最大字符串相同，那么可以用ArrayList方法来。这里提供一种string[]数组方法。

```java
public static String[] getMaxSameString1(String str1, String str2) {
    if (str1 != null && str2 != null)
    {
        StringBuffer stringBuffer = new StringBuffer();
        String stringMin = str1.length() <= str2.length()? str1 : str2;
        String stringMax = str1.length() > str2.length() ? str1 : str2;

        int length = stringMin.length();
        for (int i = 0; i < length; i++) {
            for (int x = 0, y = length - i; y <= length; y++, x++) {
                String string = stringMin.substring(x, y);
                if (stringMax.contains(string)) {
                    stringBuffer.append(string + ",");
                }
            }
            //如果较长的找到了，后面就不用找了 
            if (stringBuffer.length() != 0)
                break;
        }
        String[] split = stringBuffer.toString().replaceAll(",$", "").split("\\,");//以字符分割
        return split;
    }
    return null;
}
```

```java
public class Test {
    public static void main(String[] args) {
        String str = null;
        StringBuffer sb = new StringBuffer();
        sb.append(str);

        System.out.println(sb.length());//4
        System.out.println(sb);//null

        StringBuffer sb1 = new StringBuffer(str);
        System.out.println(sb1);//抛异常
    }
}
```

## data类

### 两个构造方法：

```java
Date(){}//分配并初始化此对象，以表示分配它的时间（精确到毫秒）
```

```java
Date(long date)//分配 Date 对象并初始化此对象，以表示自从标准基准时间（称为“历元（epoch）”，即 1970 年 1 月 1 日 00:00:00 GMT）以来的指定毫秒数。
```

### 一个成员方法：

```java
long getTime()//返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。
```

## SimpleDateFormate类 DateFormat类（日期格式化类）

这个类是一个**抽象类**。在API文档中有一个`SimpleDateFormat entends DateFormat`类。

### 构造方法：

```java
SimpleDateFormat(String pattern){}
```

String pattern 传递指定的模式。

| -    | -    |
| ---- | ---- |
| y    | 年   |
| M    | 月   |
| d    | 日   |
| H    | 时   |
| m    | 分   |
| s    | 秒   |

例如：`"yyyy-MM-dd HH : mm :: ss"`

这之中只有连接模式的符号可以改，字母不可以更改。而且字母也可以不用写那么多， 只要有一个即可，但是为了表示方便，最好还全加上，让别人更容易看清。

### 成员方法：

```java
String format(Date date);//按照指定的模式，把Date日期，格式化为符合模式的字符串
Date parse(String source);//把符合模式的字符串，解析为Date日期
```

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class Demo01 {
    public static void main(String[] args) throws ParseException {
        Date date = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        System.out.println(date);
        System.out.println(simpleDateFormat.format(date));
        System.out.println(simpleDateFormat.parse(simpleDateFormat.format(date)));
    }
}

```

这里的parse方法有点特殊，它声明了一个解析异常，

> Data parse (string source, ParsePosition pos) 解析字符串的文本，生成 `Date`。

这个是解析，但它的作用是让保证传进去的格式是正确的。有两种方案可以解决这个问题：

1. throws抛出这个异常
2. try catch自己处理

第一种就是加一个包`import java.text.ParseException;`。

总结来说：

1. 构造时要给出格式
2. 给data打印指定的格式
3. 给构造是用的格式，打印指定的data（如果不是构造时的那个格式就会报错）

1. 构造器

   ```java
   Date date1 = new Date();
   Date date2 = new Date(15515151115L);
   ```

2. 方法

   ```java
   toString():显示当前的年、月、日、时、分、秒
   getTime():返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。
   ```

3. java.sql.Date 对应着数据库中的日期类型的变量

### 面试题

1. 字符串"2020-09-08"转换为java.sql.Date

```java
public class Test {
    public static void main(String[] args) throws ParseException {
        String birth = "2020-09-08";

        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
        Date date = simpleDateFormat.parse(birth);
        System.out.println(date);

        java.sql.Date birthDate = new java.sql.Date(date.getTime());
        System.out.println(birthDate);
    }
}
```

先转成Date再转成sql.Date

## Calendar日历类的使用

```java
public class Test {
    public static void main(String[] args) throws ParseException {
        /*
        实例化：
        1. 创建子类对象
        2. 调用其静态方法getInstance()
         */
        Calendar calendar = Calendar.getInstance();
        //get
        int i = calendar.get(Calendar.DAY_OF_MONTH);
        System.out.println(i);
        //set
        calendar.set(Calendar.DAY_OF_MONTH, 22);
        System.out.println(calendar.get(Calendar.DAY_OF_MONTH));
        //add()
        calendar.add(Calendar.DAY_OF_MONTH, -3);
        System.out.println(calendar.get(Calendar.DAY_OF_MONTH));
        //getTime()返回一个Date类
        Date time = calendar.getTime();
        System.out.println(time);
        //setTime()传Date参数将其转化成日历类
        String date = "1913-12-31";
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");

        calendar.setTime(dateFormat.parse(date));
        System.out.println(calendar.get(Calendar.DAY_OF_MONTH));

    }
}
```

## Java8中新日期时间API

### LocalDate

### LocalTime

### LocalDateTime

两个实例化方法now与of，有get与set(with)方法，有返回值，说明不可以被改变。

```java
public class Test {
    public static void main(String[] args) throws ParseException {
        LocalDate localDate = LocalDate.now();
        LocalTime localTime = LocalTime.now();
        LocalDateTime localDateTime = LocalDateTime.now();
        Date date = new Date(2020, 8, 23);
        System.out.println(date);

        LocalDateTime localDateTime1 = LocalDateTime.of(2020, 8, 23, 12,32);
        System.out.println(localDateTime1);
        System.out.println(localDateTime.getDayOfMonth());
        System.out.println(localDateTime.getDayOfWeek());
        LocalDate localDate1 = localDate.withDayOfMonth(22);
        System.out.println(localDate1);
        System.out.println(localDate);
        LocalDate localDate2 = localDate.plusDays(3);
        System.out.println(localDate2);


    }
}
```

## instant瞬间

有些像java.util.Date类

```java
public class Test {
    public static void main(String[] args) throws ParseException {
        //now():获取本初子午线对应的标准时间
        Instant instant = Instant.now();
        System.out.println(instant);
        //添加时间偏移量
        OffsetDateTime offsetDateTime = instant.atOffset(ZoneOffset.ofHours(8));
        System.out.println(offsetDateTime);
        
        //1970年1月1日00时00分00秒(UTC)开始的毫秒数
        long milli = instant.toEpochMilli();
        System.out.println(milli);
        
        Instant instant1 = Instant.ofEpochMilli(1313131);
        System.out.println(instant1);
    }
}
```

## DataTimeFormatter类

1. 预定义标准格式

```
public class Test {
    public static void main(String[] args) throws ParseException {
        DateTimeFormatter formatter = DateTimeFormatter.ISO_DATE;
        //格式化：日期->字符串
        LocalDateTime localDateTime = LocalDateTime.now();
        String string = formatter.format(localDateTime);
        System.out.println(string);

        //解析：字符串->日期
        TemporalAccessor parse = formatter.parse(string);
        System.out.println(parse);
    }
}
```

2. 本地化相关格式

```java
        //方式二：本地化相关格式
        //FormatStyle.FULL / FormatStyle.LONG / FormatStyle.MEDIUM / FormatStyle.SHORT ：适用于LocalDate
        DateTimeFormatter formatter1 = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.FULL);
        //格式化
        String string = formatter1.format(localDateTime);
        System.out.println(string);
```

不知道为什么，这段代码会抛出异常。

3. **自定义格式**

```java
        //重点：自定义的格式
        DateTimeFormatter formatter2 = DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
        //格式化
        String format1 = formatter2.format(LocalDateTime.now());
        System.out.println(format1);
        //解析
        TemporalAccessor parse1 = formatter2.parse("2019-02-18 03:52:09");
        System.out.println(parse1);
```

可见DataTimeFormatter就是一个格式化的类，有三种方法，自定义的方法是最重要的，有解析与格式化两种操作，格式化得到的结果是一个字符串，解析得到的结果是一个接口对象。

## Arrays排序，实现Comparable接口

>     1. String、包装类等实现了Comparable接口，重写了compareTo(obj)方法，给出了比较两个对象大小。
>     2. 重写compareTo(obj)：如果当前this大于obj返回正整数，小于返回负整数，等于返回0
>     3. 自定义类，如果要排序，可以让自定义类实现Comparable接口，重写comparaTo()，在comparaTo()



> ```
> public static void sort(Object[] a)
> ```
>
> 根据元素的自然顺序对指定对象数组按升序进行排序。数组中的所有元素都必须实现 Comparable 接口。此外，数组中的所有元素都必须是可相互比较的（也就是说，对于数组中的任何 e1 和 e2 元素而言，e1.compareTo(e2) 不得抛出 ClassCastException）。
> 保证此排序是稳定的：不会因调用 sort 方法而对相等的元素进行重新排序。
>
> 该排序算法是一个经过修改的合并排序算法（其中，如果低子列表中的最高元素小于高子列表中的最低元素，则忽略合并）。此算法提供可保证的 n*log(n) 性能。 

```java
public class Goods implements Comparable {
    private String name;
    private double price;

    @Override
    public int compareTo(Object o) {
        if (o instanceof Goods)
        {
            Goods goods = (Goods)o;
            if (this.price > goods.price)
                return 1;
            else if (this.price < goods.price)
                return -1;
            else
                return 0;
//        return Double.compare(this.price, goods.price);
        }
        throw new RuntimeException("传入的数据类型不一致");
    }
}
```

## 使用Comparator

>    comparator：定制排序
>      当元素的类型没有实现java.lang.Comparable接口而又不方便修改代码，
>      或者实现了java.lang.Comparable接
>      口的排序规则不适合当前的操作，那么可以考虑使用Comparator的对象来排序。
>
>    重写compare(Object o1, Object o2)方法，比较o1和o2的大小：
>    o1>o2返回正整数
>    o2>o1返回负整数
>    否则返回0

```java
public class Test {
    public static void main(String[] args) {
        String[] array = new String[]{"AA", "BB", "XX", "HH"};

        Arrays.sort(array, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                if (o1 instanceof String && o2 instanceof String)
                {
                    String s1 = (String)o1;
                    String s2 = (String)o2;
                    return -s1.compareTo(s2);
                }
                throw new RuntimeException("输入数据类型不一致");
            }
        });
        System.out.println(Arrays.toString(array));
        Goods[] goods = new Goods[6];
        goods[0] = new Goods("lenove", 332);
        goods[2] = new Goods("xiaoMi", 32);
        goods[5] = new Goods("Dell", 78);
        goods[4] = new Goods("Dell", 4);
        goods[1] = new Goods("HuaWei", 3);
        goods[3] = new Goods("Dell", 3323);
        Arrays.sort(goods, new Comparator<Goods>() {
            @Override
            public int compare(Goods o1, Goods o2) {
                if (o1 instanceof Goods && o2 instanceof Goods)
                {
                    if (o1.getName().equals(o2.getName()))//名字相同比较price
                    {
                        return Double.compare(o1.getPrice(), o2.getPrice());
                    }
                    else//名字不同比较名字
                    {
                        return o1.getName().compareTo(o2.getName());
                    }
                }
                throw new RuntimeException("输入数据类型不一致");
            }
        });
        for (Goods goods1 : goods)
            System.out.println(goods1);
    }
}
[XX, HH, BB, AA]
Goods{name='Dell', price=4.0}
Goods{name='Dell', price=78.0}
Goods{name='Dell', price=3323.0}
Goods{name='HuaWei', price=3.0}
Goods{name='lenove', price=332.0}
Goods{name='xiaoMi', price=32.0}
```

当然，也可以重写Comparator的compare方法

```java
public class Com implements Comparator {

    @Override
    public int compare(Object o1, Object o2) {
        if (o1 instanceof Goods && o2 instanceof Goods)
        {
            Goods goods1 = (Goods)o1;
            Goods goods2 = (Goods)o2;
            if (goods1.getName().equals(goods2.getName()))
            {
                return Double.compare(goods1.getPrice(), goods2.getPrice());
            }
            else
            {
                return goods1.getName().compareTo(goods2.getName());
            }
        }
        throw new RuntimeException("输入数据类型不一致");
    }
}
```

## Comparator与Comparable

> Comparator接口属于临时性的比较。
>
> Comparable接口的方式一旦一定，保证Comparable接口实现类的对象在任何位置都可以比较大小。

## 面试题

1. 将字符串“2017-08-16”转换为对应的java.sql.Date类的对象。

   > 思路：java.sql.Date对象通过

   ```
   Date(long date)使用给定毫秒时间值构造一个 Date 对象。
   ```

   构造器创建。我们要获得long date就要调用Date的方法。

   ```
   long getTime() 返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。 
   ```

   这样我们就必须要知道Date，可以用SimpleDateFormat类的解析方法来获得一个“2017-08-16”的Date

```java
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
        java.util.Date parse = simpleDateFormat.parse("2017-08-16");
        java.sql.Date date = new java.sql.Date(parse.getTime());
```

据说还有一种方法说是用DataTimeFormatter，我没有找到。

2. 解释何为编码？解码？何为日期时间的格式化？解析？
   * 编码：字符串->字节
   * 解码：字节->字符串
   * 格式化：日期->字符串
   * 解析：字符串->日期

3. 用自然排序与定制排序

```java
public class Test {
    public static void main(String[] args) {
        Person[] person = new Person[5];
        person[0] = new Person("zjj", 21);
        person[1] = new Person("sm", 20);
        person[2] = new Person("zm", 32);
        person[3] = new Person("lg", 24);
        person[4] = new Person("mg", 2);

        Arrays.sort(person, new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                return -Integer.compare(o1.getAge(), o2.getAge());
            }
        });

        for (Person p : person)
            System.out.println(p);
        Arrays.sort(person);

        for (Person p : person)
            System.out.println(p);
    }
}
class Person implements Comparable
{
    private String name;
    private int age;
    Person(String name, int age)
    {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public int compareTo(Object o) {
        Person p = (Person)o;
        if (p.age > this.age)
            return -1;
        else if (p.age == this.age)
            return 0;
        else
            return 1;
    }
}
```

# 集合

> 集合、数组都是对多个数据进行存储操作的结构，简称Java容器，此时的存诸是指内存方面的存储，没有持久化的存储。

![image-20201001215019371](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201001215019371.png)

## 集合框架

* Collection 单列集合，用来存储一个一个的对象
  * ​    List 有序，可重复 “动态数组”
    ​        ArrayList、LinkedList、Vector
  * ​    set 无序，不可重复 高中的集合“无序，互异，确定性”
    ​         HashSet、LinkedHashSet、TreeSet
* Map 双列集合，用来存储一对一对的数据 “函数”
      HashMap、LinkedHashMap、TreeMap、Hashtable、Properties

## Collection接口中的方法

```java
@Test
public void test1(){
    Collection collection = new ArrayList();
    collection.add(124);
    collection.add("afd");
    collection.add("BB");
    System.out.println(collection.size());
    Collection collection1 = new ArrayList();
    collection1.add("aa");
    collection1.add("bb");
    collection.addAll(collection1);
    System.out.println(collection);
    collection.clear();
    System.out.println(collection1);
    System.out.println(collection);

    System.out.println(collection.isEmpty());
}
```

```java
boolean add(E e);
	将指定的元素添加到此列表的尾部。 
boolean addAll(Collection<? extends E> c);
	按照指定 collection 的迭代器所返回的元素顺序，将该 collection 中的所有元素添加到此列表的尾部 
void clear();
	移除此列表中的所有元素。 
boolean isEmpty();
	如果此列表中没有元素，则返回 true  
int size();
	返回此列表中的元素数。 
```

```java
@Test
public void test1(){
    Collection collection = new ArrayList();

    collection.add(new Person("Jerry", 20));
    System.out.println(collection.contains(new Person("Jerry", 20)));
    collection = Arrays.asList("hh", "hhk");
    System.out.println(collection);
}
//false
//[hh, hhk]

```

```java
static <T> List<T> asList(T... a) 
          返回一个受指定数组支持的固定大小的列表。 
boolean contains(Object o) 
          如果此列表中包含指定的元素，则返回 true。 
```

第一个返回的是false，因为contains要调用这个类的equals方法，默认的equals方法比较的是对象的地址，所以重写这个方法才行，比较值。

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

```java
    @Test
    public void test3(){
        Collection collection = new ArrayList();
        collection.add(123);
        collection.add(456);
        collection.add(new Person("Jerry",20));
        collection.add(new String("Tom"));
        collection.add(false);

        System.out.println(collection.hashCode());
        //collection与数组之间的转化
        Object[] arr = collection.toArray();
        System.out.println(Arrays.toString(arr));

        collection = Arrays.asList("AA");
        System.out.println(collection);
        List arr1 = Arrays.asList(new int[]{123, 456});//这样这个数组会被当成一个整体，没有必要用基本类型
        System.out.println(arr1.size());//1

        List arr2 = Arrays.asList(new Integer[]{123, 456});
        System.out.println(arr2.size());//2
    }
```

```java
boolean removeAll(Collection<?> c) 
		Removes from this list all of its elements that are contained in the specified collection. 
public Object[] toArray()
    	返回包含此 collection 中所有元素的数组。如果 collection 对其迭代器返回的元素顺序做出了某些保证，那么此方法必须以相同的顺序返回这些元素。
public int hashCode()
    	返回该对象的哈希码值。支持此方法是为了提高哈希表（例如 java.util.Hashtable 提供的哈希表）的性能。 
```

```java
    @Test
    public void iteratorTest()
    {
        Collection collection = new ArrayList();
        collection.add(123);
        collection.add(456);
        collection.add(new Person("Jerry",20));
        collection.add(new String("Tom"));
        collection.add(false);
        Iterator iterator = collection.iterator();

        while(iterator.hasNext())
        {
            System.out.println(iterator.next());
        }
    }
```

## 迭代器

```java
Iterator<E> iterator() Returns an iterator over the elements in this list in proper sequence. 
```

`hasNext`判断当前指针指向的容器位置是否有值，指针的初始位置在第一个元素前，`Next`将指针后移并返回当前指向的元素。

```java
@Test
public void iteratorTest()
{
    Collection collection = new ArrayList();
    collection.add(123);
    collection.add(456);
    collection.add(new Person("Jerry",20));
    collection.add(new String("Tom"));
    collection.add(false);
    Iterator iterator = collection.iterator();

    while(iterator.hasNext())
    {
        Object object = iterator.next();
        if ("Tom".equals(object))
        {
            iterator.remove();
        }
    }
    iterator = collection.iterator();
    while (iterator.hasNext())
    {
        System.out.println(iterator.next());
    }
}
```

```java
void remove() 
          从迭代器指向的 collection 中移除迭代器返回的最后一个元素（可选操作）。 
```

不要在没有调用`hasNext`之前调用`remove`，因为指针还没有指到有效值。也不要在调用完remove后，在下一次调用next之前再次调用reomve。同理，也是因为这个next指向的值已经被删除了，没有指到有效值。

**先移动，后删除**

## 增强for循环，for - each

> for (集合元素的类型 局部变量：对象)

其原理是每次从对象中取一个元素（是不是有点像iterator迭代器？），赋给前面这个局部变量。

如果不用上面的`while`做循环，还可以用for-each，其原理就是iterator迭代器。

```java
for (Object o : collection)
{
    System.out.println(o);
}
调试可以得知，会调用下面这个方法
public Iterator<E> iterator() {
    return new Itr();
}
```

因为是局部变量，所以用for-each来改变也不会改变到原本的数据

```java
@Test
public void forTest()
{
    String[] array = {"MM", "MM", "MM"};
    for (int i = 0; i < array.length; i++) {
        array[i] = "GG";
    }
    for (int i = 0; i < array.length; i++) {
        System.out.println(array[i]);
    }
    for (String string : array) {
        string = "SS";//并没有改变原来的GG
    }
    for (int i = 0; i < array.length; i++) {
        System.out.println(array[i]);
    }
}
/*
GG
GG
GG
GG
GG
GG
*/
```

##    ArrayList、LinkedList、Vector

* ArrayList：作为List接口的主要实现类， 线程不安全的，效率高，底层使用Object[] elementData存储
* LinkedList：对于频繁的插入、删除操作，使用此类效率比ArrayList高，底层使用双向链表存储
* Vector：作为List接口的古老实现类，线程安全的， 效率低
  相同点：三个类都是实现了List接口，存储数据的特点相同：存储有序的、可重复的数据

### ArrayList在jdk7与8中的不同

ArrayList源码分析：

* jdk7中

```java
ArrayList list = new ArrayList();
```

new的空参构造器，底层创建了长度是10的Object[]数组elementData
如果底层的容量不够就扩容，默认扩容到原来的1.5倍，同时还要将原有数组中的数据copy过去

如果长度不会改变，最好在new时就确定好长度

* jdk8中
  new的空参构造器，底层数组初始化为{}，只有在add后才会造一个长度为10的数组，同时将数据添加进去

> jdk7中ArrayList创建对象类似于单例中的饿汉式，而jdk8中类似于懒汉式。

### Vector源码

> 底层也是创建了一个长度为10的数组，扩容时到原来的两倍。

## ArrayList的三种构造器

```java
ArrayList() 
	Constructs an empty list with an initial capacity of ten. 空参，长度为10
ArrayList(Collection<? extends E> c) 
	Constructs a list containing the elements of the specified collection, in the order they are returned by 	 the collection's iterator. 
ArrayList(int initialCapacity) 
	Constructs an empty list with the specified initial capacity. 有参数指定长度

```

```java
向集合中添加一组元素的三种方法：
Arrays.asList()可以接受一个数组并返回一个List对象
1. Collection构造器可以接受一个Collection参数。用于对其自身的初始化。
2. Collection.addAll()方法接受一个Collection对象。将Collection中的内容添加到Collection后面去。
3. Collections.addAll()方法接受一个Collection，以及一个数组或是一个用逗号分割的列表，将此列表添加到这个Collection后面去
```

这里要注意的是，Collection是指的容器类（集合）是一个类，而Collections是一个工具类。

### ArrayList中的方法

```java
@Test
public void test1(){
    ArrayList list = new ArrayList();
    list.add(124);
    list.add(353);
    list.add("AA");
    list.add(new Person("TOM", 23));
    list.add(353);

    System.out.println(list);
    List list1 = Arrays.asList(1, 2, 3);
    list.addAll(list1);
    System.out.println(list.size());

    System.out.println(list.get(0));
    //不存在返回-1
    System.out.println(list.indexOf("AA"));

    System.out.println(list.lastIndexOf(353));

    Object object = list.remove(0);

    System.out.println(object);

    list.set(1, "CC");

    System.out.println(list);

    System.out.println(list.subList(0,3));
    
    //两种遍历方法
    1. iterator
    Iterator iterator = list.iterator; 
    while(iterator.hasNext())
    {
    	System.out.println(iterator.next());
    }
    2. for-each
    for (Object o : list)
    {
    	System.out.println(o);
    }
}
```

```java
public static <T> List<T> asList(T... a)返回一个受指定数组支持的固定大小的列表。（对返回列表的更改会“直接写”到数组。）此方法同 Collection.toArray() 一起，充当了基于数组的 API 与基于 collection 的 API 之间的桥梁。返回的列表是可序列化的，并且实现了 RandomAccess。 
此方法还提供了一个创建固定长度的列表的便捷方法，该列表被初始化为包含多个元素： 

```

这个方法返回的List不能add()， remove()，只是作为桥梁用，要正真去增删改查还是要自己new。

```java
 E get(int index) 
          返回此列表中指定位置上的元素。 
 int indexOf(Object o) 
     	  返回此列表中首次出现的指定元素的索引，或如果此列表不包含元素，则返回 -1。 
 int lastIndexOf(Object o) 
          返回此列表中最后一次出现的指定元素的索引，或如果此列表不包含索引，则返回 -1。 
 E remove(int index) 
 E remove(Object o)
          移除此列表中(指定位置上)的元素。 
 List<E> subList(int fromIndex, int toIndex) 左闭右开
		  Returns a view of the portion of this list between the specified fromIndex, inclusive, and toIndex, exclusive 
    
```

### 面试题

```java
@Test
public void testListRemove()
{
    List list = new ArrayList();
    list.add(1);
    list.add(2);
    list.add(3);
    updateList(list);
    System.out.println(list);
}
private static void updateList(List list)
{
    list.remove(2);//删掉是位置，还是数据？ 是位置，编译器不想把它当成一个Object子类，它偷懒了
    list.remove(new Integer(2));//这个删掉的才是数据
}
```

## LinkedList的方法

注意LinkedList是List的子类，有的方法是自己的新方法，不要使用多态向上转型了。用不了新方法。

```java
LinkedList<Integer> list = new LinkedList<>(Arrays.asList(1, 2, 3 ,4, 5));
//首部添加
list.addFirst(0);
//尾部添加
list.add(100);
list.addLast(8);
//获取首元素
try {
    System.out.println("list.getFirst() = " + list.getFirst());
    System.out.println("list.element() = " + list.element());//为空时抛出异常
}catch (Exception e){
    e.getStackTrace();
}

System.out.println("list.peek() = " + list.peek());//为空时返回null
//删除首元素
try {
    System.out.println("list.removeFirst() = " + list.removeFirst());
    System.out.println("list.remove() = " + list.remove());//为空时抛出异常
}catch (Exception e){
    e.getStackTrace();
}
System.out.println("list.poll() = " + list.poll());
System.out.println("list.removeLast() = " + list.removeLast());//为空时抛出异常
```

## HashSet、LindkedHashSet、TreeSet

* HashSet：作为Set接口的主要实现类，线程不安全的，可以存储null
* LinkedHashSet：作为HashSet的子类，遍历其内部数据时，可以按照添加的顺序遍历
* TreeSet：可以照添加对象的指定属性，进行排序

### HashSet源码

一、什么叫无序的，不可重复的：以HashSet为例

1. 无序性：不等于随机性，存储的数据在底层数组中并非按照数组索引的顺序添加，而是根据数据的哈希值添加

2. 不可重复性：保证添加的元素按照equals()判断时，不能返回true。即：相同的元素只能添加一个

二、添加元素的过程：以HashSet为例

> 底层用一个长度为16的数组，用add添加元素时，先调用此元素所在类的hashCode()方法，计算
> 元素a的哈希值，通过某种算法计算出HashSet底层数组中的存放位置（即为：索引位置）
> （对16取余，放到数组中）
> 如果此位置上没有其它元素，成功添加
> 如果此位置上有其它元素（或以链表形式存在的多个元素），则比较元素a与元素b的hash值，
>     	如果hash值不相同，则成功 (情况1)
>     	如果hash值相同，调用equals()方法
>         	返回true，添加失败
>         	返回false，添加成功 (情况2)
>
> 对于情况1与情况2，链表是谁接上谁有所不同（java8中是原来的接上新的，java7中是后来的占具原来的在数组中的位置，再接上原来的，七上八下）

**HashSet底层是数组加链表**

![image-20201003142525745](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201003142525745.png)

在IDE中重写的hashCode方法是这样的

```java
    @Override
    public int hashCode() {
        int result = name != null ? name.hashCode() : 0;
        result = 31 * result + age;
        return result;
    }
```

![image-20201003142707548](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201003142707548.png)

向Set中添加的元素，需要重写hashCode与equals方法。

### LinedHashSet

> LinkedHashSet是HashSet的子类，添加数据的同时，每个数据还维护了两个引用，
> 记录前后的数据。双向链表。
> 对于频繁的遍历，LinkedHashSet比HashSet更快。

### TreeSet的自然排序与定制排序

> 默认构造器调用对象重写的comparaTo方法。

有参构造器

```java
TreeSet(Comparator<? super E> comparator) 
          构造一个新的空 TreeSet，它根据指定比较器进行排序。
```

```java
@Test
public void treeSetTest() {
    Comparator comparator = new Comparator() {
        @Override
        public int compare(Object o1, Object o2) {
            if (o1 instanceof User && o2 instanceof User) {
                User user1 = (User) o1;
                User user2 = (User) o2;
                int a = Integer.compare(user1.getAge(), user2.getAge());
                if (a != 0) {
                    return a;
                } else {
                    return user1.getName().compareTo(user2.getName());
                }
            }
            throw new RuntimeException("the Type is wrong");
        }
    };
    TreeSet treeSet = new TreeSet(comparator);
    treeSet.add(new User("Mike", 32));
    treeSet.add(new User("Jerry", 42));
    treeSet.add(new User("Tom", 4));
    treeSet.add(new User("Jack", 42));
    treeSet.add(new User("Jack", 12));

    Iterator iterator = treeSet.iterator();
    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }
}
/*
User{name='Tom', age=4}
User{name='Jack', age=12}
User{name='Mike', age=32}
User{name='Jack', age=42}
User{name='Jerry', age=42}
*/
```

### 面试题

1. 去掉ArraList中重复的值  

```java
@Test
public void Test(){
    List list = new ArrayList();
    list.add(1);
    list.add(2);
    list.add(2);
    list.add(4);
    list.add(4);
    List list1 = duplicateList(list);
    for (Object o: list1) {
        System.out.println(o);
    }

}

private List duplicateList(List list) {
    HashSet hashSet = new HashSet();
    hashSet.addAll(list);
    return new ArrayList(hashSet);
}
```

2. Person类已重写equals()，`hashCode()`方法

```java
@Test
public void Test3(){
    Person person = new Person("AA", 1001);
    HashSet hashSet = new HashSet();
    
    hashSet.add(person);
    hashSet.add(new Person("BB", 1002));
    
    person.setName("CC");//属性被更改，但是还是在AA的hashCode位置上的值
    hashSet.remove(person);//这次要删的是CC,1001得到的hashCode位置上的值，显然没有值
    
    hashSet.add(new Person("CC", 1001));//加到CC1001hashCode位置上去了

    hashSet.add(new Person("AA", 1001));//本来是要加到AA的hashCode上的，但是比较equals时不同，所以以链表接上了

    for (Object o : hashSet)
        System.out.println(o);

}
```

## HashMap、TreeMap、Hashtable、Properties

* HashMap：作为Map的主要实现类：线程不安全; 存储null的key和value
  * LinkedHashMap：保证在遍历map元素时，可以按照添加的顺序实现遍历
            原因：在原有的HashMap底层结构基础上，添加了一对指针，分别指向前后两个元素
            对于频繁的遍历操作，此类执行效率高于HashMap
* TreeMap：保证按照添加的key-value对进行排序，实现排序遍历。此时考虑key的自然排序或者定制排序
          底层使用红黑树http://www.cnblogs.com/yangecnu/p/lntroduce-Red-Black-Tree.html
* Hashtable：作为古老的实现类; 线程安全的，不能存储null的key和value（返回空指针异常)
     * Properties：常用来处理配置文件。key和value都是String类型

### Map结构的理解（key-value)是什么？

Map中的key：无序，不可重复，使用Set存——> key所在类要重写equals()和hashCode()方法
Map中的value：无序，可重复，使用Collection存储所有的value
一个键值对：key-value构成了一个Entry对象
Map中的entry：无序，不可重复，使用Set存

### 面试问题

#### HashMap的底层实现原理（jdk7)

> HashMap map = new HashMap();
>  在实例化以后，底层创建了长度是16的一维数组Entry[] table
>  map.put(key1, value);
>  首先，调用key1所有类的hasCode()方法计算key1的哈希值，经过算法计算后，得到在Entry数组中存放的位置
>
> * 如果此位置上的数据为空，此时的key1-value1添加成功。------情况1
> * 如果此位置上的数据不为空，（此位置上存在一个或多个数据（链表来存）），比较key1与已经存在的一个或多个数据的哈希值：
>   * 如果key1的哈希值与已经存在的数据的哈希值都不相同， 此时key1-value1添加成功 ------情况2
>   * 如果key1的哈希值和已经存在的某一个数据(key2-value2)的哈希值相同，继续比较：调用key1所在类的equals()方法，比较：
>            *  如果equals()返回false，key1-value1添加成功------情况3
>                   *  否则：使用value1替换value
>
> ```
> 情况2与情况3：此时key1-value1和原来的数据以链表的方式存储。
> 在不断的添加过程中，会涉及到扩容问题，默认的扩容方式：扩容为原来容量的2倍，并将原有的数据复制过来
> ```

```java
DEFAULT_INITIAL_CAPACITY : HashMap的默认容量， 16
DEFAULT_LOAD_FACTOR: HashMap中的默认加载因子：0.75
threshold：扩容的临界值，= 容量 * 填充因子：16 * 0.75
TREEIFY_THRESHOLD: Bucket中链表长度大于该默认值，转化为红黑树: 8
```

![容器](C:\Users\31787\OneDrive\图片\xmind\xmind zen\容器.png)

#### LinkedHashMap底层实现

![](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201006093607302.png)

#### HshMap底层实现7与java8的不同点

>jdk8与7在底层实现方面的不同：
>  1. new是没有创建一个长度为16的数组
>  2. jdk8底层数组是Node[]，而非Entry[]
>  3. 首次调用put()方法时，底层创建长度为16的数组
>  4. jdk7底层没有红黑树
>      当数组的某一个索引位置上的元素，以链表形式存在的数据个数 > 8且当前数组长度 > 64
>      此时索引位置上的所有数据改为使用红黑树存储

### Map中的方法

#### 修改与判断

```java
@Test
public void Test2() {
    //Object put(Object key, Object value)
    Map map = new HashMap();
    map.put("AA", 123);
    map.put(45, 123);
    map.put("BB", 56);
    //修改
    map.put("AA", 87);

    //void putAll(Map m)
    System.out.println(map);
    Map map1 = new HashMap();
    map1.put("CC", 123);
    map1.put("DD", 123);
    map.putAll(map1);

    System.out.println(map);

    //Object remove(Object key)
    Object cc = map.remove("CC");
    System.out.println(cc);
    //void clear()
    map.clear();
    System.out.println(map.size());
    System.out.println(map);
}
public void test3() {
    Map map = new HashMap();
    map.put("AA", 123);
    map.put(45, 123);
    map.put("BB", 56);
    //Object get(Object key)
    System.out.println(map.get(45));

    //boolean containsKey(Object key)
    System.out.println(map.containsKey("BB"));

    //boolean containsValue(Object value)
    System.out.println(map.containsValue(123));

    //int size()
    System.out.println(map.size());

    //boolean isEmpty()
    map.clear();
    System.out.println(map.isEmpty());

    //boolean equals(Object obj)
    Map map1 = new HashMap();
    map1.putAll(map);
    System.out.println(map.equals(map1));
}
```

#### 遍历

> 元视图

```java
void clear() 
          从此映射中移除所有映射关系（可选操作）。 
 boolean containsKey(Object key) 
          如果此映射包含指定键的映射关系，则返回 true。 
 boolean containsValue(Object value) 
          如果此映射将一个或多个键映射到指定值，则返回 true。 
 
 boolean equals(Object o) 
          比较指定的对象与此映射是否相等。 
 V get(Object key) 
          返回指定键所映射的值；如果此映射不包含该键的映射关系，则返回 null。 
 int hashCode() 
          返回此映射的哈希码值。 
 boolean isEmpty() 
          如果此映射未包含键-值映射关系，则返回 true。 

 V put(K key, V value) 
          将指定的值与此映射中的指定键关联（可选操作）。 
 void putAll(Map<? extends K,? extends V> m) 
          从指定映射中将所有映射关系复制到此映射中（可选操作）。 
 V remove(Object key) 
          如果存在一个键的映射关系，则将其从此映射中移除（可选操作）。 
 int size() 
          返回此映射中的键-值映射关系数。 

 public String toString()
返回此映射的字符串表示形式。该字符串表示形式由键-值映射关系列表组成，按照该映射 entrySet 视图的迭代器返回的顺序排列，并用括号 ("{}") 括起来。相邻的映射关系是用字符 ", "（逗号加空格）分隔的。每个键-值映射关系按以下方式呈现：键，后面是一个等号 ("=")，再后面是关联的值。键和值都通过 String.valueOf(Object) 转换为字符串。 
```



```java
    @Test
    public void test8(){
        Map<String, Integer> map = new HashMap<>();
        map.put("zjjj", 213);
        map.put("smmm", 32);

        //key的元视图, 通过for-each与iterator两种遍历
        Set<String> key = map.keySet();
        for (String key1 : key)
        {
            System.out.println(key1 + " = " + map.get(key1));
        }
        Iterator<String> iterator = key.iterator();
        while (iterator.hasNext())
        {
            String next = iterator.next();
            System.out.println(next + " = " + map.get(next));
        }
        //value的元视图，只能遍历value
        Collection<Integer> value = map.values();
        for (Integer i :value)
        {
            System.out.println(i);
        }
        Iterator<Integer> iterator1 = value.iterator();
        while(iterator1.hasNext())
        {
            Integer i = iterator1.next();
            System.out.println(i);
        }

        //entrySet
        Set<Map.Entry<String, Integer>> set = map.entrySet();
        Iterator<Map.Entry<String, Integer>> iterator2 = set.iterator();
        while(iterator2.hasNext())
        {
            Map.Entry<String, Integer> entry = iterator2.next();
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }
        for (Object o : set)
        {
            System.out.println(o);//会调用HashMap中的toString()方法        public final String toString() { return key + "=" + value; }
        }
    }
zjjj = 213
smmm = 32
zjjj = 213
smmm = 32
213
32
213
32
zjjj = 213
smmm = 32
zjjj=213
smmm=32
```

```java
Set<Map.Entry<K,V>> entrySet() 
          返回此映射中包含的映射关系的 Set 视图。 
Set<K> keySet() 
          返回此映射中包含的键的 Set 视图。 
Collection<V> values() 
          返回此映射中包含的值的 Collection 视图。
```



### TreeMap

> 还是有自然排序与定制排序两种，但是只能对key进行排序。

### Properties

```java
public class PropertiesTest {
    public static void main(String[] args) throws IOException {
        Properties pros = new Properties();
        FileInputStream fis = new FileInputStream("jdbc.properties");//对物理文件进行操作
        pros.load(fis);
        String name = pros.getProperty("name");
        String pass = pros.getProperty("pass");
        System.out.println("name = " + name + ", pass = " + pass);
    }
}
```

### Collections工具类

```java
方法摘要 
static <T> boolean 
 addAll(Collection<? super T> c, T... elements) 
          将所有指定元素添加到指定 collection 中。 
static <T> Queue<T> 
 asLifoQueue(Deque<T> deque) 
          以后进先出 (Lifo) Queue 的形式返回某个 Deque 的视图。 
static <T> int 
 binarySearch(List<? extends Comparable<? super T>> list, T key) 
          使用二分搜索法搜索指定列表，以获得指定对象。 
static <T> int 
 binarySearch(List<? extends T> list, T key, Comparator<? super T> c) 
          使用二分搜索法搜索指定列表，以获得指定对象。 
static <E> Collection<E> 
 checkedCollection(Collection<E> c, Class<E> type) 
          返回指定 collection 的一个动态类型安全视图。 
static <E> List<E> 
 checkedList(List<E> list, Class<E> type) 
          返回指定列表的一个动态类型安全视图。 
static <K,V> Map<K,V> 
 checkedMap(Map<K,V> m, Class<K> keyType, Class<V> valueType) 
          返回指定映射的一个动态类型安全视图。 
static <E> Set<E> 
 checkedSet(Set<E> s, Class<E> type) 
          返回指定 set 的一个动态类型安全视图。 
static <K,V> SortedMap<K,V> 
 checkedSortedMap(SortedMap<K,V> m, Class<K> keyType, Class<V> valueType) 
          返回指定有序映射的一个动态类型安全视图。 
static <E> SortedSet<E> 
 checkedSortedSet(SortedSet<E> s, Class<E> type) 
          返回指定有序 set 的一个动态类型安全视图。 
static <T> void 
 copy(List<? super T> dest, List<? extends T> src) 
          将所有元素从一个列表复制到另一个列表。 
static boolean disjoint(Collection<?> c1, Collection<?> c2) 
          如果两个指定 collection 中没有相同的元素，则返回 true。 
static <T> List<T> 
 emptyList() 
          返回空的列表（不可变的）。 
static <K,V> Map<K,V> 
 emptyMap() 
          返回空的映射（不可变的）。 
static <T> Set<T> 
 emptySet() 
          返回空的 set（不可变的）。 
static <T> Enumeration<T> 
 enumeration(Collection<T> c) 
          返回一个指定 collection 上的枚举。 
static <T> void 
 fill(List<? super T> list, T obj) 
          使用指定元素替换指定列表中的所有元素。 
static int frequency(Collection<?> c, Object o) 
          返回指定 collection 中等于指定对象的元素数。 
static int indexOfSubList(List<?> source, List<?> target) 
          返回指定源列表中第一次出现指定目标列表的起始位置；如果没有出现这样的列表，则返回 -1。 
static int lastIndexOfSubList(List<?> source, List<?> target) 
          返回指定源列表中最后一次出现指定目标列表的起始位置；如果没有出现这样的列表，则返回 -1。 
static <T> ArrayList<T> 
 list(Enumeration<T> e) 
          返回一个数组列表，它按返回顺序包含指定枚举返回的元素。 
static <T extends Object & Comparable<? super T>> 
T 
 max(Collection<? extends T> coll) 
          根据元素的自然顺序，返回给定 collection 的最大元素。 
static <T> T 
 max(Collection<? extends T> coll, Comparator<? super T> comp) 
          根据指定比较器产生的顺序，返回给定 collection 的最大元素。 
static <T extends Object & Comparable<? super T>> 
T 
 min(Collection<? extends T> coll) 
          根据元素的自然顺序 返回给定 collection 的最小元素。 
static <T> T 
 min(Collection<? extends T> coll, Comparator<? super T> comp) 
          根据指定比较器产生的顺序，返回给定 collection 的最小元素。 
static <T> List<T> 
 nCopies(int n, T o) 
          返回由指定对象的 n 个副本组成的不可变列表。 
static <E> Set<E> 
 newSetFromMap(Map<E,Boolean> map) 
          返回指定映射支持的 set。 
static <T> boolean 
 replaceAll(List<T> list, T oldVal, T newVal) 
          使用另一个值替换列表中出现的所有某一指定值。 
static void reverse(List<?> list) 
          反转指定列表中元素的顺序。 
static <T> Comparator<T> 
 reverseOrder() 
          返回一个比较器，它强行逆转实现了 Comparable 接口的对象 collection 的自然顺序。 
static <T> Comparator<T> 
 reverseOrder(Comparator<T> cmp) 
          返回一个比较器，它强行逆转指定比较器的顺序。 
static void rotate(List<?> list, int distance) 
          根据指定的距离轮换指定列表中的元素。 
static void shuffle(List<?> list) 
          使用默认随机源对指定列表进行置换。 
static void shuffle(List<?> list, Random rnd) 
          使用指定的随机源对指定列表进行置换。 
static <T> Set<T> 
 singleton(T o) 
          返回一个只包含指定对象的不可变 set。 
static <T> List<T> 
 singletonList(T o) 
          返回一个只包含指定对象的不可变列表。 
static <K,V> Map<K,V> 
 singletonMap(K key, V value) 
          返回一个不可变的映射，它只将指定键映射到指定值。 
static <T extends Comparable<? super T>> 
void 
 sort(List<T> list) 
          根据元素的自然顺序 对指定列表按升序进行排序。 
static <T> void 
 sort(List<T> list, Comparator<? super T> c) 
          根据指定比较器产生的顺序对指定列表进行排序。 
static void swap(List<?> list, int i, int j) 
          在指定列表的指定位置处交换元素。 
static <T> Collection<T> 
 synchronizedCollection(Collection<T> c) 
          返回指定 collection 支持的同步（线程安全的）collection。 
static <T> List<T> 
 synchronizedList(List<T> list) 
          返回指定列表支持的同步（线程安全的）列表。 
static <K,V> Map<K,V> 
 synchronizedMap(Map<K,V> m) 
          返回由指定映射支持的同步（线程安全的）映射。 
static <T> Set<T> 
 synchronizedSet(Set<T> s) 
          返回指定 set 支持的同步（线程安全的）set。 
static <K,V> SortedMap<K,V> 
 synchronizedSortedMap(SortedMap<K,V> m) 
          返回指定有序映射支持的同步（线程安全的）有序映射。 
static <T> SortedSet<T> 
 synchronizedSortedSet(SortedSet<T> s) 
          返回指定有序 set 支持的同步（线程安全的）有序 set。 
static <T> Collection<T> 
 unmodifiableCollection(Collection<? extends T> c) 
          返回指定 collection 的不可修改视图。 
static <T> List<T> 
 unmodifiableList(List<? extends T> list) 
          返回指定列表的不可修改视图。 
static <K,V> Map<K,V> 
 unmodifiableMap(Map<? extends K,? extends V> m) 
          返回指定映射的不可修改视图。 
static <T> Set<T> 
 unmodifiableSet(Set<? extends T> s) 
          返回指定 set 的不可修改视图。 
static <K,V> SortedMap<K,V> 
 unmodifiableSortedMap(SortedMap<K,? extends V> m) 
          返回指定有序映射的不可修改视图。 
static <T> SortedSet<T> 
 unmodifiableSortedSet(SortedSet<T> s) 
          返回指定有序 set 的不可修改视图。 
```



```java
@Test
public void test1(){
    List list = new ArrayList();
    list.add(2);
    list.add(3);
    list.add(23);
    list.add(21);
    System.out.println(list);
	//返转
    Collections.reverse(list);
    System.out.println(list);
	//排序，记得重写equals()方法
    Collections.sort(list);
    System.out.println(list);
	//交换
    Collections.swap(list, 0, 1);
    System.out.println(list);
    // 返回指定 collection 中等于指定对象的元素数
    System.out.println(Collections.frequency(list,3));
    
    List dest = new ArrayList(list.size());//指定的是造底层数组的长度
    System.out.println(dest.size());//0
    //拷贝，拷贝前要先扩容
    List list1 = Arrays.asList(new Object[list.size()]);
	
    Collections.copy(list1, list);
    System.out.println(list1);
    
    //list2就是线程安全的list
    List list2 = Collections.synchronizedList(list);
}
```



# 泛型

1. jdk5.0新增特性
2. 在集合中使用泛型
   1. 实例化集合类时，可以指明具体的泛型类型
   2. 泛型的类型不可以是基本数据类型
   3. 如果没有指明泛型的类型，默认就是java.lang.Object类型

## 自定义泛型类

```java
public class Order<E> {
    E a;

    public Order() {
    }

    public Order(E a) {
        this.a = a;
    }

    public E getA() {
        return a;
    }

    public void setA(E a) {
        this.a = a;
    }
}
class SubOrder<E> extends Order<E>
{
    public SubOrder() {
    }

    public SubOrder(E a) {
        super(a);
    }
}
```

## 自定义泛型方法

> 在方法中出现了泛型的结构，泛型参数与类的泛型参数没有任何关系。泛型方法是不是泛型类中的都不必要。

```java
/**
* 泛型方法的基本介绍
* @param tClass 传入的泛型实参
* @return T 返回值为T类型
* 说明：
*     1）public 与 返回值中间<T>非常重要，可以理解为声明此方法为泛型方法。
*     2）只有声明了<T>的方法才是泛型方法，泛型类中的使用了泛型的成员方法并不是泛型方法。
*     3）<T>表明该方法将使用泛型类型T，此时才可以在方法中使用泛型类型T。
*     4）与泛型类的定义一样，此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型。
*/
public <T> T genericMethod(Class<T> tClass)throws InstantiationException ,
 IllegalAccessException{
       T instance = tClass.newInstance();
       return instance;
}
```

泛型方法可以是静态的，因为它与类的泛型没有任何关系，静态方法是属于类的，而类的泛型是在类实例化（造对象）时。静态方法的创建要早于泛型。

1. 泛型在继承方面的体现
    类A是类B的父类，G<A>，G<B>二者不具有子父类关系
    A<G>, B<G>
    List<String>, ArrayList<String>这样写可以

```java
    public void test3(){
        List<Object> list1 = null;
        List<String> list2 = null;

//        list1 = list2;编译不通过
        /*
        如果可以通过，list1与list2指向同一个内存地址，list1.add()可以添加
        Object类型的一些东西。这显然与list2的初衷不符。
         */
    }
```

2. 通配符的使用
   通配符：?
   类A是类B的父类，G<A>和G<B>是没有关系的，二者的共同父类是：G<?>

​       对于List<?>就不能向其内部添加数据。
​       除了添加null。
​       允许读取数据，读取的数据类型为Object。

```java
@Test
public void test2(){
    List<Object> list1 = new ArrayList<>();
    List<String> list2 = new ArrayList<>();

    List<?> list;

    list = list1;
    list = list2;

    list1.add(234);
    list2.add("fadf");
    show(list1);
    show(list2);
//  list.add("fd");
    list.add(null);
}
234
fadf
```

3. 有限制条件的通配符的使用
? extends A : G<? extends A> 可以作为G<A>和G<B>的父类，其中B是A的子类
? super A : G<? super A> 可以作为G<A>和G<B>的父类，其中B是A的父类
<? extends Person> 可以引用Person类与其子类，extends相当于小于等于
<? super Person> 可以引用Person类与其父类，super相当于大于等于

```java
    @Test
    public void test4(){
        List<? extends Person> list1 = null;
        List<? super Person> list2 = null;

        List<Student> list3 = null;
        List<Person> list4 = null;
        List<Object> list5 = null;

        list1 = list3;
        list1 = list4;
//        list1 = list5;

//        list2 = list3;
        list2 = list4;
        list2 = list5;
        //读数据
        list1 = list3;
        //list1不大于Person
        Person p = list1.get(0);
        //写数据
//        list1.add(new Student());多态性子类不能引用父类，?可能比Student类更小
        list2.add(new Person());//多态性父类可以引用子类，?一定比Person大

    
```

## 练习

```java
package io.github.zhengjj.exercise;

import java.util.*;

/**
 * @Description：
 * @Author：zhjj
 * @Date：2020-10-09 16:12
 */
public class DAO<T> {
   private Map<String, T> map = new HashMap<>();

   //保存T类型的变量到Map成员变量中
   public void save(String id, T entity)
   {
       map.put(id, entity);
   }
   //从map中获取id对应的对象
   public T get(String id)
   {
      T t = map.get(id);
      return t;
   }
   //替换map中key为id的内容，改为entity对象
   public void update(String id, T entity)
   {
       if (map.containsKey(id))
       {
          map.put(id, entity);
       }
   }
   //返回map中存放的所有T对象
   public List<T> list()
   {
       //这种方法不可行，在面向对象中，父类转到子类必须原来就是子类对象，只是用父类引用了而已
       /*
       Collection<T> list = map.values();
       return (List<T>)list;
       */
       List<T> list = new ArrayList<>();
       Collection<T> values = map.values();
       for (T t : values)
       {
           list.add(t);
       }
       return list;
   }
   //删除指定id对象
   public void delete(String id)
   {
       map.remove(id);
   }
}
```

# IO流

## 构造器

```java
File(File parent, String child) 
          根据 parent 抽象路径名和 child 路径名字符串创建一个新 File 实例。 文件路径为父路径
File(String pathname) 
          通过将给定路径名字符串转换为抽象路径名来创建一个新 File 实例。 绝对路径。
File(String parent, String child) 
          根据 parent 路径名字符串和 child 路径名字符串创建一个新 File 实例。 直接给父路径
```

## 方法

### 获取与判断



![image-20201009202952702](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201009202952702.png)

![image-20201009203021571](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201009203021571.png)

```java
@Test
public void test()
{
    //1.
    File file = new File("hello.txt");//相对于当前的module的路径
    File file_ = new File("D:\\hi.txt");
    //2.
    File file1 = new File("D:\\", "hi");//这里指定了一个文件夹，在D:\\下创建

    //3.
    File file2 = new File(file1, "hi.txt");//在file1路径下的一个hi.txt文件
    
	//返回此抽象路径名的绝对路径名形式。
    System.out.println(file1.getAbsoluteFile());
    
    //将此抽象路径名转换为一个路径名字符串。
    System.out.println(file1.getPath());
    
    //返回由此抽象路径名表示的文件或目录的名称。
    System.out.println(file1.getName());
    
    //返回此抽象路径名父目录的路径名字符串；如果此路径名没有指定父目录，则返回 null。
    System.out.println(file1.getParent());
    
	//返回此抽象路径名表示的文件最后一次被修改的时间
    System.out.println(new Date(file1.lastModified()));
    
    //返回由此抽象路径名表示的文件的长度。
    System.out.println(file1.length());
    System.out.println(file2.length());
    System.out.println(file_.length());

    //返回一个字符串数组，这些字符串指定此抽象路径名表示的目录中的文件和目录。file1路径下所有的文件和目录
    String[] list = file1.list();
    System.out.println(Arrays.toString(list));
    
    //返回一个抽象路径名数组，这些路径名表示此抽象路径名表示的目录中的文件。file1下所有文件的路径
    File[] files = file1.listFiles();
    System.out.println(Arrays.toString(files));
    
    //判断是否是目录
    System.out.println(file1.isDirectory());
    
    //判断是否是文件
    System.out.println(file_.isFile());
    
    //判断是否存在
    System.out.println(file.exists());
    System.out.println(file1.exists());
    
    //判断是否可读
    System.out.println(file1.canRead());
    
    //判断是否可写
    System.out.println(file1.canWrite());
    
    //判断是否隐藏
    System.out.println(file1.isHidden());

}
D:\hi
D:\hi
hi
D:\
Fri Oct 09 20:15:30 CST 2020
0
4
10
[hi.txt, 新建文件夹, 新建文件夹 (2), 新建文件夹 (3)]
[D:\hi\hi.txt, D:\hi\新建文件夹, D:\hi\新建文件夹 (2), D:\hi\新建文件夹 (3)]
    
true
true
false
true
true
true
false

```

```java
public boolean renameTo(File dest);    
//file1.renameTo(file2) file1在硬盘中是存在的，且file2不能在硬盘中存在， 返回true
```

### 创建

```java
    @Test
    public void test5() throws IOException {
        //文件创建
        File file = new File("hi.txt");
        if (!file.exists())
        {
            file.createNewFile();
            System.out.println("successed");
        }
        else
        {
            System.out.println("the file is present, will delete it");
            file.delete();
            System.out.println("delete successed");
        }
        
        
        //文件目录创建
        File file1 = new File("D:\\io\\ioo");
        //单层目录创建，创建此抽象路径名指定的目录。不包括父目录。
        if (file1.mkdir());

        //多层目录创建，创建此抽象路径名指定的目录，包括所有必需但不存在的父目录。
        if (file1.mkdirs())
            System.out.println("successed");
    }
```

## 有意思的练习

1. 求目录下所有文件打印出数量

```java
/**
         * @description 不使用递归的方法调用
         * @return java.util.List<java.io.File>
         * @author https://blog.csdn.net/chen_2890
         * @date 2019/6/14 17:34
         * @version V1.0
         */
public static List<File> traverseFolder1(File file) {
    List<File> fileList = new ArrayList<>();
    int fileNum = 0, folderNum = 0;
    if (file.exists()) {
        LinkedList<File> list = new LinkedList<File>();
        File[] files = file.listFiles();
        for (File file2 : files) {
            if (file2.isDirectory()) {
                System.out.println("文件夹:" + file2.getAbsolutePath());
                list.add(file2);
                folderNum++;
            } else {
                fileList.add(file2);
                System.out.println("文件:" + file2.getAbsolutePath());
                fileNum++;
            }
        }
        File temp_file;
        while (!list.isEmpty()) {
            temp_file = list.removeFirst();
            files = temp_file.listFiles();
            for (File file2 : files) {
                if (file2.isDirectory()) {
                    System.out.println("文件夹:" + file2.getAbsolutePath());
                    list.add(file2);
                    folderNum++;
                } else {
                    fileList.add(file2);
                    System.out.println("文件:" + file2.getAbsolutePath());
                    fileNum++;
                }
            }
        }
    } else {
        System.out.println("文件不存在!");
    }
    System.out.println("文件夹共有:" + folderNum + ",文件共有:" + fileNum);
    return fileList;
}
/**
         * @description 使用递归的方法调用
         * @return java.util.List<java.io.File>
         * @author https://blog.csdn.net/chen_2890
         * @date 2019/6/14 17:35
         * @version V1.0
         */
public static List<File> traverseFolder2(File file)
{
    List<File> fileList = new ArrayList<>();
    if (file.exists())
    {
        File[] files = file.listFiles();
        if (null == files || files.length == 0)
        {
            System.out.println("文件夹是空的!");
            return null;
        }
        else
        {
            for (File file2 : files)
            {
                if (file2.isDirectory())
                {
                    System.out.println("文件夹:" + file2.getAbsolutePath());
                    traverseFolder2(file2.getAbsoluteFile());
                }
                else
                {
                    fileList.add(file2);
                    System.out.println("文件:" + file2.getAbsolutePath());
                }
            }
        }
    }
    else
    {
        System.out.println("文件不存在!");
    }
    return fileList;
}
```

## 流的分类

1. 操作数据单位：字节流、字符流
2. 数据的流向：输入流、输出流
3. 流的角色：节点流、处理流

> 流的输入与输出都是相对于内存而言的，我们站在内存的角度去看，是输入还是输出。

计算机对字节敏感，人对字符敏感。

解码：字节、字节数组--->字符数组、字符串
编码：字符数组、字符串--->字节、字节数组

**一切都是字节流，其实没有字符流这个东西。字符只是根据编码集对字节流翻译之后的产物。**

## 流的体系结构

![image-20201013110318003](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201013110318003.png)

| 抽象基类     | 节点流(或文件流) | 缓冲流(处理的一种)   |
| ------------ | ---------------- | -------------------- |
| InputStream  | FileInputStream  | BufferedInputStream  |
| OutputStream | FileOutputStream | BufferedOutputStream |
| Reader       | FileReader       | BufferedReader       |
| Writer       | FileWriter       | BufferedWriter       |

### 读入

read()：返回读入的一个字符，如果达到文件末尾，返回-1。为了保证流资源一定可以关闭，用try-catch-finally处理。保证文件是可搜索到的，要不然会抛出`FileNotFoundException`异常。

```java
public void test(){
    File file = new File("hello1.txt");
    FileReader fileReader = null;
    try {
        fileReader = new FileReader(file);
        int read;
        while ((read = fileReader.read()) != -1) {
            System.out.print((char) read);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if (fileReader != null)
                fileReader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 升级版

> 一次读一个实在是太少了，我们可以一次读多个，放在数组中。

```java
    @Test
    public void test1(){
        File file = new File("hello.txt");

        FileReader fr = null;
        try {
            fr = new FileReader(file);
            char[] c = new char[1000];
            int len;//每次取出的个数
            long firstTime = System.currentTimeMillis();
            while ((len = fr.read(c)) != -1)
            {
//                for (int i = 0; i < len; i++)//注意循环终止条件
//                    System.out.print(c[i]);
                String str = new String(c, 0, len);//左闭右开的取，长度是len，但是下标只取到len - 1
                System.out.print(str);
            }
            long lastTime = System.currentTimeMillis();
            System.out.println(lastTime - firstTime);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                fr.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
//只需要1毫秒，上面在同等情况下要18毫秒
```



> 字符流：文本文件
>
> 字节流：非文本文件（图片，视频，音频，源文件），如果只是物理内存上的拷贝，字节流都可以做的到，不会乱码。

## 缓冲流

BufferedInputStream

BufferedOutputStream

BufferedReader

BufferedWriter

```java
@Test
public void test(){
    File file = new File("D:\\test\\suo.jpg");
    File copyFile = new File(file.getParent(), "a.jpg");

    BufferedInputStream bis = null;
    BufferedOutputStream bos = null;
    try{
        FileInputStream fis = new FileInputStream(file);
        FileOutputStream fos = new FileOutputStream(copyFile);

        bis = new BufferedInputStream(fis);
        bos = new BufferedOutputStream(fos);

        byte[] b = new byte[10];
        int length;
        while ((length = bis.read(b)) != -1)
        {
            bos.write(b, 0, length);
            bos.flush();//刷新缓冲区
        }
    }catch (IOException e) {
        e.printStackTrace();
    }finally {
        try {
            if (bos != null)
                bos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            if (bis != null)
                bis.close();//只用关闭这个就可以，它会帮你关闭fis流
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

可以理解为排水的水渠变大了。Buffered内部提供了一个8192缓冲区。

## 转换流

> 转换流：属于字符流
> InputStreamReader：将一个字节的输入流转换为字符的输入流，字节流读取，相对于外部设备
> OutputStreamWriter：将一个字符输出流转换为字节的输出流，字节流读出， 相对于外部设备

为什么属于字符流，我想是最后相对内存而言的是字符吧。

![image-20201013092736561](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201013092736561.png)

```java
@Test
public void test1(){
    FileInputStream fis = null;
    InputStreamReader isr = null;
    try {
        fis = new FileInputStream("hello.txt");
        isr = new InputStreamReader(fis, "gbk");//接收一个字符流读入

        char[] ch = new char[2];
        int length;
        while ((length = isr.read(ch)) != -1) {
            String s = new String(ch, 0, length);
            System.out.print(s);
        }
    }catch(IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if (isr != null)
                isr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}
```

```java
@Test
public void test2(){
    //输入与与输出
    FileInputStream fis = null;
    FileOutputStream fos = null;
    //转换流
    InputStreamReader isr = null;
    OutputStreamWriter osw = null;
    try {
        fis = new FileInputStream("hello.txt");
        fos = new FileOutputStream("hello2.txt");

        isr = new InputStreamReader(fis, "utf-8");
        osw = new OutputStreamWriter(fos, "gbk");

        char[] ch = new char[2];
        int length;
        while ((length = isr.read(ch)) != -1) {
            osw.write(ch, 0, length);
        }
    }catch(IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if (isr != null)
                isr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            if (osw != null)
                osw.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

## 对象流

>  ObjectInputStream
>  ObjectOutputStream
>  序列化：用ObjectOutputStream类保存基本类型数据或对象的机制
>  反序列化：用ObjectInputStream类读取基本类型数据或对象的机制
>  对象的序列化机制：允许把内存中的Java对象转换成平台无关的二进制流，从而允许
>  把这种二进制流持久的保存在磁盘上，或通过网络将这种二进制流伟输到另一个网络节
>  点。
>  当其它程序获取了这种二进制流，就可以恢复原来的Java对象

```java
public class O {
    /*
    序列化过程：将内存中的java对象保存到磁盘中或通过网络传输出去。
     */
    public static void main(String[] args) {
        //序列化
        ObjectOutputStream o = null;

        try {
            o = new ObjectOutputStream(new FileOutputStream("Object.dat"));
            o.writeObject(new String("我爱北京天安门"));

            o.flush();

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (o != null)
                    o.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        //反序列化
        ObjectInputStream i = null;
        try {
            i = new ObjectInputStream(new FileInputStream("object.dat"));
            Object obj = i.readObject();
            System.out.println(obj);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            try {
                if (i != null)
                    i.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## 自定义

```java
public class Person implements Serializable {
    public static final long serialVersionUID = 23242442L;
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

class out {
    public static void main(String[] args) {
        ObjectOutputStream oos = null;
        try {
            oos = new ObjectOutputStream(new FileOutputStream("Object.dat"));
            oos.writeObject(new Person("q", 23));
            oos.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (oos != null)
                    oos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        ObjectInputStream ois = null;
        try {
            ois = new ObjectInputStream(new FileInputStream("Object.dat"));
            Object object = ois.readObject();
            System.out.println(object);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            try {
                if (ois != null)
                    ois.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```

# 反射

> 获取Class实例
>
> 创建运行时对象
>
> 调用运行时类的指定结构

Reflection：执行期获得任何类的内部信息
框架 = 反射 + 注解 + 设计模式
1. 通过直接new的方式或反射的方式都可以调用公共结构，开发中到底用哪一个？
建议用直接new的方式

2. 反射机制与面向对象的封装性是不是矛盾的？如何看待两个技术
不矛盾。
java.lang.Class类

## 类的加载过程：

> javac.exe->字节码文件->jave.exe对某个字节码文件进行解释运行。
> 相当于将某个字节码文件加载到内存中，此过程就称为类的加载。
> 加载到内存中的类，就是运行时类，就作为Class的一个实例。

2. Class的实例就对应着一个运行时类
2. 加载到内存中的运行时类，会缓存一定的时间，在此时间内，我们可以通过不同的方式来
    获取此运行时类。

Class 实例对应着加载到内存中的一个运行时类。

```java
@Test
public void test1() throws ClassNotFoundException {
    //方式一：调用运行时类的属性：.class
    Class<Person> clazzs = Person.class;
    System.out.println(clazzs);

    //方式二：通过运行时类的对象：.getClass()
    Person person = new Person();
    Class<? extends Person> aClass = person.getClass();
    System.out.println(aClass);
    //方式三：调用Class的静态方法：forName(String classPath)更好的体现动态性
    Class clazz3 = Class.forName("io.github.zhengjj.Person");
    System.out.println(clazz3);

    System.out.println(clazz3 == clazzs);
    System.out.println(clazzs == aClass);
    //方式四：使用类的加载器：ClassLoader(了解)
    ClassLoader classLoader = Reflection.class.getClassLoader();
    Class clazz4 = classLoader.loadClass("io.github.zhengjj.Person");
    System.out.println(clazz4);

}
class io.github.zhengjj.Person
class io.github.zhengjj.Person
class io.github.zhengjj.Person
true
true
class io.github.zhengjj.Person
true
```

## 使用ClassLoader加载器配置文件

```java
public void test20() throws IOException {
    Properties properties = new Properties();
    FileInputStream fis = new FileInputStream("src\\jdbc.properties");
    try {
        properties.load(fis);
    } catch (IOException e) {
        e.printStackTrace();
    }
    String name = properties.getProperty("name");
    String password = properties.getProperty("password");
    System.out.println(name + password);

    ClassLoader classLoader = Reflection.class.getClassLoader();
    InputStream is = classLoader.getResourceAsStream("jdbc.properties");//默认是在src下
    properties.load(is);
    //void load(Reader reader)按简单的面向行的格式从输入字符流中读取属性列表（键和元素对）。
    System.out.println(properties.getProperty("name") + properties.getProperty("password"));

}
```

## 创建运行时类的对象

```java
public void test() throws IllegalAccessException, InstantiationException {
    Class clazz = Person.class;

    /*
    newInstance()：调用此方法，创建对应的运行时类的对象，调用了空参构造器
    1. 运行时类必须提供空参构造器
    2. 空参的构造器的访问权限要够
    在javabean中要求提供一个public的空参构造器
    1. 反射时创建空参类的对象
    2. 便于子类继承此运行时类，默认调用super()时，保证父类有此构造器

     */
    Object object = clazz.newInstance();
    System.out.println(object);

}
```

## 反射的动态性

```java
@Test
public void test1(){
    String classPath = "";
    for (int i = 0; i < 100; i++) {
        int i1 = new Random().nextInt(3);
        switch (i1)
        {
            case 0:
                classPath = "java.lang.String";
                break;
            case 1:
                classPath = "java.util.Date";
                break;
            case 2:
                classPath = "io.github.zhengjj.Reflection";
                break;
        }
        try {
            System.out.println(getInstance(classPath));
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
/*
创建一个指定类的对象
classPath：指定类的全类名
 */
public Object getInstance(String classPath) throws ClassNotFoundException {
    Class clazz = Class.forName(classPath);
    return clazz;
}
```

## 获取运行时类的方法

```java
public class Reflection {
    public static void main(String[] args) throws Exception{

        Class clazz = Person.class;
        //创建运行时类的对象
        Person p = (Person) clazz.getDeclaredConstructor().newInstance();
    /*
    1. 获取指定的某个方法
    getDeclareMethod()：参数1：指明获取的方法的名称：参数2：指明获取方法的形参列表
    */
        Method show = clazz.getDeclaredMethod("show");
        Field name = clazz.getDeclaredField("name");
        name.setAccessible(true);
        name.set(p, "Tom");
        System.out.println((String)name.get(p));
        Field age = clazz.getDeclaredField("age");
        age.setAccessible(true);
        age.set(p, 14);
        int object = (int)age.get(p);
        System.out.println(object);
    /*
    保证方法是可访问的
    */
        show.setAccessible(true);
    /*
    3. 调用方法的invoke()：参数1：方法的调用者 参数2：给方法形参赋值的实参invoke()的返回值取为对应类中调用的方法 的返回值。
    */
        Object returnValue = show.invoke(p);
        System.out.println(returnValue);
    }
   
    //静态方法
    Method showDesc = clazz.getDeclaredMethod("showDesc");
    show.setAccessiable(true);
    Object returnvalue = showDesc.invoke(Person.class);//写成null也可以
    System.out.println(returnVal);
}
```

## 静态代理

> 在B类中创建A类对象，在B类中创建方法fb，方法内部是a调用A类方法，使用时是B对象调用其自身方法。

```java
interface ClothFactory{
    void produceCloth();
}
//代理类
class ProxyClothFactory implements ClothFactory{
    private ClothFactory factory;
    public ProxyClothFactory(ClothFactory factory)
    {
        this.factory = factory;
    }

    @Override
    public void produceCloth() {
        System.out.println("代理工厂做一些准备工作");
        factory.produceCloth();
        System.out.println("代理工厂做一些后续的收性工作");
    }
}
//被代理类
class NikeClothFactory implements ClothFactory{

    @Override
    public void produceCloth() {
        System.out.println("Nike 工厂生产一批运动服");
    }
}
public class Agency {
    public static void main(String[] args) {
        ClothFactory nikeClothFactory = new NikeClothFactory();
        //创建代理类的对象
        ProxyClothFactory proxyClothFactory = new ProxyClothFactory(nikeClothFactory);

        proxyClothFactory.produceCloth();
    }
}
```



## 动态代理

```java
interface Human{
    String getBelief();
    void eat(String food);
}

class ProxyFactory{
    //返回一个代理类对象
    public static Object getProxyInstance(Object obj){//obj是被代理类的对象
        MyInvocationHandler handler = new MyInvocationHandler();
        handler.bind(obj);
        return Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(), handler);
    }
}
class MyInvocationHandler implements InvocationHandler{
    private Object object;//赋值时，也需要使用被代理类的对象进行赋值

    public void bind(Object object){
        this.object = object;
    }
    //当通过代理类的对象调用方法a时，就会自动的调用如下的方法：invoke()
    //将被代理类要执行的方法a的功能声明在invoke()中
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //method：即为代理类对象调用的方法，此方法也就是为了被代理类对象要调用的方法
        //obj：被代理类的对象
        Object args1 = method.invoke(object, args);
        return args1;
    }
}

//被代理类
class SuperMan implements Human{

    @Override
    public String getBelief() {
        return "I believe I can fly";
    }

    @Override
    public void eat(String food) {
        System.out.println("I love the " + food);
    }
}
public class Agency {
    public static void main(String[] args) {
        SuperMan superMan = new SuperMan();
        Human proxyInstance = (Human) ProxyFactory.getProxyInstance(superMan);
        String belief = proxyInstance.getBelief();
        System.out.println(belief);
        proxyInstance.eat("banana");
    }
}
```



# 练习

## 第二章

### 1. 创建一个类，它包含一个`int`域和一个`char`域，它们都没有被初始化，将它们的值打印出来，以验证java执行了默认初始化。

```java
public class A {
    int i;
    char ch;
}
class Out{
    public static void main(String[] args) {
        A a = new A();
        System.out.println("int: " + a.i);//int: 0
        System.out.println("char: " + a.ch);//char: 这里有一个格
    }
}

```

### 2. 参照本章的HelloDate.java这个例子，创建一个"Hello, World"程序，该程序只要输出这句话即可。你所编写的类里只需一个方法（即"main"方法，在程序启动时被执行）记住要把它设为static形式，并指定参数列表——即使根本不会用到这个列表。

```java
public class HelloDate {
    public static void main(String[] args) {
        System.out.println("Hello, world");
    }
}
```

### 5. 将`DataOnly`中的数据在main()方法中赋值并打印出来。

```java
public class DataOnly {
    int i;
    double d;
    boolean b;
}
class Out
{
    public static void main(String[] args) {
        DataOnly data = new DataOnly();
        data.i = 47;
        data.d = 1.1;
        data.b = false;
        System.out.println("data.i = " + data.i + " data.d = " + data.d + " data.b = " + data.b);
        //data.i = 47 data.d = 1.1 data.b = false
    }
}

```

### 8. 编写一个程序，展示无论你创建了某个特定类的多少个对象，这个类中的某个特定的static域只有一个实例。

```java
public class IsStatic {
    static int a = 10;
}
class Out
{
    public static void main(String[] args) {
        IsStatic a1 = new IsStatic();
        System.out.println(IsStatic.a);//10
        IsStatic a2 = new IsStatic();
        System.out.println(IsStatic.a);//10
        IsStatic a3 = new IsStatic();
        System.out.println(IsStatic.a);//10
    }
}

```

### 9.编写一个程序，展示自动包装功能对所有的基本类型和包装器类型起作用

```java
public class A {
}
class Out
{
    public static void main(String[] args) {
        ArrayList<Integer> list1 = new ArrayList<Integer>();
        ArrayList<Character> list2 = new ArrayList<>();
        var list3 = new ArrayList<Byte>();
        var list4 = new ArrayList<A>();
    }
}
```

## 第四章

### 4. 用两个for和取余操作符%来探测和打印素数。

```java
public void prime() {
    for (int i = 2; i <= 100; i++) {
        boolean flag = true;
        for (int j = 2; j <= Math.sqrt(i); j++) {
            if (i % j == 0) {
                flag = false;
                break;
            }
            if (flag)
                System.out.println(i);
        }
    }
}
```



```java
import static java.lang.StrictMath.sqrt;

public class prime {
    public static void main(String[] args) {
        System.out.print("3");
        for (int i = 2; i <= 100; i++)
        {
            int k = (int)sqrt(i);
            for (int j = 2; j <= k; j++)
            {
                if (i % j == 0)
                    break;
                if (j == k)
                    System.out.print(" " + i);
            }
        }
    }
}
```

### 5. 用三元操作符和按位操作符来显示二进制0和1.

> 判断一个位上是0还是1很简单，就是与1或操作，如果没变就是1，变了就是0

```java
public class Out {
    public static void compare(int c)
    {
        int d = 0x80;
        for (int i = 0; i < 8; i++) {
            int out = (d | c) == c ? 1 : 0;
            System.out.print(out);
            d >>>= 1;
        }
        System.out.println();
    }
    public static void main(String[] args) {
        int a = 0xaa;
        int b = 0x55;
        System.out.println("& " + Integer.toBinaryString(a & b));
        System.out.println("| " + Integer.toBinaryString(a | b));
        System.out.println("^ " + Integer.toBinaryString(a ^ b));
        System.out.println("~a " + Integer.toBinaryString(~a));
        System.out.println("~b " + Integer.toBinaryString(~b));
        int c = a & b;
        compare(c);
        c = a | b;
        compare(c);
        c = a ^ b;
        compare(c);
        c = ~a;
        compare(c);
        c = ~b;
        compare(c);
    }
}
& 0
| 11111111
^ 11111111
~a 11111111111111111111111101010101
~b 11111111111111111111111110101010
00000000
11111111
11111111
01010101
10101010
```

## 第5章

### 9. 编写具有两个构造器的类，并在第一个构造器中通过this调用第二个构造器

```java
public class A {
    String name;

    public A(String name) {
        this.name = name;
    }
    public A() {
        this("a");
    }
}
class Out {
    public static void main(String[] args) {
        A a = new A();
        System.out.println(a.name);
    }
}

```

this不用加点。

### 10. 编写具有finalize（）方法的类，并在方法中打印消息。在main（）中为该类创建一个对象。试解释这个程序的行为。

大多数时候不需要编写finalize方法，因为`jvm`垃圾回收器已经帮我们做好了一切，我们什么时候需要它呢？如果对象不是new获得的内存空间，或者调用了`naive`方法，在方法中用其他方式获得内存空间，需要finalize方法告诉编译器如何释放内存空间。

它的工作流程如下：一旦垃圾回收器准备释放对象的空间，先调用其finalize方法，在下一次垃圾回收动作发生的时候，才会真正的回收。

需要注意的是`fianlize`方法并不是析构方法，它可能永远不会被调用

```java
public class Test {
    public void finalize(){
        System.out.println("start finalize " + this);
    }
    public static void main(String[] args) {
        new Test();
        System.gc();
//        System.runFinalization();加上这句肯定被调用
    }
}
```

运行之后发现时而输出，时而没有，这就说明了finalize的不确定性。

### 17.创建一个类，它有一个接受一个String参数的构造器。在构造阶段，打印该参数。创建一个该类对象的引用数组，但是不实际去创建赋值给该数组。当运行程序时，请注意来自对该构造器的调用中的初始化消息是否打印了出来，通过创建对象赋值给引用数组。

```java
import java.util.Arrays;

public class A {
    private String s;
    A(String s)
    {
        this.s = s;
        System.out.println(s);
    }
    @Override
    public String toString() {
        return this.s;
    }

    public static void main(String[] args) {
        A[] a = new A[5];
        for (int i = 0; i < a.length; i++) {
            a[i] = new A(Integer.toString(i));//将int返回String
        }
        System.out.println(Arrays.toString(a));
    }
}
0
1
2
3
4
[0, 1, 2, 3, 4]
```

### 写一个类，它接受一个可变参数的String数组，验证你可以向该方法传递一个用逗号分隔的String列表，或是一个String[]

```java
public class Out {
    public static void main(String[] args) {
        String str = "Hello";
        A a = new A(str);
        A b = new A("fda", "lree");
        a.method(1, 2, 3);
        int [] array = new int[10];
        a.method2(array);
    }
}
class A {
    A(String ... list) {}
    public void method(int ... List) { } 
    public void method2(int[] List) { }
}
```

重要的是可变参数`String ... list`，与数组不同的是，它传递过来的 实参不用创建一个数组对象，`str`就是一个数组对象。在这里可以直接用引号括起来的值。

# 问题

## 构造器的作用是什么？使用中有哪些注意点

1. 创建对象
2. 初始化对象结构

## 类的属性（成员变量）的赋值，有几种赋值的方式，。

默认初始化，显式初始化，构造器初始化，对象方法，对象 



```java
@Test
public void test(){
    Map<String, Integer> map = new HashMap<>();
    map.put("zjjj", 213);
    map.put("smmm", 32);
    
    Set<String> key = map.keySet();
	Iterator<String> iterator = key.iterator();
    while(iterator.hasNext())
    {
        String string = iterator.next();
        System.out.println(string);
    }
    Collection<Integer> value = map.values();
    Iterator<Integer> iterator1 = value.iterator();
    while(iterator1.hasNext())
    {
        Integer i = iterator1.next();
        System.out.println(i);
    }
    
    Set<Map.Entry<String, Integer>> set = map.entrySet();
    Iterator<Map.Entry<String, Integer>> iterator2 = set.iterator();
    while(iterator2.hasNext())
    {
        Map.Entry<String, Integer> entry = iterator2.next();
        System.out.println(entry.getKey() + " = " + entry.getValue());
    }

}
```

