# Makefile

## 基本使用和规则

```
target ...: prerequisites ...
	command
	...
	...
```

target是目标文件(obj)，也可以是一个执行文件，还可以是一个标签(label)。

> target这一个或多个目标文件依赖于prerequisites中的文件，其生成规则定义在command中。
>
> prerequisites中如果有一个以上的文件比target文件要新的话，command所定义的命令就会被执行。

现在我的文件下有三个文件，main.c、list.c、head.h，要生成一个可执行文件main和两个二进制文件list.o、main.o

```
main: main.o list.o
	gcc main.o list.o -o main
main.o: main.c
	gcc -c main.c -o main.o
list.o: list.c
	gcc -c list.c -o list.o
```

冒号前面的叫目标文件，后面的是依赖文件，整句就是一个依赖关系。定义好一个依赖关系后，后面就是如何根据依赖文件生成目标文件。以一个Tab键开头。



依赖关系和这个生成目标的方法就是规则。



### 自动推导

make看到一个.o文件，它就会自动的把.c文件加在依赖关系中，并且其命令也会被推导出来。

```
main: main.o list.o
	gcc main.o list.o -o main
main.o:
list.o: 
```

## 使用变量

可以将这些文件定义变量，$(变量)来使用。

```
obj = main.o list.o
target = main
CC = gcc

$(target): $(obj)
	$(CC) $(obj) -o $(target)
$(obj):
```

## 清空目标文件和伪目标

不生成的目标叫做伪目标，比如下面的clean，功能就是执行其后面的命令，而没有生成clean文件。

```
.PHONY:clean
clean:
	-rm -i $(target) $(obj)
```

.PHONY来显式地指明一个目标是伪目标，向make说明，不管是否有这个文件 ，这个目标就是伪目标。rm前面的`-`表示也许某些文件出现问题，但不要管，继续做后面的事。不要把clean放在文件开头（它会变成默认目标）。

当然，你想生成多个目标文件时，可以将伪目标放在文件开头

![image-20201202225126760](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201202225126760.png)

默认目标就是开头的目标，但是它是一个伪目标，所以不会产生all文件，只会产生作为依赖的三个目标，进而去执行对应的命令。可见，目标也可以作为依赖。，伪目标也可以。

![image-20201202225502936](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20201202225502936.png)

三个伪目标，两个作为依赖，去执行这两个依赖所需的命令。

## 使用通配符

make支持三个通配符

1. *代替任意个字符

   ```
   clean:
   	rm -f *.o
   ```

   删除所有.o后缀的文件，*在给变量时就是\*本身的意思。

```
obj = *.o
```

obj的值就是*.o，它不会展开，可以使用函数。

```
obj := $(wildcard *.o)
```

obj =　所有.o后缀的文件展开

```
obj := $(patsubst %.c, %.o, $(wildcard *.c))
```

将所有.c文件转成.o文件给变量obj。%表示匹配0或若干个字符。

2. ？

3. -

所以可以改写

```
obj := $(patsubst %.c, %.o, $(wildcard *.c))
target = main
CC = gcc

$(target): $(obj)
	$(CC) $(obj) -o $(target)
.PHONY:clean
clean:
	-rm -i $(target) $(obj)
```

