# 第5章 初始化与清理

## 5.1 用构造器确保初始化

> 通过构造器，类的设计者创建类时确保每个对象都得到初始化。

1. 调用构造器是编译器的责任
2. 构造器名与类名相同
3. 初始化时自动调用构造器
4. 不接受任何参数的构造器叫默认构造器

## 5.2 方法重载

> 构造器名只能有一个（与类名相同），但是有时可能需要初始化不同的字段，多种方式创建对象。所以就有了同名不同参的多个构造器。

```java
class SmallTree {
    SmallTree() {
        System.out.println("This is a small tree!");
    }

    SmallTree(int height) {
        this.height = height;
        System.out.println("The small tree is " + height + "!");
    }

    private int height;
}

public class Tree {
    public static void main(String[] args) {
        SmallTree smallTree = new SmallTree();
        SmallTree smallTree1 = new SmallTree(1);
    }
}
//This is a small tree!
//The small tree is 1!
```

### 5.2.1 区分重载方法

> 独一无二的参数类型列表

### 5.2.2 涉及基本类型的重载

```java
public class PrimitiveOverloading {
    static void function(char x) {
        System.out.println("char");
    }

/*
    static void function(int x) {
        System.out.println("int");
    }
*/

    static void function(float x) {
        System.out.println("float ");
    }

    public static void main(String[] args) {
        function(5);
        function('c');
        function(2.5f);
    }
}
/*
float 
char
float 
*/
```

> 基本数据类型找不到匹配的类型时会自动提升，没有可向上提升的方法形参就会报错，可以使用强制类型转换来完成窄化后消除这种错误。

### 5.2.3 以返回值区分重载方法

> 行不通

## 5.3 默认构造器

> 作用是创建一个默认对象，在你没写构造器的前提下，编译器会帮你自动创造一个构造器。

## 5.4 this关键字

> 在调用方法传参时，编译器暗自把“所操作对象的引用”作为第一个参数传给了方法，以此来区分不同对象调用方法。在这个方法内部可以通过this使用这个引用。

```java
public class Leaf {
    int i = 0;

    Leaf increment() {
        i++;
        return this;
    }

    void print() {
        System.out.println("i = " + i);
    }

    public static void main(String[] args) {
        Leaf x = new Leaf();
        x.increment().increment().increment().print();
    }
}
//i = 3
```

this在方法内部使用，但本类内部的方法调用本类的方法时不需要写this。

```java
class Apricot
{
    void pick(){}
    void pit(){
//        this.pick(); //也可以，但是没必要
        pick();
    }
}
```

### 5.4.1 在构造器中调用构造器

> 为了防止构造器相同参数初始化重复写代码，可以用一个构造器调用另一个构造器，用this可以做到这一点，但它要放在构造器方法开始，且只能使用一次，不同调用两个。

```java
public class Flower {
    private String name;
    private int age;
    Flower(String name) {
        this.name = name; //区分参数与成员变量
        System.out.println("This flower's name is " + name);
    }
    Flower(String name, int age)
    {
        this(name); //这句只能放在方法开始
        this.age = age;
        System.out.println("This flower's name is " + name + " and the age is " + age);
    }
}
class out{
    public static void main(String[] args) {
        Flower flower = new Flower("Ross", 18);
    }
}
//This flower's name is Ross
//This flower's name is Ross and the age is 18
```

### 5.4.2 static 的含义

> static 方法就是没有this的方法，没有this也就是还没创建对象，没有创建对象时static就存在着。

## 5.5 清理：终结处理和垃圾回收

> 待定。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。

## 5.6 成员初始化

> 类中的数据成员(字段)会自动初始化。
>
> 方法中需要手动初始化，要不然会报错。

### 5.6.1 指定初始化

> 定义成员变量的地方赋值初始化。

## 5.7 构造器初始化

> 在创建对象时自动调用构造器初始化。但要记住，类的成员变量先完成的是自动初始化，然后才是构造器初始化。下面例子中i作为数据成员先初始化为0，再调用构造器初始化为7。

```java
public class Counter{
    int i;
    Counter(){
        i = 7;
    }
}
```

### 5.7.1 初始化顺序

> 创建对象，初始化顺序：静态方法/域-->成员变(默认)初始化-->构造器初始化

```java
public class OrderOfInitialization {
    public static void main(String[] args) {
        Test test = new Test();
        test.f();
    }
}
class Test {
    static void sf(){
        System.out.println("a = " + a);
    }
    static int a = 3;
    int b = 3;
    Test() {
        System.out.println("b = " + b);
    }
    Window window1 = new Window(1, a);
    public void f(){
        System.out.println("f()");
    }
    Window window2 = new Window(2, a);

}
class Window{
    Window(int marker, int a){
        System.out.println("Window(" + marker + ")" + "a = " + a);
        Test.sf();
    }
}

/*
Window(1)a = 3
a = 3
Window(2)a = 3
a = 3
b = 3
f()
*/
```

### 5.7.2 静态数据的初始化

```java
package io.github.little.chapter5;

class Bowl {
    Bowl(int marker) {
        System.out.println("Bowl(" + marker + ")");
    }

    void f1(int marker) {
        System.out.println("f1(" + marker + ")");
    }
}

class Table {
    static Bowl bowl1 = new Bowl(1); //1

    Table() {
        System.out.println("Table()"); //3
        bowl2.f1(1); //4
    }

    void f2(int marker) {
        System.out.println("f2(" + marker + ")");
    }

    static Bowl bowl2 = new Bowl(2); //2
}

class Cupboard {
    Bowl bowl3 = new Bowl(3); //7、11、15
    static Bowl bowl4 = new Bowl(4); //5

    Cupboard() {
        System.out.println("Cupboard"); //8、12、16
        bowl4.f1(2); //9、13、17
    }

    void f3(int marker) {
        System.out.println("f3(" + marker + ")");
    }

    static Bowl bowl5 = new Bowl(5); //6
}

public class StaticInitialization {
    public static void main(String[] args) {
        System.out.println("Creating new Cupboard() in main"); //10
        new Cupboard(); 
        System.out.println("Creating new Cupboard() in main"); //14
        new Cupboard();
        table.f2(1); //18
        cupboard.f3(1); //19
    }

    static Table table = new Table();
    static Cupboard cupboard = new Cupboard();
}

```

1. Bowl(1)
2. Bowl(2)
3. Table()
4. f1(1)
5. Bowl(4)
6. Bowl(5)
7. Bowl(3)
8. Cupboard
9. f1(2)
10. Creating new Cupboard() in main
11. Bowl(3)
12. Cupboard
13. f1(2)
14. Creating new Cupboard() in main
15. Bowl(3)
16. Cupboard
17. f1(2)
18. f2(1)
19. f3(1)

> 静态最先，只有一次且在创建对象时开始，成员变量其次，构造器最后。方法调用时才有。

假设有一个Dog()类：

1. 首次创建类型为Dog的对象时，或者Dog类的静态方法/域被访问时，事实构造器也是静态方法。Java解释器定位查找类路径，定位Dog.class文件。
2. 载入Dog.class，此时静态初始化加载，就加载这一次。
3. 当new Dog()对象时，在堆上分配空间。
4. 这块存储空间被清0，自动将Dog对象中的所有基本类型数据设置为默认值，引用被设置成null。
5. 执行所有字段定义处的初始化。
6. 执行构造器。

### 5.7.3 显式的静态初始化

> 将多个静态初始化动作组织成一个特殊的“静态子句”（静态块。

```java
public class Spoon {
    static int a;
    static int b;
    static int c;
    static {
        a = 3;
        b = 4;
    }

    public static void main(String[] args) {
        System.out.println("a = " + a);
        System.out.println("b = " + b);
        System.out.println("c = " + c);
    }
}
/*
a = 3
b = 4
c = 0
*/
```

### 5.4.7 非静态实例初始化

> 从表象上来看就是上面的显式初始化不带static。也就是每次创建对象都会初始化。

```java
package io.github.little.chapter5;

public class Mugs {
    Mug mug1;
    Mug mug2;

    {
        mug1 = new Mug(1);
        mug2 = new Mug(2);
        System.out.println("mug1 & mug2 initialized");
    }

    Mugs() {
        System.out.println("Mugs(int)");
    }

    Mugs(int i) {
        System.out.println("Mugs(int)");
    }

    public static void main(String[] args) {
        System.out.println("Inside main()");
        new Mugs();
        System.out.println("new Mugs() completed");
        new Mugs(1);
        System.out.println("new Mugs(1) completed");
    }

}

class Mug {
    Mug(int marker) {
        System.out.println("Mug(" + marker + ")");
    }

    void f(int marker) {
        System.out.println("f(" + marker + ")");
    }
}
/*
Inside main()
Mug(1)
Mug(2)
mug1 & mug2 initialized
Mugs(int)
new Mugs() completed
Mug(1)
Mug(2)
mug1 & mug2 initialized
Mugs(int)
new Mugs(1) completed
*/
```

## 5.8 数组初始化

定义一个数组：

```java
int[] a1;
```

> 编译器不允许指定数组的大小，也就是说a1只是对未来可能创建数组的一个引用，当然为这个引用已经分配了足够的空间，数组对象还没有分配空间。

```java
int[] a1 = {1, 2, 3, 4, 5};
int[] a2 = new int[5];
```

> 与c/c++一样下标从0开始，不过越界时有异常产生。

```java
package io.github.little.chapter5;

import java.util.Arrays;
import java.util.Random;

public class ArrayNew {
    public static void main(String[] args) {
        int[] a;
        Random random = new Random(47); //47是种子
        a = new int[random.nextInt(20)]; //得到0~20中的随机数
        System.out.println("length of a = " + a.length); //打印数组长
        System.out.println(Arrays.toString(a)); //打印数组
    }
}
/*
length of a = 18
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
*/
```

如果创建了一个非基本类型的数组，那就是创建了一个引用数组。

```java
Integer[] a1 = new Integer[new Random(47).nextInt(20)];
```

可以用花括号来初始化。

```java
public class DynamicArray {
    public static void main(String[] args) {
        Other.main(new String[]{"Hello", "World"});
    }
}

class Other {
    public static void main(String[] args) {
        for (String s : args) {
            System.out.print(s + " ");
        }
    }
}
//Hello World
```

记得运行第一个main()，运行错了就没有输出了。

### 5.8.1 可变参数列表

> 所有类有一个基本类——Object，可以使用这个基本类作为参数，使用所有类型数组传递都不会报错。

```java
class A {
}

public class VarArgs {
    static void printArray(Object[] args){
        for (Object obj : args)
            System.out.print(obj + " ");
        System.out.println();
    }
//    static void printArray(Object... args) { //JavaSE 5后支持
//        for (Object obj : args)
//            System.out.print(obj + " ");
//        System.out.println();
//    }

    public static void main(String[] args) {
        printArray(new Object[]{
                new Integer(47), new Float(3.14), new Double(11.11)
        });
        printArray(new Object[]{"one", "two", "three"});
        printArray(new Object[]{new A(), new A(), new A()});
    }
}
/*
47 3.14 11.11 
one two three 
io.github.little.chapter5.A@64616ca2 io.github.little.chapter5.A@13fee20c io.github.little.chapter5.A@4e04a765 //名字和对象的地址
*/
```

Java SE5后支持了可变参数列表，参数可变。上面的`Object... args`就是语法。上面只有可变参数时可以使用两种语法来表示，而下面有非可变参数时就不能使用`Object[]`数组语法。

```java
public class OptionalTrailingArguments {
    static void f(int required, String... trailing) { //不能用(int required, String[] trailing)
        System.out.println("required: " + required + " ");
        for (String s : trailing)
            System.out.println(s + " ");
        System.out.println();
    }

    public static void main(String[] args) {
        f(1, "one");
        f(1, "one", "two");
        f(0);
    }
}
```

可变参数使重载变得复杂，要给两个重载的方法都加一个非可变参数才可以避免报错。

```java
static void f(float i, Character... args){}
static void f(char i, Character... args){}
```

## 5.9 枚举类型

> Java SE5提供了枚举，与C/C++不同Java中一切都是对象。

```java
enum Spiciness {
    NOT, MILD, MEDIUM, HOT, FLAMING
}

public class SimpleEnumUse {
    public static void main(String[] args) {
        Spiciness howHot = Spiciness.MEDIUM;
        System.out.println(howHot);
        for (Spiciness s : Spiciness.values()) { //values()方法，产生常量值数组
            System.out.println(s + ", ordinal " + s.ordinal()); //ordinal()方法表示声明顺序
        }
    }
}
/*
MEDIUM
NOT, ordinal 0
MILD, ordinal 1
MEDIUM, ordinal 2
HOT, ordinal 3
FLAMING, ordinal 4
*/
```

用于switch(howHot)是一个不错的选择。

# 第6章 访问权限控制

|              | public | protected             | （default) | private |
| ------------ | ------ | --------------------- | ---------- | ------- |
| 同一个类     | √      | √                     | √          | √       |
| 同一个包     | √      | √                     | √          | ×       |
| 不同包子类   | √      | √（可以访问初始父类） | ×          | ×       |
| 不同包非子类 | √      | ×                     | ×          | ×       |

 # 第7章 复用类

> 将原先的代码再使用。有三种方法
>
> 1. 在新类中产生现有类的对象——组合
> 2. 按现有类的类型创建新类——继承
> 3. 代理

## 7.1 组合语法

> 将对象引用置新类中，要使用A类的方法，在B类中创建A类对象，以调用A类的方法。

```java
class WaterSource {
    private String s;

    WaterSource() {
        System.out.println("WaterSource()");
        s = "Constructed";
    }

    public String toString() {
        return s;
    }
}

public class SprinklerSystem {
    private String value1, value2, value3, value4;
    private WaterSource waterSource = new WaterSource();
    private int i;
    private float f;

    public String toString() {
        return
                "value1 = " + value1 + " " +
                        "value2 = " + value2 + " " +
                        "value3 = " + value3 + " " +
                        "value4 = " + value4 + "\n" +
                        "i = " + i + " " + "f = " + f + " " + "waterSource = " + waterSource;
    }

    public static void main(String[] args) {
        SprinklerSystem sprinklerSystems = new SprinklerSystem();
        System.out.println(sprinklerSystems);
    }
}
/*
WaterSource()
value1 = null value2 = null value3 = null value4 = null
i = 0 f = 0.0 waterSource = Constructed
*/
```

`toString()`方法，当编译器需要一个String而只有一个对象时，该方法就会被调用。

可以在4个位置对类中的对象进行初始化。

```java
package io.github.little.chapter6;

class Soap {
    private String s;

    Soap() {
        System.out.println("Soap()");
        s = "Constructed";
    }

    @Override
    public String toString() {
        return s;
    }
}

public class Bath {
    //1. 在定义对象的地方初始化
    private String
            s1 = "Happy",
            s2 = "Happy",
            s3, s4;
    private Soap castille;
    private int i;
    private float toy;

    //2. 在类的构造器中初始化
    Bath() {
        System.out.println("Inside Bath()");
        s3 = "Joy";
        toy = 3.14f;
        castille = new Soap();
    }

    //4. 实例初始化
    {
        i = 47;
    }
    //3. 在使用对象前
    @Override
    public String toString() {
        if (s4 == null)
            s4 = "Joy";
        return "Bath{" +
                "s1='" + s1 + '\'' +
                ", s2='" + s2 + '\'' +
                ", s3='" + s3 + '\'' +
                ", s4='" + s4 + '\'' +
                ", i=" + i +
                ", toy=" + toy +
                ", castille=" + castille +
                '}';
    }

    public static void main(String[] args) {
        Bath b = new Bath();
        System.out.println(b);
    }
}
/*
Inside Bath()
Soap()
Bath{s1='Happy', s2='Happy', s3='Joy', s4='Joy', i=0, toy=3.14, castille=Constructed}
*/
```

##  7.2 继承语法

> 继承是所有面向对象语言不可缺少的组成部分。Java创建一个类时就已经开始在继承了，隐式地从Java标准根类Object进行继承。继承时，首先要声明”新类（子类）与旧类（基类/父类）相似“。

```java
class NewObject extends OldObject{
    
}
```

当这么做时，会自动得到基类中所有的域(非private)和方法。

```java
public class A {
    protected int k;
    private int j;

    A() {
        System.out.println("A constructor");
    }

    public void method() {
        System.out.println("A method");
    }
    @Override
    public String toString(){
        return "This is A Object";
    }
}

class B extends A{
    A a = new A();
    B() {
        System.out.println("B constructor");
    }
    public void method(){
        a.method();
    }
    public static void main(String[] args) {
        B b = new B();
        b.method();
    }
}

class C{
    public static void main(String[] args) {
        B b = new B();
        b.k = 3;
        System.out.println(b.a);
        /*
        A constructor
        A constructor
        B constructor
        This is A Object
        */
    }
}
```



```java
package io.github.little.chapter6;

class Cleanser {
    private String s = "Cleanser";

    //把a加到s后面
    public void append(String a) {
        s += a;
    }

    public void dilute() {
        append(" dilute()");
    }

    public void apply() {
        append(" apply()");
    }

    public void scrub() {
        append(" scrub()");
    }

    public String toString() {
        return s;
    }

    public static void main(String[] args) {
        Cleanser x = new Cleanser();
        x.dilute();
        x.apply();
        x.scrub();
        System.out.println(x);
    }
}

public class Detergent extends Cleanser {
    @Override
    public void scrub() {
        append(" Detergent.scrub()");
        //防止递归
        super.scrub();
    }

    public void foam() {
        append(" foam()");
    }

    public static void main(String[] args) {
        Detergent x = new Detergent();
        x.dilute();
        x.apply();
        x.scrub();
        x.foam();
        System.out.println(x);
        System.out.println("Testing base class:");
        Cleanser.main(args);
    }
}

/*
Cleanser dilute() apply() Detergent.scrub() scrub() foam()
Testing base class:
Cleanser dilute() apply() scrub()
*/ 
```

基类的方法都必须是public的，这此方法都会被子类所继承，子类也可以对这些方法的内容进行更改，这就是**重写**，与重载不同的是，重写参数列表也相同。

### 7.2.1 初始化基类

```java
package io.github.little.chapter6;

public class Cartoon extends Drawing{
    public Cartoon(){
        System.out.println("Cartoon constructor");
    }

    public static void main(String[] args) {
        Cartoon cartoon = new Cartoon();
    }
}
class Art{
    Art(){
        System.out.println("Art constructor");
    }
}
class Drawing extends Art{
    Drawing(){
        System.out.println("Drawing constructor");
    }
}
/*
Art constructor
Drawing constructor
Cartoon constructor
*/
```

基类构造器会在子类构造器之前初始化，编译器会给不带参数的子类构造器自动合成一个默认构造器。

**带参数的构造器**

```java
package io.github.little.chapter6;

public class Cartoon extends Drawing{
    public Cartoon(int i){
        super(i);
        System.out.println("Cartoon constructor" + i);
    }

    public static void main(String[] args) {
        Cartoon cartoon = new Cartoon(5);
    }
}
class Art{
    Art(int i){
        System.out.println("Art constructor" + i);
    }
}
class Drawing extends Art{
    Drawing(int i){
        super(i);
        System.out.println("Drawing constructor" + i);
    }
}

/*
Art constructor5
Drawing constructor5
Cartoon constructor5
*/
```

在子类构造器内部，可以使用`super`调用基类构造器。带参数的构造器，一定要显式的使用super。<font color = red>调用基类构造器必须是你在导出类构造器中要做的第一件事情。</font>

## 7.3 代理

在Java编程思想上如此说到：**将一个成员对象置于所要构造的类中(就像组合)，但与此同时我们在新类中暴露了该成员对象的所有方法(就像继承)。**

> 在B类中创建A类对象a，在B类方法fb中是a调用A类方法，使用时是B对象调用其自身方法fb。

```java
package io.github.little.chapter6;

public class A {
    A() {
        System.out.println("A constructor");
    }

    public void method() {
        System.out.println("A method");
    }
}

class B {
    A a = new A();
    B() {
        System.out.println("B constructor");
    }
    public void method(){
        a.method();
    }
    public static void main(String[] args) {
        B b = new B();
        b.method();
    }
}
/*
A constructor
B constructor
A method
*/
```

## 7.4 结合使用组合与继承

### 7.4.1 确保正确清理

### 7.4.2 名称屏蔽

```java
package io.github.little.chapter6;

public class Hide {
    public static void main(String[] args) {
        Bart b = new Bart();
        b.doh(1);
        b.doh('x');
        b.doh(1.0f);
        b.doh(new Milhouse());
    }
}

class Homer {
    char doh(char c) {
        System.out.println("doh(char)");
        return 'd';
    }
    float doh(float f){
        System.out.println("doh(float)");
        return 1.0f;
    }
}

class Milhouse {

}

class Bart extends Homer {
    void doh(Milhouse m) {
        System.out.println("doh(Milhouse)");
    }
}
/*
doh(float)
doh(char)
doh(float)
doh(Milhouse)
*/
```

子类Bart中引用了重载方法，而不是重写方法，这有时会令人混淆，可以使用@Override表示它是一个重写方法，如果是重载就会报错。

```java
class Bart extends Homer {
    @Override
    void doh(Milhouse m) {
        System.out.println("doh(Milhouse)");
    }
}
```

## 7.5 组合与继承之间选择

> 组合技术通常用于想在新类中使用现有类的功能而非它的接口。
>
> 继承关系通常是类与类的包含与被包含的关系。

## 7.6 protected关键字

> 就类用户而言，这是private的，但对于任何继承于此类的导出类或其他任何位于同一个包内的类来说，它却是可以访问的。(protected也提供了包内访问权限)

```java
package io.github.little.chapter6;

class Villain {
    private String name;
	//受保护的set方法，会被子类继承
    protected void set(String nm) { 
        name = nm;
    }

    Villain(String name) {
        this.name = name;
    }

    public String toString() {
        return "I'am a Villain and my name is " + name;
    }
}

public class Orc extends Villain {
    private int orcNumber;

    Orc(String name, int orcNumber) {
        super(name);
        this.orcNumber = orcNumber;
    }

    public String toString() {
        return "Orc " + orcNumber + ": " + super.toString();
    }

    public void change(String name, int orcNumber) {
        set(name);
        this.orcNumber = orcNumber;
    }

    public static void main(String[] args) {
        Orc orc = new Orc("Limburger", 12);
        System.out.println(orc);
        orc.change("Bob", 19);
        System.out.println(orc);
    }
}
/*
Orc 12: I'am a Villain and my name is Limburger
Orc 19: I'am a Villain and my name is Bob
*/
```

## 7.7 向上转型

> 子类是基类的一种类型，子类引用可以自动转换为基类引用，这就是向个转型。这种情况经常发生在调用方法传参数与定义新对象时。

### 7.7.1 为什么称为向上转型

> 传统的继承图是自上而下画的，所以子类向上转型为基类，子类继承子基类的方法还要有自己的方法，所以子类是要大于基类的，向上转型只会丢失方法。这也是编译器允许向上转型的一个原因。

### 7.7.2 再论组合与继承

> 慎用继承。

## 7.8 final关键字

> 用于数据、方法和类。

### 7.8.1 final数据

> 用一种方法告诉编译器有一块数据是恒定不变的。

1. 一个永不改变的编译时常量(基本数据类型)
2. 一个在运行时被初始化的值，而你不希望它被改变

基本数据类型被final时，其值不可以被改变，并不一定是在编译时初始化。比如下面的例子：

```java
import java.util.Random;

public class FinalData {
    static Random random = new Random(20);
    private final int i = random.nextInt(20);
    static final int j = random.nextInt(20);

    public static void main(String[] args) {
        FinalData finalData = new FinalData();
        System.out.println(finalData.i);
        System.out.println(finalData.j);
    }
}
```

随机数是运行时确定，final变量的值也是。

final修饰的对象引用不能引用别的对象，而不能使引用对象的值恒定，Java现在没有使引用对象值恒定的办法，因为类机制可以实现不改变对象的效果。

```java
import java.util.Random;

public class FinalData {
    static Random random = new Random(20);
    private final int i = random.nextInt(20);
    static final int j = random.nextInt(20);
    int a = 3;

    public static void main(String[] args) {
        final FinalData finalData = new FinalData();
        System.out.println(finalData.a);//3
        finalData.a = 4;
        System.out.println(finalData.a);//4
    }
}
```

![image-20210521203146309](https://i.loli.net/2021/05/21/LkN1qFuP5Ugcl7z.png)

**空白final**

> 可以声明final域而不给初始值，这样必须在构造器中对其初始化。

**final参数**

> 传参时可以用final修饰。

### 7.8.2 final 方法

> 使用final方法的原因：
>
> 1. 锁定方法，防止继承覆盖重写
> 2. 早期的Java是为了效率，现在不需要使用final优化了。应该让编译器与虚拟机去处理效率问题。

**final和private关键字**

> 类中的所有private方法都隐式的指定为是final的。私有方法不是类接口的一部分，子类可以声明同一名称的方法，但这不是覆盖，这只是在子类中创建了一个新方法，恰好同名。但最好别这么做。

 ```java

public class FinalData {

    private int data;
    FinalData(int data){
        this.data = data;
    }
    private void changeData(int data) {
        this.data = data;
    }

    public int getData() {
        return data;
    }

    public void setData(int data) {
        this.data = data;
    }

    public static void main(String[] args) {
        FinalData finalData = new FinalData(3);
        System.out.println(finalData.getData());//3
        finalData.changeData(4);//在本类中可以访问私有方法
        System.out.println(finalData.getData());//4

        FinalData2 finalData2 = new FinalData2(3);
        System.out.println(finalData2.getData());//3
        finalData2.setData(5);//通过set来访问，这个changeData并不是覆盖
        System.out.println(finalData2.getData());//5
    }
}

class FinalData2 extends FinalData {
    private int data;

    FinalData2(int data) {
        super(data);
        this.data = data;
    }
    private void changeData(int data) {
        this.data = data;
    }

    public int getData() {
        return data;
    }

    public void setData(int data) {
        changeData(data);
    }
}
 ```

### 7.8.3 final类

> 不打算对这个类写继承，因为禁止继承，所以禁止重写，方法就相当于final的。域不变。

## 7.9 初始化及类加载

> 每个类的编译代码都存在于它自己的独立文件中，该文件只在需要使用程序代码时才会被加载。访问static域和调用static方法时才会加载(构造器也是static)。

### 7.9.1 继承与初始化

```java
package io.github.little.chapter6;

import org.w3c.dom.ls.LSOutput;

class Insert{
    private int i = 9;
    protected int j;
    Insert(){
        System.out.println("i = " + i + ", j = " + j);
        j = 39;
    }
    static int printInit(String s){
        System.out.println(s);
        return 47;
    }
    private static int x1 = printInit("static Insert.x1 initialized");
}
public class Beetle extends Insert{
    private int k = printInit("Beetle.k initialized");
    public Beetle(){
        System.out.println("k = " + k);
        System.out.println("j = " + j);
    }
    private static int x2 = printInit("static Insert.x2 initialized");

    public static void main(String[] args) {
        System.out.println("Beetle constructor");
        Beetle b = new Beetle();
    }
}
/*
static Insert.x1 initialized
static Insert.x2 initialized
Beetle constructor
i = 9, j = 0
Beetle.k initialized
k = 47
j = 39
*/
```

程序运行过程：

1. 试图访问Beetle.main()静态方法

2. 加载器开始启动找出Beetle类的编译代码(Beetle.class)

3. 发现它有个基类(由extends得知)，加载基类

4. 根基类static初始化

5. 根基类的子类static初始化……直到类加载完毕

6. 对象被创建，其中基本类型设置为默认值，引用被设置为null，创建对象的构造器被调用，但是还没进入执行——有基类就先调用基类构造器

   ![image-20210522084714029](https://i.loli.net/2021/05/22/RwV5d6g4KMFGYeT.png)

7. 根基类的构造器被调用，但在调用后并不执行构造器内部代码，而是初始化根 基类各实例变量

8. 根基类构造器其余部分被调用，完成构造器初始化

9. 然后子类也同基类构造器一样，先实例变量初始化，然后构造器初始化

# 第8章 多态

## 8.1 再论向上转型

> 对象可以作为自己本身的类型使用，也可以做为它的基类使用。

```java
package io.github.little.chapter8;

enum Note{
    MIDDLE_C, C_SHARP, B_FLAT;
}
class Instrument{
    public void play(Note n){
        System.out.println("Instructment.play()");
    }
}
class Wind extends Instrument{
    @Override
    public void play(Note n){
        System.out.println("Wind.play() " + n);
    }
}
public class Music {
    public static void tune(Instrument i){ //可以接受Instrument的子类
        i.play(Note.MIDDLE_C);
    }
    public static void main(String[] args) {
        Wind flute = new Wind();
        tune(flute);
    }
}
//Wind.play() MIDDLE_C
```

### 8.1.1 忘记对象类型

> 如果为每一个子类都编写tune方法要做的工作实在是太多了，如果一个方法只接收基类为参数，而不是特殊的子类，这样做情况会变好吗？

## 8.2 转机

> 上面的tune调用的是Wind中的方法，这正时我们所想要得到的，编译器怎么知道这个对象是Wind而不是Instrument？

### 8.2.1 方法调用绑定

> 将一个方法调用同一个方法主体关联起来被称做绑定。
>
> 前期绑定：执行前绑定，由编译器和连接程序实现
>
> 后期(动态)绑定：运行时根据对象的类型进行绑定，方法调用机制能找到正确的方法体。Java中除了static与final方法外，其他方法都是后期绑定。

### 8.2.2 产生正确的行为

```java
package io.github.little.chapter8;

import java.util.Random;

class Shapes {
    private static RandomShpaeGenerator gen = new RandomShpaeGenerator();

    public static void main(String[] args) {
        Shape[] s = new Shape[9];
        for (int i = 0; i < s.length; i++)
            s[i] = gen.next(); //调用工厂方法
        for (Shape shp : s) {
            shp.draw();
        }
    }
}

public class RandomShpaeGenerator {

    private Random rand = new Random();

    //工厂：随机产生一个Shap对象，return语句表示了向上转型
    public Shape next() {
        switch ((rand.nextInt(3))) {
            default:
            case 0:
                return new Circle();
            case 1:
                return new Square();
            case 2:
                return new Triangle();
        }
    }
}

class Shape {
    public void draw() {
    }

    public void erase() {
    }
}

class Circle extends Shape {
    @Override
    public void draw() {
        System.out.println("Circle.draw()");
    }

    @Override
    public void erase() {
        System.out.println("Circle.erase()");
    }
}

class Square extends Shape {
    @Override
    public void draw() {
        System.out.println("Suqare.draw()");
    }

    @Override
    public void erase() {
        System.out.println("Square.erase()");
    }
}

class Triangle extends Shape {
    @Override
    public void draw() {
        System.out.println("Triangle.draw()");
    }

    @Override
    public void erase() {
        System.out.println("Triangle.erase()");
    }
}
```

随机的产生对象，还是可以让程序产生我们想要的输出，表明了方法是动态绑定的。

### 8.2.3 可扩展性

> 可以根据自己的需求，对系统添加任意多的新类型，而不需要更改tune方法。

### 8.2.4 缺陷：“覆盖私有方法”

> 覆盖私有方法是不可行的，因为其被自动认为是final方法。

### 8.2.5 缺陷：“域与静态方法”

```java
public class FieldAccess {
    public static void main(String[] args)
    {
        Super sup = new Sub();
        System.out.println("sup.field = " + sup.field + ", sup.getFild() = " + sup.getField());
        Sub sub = new Sub();
        System.out.println("sub.field = " + sub.field + ", sub.getFild() = " + sub.getField()
                        + ", sub.getField() = " + sub.getSuperField());
    }
}
class Super
{
    public int field = 0;
    public int getField(){return field;}
}
class Sub extends Super
{
    public int field = 1;
    public int getField(){return field;}
    public int getSuperField(){return super.field;}
}
/*
sup.field = 0, sup.getFild() = 1
sub.field = 1, sub.getFild() = 1, sub.getField() = 0
*/
```

Sub对象转型为Supper引用时，任何域访问操作都将由编译器解析，不是运行时确定，不是多态的。Super.field与Sub.field分配了不同的存储空间，Sub实际上包含了两个叫field的域，它自己的和它从Super处得到的。编译时确定即代表声明时的对象(等号左边)就是最后得到的，sub只能得到sub自己的域，如果要得到其父类，一定要在其类中显式指明。实际应用时，因为绝大多数情况下将域都设置为private，所以这种情况很少发生。

==静态方法也没有多态性==

```java
package io.github.little.chapter8;
class StaticSuper {
    public static String staticGet() {
        return "Base StaticGet()";
    }

    public String dynamicGet() {
        return "Base dynamicGet()";
    }
}
class StaticSub extends StaticSuper {
    public static String staticGet() {
        return "Derived StaticGet()";
    }

    @Override
    public String dynamicGet() {
        return "Derived dynamicGet()";
    }
}

public class StaticPolymorphism {
    public static void main(String[] args) {
        StaticSuper sup = new StaticSub();

        System.out.println(sup.staticGet());//Base StaticGet()
        System.out.println(sup.dynamicGet());//Drived dynamicGet()
    }
}
```

## 8.3 构造器与多态

### 8.3.1 构造器的调用顺序

> 构造器因为是隐式的static，所以不具有多态性，其调用顺序是：
>
> 1. 调用基类构造器
> 2. 按声明顺序调用成员的初始化方法
> 3. 调用导出类构造器主体

在导出类构建时保证基类都已被构建，以去访问其中的public与protected成员。

### 8.3.2 继承与清理

……

### 8.3.3 构造器内部的多态方法的行为

```java
package io.github.little.chapter8;

class Glyph{
    void draw(){
        System.out.println("Glyph.draw()");
    }
    Glyph(){
        System.out.println("Glyph() before draw()");
        this.draw();//这里的this是对象的this，不是类的this，引用了什么对象类型就是什么this
        System.out.println("Glyph() after draw()");
    }
}
public class RoundGlyph extends Glyph{
    private int radius = 1;
    RoundGlyph(int r){
        super();
        radius = r;
        System.out.println("RoundGlyph.RoundGlyph(), radius = " + radius);
    }

    @Override
    void draw(){
        System.out.println("RoundGlyph.draw(), radius = " + radius);
    }
}
class PolyConstructors{
    public static void main(String[] args) {
        new RoundGlyph(5);
        /*调用方法时，如果不是static,private,final，运行时确定调用什么方法。
        虚拟机找到这个引用的实际类型，
            如果是子类且其有这个方法就调用这个方法
            否则调用父类里的。
         */
        new Glyph();
    }
}
Glyph() before draw()
RoundGlyph.draw(), radius = 0
Glyph() after draw()
RoundGlyph.RoundGlyph(), radius = 5
Glyph() before draw()
Glyph.draw()
Glyph() after draw()
```

如上面说到的

> 1. 在对象创造时，先给其内部的实例变量默认值
> 2. 然后调用到基类构造器，在此之前基类实例变量先初始化，然后再执行基类构造器内部的东西
> 3. 如果构造器内有重写的方法，那么根据多态要求，运行时确定其实际引用类型，调用其相应方法

构造器内不要调用有可能重写的方法，也就是只能调用private与final方法。

## 8.4 协变返回类型

> Java SE5：在导出类中的被覆盖方法可以返回**基类方法的返回类型**的某种导出类型。

```java
package io.github.little.chapter8;

public class CovariantReturn {
    public static void main(String[] args) {
        Mill mill = new Mill();
        Grain grain = mill.process(); //返回的是Grain
        System.out.println(grain);

        mill = new WheatMill();
        grain = mill.process(); //返回的是Wheat
        System.out.println(grain);
    }
}
//谷子
class Grain {
    public String toString() {
        return "Grain";
    }
}
//小麦
class Wheat extends Grain {
    public String toString() {
        return "Wheat";
    }
}
//面粉
class Mill {
    Grain process() {
        return new Grain();
    }
}
//小麦粉
class WheatMill extends Mill {
    @Override
    Wheat process() {//Wheat是Grain的某种导出类型
        return new Wheat();
    }
}
```

很显然，这种效果是多态机制的特点。

## 8.5 用继承进行设计

用组合来代替继承，利用多态的特点在组合内部动态选择类型。

```java
package io.github.little.chapter8;

import io.github.little.chapter6.A;

class Actor{
    public void act(){

    }
}
class HappyActor extends Actor{
    @Override
    public void act() {
        System.out.println("HappyActor");
    }
}
class SadActor extends Actor{
    @Override
    public void act() {
        System.out.println("SadActor");
    }
}
class Stage{
    private Actor actor = new HappyActor();
    //人为动态选择类型
    public void change(){
        actor = new SadActor();
    }
    public void performPlay(){
        actor.act();
    }
}
public class Transmogrify {
    public static void main(String[] args) {
        Stage stage = new Stage();
        stage.performPlay();
        stage.change();
        stage.performPlay();
        //使用继承时，虽然是运行时确定，但在编写时只能使用SacActor
        Actor actor = new SadActor();
        actor.act();
    }
}
HappyActor
SadActor
SadActor
```

《Java编程思想》上面所说的关于这点的原则是：用继承表达行为之间的差异，并用字段表达状态上的变化。也就是方法用继承重写，对象使用多态引用。

### 8.5.1 纯继承与扩展

> 纯继承意味着子类不建立新的方法。扩展意味着子类新增了方法，扩展部分不能被基类访问。

### 8.5.2 向下转型与运行时类型识别

> 向上转型是安全的：基类不会有导出类没有的接口。
>
> 向下转型不安全，不能确定一个"Animal" is "Cat"。

Java中提供多态，对基类向下转型要保证基类原本引用的就是子类对象，可以向下转型为子类对象引用。

```java
object instanceof class
```

判断对象引用的是不是那个类，如果是返回true。

```java
package io.github.little.chapter8;

import io.github.little.Exercise4.three.Rand;

public class Test {
    public static void main(String[] args) {
        Animal animal = new Bird();
        if (animal instanceof Cat){
            System.out.println("This is a Cat");
        }
        else if (animal instanceof Bird){
            System.out.println("This is a Dog");
            Bird bird= (Bird)animal;
            bird.eat();
            bird.fly();
        }
        animal.eat();
//        animal.fly();
    }
}
class Animal{
    void eat(){
        System.out.println("Animal eat");
    }
}
class Cat extends Animal{
    @Override
    void eat() {
        System.out.println("Cat eat");
    }
}
class Bird extends Animal{
    @Override
    void eat() {
        System.out.println("bird eat");
    }
    void fly(){
        System.out.println("bird fly");
    }
}

```

# 第9章 接口

> 抽象类，在普通类与抽象类之间的一种中庸之道。

## 9.1 抽象类和抽象方法

> 通用接口建立起一种基本形式，以此表示所有导出类的共同部分。这种通用接口叫做抽象基类，简称抽象类。抽象类提供的是一种接口，其中有着可以导出的通用方法。Java中称其为抽象方法，而这种方法只有声明没有方法体。
>
> 包含抽象方法的就叫抽象类，如果从一个抽象类继承，必须在子类中提供所有抽象方法的定义。

1. 抽象类不可创建对象
2. 抽象类有构造方法，因为子类继承要调用基类构造器
3. 抽象类不一定包含抽象方法，有抽象方法一定是抽象类
4. 抽象类的子类必须重写所有抽象方法，除非它也是抽象类

```java
abstract class Animals{
    abstract void eat();
    void fly(){
        System.out.println("Animals fly");
    }
}
class dog extends Animals{
    @Override
    void eat() {
        System.out.println("Dog eat");
    }
}
class bird extends Animals{
    @Override
    void eat() {
        System.out.println("Bird eat");
    }
    @Override
    void fly() {
        System.out.println("Bird eat");
    }
}
```

## 9.2 接口

> interface实现一个完全抽象的类，也就是说其中没有方法定义，只有声明。

```java
interface Animals {
    public void eat();
}

class dog implements Animals {
    @Override
    public void eat() {
        System.out.println("Dog eat");
    }
}

class bird implements Animals {
    @Override
    public void eat() {
        System.out.println("Bird eat");
    }
}
```

interface 代替了class，与class一样，也可以在前面加上public，不加的话其只具有包访问权限。接口中的方法默认是public  abstract的，也只能是public abstract。接口也可以包含域，但其是隐式的static或final的。

```java
package io.github.little.chapter9;

//枚举类型
enum Note {
    MIDDLE_C, C_SHARP, B_FLAT;
}

public class Music5 {
    static void tune(Instrument i) {
        i.play(Note.MIDDLE_C);
    }

    static void tuneAll(Instrument[] e) {
        for (Instrument i : e)
            tune(i);
    }

    public static void main(String[] args) {
        Instrument[] orchestra = {
                new Wind(),
                new Percussion(),
                new Stringed(),
                new Brass(),
                new Woodwind(),
        };
        tuneAll(orchestra);
    }
}

//接口
interface Instrument {
    //接口可以包含域，默认为static final
    static final int VALUE = 5;

    //接口的方法默认是public abstract
    public abstract void play(Note n);

    public abstract void adjust();
}

//接口实现，必须实现接口中的所有方法
class Wind implements Instrument {
    public void play(Note n) {
        System.out.println(this + ".play() " + n);
    }

    public String toString() {
        return "Wind";
    }

    public void adjust() {
        System.out.println(this + ".adjust()");
    }
}

class Percussion implements Instrument {
    public void play(Note n) {
        System.out.println(this + ".play() " + n);
    }

    public String toString() {
        return "Percussion";
    }

    public void adjust() {
        System.out.println(this + ".adjust()");
    }
}

class Stringed implements Instrument {
    public void play(Note n) {
        System.out.println(this + ".play() " + n);
    }

    public String toString() {
        return "Stringed";
    }

    public void adjust() {
        System.out.println(this + ".adjust()");
    }
}

//继承
class Brass extends Wind {
    public String toString() {
        return "Brass";
    }
}

class Woodwind extends Wind {
    public String toString() {
        return "Woodwind";
    }
}
```

## 9.3 完全解耦

```java
package chapter9;

import java.util.Arrays;

class Processor{
    //得到名字
    public String name(){
        return getClass().getSimpleName();
    }
    //接受一个输入，产生输出
    Object process(Object input){
        return input;
    }
}
class Upcase extends Processor{
    String process(Object input){
        return ((String)input).toUpperCase();
    }
}
class Downcase extends Processor{
    String process(Object input){
        return ((String)input).toLowerCase();
    }
}
class Splitter extends Processor{
    String process(Object input){
        return Arrays.toString(((String)input).split(" "));
    }
}
public class Apply {
    public static void process(Processor p, Object s){
        System.out.println("Using Processor " + p.name());
        //调用Processor类中的方法
        System.out.println(p.process(s));
    }
    public static String s = "Disagreement with beliefs is by definition incorrect";

    public static void main(String[] args) {
        //调用静态方法
        process(new Upcase(), s);
        process(new Downcase(), s);
        process(new Splitter(), s);
    }
}
Using Processor Upcase
DISAGREEMENT WITH BELIEFS IS BY DEFINITION INCORRECT
Using Processor Downcase
disagreement with beliefs is by definition incorrect
Using Processor Splitter
[Disagreement, with, beliefs, is, by, definition, incorrect]
```

这里的Processor方法可以根据所传递的参数对象的不同而具有不同行为的方法，这被称为策略设计模式。

## 9.4 Java中的多重继承

> Java中，因为接口没有具体实现（没有存储），一个类可以实现多个接口

```java
package chapter9;

interface CanFight{
    void fight();
}
interface CanSwim{
    void swim();
}
interface CanFly{
    void fly();
}
class ActionCharacter{
    public void fight(){

    }
}
class Hero extends ActionCharacter implements CanFight, CanSwim, CanFly{

    @Override
    public void swim() {

    }

    @Override
    public void fly() {

    }
}
public class Adventure {
    public static void t(CanFight x){x.fight();}
    public static void u(CanSwim x){x.swim();}
    public static void v(CanFly x){x.fly();}
    public static void w(ActionCharacter x){x.fight();}

    public static void main(String[] args) {
        Hero h = new Hero();
        /*完全可以向上转型*/
        t(h);
        u(h);
        v(h);
        w(h);
    }
}
```

如上面的代码一样，使用接口的核心原因：为了能够向上转型为多个基类型。

## 9.5 通过继承来扩展接口

```java
package io.github.little.chapter9;

import javax.management.ValueExp;

public class HorrorShow {
    static void u(Monster b){b.menace();}
    static void v(DangerousMonster d)
    {
        d.menace();
        d.destroy();
    }
    static void w(Lethal l)
    {
        l.kill();
    }

    public static void main(String[] args) {
        DangerousMonster barney = new DragonZilla();
        u(barney);
        v(barney);
        Vampire vlad = new VerBadVampire();
        u(vlad);
        v(vlad);
        w(vlad);
    }
}
interface Monster//怪物
{
    public abstract void menace();//危险
}
interface DangerousMonster extends Monster
{
    void destroy();
}
interface Lethal//致命的
{
    void kill();
}
class DragonZilla implements DangerousMonster
{
    public void menace(){}
    public void destroy(){}
}
/*接口可多继承，但是不能实现*/
interface Vampire extends DangerousMonster, Lethal
{
    public abstract void drinkBlood();
}
class VerBadVampire implements Vampire
{
    @Override
    public void destroy(){}

    @Override
    public void kill(){}

    @Override
    public void menace(){}

    @Override
    public void drinkBlood() {}
}
```

![HorrShow](https://i.loli.net/2021/07/24/TrCzbug2pDQmKG8.png)

### 9.5.1 组合接口时的名字冲突

```java
package chapter9;

interface I1 {
    void f();
}
interface I2{
    int f(int i);
}
interface I3{
    int f();
}
class C{
    public int f(){
        return 1;
    }
}
class C2 implements I1, I2{
    /*实现方法*/
    public void f(){
        
    }
    public int f(int i){
        return 1;
    }
}
class C3 extends C implements I2{
    @Override
    public int f(){
        return 1;
    }
    /*实现*/
    @Override
    public int f(int i) {
        return 0;
    }
}
public class C4 extends C implements I3{
    /*完全相同*/
    public int f(){
        return 1;
    }
}
/*编译器不知道用哪个*/
//class C5 extends C implements I1 { } 
//interface I4 extends I1, I3{}
```

## 9.6 适配接口

> 同一个接口，具有多个不同的具体实现，让任何一个类都可以对该方法进行适配。

## 9.7 接口中的域

> 接口中的域是final和static的，在Java SE5之前，这是解决没有C和C++中enum的办法。

其可以初始化。

## 9.8 嵌套接口

> 接口放在接口里，默认包权限 ，也可以是pulic和private的。



## 9.9 接口与工厂

> Creational patterns become important as systems evolve to depend more on object composition than class inheritance.

由于系统的进化更多地依赖于对象组合而不是类继承，创造模式变得重要。

```java
package chapter9;

interface Service {
    void method1();
    void method2();
}

/*抽象的接口*/
interface ServiceFactory {
    /*工厂方法，获得接口的某个实现对象*/
    /*利用多接口的多态*/
    Service getService();
}

class Implementation1 implements Service {
    Implementation1() {
    }

    public void method1() {
        System.out.println("Implementation1 method1");
    }

    public void method2() {
        System.out.println("Implementation1 method2");
    }
}

class Implementation1Factory implements ServiceFactory {
    public Service getService() {
        return new Implementation1();
    }
}

class Implementation2 implements Service {
    Implementation2() {
    }

    public void method1() {
        System.out.println("Implementation2 method1");
    }

    public void method2() {
        System.out.println("Implementation2 method2");
    }
}

class Implementation2Factory implements ServiceFactory {
    public Service getService() {
        return new Implementation2();
    }
}

public class Factories {
    public static void serviceConsumer(ServiceFactory factory) {
        /*通过工厂得到具体的Service*/
        Service s = factory.getService();
        s.method1();
        s.method2();
    }
//    public static void serviceConsumer(Service s) {
//        /*通过工厂得到具体的Service*/
////        Service s = factory.getService();
//        s.method1();
//        s.method2();
//    }

    public static void main(String[] args) {
        serviceConsumer(new Implementation1Factory());
        serviceConsumer(new Implementation2Factory());
//        serviceConsumer(new Implementation1());
//        serviceConsumer(new Implementation2());
    } 
}

```

[漫画：设计模式之 “工厂模式” - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/239952896)

工厂模式实现了方法与构造器的分离。

# 第十章 内部类

## 10.1 创建内部类

> 根据类加载的顺序可知，在外部类的非静态方法中可以直接使用类名来引用。非静态方法之外的位置就应该使用

`OutClassNamer.InnerClassName`的方式

## 10.2 链接到外部类

> 内部类拥有其外围类的所有元素的访问权。

## 10.3 使用.this与.new

内部类使用外部类对象：

```java
package io.github.little.chapter10;

public class DotThis {
    void f(){
        System.out.println("DotThis.f()");
    }
    public class Inner{
        public Inner() {
        }
        public DotThis outer(){
            return DotThis.this;
        }
    }
    public Inner inner(){return new Inner();}
    public static void main(String[] args)
    {
        DotThis dt = new DotThis();
        DotThis.Inner dti = dt.inner();
        /*内部类方法返回外部类对象，从这点也可以说明，内部类创建时外部类必先创建了*/
        dti.outer().f();
    }
}
```

外部类使用内部类对象：

```java
public class DotNew {
    public DotNew() {
        System.out.println("调用了外部构造类");
    }
    class Inner{}
    public static void main(String [] args)
    {
        DotNew dn = new DotNew();
        DotNew.Inner dni = dn.new Inner();
    }
}
```

静态内部类不需要外部类对象

```java
package chapter10;

public class Parcel3 {
    class Contents{
        private int i = 11;
        public int value(){
            return i;
        }
    }
    class Destination{
        private String label;
        Destination(String whereTo){
            label = whereTo;
        }
        String readLabel(){
            return label;
        }
    }
    static class StaticCla{
        public StaticCla() {
            System.out.println("k");
        }
    }

    public static void main(String[] args) {
        Parcel3 p = new Parcel3();
        Parcel3.Contents c = p.new Contents();
        Parcel3.Destination d = p.new Destination("Tasmania");
        Parcel3.StaticCla e = new StaticCla();
    }
}
```

## 10.4 内部类与向上转型

```java
package io.github.little.chapter10;


interface Destination{
    String readLabell();
}
interface Contents{
    int value();
}
public class Parcel4 {
    private class PContents implements Contents{
        private int i = 11;
        public int value(){return i;}
    }
    protected class PDestination implements Destination
    {
        private String label;
        private PDestination(String whereTo)
        {
            label = whereTo;
        }

        @Override
        public String readLabell() {
            return label;
        }
    }
    public Contents contents(){
        return new PContents();
    }
    public Destination destination(String s)
    {
        return new PDestination(s);
    }
}
class TestParcel
{
    public static void main(String[] args)
    {
        Parcel4 p = new Parcel4();
        /*只有Parcel4能访问它, 向上转型*/
        Contents c = p.contents();
        Destination d = p.destination("Tasmania");
    }
}

```

## 10.5 在方法和作用域的内部类

## 10.6 匿名内部类

```java
package chapter10;

public class Parcel7 {
    public Contents contents(){
        /*这种返回的类其实是对Contents的实现*/
        return new Contents(){
            private int i = 11;
            public int value(){
                return i;
            }
        };
    }
}
class Contents{
    private int i = 11;
    public int value(){
        return i;
    }
}
```

如果从外部传参进行匿名内部类给内部类的字段使用它就要将参数设置为final。

如果想有构造器行为就用下面的方式：

```java
package chapter10;

abstract class Base{
    public Base(int i){
        /*1*/
        System.out.println("Base constructor, i = " + i);
    }
    public abstract void f();
}
public class AnonymousConstructor {
    public static Base getBase(int i){
        return new Base(i) {
            {
                /*2*/
                System.out.println("Inside instance initializer");
            }
            @Override
            public void f() {
                /*3*/
                System.out.println("In anonymous f()");
            }
        };
    }
    public static void main(String[] args) {
        Base base = getBase(47);
        base.f();
    }
}
```

# 第十一章 持有对象

## 11.1 泛型和类型安全的容器

容器中选择泛型可以防止错误的对象放入容器，泛型允许向上转型。

```java
package chapter11;

import java.util.ArrayList;

class GrannySmith extends Apple{}
class Gala extends Apple{}
class Apple{}

public class GenericsAndUpcasting {
    public static void main(String[] args) {
        ArrayList<Apple> apples = new ArrayList<>();
        apples.add(new Gala());
        apples.add(new GrannySmith());
        for (Apple e: apples) {
            System.out.println(e);
        }
    }
}
类名@散列码
chapter11.Gala@7a79be86
chapter11.GrannySmith@34ce8af7
```