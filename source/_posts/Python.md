---
title: Python学习笔记
date: 2023-05-01 12:52:48
categories: 学习笔记
tags: [Python]
top_img: https://pic2.imgdb.cn/item/645106680d2dde57771a4372.jpg
cover: https://pic2.imgdb.cn/item/645106680d2dde57771a4372.jpg
description: Python学习的一些笔记
---

## 前言

写这个笔记的初衷是想要督促自己学习和查看复习的，一般是第二次巩固学习的时候才做的笔记，所以有些地方自己记住了就没写了，写的大多是感觉自己会忘记的东西或者想整理的东西，仅供参考！

# Python基本语法 

## 入门语法

### 变量于标识符

1. Python是一个动态类型的语言，可以为变量赋任意类型的值，也可以任意修改变量的值

2. 不能使用没有进行过赋值的变量

### 命名规范

如果使用不符合标准的标识符，将会报错  SyntaxError: invalid syntax  

1. 下划线命名法
   - 所有字母小写，单词之间使用_分割
   - max_length   min_length   hello_world 

2. 帕斯卡命名法（大驼峰命名法）
   - 首字母大写，每个单词开头字母大写，其余字母小写
   - MaxLength   MinLength   HelloWorld   XxxYyyZzz  

### 数值

1. Python中的整数的大小没有限制，可以是一个无限大的整数

2. 二进制 0b开头 。 八进制 0o开头。十六进制 0x开头

3. 浮点数（小数），在Python中所有的小数都是float类型

   - 对浮点数进行运算时，可能会得到一个不精确的结果

     ```  
      c = 0.1 + 0.2 # 0.30000000000000004
     ```

- 为什么0,1+0.2不是0.3呢？问题的根源是十进制小数转为二进制小数的过程中，会损失精度,其实是在计算机的运行中 0.1+0.2是0.1的近似值和0.2的近似值的相加

### 字符串 

引号可以是双引号，也可以是单引号

1. 可以使用三重引号来表示一个长字符串，三重引号可以换行，并且会保留字符串中的格式

``` python
s = '''锄禾日当午，
汗滴禾下土，
谁知盘中餐，
粒粒皆辛苦'''
```

2. 转义 ： \uxxxx 表示Unicode编码

3. 字符串之间也可以进行加法运算，但只限于相同类型的进行加法运算

4. 在创建字符串时，可以在字符串中指定占位符 

   ``` py
   b = 'Hello %s'%'孙悟空'
   b = 'hello %3.5s'%'abcdefg' # %3.5s字符串的长度限制在3-5之间
   print(b)# hello abcde
   ```

5. 格式化字符串，可以通过在字符串前添加一个f

   ``` py
   a="Hello"
   b="World"
   c = f'hello {a} {b}'
   print(c) # hello Hello World
   print(f'a = {a}') # a = Hello
   ```

6. 字符串和列表相互转化

   ```py
   # 列表转换为字符串的最常见方法是使用join()方法
   #语法 string.join(iterable) iterable中的值应是String类型 iterable -任何可迭代的-列表，元组，集合
   flexiple = ["Hire", "the", "top", "freelancers"]
   print(" ".join(flexiple))# Hire the top freelancers " "为分隔符
   #如果列表中有数字的话，要用join+map函数,先把列表转换为字符，再用join转换
   print(" ".join(map(str,flexiple))
   #字符串转列表 list 或 split
   ```

7. split()和splitlines（）

   split方法函数可以分割字符串成列表，默认是以空格作为分隔符来分割字符串。

   splitlines是按行分割字符串，返回值也是个列表。

   ```python
   #split
   s = "www jeapedu com" 
   print s.split()  
   #splitlines()
   # 如果括号中为True 结尾就保留\n
   t =  """www.jeapedu.com
          www.chinagame.me
          www.quanzhan.org
        """
   print (t.splitlines())
   print (t.splitlines(True) )
   ```

   

### 类型的检查与转换

type()可以用于检查值的类型，可用变量来接收返回值

``` python
a = 123 # 数值
b = '123' # 字符串
c = type('123') 
print(type(c))# <class 'type'>
print(type(b))# <class 'str'>
print(type(1)) # <class 'int'>
```

类型转换四个函数 int() 、float()、 str()、 bool()

- bool() 可以将对象转换为布尔值，任何对象都可以转换为布尔值，

- 规则：对于所有表示空性的对象都会转换为False，其余的转换为True。 哪些表示的空性：0 、 None 、 '' 、。。。

- int()转化不了字符串形式的浮点型

  ``` python
  a = '11.5'
  # a = int(a)
  print(a) # ValueError: invalid literal for int()
  ```

### 算数运算符 

- 加法(字符串进行加法运算进行拼串操作)

  ```python
  a = 10 + 5 # 计算
  a = 'hello' + ' ' + 'world' # 拼串
  ```

- 乘法(如果将字符串和数字相乘，进行复制操作，将字符串重复指定次数)

  ```python
  a = "Hello" * 5
  ```

- 除法: / ，返回浮点类型 。整除: //，返回整型

- 幂运算：** 。 求一个值的几次幂

-  ==  ， != 

  ```python
  # == ，!= 比较的的是对象的值(value)是否相等
  ```

  

### 关系运算符 

- is , is not 比较的是两个对象是否为同一个，比较的是对象的id

  ```python
  result = 1 is True # False
  result = 1 is not True # True
  print('result =',result ) # True
  print(id(1),id(True)) # id函数用于获取对象的内存地址
  ```

  

- 在Python中可以对两个字符串进行大于（等于）或小于（等于）的运算  

  - 当对字符串进行比较时，实际上比较的是字符串的Unicode编码

  - 比较两个字符串的Unicode编码时，是逐位比较的

  - 如果不希望比较两个字符串的Unicode编码，则需要将其转换为数字然后再比较

    ``` python
    result = '0061' > '0062'
    print(result)
    ```

###  逻辑运算符

- not 逻辑非

  - 对符号右边的值进行非运算
  - 对于非布尔值，非运算会先将其转化为布尔值再取反

- and 逻辑与

  - 双1 为 1 ， Python中的与运算符是短路运算符，如果第一个为False，则不看第二个值

  - 如何是非布尔值的运算，第一个值是False，则直接返回第一个值，否则返回第二个值

    ``` python
    result = 1 and 0 # 0
    result = 0 and None # 0
    ```

    

- or 逻辑或

  - 双0 为 0 ，Python中的或运算是短路的或，如果第一个值为True，则不再看第二个值

  - 如果是非布尔值的运算，如果第一个值是True，则直接返回第一个值，否则返回第二个值

    ``` python
    result = 1 or 0 # 1
    result = 0 or None # None
    ```

### 条件运算符（三元运算符）

语法： 语句1 if 条件表达式 else 语句2

```python
a = 30
b = 50
max = a if a > b else b # 50
```

### 运算符优先级

py中and 的 优先级比 or高，如果优先级一样则自左向右计算，关于优先级的表格，你知道有这么一个东西就够了，千万不要去记。在开发中如果遇到优先级不清楚的,则可以通过小括号来改变运算顺序

```python
result = 1 < 2 < 3 # 相当于 1 < 2 and 2 < 3
```

![](https://pic2.imgdb.cn/item/644fb3df0d2dde57776c1bb5.png)

## 流程控制语句

### 条件判断(if-else语句)

```py
# if-else语句
# 语法： 
#   if 条件表达式 :
#       代码块
#   else(elif) :
#       代码块
```

###  input函数

该函数用来获取用户的输入，注意：input()函数返回值是一个字符串

```py
#实现input单行输入多个值
#split方法函数可以分割字符串成列表，默认是以空格作为分隔符来分割字符串。
#map()让 输入的列表元素都转为int类型
gender, height, weight = map(int, input().split())  # 输入性别、身高和体重
```



###  循环语句

#### while循环

```python
# 语法：
#   while 条件表达式 :(条件表达式为True时执行)
#       代码块
#   else :
#       代码块
#-------------------------------
# 小练习:获取用户输入的任意数,判断其是否为质数
num = int(input('输入一个任意的大于1的整数：'))
i=2
# 创建一个变量,用来记录num是否为质数
flag = true;
while i< num :
    if num % i ==0 :
        flag =false
    i+=1
if flag : 
    print(num, '是质数')
else :
    print(num , '不是质数')

```

#### break，continue，pass

break跳出循环,continue跳出当前次循环 不做解释了。pass 用来在判断或循环语句中占位的

```python
i = 0
if i < 5:
    pass #占位 
```

#### for循环

在说for循环之前先说说 range()函数，可以用来生成一个自然数的序列

```python
# 该函数需要三个参数
#   1.起始位置（可以省略，默认是0）
#   2.结束位置
#   3.步长（可以省略，默认是1）
r = range(5) # 生成一个这样的序列[0,1,2,3,4]
r = range(10,0,-1)
print(list(r))
```

通过range()可以创建一个执行指定次数的for循环

```python
for i in range(30):
    print(i)
for s in 'hello':
    print(s)    
```

用for循环来遍历列表

```python
# 语法：
#   for 变量 in 序列 :
#       代码块
# for循环的代码块会执行多次，序列中有几个元素就会执行几次
#   每执行一次就会将序列中的一个元素赋值给变量，
#   所以我们可以通过变量，来获取列表中的元素
for s in stus :
    print(s)
```



#### 质数练习优化

```py
# 模块，通过模块可以对Python进行扩展
# 引入一个time模块，来统计程序执行的时间
from time import *
begin = time()
i = 2
while i <= 100000:
    flag = True
    j = 2 
    while j <= i ** 0.5: # 循环到 根号i
        if i % j == 0:
            flag = False
            # 一旦进入判断，则证明i一定不是质数，此时内层循环没有继续执行的必要
            # 使用break来退出内层的循环
            break
        j += 1
    if flag :
        # print(i)  
        pass
    i += 1
# 获取程序结束的时间
end = time()

# 计算程序执行的时间
print("程序执行花费了：",end - begin , "秒")
```

## 序列

###  列表

```python
# 创建列表，通过[]来创建列表
my_list = [] # 创建了一个空列表

# 列表中可以保存任意的对象,里面的数据叫元素
my_list = [10,'hello',True,None,[1,2,3],print]

# 如果使用的索引超过了最大的范围，会抛出异常
#  print(my_list[5]) IndexError: list index out of range

# 列表的索引可以是负数
# 如果索引是负数，则从后向前获取元素，-1表示倒数第一个，-2表示倒数第二个 以此类推
# print(stus[-2])
```

列表的切片操作，切片指从现有列表中，获取一个子列表

```python
# 语法：列表[起始:结束] 
stus = ['a','b','c','d','e','f']
print(stus[1:3])
# 语法：列表[起始:结束:步长]  步长表示，每次获取元素的间隔，默认值是1，不能为0
# 如果是负数，则会从列表的后部向前边取元素
print(stus[::-1]) #相当于逆序输出列表
# 通过切片来修改列表

stus[0:2] = ['a','b'] #使用新的元素替换旧元素
stus[0:0] = ['c'] # 向索引为0的位置插入元素
# 当设置了步长时，序列中元素的个数必须和切片中元素的个数一致
stus = ['a','b','c','d','e','f']
stus[::2] = ['q','w','e']
# 通过切片来删除元素
# del stus[::2]
# 以上操作，只适用于可变序列
```

切片操作注意点

- 通过切片获取的元素，包扣起始位置的元素，但不包括结束位置的元素
- 做切片操作是返回一个子列表，不会影响原本的列表
- 起始和结束位置的所以都可以省略，省略结束位置，则一直截取到最后，省略起始位置，则从第一个元素开始，如果都省略就相当于创建了列表的一个副本

### 通用操作

1. 加号 + 可以将两个列表拼接为一个列表 ，*  可以将列表重复指定次数

2.  in 和 not in 检查元素是否存在或者不存在列表中

   ```py
   stu = [10,20,30,40,50]
   print('20' not in stu)
   ```

3.  len()函数 获取列表的长度或者元素的个数

4. max() , min() 获取列表中最大最小值

5. s.index() 获取指定元素在列表中的第一次出现时索引

   s.sout() 统计指定元素再列表中出现的次数

   ```python
   # 语法: index(需要查找的元素，查找的起始位置，查找的结束位置)
   stu = [10,20,30,40,50]
   print(stu.index(30,1,4)) # 2
   # 如果获取列表中没有的元素，会抛出异常
   print(stu.count(30)) # 1
   ```

6. del 删除列表中的元素

   ```python
   del stus[2] # 删除索引为2的元素
   ```

7. list()函数 ，将其他序列转化成list

   ```python
   s = 'hello'
   # s[1] = 'a' 不可变序列，无法通过索引来修改
   # 可以通过 list() 函数将其他的序列转换为list
   s = list(s)
   print(s)

8. append() : 向列表的最后添加一个元素

   ```py
   stus = ['a','b','v','e']
   stus.append('t')
   ```

9. insert() : 像列表指定位置插入一个元素

   ```python
   stus = ['a','b','v','e']
   # 1. 要插入的位置 ， 2.要插入的元素
   stus.insert(1,'f')
   ```

10. extend() : 使用新的序列来拓展当前的序列

    ```python
    # 需要一个序列作为参数，它会将该序列中的元素添加到当前列表中
    stus = ['a','b','v','e']
    stus.extend(['f','g'])
    ```

11.  clear() : 清空序列

    ```python
    stus.clear()
    ```

12.  pop() : 根据索引删除并返回被删除的元素 (del 不能返回，它可以)

    ```python
    stus = ['a','b','v','e']
    a =stus.pop(1)
    # a = stus.pop() # 删除最后一个
    print(stus)
    print(a)
    ```

13.  remove() :  删除指定元素，如果相同值的元素有多个，则只会删除第一个

    ```python
    stus = ['a','a','b','v','e']
    stus.remove('a')
    print(stus)
    ```

14. reverse() : 用来反转列表

    ```py
    stus = ['a','a','b','v','e']
    stus.reverse()
    ```

15. sort() : 用来对列表中的元素进行排序，默认是升序排列

    ```python
    my_list = list('asnbdnbasdabd')
    #如果需要降序排列，则需要传递一个reverse=True作为参数
    my_list.sort(reverse=True)
    print(my_list)
    # key需要一个函数作为参数
    sort(key = len) #根据长度来进行排序
    # sorted()
    # 这个函数和sort()的用法基本一致，但是sorted()可以对任意的序列进行排序
    #并且使用sorted()排序不会影响原来的对象，而是返回一个新对象
    l = [2,5,'1',3,'6','4']
    print('排序前:',l)
    print(sorted(l,key=int))
    print('排序后:',l)
    ```

### 元组

元组 tuple， 元组是一个不可变的序列，操作和列表基本一致

一般当我们希望数据不改变时，就用元组

```python
# 创建元组 使用()来创建元组
my_tuple = (1,2,3,4,5)
# 元组是不可变对象，不能为元组中的元素重新赋值
# my_tuple[3] = 10 TypeError: 'tuple' object does not support item assignment
# 当元组不是空元组时，括号可以省略
my_tuple = 10,20,30,40
```

### 解包操作

解包指就是将字符串，列表，元组(暂且知道这几个)当中每一个元素都赋值给一个变量

```py
my_tuple = 10 , 20 , 30 , 40
a,b,c,d = my_tuple # 尝试打印出每个变量之后，a=10，b=20 .....
# 交互a 和 b的值，这时我们就可以利用元组的解包
a = 100
b = 300
a , b = b , a #就能实现值的交换
# 在对一个元组进行解包时，变量的数量必须和元组中的元素的数量一致
# 也可以在变量前边添加一个*，这样变量将会获取元组中所有剩余的元素,变成一个列表
a , b , *c = my_tuple # a=10 ，b=20 , c=[30,40]
a , *b , c = my_tuple # a=10 , b=[20,30], c=40
# 也可以对字符串，列表解包 ,但不能同时出现两个或以上的*变量
a , b , *c = [1,2,3,4,5,6,7]
a , b , *c = 'hello world'
print('a =',a)
print('b =',b)
print('c =',c)
# 对元组进行解包操作
def fn(a,b,c) :# 分别打印a,b,c
t={10,20,30}
fn(*t) # 在前面添加* 来实现解包
# 对字典的解包
d={'a':199,'b':200,'c':300}
fn(**d) # 通过在参数前面添加** 来实现解包
```

### 可变对象

- 每一个对象都保存了三个数据 ： 

  ​	id（标识）

  ​	type(类型)

  ​	value(值)

- 列表就是个可变对象

- 改对象

  ```python
  a = [1,2,3]
  a[0] = 10 # 改对象
  b=a
  # 这个操作是通过变量去修改对象的值
  # 这个操作不会去改变变量所指向的对象
  # 当我们去修改对象时，如果其他变量也指向了该对象，则修改也会在其他变量中体现
  b[0] =10
  # a和b的对象一样，所以b修改对象的值之后，输出a ，a[0]也会被改变
  ```

  ![](https://pic2.imgdb.cn/item/6454b00f0d2dde5777140ee5.png)

- 改变量

  ```python
  a = [1,2,3]
  a[0] = 10 # 改对象
  b= [4,5,6]
  # 这种操作是给变量重新赋值
  # 此操作会改变变量的对象
  # 为一个变量重新赋值时，不会影响其他的变量
  ```

![](https://pic2.imgdb.cn/item/6454b00f0d2dde5777140f07.png)

- 一般只有在为变量赋值的时候才是修改变量，其余大部分修改对象

###  字典的概念与含义

- 字典属于一种新的数据结构，称为映射（mapping）

- 字典的作用和列表类似，都是用来存储对象的容器

- 列表存储数据的性能很好，但是查询数据的性能的很差

- 在字典中每一个元素都有一个唯一的名字，通过这个唯一的名字可以快速的查找到指定的元素

- 在查询元素时，字典的效率是非常快的

- 在字典中可以保存多个对象，每个对象都会有一个唯一的名字
  这个唯一的名字，我们称其为键（key），通过key可以快速的查询value
  这个对象，我们称其为值（value）
  所以字典，我们也称为叫做键值对（key-value）结构
  每个字典中都可以有多个键值对，而每一个键值对我们称其为一项（item）

```python
# 使用 {} 来创建字典
d= {}
# 语法：
#   {key:value,key:value,key:value}
#   字典的值可以是任意对象
#   字典的键可以是任意的不可变对象（int、str、bool、tuple ...），但是一般我们都会使用str
#   字典的键是不能重复的，如果出现重复的后边的会替换到前边的
d = {'name':'aaa' , 'age':18 , 'gender':'男' }
print(d , type(d))
# 根据键来获取值
print(d['name'],d['age'],d['gender'])
```

### 字典的使用

1. dict ()

   ```python
   d = dict(name='aaa',age=18,gender='男') 
   # 也可以将一个包含有双值子序列的序列转换为字典
   # 双值序列，序列中只有两个值，[1,2] ('a',3) 'ab'
   # 子序列，如果序列中的元素也是序列，那么我们就称这个元素为子序列
   f = dict([('name','bbbb'),('age',18)])
   ```

2. len() 

   ```python
   # len() 获取字典中键值对的个数
   d = dict(name='aaa',age=18,gender='男') 
   print(len(d))
   # len 也可以获取集合中的元素数量
   s = {10,3,5,1,2} 
   print(len(s))
   ```

3. in，not in 

   ```python
   # in 检查字典中是否包含指定的键
   # not in 检查字典中是否不包含指定的键
   # 也可以检查集合中的元素
   d = dict(name='aaa',age=18,gender='男') 
   print('hello' in d)
   ```

4. get() 获取键所对应的值

   ```python
   # 语法：d[key]
   d = dict(name='aaa',age=18,gender='男') 
   print(d['age'])
   n = 'name'
   print(d[n]) # 用变量来获取对应值时，注意不能加引号
   # get(key[, default]) 该方法用来根据键来获取字典中的值
   # 如果获取的键在字典中不存在，会返回None
   # 也可以指定一个默认值，来作为第二个参数，这样获取不到值时将会返回默认值
   print(d.get('hello','默认值'))
   ```

5. setdefault ()

   ```python
   # 修改字典
   # d[key] = value 如果key存在则覆盖，不存在则添加
   # setdefault(key, default]) 可以用来向字典中添加key-value
   d = dict(name='aaa',age=18,gender='男') 
   # 如果key已经存在于字典中，则返回key的值，不会对字典做任何操作
   result = d.setdefault('name','bbb') # 返回 aaa
   # 如果key不存在，则向字典中添加这个key，并设置value，返回默认值
   result = d.setdefault('hello','ccc') # 返回 ccc
   print('result =',result)
   ```

6. update()

   ```python
   # update([other]) 也可用于集合
   # 将其他的字典中的key-value添加到当前字典中d = {'a':1,'b':2,'c':3}
   # 如果有重复的key，则后边的会替换到当前的
   d = {'a':1,'b':2,'c':3}
   d2 = {'d':4,'e':5,'f':6, 'a':7}
   d.update(d2)
   print(d)
   ```

7.  popitem()

   ```python
   # 随机删除字典中的一个键值对，一般都会删除最后一个键值对
   d = {'a':1,'b':2,'c':3}
   d.popitem()
   result = d.popitem() # 删除之后，将元组形式的key-value作为返回值返回
   ```

8. pop()

   ```python
   # pop(key, default])
   # 根据key删除字典中的key-value
   d = {'a':1,'b':2,'c':3}\
   result = d.pop('d') # 将被删除的value返回
   result = d.pop('z','这是默认值') # 如果key不存在 没有默认值时报错，有默认值返回默认值
   s = {10,3,5,1,2}
   # 随机删除集合中的一个元素并返回
   result = s.pop()
   print(result)
   ```

9. clear()

   ```python
   # clear()用来清空字典，集合
   d.clear()
   ```

10. copy()

    ```python
    # 该方法用于对字典,集合 进行浅复制
    # 复制以后的对象，和原对象是独立，修改一个不会影响另一个
    # 注意，浅复制会简单复制对象内部的值，如果值也是一个可变对象，这个可变对象不会被复制
    d = {'a':1,'b':2,'c':3}
    d2 = d.copy()
    
    # print('d = ',d , id(d)) 两者对象不同
    # print('d2 = ',d2 , id(d2)) 
    
    d = {'a':{'name':'aaa','age':18},'b':2,'c':3}
    d2 = d.copy()
    d2['a']['name'] = 'bbb' 
    # 因为是浅复制，所以只复制了值，如果值里面有可变对象，两者的可变对象用的是同一个对象
    # 所以修改d2 中的a 中的 name的值 ， d也会改变
    ```


### 遍历字典

```python
d = {'name':'milet','age':18,'gender':'男'}
# key() 方法会返回字典所有的key，通过key来获取值，返回序列
# 通过key值来遍历字典
for k in d.keys() :
    print(d[k])
# values() 方法会返回一个序列，序列中保存有字典的左右的值
for v in d.values() :
    print(v)
# items() # 该方法会返回字典中所有的项 它会返回一个包含有双值子序列
# 通过元组解包 来进行遍历
for k ,v in d.items() :
    print(k , "=" , v)
```

###  集合

```python
# 使用 {} 来创建集合 集合里面不能添加序列
# 如果 集合里面有重复的值，只会显示一个重复的值
s = {10,3,5,1,2}   
# 使用 set() 函数来创建集合 ,可以将字典，序列转化为集合
s = set([1,2,3,4,5,1,1,2,3,4,5])
s = set('hello')
s = set({'a':1,'b':2,'c':3}) # 使用set()将字典转换为集合时，只会包含字典中的键
# add() 向集合中添加元素
s.add(10)
# remove() 删除集合中的指定元素
s.remove(100)
s.remove(1000)
```

###  集合的运算

1. 交集运算

   ```python
   # & 交集运算
   s = {1,2,3,4,5}
   s2 = {3,4,5,6,7}
   result = s & s2 # {3, 4, 5}
   ```

2. 并集运算

   ```python
   # | 并集运算
   s = {1,2,3,4,5}
   s2 = {3,4,5,6,7}
   result = s | s2 # {1,2,3,4,5,6,7}
   ```

3. 差集

   ```python
   # - 差集
   s = {1,2,3,4,5}
   s2 = {3,4,5,6,7}
   result = s - s2 # {1, 2}
   ```

4. 异或集

   ```python
   # ^ 异或集 获取只在一个集合中出现的元素
   s = {1,2,3,4,5}
   s2 = {3,4,5,6,7}
   result = s ^ s2 # {1, 2, 6, 7}
   ```

5.  子集

   ```python
   # <= 检查一个集合是否是另一个集合的子集
   # 如果a集合中的元素全部都在b集合中出现，那么a集合就是b集合的子集，b集合是a集合超集
   a = {1,2,3}
   b = {1,2,3,4,5}
   result = a <= b # True
   result = {1,2,3} <= {1,2,3} # True
   result = {1,2,3,4,5} <= {1,2,3} # False
   ```

6. 真子集

   ```python
   # < 检查一个集合是否是另一个集合的真子集
   # 如果超集b中含有子集a中所有元素，并且b中还有a中没有的元素，则b就是a的真超集，a是b的真子集
   result = {1,2,3} < {1,2,3} # False
   result = {1,2,3} < {1,2,3,4,5} # True=超集和真
   # >=和 > 检查超集和真超集同理
   ```

## 函数

### 函数简介

- 函数也是一个对象

- 对象是内存中专门用来存储数据的一块区域

  ```py
  # 创建函数
  	def 函数名 ([形参1,形参2,...形参n])#  函数名要符合规范，不能以数字开头
      	代码块
    def fn() :
      print("这是第一个函数")
  # fn 是函数对象，fn() 是调用函数
  # print是函数对象, print()调用函数
  
  a=20
  def fn3():
      global a # 如果要在函数中修改全局变量，用global关键字
      a = 10 # 修改全局变量
      # 如果要在函数中修改全局变量，用global关键字
  ```

### 函数的参数

```python
# 定义一个函数
# 定义形参时，可以为形参指定默认值
# 指定了默认值以后，如果用户传递了参数则默认值没有任何作用
# 如果用户没有传递，则默认值就会生效

def fn(a = 5 , b = 10 , c = 20):
    print('a =',a)
    print('b =',b)
    print('c =',c)
fn(1,2)
```

```py
# 关键字参数：直接根据参数名去传递参数
# 位置参数和关键字参数可以混合使用
# 混合使用关键字和位置参数时，必须将位置参数写到前面
def fn(a = 5 , b = 10 , c = 20):
    print('a =',a)
    print('b =',b)
    print('c =',c)
fn(1,c=30)
```

```python
def fn4(a):
    # 在函数中对形参进行重新赋值，不会影响其他的变量
    # a = 20
    # a是一个列表，尝试修改列表中的元素
    # 如果形参执行的是一个对象，当我们通过形参去修改对象时
    #会影响到所有指向该对象的变量
    a[0] = 30
    print('a =',a,id(a)) # a=[30,2,3] ,c=[30,2,3]
    

c = 10   
c = [1,2,3] 
fn4(c)

# 解决方法就是浅复制或者切片复制
# fn4(c.copy())
# fn4(c[:])
```

### 不定长参数

```python
# 在定义函数时，可以在形参前边加上一个*，这样这个形参将会获取到所有的实参
# 它将会将所有的实参保存到一个元组中（装包）
#eg:
def sum(*nums):
    # *num会获取所有位置参数，并保存进元组内
    result = 0
    # 遍历元组，并将元组中的数进行累加
    for n in nums :
        result += n
    print(result)
# 位置参数：eg:sum(1,2,3)
# 关键词参数：eg:sum(a=1,b=2,c=3)
fn2(1,2,3,4,5)
 def fn2(a,b,*c): ## 第一个参数给a，第二个参数给b，剩下的都保存到c的元组中
     print('a =',a)# print b ,c 同理
 def fn3(a,*b,c=1):
#如果可变参数在中间，可变参数之后的参数只能用关键词参数
 def fn4(*a)  #只能传递位置参数
    
# **形参接受其他的关键字参数，会统一保存在字典中
# **形参只能有一个，并且必须写在参数最后面
def fn5(b,c,**a) :
fn5(b=1,d=2,c=3,e=10,f=20)    
```

### 文档字符串

```python
# help()是Python中的内置函数，help(对象)
# 通过此函数可以查询python中的函数的用法
# 文档字符串（doc str）
# 在定义函数时，可以在函数内部编写文档字符串，文档字符串就是函数的说明
def fn(a:int,b:bool,c:str='hello') -> int:
    '''
    这是一个文档字符串的示例

    函数的作用：。。。。。
    函数的参数：
        a，作用，类型，默认值。。。。
        b，作用，类型，默认值。。。。
        c，作用，类型，默认值。。。。
    '''
    return 10
help(fn)
```

### 作用域与命名空间

```python
a=100
def fn3():
    # a = 10 # 在函数中为变量赋值时，默认都是为局部变量赋值
    # 如果希望在函数内部修改全局变量，则需要使用global关键字，来声明变量
    global a # 声明在函数内部的使用a是全局变量，此时再去修改a时，就是在修改全局的a
    a = 10 # 修改全局变量
    print('函数内部：','a =',a)
# locals()用来获取当前作用域的命名空间，返回的是一个字典
scope = locals() # 当前命名空间
scope['c'] = 1000 # 向字典中添加key-value就相当于在全局中创建了一个变量（一般不建议这么做）
def fn4():
    a = 10
    # globals() 函数可以用来在任意位置获取全局命名空间
    global_scope = globals()
    global_scope['a'] = 30
    print(global_scope['a'])
```

### 递归 

 递归的整体思想是，将一个大问题分解为一个个的小问题，直到问题无法分解时，再去解决问题，比如说解决10的阶乘问题时，先把阶乘拆成 10\*9!，把9的阶乘拆成9\*10!,以此类推，拆到 1的阶乘等于1.再去解决

```python
def factorial(n): # 解决阶乘问题
    if(n==1):
        return 1
    return n* factorial(n-1)
print(factorial(10))

def power(num,i): # 解决幂运算
    if(i==0):
        return 1
    return num * power(num,i-1)
print(power(2,6))  

str = input("输入一个字符串") #判断字符串是否为回文字符串
def reverable(str):
    if(len(str) < 2):
        return True
    elif(str[0]!= str[-1]):
        return False
    return reverable(str[1:-1])
if(reverable(str)):
    print("是回文字符串")
else:
	print("不是")
```

### 高阶函数，匿名函数，map函数

接收函数作为参数，或者将函数作为返回值的函数是高阶函数 

匿名函数 lambda 函数表达式 （语法糖）

```python
# filter()
# filter()可以从序列中过滤出符合条件的元素，保存到一个新的序列中
# filter()调用完毕以后，fn4就已经没用,就会被回收
# 参数：
#  1.函数，根据该函数来过滤序列（可迭代的结构）
#  2.需要过滤的序列（可迭代的结构）
# 返回值： 过滤后的新序列（可迭代的结构）
# 匿名函数 语法：lambda 参数列表 : 返回值 一般作为参数使用
l = [1,2,3,4,5,6,7,8,9,10]
r =filter(lamba i : i >5 ,l)
print(list(r)) #[6, 7, 8, 9, 10]
```

map函数：可以对可迭代对象中的所有元素做指定操作，然后将其添加到一个新对象中返回

```python
l = [1,2,3,4,5,6,7,8,9,10]
r = map(lambda i : i ** 2 , l) #对列表中的每一个元素进行平方
print(list(r))
```

### 闭包

简而言之，就是用函数形成闭包，让他人无法直接通过对象来修改value

```python
# 形成闭包的要件
#   ① 函数嵌套
#   ② 将内部函数作为返回值返回
#   ③ 内部函数必须要使用到外部函数的变量
def make_averager():
    # 创建一个列表，用来保存数值
    nums = []

    # 创建一个函数，用来计算平均值
    def averager(n) :
        # 将n添加到列表中
        nums.append(n)
        # 求平均值
        return sum(nums)/len(nums)

    return averager

averager = make_averager()
#这样无法通过下面语句来修改函数内部的nums
nums =[1,2,3]
print(averager(10))
print(averager(20))
```

### 装饰器

程序的设计，要求开发对程序的扩展，要关闭对程序的修改，装饰器简而言之就是对功能函数的拓展，但是不修改其源码

```python
#定义两个函数
def add(a , b):
    r = a + b
    return r
def fn():
    print('我是fn函数....')
#装饰器的使用
def begin_end(old):
    '''
        用来对其他函数进行扩展，使其他函数可以在执行前打印开始执行，执行后打印执行结束
        参数：
            old 要扩展的函数对象
    '''
    # 创建一个新函数
    def new_function(*args , **kwargs):#传入的实参进行装包，*args获取所有位置参数
        ##**获取关键字参数，获取到之后保存进元组中
        print('开始执行~~~~')
        # 调用被扩展的函数，将元组中的元素进行解包
        result = old(*args , **kwargs) 
        print('执行结束~~~~')
        # 返回函数的执行结果
        return result

    # 返回新函数        
    return new_function
f = begin_end(fn)
f2 = begin_end(add)
f3 = begin_end(mul)
# r = f()
# r = f2(123,456)
# r = f3(123,456)
# print(r)

#实际开发中，以下方法用的最多
#从内而外调用装饰器
@fn3
@begin_end 
def say_hello():
    print('大家好~~~')

say_hello()
```

## 对象

### 类

isinstance(实例名(对象) ,类名 )用来检查一个对象是否是一个类的实例 
