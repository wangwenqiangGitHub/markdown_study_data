# Java文件运行全流程及JVM类加载机制

1. .java文件通过java编译器(javac.exe)编译成.class文件，字节码文件。
2. .class文件加载到jvm中运行。

* JVM如何处理这些文件.class文件?

  jvm被分为三个主要的子系统

  1. 类加载器子系统
  2. 运行时栈区
  3. 执行引擎

  一、加载（开发人员可以使用系统提供的类加载器来完成加载）

  ​		此时虚拟机需要完成三件事

  1. 通过一个类的全限定名来获取其定义的二进制字节流
  2. 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构
  3. 在Java堆中生成一个代表这个类的java.lang.Class对象，作为方法区中这些数据的访问入口

  加载阶段完成后，虚拟机外部的二进制字节流就按照虚拟机所需格式存储在方法区之中，而且在Java堆中也创建一个java.lang.Class类的对象，这样便可以通过该对象访问方法区中的这些数据。

  二、连接

  1. 验证：确保被加载的的类的正确性
     1. 文件格式验证
     2. 元数据验证
     3. 字节码验证
     4. 符号引用验证
  2. 准备：为类的静态变量分配内存，并将其初始化为默认值
     1. 仅包括类变量，不包括实例变量，实例变量是在对象实例化时随着对象一块分配到Java堆中。
     2. 这里所设置的值通常都是数据类型默认的零值，不是在Java代码中的显式 赋值。
  3. 解析：所类中的符号引用转换为直接引用
     1. 符号引用就是一组符号来御描述目标，可以是任何字面量。
     2. 直接引用就是直接指向引用的指针、相对偏移量，或一个间接定位到目标的句柄。

  三、初始化

  到了这个阶段，才真正开始执行类中定义的Java程序代码（字节码）。这里所有的静态变量会被赋初值，并且静态块将会被执行。



# Java面试题

## 基础语法

### 1. JDK、JRE、JVM

> Java Vritual Machine (java虚拟机) 实现了不同平台间不需要多次编译，一次编译，多次运行。
>
> Java Runtime Enviroment (java运行时环境) 包含了JVM和Java核心类库。
>
> Java Development Kit (java 开发工具包) Java语言编写程序所需要的开发工具包。

```java
JDK = JRE + Java 的开发工具（javac.exe, java.exe, javadoc.exe)

JRE = JVM + Java核心类库
```

### 2. == 和 equals的区别是什么？

* ==是关系运算符，equals()是方法，结果都返回布尔值。
* 对于对象，也就是Object而言，比较的都是地址，两者作和相同

* ==作用：
  * 基本类型：比较值是否相等
  * 引用类型：比较内存地址值是否相等
  * 不能比较没有父子关系的两个对象

```java
    @Test
    public void equalsTest(){
        Person person = new Person("Mike", 23);
        Student student = new Student();

        System.out.println(student == person);//不能比较没有父子关系两个类对象
        System.out.println(student.equals(person));
    }
```

* equals()方法的作用：
  * JDK中的类一般已经重写了euqals()，比较的是它们中的内容。String，Date，File，包装类等
  * 自定义类中如果没有重写equals()方法，将调用父类(默认Object类)的equals()方法，Object的equals()比较使用了this == obj
  * 可以按照需逻辑重写对对象的equals()方法。（此时一般需要重写hashCode()方法）

### 3. 基本类型和包装类对象使用==和equals()进行比较的结果

1. 值不同：两者都返回false
2. 值相同
   1. 用==
      1. 基本类型-基本类型、基本类型-包装类型：返回true
      2. 包装类-包装类：返回true
      3. 缓存中取的包装对象比较返回true，（原因是JVM缓存部分基本类型常用的包装类对象是被缓存的Integer的-127~128。
   2. 用equals()比较
      1. 包装类-基本类型，返回true
      2. 包装类-包装类型，返回true
3. 不同类型，返回false

### 4. 什么是装箱，什么是拆箱

1. 装箱：基本类型->包装类
2. 拆箱：包装类->基本类型

* 执行过程

  * 装箱调用包装类valueOf(int)静态方法
  * 拆箱调用包装类的Integer.XXXValue()方法

  ```java
  //装箱
  Integer i = Integer.valueOf(3);
  //拆箱
  i.intValue();
  ```

* 注意
  * 整型的包装类valueOf()方法返回对象时，在常用的取值范围，会返回缓存对象
  * 浮点型的包装类valueOf()方法返回新的对象
  * 布尔型的包装类valueOf()方法会返回Boolean类的静态常量TRUE|FALSE

包含算术运算会触发自动拆箱。创建缓存是为了节约内存，因为会产生很多新的对象。

### 5. hashCode()相同，equals()也一定为true吗？

不一定，反过来也不一定

* 两个方法都可以自己重写，完全由自己定义
* hashCode()返回哈希值，equals()返回两个对象是否相等

常规规定

* 两个对象equals()返回true，它们就是一样的字面值，可能地址不同，所以两者的hashCode()应该返回相同的结果。
* 两个对象equals()返回true，不要求哈希值一定不同
* 重写equals()方法，必须重写hashCode()方法，以保证equals()方法相等时两个对象hashCode()返回相同的值。

### 6. final在java中的作用

final可以修饰数据，方法，和类

* 类：不能被继承
* 方法：不能被重写，可以被重载
* 数据：
  * 成员变量要初始化，赋初值后不能再修改
  * 引用：不可以指向别的对象了

### 7. final、finally、finalize()的区别

1. final上面说了
2. finall是异常处理的一部分，表示希望finally语句块中的代码最后一定被执行
3. finalize()是在java.lang.Object里定义的，对象被回收时会调用。一般由JVM调用。当对象被回收时，释放一些资源，须调用super.finalize()

### 8. finally语句一定执行吗

1. 还没运行到try之前就返回，抛出异常
2. 还没运行到finally语句就系统退出

### 9. final与static的区别

* 都可以修饰类、方法、成员变量
* 都不能用于修饰构造方法
* static 可以修饰类的代码块，final不可以
* final可以修饰局部变量，static 不可以

static 

1. 表示静态或全局，被修饰的属性和方法属于类，可用类名.静态属性/方法名访问
2. 修饰的属性，代码块表示静态（属性）代码块，当Java虚拟机（JVM）加载类时，变会执行该代码块，而且只会被执行一次。
3. 可以重新赋值
4. 不能用this和super关键字
5. static方法必须被实现，而不能是抽象的abstract
6. static方法不能被重写

final

1. 一旦创建不可改变
2. 标记的成员变量必须在声明的同时赋值，或在该类的构造方法中赋值，不可重新赋值
3. 方法不能重写
4. 类不能继承，其中的方法是默认final的

### 10. retrun与finally的执行顺序对返回值的影响

* finally语句会执行
* 如果都有return，以finally的return为准

```java
public class Change {
    @Test
    public void equalsTest(){
        System.out.println(get());
    }
    public static String get()
    {
        String str = "A";
        try{
            str = "B";
            return str;
        }finally {
            str = "C";
            return str;
        }
    }
}
C
```

### 11. String对象中的replace和replaceAll的区别

1. 都是替换字符或字符串的方法，返回一个新对象
2. 后者支持正则表达式

### 12. Math的round()方法

JDK中的java.lang.Math类

* ceil()：向上取整
* floor()：向下取整
* round()：+0.5向下取整，朝正方向4舍5入

Math.round(1.5) = 2.0

Math.round(-1.5) = 1.0

### 13. String属于基础的数据类型吗

8种基本类型：byte、short、char、int、long、float、double、boolean

String是最常用到的引用类型

### 14. Java中操作字符串的都有哪些类，它们的区别是什么？

String、StringBuilder、StringBuffer三者底层都是用char[]存

* String：final修饰，其返回的方法都是返回一个新的对象，所以原对象不会改变
* StringBuffer：线程安全，效率低，JDK5.0新增
* StringBulider：线程不安全，可以修改原字符串

### 15. 反转字符串

* 使用StringBulider和StringBuffer里的reverse方法，本质是调用了AbstractStringBuilder中的reverse(JDK8)

```java
/**
 * 非递归反转字符串, 数组法
 * @param str
 * @return
 */
public String reverse(String str)
{
    char[] ch = str.toCharArray();
    int length = str.length();
    for (int i = 0, j = length - 1; i < j; i++, j--)
    {
        char temp = ch[i];
        ch[i] = ch[j];
        ch[j] = temp;
    }
    return new String(ch);
}

public String reverse0(String str)
{
    int length = str.length();
    char[] ch = new char[length];
    for (int i = length - 1, j = 0; i >= 0; i--, j++)
    {
        ch[j] = str.charAt(i);
    }
    return new String(ch);
}
/**
 * 递归反转字符串
 * @param str
 * @return
 */
public String reverse1(String str)
{
    if (str == null || str.length() <= 1)
        return str;
    return reverse1(str.substring(1)) + str.charAt(0);
}
```

### 17. 普通类和抽象类的区别

* 抽象类不能实例化
* 抽象类可以有抽象方法，抽象方法无需实现
* 有抽象方法的类就是抽象类
* 抽象类的子类必须实面抽象类的所有抽象方法，否则这个类也是抽象类
* 抽象方法不能为静态
* 抽象方法不能是private
* 抽象方法不能用final修饰

### 18. 抽象类一定要有抽象方法吗？

不一定，但是有抽象方法的一定是抽象类。

### 19. 抽象类能使用final修饰吗？

不能抽象类就是要被继承。

### 20. 接口和抽象类的区别

* 抽象类可以有构造器，接口不可以
* 抽象类中可以有普通成员变量，接口中的域都是public static final
* 抽象类中方法可以是protected、public、default，接口中方法一般只能是public，默认为public abstract。但在JDK9中接口中的方法可以是private。抽象类中可以有静态方法，接口在JDK8前不可以，8之后就可以。接口中也可以有默认方法
* 一个类可以实现多个接口，用逗号隔开，但是只能继承一个抽象类
* 接口不能实现一个接口，但是可以继承接口，用逗号隔开

> 从构造器，域，和方法，使用四个层面来说

### 21. Java访问修饰符有哪些？

|              | public | protected             | （default) | private |
| ------------ | ------ | --------------------- | ---------- | ------- |
| 同一个类     | √      | √                     | √          | √       |
| 同一个包     | √      | √                     | √          | ×       |
| 不同包子类   | √      | √（可以访问初始父类） | ×          | ×       |
| 不同包非子类 | √      | ×                     | ×          | ×       |

### 22. >> << >>>

<< 左移，低位补0

\>>右移，为正，高位补0，为负，高位补1

\>>>右移，全补0

> 左移都是无符号的，右移有两种

### 23. javap的作用是什么？

是Java class文件分解器，可以反编译，也可以查看java编译器生成的字节码等

### 24. throw和throws

* throw

  * 表示方法内抛出某种异常对象
  * 用于程序自行产生并抛出异常
  * 位于方法体内部，可以作为单独语句使用
  * 如果不是RuntimeException则要多throws要么try-catch-finally
  * 执行到throwr后面的语句不执行

* throws

  * 用在方法的定义上，表示这个方法可能会抛出某些异常
  * 必须放在参数列表后，不能单独使用
  * 在方法的调用处需要进行异常处理

  

### 25. try-catch-finally是否可以省略其中的某些部分

catch和finally语句块可以省略其中的一个，否则编译会报错。

### 26. 常见的异常类

Throwable是异常的根类

#### 异常的体系结构

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

### 27. 什么是Java内部类

#### 存在于Java类内部的类。

* 成员内部类

  * 编译生成OutClass.class和OutClass$InnerClass.class

* 方法内部类

  * 编译也会生成两个.class文件
  * 该类只能在该方法内实例化
  * 方法结束后，其栈结构被删除，在其内创建的内部类还可能仍然在堆中

* 匿名内部类

* 接口式匿名内部类

  * ```java
    interface IFish{
        public void swim();
    }
    public class TestIFish {
        public static void main(String[] args) {
            IFish fish = new IFish() {
                @Override
                public void swim() {
                    System.out.println("Swimming");
                }
            };
        }
    }
    ```

    相当于实现在接口的一个匿名内部类

* 参数式匿名内部类

* 静态嵌套类

  ```java
  Father.Son son = new Father.Son();//实例化
  class Father{
      static class Son{
      }
  }
  ```

  #### 作用

  1. 提供了进入其继承的类或实现的接口的窗口
  2. 与外部类无关，独立继承其他类或实现接口
  3. 提供了Java多继承的解决方案，弥补了Java单继承的不足

### 28. nio中的Files类常用方法有哪些？

- isExecutable：文件是否可以执行
- isSameFile：是否同一个文件或目录
- isReadable：是否可读
- isDirectory：是否为目录
- isHidden：是否隐藏
- isWritable：是否可写
- isRegularFile：是否为普通文件
- getPosixFilePermissions：获取POSIX文件权限，windows系统调用此方法会报错
- setPosixFilePermissions：设置POSIX文件权限
- getOwner：获取文件所属人
- setOwner：设置文件所属人
- createFile：创建文件
- newInputStream：打开新的输入流
- newOutputStream：打开新的输出流
- createDirectory：创建目录，当父目录不存在会报错
- createDirectories：创建目录，当父目录不存在会自动创建
- createTempFile：创建临时文件
- newBufferedReader：打开或创建一个带缓存的字符输入流
- probeContentType：探测文件的内容类型
- list：目录中的文件、文件夹列表
- find：查找文件
- size：文件字节数
- copy：文件复制
- lines：读出文件中的所有行
- move：移动文件位置
- exists：文件是否存在
- walk：遍历所有目录和文件
- write：向一个文件写入字节
- delete：删除文件
- getFileStore：返回文件存储区
- newByteChannel：打开或创建文件，返回一个字节通道来访问文件
- readAllLines：从一个文件读取所有行字符串
- setAttribute：设置文件属性的值
- getAttribute：获取文件属性的值
- newBufferedWriter：打开或创建一个带缓存的字符输出流
- readAllBytes：从一个文件中读取所有字节
- createTempDirectory：在特殊的目录中创建临时目录
- deleteIfExists：如果文件存在删除文件
- notExists：判断文件不存在
- getLastModifiedTime：获取文件最后修改时间属性
- setLastModifiedTime：更新文件最后修改时间属性
- newDirectoryStream：打开目录，返回可迭代该目录下的目录流
- walkFileTree：遍历文件树，可用来递归删除文件等操作

 

如测试获取文件所属人

```javascript
public static void testGetOwner() throws IOException {
	Path path_js = Paths.get("/Users/constxiong/Desktop/index.js");
	System.out.println(Files.getOwner(path_js));
}
```

 

具体介绍和使用，可参照：

- https://www.cnblogs.com/ixenos/p/5851976.html
- https://www.jianshu.com/p/3cb5ca04e3c8

 

### 29. 什么是反射？有什么作用？

Java 反射，就是在运行状态中

- 获取任意类的名称、package 信息、所有属性、方法、注解、类型、类加载器、modifiers（public、static）、父类、现实接口等
- 获取任意对象的属性，并且能改变对象的属性
- 调用任意对象的方法
- 判断任意一个对象所属的类
- 实例化任意一个类的对象

Java 的动态就体现在反射。通过反射我们可以实现动态装配，降低代码的耦合度；动态代理等。反射的过度使用会严重消耗系统资源。

JDK 中 java.lang.Class 类，就是为了实现反射提供的核心类之一。

一个 jvm 中一种 Class 只会被加载一次。

### 30. 动态代理是什么？应用场景？

**动态代理：在运行时，创建目标类，可以调用和扩展目标类的方法。**

 

**Java 中实现动态的方式：**

- JDK 中的动态代理 
- Java类库 CGLib

 

**应用场景：**

- 统计每个 api 的请求耗时
- 统一的日志输出
- 校验被调用的 api 是否已经登录和权限鉴定
- Spring的 AOP 功能模块就是采用动态代理的机制来实现切面编程

### 31. 怎么实现动态代理？

- JDK 动态代理
- CGLib 动态代理
- 使用 Spring aop 模块完成动态代理功能

### 32. 什么是java序列化？什么情况下需要序列化？

**序列化：将 Java 对象转换成字节流的过程。**

**反序列化：将字节流转换成 Java 对象的过程。**

 

当 Java 对象需要在网络上传输 或者 持久化存储到文件中时，就需要对 Java 对象进行序列化处理。

序列化的实现：类实现 Serializable 接口，这个接口没有需要实现的方法。实现 Serializable 接口是为了告诉 jvm 这个类的对象可以被序列化。

 

**注意事项：**

- 某个类可以被序列化，则其子类也可以被序列化
- 对象中的某个属性是对象类型，需要序列化也必须实现 Serializable 接口
- 声明为 static 和 transient 的成员变量，不能被序列化。static 成员变量是描述类级别的属性，transient 表示临时数据
- 反序列化读取序列化对象的顺序要保持一致

 

具体使用

```javascript
package constxiong.interview;
 
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;
 
/**
 * 测试序列化，反序列化
 * @author ConstXiong
 * @date 2019-06-17 09:31:22
 */
public class TestSerializable implements Serializable {
 
	private static final long serialVersionUID = 5887391604554532906L;
	
	private int id;
	
	private String name;
 
	public TestSerializable(int id, String name) {
		this.id = id;
		this.name = name;
	}
	
	@Override
	public String toString() {
		return "TestSerializable [id=" + id + ", name=" + name + "]";
	}
 
	@SuppressWarnings("resource")
	public static void main(String[] args) throws IOException, ClassNotFoundException {
		//序列化
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("TestSerializable.obj"));
		oos.writeObject("测试序列化");
		oos.writeObject(618);
		TestSerializable test = new TestSerializable(1, "ConstXiong");
		oos.writeObject(test);
		
		//反序列化
		ObjectInputStream ois = new ObjectInputStream(new FileInputStream("TestSerializable.obj"));
		System.out.println((String)ois.readObject());
		System.out.println((Integer)ois.readObject());
		System.out.println((TestSerializable)ois.readObject());
	}
 
}
```

打印结果：

```javascript
测试序列化
618
TestSerializable [id=1, name=ConstXiong]
```

### 33. 什么场景要对象克隆？

- 方法需要 return 引用类型，但又不希望自己持有引用类型的对象被修改。
- 程序之间方法的调用时参数的传递。有些场景为了保证引用类型的参数不被其他方法修改，可以使用克隆后的值作为参数传递。

### 34. 深拷贝和浅拷贝区别是什么？

复制一个 Java 对象


**浅拷贝：复制基本类型的属性；引用类型的属性复制，复制栈中的变量 和 变量指向堆内存中的对象的指针，不复制堆内存中的对象。**

![img](https://www.javanav.com/aimgs/20190618153224322__20190922012225.png)

 

**深拷贝：复制基本类型的属性；引用类型的属性复制，复制栈中的变量 和 变量指向堆内存中的对象的指针和堆内存中的对象。**

![img](https://www.javanav.com//aimgs/20190618154423857__20190922012336.png)

### 35. 如何实现对象克隆与深拷贝？

**1、实现 Cloneable 接口，重写 clone() 方法。**

**2、不实现 Cloneable 接口，会报 CloneNotSupportedException 异常。**

```javascript
package constxiong.interview;
 
/**
 * 测试克隆
 * @author ConstXiong
 * @date 2019-06-18 11:21:21
 */
public class TestClone {
 
	public static void main(String[] args) throws CloneNotSupportedException {
		Person p1 = new Person(1, "ConstXiong");//创建对象 Person p1
		Person p2 = (Person)p1.clone();//克隆对象 p1
		p2.setName("其不答");//修改 p2的name属性，p1的name未变
		System.out.println(p1);
		System.out.println(p2);
	}
	
}
 
/**
 * 人
 * @author ConstXiong
 * @date 2019-06-18 11:54:35
 */
class Person implements Cloneable {
	
	private int pid;
	
	private String name;
	
	public Person(int pid, String name) {
		this.pid = pid;
		this.name = name;
		System.out.println("Person constructor call");
	}
 
	public int getPid() {
		return pid;
	}
 
	public void setPid(int pid) {
		this.pid = pid;
	}
 
	public String getName() {
		return name;
	}
 
	public void setName(String name) {
		this.name = name;
	}
 
	@Override
	protected Object clone() throws CloneNotSupportedException {
		return super.clone();
	}
 
	@Override
	public String toString() {
		return "Person [pid:"+pid+", name:"+name+"]";
	}
	
}
```

打印结果

```javascript
Person constructor call
Person [pid:1, name:ConstXiong]
Person [pid:1, name:其不答]
```

 

**3、Object 的 clone() 方法是浅拷贝，即如果类中属性有自定义引用类型，只拷贝引用，不拷贝引用指向的对象。**

 

**可以使用下面的两种方法，完成 Person 对象的深拷贝。**

**方法1、对象的属性的Class 也实现 Cloneable 接口，在克隆对象时也手动克隆属性。**

```javascript
    @Override
	public Object clone() throws CloneNotSupportedException {
		DPerson p = (DPerson)super.clone();
		p.setFood((DFood)p.getFood().clone());
		return p;
	}
 
```

 

完整代码

```javascript
package constxiong.interview;
 
/**
 * 测试克隆
 * @author ConstXiong
 * @date 2019-06-18 11:21:21
 */
public class TestManalDeepClone {
 
	public static void main(String[] args) throws Exception {
		DPerson p1 = new DPerson(1, "ConstXiong", new DFood("米饭"));//创建Person 对象 p1
		DPerson p2 = (DPerson)p1.clone();//克隆p1
		p2.setName("其不答");//修改p2的name属性
		p2.getFood().setName("面条");//修改p2的自定义引用类型 food 属性
		System.out.println(p1);//修改p2的自定义引用类型 food 属性被改变，p1的自定义引用类型 food 属性也随之改变，说明p2的food属性，只拷贝了引用，没有拷贝food对象
		System.out.println(p2);
	}
	
}
 
class DPerson implements Cloneable {
	
	private int pid;
	
	private String name;
	
	private DFood food;
	
	public DPerson(int pid, String name, DFood food) {
		this.pid = pid;
		this.name = name;
		this.food = food;
		System.out.println("Person constructor call");
	}
 
	public int getPid() {
		return pid;
	}
 
	public void setPid(int pid) {
		this.pid = pid;
	}
 
	public String getName() {
		return name;
	}
 
	public void setName(String name) {
		this.name = name;
	}
 
	@Override
	public Object clone() throws CloneNotSupportedException {
		DPerson p = (DPerson)super.clone();
		p.setFood((DFood)p.getFood().clone());
		return p;
	}
 
	@Override
	public String toString() {
		return "Person [pid:"+pid+", name:"+name+", food:"+food.getName()+"]";
	}
 
	public DFood getFood() {
		return food;
	}
 
	public void setFood(DFood food) {
		this.food = food;
	}
	
}
 
class DFood implements Cloneable{
	
	private String name;
	
	public DFood(String name) {
		this.name = name;
	}
 
	public String getName() {
		return name;
	}
 
	public void setName(String name) {
		this.name = name;
	}
 
	@Override
	public Object clone() throws CloneNotSupportedException {
		return super.clone();
	}
	
}
```

 

打印结果

```javascript
Person constructor call
Person [pid:1, name:ConstXiong, food:米饭]
Person [pid:1, name:其不答, food:面条]
```

 

**方法2、结合序列化(JDK java.io.Serializable 接口、JSON格式、XML格式等)，完成深拷贝**

结合 java.io.Serializable 接口，完成深拷贝

```javascript
package constxiong.interview;
 
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;
 
public class TestSeriazableClone {
 
	public static void main(String[] args) {
		SPerson p1 = new SPerson(1, "ConstXiong", new SFood("米饭"));//创建 SPerson 对象 p1
		SPerson p2 = (SPerson)p1.cloneBySerializable();//克隆 p1
		p2.setName("其不答");//修改 p2 的 name 属性
		p2.getFood().setName("面条");//修改 p2 的自定义引用类型 food 属性
		System.out.println(p1);//修改 p2 的自定义引用类型 food 属性被改变，p1的自定义引用类型 food 属性未随之改变，说明p2的food属性，只拷贝了引用和 food 对象
		System.out.println(p2);
	}
	
}
 
class SPerson implements Cloneable, Serializable {
	
	private static final long serialVersionUID = -7710144514831611031L;
 
	private int pid;
	
	private String name;
	
	private SFood food;
	
	public SPerson(int pid, String name, SFood food) {
		this.pid = pid;
		this.name = name;
		this.food = food;
		System.out.println("Person constructor call");
	}
 
	public int getPid() {
		return pid;
	}
 
	public void setPid(int pid) {
		this.pid = pid;
	}
 
	public String getName() {
		return name;
	}
 
	public void setName(String name) {
		this.name = name;
	}
 
	/**
	 * 通过序列化完成克隆
	 * @return
	 */
	public Object cloneBySerializable() {
		Object obj = null;
		try {
			ByteArrayOutputStream baos = new ByteArrayOutputStream();
			ObjectOutputStream oos = new ObjectOutputStream(baos);
			oos.writeObject(this);
			ByteArrayInputStream bais = new ByteArrayInputStream(baos.toByteArray());
			ObjectInputStream ois = new ObjectInputStream(bais);
			obj = ois.readObject();
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return obj;
	}
 
	@Override
	public String toString() {
		return "Person [pid:"+pid+", name:"+name+", food:"+food.getName()+"]";
	}
 
	public SFood getFood() {
		return food;
	}
 
	public void setFood(SFood food) {
		this.food = food;
	}
	
}
 
class SFood implements Serializable {
	
	private static final long serialVersionUID = -3443815804346831432L;
	
	private String name;
	
	public SFood(String name) {
		this.name = name;
	}
 
	public String getName() {
		return name;
	}
 
	public void setName(String name) {
		this.name = name;
	}
	
}
```



打印结果

```javascript
Person constructor call
Person [pid:1, name:ConstXiong, food:米饭]
Person [pid:1, name:其不答, food:面条]
```

### 36. Java跨平台运行原理

1. .java源文件要先编译成与操作系统无关的.class字节码文件，然后字节码文件再通过Java虚拟机解释成机器码运行。
2. .class字节码文件面向虚拟机，不面向任何具体操作系统。
3. 不同平台的虚拟机是不同的，但它们给JDK提供了相同的接口。
4. Java的跨平台依赖于不同系统的Java虚拟机。

### 37.Java的安全性体现在哪里？

1、使用引用取代了指针，指针的功能强大，但是也容易造成错误，如数组越界问题。

2、拥有一套异常处理机制，使用关键字 throw、throws、try、catch、finally

3、强制类型转换需要符合一定规则

4、字节码传输使用了加密机制

5、运行环境提供保障机制：字节码校验器->类装载器->运行时内存布局->文件访问限制

6、不用程序员显示控制内存释放，JVM 有垃圾回收机制

### 38. Java针对不同的应用场景提供了哪些版本？

----

J2SE：Standard Edition(标准版) ，包含 Java 语言的核心类。如IO、JDBC、工具类、网络编程相关类等。从JDK 5.0开始，改名为Java SE。

J2EE：Enterprise Edition(企业版)，包含 J2SE 中的类和企业级应用开发的类。如web相关的servlet类、JSP、xml生成与解析的类等。从JDK 5.0开始，改名为Java EE。

J2ME：Micro Edition(微型版)，包含 J2SE 中的部分类，新添加了一些专有类。一般用设备的嵌入式开发，如手机、机顶盒等。从JDK 5.0开始，改名为Java ME。

### 39. 什么是JVM？

---

1、Java Virtual Machine（Java虚拟机）的缩写

2、实现跨平台的最核心的部分

3、.class 文件会在 JVM 上执行，JVM 会解释给操作系统执行

4、有自己的指令集，解释自己的指令集到 CPU 指令集和系统资源的调用

5、JVM 只关注被编译的 .class 文件，不关心 .java 源文件

### 40. 什么是JDK？

-----

1、Java Development Kit（Java 开发工具包）的缩写。用于 java 程序的开发，提供给程序员使用
2、使用 Java 语言编程都需要在计算机上安装一个 JDK
3、JDK 的安装目录 5 个文件夹、一个 src 类库源码压缩包和一些说明文件

- bin：各种命令工具， java 源码的编译器 javac、监控工具 jconsole、分析工具 jvisualvm 等
- include：与 JVM 交互C语言用的头文件
- lib：类库    
- jre：Java 运行环境 
- db：安装 Java DB 的路径

- src.zip：Java 所有核心类库的源代码
- jdk1.8 新加了 javafx-src.zip 文件，存放 JavaFX 脚本，JavaFX 是一种声明式、静态类型编程语言

### 41. 什么是JRE？

------

* Java Runtime Environment（Java运行环境）的缩写
* 包含 JVM 标准实现及 Java 核心类库，这些是运行 Java 程序的必要组件
* 是 Java 程序的运行环境，并不是一个开发环境，没有包含任何开发工具（如编译器和调试器）
* 是运行基于 Java 语言编写的程序所不可缺少的运行环境，通过它，Java 程序才能正常运行

### 42. JDK、JRE、JVM之间的关系是什么样的?

------

* JDK 是 JAVA 程序开发时用的开发工具包，包含 Java 运行环境 JRE
* JDk、JRE 内部都包含 JAVA虚拟机 JVM
* JVM 包含 Java 应用程序的类的解释器和类加载器等

### 43. Java语言有哪些注释的方式？

单行注释
多行注释，不允许嵌套
文档注释，常用于类和方法的注释

形式如下：

```javascript
package constxiong.interview;

/**
 * 文档注释
 * @author ConstXiong
 * @date 2019-10-17 12:32:31
 */
public class TestComments {
	
	/**
	 * 文档注释
	 * @param args 参数
	 */
	public static void main(String[] args) {
		//单行注释
		//System.out.print(1);
		
		/* 多行注释
		System.out.print(2);
		System.out.print(3);
		*/
	}

}
```

 ### 44. Java中有几种基本数据类型？它们分别占多大字节？

**基本数据类型**

- byte：1个字节，8位
- short：2个字节，16位
- int：4个字节，32位
- long：8个字节，64位
- float：4个字节，32位
- double：8个字节，64位
- boolean：官方文档未明确定义，依赖于 JVM 厂商的具体实现。逻辑上理解是占用 1位，但是实际中会考虑计算机高效存储因素
- char：2个字节，16位

 

补充说明：字节的英文是 byte，位的英文是 bit

 

详细说明可以参考：

- https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html

### 45. i++和++i的作用和区别

作用：都是给变量 i 加 1，相当于 i = i + 1; 

 

区别：

- i++ 先运算后加 1
- ++i 先加 1 再运算

```javascript
package constxiong.interview;

/**
 * 测试 ++i 和 i++
 * @author ConstXiong
 * @date 2019-10-17 13:44:05
 */
public class TestAdd {

	public static void main(String[] args) {
		int a = 3;
		int b = a++;
		System.out.println("a=" + a);
		System.out.println("b=" + b);
		
		int x = 3;
		int y = ++x;
		System.out.println("x=" + x);
		System.out.println("y=" + y);
	}
	
}
```

打印

```javascript
a=4
b=3
x=4
y=4
```

 ### 46. &和&&的作用和区别

&

- 逻辑与，& 两边的表达式都会进行运算
- 整数的位运算符

 

&&

- 短路与，&& 左边的表达式结果为 false 时，&& 右边的表达式不参与计算

```javascript
package constxiong.interview;

/**
 * 测试 & &&
 * @author ConstXiong
 */
public class TestAnd {

	public static void main(String[] args) {
		int x = 10;
		int y = 9;
		if (x == 9 & ++y > 9) {
		}
		System.out.println("x = " + x + ", y = " + y);
		
		int a = 10;
		int b = 9;
		if (a == 9 && ++b > 9) {//a == 9 为 false，所以 ++b 不会运算，b=9
		}
		System.out.println("a = " + a + ", b = " + b);
		
		//00000000000000000000000000000001
		//00000000000000000000000000000010
		//=
		//00000000000000000000000000000000
		System.out.println(1 & 2);//打印0
	}
	
}
```

打印

```javascript
x = 10, y = 10
a = 10, b = 9
0
```

### 47. |和||的作用和区别

|

- 逻辑或，| 两边的表达式都会进行运算
- 整数的或运算符

 

||

- 短路或，|| 左边的表达式结果为 true 时，|| 右边的表达式不参与计算

```javascript
package constxiong.interview;

/**
 * 测试 | ||
 * @author ConstXiong
 */
public class TestOr {

	public static void main(String[] args) {
		int x = 10;
		int y = 9;
		if (x == 10 | ++y > 9) {
		}
		System.out.println("x = " + x + ", y = " + y);
		
		int a = 10;
		int b = 9;
		if (a == 10 || ++b > 9) {//a == 10 为 true，所以 ++b 不会运算，b=9
		}
		System.out.println("a = " + a + ", b = " + b);
		
		/*
		00000000000000000000000000000001
		|
		00000000000000000000000000000010
		=
		00000000000000000000000000000011
		*/
		System.out.println(1 | 2);//打印3
	}
	
}
```

打印

```javascript
x = 10, y = 10
a = 10, b = 9
3
```

### 48. 如何让计算机最高效的算出2乘以8？

2 <<3

- 位运算符 <<，是将一个数左移 n 位，相当于乘以了 2 的 n 次方
- 一个数乘以 8 只要将其左移 3 位即可
- CPU 直接支持位运算，效率最高

 

补充：当这个数接近Java基本整数类型的最大值时，左移位运算可能出现溢出，得出负值。

### 49. Java中基本类型的转换规则？

---

**等级低到高:**  

- byte、short、int、long、float、double
- char、int、long、float、double

自动转换：运算过程中，低级可以自动向高级转换

强制转换：高级需要强制转换为低级，可能会丢失精度

**规则：**

- = 右边先自动转换成表达式中最高级的数据类型，再进行运算。整型经过运算会自动转化最低 int 级别，如两个 char 类型的相加，得到的是一个 int 类型的数值。
- = 左边数据类型级别 大于 右边数据类型级别，右边会自动升级
- = 左边数据类型级别 小于 右边数据类型级别，需要强制转换右边数据类型
- char 与 short，char 与 byte 之间需要强转，因为 char 是无符号类型

### 55. 可变参数的作用和特点是什么？

**作用：**

在不确定参数的个数时，可以使用可变参数。

**语法：**参数类型...

**特点：**

- 每个方法最多只有一个可变参数
- 可变参数必须是方法的最后一个参数
- 可变参数可以设置为任意类型：引用类型，基本类型
- 参数的个数可以是 0 个、1 个或多个
- 可变参数也可以传入数组
- 无法仅通过改变 可变参数的类型，来重载方法
- 通过对 class 文件反编译可以发现，可变参数被编译器处理成了数组

```java
    @Test
    public void test(){
        change(1, 'c', "fdsf", "fsad");
    }
    public void change(int a, char b, String... c){
        System.out.println(a);
        System.out.println(b);
        System.out.println(Arrays.toString(c));
    }
1
c
[fdsf, fsad]
```

### 56. 类和对象的关系？

---

类是对象的抽象；对象是类的具体实例

类是抽象的，不占用内存；对象是具体的，占用存储空间

类是一个定义包括在一类对象中的方法和变量的模板

### 57.说一说你的对面向过程和面向对象的理解

---

- 软件开发思想，先有面向过程，后有面向对象
- 在大型软件系统中，面向过程的做法不足，从而推出了面向对象
- 都是解决实际问题的思维方式
- 两者相辅相成，宏观上面向对象把握复杂事物的关系；微观上面向过程去处理
-  面向过程以实现功能的函数开发为主；面向对象要首先抽象出类、属性及其方法，然后通过实例化类、执行方法来完成功能
- 面向过程是封装的是功能；面向对象封装的是数据和功能
- 面向对象具有继承性和多态性；面向过程则没有

### 58. 重写与重载

---

**重写：**在子类中将父类的成员方法的名称保留，重新编写成员方法的实现内容，更改方法的访问权限，修改返回类型的为父类返回类型的子类。

**一些规则：**
重写发生在子类继承父类
参数列表必须完全与被重写方法的相同
重写父类方法时，修改方法的权限只能从小范围到大范围
返回类型与被重写方法的返回类型可以不相同，但是必须是父类返回值的子类(JDK1.5 及更早版本返回类型要一样，JDK1.7 及更高版本可以不同)
访问权限不能比父类中被重写的方法的访问权限更低。如：父类的方法被声明为 public，那么子类中重写该方法不能声明为 protected
重写方法不能抛出新的检查异常和比被重写方法申明更宽泛的异常(即只能抛出父类方法抛出异常的子类)
声明为 final 的方法不能被重写
声明为 static 的方法不能被重写
声明为 private 的方法不能被重写

 

**重载：**一个类中允许同时存在一个以上的同名方法，这些方法的参数个数或者类型不同

**重载条件：**

- 方法名相同
- 参数类型不同 或 参数个数不同 或 参数顺序不同

**规则：**
被重载的方法**参数列表(个数或类型)不一样
被重载的方法可以修改返回类型
被重载的方法可以修改访问修饰符
被重载的方法可以修改异常抛出
方法能够在同一个类中或者在一个子类中被重载
无法以返回值类型作为重载函数的区分标准**

 

**重载和重写的区别：**

- 作用范围：重写的作用范围是父类和子类之间；重载是发生在一个类里面
- 参数列表：重载必须不同；重写不能修改
- 返回类型：重载可修改；重写方法返回相同类型或子类
- 抛出异常：重载可修改；重写可减少或删除，一定不能抛出新的或者更广的异常
- 访问权限：重载可修改；重写一定不能做更严格的限制

### 59.this和super关键字的作用

***

**this：**

- 对象内部指代自身的引用
- 解决成员变量和局部变量同名问题
- 可以调用成员变量
- 不能调用局部变量
- 可以调用成员方法
- 在普通方法中可以省略 this
- 在静态方法当中不允许出现 this 关键字

**super：**

- 代表对当前对象的直接父类对象的引用
- 可以调用父类的非 private 成员变量和方法
- super(); 可以调用父类的构造方法，只限构造方法中使用，且必须是第一条语句

### 60. static关键字的作用是什么

- static 可以修饰变量、方法、代码块和内部类
- static 变量是这个类所有，由该类创建的所有对象共享同一个 static 属性
- 可以通过创建的对象名.属性名 和 类名.属性名两种方式访问
- static 变量在内存中只有一份
- static 修饰的变量只能是类的成员变量
- static 方法可以通过对象名.方法名和类名.方法名两种方式来访问
- static 代码块在类被第一次加载时执行静态代码块，且只被执行一次，主要作用是实现 static 属性的初始化
- static 内部类属于整个外部类，而不属于外部类的每个对象，只可以访问外部类的静态变量和方法

### 61. abstract关键字的作用是什么

- 可以修饰类和方法
- 不能修饰属性和构造方法
- abstract 修饰的类是抽象类，需要被继承
- abstract 修饰的方法是抽象方法，需要子类被重写

### 63. 子类构造方法的执行过程是什么样的？

子类构造方法的调用规则：

- 如果子类的构造方法中没有通过 super 显式调用父类的有参构造方法，也没有通过 this 显式调用自身的其他构造方法，则系统会默认先调用父类的无参构造方法。这种情况下，写不写 super(); 语句，效果是一样的
- 如果子类的构造方法中通过 super 显式调用父类的有参构造方法，将执行父类相应的构造方法，不执行父类无参构造方法
- 如果子类的构造方法中通过 this 显式调用自身的其他构造方法，将执行类中相应的构造方法
- 如果存在多级继承关系，在创建一个子类对象时，以上规则会多次向更高一级父类应用，一直到执行顶级父类 Object 类的无参构造方法为止

### 64. 什么是Java的多态？

**实现多态的三个条件**

- 继承的存在。继承是多态的基础，没有继承就没有多态
- 子类重写父类的方法，JVM 会调用子类重写后的方法
- 父类引用变量指向子类对象

**向上转型：将一个父类的引用指向一个子类对象，自动进行类型转换。**

- 通过父类引用变量调用的方法是子类覆盖或继承父类的方法，而不是父类的方法。
- 通过父类引用变量无法调用子类特有的方法。

**向下转型：将一个指向子类对象的引用赋给一个子类的引用，必须进行强制类型转换。**

- 向下转型必须转换为父类引用指向的真实子类类型，不是任意的强制转换，否则会出现 ClassCastException
- 向下转型时可以结合使用 instanceof 运算符进行判断

### 65. instanceof关键字的作用是什么？

instanceof 运算符是用来在运行时判断对象是否是指定类及其父类的一个实例。
比较的是对象，不能比较基本类型

### 66. 什么是Java的垃圾回收机制？

**垃圾回收机制，简称 GC**

- Java 语言不需要程序员直接控制内存回收，由 JVM 在后台自动回收不再使用的内存
- 提高编程效率
- 保护程序的完整性
- JVM 需要跟踪程序中有用的对象，确定哪些是无用的，影响性能

**特点**

- 回收 JVM 堆内存里的对象空间，不负责回收栈内存数据
- 无法处理一些操作系统资源的释放，如数据库连接、输入流输出流、Socket 连接
- 垃圾回收发生具有不可预知性，程序无法精确控制垃圾回收机制执行
- 可以将对象的引用变量设置为 null，垃圾回收机制可以在下次执行时回收该对象。
- JVM 有多种垃圾回收 实现算法，表现各异
- 垃圾回收机制回收任何对象之前，会先调用对象的 finalize() 方法
- 可以通过 System.gc() 或 Runtime.getRuntime().gc() 通知系统进行垃圾回收，会有一些效果，但系统是否进行垃圾回收依然不确定
- 不要主动调用对象的 finalize() 方法，应该交给垃圾回收机制调用

### 67. 什么是包装类？为什么要有包装类？基本类型与包装类如何转换？

**Java 中有 8 个基本类型，分别对应的包装类如下**

- byte -- Byte
- boolean -- Boolean
- short -- Short
- char -- Character
- int -- Integer
- long -- Long
- float -- Float
- double -- Double

**为什么要有包装类**

- 基本数据类型方便、简单、高效，但泛型不支持、集合元素不支持
- 不符合面向对象思维
- 包装类提供很多方法，方便使用，如 Integer 类 toHexString(int i)、parseInt(String s) 方法等等

**基本数据类型和包装类之间的转换**

- 包装类-->基本数据类型：包装类对象.xxxValue() 
- 基本数据类型-->包装类：new 包装类(基本类型值)
- JDK1.5 开始提供了自动装箱（autoboxing）和自动拆箱（autounboxing）功能, 实现了包装类和基本数据类型之间的自动转换
- 包装类可以实现基本类型和字符串之间的转换，字符串转基本类型：parseXXX(String s)；基本类型转字符串：String.valueOf(基本类型)

### 68. 基本类型和包装类的区别？

- 基本类型只有值，而包装类型则具有与它们的值不同的同一性（即值相同但不是同一个对象）
- 包装类型比基本类型多了一个非功能值：null
- 基本类型通常比包装类型更节省时间和空间，速度更快
- 但有些情况包装类型的使用会更合理：

1. 泛型不支持基本类型，作为集合中的元素、键和值直接使用包装类（否则会发生基本类型的自动装箱消耗性能）。如：只能写 ArrayList<Integer>，不能写 List<int>
2. 在进行反射方法的调用时

补充：两者之间的转换存在自动装/拆箱，可以提一下。

### 69. java.sql.Date和java.util.Date的区别

- java.sql.Date 是 java.util.Date 的子类
- java.util.Date 是 JDK 中的日期类，精确到时、分、秒、毫秒
- java.sql.Date 与数据库 Date 相对应的一个类型，只有日期部分，时分秒都会设置为 0，如：2019-10-23 00:00:00
- 要从数据库时间字段取 时、分、秒、毫秒数据，可以使用 java.sql.Timestamp

### 70. 关于Java编译，下面哪一个正确()

- A、Java程序经编译后产生机器码
- B、Java程序经编译后会生产字节码
- C、Java程序经编译后会产生 DLL 文件
- D、以上都不正确

---

**答案：B**

**分析：**

- .java 源码代码经编译后会生成 .class 字节码，通过 JVM 翻译成机器码去执行
- DLL 文件是 C/C++ 语言编译的动态链接库

### 71. 关于构造方法，下列说法正确的是()

---

- A、类中的构造方法不可省略
- B、构造方法可以与类同名，但方法不能与类同名
- C、构造方法在一个对象被 new 时执行
- D、一个类只能定义一个构造方法

**答案：C**

**分析：**

Java 类中不写构造方法，编译器会默认提供一个无参构造

方法名可以与类名相同，但不符合命名规范，类名首字母建议大写，方法名建议首字母小写

一个类中可以定义多个构造方法，这就是构造方法的重载

### 72. Java中接口的修饰符可以是()

- A、private
- B、protected
- C、abstract
- D、final

**答案：C**

**分析：**

- 接口中的访问权限修饰符只能是 public 或 default
- 接口中的方法必须要实现类实现，所以不能使用 final
- 接口中所有的方法默认都是 abstract，通常 abstract 省略不写

### 73. 以下代码将输出()

------

- A、不能通过编译
- B、通过编译，输出A
- C、通过编译，输出AA
- D、通过编译，输出AAA 

```javascript
public class AA extends A{
    public AA(){
        System.out.print("AA");
    }

    public static void main(String[] args) {
        AA aa = new AA();
    }
}

class A {
    public A(){
        System.out.print("A");
    }
}
```

**答案：D**

**分析：**
创建子类对象，先执行父类的构造方法，再执行子类的构造方法

### 74. 关于关键字的使用说法错误的是()

------

- A、static 方法能处理非 static 的属性
- B、abstract 方法必须在 abstract 类中
- C、abstract 类中可以有 private 的成员
- D、abstract 不能与 final 并列修饰同一个类

**答案：A**

**分析：**
加载 class 时首先完成 static 方法装载，非 static 属性和方法还没有完成初始化，所以不能调用。

### 75. 关于内存回收正确的是()

------

- A、内存回收程序负责释放无用内存
- B、必须创建一个线程来释放内存
- C、内存回收程序可以在指定的时间释放内存对象
- D、内存回收程序允许程序员直接释放内存

**答案：A**

**分析：**

- 内存由 JVM 负责释放
- 程序员无法直接释放内存
- 垃圾回收时间不确定

### 76. 哪些标识符合法？

------

- A、class
- B、$change
- C、1 nil
- D、_sysl_111

**答案：BD**

**分析：**

标识符的命令规范

- 可以包含字母、数字、下划线、$
- 不能以数字开头
- 不能是 Java 关键字

### 77. 说法正确的是()

------

- A、java.lang.Runnable 是接口
- B、java.lang.Cloneable 是类
- C、Double 对象在 java.lang 包中
- D、Double a = 1.0 是正确的java语句

**答案：ACD**

**分析：**

java.lang.Cloneable 是接口

### 78. 定义一个Java类，可被所有类访问，申明正确的是()

------

- A、public class A extends Object
- B、class A extends Object
- C、protected class A
- D、public class A

**答案：AD**

**分析：**

- 无权限修饰符的类，只能在同包中访问，所以 B 不正确
- 类的访问权限修饰符只能是 public 和 default，所以 C 不正确

### 79. 说说你对面向对象的理解

---

对 Java 语言来说，一切皆是对象。

对象有以下特点：

- 对象具有属性和行为
- 对象具有变化的状态
- 对象具有唯一性
- 对象都是某个类别的实例
- 一切皆为对象，真实世界中的所有事物都可以视为对象

 

面向对象的特性：

- 抽象性：抽象是将一类对象的共同特征总结出来构造类的过程，包括数据抽象和行为抽象两方面。
- 继承性：指子类拥有父类的全部特征和行为，这是类之间的一种关系。Java 只支持单继承。
- 封装性：封装是将代码及其处理的数据绑定在一起的一种编程机制，该机制保证了程序和数据都不受外部干扰且不被误用。封装的目的在于保护信息。
- 多态性：多态性体现在父类的属性和方法被子类继承后或接口被实现类实现后，可以具有不同的属性或表现方式。

### 80. 内存泄漏和内存溢出的区别

---

- 内存溢出(out of memory)：指程序在申请内存时，没有足够的内存空间供其使用，出现 out of memory。
- 内存泄露(memory leak)：指程序在申请内存后，无法释放已申请的内存空间，内存泄露堆积会导致内存被占光。
- memory leak 最终会导致 out of memory。

  

### 81. 不通过构造方法能创建对象吗？

---

**Java 创建对象的方式：**

1. 用 new 语句创建对象
2. 运用反射，调用 java.lang.Class 或 java.lang.reflect.Constructor 类的 newInstance() 方法
3. 调用对象的 clone() 方法
4. 运用反序列化手段，调用 java.io.ObjectInputStream 对象的 readObject() 方法

1、2 会调用构造函数
3、4 不会调用构造函数

```javascript
package constxiong.interview;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

/**
 * 测试创建对象
 * @author ConstXiong
 * @date 2019-10-31 11:53:31
 */
public class TestNewObject implements Cloneable, Serializable{
	
	private static final long serialVersionUID = 1L;

	public TestNewObject() {
		System.out.println("Constructor init");
	}
	
	public static void main(String[] args) throws Exception {
		TestNewObject o1 = new TestNewObject();
		TestNewObject o2 = TestNewObject.class.newInstance();
		TestNewObject o3 = TestNewObject.class.getConstructor().newInstance();
		
		TestNewObject o4 = (TestNewObject)o1.clone();
		
		ByteArrayOutputStream baos = new ByteArrayOutputStream();
		ObjectOutputStream oos = new ObjectOutputStream(baos);
		oos.writeObject(o1);
		ObjectInputStream ois = new ObjectInputStream(new ByteArrayInputStream(baos.toByteArray()));
		TestNewObject o5 = (TestNewObject)ois.readObject();
	}

}
```

打印

```javascript
Constructor init
Constructor init
Constructor init
```

### 82. 匿名内部类可以继承类或实现接口吗？为什么？

---

- 匿名内部类本质上是对父类方法的重写 或 接口的方法的实现
- 从语法角度看，匿名内部类创建处是无法使用关键字继承类 或 实现接口

 

**原因：**

- 匿名内部类没有名字，所以它没有构造函数。因为没有构造函数，所以它必须通过父类的构造函数来实例化。即匿名内部类完全把创建对象的任务交给了父类去完成。
- 匿名内部类里创建新的方法没有太大意义，新方法无法被调用。
- 匿名内部类一般是用来覆盖父类的方法。
- 匿名内部类没有名字，所以无法进行向下的强制类型转换，只能持有匿名内部类对象引用的变量类型的直接或间接父类。

### 83. 什么是多态？如何实现？有什么好处？

**多态：**
同一个接口，使用不同的实例而执行不同操作。同一个行为具有多个不同表现形式或形态的能力。

**实现多态有三个条件：**

- 继承
- 子类重写父类的方法
- 父类引用变量指向子类对象

实现多态的技术称为：动态绑定(dynamic binding)，是指在执行期间判断所引用对象的实际类型，根据其实际的类型调用其相应的方法。 

Java 中使用父类的引用变量调用子类重写的方法，即可实现多态。

**好处：**

- 消除类型之间的耦合关系(本来赋值左右两边是同一类型才可以，现在可以用父类引用子类)
- 可替换性(substitutability)
- 可扩充性(extensibility)
- 接口性(interface-ability)
- 灵活性(flexibility)
- 简化性(simplicity)

### 84. Java中关于继承，错误的是()

------

- A、当实例化子类时会递归调用父类中的构造方法
- B、父类更具通用性，子类更具体
- C、继承存在着传递性
- D、继承允许一个子类继承多个父类

**答案：D**

**分析：**

Java是单继承的，一个类只能继承一个父类。

### 85. Math.random()的返回值是多少？

------

greater than or equal to 0.0 and less than 1.0

### 86. 同步代码块和同步方法有什么区别？

---

- 同步方法就是在方法前加关键字 synchronized；同步代码块则是在方法内部使用 synchronized
- 加锁对象相同的话，同步方法锁的范围大于等于同步方法块。一般加锁范围越大，性能越差
- 同步方法如果是 static 方法，等同于同步方法块加锁在该 Class 对象上

### 87. 静态内部类和非静态内部类有什么区别？

- 静态内部类不需要有指向外部类的引用；非静态内部类需要持有对外部类的引用
- 静态内部类可以有静态方法、属性；非静态内部类则不能有静态方法、属性
- 静态内部类只能访问外部类的静态成员，不能访问外部类的非静态成员；非静态内部类能够访问外部类的静态和非静态成员
- 静态内部类不依赖于外部类的实例，直接实例化内部类对象；非静态内部类通过外部类的对象实例生成内部类对象

### 88. 下列运算符合法的是()

------

- A、<>
- B、&&
- C、if
- D、=

**答案：BD**

**分析：**

- <> 在某些语言中表示不等于，但 Java中 不能这么使用
- && 是逻辑运算符中的短路与
- if 是条件判断符号，不是运算符
- = 是赋值运算符

### 89. 打印值是多少？

------

- A、-1
- B、0
- C、1
- D、死循环

```javascript
package constxiong.interview;

public class TestDoWhile {

	public static void main(String[] args) {
		int a = 0;
		int b = 0;
		
		do{
			--b;
			a = a - 1;
		} while (b > 0);
		
		System.out.println(b);
	}
}
```

**答案：A**

**分析：**

- do while 循环是先执行后判断
- 代码先执行 --b 操作，b = -1
- 之后执行 a=a-1，a 为 -1
- 然后判断 b 是否大于 0 ，条件不成立，退出循环
- b 输出 -1

### 90. 关于抽象，正确的是()

------

- A、abstract 修饰符可修饰字段、方法和类
- B、声明抽象方法不可写出大括号
- C、声明抽象方法，大括号可有可无
- D、抽象方法的 body 部分必须用一对大括号包住

**答案：B**

**分析：**

- abstract 只能修饰方法和类，不能修饰字段
- 抽象方法不能有方法体，即没有括号

### 91. 正确的是()

------

- A、实例方法可直接调用超类的实例方法
- B、实例方法可直接调用超类的类方法
- C、实例方法可直接调用本类的类方法
- D、实例方法可直接调用其他类的实例方法

**答案：C**

**分析：**

- 子类无法调用超类的私方法
- 调用其他类的方法，要看方法的修饰符、两个类的关系和包路径

### 92. 正确的是()

------

- A、环境变量可在编译 source code 时指定
- B、在编译程序时，所指定的环境变置不包括 class path
- C、javac —次可同时编译数个 Java 源文件
- D、javac.exe 能指定编译结果要置于哪个目录

**答案：CD**

**分析：**

- java_home 无法再编译时指定，最多在命令行，让操作系统直接找到可执行程序
- 编译程序时，环境变量包括 java_home 和 class path
- javac.exe -d 参数可指定生成类文件的位置

### 93. 错误的是()

------

- A、数组属于一种基本数据类型
- B、数组是—种对象
- C、int num[]=(1,2,3,4)
- D、数组的长度可以任意改变

**答案：ACD**

**分析：**

- Java中的基本数据类型有 8 种，没有数组
- C、语法错误，应该用 {}
- D、数组的长度一旦确定就不能修改

### 94. 哪些不能修饰 interface

------

- A、public
- B、private
- C、protected
- D、static

**答案：BCD**

**分析：**
只有 public、abstract和默认的 3 种修饰符能够修饰 interface

### 95. 正确的是()

------

- A、call by value 不会改变实际参数的数值
- B、call by reference 能改变实际参数的参考地址
- C、call by reference 不能改变实际参数的参考地址
- D、call by reference 能改变实际参数的内容

**答案：ACD**

**分析：**
首先了解下 Java 中参数的传递有两种

- call by value：传递的是具体的值，基础数据类型就是这种类型
- call by reference：传递的是对象的引用，即对象的存储地址

call by value 不能改变实参的数值
call by reference 不能改变实参的参考地址，但可以访问和改变地址中的内容

### 96. 存在i+1< i的数吗？为什么？

----

存在

```javascript
package constxiong.interview;

/**
 * 测试最大值加1
 * @author ConstXiong
 */
public class TestMaxValueAddOne {

	public static void main(String[] args) {
		int i = Integer.MAX_VALUE;
		System.out.println(i+1<i);
		System.out.println(i+1);
	}
	
}
```

打印

```javascript
true
-2147483648
```

### 97. 接口可否继承接口？抽象类是否可实现接口？抽象类是否可继承实体类？

---

都可以

### 98. 可序列化对象为什么要定义serialversionUID值?

------

参考答案

- SerialVersionUid 是为了序列化对象版本控制，告诉 JVM 各版本反序列化时是否兼容
- 如果在新版本中这个值修改了，新版本就不兼容旧版本，反序列化时会抛出InvalidClassException异常
- 仅增加了一个属性，希望向下兼容，老版本的数据都保留，就不用修改
- 删除了一个属性，或更改了类的继承关系，就不能不兼容旧数据，这时应该手动更新 SerialVersionUid

### 99. 十进制100转换成八进制是多少？

----

100 = 1\*(8\*8) + 4\*(8\*1) + 4*1 

八进制：144

Java中八进制数必须以0开头，0144

### 100. Class类的getDeclaredFields()与getFields()方法的区别？

- getDeclaredFields(): 获取所有本类自己声明的属性, 不能获取父类和实现的接口中的属性
- getFields(): 只能获取所有 public 声明的属性, 包括获取父类和实现的接口中的属性



### 101. 解释以下正则表达式的含义

------

- \d
- \D
- \s
- .
- *
- ?
- |
- +
- [0-9]{2}

- \d  匹配一个数字字符，等价于[0-9]
- \D  匹配一个非数字字符，等价于[^0-9]
- \s  匹配任何空白字符，包括空格、制表符、换页符等，等价于 [ \f\n\r\t\v]
- .  匹配除换行符 \n 之外的任何单字符，匹配 . 字符需要转译，使用 \. 
- \*  匹配前面的子表达式零或多次，匹配 * 字符，需要转译使用 \*
- 匹配前面子表达式零或一次，或表示指明表达式为非贪婪模式的限定符。匹配 ? 字符，需要转译使用 \?
- |  将两个匹配条件进行逻辑 或 运算
- \+  匹配前面的子表达式一次或多次，要匹配 + 字符需要转译，使用 \+。
- [0-9]{6}  匹配连续6个0-9之间的数字

### 102. 声明合法的是()

------

- A、long l = 1234
- B、int i = 1234L
- C、float f = 12.34
- D、double d = 12.34

**答案：AD**

**分析：**

- int 类型申明不需要在值后面加字母，如 int = 4
- float 类型申明需要在值后面加字母 f 或 F，如 float f = 12.34f

### 103. 面打印结果是？

------

```javascript
int i = 5;
switch (i) {
default:
	System.out.println("default");
case 0:
	System.out.println(0);
	break;
case 1:
	System.out.println(1);
	break;
case 2:
	System.out.println(2);
	break;
}
```

```javascript
default
0
```

### 104. Java属于编译型还是解释型语言

---

计算机不能直接理解高级语言，只能理解和运行机器语言。必须要把高级语言翻译成机器语言，计算机才能运行高级语言所编写的程序。
翻译的方式有两种，一个是编译，一个是解释。

用编译型语言写的程序执行之前，需要一个专门的编译过程，通过编译系统把高级语言翻译成机器语言，把源高级程序编译成为机器语言文件，以后直接运行而不需要再编译了，所以一般编译型语言的程序执行效率高。

解释型语言在运行的时候才解释成机器语言，每个语句都是执行时才翻译。每执行一次就要翻译一次，效率较低。

Java 是一种兼具编译和解释特性的语言，.java 文件会被编译成与平台无关的 .class 文件，但是 .class 字节码文件无法被计算机直接，仍然需要 JVM 进行翻译成机器语言。
所以严格意义上来说，Java 是一种解释型语言。

### 105. 如果有两个类A、B（注意不是接口），如何编写C类同时使用这两个类的功能？

让A、B成为父子类，C继承子类即可。

### 106. 构造方法是否可以被重载？重写？

构造方法可以被重载

构造方法不可以被重写

### 107. 基本类型byte表示的数值范围是多少？

-128 至 127

### 108. 日期类型如何格式化？字符串如何转日期？

---

```java
//日期格式为字符串
DateFormat  sdf = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
String s = sdf.format(new Date());
```

```java
//字符串转日期
DateFormat  sdf = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
String s = "2019-10-31 22:53:10";
Date date = sdf.parse(s);
```

### 109. 当输入为2的时候返回值是

------

```javascript
	public static int cal(int i) {
		int result = 0;
		switch (i) {
		case 1:
			result = result + i;
		case 2:
			result = result + i * 2;
		case 3:
			result = result + i * 3;
		}
		return result;
	}
```

 10

### 110. System.out.println('a'+1);的结果是

98

### 111. 静态与非静态成员变量区别？

- 生命周期不同：非静态成员变量随着对象的创建而存在；静态成员变量随着类的加载而存在
- 调用方式不同：非静态成员变量用 对象名.变量名 调用；静态成员变量用 类名.变量名，JDK1.7以后也能用对象名.变量名调用
- 别名不同：非静态成员变量也称为实例变量；静态变量称为类变量
- 数据存储位置不同：成员变量数据存储在堆内存的对象中，对象的特有数据；静态变量数据存储在方法区(共享数据区)的静态区，对象的共享数据

### 112. 二进制数，小数点向右移一位，值会发生什么变化？

相当于乘以 2

如，1.1 = 1 * 2^0 + 1 * 2^-1 = 1.5

小数点向右移 1 位为 11， 1 * 2^1 + 1 * 2^0 = 3

### 113. 下面两段代码的区别是？

------

```javascript
short s1 = 1; s1 = s1 + 1;
short s1 = 1; s1 += 1;
```

第一段编译报错，s1 + 1自动升级为 int 型，int 型赋值给 s1，需要手动强转

第二段隐含类型强转，不会报错

 ### 114. switch能否作用在byte、long、String上？

- 早期 JDK，switch(expr)，expr 可以是 byte、short、char、int
- JDK 1.5 开始，引入了枚举(enum)，expr 也可以是枚举
- JDK 1.7 开始，expr 还可以是字符串(String)
- 长整型(long)是不可以的

 ### 115. 在Java 中，如何跳出当前的多重嵌套循环？

使用标签标注循环，使用 break 标签即可。

```javascript
package constxiong.interview;

/**
 * 跳出多重循环
 * @author ConstXiong
 */
public class TestBreakMulti {

	public static void main(String[] args) {
		A:for (int i = 0; i <10; i++) {
			for (int j = 0; j <10; j++) {
				System.out.println(j);
				if (j == 5) {
					break A;
				}
			}
			
		}
	}
	
}
```

打印

```javascript
0
1
2
3
4
5
```

### 116. 为什么不能根据返回类型来区分方法重载？

同时方法的重载只是要求两同三不同

- 在同一个类中
- 相同的方法名称
- 参数列表中的参数类型、个数、顺序不同
- 跟权限修饰符和返回值类型无关

如果可以根据返回值类型来区分方法重载，那在仅仅调用方法不获取返回值的使用场景，JVM 就不知道调用的是哪个返回值的方法了。

### 117. Inner Class和Static Nested Class的区别？

**Inner Class：内部类**

- 内部类就是在一个类的内部定义的类
- 内部类中不能定义静态成员
- 内部类可以直接访问外部类中的成员变量
- 内部类可以定义在外部类的方法外面，也可以定义在外部类的方法体中
- 在方法体外面定义的内部类的访问类型可以是 public, protected , 默认的，private 等 4 种类型
- 方法内部定义的内部类前面不能有访问类型修饰符，可以使用 final 或 abstract 修饰符
- 创建内部类的实例对象时，一定要先创建外部类的实例对象，然后用这个外部类的实例对象去创建内部类的实例对象
- 内部类里还包含匿名内部类，即直接对类或接口的方法进行实现，不用单独去定义内部类

```javascript
//内部类的创建语法
Outer outer = new Outer();
Outer.Inner inner = outer.new Innner();
```

**Static Nested Class：静态嵌套类**

- 不依赖于外部类的实例对象
- 不能直接访问外部类的非 static 成员变量
- 可以直接引用外部类的static的成员变量，不需要加上外部类的名字
- 在静态方法中定义的内部类也是Static Nested Class

```javascript
//静态内部类创建语法
Outter.Inner inner = new Outter.Inner();
```

 ### 118 . final修饰变量，是引用不能变？还是引用的对象不能变？

- final 修饰基本类型变量，值不能改变
- final 修饰引用类型变量，栈内存中的引用不能改变，所指向的堆内存中的对象的属性值可能可以改变

### 119. abstract方法是否可是static的？native的？synchronized的?

都不能

- 抽象方法需要子类重写，而静态的方法是无法被重写的
- 本地方法是由本地动态库实现的方法，而抽象方法是没有实现的
- 抽象方法没有方法体；synchronized 方法，需要有具体的方法体，相互矛盾

### 120. 静态方法内部能对非静态调用吗？

不能

- 静态方法只能访问静态成员
- 调用静态方法时可能对象并没有被初始化，此时非静态变量还未初始化
- 非静态方法的调用和非静态成员变量的访问要先创建对象

### 121. 内部类可以引用它的外部类的成员吗？有什么限制？

- 内部类对象可以访问创建它的外部类对象的成员，包括私有成员
- 访问外部类的局部变量，此时局部变量必须使用 final 修饰

### 122. 打印结果是什么

------

```javascript
package constxiong.interview;

/**
 * 测试父类、子类构造函数打印
 * 
 * @author ConstXiong
 * @date 2019-11-01 09:30:26
 */
public class TestConstructorPrint {

	public static void main(String[] args) {
		Parent parent = new Child();
		parent = new Child();
	}

}

class Parent {
	static {
		System.out.print("1");
	}

	public Parent() {
		System.out.print("2");
	}
}

class Child extends Parent {
	static {
		System.out.print("3");
	}

	public Child() {
		System.out.print("4");
	}
}
```

 打印：132424

创建对象时构造器的调用顺序

- 递归初始化父类静态成员和静态代码块，上层优先
- 初始化本类静态成员和静态代码块
- 递归父类构造器，上层优先
- 调用自身构造器

### 123. 说说字符串与基本数据之间的转换

**字符串转基本数据**

- 基本数据类型的包装类中的 parseXXX(String)可以字符串转基本类型
- valueOf(String) 可以字符串转基本类型的包装类

**基本数据转字符串**

- 基本数据类型与空字符串 "" 用 + 连接即可获得基本类型的字符串
- 调用 String 类中的 valueOf(…) 方法返回相应字符串 

### 124. GB2312编码的字符串如何转换为ISO-8859-1编码？

```javascript
package constxiong.interview;

import java.io.UnsupportedEncodingException;

/**
 * 字符串字符集转换
 * @author ConstXiong
 * @date 2019-11-01 10:57:34
 */
public class TestCharsetConvert {

	public static void main(String[] args) throws UnsupportedEncodingException {
		String str = "爱编程";
		String strIso = new String(str.getBytes("GB2312"), "ISO-8859-1");
		System.out.println(strIso);
	}
}
```

### 125. Java中的日期与时间获取与转换？

------

- 如何取得年、月、日、时、分、秒、毫秒、纳秒？
- 如何取得从1970年1月1日0时0分0秒到现在的毫秒数？
- 如何取得某月的最后一天？
- 如何格式化日期？

- DK1.8 之前，使用 java.util.Calendar
- JDK1.8 提供了 java.time 包
- 还有第三方时间类库 Joda-Time 也可以

```javascript
package constxiong.interview;

import java.text.SimpleDateFormat;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.MonthDay;
import java.time.Year;
import java.time.format.DateTimeFormatter;
import java.util.Calendar;
import java.util.Date;

/**
 * 测试时间和日期
 * @author ConstXiong
 * @date 2019-11-01 11:05:59
 */
public class TestDateAndTime {

	public static void main(String[] args) {
		//获取当前的年、月、日、时、分、秒、毫秒、纳秒
		//年
		System.out.println(Calendar.getInstance().get(Calendar.YEAR));
		//JDK 1.8 java.time 包
		System.out.println(Year.now());
		System.out.println(LocalDate.now().getYear());
		
		//月
		System.out.println(Calendar.getInstance().get(Calendar.MONTH)+1);
		//JDK 1.8 java.time 包
		System.out.println(MonthDay.now().getMonthValue());
		System.out.println(LocalDate.now().getMonthValue());
		
		//日
		System.out.println(Calendar.getInstance().get(Calendar.DAY_OF_MONTH));
		//JDK 1.8 java.time 包
		System.out.println(MonthDay.now().getDayOfMonth());
		System.out.println(LocalDate.now().getDayOfMonth());
		
		//时
		System.out.println(Calendar.getInstance().get(Calendar.HOUR_OF_DAY));
		//JDK 1.8 java.time 包
		System.out.println(LocalTime.now().getHour());
		
		//分
		System.out.println(Calendar.getInstance().get(Calendar.MINUTE));
		//JDK 1.8 java.time 包
		System.out.println(LocalTime.now().getMinute());
		
		//秒
		System.out.println(Calendar.getInstance().get(Calendar.SECOND));
		//JDK 1.8 java.time 包
		System.out.println(LocalTime.now().getSecond());
		
		//毫秒
		System.out.println(Calendar.getInstance().get(Calendar.MILLISECOND));
		
		//纳秒
		System.out.println(LocalTime.now().getNano());
		
		
		//当前时间毫秒数
		System.out.println(System.currentTimeMillis());
		System.out.println(Calendar.getInstance().getTimeInMillis());
		
		
		//某月最后一天
		//2018-05月最后一天,6月1号往前一天
		Calendar c = Calendar.getInstance();
		c.set(Calendar.YEAR, 2018);
		c.set(Calendar.MONTH, 5);
		c.add(Calendar.DAY_OF_MONTH, -1);
		System.out.println(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH)+1) + "-" + c.get(Calendar.DAY_OF_MONTH));
		//JDK 1.8 java.time 包
		LocalDate date = LocalDate.of(2019, 6, 1).minusDays(1);
		System.out.println(date.getYear() + "-" + date.getMonthValue() + "-" + date.getDayOfMonth());
		
		
		//格式化日期
		System.out.println(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date()));
		//JDK 1.8 java.time 包
		System.out.println(LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
	}
}
```

### 126. 反射主要实现类有哪些？

在JDK中，主要由以下类来实现 Java 反射机制，除了 Class 类，一般位于 java.lang.reflect 包中

- java.lang.Class ：一个类
- java.lang.reflect.Field ：类的成员变量(属性)
- java.lang.reflect.Method ：类的成员方法
- java.lang.reflect.Constructor ：类的构造方法
- java.lang.reflect.Array ：提供了静态方法动态创建数组，访问数组的元素

### 127. Class类的作用是什么？如何获取Class对象？

Class 类是 Java 反射机制的起源和入口，用于获取与类相关的各种信息，提供了获取类信息的相关方法。
Class 类存放类的结构信息，能够通过 Class 对象的方法取出相应信息：类的名字、属性、方法、构造方法、父类、接口和注解等信息

- 对象名.getClass()   
- 对象名.getSuperClass()
- Class.forName("oracle.jdbc.driver.OracleDriver");
- 类名.class

```javascript
Class c2 = Student.class;
Class c2 = int.class
```

- 包装类.TYPE

```javascript
Class c2 = Boolean.TYPE;
```

- Class.getPrimitiveClass()

```javascript
(Class<Boolean>)Class.getPrimitiveClass("boolean");
```

### 128. 面向对象设计原则有哪些？

- 单一职责原则 SRP
- 开闭原则 OCP
- 里氏替代原则 LSP
- 依赖注入原则 DIP
- 接口分离原则 ISP
- 迪米特原则 LOD
- 组合/聚合复用原则 CARP

其他原则可以看作是开闭原则的实现手段或方法，开闭原则是理想状态

### 129. 反射的使用场景、作用及优缺点?

**使用场景**

- 在编译时无法知道该对象或类可能属于哪些类，程序在运行时获取对象和类的信息

**作用**

- 通过反射可以使程序代码访问装载到 JVM 中的类的内部信息，获取已装载类的属性信息、方法信息

**优点**

- 提高了 Java 程序的灵活性和扩展性，降低耦合性，提高自适应能力。
- 允许程序创建和控制任何类的对象，无需提前硬编码目标类
- 应用很广，测试工具、框架都用到了反射

**缺点**

- 性能问题：反射是一种解释操作，远慢于直接代码。因此反射机制主要用在对灵活性和扩展性要求很高的系统框架上,普通程序不建议使用
- 模糊程序内部逻辑：反射绕过了源代码，无法再源代码中看到程序的逻辑，会带来维护问题
- 增大了复杂性：反射代码比同等功能的直接代码更复杂

### 130. 下面代码的输出是？

------

```javascript
public class TestExchange {
	String str = new String("1");
	char[] arr = { 'a', 'b', 'c' };

	public static void main(String[] args) {
		TestExchange te = new TestExchange();
		te.exchange(te.str, te.arr);
		System.out.print(te.str + " ");
		System.out.print(te.arr);
	}

	public void exchange(String str, char arr[]) {
		str = "2";
		arr[0] = 'd';
	}
}
```

**输出**

```javascript
1 dbc
```

 

**分析：**

- te 对象的 str 属性被 exchange 方法处理过之后，仍然指向字符串常量缓冲区 "1"
- arr 属性是个数组，在 exchange 方法中，数组的内部第一个位置的值被修改，是生效的。如果加上 arr = new char[0]; 这段，输出值不会改变，原理同上。

### 131. 关于String[] strArr=new String[10];正确的是()

------

- A、strArr[10] 为 ""
- B、strArr[0] 为未定义
- C、strArr[9] 为 null
- D、strArr.length 为 10

**答案：CD**

**分析：**

- 引用数据类型的默认值为 null
- strArr.length 为数组的长度 10

### 132. 写一个方法实现String类的replaceAll方法

String 的 replaceAll 是基于正则表达式实现的，借助 JDK 中正则表达式实现。

```javascript
package constxiong.interview;

import java.util.regex.Pattern;

/**
 * 测试实现 replaceAll 方法
 * @author ConstXiong
 */
public class TestReplaceAll {

	public static void main(String[] args) {
		String s = "01234abcd";
		System.out.println(replaceAll(s, "[a-z]", "CX"));
	}
	
	public static String replaceAll(String s, String regex, String replacement) {
		return Pattern.compile(regex).matcher(s).replaceAll(replacement);
	}
}
```

### 133. String类是否可以继承？

不可以

String 类在 JDK 中被广泛使用，为了保证正确性、安全性，String 类是用 final 修饰，不能被继承，方法不可以被重写。

### 134. String、StringBuilder、StringBuffer的区别?

**相同点：**

- 都可以储存和操作字符串
- 都使用 final 修饰，不能被继承
- 提供的 API 相似

**区别：**

- String 是只读字符串，String 对象内容是不能被改变的
- StringBuffer 和 StringBuilder 的字符串对象可以对字符串内容进行修改，在修改后的内存地址不会发生改变
- StringBuilder 线程不安全；StringBuffer 线程安全

方法体内没有对字符串的并发操作，且存在大量字符串拼接操作，建议使用 StringBuilder，效率较高。

### 135. 为什么String类被设计用final修饰？

- String 类是最常用的类之一，为了效率，禁止被继承和重写
- 为了安全。String 类中有很多调用底层的本地方法，调用了操作系统的 API，如果方法可以重写，可能被植入恶意代码，破坏程序。Java 的安全性也体现在这里。

### 136. String s = new String("xyz");创建几个String对象？

两个或一个

- 第一次调用 new String("xyz"); 时，会在堆内存中创建一个字符串对象，同时在字符串常量池中创建一个对象 "xyz"
- 第二次调用 new String("xyz"); 时，只会在堆内存中创建一个字符串对象，指向之前在字符串常量池中创建的 "xyz"

### 137. String s="a"+"b"+"c"+"d";创建了几个对象？

1个

Java 编译器对字符串常量直接相加的表达式进行优化，不等到运行期去进行加法运算，在编译时就去掉了加号，直接将其编译成一个这些常量相连的结果。

所以 "a"+"b"+"c"+"d" 相当于直接定义一个 "abcd" 的字符串。

### 138. 对比一下Java和JavaScriprt

JavaScript 与 Java 是两个公司开发的不同的两个产品。

- Java 是 Sun 公司推出的面向对象的编程语言，现在多用于于互联网服务端开发，前身是 Oak
- JavaScript 是 Netscape 公司推出的，为了扩展 Netscape 浏览器功能而开发的一种可以嵌入Web 页面中运行的基于对象和事件驱动的解释性语言，前身是 LiveScript

**区别：**

- 面向对象和基于对象：Java是一种面向对象的语言；JavaScript 是一种基于对象（Object-Based）和事件驱动（Event-Driven）的编程语言，提供了丰富的内部对象供开发者使用
- 编译和解释：Java 的源代码在执行之前，必须经过编译；JavaScript 是一种解释型编程语言，其源代码不需经过编译，由浏览器直接解释执行
- 静态与动态语言：Java 是静态语言(编译时变量的数据类型即可确定的语言)；JavaScript 是动态语言(运行时确定数据类型的语言)
- 强类型变量和类型弱变量：Java 采用强类型变量检查，所有变量在编译之前必须声明类型；JavaScript 中变量声明，采用弱类型，即变量在使用前不需作声明类型，解释器在运行时检查其数据类型

### 139. 什么是assert？

- assert：断言
- 一种常用的调试方式，很多开发语言中都支持这种机制
- 通常在开发和测试时开启
- 可以用来保证程序最基本、关键的正确性
- 为了提高性能，发布版的程序通常关闭断言
- 断言是一个包含布尔表达式的语句，在执行这个语句时假定该表达式为 true；如果表达式计算为 false ，会报告一个 AssertionError
- 断言在默认情况下是禁用的，要在编译时启用断言，需使用source 1.4 标记，如 javac -source 1.4 TestAssert.java
- 要在运行时启用断言，需加参数 -ea 或 -enableassertions
- 要在运行时选择禁用断言，需加参数 -da 或 -disableassertions
- 要在系统类中启用或禁用断言，需加参数 -esa 或 -dsa

 

**Java 中断言有两种语法形式：**

- assert 表达式1;
- assert 表达式1 : 错误表达式 ;

表达式1 是一个布尔值

错误表达式可以得出一个值，用于生成显示调试信息的字符串消息

```javascript
package constxiong.interview;

public class TestAssert {

	public static void main(String[] args) {
		assert 1 > 0;
		int x = 1;
		assert x <0 : "大于0";
	}
	
}
```

打印：

```javascript
Exception in thread "main" java.lang.AssertionError: 大于0
	at constxiong.interview.TestAssert.main(TestAssert.java:8)
```

### 140. 类的实例化方法调用顺序

类加载器实例化时进行的操作步骤：加载 -> 连接 -> 初始化

- 代码书写顺序加载父类静态变量和父类静态代码块
- 代码书写顺序加载子类静态变量和子类静态代码块
- 父类非静态变量（父类实例成员变量）
- 父类构造函数
- 子类非静态变量（子类实例成员变量）
- 子类构造函数

### 141. JDK8中Stream接口的常用方法

Stream 接口中的方法分为中间操作和终端操作，具体如下。

**中间操作：**

- filter：过滤元素
- map：映射，将元素转换成其他形式或提取信息
- flatMap：扁平化流映射
- limit：截断流，使其元素不超过给定数量
- skip：跳过指定数量的元素
- sorted：排序
- distinct：去重


**终端操作：**

- anyMatch：检查流中是否有一个元素能匹配给定的谓词
- allMatch：检查谓词是否匹配所有元素
- noneMatch：检查是否没有任何元素与给定的谓词匹配
- findAny：返回当前流中的任意元素（用于并行的场景）
- findFirst：查找第一个元素
- collect：把流转换成其他形式，如集合 List、Map、Integer
- forEach：消费流中的每个元素并对其应用 Lambda，返回 void
- reduce：归约，如：求和、最大值、最小值
- count：返回流中元素的个数

### 142. 说说反射在你实际开发中的使用

反射使用不好，对性能影响比较，一般项目中很少直接使用。

反射主要用于底层的框架中，Spring 中就大量使用了反射，比如：

- 用 IoC 来注入和组装 bean
- 动态代理、面向切面、bean 对象中的方法替换与增强，也使用了反射
- 定义的注解，也是通过反射查找

### 143. 什么是泛型？为什么要使用泛型？

**泛型：**

- "参数化类型"，将类型由具体的类型参数化，把类型也定义成参数形式（称之为类型形参），然后在使用/调用时传入具体的类型（类型实参）。
- 是 JDK 5 中引入的一个新特性，提供了编译时类型安全监测机制，该机制允许程序员在编译时监测非法的类型。
- 泛型的本质是把参数的类型参数化，也就是所操作的数据类型被指定为一个参数，这种参数类型可以用在类、接口和方法中。


**为什么要用泛型？**

- 使用泛型编写的程序代码，要比使用 Object 变量再进行强制类型转换的代码，具有更好的安全性和可读性。
- 多种数据类型执行相同的代码使用泛型可以复用代码。

比如集合类使用泛型，取出和操作元素时无需进行类型转换，避免出现 java.lang.ClassCastException 异常

### 144. 有没有使用JDK1.8 中的日期与时间API?

**为什么 JDK 1.8 之前的时间与日期 API 不好用？**

1、java.util.Date 是从 JDK 1.0 开始提供，易用性差

- 默认是中欧时区(Central Europe Time)
- 起始年份是 1900 年
- 起始月份从 0 开始
- 对象创建之后可修改

2、JDK 1.1 废弃了 Date 中很多方法，新增了并建议使用 java.util.Calendar 类

- 相比 Date 去掉了年份从 1900 年开始
- 月份依然从 0 开始
- 选用 Date 或 Calendar，让人更困扰

3、DateFormat 格式化时间，线程不安全

为了解决 JDK 中时间与日期较难使用的问题，JDK 1.8 开始，吸收了 Joda-Time 很多功能，新增 java.time 包，加了**新特性**：

- 区分适合人阅读的和适合机器计算的时间与日期类
- 日期、时间及对比相关的对象创建完均不可修改
- 可并发解析与格式化日期与时间
- 支持设置不同的时区与历法

 

| LocalDate         | 本地日期                     |
| ----------------- | ---------------------------- |
| LocalTime         | 本地时间                     |
| LocalDateTime     | 本地日期+时间                |
| Instant           | 时间戳，适合机器时间计算     |
| Duration          | 时间差                       |
| Period            | 年、月、日差                 |
| ZoneOffset        | 时区偏移量                   |
| ZonedDateTime     | 带时区的日期时间             |
| Clock             | 时钟，获取其他地区时钟       |
| DateTimeFormatter | 时间格式化                   |
| Temporal          | 日期-时间获取值的字段        |
| TemporalAdjuster  | emporal 对象转换，实现自定义 |
| ChronoLocalDate   | 日历系统接口                 |

 

**常用 api**

1、 获取当前日期

```javascript
LocalDate.now()
```

 

2、创建日期

```javascript
LocalDate date = LocalDate.of(2020, 9, 21)
```

 

3、获取年份

```javascript
date.getYear()

//通过 TemporalField 接口的实现枚举类 ChronoField.YEAR 获取年份
date.get(ChronoField.YEAR)
```

 

4、获取月份

```javascript
date.getMonth().getValue()

//通过 TemporalField 接口的实现枚举类 ChronoField.MONTH_OF_YEAR 获取月份
date.get(ChronoField.MONTH_OF_YEAR)
```

 

5、获取日

```javascript
date.getDayOfMonth()

//通过 TemporalField 接口的实现枚举类 ChronoField.DAY_OF_MONTH 获取日
date.get(ChronoField.DAY_OF_MONTH)
```

 

6、获取周几

```javascript
date.getDayOfWeek()
```

 

7、获取当前月多少天

```javascript
date.lengthOfMonth()
```

 

8、获取当前年是否为闰年

```javascript
date.isLeapYear()
```

 

9、当前时间

```javascript
LocalTime nowTime = LocalTime.now()
```

 

10、创建时间

```javascript
LocalTime.of(23, 59, 59)
```

 

11、获取时

```javascript
nowTime.getHour()
```

 

12、获取分

```javascript
nowTime.getMinute()
```

 

13、获取秒

```javascript
nowTime.getSecond()
```

 

14、获取毫秒

```javascript
nowTime.getLong(ChronoField.MILLI_OF_SECOND)
```

 

15、获取纳秒

```javascript
nowTime.getNano()
```

 

16、创建日期时间对象

```javascript
LocalDateTime.of(2020, 9, 21, 1, 2, 3);
LocalDateTime.of(date, nowTime);
```

 

17、获取当前日期时间对象

```javascript
LocalDateTime.now()
```

 

18、通过 LocalDate 创建日期时间对象

```javascript
date.atTime(1, 2, 3)
```

 

19、通过 LocalTime 创建日期时间对象

```javascript
nowTime.atDate(date)
```

 

20、通过 LocalDateTime 获取 LocalDate 对象

```javascript
LocalDateTime.now().toLocalDate()
```

 

21、通过 LocalDateTime 获取 LocalTime 对象

```javascript
LocalDateTime.now().toLocalTime()
```

 

22、解析日期字符串

```javascript
LocalDate.parse("2020-09-21")
```

 

23、解析时间字符串

```javascript
LocalTime.parse("01:02:03")
```

 

24、解析日期时间字符串

```javascript
LocalDateTime.parse("2020-09-21T01:02:03", DateTimeFormatter.ISO_LOCAL_DATE_TIME)
```

 

25、方便时间建模、机器处理的时间处理类 Instant，起始时间 1970-01-01 00:00:00

```javascript
//起始时间 + 3 秒
Instant.ofEpochSecond(3)
//起始时间 + 3 秒 + 100 万纳秒
Instant.ofEpochSecond(3, 1_000_000_000)
//起始时间 + 3 秒 - 100 万纳秒
Instant.ofEpochSecond(3, -1_000_000_000))
//距离 1970-01-01 00:00:00 毫秒数
Instant.now().toEpochMilli()
```

 

26、Duration：LocalTime、LocalDateTime、Intant 的时间差处理

```javascript
Duration.between(LocalTime.parse("01:02:03"), LocalTime.parse("02:03:04"))
Duration.between(LocalDateTime.parse("2020-09-21T01:02:03"), LocalDateTime.parse("2020-09-22T02:03:04"))
Duration.between(Instant.ofEpochMilli(1600623455080L), Instant.now())
```

 

27、日期时间，前、后、相等比较

```javascript
//2020-09-21 在 2020-09-18 前？
LocalDate.parse("2020-09-21").isBefore(LocalDate.parse("2020-09-18"))
//01:02:03 在 02:03:04 后？
LocalTime.parse("01:02:03").isAfter(LocalTime.parse("02:03:04"))
```

 

28、修改日期、时间对象，返回副本

```javascript
//修改日期返回副本
LocalDate.now().withYear(2019).withMonth(9).withDayOfMonth(9)
LocalDate date4Cal = LocalDate.now();
//加一周
date4Cal.plusWeeks(1)
//减两个月
date4Cal.minusMonths(2)
//减三年
date4Cal.minusYears(3)
```

 

29、格式化

```javascript
//格式化当前日期
LocalDate.now().format(DateTimeFormatter.ISO_LOCAL_DATE)
//指定格式，格式化当前日期
LocalDate.now().format(DateTimeFormatter.ofPattern("yyyyMMdd"))
指定格式，格式化当前日期时间
//格式化当前日期时间
LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyyMMdd  HH:mm:ss"))
```

 

30、解析

```javascript
//日期解析
LocalDate.parse("2020-09-20")
//指定格式，日期解析
LocalDate.parse("2020/09/20", DateTimeFormatter.ofPattern("yyyy/MM/dd"))
//指定格式，日期时间解析
LocalDateTime.parse("2020/09/20 01:01:03", DateTimeFormatter.ofPattern("yyyy/MM/dd HH:mm:ss"))
```

 

31、时区

```javascript
//上海时区
ZoneId shanghaiZone = ZoneId.of("Asia/Shanghai");
//设置日期为上海时区
LocalDate.now().atStartOfDay(shanghaiZone)
//设置日期时间为上海时区
LocalDateTime.now().atZone(shanghaiZone)
//设置 Instant 为上海时区
Instant.now().atZone(shanghaiZone)
```

 

32、子午线时间差

```javascript
//时间差减 1 小时
ZoneOffset offset = ZoneOffset.of("-01:00");
//设置时间差
OffsetDateTime.of(LocalDateTime.now(), offset)
```