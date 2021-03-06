# 二级C语言错题

## 第一次

### 1.数据流图中带有箭头的线段表示的是( D)。

A) 控制流

B) 事件驱动

C) 模块调用

D) 数据流

![img](https://raw.githubusercontent.com/little-zhengjj/PicGo/master/img/20180916144226259)

### 2.在软件开发中，需求分析阶段可以使用的工具是(B )。

A) N－S图

B) DFD图

C) PAD图

D) 程序流程图

![image-20200811211314245](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20200811211314245.png)**在需求分析阶段可以使用的工具有数据流图DFD图，数据字典DD，判定树与判定表**

### 3.有三个关系R、S和T如下：
由关系R和S通过运算得到关系T，则所使用的运算为(D )。
![img](C:\Users\31787\OneDrive\文档\未来教育考试系统\data\Cpro\Images\ms2-10-1.png)

A) 笛卡尔积

B) 交

C) 并

D) 自然连接

> 自然连接是一种特殊的等值连接，它要求两个关系中进行比较的分量必须是相同的属性组，并且在结果中把重复的属性列去掉，所以根据T关系中的有序组可知R与S进行的是自然连接操作。

**这是数据库的知识，简单来说，就是找到两个表中同名的列，只保留一个，然后根据原先表中的关系，创建一个新表。此题同名列B与A、C有关创建新表。**

**笛卡尔积**

![image-20200811213350981](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20200811213350981.png)

有点像乘积，取A中每个元素与B中每个进行配对，组成一个有序的新集合。

### 4.以下选项中，能用作数据常量的是(A )。

A) 115L

B) 0118

C) 1.5e1.5

D) o115

**长整型可以，B中八进制没有8，C幂不能是小数，o作为前缀没有这一表示。**

### 5.设有定义：`int` x＝2;，以下表达式中，值不为6的是( A)。

A) 2*x,x+=2

B) x++,2*x

C) x*＝(1＋x)

D) x*＝x＋1

**D中+的优先级比*=更高**

### 6.有以下程序：

```c
#include <stdio.h>
int main(void)
{
    int x, y, z;
    x = y = 1;
    z = x++, y++, ++y;
    printf("%d, %d, %d\n", x, y, z);

    return 0;
}
```


   程序运行后的输出结果是( C)。

   A) 2,3,3

   B) 2,3,2

   C) 2,3,1

   D) 2,2,1

**这里涉及到逗号运算符，当几个表达示用逗号隔开时，它们的值是最后一个的值，但是这里`=`的优先级更高，所以先`z = x++`。**

```c
#include <stdio.h>

int main(void)
{
    int a1, a2, b = 2, c = 7, d = 5; 
    a1 = (++b, c--, d + 3);          // a1 = d + 3
    a2 = ++b, c--, d + 3;            // a1 = ++b
    printf("a1 = %d  a2 = %d", a1, a2);//a1 = 8  a2 = 4
    return 0;
}
```

### 7.有以下程序：

```c
#include <stdio.h>
int fun(int x，int y)
{ 
 if (x！＝y) return ((x＋y)/2)；
 else return (x)；
}
main()
{ 
 int a＝4，b＝5，c＝6；
 printf("%d\n"，fun(2*a,fun(b,c)));
}
```

A) 6

B) 3

C) 8

D) 12

**看错。。。。。**

### 8. 在一个C源程序文件中所定义的全局变量，其作用域为( A)。

A) 由具体定义位置和extern说明来决定范围 

B) 所在程序的全部范围

C) 所在函数的全部范围

D) 所在文件的全部范围

 【解析】全局变量的作用域是从声明处到文件的结束。所以选择A)

### 9.有以下程序：

```c
#include <stdio.h>
#define PT 3.5；
#define S(x) PT*x*x；
main()
{ 
 int a＝1，b＝2；  
 printf("%4.1f\n"，S(a＋b))； 
}
```

程序运行后的输出结果是(C )。

A) 7.5

B) 31.5

C) 程序有错无输出结果

D) 14.0

【解析】宏定义不是C语句，末尾不需要有分号。所以语句`printf("%4.1f\n"，S(a＋b))；`展开后为`printf("%4.1f\n" ,3.5；*a＋b*a＋b；)；`所以程序会出现语法错误。

### 10. 有以下程序：

```c
#include <stdio.h>
#include <stdlib.h> 
main()
{ 
 int *a，*b，*c；
 a＝b＝c＝(int *)malloc(sizeof(int))；
 *a＝1；*b＝2，*c＝3；
 a＝b；
 printf("%d，%d，%d\n"，*a，*b，*c)；
}
```

【解析】malloc函数动态分配一个整型的内存空间，然后把函数返回的地址用(int*)强制类型转换为整型指针，再把它赋给a，b，c，即让指针变量a，b，c都指向刚申请的内存空间。所以只有最后一个赋值语句*c＝3的值保留在了该空间内，因为a，b，c三个指针变量均指向该空间，所以打印该空间内的数值为3。

![image-20200811220911332](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20200811220911332.png)

错了10题，总结为以下这几个方面

| 知识点                       | 题号     |
| ---------------------------- | -------- |
| 不了解                       | 1，2 ，3 |
| 数据类型                     | 4        |
| 运算符，优先级               | 5，6，10 |
| 粗心                         | 7        |
| 基本知识（作用域，编译错误） | 8， 9    |

## 难题

还有几题当时遇到的比较难的。

### 1.以下语句中存在语法错误的是( A)。

A) `char ss[6][20]； ss[1]＝ "right？"；`

B) `char ss[][20]＝{ "right？"}；`

C) `char *ss[6]； ss[1]＝ "right？"；`

D)` char *ss[]＝{ "right？"};`

**字符串(数组）不可以整体赋值（可以初始化），所以只能去指针指向它。显然A错。**

### 2. 以下不能将s所指字符串正确复制到t所指存储空间的是(A )

A)` do{*t＋＋＝*s＋＋；}while(*s )；`

B) `for(i＝0；t[i]＝s[i]；i＋＋)；`

C) `while(*t＝*s){t＋＋；s＋＋} `

D) `for(i＝0，j＝0；t[i＋＋]＝s[j＋＋]；)；`

**首先B没有错，C也是对的，D也一样，它们有个共同点就是先判断（赋值）在前，++在后。当s到最后时，会赋给t，而A不同，当s到'\0'时会跳出，没有赋级t。`\0`的的ASCII码是0。**

**这两题都是字符串。**

## 第二次

### 1.支持子程序调用的数据结构是(A )。

A) 栈

B) 树

C) 队列

D) 二叉树

> 【解析】栈支持子程序调用。栈是一种只能在一端进行插入或删除的线性表，在主程序调用子函数时要首先保存主程序当前的状态，然后转去执行子程序，最终把子程序的执行结果返回到主程序中调用子程序的位置，继续向下执行，这种调用符合栈的特点，因此本题的答案为A)。

题意没有理解，用这个数据结构以支持子程序调用的方法，而不是栈是调用来的

### 2.下面叙述中错误的是( A)

A) 软件测试的目的是发现错误并改正错误

B) 对被调试的程序进行"错误定位"是程序调试的必要步骤

C) 程序调试通常也称为Debug

D) 软件测试应严格执行测试计划，排除测试的随意性

> 【解析】软件测试的目的是为了发现错误而执行程序的过程，并不涉及改正错误，所以选项A)错误。程序调试的基本步骤有：错误定位、修改设计和代码，以排除错误、进行回归测试，防止引进新的错误。程序调试通常称为Debug，即排错。软件测试的基本准则有：所有测试都应追溯到需求、严格执行测试计划，排除测试的随意性、充分注意测试中的群集现象、程序员应避免检查自己的程序、穷举测试不可能、妥善保存测试计划等文件。

**A选项没有改正错误**

### 3.数据库应用系统中的核心问题是( A)。

A) 数据库设计

B) 数据库系统设计

C) 数据库维护

D) 数据库管理员培训

**常识错误**

### 4.以下叙述中错误的是( A)。

A) 使用三种基本结构构成的程序只能解决简单问题

B) 结构化程序由顺序、分支、循环三种基本结构组成

C) C语言是一种结构化程序设计语言

D) 结构化程序设计提倡模块化的设计方法

**B选项中分分支就是选择**

### 5.C源程序中不能表示的数制是( D)。

A) 十六进制

B) 八进制

C) 十进制

D) 二进制

**C源程序就是你自己写的，在源代码里二进制不被理解**

### 6.以下选项中，能用作用户标识符的是( )。

A) _0_

B) 8_8

C) void

D) unsigned

**标识符由数字，字母， 下划线构成，首字母不能是数字**

### 7.有以下程序：
```c
#include <stdio.h>
main()
{ 
 int a1，a2；char c1，c2；
 scanf("%d%c%d%c"，&a1，&c1，&a2，&c2)；
 printf("%d，%c，%d，%c"，a1，c1，a2，c2)；
}
```


若想通过键盘输入，使得a1的值为12，a2的值为34，c1的值为字符a，c2的值为字符b，程序输出结果是：12,a,34,b 则正确的输入格式是(以下□代表空格，<CR>代表回车)( )。

A) 12□a34□b<CR>

B) 12□a□34□b<CR>

C) 12,a,34,b<CR>

D) 12a34b<CR>

【解析】在输入多个数据时，若格式控制串中无非格式字符，则认为所有输入的字符均为有效字符。所以应按选项D)的顺序输入数据。

**也就是你输入的空格也会被当作%c。**

### 8.以下不构成无限循环的语句或语句组是( A)。

A) 
n＝0；

do { ＋＋n；} while (n<＝0)；

B) 
n＝0；

while (1) { n＋＋；}

C) 
n＝10； 

while (n)；{ n－－；}

D) 
for(n＝0，i＝1；；i＋＋) n＋＝i；

**看清楚C`while`后面有分号`;`**

### 9. 有以下程序

```c
#include <stdio.h>
main()
{ 
 int c＝0，k；
 for(k＝1；k<3；k＋＋)
  switch(k)
  {
   default：c＋＝k；
   case 2：c＋＋；break；
   case 4：c＋＝2；break；
  }
 printf("%d\n"，c)；
}
```


程序运行后的输出结果是(C )。

A) 7

B) 5

C) 3

D) 9

【解析】向switch语句块传送参数后，编译器会先寻找匹配的case语句块，找到后就执行该语句块，遇到break跳出；如果没有匹配的语句块，则执行default语句块。case与default没有顺序之分。所以第一次循环k的值为1，执行c＋＝k，c的值为1，再执行case 2 后的语句c＋＋，c的值为2，遇到break语句跳出循环；第二次循环k的值为2，执行case 2 后面的语句c＋＋，c的值为3，跳出循环。

**switch是先找case然后向下走 ，所以写`switch`语句要注意这些case的顺序**



### 10.若有定义语句：double a，*p＝&a；以下叙述中错误的是( A)。

A) 定义语句中的*号是一个间址运算符

B) 定义语句中的*号是一个说明符

C) 定义语句中的p只能存放double类型变量的地址

D) 定义语句中，*p＝&a把变量a的地址作为初值赋给指针变量p

【解析】在变量定义double a，`*`p＝&a；中，`* `号是一个指针运算符，而非间址运算符，所以A)错误。

**`*`号这里没有仔细想，指针初始化时的`*`应该就是指针运算符的意思，而间址运算符是间接改变地址运算符时用的。**

### 11.以下定义数组的语句中错误的是(B )。

A)` int num[][3]＝{ {1,2},3,4,5,6 }；`

B) `int num[2][4]＝{ {1,2}，{3,4}，{5,6} }；`

C)` int num[]＝{ 1,2,3,4,5,6 }；`

D) `int num[][4]＝{1,2,3,4,5,6}；`

**一下就可以看出B不对，B有三行，A值得考虑，因为不知道A的行数，所以看后面可以满足需要吗，它是固定三列不变，所以可以1，2 ，0， 3，4，5，6，0，0。三行**

**还有一点要注意的是C语言中二维数组按行存储，所以用`sizeof`可以很简单求出行数（总地址长除单行地址长）**

### 12. 有以下程序段：

```c
 #include <stdio.h>
 int j；float y；char name[50]；
 scanf("%2d%3f%s"，&j，&y，name)；
```


当执行上述程序段，从键盘上输入55566 7777abc后，y的值为( A)。

A) 566.0

B) 55566.0

C) 7777.0

D) 566777.0

【解析】它是格式输入函数，即**按用户指定的格式从键盘上把数据输入到指定的变量之中**。其中的格式命令可以说明最大域宽。 在百分号(%)与格式码之间的整数用于限制从对应域读入的最大字符数。所以j的值为55，y的值为566.0，字符数组name为7777abc。

### 13.下列语句组中，正确的是(A )。

A) `char *s；s＝"Olympic"；`

B) `char s[7]；s＝"Olympic"；`

C) `char *s；s＝{"Olympic"}；`

D) `char s[7]；s＝{"Olympic"}；`

解析】字符型指针变量可以用选项A)的赋值方法：char*s；s＝"Olympic"，选项C)的写法：char*s，s＝{"Olympic"}；是错误的。字符数组可以在定义的时候初始化：char s[]＝{"Olympic"}；？ 或者char s[]＝"Olympic"，都是正确的。但是不可以在定义字符数组后，对数组名赋值。(数组名是常量，代表数组首地址)所以选项B)和选项D)都是错误的。对于本例，选项B)、D)中字符数组s的大小至少为8，才能存放下字符串。(字符串的末尾都有结束标志"\0")。

**字符串要想给整体给数组，只能在定义时。不能在定义完了将字符串给数组名 **

### 赋值与初始化

“变量赋值”发生在运行期，其写法遵循赋值语法规定。
“变量初始化”发生在编译期或运行期，其写法遵循初始化列表语法规定。

这里字符串就可以理解为`const *p`指针，它不可以被改变，但可以去用别的指针指向它。数组名是个`char *const p`，它不可以指向别处了，所以不允许赋值。而普通指针可以赋值的原因是它不是去改变这个字符串，只是改变这个指针的指向。那么我们就可以去思考`strcpy`函数的原理，获得一个常量指针，然后用一个普通指针指向这个地址再通过这个指针中改变地址上的值。

从这点来看数组与指针是不同的。



### 14.有以下函数：
```c
int fun(char *s)
{ 
 char *t＝s；
 while(*t＋＋)；
 t--;
 return(t－s)；
}
```


该函数的功能是( A)

A) 计算s所指字符串的长度

B) 比较两个字符串的大小

C)  计算s所指字符串占用内存字节的个数

D)  将s所指字符串复制到字符串t中

**没有`sizeof`就没有字节**

### 15.设有定义：
`struct complex
{ int real，unreal；} data1＝{1,8}，data2；`
则以下赋值语句中错误的是( A)。

A) data2＝(2,6)；

B) data2＝data1；

C) data2.real＝data1.real；

D) data2.real＝data1.unreal；



### 16. 有以下程序：

```c
#include <stdio.h>
main()
{ 
 FILE *fp；int a[10]＝{1,2,3}，i，n；
 fp＝fopen("d1.dat"，"w")；
 for(i＝0；i<3；i＋＋) fprintf(fp，"%d"，a[i])；
 fprintf(fp，"\n")；
 fclose(fp)；
 fp＝fopen("d1.dat"，"r")；
 fscanf(fp，"%d"，&n)；
 fclose(fp)；
 printf("%d\n"，n)；
}
```


程序的运行结果是(D )。

A) 321

B) 12300  

C) 1

D) 123

【解析】程序首先将数组a[10]中的元素1、2、3分别写入了文件d1.dat文件中，然后又将d1.dat文件中的数据123，**整体写入**到了变量n的空间中，所以打印n时输出的数据为123。

| 知识点                               | 题号         |
| ------------------------------------ | ------------ |
| 不了解                               | 2，3，4      |
| 数据类型                             |              |
| 运算符，优先级                       | 10           |
| 粗心                                 | 7，8，11，15 |
| 基本知识（作用域，编译错误）掌握不牢 | 6，9，13，14 |
| 题意没有理解                         | 1，5         |
| 格式化输入/输出                      | 12           |

## 第三次

### 1.下列叙述中正确的是(B )。

A) 线性表的链式存储结构与顺序存储结构所需要的存储空间是相同的

B) 线性表的链式存储结构所需要的存储空间一般要多于顺序存储结构

C) 线性表的链式存储结构所需要的存储空间一般要少于顺序存储结构

D) 线性表的链式存储结构与顺序存储结构在存储空间的需求上没有可比性

**数据域与指针域**

### 2.数据库设计中反映用户对数据要求的模式是( C)。

A) 内模式

B) 概念模式

C) 外模式

D) 设计模式

> 【解析】数据库系统的三级模式是概念模式、外模式和内模式。概念模式是数据库系统中全局数据逻辑结构的描述，是全体用户公共数据视图。外模式也称子模式或用户模式，它是用户的数据视图，给出了每个用户的局部数据描述，所以选择C)。内模式又称物理模式，它给出了数据库物理存储结构与物理存取方法。

### 3.以下选项中可用作C程序合法实数的是 (B)

A) 3.0e0.2

B)  .1e0

C) E9

D) 9.12E

> 【解析】A选项中E后面的指数必须为整数?C语言规定,E之前必须要有数字,所以C选项错误?E后面必须要有数字,且必须为整数,所以D选项错误。

### 4.下列定义变量的语句中错误的是 (D)

A) int_int;

B) double int_;

C) char For;

D) float US$;

> 【解析】C语言规定,变量命名必须符合标识符的命名规则。D选项中包含了非法字符"$",所以错误。标识符由字母、数字或下划线组成,且第一个字符必须是大小写英文字母或者下划线,而不能是数字。大写字符与小写字符被认为是两个不同的字符,所以For不是关键字for。

### 5.以下选项中不能作为C语言合法常量的是(B)

A) 0.1e+6

B) 'cd'

C) "\a"

D) '\011'

**cd不是一个字符而一个字符串，应当用双引号括起来**

### 6.有以下程序

```c
#include<stdio.h>

int main(void)
{
    int k = 5, n = 0;
    do
    {
        switch (k)
        {
        case 1:
        case 3:
            n += 1;
            k--;
            break;
        default:
            n = 0;
            k--;
        case 2:
        case 4:
            n += 2;
            k--;
            break;
        }
        printf("%d", n);

    } while (k > 0 && n < 5);
}
```


程序运行后的输出结果是(C)

A) 02356

B) 0235

C) 235

D) 2356

**注意n=5时循环已经跳出了**

### 7.以下程序段完全正确的是 (C)

A) int *p;  scanf("%d", &p);

B) int *p;  scanf("%d", p);

C) int k, *p=&k; scanf("%d", p);

D) int k, *p; *p=&k; scanf("%d", p);

**指针没有初始化，得先给它一个地址值**

### 8.设有定义：(B)
double a[10],*s=a;
以下能够代表数组元素a[3]的是

A) (*s)[3]

B) *(s+3)

C) *s[3]

D) *s+3

**想清楚再写**

### 9.以下选项中正确的语句组是(B)

A) char *s; s={"BOOK!"};

B) char *s; s="BOOK!";

C) char s[10]; s="BOOK!";

D) char s[]; s="BOOK!";

**上面有解释**

### 10.有以下程序：

```c
#include <stdio.h>
int f(int x)
{   
 int y;  
 if(x==0||x==1) return (3);  
 y=x*x-f(x-2);  
 return y;
}
main()
{   
 int z;  
 z=f(3);   
 printf("%d\n",z);
}
```


程序的运行结果是(C)

A) 0

B) 9

C) 6

D) 8

**看仔细** 