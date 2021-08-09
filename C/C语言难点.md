# C语言难点

## 字符串

> 用双引号括起来的多个字符就是字符串

1. 字符串常量的值是其地址。有了字符串，就要用，可以将它给数组与指针初始化。

```c
char array[10] = "array";
char *p = "array";
```

2. 但是不可以将它赋值给这个数组。

```c
array = "array";//错误
```

这里要说明赋值与初始化的不同：

> “变量赋值”发生在运行期，其写法遵循赋值语法规定。
> “变量初始化”发生在编译期或运行期，其写法遵循初始化列表语法规定。

赋值是与左值与右值相关的，左值要是一个可以被改变的值，而数组名就是一个**指针常量**，也就是`char *const array`所以它不能指向别处了。如果以数组名做为左值，肯定是希望数组名指向别处的，这是不允许的。

**C primer plus中这样写到**

> 在数组形式中，array是地址常量，不能更改array，如果改变了array，则意味着改变了数组的存储位置
>
> 字符串储存在静态存储区中，但是，程序在开始运行时才会为该数组分配内存。此时，才将字符串拷贝到数组中。注意，此是字符串有两个副本，一个是静态内存中的字符串常量，另一个是储存在array数组中的字符串。

那c语言为什么要将数组名定义为常量呢？

**C primer plus中也有介绍**

> array = &array[0] 都表示数组首元素的内存地址，两者都是常量，在程序的运行过程中不会改变。

我想这就是c语言中数组与指针的不同，指针是可以修改的，可是数组的一地址一开始就固定好了。

3. 而有一个`strcpy`函数就可以实现，字符串的拷贝，为什么？

   看这个函数定义：

   ```c
   char* strcpy(char* des,const char* source)
   {
       char* r=des;
       assert((des != NULL) && (source != NULL));
       while((*r++ = *source++)!='\0');
       
       return des;
   }
   ```

   调用时：

   ```c
   strcpy(array, "array");
   ```

   看这个函数定义，尽管des（array)是个指针常量，但是通过一个新指针r获得地址，再通过`*`间接的去改变地址上的值。

所以对字符串的操作归根结底是对指针的操作，它的赋值也就是指针的指向不同，而不是改变地址上的值。

## 指针

### 指针常量与常量指针

好像中文翻译的不同，导致了不同，参考大多数：

> 指针常量：指针是一个常量 。指针不能被修改，不能指向别处	`int* const p`
>
> 常量指针：这是一个指针，指向常量 。指针指向的值是一个常量，这个值不能通过指针来改`int const *p`

## 尽量用函数来代替宏，无符号数与有符号数比较大小问题。

```c
#include <stdio.h>
#include "head.h"
#define MAX(a, b) ((a) > (b) ? (a) : (b))

int max(int a, int b);

int main(void)
{
    unsigned int a = 1;
    int b = -1; 

    printf("THE MAX IS %d\n", MAX(++a, b));
    printf("the max is %d.\n", max(a, b));

    return 0;
}
int max(int a, int b)
{
    return a > b ? a : b;
}

```

输出：

![image-20201217205832203](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201217205832203.png)

无符号数和有符号数在进行比较时， C语言会隐式地将有符号数转化成无符号数。原先的补码就会成了原码。

例子中：64位编译中`int`和`unsigned int`都占4个字节（32bit）

* 对于a，有符号数的原码与补码相同。 a的二进制码为：

  `a  = 0000 0000 0000 0000 0000 0000 0000 0001`

* 对于b来说，计算机用补码来表示负数。b的二进制码即为：

  `b = 1111 1111 1111 1111 1111 1111 1111 1111`

如果认为b是一个无符号数，而它的二进制码不变：

`b = 1111 1111 1111 1111 1111 1111 1111 1111`

那b的值就是`unsigned int`能表示的最大值 = $2^{32} - 1$。改一下代码：

```c
include <stdio.h>
#include <math.h>
#define MAX(a, b) ((a) > (b) ? (a) : (b))

int max(int a, int b);

int main(void)
{
    unsigned int a = pow(2, 32) - 1;
    int b = -1; 

    printf("while a = pow(2, 32) - 1, a == b ? =>%d\n", a == b); 
    printf("THE MAX IS %d\n", MAX(++a, b));
    printf("the max is %d.\n", max(a, b));

    return 0;
}
int max(int a, int b)
{
    return a > b ? a : b;
}

```

![image-20201217210811157](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201217210811157.png)

此时a和b的二进制码都是

`1111 1111 1111 1111 1111 1111 1111 1111`

所以是相等的。判断为1。

`++a = 0000 0000 0000 0000 0000 0000 0000 0000` < ` b = 1111 1111 1111 1111 1111 1111 1111 1111`

所以`MAX(a, b) ((a) > (b) ? (a) : (b))`就为b = -1。

因为函数的在传递参数时，已经强制类型转换了。所以两个`int`型比较 =>`a = 0 > b = -1`返回0。

可以将参数a的类型改为无符号看看结果。

```c
include <stdio.h>
#include <math.h>
#define MAX(a, b) ((a) > (b) ? (a) : (b))

int max(unsigned int a, int b);

int main(void)
{
    unsigned int a = pow(2, 32) - 1;
    int b = -1; 

    printf("while a = pow(2, 32) - 1, a == b ? =>%d\n", a == b); 
    printf("THE MAX IS %d\n", MAX(++a, b));
    printf("the max is %d.\n", max(a, b));

    return 0;
}
int max(unsigned int a, int b)
{
    return a > b ? a : b;
}

```

![image-20201217212148603](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201217212148603.png)

可以看到最后的函数也输出-1了。



==不要将不同类型的变量放在一起比较或者运算，如果有必要，将它们转换成同一类型（确保数据不丢失）后再开始。==

域f在结构体type中的偏移量

```c
#define offsetof(type, f) ((size_t) ((char *) &((type *)0)->f - (char *)(type *)0))
```

```c
/*计算类内成员偏移*/
#define offsetof(type, member) ((size_t) (((char *)&((type *)0)->member)))
/*通过成员地址确定类的地址*/
#define container_of(ptr, type, member) (type *)((char *)(ptr) - (char *) &((type *)0)->member))
```

```c
struct A {
    int a;
    int b;
    int c;
};

struct A test;
struct A *p_test;

int *pb = &(test.b);
size_t offset_b;
/*可以计算出b对A的偏移*/
offset_b = offsetof(A, b);
/*可以根据指向成员b的指针pb计算出指向test的指针*/
p_test   = container_of(pb, A, b);

```

