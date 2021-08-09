# 基础

## 基本程序设计结构

### 数据类型

|        | 类型   | 存储要求 |
| ------ | ------ | -------- |
| 整型   | int    | 4字节    |
|        | short  | 2字节    |
|        | long   | 8字节    |
|        | byte   | 1字节    |
| 浮点型 | float  | 4字节    |
|        | double | 8字节    |
| 字符型 | char   | 2字节    |

1. 长整型加上后缀L
2. 前缀加上0B或0b就可以写二进制数
3. 可以为数字字面量加下划线，只是为了好读100_000

### 变量与常量

- 声明变量

> 变量是由字母开头并由字母与数字构成的序列（java的字母范围更大，包括在某种语言中的任何Unicode字符）

- 初始化

> 声明后必须显式初始化，java中不区分声明与定义，与c/c++不同

- 常量

> java中用final指示常量



> 类常量可以使用关键字static final，在main外定义

```java
public class Constants
{
    public static void main(String[] args)
    {
        final double CM_PER_INCH = 2.54;//只能赋值一次有点像c/c++中的const，但const是java中保留的关键字
        double paperWidth = 8.5;
        double paperHeight = 11;
        System.out.println("Paper size in centimeters: "+ paperWidth * CM_PER_INCH + " by " + paperHeight * CM_PER_INCH);
    }
}
```

#### 枚举类型

可以把它当作自己新建的一个类型

```java
enum Size{ SMALL, MEDIUM, LARGE, EXTRA_LARGE};
Size s = Size.MEDIUM;//打印也只会打印MEDIUM这些在大括号里的数
```

### 运算符

> 可以把main方法标记为

```java
public static strictfp void main(String[ ] args)
```

> 那么main方法中所有的指令都将要使用严格的浮点计算。

java中不用逗号运算符，除了在 `for` 表达式中的第1部分与第3部分使用逗号分隔表达式列表

> 逗号运算符是指在C语言中，多个表达式可以用逗号分开，其中用逗号分开的表达式的值分别结算，但整个表达式的值是最后一个表达式的值。

#### 数字函数与常量

Math类中有各种各样的数学函数

```java
double x = 4;
double y = Math.sqrt(x);
System.out.println(y);//2.0
```

#### 数值类型之间的转换

> 能表示数值范围小的可以赋值给表示范围大的，而反过来却不行

```java
double a;
int b;
a = b;//不会报错，因为不会有精度损失，编译器会帮你转换
b = a;//会报错，因为可能会有精度损失，需要强制类型转换
```

注意：float可以表示的范围比long大

#### 强制类型转换

> 当范围大的给小的时，要这样做

```java
double a = 3.0D;
int c = (int) a;
```

#### 位运算符

> \>>右移
>
> <<左移
>
> \>>>右移用0填充高位

#### 标识符

- 命名规则

> 不能字母开头，不能是关键字

- 命名规范

- - 类：首字母大写，后面的每个单词首字母大写
  - 变量：首字母小写，后面的每个单词首字母大写



编译器可以对右侧的常量直接求和，

```java
short a = 5 + 3; // 不会报错
int b = 5;
int c = 3;
short a = b + c;//只有当编译器认为右侧都是常才不会报错，这样不行。
```

### 字符串

> Java库中提供了一个预定义的类，叫String，每个用双引号括起来的字符串都是String类的一个实例。

特点：

1. 字符串中的内容永远都不可以改变。
2. 可以共享使用
3. 效果上等于Char[]，底层原理是byte[]字节数组

三种构造方法

```
public String();创建一个空白字符串，不含有任何内容
public String(char[] array);根椐字符数组的内容，来创建对应的字符串
public String(byte[] array);根据字节数组的内容，来创建对应的字符串
```

如果在程序当中直接写上的双引号的字符串，就在字符串常量池（堆中）中。

```
String str = "hello";
```

常量值中的地址值相同。

![image-20200731142755668](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20200731142755668.png)

#### 子串

```java
String greeting = "Hello";
String s = greeting.substring(0, 3);
```

从0到2提取一个子串。

#### 拼接

可以用+号拼接

```java
int age = 13;
String rating = "PG" + age;
```

后面的age会自动转成字符串。

```java
String all = String.join("/ ","S", "M", "L", "XL");
```

all就是S / M / L / XL

#### 不可变字符串

> java中不可以修改字符串，但可以用上面提到的提取与添加进行操作，字符串是可以共享使用的。

#### 检测字符串是否相等

```java
s.equals(t);//如果s与t相等则返回true否则返回false，把带引号的也就是常量对象写前面，变量对象写在后面，防止s可能为NULL，会报错NullPointerException
s.equalsIgnoreCase(t);//不区分大小写
```

#### 空串与null串

> 空串是长度为0的字符串

```java
if(str.length() == 0)
if(str.equal(""))
```

> null串

```java
if(str == null)
```

#### 码点与代码单元

> Java字符串由char值序列组成。char数据类型是一个采用UTF-16编码表示Unicode码点的代码单元。最常用的Unicode字符使用一个代码单元就可以表示，而辅助字符需要一对代码单元表示。

#### 构建字符串

```java
StringBuilder builder = new StringBuilder();
builder.append(ch);
String com = builder.toString();//构建完成后调用toString方法，将可以得到一个String对象，其中包含了构建器中的字符序列
```

### 输入输出

#### 输入

> 构造一个与“标准输入流”System.in关联的Scanner对象

```java
Scanner in = new Scanner（System.in);
```

现在就可以与各种方法来读了

```java
String name = in.nextline();//句子，有空格
String firstName = in.next();//单词
String age = in.nextInt();//整数
String price = in.nextDouble();//浮点数
```

#### 格式化输出

> 与C语言一样，可以用`printf`，与C语言相同。

### 数组

#### 一维数组

##### 声明数组

```java
int[] a;
int[] a = new int[100];//声明并初始化了一个一个可以存储100个整数的数组
```

创建对象并同时初始化

```java
int[] smallPrimes = { 2, 3, 5, 7, 11, 13,};//简写
smallPrimes = new int[] {17, 19, 23, 29, 31, 37};
```

Java中允许一个长度为0的数组。创建数字数组时，所有元素都初始化0，`boolean`会初始化为`false`，对象数组的元素则初始化为一个特殊值`null`

##### for each

```java
for(int c : smallPrimes)
    System.out.println(c);//打印数组smallPrimes中的每一个元素，一个元素占一行
```

### 二维数组

#### 声明

```java
int [][] array = new int[10][20];//10行20列
int [][] array = {
    {1,2,3,4,5,6,7,8,9,10},
    {2,2,3,4,5,6,7,8,9,10},
};
```

不同与C++ 中的

```c++
int array[10][20];
int (*array)[20];
```

而是C++中的

```C++
int *array[20];
```

是分配了包含10个指针的数组。然后再给每一个元素填充20个数字的数组。当`new`的时候这个循环是自动的。

求行长与列长

```java
array.length//行长
array[0].length//列长
```

与c语言不同的是，这两个的地址是不同的

![点击查看源网页](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\timg.jpg)



从图中可以看出，数组名`balance`是一包含10个元素的数组，而每个又是包含6个`float`的数组。行与列的求值是这么来的。看图，地址不同也是因为，`banlance`与`balance[0]`确实是不一样的东西。

![timg](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\timg.png)

#### 二维数组的行交换

```java
double[] temp = balances[0];
balances[0] = balances[1];
balances[1] = temp;
```

# 对象与类

## 两种思想的不同

> 面向过程：当需要实现一个功能的时候，每一个具体的步骤都要亲力亲为，详细处理每一个细节。
> 面向对象：当需要实现一个功能的时候，不关心具体的步骤，而是找一个已经具有该功能的人，来帮我做事。
>
> 面向过程：把衣服脱下来->找一个盆->放点洗衣粉->加点水->浸泡10分钟->揉->清洗衣服->拧干->晾起来，更强调方法
> 面向对象：脱衣服->打开洗衣机->放衣服->晾起来，强调对象，这时的对象就是洗衣机

### 特点

面向对象思想是一种更符合我们思考习惯的思想，它可以将复杂的事性简单化，并将我们从执行者变成了指挥者。
面向对象的语言中，包含了三大基本特征，封装、继承和多态

## 类和对象的不同

>类和对象
>类：是一组相关属性和行为的集合，可以看是一类事物的模板，使用事物的属性特征和行为特征来描述该类事物。
>现实中，描述一类事物
* 属性：名字，体重，年龄，颜色
* 行走：走，跑，叫

### 特点

> 类：是对一类事物的描述：是抽象的
> 对象：是一类事物的实例，是具体的，是一类事物的具体体现，对象是类的一个实例，必然具备该类事物的属性和行为。
> 类是对象的模板，对象是类的实体。

### 类的定义：

> 成员变量：就是事物的属性
> 成员方法：就是事物能做什么

## 使用类

1. 导包：也就是指出需要使用的类，在什么位置
   import 包名称.类名称

   ```java
   import in.itcast.day01.student.Student;
   ```

   对于和当前类属于同一个包的情况，可以省略导包语句不写。

2. 创建，格式
   类名称 对象名 = new 类名称();

   ```java
   Student stu = new Student();
   ```

3. 使用，分为两种情况
   使用成员变量：对象名. 成员变量名
   使用成员方法：对象名.成员方法名（参数）

### 对象与对象变量

* 对象的三个特性：

  * 行为：可以对对象完成哪些操作，应用哪些方法
  * 状态：调用那些方法时，对象会如何响应？
  * 标识：如何区分具有相同行为与状态的不同对象？


> 要用对象，就要先构造对象，并指定初始化状态，然后应用方法。
>
> 在Java中，要使用构造器（constructor，构造函数）构造新实例。

构造器：是一个特殊的方法 ，用来构造并初始化对象。

> 构造器的名字应该与类名相同，Date类的构造器名为Date，如果要构造一个Date对象，需要在构造器前面加一个new

```java
Date birthday = new Date();
System.out.println(new Date());
String s = new Date().toString();//toString是Date类中一个方法返回日期的字符串描述
```

在`java`中对象变量并没有实际包含一个对象，**任何对象变量的值都是对存储在另外一个地方的某个对象的引用**，`new`操作符的返回值也是一个引用。

```java
package in.itcast.day01.constructor;
 
import java.time.DayOfWeek;
import java.time.LocalDate;

public class Constructor {
    public static void main(String[] args)
    {
        LocalDate date = LocalDate.now();//获得当前时间
        int month = date.getMonthValue();//获得当前月份
        int today = date.getDayOfMonth();//获得当前日期
        date = date.minusDays(today - 1);//结果应该是date- (today - 1), 得到的是这个月一号
        
        DayOfWeek weekday = date.getDayOfWeek();//得到当前日期是星期几
        int value = weekday.getValue();//getValue方法得到一个数值，与星期对应

        System.out.println("Mon Tue Wed Thu Fri Sat Sun");
        for (int i = 1; i < value; i++)//日期缩进
            System.out.printf("    ");

        while (date.getMonthValue() == month)//月份相同
        {
            System.out.printf("%3d", date.getDayOfMonth());//打印日
            if (date.getDayOfMonth() == today)//如果就是今天，那打印一个*
            {
                System.out.print("*");
            }
            else//否则打一个空
            {
                System.out.print(" ");
            }
            date = date.plusDays(1);//日期加1
            if (date.getDayOfWeek().getValue() == 1)//如果是周一就换行
                System.out.println();
        }
        if (date.getDayOfWeek().getValue() != 1)//如果不是周1就换行
            System.out.println();
    }
}

```

有些对象要用到静态工厂方法：

```java
LocalDate date = LocalDate.now();//获得当前时间
```

比如上面的`LocalDate`类的对象。

### 更改器与访问器方法

1. 只访问对象而不修改方法对象的方法称为访问器方法（accessor method)`LocalDate.getYear`就是访问器方法。
2. 可以改变对象的状态的就是更改器方法。

## 自己的类

```java
public class Employee 
{
    private String name;//三个实例字段
    private double salary;
    private LocalDate hireDay;

    public Employee(String name, double salary, int year, int month, int day)//构造器
    {
        this.name = name;
        this.salary = salary;
        this.hireDay = LocalDate.of(year, month, day);
    }

    public String getName()//方法1
    {
        return name;
    }
    public double getSalary()//方法2
    {
        return salary;
    }
    public LocalDate getHireDay()//方法3
    {
        return hireDay;
    }
    public void raiseSalary(double byPercent)//方法4
    {
        double raise = salary * byPercent / 100;
        salary += raise;
    }
    
}

```

### 实例字段与构造器

* 这个类的所有方法都被标记为`public`意味着任何类的任何方法都可以调用这些方法(共有4种访问级别)。而`private`确保只有`Employee`类自身的方法能够访问这些实例字段。

> 也可以看出类包含的实例字段本身也有可能是字段。name字段是String对象，`hireDay`字段是`LocalDate`类对象。

**类包含的实例字段通常属于某个类类型**

#### final实例字段

```java
class Employee
{
    private final String name;
    ...
}
```

`name`对象不可以被改变了。

如果对可变的类用`final`可能会造成混乱。表示其引用不会再指示另一个不同的对象了。对象还是可以通过引用来更改。

有点像C++中的

```c++
int * const p;
```



* 构造器
  * 构造器与类同名
  * 每个类可以有一个以上的构造器
  * 构造器可以有0，1，或者多个参数
  * 构造器没有返回值
  * 构造器总是伴随着new操作符一起调用

* 如果可以从变量的初始值推导出它们的类型，那么可以用`var`关键字声明局部变量，而无须指定类型。

```java
Employee harry = new Employee("Harry Hacker", 50000, 1989, 10, 1);
var harry = new Employee("Harry Hacker", 50000, 1989, 10, 1);
```

**这种方法只能用于方法中的局部变量，参数和字段的类型必须声明** 方法 

#### 隐式参数与显式参数

```java
public void raiseSalary(double byPercent)
{
    double raise = salary * byPercent / 100;
    salary += raise;
}
num.raiseSalary(5);
// 这个调用将执行下列指令
double raise = num.salary*5 / 100;
num.salary + = raise;
```

`raiseSalary`有两个参数，

1. 第一个称为**隐式(implicit)**参数，就是在方法名前面的类类型对象（Employee)，可以用关键字`this`指示隐式参数。
2. 第二个位于方法名后面的数值，就是**显示(explicit)**参数。

把隐式参数称为方法调用的**目标**或**接收者**。

#### 方法参数

1. 按值调用：(call by value)：接收的是调用者提供的值。
2. 按引用调用（call by reference)：接收的是调用者提供的变量地址

以上两种是程序设计语言中关于将参数传递给方法的一些专业术语。

> Java程序设计语言总是采用按值调用，方法得到的是所有参数值的一个副本。

但是实现一个改变对象状态的方法是可以的，这里方法得到的是对象引用的副本，原来的对象和这个副本都引用同一个对象。我们可以通过引用对对象进行修改，对象更改后原来的引用还是引用这个对象的，是这样改变的。

但是它不是按引用调用，这种还是**按值传递**的

```java
public static void swap(Employee x, Employee y)
{
    Employee temp = x;
    x = y;
    y = temp;
}
```

如果是按引用调用，那么是可以交换的，只是交换了`x`和`y`副本。用完后两者被丢弃了，最后的`a`和`b`仍然引用之前的对象。

Java方法通过对象引用的副本来修改所引用对象的状态。

* 方法不能修改基本数据类型的参数
* 方法可以改变对象参数的状态
* 方法不能让一个对象参数引用一个新的对象

### 静态字段与静态方法

#### 静态字段

> 在类型名称前加一个static，那么每一个类都只有一个这样的字段，而对于非静态的实例字段，每个对象都有自己的副本

每一个对象都有一个自己的非静态字段，而静态字段属于一个类。无论有没有对象，它都在那里。

```java
class Employee
{
    private static int nextId = 1;
    private int id;
    ...
}
```

#### 静态常量

```java
public class Math
{
    ...
    public static final double PI = 3.1414926;
    ...
}
```

可以通过`Math.PI`来访问这个常量，如果没有关键字，那么这个就变成了一个实例字段，要通过`Math`对象来访问`PI`，我们最常用的静态常量是`System.out`

```java
public class System
{
    ...
    public static final PrintStream out = ...;
    ...
}
```

这里`out`被声明为一个公共常量字段，而不是公共字段，不要用公共字段。

#### 静态方法

> 就是不在对象上执行的方法，也就是不需要隐式参数。

* 方法不需要访问对象状态，因为它需要的所有参数都通过显式参数提供
* 方法只需要访问类的静态字段。

```java
public static int getNextId()
{
    return NextId;
}
int n = Employee.getNextId();//提供类名来调用方法
```

> 所谓静态就是属于类而不属于任何类对象的函数。

#### 静态工厂方法

* 无法命名构造器：希望有多个不同的名字分别得到不同的值。
* 使用构造器无法改变所构造对象的类型。

### 对象构造

#### 重载

> 多个方法有相同的名字不同的参数就出现了重载，由编译器来挑出具体调用哪一个方法。

> java中完整地描述一个方法，需要指定方法名以及参数类型，这就是方法的签名。返回类型不是方法的签名。

#### 默认字段初始化

> 在构造器中没有显式地为字段设置初值，那就会被自动地赋一个默认值。

#### 无参数构造器

> 如果在编写一个类时，没有写构造器，变会提供一个无参数的构造器，其将所有字段设置为默认值。

#### 显式字段初始化

可以在类定义中直接为任何字段赋值

#### 参数名

* 用单个字母， `name`用n，`salary`用s
* 前面加a ，`name`用`aName`，`salary`用`aSalary`
* 隐式参数用this

#### 调用另一个构造器

```java
public Employee(double s)
{
    this("Employee #" + nextId, s)//当new Employee(100)时，将调用Employee(String, double)构造器
    nextId++;
}
```

```java
//Employee类
public Employee(String name)
{
    this("haha" + name, 434);
}
public Employee(String name, double salary)
{
    this.name = name;
    this.salary = salary;
}
public String getName()
{
    return name;
}
//主函数
Employee b = new Employee("fd");
System.out.println(b.getName());//hahafd
```



## 封装

> **封装encapsulation**，有时称为数据隐藏，是处理对象的一个重要概念。
>
> 就是将数据和行为组合在一个包中，并对对象的使用都隐藏具体的实现方式。

对象中的数据称为**实例字段(instance field)**，操作数据的过程称为**方法(method)**。

对象作为一个类的实例，特定对象都有一组特定的实例字段值，而这些值的集合就是这个对象当前的**状态(state)**。

## 继承 

通过扩展一个类的过程称为**继承(inheritance)**

#### 定义：

```java
public class Manager extends Employee
{
    added methods and fields
}
```

关键字`extend`表明正在构造的新类派生于一个已存在的类。已存在的类叫做超类（superclass)，基类(base class)，父类（parent class)，新类就叫子类(subclass)，派生类(derived class)，或者孩子类（child class）。叫做超类的原因我想应该是它可以扩展的方面更多，有更多的发展空间吧，但是究二者功能来说，子类应该是更多功能的。

也可以用**超集**与**子集** 来描述两者之间的关系，4因为经理本身也是员工，所有员工的集合包含所有经理组成的集合。

#### 覆盖方法

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

**重写则是在不同类中，且重写方法在 子类中，参数列表，方法名与返回值都要相同。在javaSE5后，覆盖重写方法可以返回基类方法返回值类型的某种导出类型**

#### 子类构造器

子类构造器调用前，必须要调用超类构造器，而且要放在第一个位置。与原先一样，编译器会默认给你一个无参构造器，但是这个构造器超类中如果没有的话就会报错。你自己也可以写一个调用父类重载构造。

```java
public Manager (String name, double salary, int year, int month, int day)
{
    super(name, salary, year, month, day);
    bonus = 0;
}
```

这里的`super`指的是调用超类中的构造器的简写。**使用`super`调用构造器的语句必须是子类构造器的第一条语句。**

#### super与this关键字

`this`有两个含义：

1. 在本类的成员方法中，访问本类的成员变量
2. 在本类的成员方法中，访问本类的成员方法
3. 在本类的构造方法中，访问本类的另一个构造方法，this也要放在第一个，不能与super同时使用

`super`也有两个含义

1. 在子类的成员方法中，访问父类的成员方法
2. 在子类的构造方法中，访问父类的构造方法
3. 在子类的成员方法中，访问父类的成员变量

但是`super`并不是一个对象的引用，不能将值赋给另一个对象变量，它只是编译器调用超类方法的特殊关键字。

#### 多态与动态绑定

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

##### 理解方法调用

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

#### 阻止继承：final类和方法

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

#### 抽象类

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



#### 受保护访问

最好将类中的字段标记为`private`，而方法标记为`public`，私有内容在其它类中是不可见的，即使子类也不能访问超类的字段。

为了解决这个问题，我们可以将这些方法与字段声明设置为受保护`protected`，在java中保护字段只能由同一个包中的类访问。 

1. 对本类可见——private
2. 对外部完全可见——public
3. 对本包和所有子类可见——protected
4. 对本包可见——默认，不需要修饰符

#### Object：所有类的超类

> Object类是Java中所有类的始祖，其中每一个类都是由Object类扩展而来的。

根据多态性，超类可以引用子类，所以可以使用Object类型的变量引用任何类型的对象。这种方法只能用于作为各种值的一个泛型容器，想要对其内容进行具体的操作还是要清楚对象的原始类型，并进行强制类型转换。

````java
Employee e = (Employee) obj;
````

在java中只有基本类型不是对象，数组都扩展了`Object`类

```java
obj = new int[10];
```

##### equals方法

> Object类中的这个方法用于检测一个对象是否等于另一个对象。

```java
public boolean equals(Object otherObject)
{
    if (this == otherObject)
        return true;
    if (otherObject == null)
        return false;
    //getClass方法用来返回一个对象所属的类，下面判断的意思是，如果它们不匹配，那就不相等
    if (getClass() != otherObject.getClass())
        return false;
    var other = (Employee) otherObject;
    return Objects.equals(name, other.name)
        && salary == other.salary
        && Objects.equals(hireDay, other.hireDay);
}
```

#### 泛型数组列表

> 泛型数组可自动地调整数组容量

##### 声明

> `ArrayList` <类型名> = new `ArrayList` <类型名>(); 后面的类型名是可选的
>
> `var` 对象名 = new` ArrayList`<类型名>();

```java
ArrayList<Emloyee> staff = new ArrayList<Employee>();
ArrayList<Emloyee> staff = new ArrayList<>();
var staff = new ArrayList<Employee>();
```

#### 对象包装器与自动装箱

> 有时要将基本类型转换为对象，所有的基本类型都有一个对应的类，称为**包装器(wrapper)**，有`Integet`、`Long`、`Float`、`Double`、`Short`、`Byte`、`Character`和`Boolean`，包装类是不可变的，一旦构造就不可以改变其中的值，不可以派生子类。

```java
var list = new ArrayList<Integer>();
```

* 自动装箱

  ```java
  list.add(3) => list.add(Inter.valueOf(3))
  ```

* 自动拆箱

  ```java
  int n = list.get(i) => list.get(i).intValue()
  ```

装箱与拆箱都是编译器要做的工作，而不是虚拟机。而显然要比较它们应该用`equals`方法

##### 参数数量可变的方法

#### 枚举类

没有参数的

```java
public enum Size{SMALL, MEDIUM, LARGE, EXTRA_LARGE}
```

有参数的，实例在前，括号中就是参数，构造函数在后面。

```java
public enum Size
{
    SMALL("s"), MEDIUM("M"), LARGE("L"). EXTRA_LARGE("XL");
    
    private String abbreviation;
    
    private Size(String abbreviation){ this.abbreviation = abbreviation; }
    public String getAbbreviation(){ return abbreviation; }
}
```

枚举的构造器总是私有的，可以省略`private`修饰符，所有枚举类都是`Enum`的子类。

