# 包装类

## 1

```java
public void test1()
{
    Object o1 = true ? new Integer(1) : new Double(2.0);
    System.out.println(o1);//1.0
}
```

`Double`与Integer放一起编译器会给你提升一下类型，把Integer变Double。另外，java中表达式没有`boolean`类型的值，所以这里是o1为true所以执行第一个new，而不是表达式是true。

## 2

```java
    public void test2()
    {
        Integer i = new Integer(1);
        Integer j = new Integer(1);
        System.out.println(i == j);//false

        Integer m = 1;
        Integer n = 1;
        System.out.println(m == n);//true

        Integer x = 128;
        Integer y = 128;
        System.out.println(x == y);//false
    }
```

第一个不同对象所以是false，第二个是因为在自动装箱时，它里面有一个内部类，类里面有一个数组，范围是-128~127，自动装箱时用这个范围的数就可以直接从这里取。所以第二个是true，第三个是false。另外Character中范围是0~128.。

# 异常

常见的异常都有哪些？

```java
public class Out {
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

