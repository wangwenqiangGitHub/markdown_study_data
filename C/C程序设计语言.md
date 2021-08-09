# 第一章：导言

```c
#include<stdio.h>

int main(void)
{
    printf("hello, world\n");
    return 0;
}
```

> 一个C语言程序，无论其大小如何，都是由函数和变量组成的。函数中包含一些语句，以指定所要执行的计算操作;变量则用于存储计算过程中使用的值。
>
> 每个程序都从main函数的起点开始执行。

- 用双引号括起来的字符序列称为字符串或字符串常量。
- printf仅仅是标准库函数中的一个有用的函数而已，ANSI标准定义了printf函数的行为，所以对每个符合该标准的编译器和库来说，该函数的属性都是相同的。



## 符号常量

```
#define 名字 替换文本
```

在该定义后，程序中出现的所有在define中定义的名字，都将用相应的替换文本替换。替换文本可以是任何字符序列，而不仅限于数字。

## 字符的输入与输出

> 标准库提供的输入/输出模型很简单。无论文本从何处输入，输出到何处，其输入，输出都是按照字符流的方式处理。
>
> **文本流**是由多行字符构成的字符序列，而每行字符则由0个或多个字符组成，行末是一个换行符。



编写一个程序，打印输入中单词长度的垂直方图。

```c
#include<stdio.h>

#define MAXHIST 15
#define MAXWORD 11
#define IN 1
#define OUT 0

int main(void)
{
    int c, i, j;  
    int nc = 0;//一个单词的个数
    int state = OUT;
    int len;
    int maxvalue;
    int ovflow = 0;
    int wl[MAXWORD] = { 
        0
    };  
    
    while((c = getchar()) != EOF )
    {   
        if (c == ' ' || c == '\t' || c == '\n' )
        {
            state = OUT;
            if (nc > 0)
                if (nc < MAXWORD)
                    ++wl[nc];
                else
                     ++ovflow;
            nc = 0;
        }
        else if (state == OUT)//如果遇到单词且它在外面
        {
            state = IN; 
            nc = 1;
        }
        else//如果没有遇到空格且它在里面
            ++nc;
    }   
    maxvalue = 0;
    for (i = 1; i < MAXWORD; ++i)
        if (wl[i] > maxvalue)
            maxvalue = wl[i];
    for (i = MAXHIST; i > 0; --i)
    {   
        for (j = 1; j < MAXWORD; ++j)
            if (wl[j] * MAXHIST / maxvalue >= i)
                printf(" * ");
            else
                printf("   ");
        putchar('\n');
    }   
    for ( i = 1; i < MAXWORD; ++i)
        printf("%2d ", i); 
    putchar('\n');
    for ( i = 1; i < MAXWORD; ++i)
        printf("%2d ", wl[i]);
    putchar('\n');
    if (ovflow > 0)
        printf("There are %d words >= %d\n", ovflow, MAXWORD);

    return 0;
}
```

编写一个detab，将输入中的制表符替换成适当数目的空格，使空格充满到下一个制表符终止位的地方。假设制表终止位的位置是固定的，比如，每隔n列就会出现一个制表符终止位。n应该作为变量还是符号常量呢？

也就是我们遇到制表符时，如果前面的字符不足制表符个，那就用空格补齐。

```c
#include<stdio.h>

#define TAB 8

int main(void)
{
    int ch;
    int n = 0kkk;
    int a = 1;
    while((ch = getchar()) != EOF)
    {
        if (ch == '\t')
        {
            n = TAB - (a - 1) % TAB;
            while (a > 0)
            {
                printf(" ");
                a++;
                n--;
            }
        }
        else if (ch == '\n')
        {
            putchar('\n');
            a = 1;
        }
        else
        {
            printf("ch");
            a++;
        }
    
    }
    return 0;
}
```

# 第二章：类型、运算符、表达式

程序离不开数据。计算机就是像黑盒一样，把数字、字母和文字输入计算机，就是需要用这些数据去完成一些任务，输出想要的结果。

而这些数据大多时候都是不唯一的（比如求1到100之和），你可以写1+2+3+……+100，更好的想法是用一个未知数来表示每一个数，刚开始这个未知数是1，到最后变成了100。根据这两种思想，就有了**常量（constant）**和**变量（variable）**。

* 常量：在整个程序中没有变化。
* 变量：在程序运行期间可能会改变或被改变或赋值。

在C语言中，要用数据前，要先确定好这个数据的类型，然后才可以在这种类型的范围内去改变它。因为计算机中不同类型的存储方式不一样。

```c
#include<stdio.h>

int main(void)
{
    int a;//声明语句
    
    a = 3;//赋值语句，用了赋值运算符`=`
    
    a = 3 + a;//表达式
    
    return 0;
}
```

声明语句说明变量的名字及类型，也可以指定变量的初值。

运算符指定将要进行的操作。

表达式把变量与常量组合起来生成新的值。

## 2.1 变量名

> 名字是由字母、数字、下划线组成，下划线被看作是字母。首字母不能是数字。但因为库例程的名字多用下划线开头（也就是大佬写好的），所以变量名尽量使用除下划线以外的字母。而且26个字母分区大小写。

## 2.2 数据类型及长度

> 对象（不是指面向对象语言中的对象）的类型决定该对象可以取值的集合以及可以对该对象执行的操作。

```c
int a;
```

这里的a就是对象，只是一个代词。

### 整数类型

1字节(Byte) = 8位(bit)

| 类型          | 占用字节（32） | 占用字节64 |
| ------------- | -------------- | ---------- |
| char          | 1              | 1          |
| short int     | 2              | 2          |
| int           | 4              | 4          |
| long int      | 4              | 8          |
| long long int | 8              | 8          |

每个编译器都可以自由地为自己的硬件选择合适的大小，只受以下限制：short 和int 至少是16位，long至少32位，short不超过int（int不超过long）。

字符型 char 占一个字节，可以存放本地字符集中的一个字符。

#### unsigned 与signed

类型限定符`signed`和`unsigned`可以限定任意整型。

> `unsigned`是无符号之意，可以理解为没有负号，不是负数。其能表示的范围遵循$2^n$定律，n是其占用的位数，如果是char型，其范围就是0~255。共256 = $2^8$个数。

> `signed`限定时，可以不写出，其可能表示的范围也是遵循$2^n$定律，但是其因为可以表示负数，**在采用对二的补码的机器上**，是从$-2^{n-1} - 1$至$2^{n-1}$也就说char型可能表示数的范围是-127~128。

### 浮点类型

| 类型   | 占用字节（64） |
| ------ | -------------- |
| float  | 4              |
| double | 8              |

### 练习2-1：

编写一个程序以确定分别由signed及unsigned限定的char、short、int与long类型变量的取值范围。采用打印标准头文件中的相应值 以及直接计算两种方式实现。后一种方法的实现较困难一些，因为要确定各种浮点类型的取值范围。

```c
#include <stdio.h>
#include <limits.h>

/* determine range of types*/
int main(void)
{
    //signed types
    printf("signed char min  =  %d\n", SCHAR_MIN);
    printf("signed char max  =  %d\n", SCHAR_MAX);
    printf("signed short min =  %d\n", SHRT_MIN);
    printf("signed short max =  %d\n", SHRT_MAX);
    printf("signed int min   =  %d\n", INT_MIN);
    printf("signed int max   =  %d\n", INT_MAX);
    printf("signed long min  =  %ld\n", LONG_MIN);
    printf("signed long max  =  %ld\n", LONG_MAX);

    printf("*************************************************************\n");

    //unsigned types
    printf("unsigned char max  =  %u\n", UCHAR_MAX);
    printf("unsigned short max =  %u\n", USHRT_MAX);
    printf("unsigned int max   =  %u\n", UINT_MAX);
    printf("unsigned long max  =  %lu\n", ULONG_MAX);

    return 0;
}
/*
signed char min  =  -128
signed char max  =  127
signed short min =  -32768
signed short max =  32767
signed int min   =  -2147483648
signed int max   =  2147483647
signed long min  =  -9223372036854775808
signed long max  =  9223372036854775807
*************************************************************
unsigned char max  =  255
unsigned char max  =  -1
unsigned short max =  65535
unsigned int max   =  4294967295
unsigned long max  =  18446744073709551615
*/
```

第一种方法就是`<limits.h>`头文件中的常量，直接包含头文件就行。

```c
#include <stdio.h>

int main(void)
{
    //signed types 
    printf("signed char min =  %d\n", -(char)((unsigned char) ~0 >> 1) - 1); //-127 - 1
    printf("signed char max =  %d\n", (char)((unsigned char) ~0 >> 1)); //127
    printf("signed short min =  %d\n", -(short)((unsigned short) ~0 >> 1) - 1); 
    printf("signed short max =  %d\n", (short)((unsigned short) ~0 >> 1)); 
    printf("signed int min =  %d\n", -(int)((unsigned int) ~0 >> 1) - 1); 
    printf("signed int max =  %d\n", (int)((unsigned int) ~0 >> 1)); 
    printf("signed long min =  %ld\n", -(long)((unsigned long) ~0 >> 1) - 1); 
    printf("signed long max =  %ld\n", (long)((unsigned long) ~0 >> 1)); 

    printf("*************************************************************\n");
    //unsigned types
    printf("unsigned char max  =  %u\n", (unsigned char) ~0);
    printf("unsigned short max =  %u\n", (unsigned short) ~0);
    printf("unsigned int max  =  %u\n", (unsigned int) ~0);
    printf("unsigned long max =  %lu\n", (unsigned long) ~0);
    

    return 0;
}
/*
signed char min  =  -128
signed char max  =  127
signed short min =  -32768
signed short max =  32767
signed int min   =  -2147483648
signed int max   =  2147483647
signed long min  =  -9223372036854775808
signed long max  =  9223372036854775807
*************************************************************
unsigned char max  =  255
unsigned char max  =  -1
unsigned short max =  65535
unsigned int max   =  4294967295
unsigned long max  =  18446744073709551615
*/
```

第二种方法，首先，无符号的最大值好求，n位的数，最大值就是$2^n-1$，用二进制就是11111……，n个1。0的原码是00000……n个0，将其取反即得到所有位都是1的最大值。

注意：

```c
 printf("unsigned char max  =  %u\n", (unsigned char) ~0);
//unsigned char max  =  255
```

这里打印格式是%u，所以必然输出非负才正确。如果将unsigned char 改成char，取反后就是-1，计算机就会提升为int型，从而输出int型的最大值。

```c
printf("unsigned char max  =  %u\n", (char) ~0); //与下面的输出-1效果一样
printf("unsigned char max  =  %u\n", -1);
//unsigned char max  =  4294967295
//unsigned char max  =  4294967295
```

再者~取反符号的位置不要搞错了。

```c
printf("unsigned char max  =  %d\n", (unsigned char) ~0);
printf("unsigned char max  =  %d\n", ~(unsigned char) 0);
printf("unsigned char max  =  %u\n", ~(unsigned char) 0); //必然为非负数，提升为int
printf("unsigned char max  =  %u\n", -1);
//unsigned char max  =  255
//unsigned char max  =  -1
//unsigned char max  =  4294967295
//unsigned char max  =  4294967295
```

所以要最后强制转换再参与格式化输出，以限定它打印时不被自动转换。

然后，输出无符号的范围

```c
printf("signed char min =  %d\n", -(char)((unsigned char) ~0 >> 1) - 1); //-127 - 1
printf("signed char max =  %d\n", (char)((unsigned char) ~0 >> 1)); //127
```

> <<左移运算符，左移运算符会在右边空出的位置填补0。左移n位等价于该数乘$2^n$。

> \>>右移运算符，在对unsigned char（无符号）值进行右移时会在左边空出的位置填0。如果是有符号，有些机器会用1填补左边空出的位置(算术移位)，有些机器会用0(逻辑移位)。

对于以二进制补码存放的机器而言，有符数的最最大值的二进制表示为最高位为0，后面全是1，01111……。

1. ~0 对0取反，得到所有二进制位全部为1
2. (unsigned char) ~0，转化为无符号型，因为右移有符号数结果是不确定的。

```c
printf("signed char max =  %d\n", (char)(~0 >> 1)); //-1 0先被提升int，然后我的机器最左边补1
printf("signed char max =  %d\n", (char)(~0)); //-1
printf("signed char max =  %d\n", (char)((unsigned char) ~0 << 1)); //-2 左移最右边补0
printf("signed char max =  %d\n", (char)(~0 << 1)); //-2
```

3. (unsigned char) ~0 >>1，右移一位最左边空出的一位填0，得到想到的效果。

4. (char)((unsigned char) ~0 >>1)，因为是求有符号数，再转回去

有符号数最小值: 

$$Min(char) = -Max(char) - 1$$

## 2.3 常量

> 常量在程序中大部分保持固定的值。基中典型的就是数字，也还有字符常量，字符串常量。

### 整型常量

long类型的常量可以以字母L或l结尾。无符号的常量可以字母u或U结尾。所以`ul`或`UL`可以表示`unsigned long`。

整数除了可以用十进制来表示，也可以用八进制、十六进制来表示。只需要在八进制数前加0，十六进制前加0x或0X。

| 十进制 | 八进制 | 十六进制  |
| ------ | ------ | --------- |
| 31     | 037    | 0x1f/0x1F |

### 浮点型常量

> 浮点数常量包含一个小数点(123.4)，或是指数(1e-2)。
>
> 没有后缀是double，有F或f后缀是float

### 字符常量 

一个字符常量是一个整数，书写时将一个字符括在单引号中，如'x'。ASCII字符集给予这些字符相应的整型数值，这样就与整型相对应，例如，字符'0'的值为48，它与数值0没有任何关系。

> 如果用字符('0')来代替与具体字符集有关的值(48)，程序与操作人员就不用去关心其具体的值是多少。

### 字符串常量

> 字符串常量也叫字符串字面值，是用双引号括起来的0个或多个字符组成的字符序列。双引号不是字符串的一部分，它只用于限定字符串。

**从技术角度来看，字符串常量就是字符数组，字符串内部表示使用一个空字符'\0'作为字符串的结尾。所以，存储字符串的特理存储单元数比括在双引号中的字符数多一个**



某些字符可以通过转义*字符序列*表示为字符和字符串常量。例如'\n'，转义字符序列看起来像两个字符，但其只表示一个字符。

还可以用

`'\ooo'，ooo`表示3个8进制数

`'\xhh'，hh`表示2个16进制数

表示任意大小的**位模式**。

### 枚举常量

```c
enum boolean{NO, YES}
```

没有显式说明时，`enum`类型的第一个枚举值为0，第二个为1……

```
“Although variables of enum types may be declared, compilers need not check that what you store in such a variable is a valid value for the enumeration.
尽管enum类型变量可以被声明，但是编译器不需要检查你存储在这个变量上的值是该枚举的有效值。

Nevertheless, enumeration variables offer the chance of checking and so are often better than #defines.”
不过，枚举变量提供这种检查，所以常常比#define好。
```

==常量表达式是仅仅只包含常量的表达式，此表达式在编译时求值。==可以出现在常量可以出现的任何位置。

例如：

```c
#define MAXLINE 1000
char line[MAXLINE + 1];
```

## 2.4 声明

> C语言中所有变量都要先声明后使用，声明就确定了变量的类型。也就是先告诉它有这个东西是这个类型，后面更好使用。

<font color = #ff66ff>**每个变量声明尽量独占一行，更容易理解、注释和修改。**</font>

```c
//建议
int lower; 
int upper;

//不建议
int lower, upper;
```

更进一步的，可以在声明的同时对变量进行初始化。 声明中，如果变量后面跟上等号和表达式，该表达式就是对变量的初始化表达式(且要是常量表达式)。

```c
int lower = 1; 
int upper = 1000;
```

> 如果该变量不是自动变量(静态、全局)，则只能进行初始化一次。
>
> * 静态与全局(外部)变量如果不对其显式初始化，则其值默认被赋0。
> * 自动变量则是在每次进入函数或程序块时，显式初始化的自动变量都将被初始化一次(自动变量在其函数进栈时创建初始化，弹栈后销毁)。如果自动变量没有显式初始化，那么其值为垃圾值(未定义值)。

为了对变量做进一步的限制，任何变量的==声明==都可以使用`const`限定符限定。

```c
const double e = 2.71828;
const char msg[] = "warning:";//数组名本身是一个地址常量。本来可以通过下标去改变其中的值，可以用strcpy去改变整个数组的值。但是加上了const就不能改变了。
```

<font color = red >例子：</font>

```c
#include<stdio.h>
#include<assert.h>

char* strcpy(char* des,const char* source);

int main(void)
{
    char msg[] = "Hello World!";

    char *p = NULL;

    p = strcpy(msg, "array");

    puts(msg);
    puts(p);

    return 0;
}

char* strcpy(char* des,const char* source)
{
    char* record = des;
    assert((des != NULL) && (source != NULL));
    while((*record++ = *source++) != '\0');

    return des;
}
/*
输出
array
array
*/
```

从`strcpy`函数可以看出，其并没有修改传入数组的指向(<font color = blue>因为des是一个一级指针，如果修改了des的指向，并不会影响`mes`，但是这样出现的bug不会报错</font>)，其通过数组首地址(record)修改了数组中的每一个位置上的值。

注意数组名是一个**地址常量**，也就相当于**指针常量**(`char* const p`)不可以指向别的位置，所以并不可以通过数组首地址的地址赋值给二级指针去改变原先数组的指向。

从右向左读，遇到p就换成"p is a"，遇到*就替换成"point to"， `p is a const point to char`。

```c
char msg[] = "Hello World!";
char **q = &msg[0];
```

![image-20210317205826467](https://i.loli.net/2021/04/22/JbvkRyd1D93jF5I.png)

<font color = red>const指针作为右值，类型不匹配。</font>可以编译成功，但如果去运行，你会得到！没错——<font color = red size = 4>**段错误核心转储**</font>

![image-20210317210459886](https://i.loli.net/2021/04/22/JbvkRyd1D93jF5I.png)

`const`限定符==指定变量==的值不能被修改。(注意是指定变量，对于变量这两个字，在其是指针时要着重理解)

## 2.5 算术运算符

二元算术运算符：+ 、-、*、/、%，二元的意思是其左右两边都有变量或值。要注意的是%(取模运算不能用于浮点数)。

## 2.6 关系运算符和逻辑运算符

关系运算符：

```
> < >= <= == !=
```

其优先级比算术运算符要低。

逻辑运算符：

```c
&& || ！ 
与 或  非
```

c语言中的逻辑运算符是自左向右判断的。其也相应的有一点智能性。

比如：`expression1` && `expression2`

如果从左开始判断出了expression1是一个0(假)，那么它就不会继续判断。同理，||如果从左开始有一个是真，那相应后面也不会继续判断。

经常要注意的是不要把它们写在一起，比如a = (3, 5)， 在c语言中不可能写成  -4 < a < -1的这种形式。

```c
#include <stdio.h>

int main(void)
{
    int a = -2;
    printf("%d\n", -4 < a < -1);

    return 0;
}
//0
```

< 从左向右结合，先运算 -4 < a，得出结果为逻辑1，再与-1结合就变成了`1 < -1`得到逻辑0。要写成` -4 < a && a < -1`逻辑与的关系。

```c
#include <stdio.h>

int main(void)
{
    int a = -2;
    printf("%d\n", -4 < a && a < -1);

    return 0;
}
//1
```

在关系表达式中，如果关系为真，则表达式的结果值为数值1，如果为假，则结果值为数值0。

<font color = blue >还有经常要注意的就是：&、^、|的优先级还没有`==  !=`优先级高。所以使用进记得加括号。</font>

```c
for (i = 0; i < lim - 1 && (c = getchar()) != '\n' && c != EOF; ++i)
    s[i] = c;
```

### 练习2-2

在不使用运算符&&或||的条件下编写一个与上面的for循环语句等价的循环语句。

```c
enum loop { NO, YES}
enum loop okloop = YES

i = 0;
while(okloop == YES)
{
    if (i >= lim - 1)
        okloop = NO;
    else if ((c = getchar()) == '\n')
        okloop = NO;
    else if (c == EOF)
        okloop = NO;
    else
    {
        s[i] = c;
        i++;
    }
}
```

## 2.7类型转换

### 自动类型转换

> 占字节数小的向上提升为大的，比如是char -> `int`
>
> 因为在C语言中char就是较小的整型，因此在算术表达式中可以使用char类型的变量，为实现字符转换提供了很大的灵活性。

```c
int atoi(char s[])
{
    int i, n;
    n = 0;
    for (i = 0; s[i] >= '0' && s[i] <= '9'; i++)
    {   
        n = 10 * n + (s[i] - '0');
    }   
    return n;
}
```

<font color = blue> 在char类型向上转型的时候要注意，无符号和有符号的char是不一样的。有符号的char在向上转换时，在某些机器中是最左位是填充1(符号扩展)，在另一些机器中是左边填充0的，导致了最后结果是正数。</font>

C语言的定义保证了机器的标准打印字符集中的字符不会是负值 。

<font color = red>为了保证程序的可移植性，如果要在char类型的变量中存储非字符数据，最好指定signed 或unsigned限定符。</font>

#### 隐式类型转换

> 二元运算符具有不同类型，会把“较低”的提升成“较高”的。运算结果也是最高的类型。

这种提升对于unsigned类型来说有时会出现一些问题。

比如：

`int` 类型占16位，long类型占32位，那么 -1L < 1U。1U也就是`unsigned int`会被提升为long。但是-1L > 1UL，因为-1L(long)型会被提升为`unsigned long`，变成一个较的正数。

> 赋值时右边的要转成左边的类型。

> 函数调用的参数是表达式，所以参数传递时也可以进行类型转换。



### 强制类型转换

```c
(类型名)表达式
```

类型名即为你要转换的类型。

通常会声明函数原型，当函数被调用时，声明将对参数进行自动强制转换。

```c
double sqrt(double c);
root = sqrt(2);
//2会被自动强制转换为2.0000000
```

### 练习2-3 

编写函数`htoi(s)`，把十六进制数字组成的字符串(包含 可选 的前缀0x或0X)转换为与之等价的整型值。字符串中允许包含的数字包括：0\~9、a\~f、A\~F。

```c
int htoi(char s[])
{
    int n = 0;
    int i = 0;
    if (s == NULL)
    {   
        return -1; 
    }   
    if (s[i] == '0')
    {   
        i++;
        if (s[i] == 'x' || s[i] == 'X')
        {
            i++;
        }   
    }   
    for (; s[i] != '\0'; i++)
    {   
        if (s[i] >= '0' && s[i] <= '9')
            n = n * 16 + s[i] - '0';
        else if (s[i] >= 'a' && s[i] <= 'f')
            n = n * 16 + s[i] - 'a' + 10; 
        else if (s[i] >= 'A' && s[i] <= 'F')
            n = n * 16 + s[i] - 'A' + 10; 
        else
            return -1; 
    }   
    return n;
}
```

答案：

```c
#define YES 1
#define NO  0
int htoi(char s[])
{
    int hexdight, i, inhex, n;
    i = 0;
    if (s[i] == '0')
    {   
        i++;
        if (s[i] == 'x' || s[i] == 'X')
        {
            i++;
        }
    }   
    n = 0;
    inhex = YES;
    for (; inhex == YES; i++)
    {   
        if (s[i] >= '0' && s[i] <= '9')
            hexdight = s[i] - '0';
        else if (s[i] >= 'a' && s[i] <= 'f')
            hexdight = s[i] - 'a' + 10; 
        else if (s[i] >= 'A' && s[i] <= 'F')
            hexdight = s[i] - 'A' + 10; 
        else
            inhex = NO; 
        if (inhex == YES)
            n = 16 * n + hexdight;
    }   
    return n;
}

```

## 2.8 自增运算符与自减运算符

```c
++ --
```

这两个运算符特殊的地方主要表现在：既可以用作前缀运算符，也可以用作后缀运算符。

对于++n和n++而言，前者是选将n的值递增1，然后再使用变量的值，而后者是先使用n的值，再将n递增1。

```c
#include <stdio.h>

int main(void)
{
    int n = 1;
    printf("%d\n", n++); //先使用1，再递增 ->1
    printf("%d\n", ++n); //先递增为2，再使用 ->2
    printf("%d\n", n); //2
    
    return 0;
}
```

==自增与自减运算符只能作用于变量，类似于`(i + j)++`是非法的。==

```c
void GetBin(uint num, uint count)
{
    if (num == 0)
        return;
    GetBin(num / 2, count + 1);
    printf("%d", num % 2);
    if (count != 0 && count % 4 == 0)
    {
        printf("-");
    }
}
```

这是一段递归求二进制的代码，每输出4个二进制就输出一个`-`用于分隔。

回调时的理想栈

| num  | count | 余数输出 |
| ---- | ----- | -------- |
| 0    | 8     | 不输出   |
| 1    | 7     | 1        |
| 3    | 6     | 1        |
| 7    | 5     | 1        |
| 15   | 4     | 1        |
| 31   | 3     | 1        |
| 63   | 2     | 1        |
| 127  | 1     | 1        |
| 255  | 0     | 1        |

如果将递归调用代码改成自增。

```c
GetBin(num / 2, count++);
```

显然这样没有用，每次进入调用都是0。栈中永远存1。

![image-20210414194302951](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210414194302951.png)

![image-20210414194321288](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210414194321288.png)

实际栈

| num  | count | 余数输出 |
| ---- | ----- | -------- |
| 0    | 1     | 不输出   |
| 1    | 1     | 1        |
| 3    | 1     | 1        |
| 7    | 1     | 1        |
| 15   | 1     | 1        |
| 31   | 1     | 1        |
| 63   | 1     | 1        |
| 127  | 1     | 1        |
| 255  | 1     | 1        |

```c
GetBin(num / 2, ++count);
```

这样写进入调用是正确了，但是存的值会在原来的基础上加1，也就是在回调的时候`count`比预想的值增加了1。

```c
void GetBin(uint num, uint count)
{
    if (num == 0)
        return;
    GetBin(num / 2, ++count);
    printf("%d", num % 2);
    if (count != 1 && (count - 1) % 4 == 0)
    {
        printf("-");
    }
}
```

实际栈

| num  | count | 余数输出 |
| ---- | ----- | -------- |
| 0    | 9     | 不输出   |
| 1    | 8     | 1        |
| 3    | 7     | 1        |
| 7    | 6     | 1        |
| 15   | 5     | 1        |
| 31   | 4     | 1        |
| 63   | 3     | 1        |
| 127  | 2     | 1        |
| 255  | 1     | 1        |

### 练习2-4

重新编写函数squeeze(s1, s2)，将字符串s1中任何与字符串s2中字符匹配的字符都删除。

自己的：

```c
char* Squeeze(char *p, char *q) 
{
    for (int k = 0; q[k] != '\0'; k++) 
    {   
        int j = 0;
        int i = 0;
        for (i = 0, j = 0; p[i] != '\0'; i++)
        {
            if (p[i] != q[k])
            {
                p[j++] = p[i];
            }
        }
        p[j] = '\0';
    }   
    return p;
}
```

答案：

```c
char* Squeeze2(char s1[], char s2[])
{
    int k = 0;
    for (int i = 0; s1[i] != '\0'; i++) //从s1字符串开始循环
    {   
        int j = 0;
        for (j = 0; s2[j] != '\0' && s2[j] != s1[i]; j++) //s2一个个判断，等于就跳出
        {
            ;
        }
        if (s2[j] == '\0') //表明没有相等，就写入
        {
            s1[k++] = s1[i];
        }
    }   
    s1[k] = '\0'; //添加结束符
    return s1; 
}
```

### 练习2-5

编写函数`any(s1, s2)`，将字符串s2中的任一字符在字符串s1中<font color = red>第一次</font>出现的位置作为结果返回。如果s1中不包含s2中的字符则返回-1。(标准库函数`strpbrk`具有同样的功能，但它返回的是指向该位置的指针)

从头对s1进行判断，如果其有一个在s2中则返回。

```c
int any(s1[], s2[])
{
    for (int i = 0; s1[i] != '\0'; i++)
    {
        for (int j = 0; s2[j] != '\0'; j++)
        {
            if (s1[i] == s2[j])
            {
                return i;
            }   
        }
    }   
    return -1; 
}
```

## 2.9 按位运算符

```
&
|
^
<< 右补0
>> 无符号左补0，有符号不确定
~
```



```c
#include <stdio.h>
typedef unsigned char uchar;
uchar getbits(uchar x, int p, int n); 

int main(void)
{
    printf("%d\n", getbits(8, 4, 2));
//  printf("%d\n", getbits(8, 3, 2));

    return 0;
}
uchar getbits(uchar x, int p, int n)
{
    return (x >> (p - n)) & ~(~0 << n); //第p位有p＋1个数，p + 1 - n代表要去掉的位数
//  return (x >> (p + 1 - n)) & ~(~0 << n); //第p位有p个数，p - n代表要去掉的位数
}

//2
```

### 练习2-6

编写一个函数setbits(x, p, n, y)，该函数返回对x执行下列操作后的结果值：将x中从第p位开始的n个（二进制）位设置为y中最右边n位的值，x的其余各位保持不变。

```c
typedef unsigned int uint;
uint setbits(uint x, uint p, uint n, uint y)
{
     
    uint temp;
    //得到中间n位为0的二进制数
    temp = ~(~(~0 << n) << (p + 1 - n));

    //将x的中间位清0
    x &= temp;

    //得到y最右边n位
    y &= ~(~0 << n);
    //y左移到替换位置
    y <<= (p + 1 -n);

    return y | x;
    
    //return x & ~(~(~0 << n) << (p + 1 - n)) | (y & ~(~0 << n)) << (p + 1 - n); 
}
/*
x = 255 p = 4 n = 3 y = 5 1111 1111
    247 1111 0111
*/   
void GetBin(char *p, int num)
{
    //char还是int类型
    char *array[] = {
        "char",
        "int",
    };
    //字符串列表长度，可以自己扩充，现在是2
    int choiceLength = sizeof(array) / sizeof(array[0]);
    for (int i = 0; i < choiceLength; i++)
    {
        if (!strncmp(p, array[i], 4))//遍历发现有对应字符串
        {
            GetChoiceBin(num, i); //i是列表中字符串序号，0是char，1是int
            return ;
        }
    }
    printf("No find!\n");
}
void GetChoiceBin(unsigned int num, int choice)//根据选择打印二进制数
{
    unsigned int length = 0;
    if (choice == 0)
    {
        if (num > 255)
        {
            printf("The char can`t big 255!\n");
            exit(EXIT_FAILURE);
        }
        length = sizeof(char) * 8;
    }
    if (choice == 1)
        length = sizeof(int) * 8;
    unsigned int array[length]; //一个能放最大长度的数组

    int i = 0;
    int count = 1;
    for (i = 0; num != 0; i++)
    {
        array[i] = num % 2; //余数放在数组中
        num /= 2;
    }
    //打印开始的0
    for (int j = 0; j < length - i; j++, count++)
    {
        printf("0");
        if (count % 4 == 0)
        {
            printf("-");
        }
    }
    //打印刚才求的二进制，注意是逆序
    for (--i; i >= 0; i--, count++)
    {
        printf("%d", array[i]);
        if (count % 4 == 0 && count != length)
        {
            printf("-");
        }
    }
    printf("\n");
}

```

### 练习2-7

编写一个函数invert(x,p, n)，该函数返回对x执行下列操作后的结果值：将x中从第p位开始的n个(二进制)位求反(即，1变成0，0变成1)，x的其余各位保持不变。

```c
uint invert(uint x, uint p, uint n)
{
    //获得中间为n个1的二进制数
    uint temp = ~(~0 << n) << (p + 1 - n);
    
    //获得x中间的n位
    uint temp1 = temp & x;
    //x中间位清1
    x |= temp;
    return x & (~temp1);
    //return x ^ (~(~0 << n) << (p + 1 - n))
}
x = 255 p = 4 n =3 1111 1111
返回 227 11100011
```

这里用到`^`异或是没有想到的。

|      | 1    | 0    |
| ---- | ---- | ---- |
| 1    | 0    | 1    |
| 0    | 1    | 0    |

与1异或可以取反，与0异或不变。

```c
int rightrot(uint x, uint n)
{
    int length = sizeof(char) * 8;
    if (n > length)
    {   
        n %= length;
    }   
    /*  
    if (n == 0)
    {
        return x;
    }
    //最右边n位1
    uint temp = ~(~0 << n);
    //获得x最右边n位
    temp &= x;
    //x右移n位
    x >>= n;
    //将x最右边n位移到左边
    temp <<= (length - n);
    return x | temp;
    */
    int rbit;
    while (n-- > 0)
    {   
        rbit = (x & 1) << (length - 1); 
        x >>= 1;
        x |= rbit;
    }   
    return x;
}
```

### 练习2-8

编写一个函数rightrot(x, n)，该函数返回将x循环右移(即从最右端移出的位将从左端移入)n(二进制)位后所得到的值。

```c
uint rightrot(uint x, uint n)
{
    int length = sizeof(char) * 8;
    if (n > length)
    {
        n %= length;
    }
    /*
    if (n == 0)
    {
        return x;
    }
    //最右边n位1
    uint temp = ~(~0 << n);
    //获得x最右边n位
    temp &= x;
    //x右移n位
    x >>= n;
    //将x最右边n位移到左边
    temp <<= (length - n);
    return x | temp;
    */
    int rbit;
    while (n-- > 0)
    {
        rbit = (x & 1) << (length - 1); //找到x最右位置，移到最左边的值赋给rbit
        x >>= 1; //x右移一位，最左边填0
        x |= rbit;
    }
    return x;
}
```

## 2.10 赋值运算符与表达式

```c
i = i + 2
可以缩写成 i += 2
```

大多数二元操作符都可以此操作。

`expr1 op= expr2`

等价于

`expr1 = expr1 op expr2` 

### 练习2.9

在求对二的补码时，表达式x&=(x -1)可以删除x中最右边值为1的一个二进制位。请解释这样做的道量。用这一方法重写bitcount函数，以加快其执行速度。

对一个8位的数，x-1的结果有两种可能：

xxxx-xxx1 => x

xxxx-xxx0 => x - 1

xxxx-xxx0 => x&(x-1)

显然是删除了最右边的

xxx1-0000 => x

xxx0-1111 => x - 1

xxx0-0000 => x&(x-1)

也是成立的。

```c
int bitcount(unsigned x)
{
    if (x == 0)
        return 0;
    int count = 1;
    while (x &= (x - 1)) 
    {   
        count++;
    }   
    return count;
}
```

## 2.11 条件表达式

> expr1 ? expr2 : expr3

expr1为真，则执行expr2，否则执行expr3。

# 第八章：UNIX系统接口

> 在UNIX操作系统中，所有的外围设备（包括键盘和显示器）都被看作是文件系统的文件。所有的输入/输出都要通过读文件或写文件完成。

## 文件的输入与输出

```c
int main(int argc,char *argv[])
```

`argc`记录命令行参数的个数，`argv`是一个指针数组，每一个都指向每个参数的首位置，下标从0开始。

### 错误报告

```c
void perror (char const *message);
```

`perror`函数简化向用户报告特定的错误。原型在`stdio.h`中。打钱message，后面跟一个分号一个空格。再打印出一条用于解释当前错误的代码。

当然这个错误要你自己去检查，不是说你随便放在哪里都可以，比如下面的`fopen`返回一个NULL，这就是一个错误。

```c
fopen = NULL;
if (fopen == NULL)
    perror(ERROR);
```

### 终止执行

```c
void exit(int status);
```

- status有两和预定义符号EXIT_SUCCESS表示成功EXIT_FAILURE表示失败。
- 与return不同，它直接停止程序，在递归中它们很不一样。

### 流

> ANSI C对I/O的概念进行的抽象，就c程序而言，所有的I/O操作只是简单地从程序移进或移出字节的事情，这种字节流就被称为流。
>
> 绝大数流是完全缓冲的（`fully burrered`），读入与写入是从缓冲区（内存区域）来回复制数据。对于输出流，缓冲区满刷新到设备或文件中。

#### 文本流

> 标准规定至少允许254个字符。
>
> MS-DOS系统中结束为一个回车与一个换行符，但是UNIX只用一个换行

#### 二进制流

> 完全根据编写它们的形式写入到文件或设备中，完全根据它们从文件或设备读取的形式读入到程序中，没有作任何改变。

### fopen

```c
FILE *fopen(char const *name, char const *mode);
```

> name是你希望打开文件或设备的名字。

| mode              | 读           | 写                         | 添加                       |
| ----------------- | ------------ | -------------------------- | -------------------------- |
| 文本              | "r"          | "w"                        | "a"                        |
| 二进制            | "rb"         | "wb"                       | "ab"                       |
| 更新模式（读+写） | "r+"         | "w+"                       | "a+"                       |
| 说明              | 原先一定存在 | 存在就从头写，不存在就创建 | 直接在后面写，不存在就创建 |

失败会返回NULL，你要做失败检查。

搞清楚你是要读文件还是要写文件，只读文件是不可以写的。

### fclose

```c
int fclose(FILE *fp);
```

对于输出流，关闭前刷新缓冲区，成功返回0，否则返回EOF。

当然，这个函数也要进行检查。

```c
if (fclose(fp) != 0)//返回0才是正确关闭
```

### getc与putc

```c
int getc(FILE *);//读取指定输入流中的下一个字符
int putc(int, FILE *);//把指定字符写入到指定输出中 
```

注意在使用时FILE的指针会移到后面，所以下一次要用fseek移到开头。

### fprintf与fscanf

```c
fprintf(FAIL *restrict, const char *restrict, ...);//第一个参数是文件指针，将最后一个参数写入到文件指针中去
fscanf(FAIL *restrict, const char *restrict, ...);//将文件指针中的内容写到一个地方，也就是从输入流写
fprintf(stderr, "Can't open\n");
```

### fseek与ftell

```c
fseek(FAIL *restrict, long, int);
ftell(FAIL *restrict, long, int);//把文件开始处到文件规定处的字节数返回
ftell(fp, 0L, SEEK_SET);//开始到结尾的字节数返回
```

| 模式     | 偏移量起始点 |
| -------- | ------------ |
| SEEK_SET | 文件开始处   |
| SEEK_CUR | 当前位置     |
| SEEK_END | 文件末尾     |

```c
fseek(fp, -10L, SEEK_END);//从文件结尾处回退10个字节
```

### fread与fwrite

```c
size_t fwrite(const void* restrict ptr, size_t size, size_t nmemb, FILE* restrict fp);//将ptr的数据写入文件，数据被分成nmemb块
size_t fread(void* restrict ptr, size_t size, size_t nmemb, FILE* restrict fp);//将文件写入ptr中
```

注意：第二个是单位尺寸，也就是 `sizeof(double)` （每个长度一个double）这种格式的，第三个是尺寸其长度，也就是有几个 `double` 。

```c
#include<stdio.h>
#include<stdlib.h>

int main(void)
{
    char words[10] = { 
        "abcdef"
    };  
    FILE *fp;
    if ((fp = fopen("zjj","a+")) == NULL)
    {   
        fprintf(stderr, "wrong\n");
        exit(EXIT_FAILURE);
    }   
    char ch; 
    while ((ch = getc(fp)) != EOF)
    {   
        putc(ch, stdout);
    }   
    fwrite(words, sizeof(char), 6, fp);
    char k[10];
    fseek(fp, 0L, SEEK_SET);//记得到开头去
    fread(k, sizeof(char), 6, fp);
    puts(k);
    if (fclose(fp) != 0)
        printf("error\n");

    return 0;
}
```

## Day1系统级文件IO

```
gec@ubuntu:/mnt/hgfs/共享文件夹$ man -f open
open (1)             - start a program on a new virtual terminal (VT).
open (2)             - open and possibly create a file or device
open (3posix)        - open a file
gec@ubuntu:/mnt/hgfs/共享文件夹$ man 2 open
```

总头文件：

```c
#include <sys/types.h> //Unix/Linux系统的基本系统数据类型的头文件
#include <sys/stat.h> //
#include <fcntl.h> //
int open(const char * pathname, int flags);
//打开已存在的文件
int open(const char * pathname, int flags, mode_t mode);
int open("1.txt", O_RDWR | O_CREAT, 0777);
//打开文件，没有则创建
int creat(const char * pathname, mode_t mode);
//创建目标文件 
参数：
    pathname：目标文件的所在位置
    flags：权限标志，即是权限的选择
    必选项，三选其一
    只读		O_RDONLY
    只写		O_WRONLY
    可读可写   O_RDWR
    其它选项：或操作
    创建		O_CREAT
    mode：文件权限，创建文件时进行赋予

返回值：
    成功时，返回一个文件描述符，范围是0~1023
    失败时，返回-1

```

读取与写入

```c
#include <unistd.h>
int close(int fd);

参数；
    fd：目标文件的文件描述符

    返回值：
    成功时，返回值为0
    失败时，返回值为-1	
ssize_t read(int fd, void *buf, size_t count);//read	读取目标文件的数据，到程序空间

参数：
    fd：目标文件的文件描述符
    buf：将要用来存放读取数据的空间的地址
    count：想要读取的数据的上限，单位字节

返回值：
    成功时，返回实际读取到的数据的字节数，范围是0~count
    失败时，返回-1

    返回值可以为0吗？
    可以
    返回值为0时，成功了吗？
    成功
    什么情况下，返回值会为0？
    1.目标文件为NULL
    2.文件光标与文件尾重合
    3.count为0	

ssize_t write(int fd, const void *buf, size_t count);    //write	把程序空间的数据，写入目标文件	
	
参数：
    fd：目标文件的文件描述符
    buf：将要写入的数据在程序中的存放地址
    count：想要写入的数据的大小，单位字节

返回值：
    成功时，返回实际写入的数据的字节数，大小是count
    失败时，返回-1

    返回值可以为0吗？
    可以
    返回值为0时，成功了吗？
    成功
    什么情况下，返回值会为0？
    1.count为0	
```

光标移动

```c
#include <sys/types.h>
#include <unistd.h>

off_t lseek(int fd, off_t offset, int whence);//lseek	重定位文件光标

参数：
    fd：目标文件的文件描述符
    offset：相对于参考位置进行偏移方向和偏移量的确定，单位字节
    负数，表示向文件头进行偏移
    正数，表示向文件尾进行偏移
    0，表示相对于参考位置不进行偏移
    whence：选择光标重定位的参考位置
    利用相关的宏定义进行选择
    文件头		SEEK_SET
    文件尾		SEEK_END
    当前位置		SEEK_CUR

返回值：
    成功时，返回文件光标重定位后距离文件头的远近，单位字节
    失败时，返回-1

    例子：
    文件光标会被定位在哪里？返回值是什么？可表示什么意义？
    lseek(fd,0,SEEK_SET);
文件头，返回值为0
    lseek(fd,0,SEEK_CUR);
当前位置，即文件光标重定位前后没有区别，可得到当前光标和文件头之间的数据的多少
    lseek(fd,0,SEEK_END);
文件尾，返回值可表示文件的大小
```

## Day2 开发板LCD的使用

> 宽：800像素
>
> 高：480像素
>
> 像素点位深度：32位----->4 字节 == ARGB

LCD设备文件：/dev/fb0

LCD像素点排列顺序：从左向右 逐行 从上向下

```
操作流程：
	1.在虚拟机中对源程序进行交叉编译（arm-linux-gcc）
	2.把交叉编译生成的可执行程序下载到开发板上
		2.1在开发板的终端输入rx xxx(目标文件)，然后回车
			目标文件即是交叉编译生成的可执行文件
		2.2点击开发板终端上方的“传输”按钮，选择“发送XModem”
		2.3在弹出框中浏览选择目标文件，然后发送
	3.下载到开发板的文件默认是没有执行权限的，所以需要赋予其执行权限后，才能被执行
		chmod 777 xxx(下载的目标文件)
	4.运行目标文件
		./xxx
```

## Day3 映射

> 映射：把目标文件的物理空间映射到程序的内存空间中。

在BMP图片中读到的数据是从左向右，再从下向上的。这与我们写入图片的顺序有一点区别。注意这里是小端存储，低位存储在低地址上。所以我们要用一个3个字节大小的数组读到RGB的值时，就会得到B在第一个字节，R在最高的字节，此时需要将各字节存储到一个固定的位置 。典型就用就是一个int型的数据来接由4个char型ARGB组成的一数据。

```c
int color = BMP[0] | BMP[1] << 8 | BMP[2] << 16 | 0x00 << 24;
```

```c
#include <sys/mman.h>
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);//申请一块连续的堆空间，进行映射
int munmap(void *addr, size_t length);//释放申请的映射空间
		参数：
			addr：映射空间的地址
				一般可填NULL，表示由系统自动分配
			length：映射空间的大小，单位字节
			prot：对映射空间属性的选择设置
				读写属性的选择设置
				PROT_READ
				PROT_WRITE
			flags：对映射空间属性的选择设置
				共享属性的选择设置
				MAP_SHARED
			fd：目标文件的文件描述符
			offset：映射的偏移量
				一般，若非特别说明，偏移量为0
		
		返回值：
			成功时，返回映射的地址
			失败时，返回MAP_FAILED
	
```

把图片写到板子上：

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <unistd.h>
#include <stdio.h>

int main(void)
{
	//打开LCD文件
	char *pathLCDName = "/dev/fb0";
	int openLCDFileData = open(pathLCDName, O_RDWR);
	if (-1 == openLCDFileData)
	{
		printf("Open %s failed!\n", pathLCDName);
		return -1;
	}
	//映射
	int *mapPoint = mmap(NULL, 800 * 480 * 4, PROT_READ | PROT_WRITE, MAP_SHARED, openLCDFileData, 0);
	if (mapPoint == MAP_FAILED)
	{
		printf("mamap LCD failed!\n");
		return -1;
	}
	//打开图片文件
	char *pathBMPName = "./2.bmp";
	int openBMPFileData = open(pathBMPName, O_RDWR);
	if (-1 == openBMPFileData)
	{
		printf("Open %s failed!\n", pathBMPName);
		return -1;
	}
	/*********************图片的像素点是三字节，LCD的像素是四字节******************/
	//前54字节是图片信息
	int seekBMPData = lseek(openBMPFileData, 54, SEEK_SET);

	//装三字节图片的信息
	char bufferBMP[800 * 480 * 3];

	//读图片
	read(openBMPFileData, bufferBMP, sizeof(bufferBMP));

	//转换成四字节的信息
	int bufferLCD[800 * 480];
	printf("Hello\n");
	int i;
	for (i = 0; i < 800 * 480; i++)
	{
		//ARGB
		bufferLCD[i] = (0x00 << 24) | (bufferBMP[i * 3 + 2] << 16) | (bufferBMP[i * 3 + 1] << 8) | (bufferBMP[i * 3]);
	}
	
	int closeBMPData = close(openBMPFileData);
	if (-1 == closeBMPData)
	{
		printf("close %s failed!\n", pathBMPName);
		return -1;
	}
	//写入LCD
	int x, y;
	for (y = 0; y < 480; y++)
	{
		for (x = 0; x < 800; x++)
		{
			*(mapPoint + 800 * y + x) = bufferLCD[(479 - y) * 800 + x];
		}
	}
	int closeLCDData = close(openLCDFileData);
	if (-1 == closeLCDData)
	{
		printf("close %s failed!\n", pathLCDName);
		return -1;
	}

	int mapData = munmap(mapPoint, 800 * 480 * 4);

	return 0;
}

```

BMP图片有一个特点，尽管它的宽和高在头信息中，但是实际的宽必定是4的整数倍，所以实际的数据大多数情况下是比原始的数据要多的。

```c
//由头文件得出宽,得出一行的字节数
line_bytes = weight * 3;
//根据一行的字节数求出应该补齐的字节数
b = (4 - (line_bytes % 4)) % 4;
//总的字节数
all_bytes = (b + weight) * hight;
```



# C语言标准库

## 字符串函数<string.h>

### 1. char *strcpy(char *s, const char *ct)

```c
   1   /* Copyright (C) 1991-2021 Free Software Foundation, Inc.
   2    This file is part of the GNU C Library. 此文件是GNU C 库的一部分
   3 
   4    The GNU C Library is free software; you can redistribute it and/or CNU C 库是开源软件，你可以根据自由软件基金会出版的GNU次级通用公共许可协议（GNU Lesser General Public License）的条款对其进行修改
   5    modify it under the terms of the GNU Lesser General Public
   6    License as published by the Free Software Foundation; either
   7    version 2.1 of the License, or (at your option) any later version.
   8 
   9    The GNU C Library is distributed in the hope that it will be useful,
  10    but WITHOUT ANY WARRANTY; without even the implied warranty of 但没有任何保证，甚至没有暗示的保证
  11    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU 适销性或为了某一个特定的目的
  12    Lesser General Public License for more details.
  13 
  14    You should have received a copy of the GNU Lesser General Public
  15    License along with the GNU C Library; if not, see
  16    <https://www.gnu.org/licenses/>.  */
  17 
  18 #include <stddef.h>
  19 #include <string.h>
  20 
  21 #undef strcpy
  22 
  23 #ifndef STRCPY
  24 # define STRCPY strcpy
  25 #endif
  26 
  27 /* Copy SRC to DEST.  */
  28 char *
  29 STRCPY (char *dest, const char *src)
  30 {
  31   return memcpy (dest, src, strlen (src) + 1);
  32 }
  33 libc_hidden_builtin_def (strcpy)
```

### 2. void *memcpy(char *s, const char *ct, size_t n)

```c
   1 /* Copy memory to memory until the specified number of bytes具体说明的字符数
   2    has been copied.  Overlap is NOT handled correctly.重叠部分不能准确地处理
   3    Copyright (C) 1991-2021 Free Software Foundation, Inc.
   4    This file is part of the GNU C Library.
   5    Contributed by Torbjorn Granlund (tege@sics.se).
   6 
   7    The GNU C Library is free software; you can redistribute it and/or
   8    modify it under the terms of the GNU Lesser General Public
   9    License as published by the Free Software Foundation; either
  10    version 2.1 of the License, or (at your option) any later version.
  11 
  12    The GNU C Library is distributed in the hope that it will be useful,
  13    but WITHOUT ANY WARRANTY; without even the implied warranty of
  14    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
  15    Lesser General Public License for more details.
  16 
  17    You should have received a copy of the GNU Lesser General Public
  18    License along with the GNU C Library; if not, see
  19    <https://www.gnu.org/licenses/>.  */
  20 
  21 #include <string.h>
  22 #include <memcopy.h>
  23 
  24 #ifndef MEMCPY
  25 # define MEMCPY memcpy
  26 #endif
  27 
  28 void *
  29 MEMCPY (void *dstpp, const void *srcpp, size_t len)
  30 {
  31   unsigned long int dstp = (long int) dstpp;
  32   unsigned long int srcp = (long int) srcpp;
  33 
  34   /* Copy from the beginning to the end.  */
  35 
  36   /* If there not too few bytes to copy, use word copy.  */
  37   if (len >= OP_T_THRES)
  38     {
  39       /* Copy just a few bytes to make DSTP aligned.  */
  40       len -= (-dstp) % OPSIZ;
  41       BYTE_COPY_FWD (dstp, srcp, (-dstp) % OPSIZ);
  42 
  43       /* Copy whole pages from SRCP to DSTP by virtual address manipulation,
  44          as much as possible.  */
  45 
  46       PAGE_COPY_FWD_MAYBE (dstp, srcp, len, len);
  47 
  48       /* Copy from SRCP to DSTP taking advantage of the known alignment of
  49          DSTP.  Number of bytes remaining is put in the third argument,
  50          i.e. in LEN.  This number may vary from machine to machine.  */
  51 
  52       WORD_COPY_FWD (dstp, srcp, len, len);
  53 
  54       /* Fall out and copy the tail.  */
  55     }
  56 
  57   /* There are just a few bytes to copy.  Use byte memory operations.  */
  58   BYTE_COPY_FWD (dstp, srcp, len);
  59 
  60   return dstpp;
  61 }
  62 libc_hidden_builtin_def (MEMCPY)
```

