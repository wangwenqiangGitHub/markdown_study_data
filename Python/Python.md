```
ctrl / 注释
ctrl Alt L 格式化代码
```



# Python

```python
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
优美 > 丑陋
明确 > 隐晦 
简单 > 复杂
复杂 > 繁复 
扁平 > 嵌套
稀疏 > 拥挤
可读性很重要
固然代码的实用性比洁癖更重要，
所谓的“特例”也往往没有特殊到必须违背上述规则的程度
除非必要，否则不要无故忽视异常
如果遇到模棱两可的逻辑，请不要自作聪明地瞎猜。
应该提供一种，且最好只提供一种，一目了然的途径
当然这是没法一蹴而就的，除非你是荷兰人
固然，立刻着手 好过 永远不做。
然而，永远不做 也好过 闷头蛮干
倘若你的实现很难解释，它一定不是个好主意
倘若你的实现一目了然，它可能是个好主意
命名空间大法好，同志们要多多搞！
```

注意：

position表示位置，是一个总称

argument 表示列表中的参数



## 1. 注释

* 单行：#
* 多行：“”“或'''同样不可以嵌套

句尾没有分号。

## 2. 变量

python 不是强类型，可以是整形，浮点，甚至字符串。规则也是字母、数字、下划线，首字母不能是数子，中间不能用空格，不能是关键字。

## 3. 字符串

单引号双引号都可以作为字符串。

### 字符串方法

python对字符串操作提供了很多方法

前面说过变量也可以接收python，这里用string来表示一个字符串。

```python
#首字母大写
string.title()
#全部大写
string.upper()
#全部小写
string.lower()
#删除开头和末尾的空白
string.strip()
#删除末尾的空白 right
string.rstrip()
#删除开头的空白 left
string.lstrip()
```

### 合并字符串

用+号可以合并

```python
stringfinal = string0 + string1
```

用字符串时避免语法错误，因为没有转义字符， 字符串中有单引号就用“”作为字符串的开始和结束，反之也是。

## 4. 数字

整数，浮点数混合运算的结果是浮点数。

整数，整数混合运算的结果是整数。

数字和字符串不能直接全并，可以用str()方法。

```python
age = 23
message = "Happy " + str(age) + "rd Birthday!"
```

## 5. 列表

c语言中的数组类似，print打印就可以直接打印全部的值。

方法：

```python
#添加元素
arrays.append(array) 
#插入元素
arrays.insert(position, array)
#删除元素
del arrays[position]#注意这不是一个方法
arrays.pop(position)#这是一个方法，可以没有参数则会删除最后一个
arrays.remove(arguement)#删除对应参数值的参数
#排序
arrays.sort()#这永久的改变了列表
arrays.sorted()#临时排序
#反转
arrays.reverse()#永久
#长度
len(arrays)
#最大值
max(arrays)
#最小值
min(arrays)
#求和
sum(arrays)
```

创建：

```python
arrays = [array0, array1, array2……]
```

## 6. for循环

与java的for-each语句类似

```python
for array in arrays:
    print(array)
```

冒号别少了，不用大括号语句体用缩进来区分。

### 创建数字列表

```python
for value in range(1, 5):
    print(value)
```

以左闭右开区间创建了一个列表，步长为默认为1

```python
for value in range(1, 5, 2):#步长为2 
    print(value)
```

可以用变量来接收：

```python
numbers = list(range(1, 6))
```

### 表列解析 

简单写法：

```python
squares = [value**2 for value in range(1, 11)]
```

列表中存[1, 10]的平方。

### 切片

> 只用列表中的一部分

```python
arrays[a:b]
arrays[:b]#从头开始b-1结束
arrays[a:]#从a开始尾结束
arrays[-a:]#打印最后a个
```

同样也是左闭右开。

### 复制列表

```python
arrays0 = array1[:]
```

复制不是赋值，用切片来

### 元组

与列表不同的是，其用圆括号标识，而且其中的元素不能被修改。

```python
dimensions = (200, 500)
dimensions[0] = 0 #禁止
```

如果想修改，就要改变其整体。

```python
dimensions = (100, 400 )
```

## 7. if语句

与C不同点：

1. and代表&，or代表||
2. 语句后用冒号
3. 语句体要缩进

### 检查值是否在列表中

```python
banned_users = ['andrew', 'carolina', 'david']
user = 'marie'
if user in banned_users:
    True
if user not in banned_users:
    false
```

## 8. 字典

> 在python中字典是一系列的键值对。每个键都与一个值相关联。

```python
alien_0 = {'color': "green", "point": 5}
print(alien_0['color'])
#green
```

`color`就是键， `green`就是值。

### 添加键值对

```python
alien_0['x_position'] = 0
alien_0['y_position'] = 25
#{'color': 'green', 'point': 5, 'x_position': 0, 'y_position': 25}
```

### 修改键值对

与数组一样相似，根据键来修改值。

### 删除键值对

```python
del alien_0['points']
```

### 遍历字典

```python
me = {
    "first_name" : "Jingjing",
    "last_name" : "Zheng",
    "City" : "AnQing"
}
for key, value in me.items():
    print("\nKey:" + key)
    print("Value:" + value)
```

for中声明两个变量来接收，后面的字典则是调用了一个方法。

==返回顺序和存储顺序不一定相同，Python只关心键与值的关联关系。==

#### 遍历所有键

```python
for key in me.keys():
    print("Key:" + key)
for key in me:
    print("Key:" + key)

```

同理，一个参数，调用keys()方法。也可以省略keys()方法，默认会遍历键。

#### 遍历所有值

```python
for value in me.values():
    print("value:" + value)
```

这样不同键相同值也会被打印出来。

```python
for value in set(me.values()):
    print("value:" + value)
```

用set()方法，创建了一个集合。

### 嵌套

> 可以在列表中嵌套字典、在字典中嵌套列表、甚至在字典中嵌套字典

#### 字典列表

```python
alien_0 = {'color':'green', 'point':5}
aliens = [alien_0, alien_0, alien_0]
for alien in aliens:
    print(alien)
```



#### 字典中存储列表

```python
favorite_languages = {
    'jen':['python', 'ruby'],
    'sarah':['c'],
    'edward':['ruby', 'go'],
    'phil':['python', 'haskell'],
}
for name, languages in favorite_languages.items():
    print(name.title() + "'s favorite languages are:")
    for language in languages:
        print("\t" + language.title())
```

## 9. 用户输入

```python
name = input("Please enter your name: ")
print("Hello, " + name + "!")
```

`input()`输入会被python解读为字符串。可以用强制类型转换来改变其类型。

### 强制类型转换

```python
age = int(age)
```

### while

```python
while 循环条件:
   	循环体
```

## 10.函数 

### 关键字实参

> 传递给函数的名称-值对，直接在实参中将名称和值关联起来。 

```python
def describe_pet(animal_type, pet_name):
    print("I have a " + animal_type + ".")
    print("My " + animal_type + "'s name is " + pet_name.title() + ".")
    
describe_pet(animal_type = "dog", pet_name = "zheng")

```

关键字实参调用时顺序是无关紧要的。 

### 等效的函数调用

> 可以给形参提供默认值

```python
def describe_pet(pet_name, animal_type='dog'):
    print("I have a " + animal_type + ".")
    print("My " + animal_type + "'s name is " + pet_name.title() + ".")
    

describe_pet('while')

```

但是不要将有默认值的参数放在没有默认值参数的前面。

### 返回值

> 直接返回

```python
def describe_pet(name, animal_type, pet_name='while'):
    print("I have a " + animal_type + ".")
    print("My " + animal_type + "'s name is " + pet_name.title() + ".")
    return name.title()


print(describe_pet('z', 'while'))

```

### 可选实参

> 提供默认值，如果有实参，那就用实参， 否则就用默认值。

### 传递列表

```python
def get_name(names):
    for name in names:
        print("hello".title() + " " + name.title() + "!")


user_names = ["hh", "ll", "kk"]
get_name(user_names)

#Hello Hh!
#Hello Ll!
#Hello Kk!

```

### 传递副本 

```python
function_name(list_name[:])
```

### 传递任意数量的实参

```python
def make_pizze(*toppings):
```

同样的任意数量的实参应该放在最后。

### 使用任意数量的关键字实参

```python
def build_profile(first, last, **user_info)
```

### 将函数存储模块中并导入

```python
import 文件名
文件名.函数名
```

#### 导入特定函数

```python
fromm module_name import function_name_0, function_name_1
```

### 给函数或模块指定别名

`make_pizza`指定别名为`mp`

```python
from module_name import function_name as mp
from module_name as p
```

### 导入模块中的所有函数

```python
from module_name import *
```

### 函数编写指南

> 函数名就应该是描述性名称并只用小写字母和下划线

> 给形参指定默认值时，等号两边不要有空格，调用关键字实参，也应该这样做。

## 11. 类

### 创建类

dog.py

```python
class Dog():
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def sit(self):
        print(self.name.title() + " is now sitting")
```

`class Dog()`：定义一个名为Dog的类，类名请大写

`_init_（slef)`：这与java中的构建方法相同， 就是用来初始化实例地象的方法 ， 但是与java中隐式的创建了一个指向实例本身的引用不同，这里必须要写出`self`，它是一个指向实例本身的引用。让实例能访问类中的属性和方法。

### 创建实例调用方法

```python
my_dog = Dog("while", 6)
my_dog.sit()
my_dog.roll_over()
```

### 给属性指定默认值

```python
class Dog():
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.host = "Make"

    def sit(self):
        print(self.name.title() + " is now sitting")

    def roll_over(self):
        print(self.name.title() + "rolled over!")

    def get_host(self):
        print("the host is " + self.host + "!")


my_dog = Dog('while', 6)
my_dog.sit()
my_dog.roll_over()
my_dog.get_host()

```

也就是在初始化方法里加一个参数而已。

### 修改属性的值

#### 直接修改

```python
class Dog():
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.host = "Make"

    def sit(self):
        print(self.name.title() + " is now sitting")

    def roll_over(self):
        print(self.name.title() + "rolled over!")

    def get_host(self):
        print("the host is " + self.host + "!")

    def get_age(self):
        print("the dog's age is " + str(self.age) + "!")


my_dog = Dog('while', 6)
my_dog.get_age()
my_dog.age = 15
my_dog.get_age()


the dog's age is 6!
the dog's age is 15!
```



#### 通过方法

```python
class Dog():
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.host = "Make"

    def sit(self):
        print(self.name.title() + " is now sitting")

    def roll_over(self):
        print(self.name.title() + "rolled over!")

    def get_host(self):
        print("the host is " + self.host + "!")

    def set_age(self, age):
        self.age = age

    def get_age(self):
        print("the dog's age is " + str(self.age) + "!")


my_dog = Dog('while', 6)
my_dog.get_age()
my_dog.set_age(15)
my_dog.get_age()


the dog's age is 6!
the dog's age is 15!

```

### 继承

> 创建子类时， 父类必须包含在当前文件中，且位于子类前面。

```python
class Animal(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.host = "Make"

    def sit(self):
        print(self.name.title() + " is now sitting")

    def roll_over(self):
        print(self.name.title() + "rolled over!")

    def get_host(self):
        print("the host is " + self.host + "!")

    def set_age(self, age):
        self.age = age

    def get_age(self):
        print("the animal's age is " + str(self.age) + "!")


class Dog(Animal):
    def __init__(self, name, age, host):
        super().__init__(name, age)
        self.host = host
   


my_dog = Dog('while', 6)
my_dog.get_age()
my_dog.get_host()

```

子类可以不写初始化方法，与上面的结果相同。但是最好不要这样，保持python自己的特性。

### 给子类定义属性和方法

```python
class Animal(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.host = "Make"
	……

class Dog(Animal):
    def __init__(self, name, age):
        super().__init__(name, age)
        self.foots = 4

    def describe_foots(self):
        print("The " + self.name + " have " + str(self.foots) + "!")


my_dog = Dog('while', 6)
my_dog.describe_foots()

```

### 将实例作为属性

```python
class Animal(object):...


class Cry():
    def __init__(self, cry="汪汪汪"):
        self.cry = cry
        
    def get_cry(self):
        return self.cry


class Dog(Animal):
    def __init__(self, name, age):
        super().__init__(name, age)
        self.foot = 4
        self.cry = Cry()

    def describe_foot(self):
        print("The " + self.name + " have " + str(self.foots) + "!")

    def describe_cry(self):
        print("The " + self.name + " have " + self.cry.get_cry() + " cry !")


my_dog = Dog('while', 6)
my_dog.describe_cry()

```

弱类型省了很多麻烦。 

### 导入类

单个类导入：

```python
from car import Car #模块Car中的类car被导入
```

导入多个类：

```python
from car import Car, ElectricCar
```

导入所有类

```python
from car import *
```

## 12. 文件和异常

读取文件：

```python
with open('file_path') as file_object:
    contents = file_object.read()
    print(contents)
```

关键字`with`表示在不用时python会帮你关闭文件。用`read()`方法读取这个文件的全部内容。

Linux路径是分隔符/

Windows路径分隔符是\，单引号前加r，就不会被认为是转义字符

### 逐行读取

```python
filename = 'pi.txt'
with open(filename) as file_object:
    for line in file_object:
        print(line.rstrip())
```

可以看出for就有了`read()`函数的功能。

### 把各行放到列表中

```python
filename = 'pi.txt'
with open(filename) as file_object:
    lines = file_object.readlines()
    for line in lines:
        print(line.rstrip())
```

### 使用文件的内容

```python
file_path = 'pi.txt'
with open(file_path, 'rb') as file_object:
    lines = file_object.readlines()

pi_string = ''
for line in lines:
    pi_string += line.rstrip()

print(pi_string)
print(len(pi_string))

```

### 写入文件

```python
file_path = 'pi.txt'
with open(file_path, 'w') as file_object:
    file_object.write("I love programming.")
```

Python只能写入字符串，可以用`str()`函数改掉所有的数字。

`open(file_path, 模式)

模式有读r, 写w，附加a

#### 写入多行

用\n换行符放在字符串中。

## 13. 异常

```python
try:
    可以出现异常的代码块
except ZeroDivisionError:
    出现分母为0异常的相应处理
else:
    try成功执行后需要执行的代码
```

如果可能出现异常的代码块没有出现问题，Python将跳过后面的except语句，如果有错误将查找相应的except代码块运行其中的代码。

`pass`语句就什么也不做。

### 存储数据

用模块json存储数据。

### json.dump()和json.load()

```python
import json

numbers = [2, 3, 5, 7, 11, 13]

filename = 'numbers.json'
with open(filename, 'w') as f_obj:
    json.dump(numbers, f_obj)

```

```python
import json

filename = 'numbers.json'
with open(filename) as f_obj:
    numbers = json.load(f_obj)

print(numbers)

```

json.dump(a, b)将列表a写入b文件中。

json.load(b)读出b文件中的内容，返回一个列表。 

### 重构

```python
import json


def get_new_username():
    username = input("Please enter your name!")
    filename = 'username.json'
    with open(filename, 'w') as file_obj:
        json.dump(username, file_obj)
    return username


def get_stored_username():
    filename = 'username.json'
    try:
        with open(filename) as f_obj:
            username = json.load(f_obj)
    except FileNotFoundError:
        return None
    else:
        return username


def greet_user():
    username = get_stored_username()
    if username:
        print("welcome back, " + username + "!")
    else:
        username = get_new_username()
        print("We'll remember you when you come back!")


greet_user()

```

将所有功能分成三个函数来实现，每一个函数都只实现一个功能。

## 14.测试

### 测试类（测试是一个动词）

```python
import unittest
from name_function import get_formatted_name


class NamesTestCase(unittest.TestCase):
    def test_first_last_name(self):
        formatted_name = get_formatted_name('janis', 'joplin')
        self.assertEqual(formatted_name, 'Janis Joplin')


unittest.main

```

测试导入unittest包，导入方法， 测试方法在测试类中， 以test开头。

`survey.py`

```python
class AnonymousSurvey():
    def __init__(self, question):
        self.question = question
        self.responses = []

    def show_question(self):
        print(self.question)

    def store_response(self, new_response):
        self.responses.append(new_response)

    def show_results(self):
        print("Survey results:")
        for response in self.responses:
            print("- " + response)
```

`test_survey.py`

```python
import unittest
from survey import AnonymousSurvey


class TestAnonymousSurvey(unittest.TestCase):
    def test_stroe_single_resopnse(self):
        question = "What languages did you first learn to speak?"
        my_survey = AnonymousSurvey(question)
        responses = ['English', 'Spanish', 'Mandarin']
        for response in responses:
            my_survey.store_response(response)

        for response in responses:
            self.assertIn('English', my_survey.responses)
 

unittest.main

```

### setUp()

可以在方法里面创建要测试的对象，这样就不用每个方法都搞一个对象了。

```python
import unittest
from survey import AnonymousSurvey


class TestAnonymousSurvey(unittest.TestCase):
    def setUp(self):
        question = "What languages did you first learn to speak?"
        self.my_survey = AnonymousSurvey(question)
        self.responses = ['English', 'Spanish', 'Mandarin']

    def test_stroe_single_resopnse(self):
        for response in self.responses:
            self.my_survey.store_response(response) 
        for response in self.responses:
            self.assertIn(response, self.my_survey.responses)

unittest.main
```

从这里开始执行：

```python
if __name__ = "__main__"
	函数
```

## 15. 正则表达式

![image-20210127134704044](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210127134704044.png)

![image-20210127135018202](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210127135018202.png)

1、. 匹配任意除换行符“\n”外的字符；
2、\*表示匹配前一个字符0次或无限次；
3、?表示前边字符的0次或1次重复
4、+或\*后跟？表示非贪婪匹配，即尽可能少的匹配，如*？重复任意次，但尽可能少重复；
5、 .\*? 表示匹配任意数量的重复，但是在能使整个匹配成功的前提下使用最少的重复。
如：a.\*?b匹配最短的，以a开始，以b结束的字符串。如果把它应用于aabab的话，它会匹配aab和ab。

## 爬豆瓣

```python
import re
from bs4 import BeautifulSoup  # 网页解析，获取数据
import urllib  # 正则表达式， 进行文匹配
import urllib.request, urllib.error  # 制定URL，获取网页数据


# import xlwt  # 进行excel操作
# import sqlite3  # 进行SQLite数据库操作

def main():
    # 爬取网页
    baseurl = 'https://movie.douban.com/top250?start='
    data_list = getData(baseurl)


# findall()使用时，正则中有括号就是输出括号中的内容，用两个括号就是一个list两个元组
# 全局变量
find_link = re.compile(r'<a href="(.*?)">')  # 创建正则表达式对象，表示规则（字符串模式）
find_name = re.compile(r'<img alt="(.*?)"')
find_img = re.compile(r'<img.*src="(.*?)"', re.S)  # 让换行符包含在字符中


# 3. 保存数据
def getData(baseurl):
    data_list = []
    for i in range(0, 10):
        url = baseurl + str(i * 25)
        html = askURL(url)  # 保存获取到的网页源码

        # 2. 逐一解析数据
        soup = BeautifulSoup(html, "html.parser")
        # 查找div，后面class的性属性是item的项
        for item in soup.find_all('div', class_="item"):  # 查找符合要求的字符串，形成列表
            data = []  # 保存每部电影的所有信息
            item = str(item)

            link = re.findall(find_link, item)[0]  # re库通过正则表达式查找指定的字符串,只要第一个
            name = re.findall(find_name, item)[0]
            img = re.findall(find_img, item)[0]
            with open("douban.html", "a", encoding="utf-8") as fp:
                fp.write(name + "\n" "图片地址: " +  img + "\n网址:" + link + "\n")

    return data_list


def askURL(url):
    # 伪装
    head = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36"
    }
    # 请求
    request = urllib.request.Request(url, headers=head)

    try:
        # 回复
        response = urllib.request.urlopen(request)
        html = response.read().decode("utf-8")
    except urllib.error.URLError as e:
        if hasattr(e, "code"):
            print(e.code)
        if hasattr(e, "reason"):
            print(e.reason)
    # 返回
    return html


if __name__ == "__main__":
    main()

```

# 爬虫

## 爬虫基础

### HTTP基本原理

> URL: Uniform Resource Locator 统一资源定位符
>
> URI: Uniform Resource Identifier 统一资源标志符
>
> URN: Uniform Resource Name 统一资源名称

![image-20210201120543655](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210201120543655.png)

目前互联网中，URN用得非常少，几乎所有的URI都是URL。

### 超文本

> 其英文名为hypertext，我们在浏览棉里看到的网 页就是超文本解析而成的， 其网页源代码是一系列 HTML代码，里面包含了一系列标签，比如 img 显 示图片， p 指定显示段落等。 浏览器解析这些标签后，便形成了我们平常看到的网页，而网页的源代 码 HTML就可以称作超文本。

> HTTP: Hyper Text Transfer Protocol 超文本传输协议。 HTTP协议是用于从 网络传输超文本数据到本地浏览器的传送协议。
>
> HTTPS: Hyper Text Transfer Protocol over Secure Socket Layer，是 HTTP 的安全版，是以安全为目标的 HTTP 通道， 即 HTTP 下加入 SSL 层，简称为 HTTPS

### HTTP请求过程

在浏览器中输入一个URL，浏览器向网站所在的服务器发送了一个请求，网站服务器收到后进行处理和解析，返回对应的响应，接着传回给浏览器，这个响应有页面源码，浏览器再对这个响应进行解析生成网页。

![image-20210201122609766](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210201122609766.png)

* 第一列 Name：请求的名称，一般会将 URL 的最后一部分内容当作名称。 

* 第二要列Status ：响应的状态码，这里显示为 200 ， 代表响应是正常的。 通过状态码，我们可 以判断发送了请求之后是杏得到了正常的响应。
* 第三列Type ： 请求的文梢类型。 这里为 document，代表我们这次请求的是一个HTML文档， 内容就是一些 HTML代码。
* 第四列 Initiator ： 请求源。 用来标记请求是由哪个对象或进程发起的。
* 第五列 Size ： 从服务器下载的文件和请求的资源大小。 如果是从缓存中取得的资源，则该列 会显示台om cache。 
* 第六列 Time ： 发起请求到获取响应所用的总时间。 
* 第七列 Waterfall：网络请求的可视化瀑布流。

点击每个网址：可以得下面的内容

![image-20210201122839769](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210201122839769.png)

* General：
  * Request URL：就是请求的URL
  * Request Method：请求的方法
  * Status Code：响应状态码
  * Remote Address：远程服务器的地址和端口
  * Referrer Policy：Referrer判别策略 
* Response Headers：
* Reuqest Headers:

### 请求

客户端向服务端发出，分为4部分：

* Request Method

  * GET方法：请求中的参数包含在URL中，数据可以在URL中看到。数据最多只有1024byte。
  * POST方法：通过表单形式传输，会包含在请求体中，数据没有限制。

  一般登录时提交用户名和密码时，GET会不安全，所以多用POST，上传文件时，文件内容比较大，也会用POST方式。

  ![image-20210201125138525](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210201125138525.png)

* Request URL：就是URL，唯一确定我们请求的资源。

* Request Headers：
  * Accept：请求报头域，指定客户端可接受哪些类型的信息
  * Accept-Language：指定客户端可接受的语言类型
  * Accept-Encoding：指定客户端可接受的内容编码
  * Host：指定请求资源的主机IP和端口号，内容就是URL的原始服务器或网关的位置。HTTP1.1版本，请求必须有些内容
  * Cookies：网站为了辨别用户进行会话跟踪而存储在用户本地的数据。用户登录用的。
  * Referer：标识请求是从哪个页面发过来，可以用来做源统计、防盗链处理。
  * User-Agent： 可以使服务器识别客户使用的操作系统 及版本 、 浏览器及版本等信息。 在做爬虫时加上此信息，可以伪装为浏览器。
  * Content-Type：用来表示具体请求中的媒体类型信息。

* Request Body：请求体，POST请求中的表单数据，GET中没有。

### 响应

* 状态码：

  ![image-20210201132426383](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210201132426383.png)

  ![image-20210201132440293](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210201132440293.png)

* 响应头：
  * Date：标识响应产生的时间
  * Last-Modified：指定资源的最后修改时间
  * Content-Encoding：指定响应内容的编码
  * Sever：包含服务器的信息，名称，版本号等
  * Content-Type：文档类型，指定返回的数据是什么
  * Set-Cookie：设置Cookies，告诉浏览器需要将此内容放在Cookies中，下次请求携带
  * Expires：指定响应的过期时间，可以使代理服务器或浏览器将加载的内容更新到缓存中。 如 果再次访问时，就可以直接从缓存中加载， 降低服务器负载，缩短加载时间
* 响应体：响应的正文数据，我们对其进行解析。

### 网页基础

#### 组成

1. HTML：骨架

   > 描述网页的一种语言，Hyper Text Markup Language，网页中的复杂元素用其中的各种标签来表示。

2. JavaScript：肌肉

   > 与用户动态交互。动画，进度条，提示框等。

3. CSS：皮肤

   > Cascading Style Sheets层叠样式表，使页面更美观。

#### 网页结构与结点

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>This is a Demo</title>
</head>
<body>
<div id=”container”>
    <div class＝、rapper”>
        <h2 class=”title”>Hello World</h2>
        <p class＝”text”>Hello, this is a paragraph. </p>
    </div>
</div>
</body>
</html>
```

![image-20210201134544253](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210201134544253.png)

在HTML中所有标签定义的内容都是节点，构成了一个HTML DOM树。

* 整个文档是一个文档节点。 
* 每个 HTML元素是元素节点。
*  HTML元素内的文本是文本节点。 
* 每个 HTML 属性是属性节点。 
* 注释是注释节点

![image-20210201134838852](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210201134838852.png)

#### 选择器

![image-20210201135917326](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210201135917326.png)

![image-20210201135939075](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210201135939075.png)

### 无状态HTTP

> HTTP协议对事务处理是没有记忆能力的。

会话：会话对象在服务器端用来存储特定用户所需是属性及配置信息。

Cookies：存储在本地终端上的数据。可以理解为保存了登录的凭证。

### Cookies属性

* Name ： 该 Cookie 的名称。一旦创建，该名称便不可更改。 
* Value ： 该 Cookie 的值。 如果值为 Unicode字符，需要为字符编码。 如果值为二进制数据，则 需要使用 BASE64 编码
* Domain ：可以访问该Cookie的域名。 例如，如果设置为. zhihu.com，则所有以 zhihu .com结尾 的域名都可以访问该 Cookie。
* Max Age ： 该 Cookie失效的时间， 单位为秒，也常和 Expires一起使用，通过它可以计算出其有效时间。 Max Age 如果为正数，则该 Cookie 在 Max Age秒之后失效。 如果为负数，则关闭 浏览器时 Cookie 即失效，浏览器也不会以任何形式保存该 Cookie 。
* Path ： 该 Cookie 的使用路径。 如果设置为／path/ ，则只有路径为／path/的页面可以访问该 Cookie。 如果设置为/则本域名下的所有页面都可以访问该 Cookie。
*  Size 字段 ： 此 Cookie 的大小。 
*  HTTP 字段： Cookie 的 httponly 属性。 若此属性为 true ，则只有在 HTTP 头中会带有此 Cookie 的信息，而不能通过 document.cookie 来访问此 Cookie。
* Secure ： 该 Cookie 是否仅被使用安全协议传输。 安全协议有 HTTPS 和 SSL 等，在网络上传 输数据之前先将数据加密。 默认为 false 。

会话Cookie将Cookies放在内存中，持久Cookie会放在客户端的硬盘中。

### 代理

本机向代理服务器发出请求，然后由代理服务器发送给Web服务器，Web的响应经由代理服务器再返回给本机，实现了本机的IP伪装。避免同一个IP访问过于频繁的问题。

#### 代理分类：根据协议区分

* FTP 代理服务器： 主要用于访问 FTP 服务器， 一般有上传、 下载以及缓存功能，端口一般为 21 、 212 1 等。
* HTTP 代理服务器： 主要用于访问网页，一般有内容过滤和缓存功能，端口一般为 80 、 8080 、 3128 等。
* SSL厅LS 代理： 主要用于访问加密网站， 一般有 SSL 或 TLS 加密功能（最高支持 128 位加密 强度），端口一般为 443 。
* RTSP 代理： 主要用于访问 Real 流媒体服务器， 一般有缓存功能，端口一般为 554 。 
* Telnet代理： 主要用于telnet远程控制（黑客人侵计算机时常用于隐藏身份），端口一般为 23 。 
* POP3/SMTP 代理： 主要用于 POP3/SMTP方式收发邮件， 一般有缓存功能，端口一般为 110/25。 
* SOCKS 代理： 只是单纯传递数据包，不关心具体协议和用法，所以速度快很多 ， 一般有缓 存功能，端口一般为 1080。 SOCKS代理协议又分为 SOCKS4和 SOCKS5 ，前者只支持 TCP, 而后者支持 TCP 和 UDP，还支持各种身份验证机制、服务器端域名解析等。 简单来说， SOCKS4 能做到的 SOCKS5 都可以做到，但 SOCKS5 能做到的 SOCKS4不一定能做到

#### 代理分类：根据匿名程序区分

* 高度匿名代理： 会将数据包原封不动地转发，在服务端看来就好像真的是一个普通客户端在 访问，而记录的 IP 是代理服务器的 IP
* 普通匿名代理： 会在数据包上做一些改动， 服务端上有可能发现这是个代理服务器，也有一定几 率追查到客户端的真实E。 代理服务器通常会加入的Hπ？头有 HTTP VIA和 HTTP X FOR帆DED FOR。 
* 透明代理： 不但改动了数据包 还会告诉服务器客户端的真实 IP。 这种代理除了能用缓存技 术提高浏览速度，能用内容过滤提高安全性之外，并无其他显著作用，最常见的例子是内网 巾的硬件防火墙。 
* 间谍代理： 指组织或个人创建的用于记录用户传输的数据，然后进行研究 、 监控等目的的代 理服务器。

### 常见代理设置

* 使用网上的免费代理：
* 使用付费代理服务：
* ADSL拨号：拨一次号换一次IP，稳定性高

## 基本库的使用

urllib库

```python
request ： 它是最基本的 HTTP 请求模块，可以用来模拟发送请求。 就像在浏览器里输入网挝 然后回车一样，只需要给库方法传入 URL 以及额外的参数，就可以模拟实现这个过程了。
error ： 异常处理模块，如果出现请求错误 ， 我们可以捕获这些异常，然后进行重试或其他操 作以保证程序不会意外终止。
parse ： 一个工具模块，提供了许多 URL处理方法，比如拆分、解析、 合并等。 
robotparser：主要是用来识别网站的 robots.txt 文件，然后判断哪些网站可以爬，哪些网站不 可以爬，它其实用得比较少。
```

重点是前三个模块。

### 发送请求

#### urllib.request.urlopen()

> 此模块提供了最基本的构造HTTP请求的方法，可以利用它模拟浏览器的一个请求发起过程。

```python
import urllib.request

response = urllib.request.urlopen('https://www.python.org')
print(response.read().decode('utf-8'))
#decode 解码
```

两行代码就可以得到python官网的源代码。

```python
import urllib.request

response = urllib.request.urlopen('https://www.python.org')
print(type(response)) 
#<class 'http.client.HTTPResponse'>
```

响应的类型是一个HTTPResponse对象，调用read()方法可以得到返回网页内容，调用status属性可以得到结果的状态码。

```python
import urllib.request

response = urllib.request.urlopen('https://www.python.org')
print(response.status)
print(response.getheaders())
print(response.getheader('Server'))
''''''
200
[('Connection', 'close'), ('Content-Length', '49263'), ('Server', 'nginx'), ('Content-Type', 'text/html; charset=utf-8'), ('X-Frame-Options', 'DENY'), ('Via', '1.1 vegur, 1.1 varnish, 1.1 varnish'), ('Accept-Ranges', 'bytes'), ('Date', 'Mon, 01 Feb 2021 10:07:04 GMT'), ('Age', '1160'), ('X-Served-By', 'cache-bwi5121-BWI, cache-hkg17925-HKG'), ('X-Cache', 'HIT, HIT'), ('X-Cache-Hits', '2, 3600'), ('X-Timer', 'S1612174024.322009,VS0,VE0'), ('Vary', 'Cookie'), ('Strict-Transport-Security', 'max-age=63072000; includeSubDomains')]
nginx
''''''
```

可以得到响应头，还可以得到头中的特定值。

```python
urllib.request.urlopen(url, data=None, [ timeout, ]*, cafile=None, capath=None, cadefault=False, context=None )
```

urlopen()方法可以带很多参数。

##### data参数：

如果它是字节流编码格式的内容，即 bytes 类型，则需要通过 bytes()方法转化。 另外，如果传递了这个参数，则它的请求方式就不再是 GET方式，而 是 POST方式。

```python
import urllib.request
import urllib.parse
#parse 语法

data = bytes(urllib.parse.urlencode({'word':'hello'}), encoding='utf-8')
response = urllib.request.urlopen('http://httpbin.org/post', data=data)
print(response.read())
''''''
{

	"args": {}, 
	"data": "",   "files": {}, 
	"form": {
		"word": "hello"
	}, 
	"headers": {
		"Accept-Encoding": "identity", 
		"Content-Length": "10", 
		"Content-Type": "application/x-www-form-urlencoded", 
		"Host": "httpbin.org", 
		"User-Agent": "Python-urllib/3.9", 
		"X-Amzn-Trace-Id": "Root=1-6017d47b-3902b4735ab8489273e5ea47"
	}, 
	"json": null, 
	"origin": "120.242.255.64", 
	"url": "http://httpbin.org/post"
}"

''''''
```

```python
urllib.parse.urlencode(dir) #将字典转化为字符串
bytes(str, encoding) #指定编码格式，转码成字节流类型
```

传递的参数出在了form字段中，模拟表单提交的方式，以POST方式传输数据。

##### timeout参数

设置时间，单位为秒，超过设定时间就会抛出异常，如果不指定就会使用全局默认时间。

```python
import urllib.request
import urllib.error
import socket 
#socket 电源

try:
    response = urllib.request.urlopen('http://httpbin.org/get', timeout=0.001)
except urllib.error.URLError as e:
    if isinstance(e.reason, socket.timeout): #如果是超时异常
        print('TIME OUT')
    
```

#### urllib.request.Request()

```python
import urllib.request

request = urllib.request.Request('https://www.python.org')
response = urllib.request.urlopen(request)
print(response.read().decode('utf-8'))
```

可以看出与前面的不同， 多了一个request参数，这样可以更灵活的进行配置。

```python
class urllib.request.Request ( url, data=None, headers={}, origin_req_host=None, unverifiable=False, method=None)
```

* 第一个参数：URL
* 第二个参数：必须传bytes字节流类型，如果是字典，可以用urllib.parse模块里的urlencode()编码
* 第三个参数：headers，是一个字典，就是请求头，通常设置为浏览器里的User-Agent
* 第四个参数：请求方的host名称或IP地址
* 第五个参数：表示这个请求是否是无法验证的，默认是False，表示用户没有足够权限来选择接收这个请求的结果。例如，我们请求一个 HTML 文档中的图片，但是我 们没有向动抓取图像的权限，这时 unverifiable 的值就是 True。
* 第六个参数：method是字符串，用于指示请求所用的方法，GET，POST，PUT等



```python
from urllib import request, parse

url = 'http://httpbin.org/post'
headers = {
    'User-Agent ': 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)',
    'Host': 'httpbin.org '
}
dict = {
    'name': 'Germey'
}
data = bytes(parse.urlencode(dict), encoding='utf8 ')
req = request.Request(url=url, data=data, headers=headers, method='POST')
response = request.urlopen(req)
print(response.read().decode('utf - 8'))
''''''
{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "name": "Germey"
  }, 
  "headers": {
    "Accept-Encoding": "identity", 
    "Content-Length": "11", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "User-Agent": "Python-urllib/3.9,Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)", 
    "X-Amzn-Trace-Id": "Root=1-6017f4c6-5adf8ffc54fa7a9c73306037"
  }, 
  "json": null, 
  "origin": "120.242.255.64", 
  "url": "http://httpbin.org/post"
}

''''''

```

### 添加代理

```python
from urllib.error import URLError
from urllib.request import ProxyHandler, build_opener

proxy_handler = ProxyHandler({
    ' http': ' http://127.o.o .1:9743 ',
    ' https ': ' https://127.0 .0.1:9743 '
})
opener = build_opener(proxy_handler)#创建代理
try:
    response = opener.open(' https: // www.baidu.com') #用open方法
    print(response.read().decode(' utf-8'))
except URLError as e:
    print(e.reason)

```

在本地搭建了一个代理，运行在9743端口。

这里使用了 ProxyHand ler，其参数是一个字典，键名是协议类型（比如 HTTP 或者 HTTPS 等）， 键值是代理链接，可以添加多个代理。 然后，利用这个 Handler及 build_opener（）方法构造一个 Opener，之后发送请求即可

### 获取cookies

```python
import http.cookiejar, urllib.request
cookie = http.cookiejar.CookieJar()#声明一个CookieJar对象。
handler = urllib.request.HTTPCookieProcessor(cookie)#利用HTTPCookieProcessor构建一个Handler
opener = urllib.request.build_opener(handler)#最后利用 build_opener（）方法构建出 Opener
response = opener.open('http://www.baidu.com')
for item in cookie:
    print(item.name + "=" + item.value)
```

输出保存为文件

```python
from urllib.request import ProxyHandler
import http.cookiejar, urllib.request

filename = 'cookies.txt'
cookie = http.cookiejar.MozillaCookieJar(filename) #就创建类时不同
#cookie = http.cookiejar.LWPCookieJar(filename)生成LWP格式
handler = urllib.request.HTTPCookieProcessor(cookie)
opener = urllib.request.build_opener(handler)
response = opener.open('http://www.baidu.com')
cookie.save(ignore_discard=True, ignore_expires=True)
```

利用cookies.txt文件LWP格式。

```python
from urllib.request import ProxyHandler
import http.cookiejar, urllib.request

cookie = http.cookiejar.LWPCookieJar()
cookie.load('cookies.txt', ignore_discard=True, ignore_expires=True)
handler = urllib.request.HTTPCookieProcessor(cookie)
opener = urllib.request.build_opener(handler)
response = opener.open('http://www.baidu.com')
print(response.read().decode('utf-8'))

```

### 处理异常

```python
from urllib import request, error

try:
    response = request.urlopen('https://cuiqingcai.com/index.html')
except error.HTTPError as e:
    print(e.reason, e.code, e.headers, sep='\n')
except error.URLError as e:
    print(e.reason)
else:
    print('RequestSuccessfully')

```

### 解析链接

标准链接格式：

`scheme://netloc/path ;params?query#fragment`

协议://域名/路径;参数？查询条件#锚点（定位页面内部的下拉位置）

```python
from urllib.parse import urlparse

result = urlparse('http://www.baidu.com/index.html;user?id=5#comment')
print(type(result), result)
''''''
<class 'urllib.parse.ParseResult'> ParseResult(scheme='http', netloc='www.baidu.com', path='/index.html', params='user', query='id=5', fragment='comment')
''''''
```

### 使用requests

```python
import requests

r = requests.get('https://www.baidu.com/')
print(type(r))
print(r.status_code)
print(type(r.text))
print(r.text)
print(r.cookies)
```



## Linux基础

### gcc命令详解

> C语言源文件要经过预处理、编译、汇编、链接才可以生成可执行文件

* 预处理

```
gcc -E -main.c -o main.i 
```

-E是仅做预处理，不进行后面三个步骤。这个阶段就是头文件的加入和宏替换。

* 编译

```
gcc -S -main.i -o main.s
```

-S编译到汇编语言，也就是将.i文件生成后缀为.s的汇编语言。

* 汇编

```
gcc -c main.s -o main.o
```

-c编译、汇编到目标代码也就是计算机可以看的懂的二进制，如果你自己打开的话就是一堆乱码。

* 链接

```
gcc main.o -o main.out
```

链接生成可以执行的文件，在Linux下是.out，在Windows下是.exe文件。

-o 后面接上执行该命令后要生成的文件名。

```
gcc -g main.c -main.out
gdb ./main.out
```

-g生成调试信息

```
gcc -w main.c -o main.out不生成任何警告
gcc -Wall -main.c -o main.out生成所有警告 
```

## 自动化任务

> 导入re模块，Python所有正则表达式的函数都在re模块中。

### 模式匹配与正则表达式

```python
import re

# 向re.compile()(编写)传入一个字符串值，表示正则表达式，它将返回一个Regex模式对象(Regex对象)
phoneNumRegex = re.compile(r'\d\d\d-\d\d\d-\d\d\d\d')

# 查找字符串中第一个匹配正则表达式的字符串，返回一个Match对象
mo = phoneNumRegex.search("My number is 415-555-4242.")
# 查找字符串中所有匹配正则表达式的字符串，返回的是列表
mo1 = phoneNumRegex.findall('Cell: 415-555-9999 Work: 212-555-0000')

# mo.group()是输出完整匹配，March对象的group()方法，返回被查找字符串中实际匹配的文本
print('Phone number found: ' + mo.group())
# print(re.compile(r'\d\d\d-\d\d\d-\d\d\d\d').search("123-312-3213").group())
for array in mo1:
    print(array)

#Phone number found: 415-555-4242
#123-312-3213
#415-555-9999
#212-555-0000
```

在字符串前面加上r，就不需要再转义了，表示就为原始字符串。

![image-20210528091739001](https://i.loli.net/2021/05/28/MeszlncAymDZjXQ.png)

### 利用括号分组

```python
import re

# 向re.compile()(编写)传入一个字符串值，表示正则表达式，它将返回一个Regex模式对象(Regex对象)
phoneNumRegex = re.compile(r'(\d\d\d)-(\d\d\d-\d\d\d\d)')

# 查找字符串中第一个匹配正则表达式的字符串
mo = phoneNumRegex.search("My number is 415-555-4242.")

# 查找所有匹配的字符串，返回元组，元组中是列表
#mo1 = phoneNumRegex.findall('Cell: 415-555-9999 Work: 212-555-0000')

# mo.group()是输出完整匹配
print('Phone number found: ' + mo.group(0))  # 传入0返回整个匹配的字符串
print('Phone number found: ' + mo.group(1))  # 传入1返回第一个括号里的字符串
print('Phone number found: ' + mo.group(2))  # 传入2返回第二个括号里的字符串

# mo.groups()返回第一个匹配字符串所组成的元组
areaCode, mainNumber = mo.groups()
print(areaCode)
print(mainNumber)
'''
areaCode, mainNumber = mo1
# 打印第一个元组
print(areaCode)
# 打印元组中的列表
for array in areaCode:
    print(array)

# 打印第二个元组
print(mainNumber)
for array in mainNumber:
    print(array)
'''

'''
Phone number found: 415-555-4242
Phone number found: 415
Phone number found: 555-4242
415
555-4242

('415', '555-9999')
415
555-9999
('212', '555-0000')
212
555-0000
'''
```

### 管道|

```python
import re

# 管道用在希望匹配许多表达式中的一个时，这里要匹配的是Batman或者Tina Fey
heroRegex = re.compile(r'Batman|Tina Fey')

mo1 = heroRegex.search('Batman and Tina Fey.')
# 返回第一个匹配到的字符串
print(mo1.group())

mo2 = heroRegex.search('Tina Fey and Batman.')
print(mo2.group())

# 当多个字符串共同首部时，可以用括号分组
batRegex = re.compile(r'Bat(man|mobile|bile|copter|bat)')
mo = batRegex.search('Batmobile lost a wheel')

# 返回第一个完全匹配的文本Batmobile
print(mo.group())
print(mo.group(0))
# 括号分组内匹配的文本
print(mo.group(1))

'''
Batman
Tina Fey
Batmobile
Batmobile
mobile
'''
```

### 问号

```python
import re

batRegex = re.compile(r'Bat(wo)?man')
# 问号是可选内容，匹配0次或1次
mo1 = batRegex.search('The Adventures of Batman')
print(mo1.group())
mo2 = batRegex.search('The Adventures of Batwoman')
print(mo2.group())

'''
Batman
Batwoman
'''
```

### 星号

```python
import re

batRegex = re.compile(r'Bat(wo)*man')
# 星号匹配0次或多次
mo1 = batRegex.search('The Adventures of Batman')
print(mo1.group())
mo2 = batRegex.search('The Adventures of Batwowoman')
print(mo2.group())

'''
Batman
Batwowoman
'''
```

### 加号

```python
import re

batRegex = re.compile(r'Bat(wo)+man')
# 加号匹配1次或多次
mo1 = batRegex.search('The Adventures of Batman')
try:
    print(mo1.group()) # 没有返回一个None输出会出现异常
except AttributeError:
    print("error")
else:
    print(mo1.group())
mo2 = batRegex.search('The Adventures of Batwoman')
print(mo2.group())
mo3 = batRegex.search('The Adventures of Batwowoman')
print(mo3.group())
'''
error
Batwoman
Batwowoman
'''
```

### 括号限定次数

```python
import re

# 只匹配3个
haRegex = re.compile(r'(Ha){3}')
mo1 = haRegex.search('HaHaHaHafdfd')
print(mo1.group())

# 匹配3个到5个
haRegex = re.compile(r'(Ha){3,5}')
mo1 = haRegex.search('HaHaHaHafdfd')
print(mo1.group())
'''
HaHaHa
HaHaHaHa
'''
```

### 贪心和非贪心匹配

```python
import re

# 匹配3个到5个，默认贪心，匹配最多的
haRegex = re.compile(r'(Ha){3,5}')
mo1 = haRegex.search('HaHaHaHafdfd')
print(mo1.group())

# 匹配3个到5个，加问号非贪心，匹配最少的
haRegex = re.compile(r'(Ha){3,5}?')
mo1 = haRegex.search('HaHaHaHafdfd')
print(mo1.group())
'''
HaHaHaHa
HaHaHa
'''
```

### 字符分类

![image-20210413194523094](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210413194523094.png)

![image-20210413194540107](C:\Users\31787\AppData\Roaming\Typora\typora-user-images\image-20210413194540107.png)

```python
import re

xmasRegex = re.compile(r'\d+\s\w+')
mo = xmasRegex.findall(
    '12 drummers,11 pipers,10 lords,9 ladies,8 maids,7 swans, 6 geese,5 rings,4 birds,3 hens,2 doves,1 partridge')
for array in mo:
    print('[' + array, end=',')
print(']')
'''
[12 drummers,[11 pipers,[10 lords,[9 ladies,[8 maids,[7 swans,[6 geese,[5 rings,[4 birds,[3 hens,[2 doves,[1 partridge,]
'''
```

### 建立自己的字符分类

```python
import re
# 把自己的想要的字符放到放括号中
myself_string = re.compile(r'[aeiou]')
mo = myself_string.findall("faldfhjflffj,afnsfsodfsflajfl")
print(mo)
'''
['a', 'a', 'o', 'a']
'''
```

### ^和$

```python
import re

# ^匹配以后面字符串开头的字符
# ^Hello 匹配以Hello开头的字符串
beginsWithHello = re.compile(r'^Hello')
mo = beginsWithHello.search("Hello world!")
print(mo)

mo = beginsWithHello.search("kfdHello world!")
print(mo)
# 匹配0~9结束的字符串
endsWithNumber = re.compile(r'\d$')
mo1 = endsWithNumber.search('Your number is 42')
print(mo1)
# 匹配开始到结束都是数字的字符串
wholeStringIsNum = re.compile(r'^\d+$')
mo2 = wholeStringIsNum.search('1234567890')
print(mo2)
mo2 = wholeStringIsNum.search('12i34567890')
print(mo2)
'''
<re.Match object; span=(0, 5), match='Hello'>
None
<re.Match object; span=(16, 17), match='2'>
<re.Match object; span=(0, 10), match='1234567890'>
None
'''
```

