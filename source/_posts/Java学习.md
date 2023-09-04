---
title: Java学习
date: 2023-05-13 20:47:52
categories: 学习笔记
tags: [Java]
top_img: https://pic2.imgdb.cn/item/645f8dc80d2dde57771eb6e9.png
cover: https://pic2.imgdb.cn/item/645f8dc80d2dde57771eb6e9.png
description: Java自学笔记
---

# Java学习笔记

## 快捷键 

- sout ：System.out.println();
- psvm：public static void main(String[] args){} 主入口
- ctrl+ alt + L 自动的格式化代码
- 快速多行注释 ctrl + /

## 注释，关键字

### 注释

- 单行注释  \\\ +注释内容。
- 多行注释  /* 注释内容 */  
- 文档注释 /** 注释内容 */ （暂时用不上 用于大量代码注释时，会自动生成一个文档，此时就用上文档注释了）

### 关键字

- 关键字字母**全部小写**
- **class**:用于(创建/定义)一个类，类是Java最基本的组成单元
- 在编辑器中关键字会有特殊颜色标记

## 字面量 ，变量

### 字面量

- **字面量分类**
  1. 整数类型：不带小数点的数字(eg:666,-88)
  2. 小数类型：带小数点的数字 (eg:-5.2,6.8)
  3. 字符串类型：用双括号括起来的内容(eg:"我是java")
  4. 字符类型：用单括号括起来的，但内容只能有一个(eg:'A','0','我')
  5. 布尔类型：布尔值，表示真假，只有两个值：true和false
  6. 空类型 ：一个特殊的值，空值。值是：null (null 不能直接打印，只能用字符串的形式打印)
  7. /t  制表符  在打印的时候，把前字符串的长度补齐到8，或者8的整数倍，最少补一个空格，最多补8个空格，用于对齐
- **变量**
  1. 定义： 在程序的执行过程中,其值有可能发生改变的量
  2. 变量的定义格式： 数据类型 变量名 = 数据值; 
     - 数据类型: 限定了变量能存储数据的类型  常见的整数类型:**int**  小数(浮点)类型:**double** 
  3. 变量名:就是存储空间的名字  作用:方便以后使用
  4. 数据值:真正存在变量中的数据
  5. 等号: 赋值。把右边的数据赋值在左边的变量
  6. 注意事项 变量只能存在一个值 不能重复  变量在使用之前必须赋值

 ## 计算机的存储规则

  1. 计算机中任意数据都是以二进制的形式来存储的
  2. 不同进制在代码中的表现形式
     - 二进制(**BIN**)：由0和1组成，代码中以**0b**开头
     - 十进制(**DEC**)：由0~9组成，前面不加任何前缀
     - 八进制(**OCT**)：由0~7组成，代码中以**0**开头
     - 十六进制(**HEX**)：由0~9还有a-f组成，代码中以**0x**开头  
   3. 其余进制转为十进制公式；**系数**(就是每一位上的数) * **基数**(当前进制数)的**权**(从右往左,依次为 0 1 2 3 4)次幂 相加
   4. 计算机的字母存储是通过查询码表来储存的
   5. ASCⅡ表[![ASCⅡ.png](https://s1.ax1x.com/2022/08/16/v0U49U.png)](https://imgtu.com/i/v0U49U)
   6. 计算机对汉字的存储是根据查询码表来存储的
   7. 其余编码类型   **GB2312编码** 1981年发布的中文汉字编码标准，**GBK编码** 00年发布  **Unicode编码**：国际标准字符集，将每个字符定义一个唯一的编码，满足跨平台的文本信息转换 
   8. 计算机中图片分为 黑白图，灰度图(0代表纯黑，255代表纯白)，彩色图
   9. 计算机的图片存储是通过每个像素点中的RGB三原色来存储的
      - 彩色图 ：计算机三原色，红色，绿色，蓝色取值范围0~255。(**eg**：(255.0.0)就代表红色)
   10. 计算机对声音的波形图进行采样后再存储声音的。 

## 数据类型

### 数据类型 变量名 = 数据值

1. **数据类型** 
   - 基本数据类型
     - [![基本数据类型.png](https://s1.ax1x.com/2022/08/17/vDZD9x.png)](https://imgtu.com/i/vDZD9x) 其中 **int**类型和**doube**l类型为默认常用数据类型  注意！ 用long类型的时候 后面数字要加上大写或小写L
     - **float**类型在定义的时候后面要加大写或小写的 **F**
     - 整数和小数取值范围大小关系
       - double > float > long > int > short >byte    
 2. **标识符**
    -  定义：是给类，方法，变量等起的名字
    -  标识符命名的规则 ---- 硬性要求
       1. 由**数字**，**字母**，**下划线**(**_**)和**美元符**(**$**)构成
       2. 不能<以**数字**开头
       3. 不能>是**关键字**
     -  标识符命名规则 ----软性建议
        1. 小驼峰命名法：方法，变量 的起名
           - 规范1：标识符是一个单词的时候，**全部小写**(**eg: name**)
           - 规范2：标识符由多个单词组成的时候，**第一个单词首字母小写**，其他单词**首字母大写**(**eg：firstName**)
         2. 大驼峰命名法：类名的起名
            - 规范1：便师傅是一个单词的时候，**首字母大写**(**eg：Student**)
            - 规范2：标识符由多个单词组成的时候，**每个单词首字母大写**(**eg：GoodStudent**)

## 键盘录入

- Java中的类**Scanner**，这个类可以接收键盘输入的数字

#### 基础格式步骤

   - 步骤一：导包 (IDEA里可以自动导包)
     ```import java.util.Scanner; ```  
        - 步骤二：创建对象 ---使用Scanner类
          ```Scanner sc = new Scanner(System.in);```
     - 步骤三：接收数据
       ```int i = sc.nextInt();```

### 项目结构介绍

- project  **项目**   module  **模块**   package   **包**   class  **类**	

## 运算符

### 算数运算符

   - 加 (**+**)   减  (**-**)   乘 (__*__)   除 (**/**)   取余或取模 (**%**)
   - 在代码中，如果有小鼠参与计算有可能会不精确**(涉及小数在计算机中的存储模式)** 
   - **取余取模的应用场景：**
     - **判断A是否为偶数** 如果A % 2 如果余数为1 ，A为偶数，反之则为单数
     - **斗地主的发牌顺序** ，把每张牌定一个序号，A %3 如果为1 就发给第一个玩家，如果为2 就发给第二个玩家 如果为3 就发给第三个玩家，实现发牌的顺序
     - **小练习** 键盘录入一个三位数 然后依次输出他的**百位，十位，个位**

```
package com.company;  
import java.util.Scanner;  
public class class4 {  
    public static void main(String[] args) {  
        Scanner sc = new Scanner(System.in);  
 int number = sc.nextInt();  
  /* 输出个位 number % 10  输出十位 number / 10 % 10  输出百位 number / 100 % 10 */  System.out.println(number / 100 % 10);//百位 
  System.out.println(number / 10 % 10); //十位 
  System.out.println(number % 10); 个位
  }  
}  
```

- 数字在进行运算时,数据类型不一样是不能运算的，需要转成一样的，才能运算
- **数字相加**
  一. **隐式转换**
  1. 自动类型的提升  把一个取值范围小的数值，转成取值范围大的数据
  2. byte  short  char 三种类型的数据在运算的时候 ， 都会直接提升为int 在进行运算.

```
package com.company;  
public class class4 {  
    public static void main(String[] args) {  
        byte number1 = 10;  
 byte number2 =20;  
 int c = number1 + number2;  
  }  
}
```

  二.  **强制转换**

      1. 如果把一个取值范围大的数赋值给取值范围小的变量，是要进行强制转换的
      2. <font color="red">**格式**</font> **目标数据类型 变量名 =(目标数据类型) 被强制转换的数据** 

- **字符串相加**
  1.  字符串中的"+" 是字符串的**连接符**，而不是算术运算符，他会将前后的数据进行拼接，产生一个**新**的字符串
      **eg: "123" + 123 = "123123"**
      连续进行“+”操作时，从左到右逐个执行
      **eg：1 + 299 + "英雄" = “300英雄”** 
      **总结: 简而言之就是“+”操作   在有字符串参与 就是拼接，没有字符串参与就是算数运算符**
- **字符相加**
  -  字符相加是将字符通过ASCII码表查询到对应的数字在进行计算
     e

```
package com.company;  
public class class4 {  
    public static void main(String[] args) {  
        char c = 'a';  
 int result = c + 1;  
  System.out.println(result);  // 98
  System.out.println( 'a'+ "abc"); //aabc
  }  
}
```

### 自增自减运算符

| 符号                                                         | 作用 | 说明        |
| ------------------------------------------------------------ | ---- | ----------- |
| ++                                                           | 加   | 变量的值加1 |
| ——                                                           | 减   | 变量的值减1 |
|** 注意： ++和—— 既可以放在变量的前边，也可以放在变量的后边**|      |             |

- 变量++ 代表的是 先赋值再加减 
- ++变量 代表的是 先加减再赋值
- 小练习

```
package com.company;  
public class class4 {  
    public static void main(String[] args) {  
     int a = 10;  
 int y = a++;// a =11 y =10 先用后算  
  int x = ++a;// a =12 x =12 先算后用  
  System.out.println("a:"+ a);  
  System.out.println("y:"+ y);  
  System.out.println("x:"+ x);  
  }  
}
```

### 赋值运算符

| 符号                                             | 作用       | 说明                      |
| ------------------------------------------------ | ---------- | ------------------------- |
| =                                                | 赋值       | int a=10，将10赋值给变量a |
| +=                                               | 加后赋值   | a+=b，将a+b得到值给a      |
| -=                                               | 减后赋值   | a-=b，将a-b的值给a        |
| *=                                               | 乘后赋值   | a*=b，将a×b的值给a        |
| /=                                               | 除后赋值   | a/=b，将a÷b的商给a        |
| %=                                               | 取余后赋值 | a%b，将a÷b的余数给a       |
| 底层蕴含强制转换小案例 |            |                           |

```
package com.company; 
public class class5 {  
    public static void main(String[] args) {  
        short s = 1;  
  //把左边的和右边的相加，然后再赋值给变量  
  s += 1;  
   }  
}
```

解析：底层蕴含着强制转换 所以说该式子等于 s = (short) (s + 1)  ，因为short类型在参与计算的时候会自动的转换为**int类型**，在第六行时   因为要赋值给变量，变量是short类型，所以要把int类型的计算结果强制转换成short类型再赋值给变量 ，否则会报错  

### 关系运算符（比较运算符）

| 符号                                                         | 说明                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| ==                                                           | a==b，判断a和b的值是否相等，成立为true，不成立为false   |
| !=                                                           | a!=b，判断a和b的值是否不相等，成立为true，不成立为false |
| >                                                            | a>b，判断a是否大于b，成立为true，不成立为false          |
| >=                                                           | a>=b，判断a是否大于等于b，成立为true，不成立为false     |
| <                                                            | a<b，判断a是否小于b，成立为true，不成立为false          |
| <=                                                           | a<=b，判断a是否小于等于b，成立为true，不成立为false     |
| **注意事项：关系运算符的 结果都是boolean类型，要么是true，要么是false。千万不要把"==" 误写成 "="** |                                                         |

### 逻辑运算符

| 符号     | 作用       | 说明                         |
| -------- | ---------- | ---------------------------- |
| &        | 逻辑与(且) | 并且，两边都为真，结果才是真 |
| l        | 逻辑或     | 或者，两边都为假，结果才是假 |
| ^        | 逻辑异或   | 相同为false，不同为true      |
| ！(常用) | 逻辑非     | 取反                         |
| 取反例子 |            |                              |

```
System.out.println(!true) // false
```

 ####短路逻辑运算符：

| 符号                                                         | 作用   | 说明                                                         |
| ------------------------------------------------------------ | ------ | ------------------------------------------------------------ |
| && (常用)                                                    | 短路与 | 左边为false，右边不执行，结果一定等于false，但是有**短路效果** |
| ll (常用)                                                    | 短路或 | 左边为true，右边不执行，结果以定为true，但是有**短路效果**   |
| 何为 **短路效果**： 在做逻辑运算时候，逻辑与要分别判断左右两边，短路与就是当你判断左边为假的时候，你就不用去判断右边的真假了,因为结果已经一样了，当你判断左边为真时，再去判断右边是否为真，   好处就在于**提高了程序的运行效率** 简单来说就是，当左边的表达式能确定最终的结果，那么右边就不会参与运行了 |        |                                                              |
| **小案例**：                                                 |        |                                                              |

```
package com.company;  
public class class5 {  
    public static void main(String[] args) {  
       int a =10 ;  
 int b =10;  
 boolean result = ++a<5 && ++b <5;  
  System.out.println(result); //false 左边的为false，故输出false
  System.out.println(a); //11  因为++a实现自增
  System.out.println(b); //10,因为短路逻辑运算符在左边判断为假之后， ++b<5这个式子就没有参与计算，所以没有实现自增，输出10
  }  
}
```

小练习
键盘输入两个整数，如果其中一个为6 则输出true，两个数相加是6的倍数，则输出true，其余情况输出false

```
package com.company;  
import java.util.Scanner;  
public class class5 {  
    public static void main(String[] args) {  
        Scanner sc = new Scanner(System.in);  
  System.out.println("请输入一个整数");  
 int number1 = sc.nextInt();  
  System.out.println("请输入一个整数");  
 int number2 = sc.nextInt();  
  //判断条件，用短路或来判断  
  boolean result1 = number1 == 6 || number2 == 6 ;  
  /*也可以写成boolean result1 = number1 == 6 || number2 == 6 || (number1+number2)%6 == 0 ;  
  把下面的判断倍数一起写到上面  
 */  int sum = number1+number2;  
 boolean result2 =  sum%6 == 0 ;  
  System.out.println(result1);//true / false  
  System.out.println(result2);//true / false  
  }  
}
```

### 三元运算符

- **作用**：可以进行判断，根据判断的结果得到不同的内容
- **格式** = 关系表达式 ？ 表达式1 ： 表达式2
- **范例** `int max = a > b ? a : b; ` 
  首先计算关系表达式的值，如果值为true，则表达式1 的值就是运算结果，值为false，表达式2为结果
  **练习**
  有三个人，身高分别是150cm，210cm，165cm，用程序实现获取这三个和尚的最高身高

```
package com.company;  
public class class1 {  
    public static void main(String[] args) {  
        int height1 = 150;  
 int height2 = 210;  
 int height3 = 165;  
 int height = height1 > height2 ? height1 : height2;  
 int result = height > height3 ? height : height3;  
  System.out.println(result);  //210
  }  
}
```

### 运算符的优先级

| 优先级                                                       | 运算符                                                      |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| 1                                                            | **.**  / **()** / **{}**                                    |
| 2                                                            | **!** / **-** /  **++** / **- -**                           |
| 3                                                            | **\*** / **/(除号**)  / **%**                               |
| 4                                                            | **+** / **-**                                               |
| 5                                                            | **<<** / **>>** / **>>>**                                   |
| 6                                                            | **<** / **<=** / **>** / **>=** / **instanceof**            |
| 7                                                            | **==** / **!=**                                             |
| 8                                                            | **&**                                                       |
| 9                                                            | **^**                                                       |
| 10                                                           | **l**                                                       |
| 11                                                           | **&&**                                                      |
| 12                                                           | **ll**                                                      |
| 13                                                           | **?  :  (三元运算符)**                                      |
| 14                                                           | **=** / **+=** / **-=** / ***=** / **/=** / **%=** / **&=** |
| 这张表中最重要的就是注意：小括号()是优先于是所有的运算符 |                                                             |
| 第5行运算符 **<<**  , **>>** , **>>>**属于位移运算符（在原码，反码单元有详细的解释，可直接看原码补码行。这里写的时候才发现后面有更详细的） |                                                             |

- **\>>**:右移，往右移动两位，负数高位补1，正数高位补0
- eg： 6>>2
  正常
  0000 0000 0000 0000 0000 0000 0000 0110
  结果
  0000 0000 0000 0000 0000 0000 0000 0001
- eg： -6>>2
  正常
  1111 1111 1111 1111 1111 1111 1111 1010
  结果：
  1111 1111 1111 1111 1111 1111 1111 1110
- **\<<**  往左移动两位，负数高位补1，正数高位补0
- **\>>>**:无符号右移，向右移动两位，正数负数都是高位补0
- eg：6 >>> 2
  0000 0000 0000 0000 0000 0000 0000 0110
  结果：
  0000 0000 0000 0000 0000 0000 0000 0001
- eg：-6>>>2
  负数：-6>>>2
  1111 1111 1111 1111 1111 1111 1111 1010
  结果：
  0011 1111 1111 1111 1111 1111 1111 1110

### 原码，反码，补码

#### 原码

- **定义**：十进制数据的二进制表现形式，最左边的是符号位，0为正数，1为负数。 
- **eg：** **56** 的原码就是**00111000** ，最左边的**0**为符号位，其余的部分“0111000”为**数据** 
  在**00111000**中，一个位置代表一个**bit(俗称比特位)** 把8个**bit**分为一组形成一个**byte(俗称字节)**
- 利用原码对正数进行计算是不会有问题的，但是如果是负数计算，结果就会出错，实际运算的的结果和我们预期的结果是相反的
- eg：10000000 在此基础上 **+1** 得到100000001 ， 这个数据的正确值是 **+1**，但是所得到是实际值是 **-1**
- 那如何解决原码对负数计算结果出错的问题呢 -----反码

#### 反码

- 为了解决原码**不能计算负数**的问题而出现
- **计算规则**：正数的反码不变，负数的反码在原码的基础上，符号位不变。数值取反，**0变1**，**1变0**
 - **eg：** **-56** 原码  **10111000**

| 十进制数字 | 原码      | 反码      |
| ---------- | --------- | --------- |
| +0         | 0000 0000 | 0000 0000 |
| -0         | 1000 0000 | 1111 1111 |
| -1         | 1000 0001 | 1111 1110 |
| -2         | 1000 0010 | 1111 1101 |
| -3         | 1000 0011 | 1111 1100 |

- 反码的弊端就是在做跨 **0** 的计算时，结果会出错
- eg：计算 **-3 +5**     ------  `11111100+00000101`
  实际结果等于 **+1**  但是正确值为 **2**
  如何避免反码跨0计算的错误呢？-----**补码**

 #### 补码

 简而言之，补码就是反码 **+1**  正数的补码和原码，反码是一样的
 计算机中数字的计算和存储都是以补码的形式存在的

| 十进制数字 | 原码      | 反码      | 补码      |
| ---------- | --------- | --------- | --------- |
| +0         | 0000 0000 | 0000 0000 | 0000 0000 |
| -0         | 1000 0000 | 1111 1111 | 0000 0000 |
| -1         | 1000 0001 | 1111 1110 | 1111 1111 |
| -2         | 1000 0010 | 1111 1101 | 1111 1110 |
| -3         | 1000 0011 | 1111 1100 | 1111 1101 |
| ...        | ...       | ...       | ...       |
| -126       | 1111 1110 | 1000 0001 | 1000 0010 |
| -127       | 1111 1111 | 1000 0000 | 1000 0001 |
| -128       | 无        | 无        | 1000 0000 |

### 不同数据在不同数据类型的差异

| 数据          | 占用字节位           | 原码           |
| ------------- | -------------------- | -------------- |
| byte类型的10  | 1个字节 (8个比特位)  | 0000 1010      |
| short类型的10 | 2个字节 (16个比特位) | 0000 * 3  1010 |
| int类型的10   | 4个类型(32个比特位)  | 0000 * 7 1010  |
| long类型的10  | 8个字节(64个比特位)  | 0000 * 15 1010 |

#### 其他运算符

| 运算符 | 含义       | 运算规则                |
| ------ | ---------- | ----------------------- |
| &      | 逻辑与     | 0为 false，1 为true     |
| \|     | 逻辑或     | 0 为flase，1为ture      |
| <<     | 左移       | 向左移动，低位补 0      |
| >>     | 右移       | 向右移动，高位补 0 或 1 |
| >>>    | 无符号右移 | 向右移动，高位补 0      |

-  &：此逻辑与非上面的逻辑运算符，两个都位true才输出true
-  eg：

```
package com.company;  
public class class3 {  
    public static void main(String[] args) {  
        int a = 200; // 补码为 0000 * 6 1100 1000  int b = 10; // 补码为 0000 * 7 0000 1010 //&：计算过程 0000 * 6  0000 1000 都为1时才输出true，为1  
  System.out.println(a & b); // 补码为 0000 * 6 0000 1000 输出十进制为 8 }  
}
```

- **\| ：逻辑或**，只要一个是true，就输出ture
- eg:

```
package com.company;  
public class class3 {  
    public static void main(String[] args) {  
        int a = 200; // 补码为 0000 * 6 1100 1000  int b = 10; // 补
        码为 0000 * 7 0000 1010 //&：计算过程 0000 * 6  1100 1010 都
        为1时才输出true，为1  
  System.out.println(a | b); // 补码为 0000 * 6 0000 1000 输出十进制
  为 202  }  
}
```

- **左移 ： <<** 向左移动，低位补0
- eg：公式：左移一次乘2 ，左移两次乘4

```
package com.company;  
public class class4 {  
    public static void main(String[] args) {  
        int a = 100; //补码为0000 * 6 1100 1000  
  System.out.println( a << 2); // 0000 * 6 0011 0010 000 十进制输出是800  
  }  
}
```

- **右移：>>**向右移动，高位补0或1 , 数值位补0，符号位根据原来是正数就补0，原来是负数就补1
- eg：公式：右移一次除2，右移两次除4

```
package com.company;  
public class class4 {  
    public static void main(String[] args) {  
        int a = 200; //补码为0000 * 6 1100 1000  
  System.out.println( a << 2); // 0(⬅根据原本的补码正负来决定符号
  位)000 * 6 0011 0010 十进制输出是50 
  }  
}
```

-  **无符号右移：**>>> 向右移动，高位补0，移动规则和右移一样，只是补符号位 的区别

### 流程控制语句

#### 顺序结构，代码从上往下运行，从左往右运行

#### 分支结构1：if

- if语句对的第一种形式
  - 如果对一个布尔类型的变量进行判断，不要用==号，直接把变量卸载小括号即可

```
  if (关系表表达式(判断语句)) {
     语句体;
}
boolean flag = true;
if(flag){ //小括号中不要用 flag == true 的判断语句
   System.out.println("flag的值为true")
}
```

- if的第二种格式

```
if(关系表达式) {  
    语句体1;  
}else {  
    语句体2;  
}
```

   - eg： 需求某影院售出100张票，票的序号为1~100，其中奇数票坐左边，偶数票坐右边，键盘录入一个整数表示电影票的票号。判断其票号和座位方向

```
package com.company;  
import java.util.Scanner;  
public class class5 {  
    public static void main(String[] args) {  
        Scanner sc = new Scanner(System.in);  
  System.out.println("请输入电影票的票号");  
 int number = sc.nextInt();  
 if (number >=1 && number <=100){  
            if(number %2 ==0) {  
                System.out.println("请坐右边");;  
  }else {  
                System.out.println("请坐左边");;  
  }  
        }else{  
            System.out.println("你竟然拿假票？ 看我蛇形拳！");  
  }  
 }  
}
```

- if的第三种格式 if嵌套

```
if (关系表达式1) {  
    语句体1;  
}else if(关系表达式2){  
    语句体2;  
}  
...  
else {  
    语句体 n + 1 ;  
}
```

   -  **if嵌套的案例,**需求：小明获得不同分数后获得不同的奖励和惩罚(注意中括号)

 ```
	 package com.company;  
import java.util.Scanner;  
public class class5 {  
    public static void main(String[] args) {  
        Scanner sc = new Scanner(System.in);  
  System.out.println("请输入小明的分数");  
 double Fraction = sc.nextDouble();  
 if (Fraction <= 100 && Fraction > 0) {  
            if (Fraction >= 95 && Fraction <= 100) {  
                System.out.println("小明获得了自行车");;  
  } else if (Fraction >= 90 && Fraction <= 94) {  
                System.out.println("小明可以去游乐场");;  
  } else if (Fraction >= 80 && Fraction <= 89) {  
                System.out.println("小明可以得到变形金刚");  
  } else if (Fraction <= 80) {  
                System.out.println("小明要倒灶了");;  
  }  
             } else {  
            System.out.println("你在赣神魔？");  
  }  
    }  
}
 ```

#### 分支结构2：switch

- switch语句格式  

```
switch(表达式){  //先计算表达式的值
    case 值1:  //依次和case后面的值进行比较，如果有对应的值，就会执行相应的语句，在执行过程中，遇到break就会结束
        语句体1;  //如果所有的case后面的值和表达式的值都不匹配，就会执行default后面的语句，然后结束整个switch语句
 break; 
 case 值2:  
        语句体2;  
 break;  ...  
    default:  
        语句体n+1;  
 break;}
```

    - 表达式：(将要匹配的值)取值为byte,short,int,char.JDK5以后可以是枚举，JDK7以后可以是String。
    - case: 后面跟的是要和表达式进行比较的值(被匹配的值)
    - break：表示中断，结束的意思，用来结束switch语句
    - default：表示所有情况都不匹配的时候，就执行该处的内容，和if语句的else相似
    - case后面的值只能是字面量，不能是变量
    - case给出的值不允许重复

   - default的位置和省略
     - default可以写在任意位置且可省略(但是不建议省略)
       - case穿透
      - 语句中没有break导致的，程序会一直往下执行语句体直到遇到大括号或者遇到break为止。
   - switch新特性
     -  可吧原格式的语句变成新格式，省略了**break**，代码更清爽 **注意该特性JDK12之后才有**

 ```
 switch(表达式){ //当语句体是一句时，case后的大括号可省略
 case 值1 ->{                        
        语句体1;  
}
 case 值2 ->{  
        语句体2;  
}        
 ...  
    default ->{  
        语句体n+1;  
 }
 ```

   - switch和if第三种格式各自的使用场景
     -如果是有限个数据来进行判断就用switch，如果是范围的判断就用if嵌套 

   #### 循环结构

   - **for循环** 
     -  **格式** (初始化语句只执行一次)

```
for (初始化语句;条件判断语句;条件控制语句){  
    循环体语句;  
}
```

[![v7XBHP.png](https://s1.ax1x.com/2022/09/06/v7XBHP.png)](https://imgse.com/i/v7XBHP)

   - **小练习**: 求1-5的总和

   ```
public static void main(String[] args) {  
     int sum =0;  
 for (int i =1 ; i <= 5 ; i++){  
     //int sum =0;  //如果把变量和输出语句定义在循环里面 ，那么当前变量只在本次循环中有效
 sum += i;//把得到的每一个数字累加到变量sum中  
  //System.out.println(sum);  输出 1-5，因为每次循环结束都让变量从内存中消失，下一次变量中再重新初始化一个新的变量，简而言之就是重置了。
  }  
    System.out.println(sum);  
}
   ```

   - **for循环小练习：** 需求：键盘录入两个数，表示一个范围，统计这个范围里面既能被3整除，又能被5整除的数字有几个

   ```
   package Test;  
import java.util.Scanner;  
public class test5 {  
    public static void main(String[] args) {  
        Scanner sc = new Scanner(System.in);  
        //键盘录入两个数
  System.out.println("请输入最大值");  
 int max = sc.nextInt();  
  System.out.println("请输入最小值");  
 int min = sc.nextInt();  
 //定义统计变量
 int number = 0;  
 for (int i = min; i <=max  ; i++) {  
 //如果符合条件就执行统计变量自增
            if ( i % 3 == 0 && i % 5 == 0){  
                number ++;  
  }  
        }  
         System.out.println("即能被3整除，又能被5整除的个数为"+number+"个");  
  }  
}
   ```

- 需求：获取一个[a,b]范围的随机整数 ：(int)(Math.random() *(b-a+1)) +a
- 获取一个[1,100]范围的随机整数:(int)(Math.random() * 100 ) +1

## 面向对象 

- Java类及类的成员：（重点）属性、方法、构造器；（熟悉）代码块、内部类
- 面向对象的特征：封装、继承、多态、（抽象）
- 其他关键字的使用：this、super、package、import、static、final、interface、abstract等

一个整体的系统，能不能把这些步骤和功能进行封装，封装时根据不同的功能，进行不同的封装，功能类似的封装在一起。**这样结构就清晰了很多。用的时候，找到对应的类就可以了。这就是面向对象的思想。(自己理解)

### 类和对象

- **类**：具有相同特征的事物的抽象描述，是`抽象的`、概念上的定义。
- **对象**：实际存在的该类事物的`每个个体`，是`具体的`，因而也称为`实例(instance)`。

### 类的定义

```java
[修饰符] class 类名{
	属性声明;
    方法声明;
}
public class Person{
    //声明属性age
    int age ;	                   
    
    //声明方法showAge()
    public void eat() {        
	    System.out.println("人吃饭");
    }
}

```

#### 对象实例化

```java
//方式1：给创建的对象命名
//把创建的对象用一个引用数据类型的变量保存起来，这样就可以反复使用这个对象了
类名 对象名 = new 类名()
    对象名.属性或者方法就能调用，前提公共

//方式2：
new 类名()//也称为匿名对象
```

#### 匿名对象

1. 如果一个对象只需要进行一次方法调用，那么就可以使用匿名对象。 
2. 我们经常将匿名对象作为实参传递给一个方法调用。 

```java
//我们也可以不定义对象的句柄，而直接调用这个对象的方法。这样的对象叫做匿名对象。
new Person().shout(); 
```

#### 对象在内存中分配涉及到的内存结构

- 栈：方法内定义的变量（局部变量） 存储在栈中
- 堆：存放对象实例，包括对象中的属性
- 方法区：存放类的模板，已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

![](https://pic2.imgdb.cn/item/645f3f7d0d2dde5777946fde.png)

### 成员变量 和局部变量

1. 类中声明的位置不同
   - 属性： 声明在类中，方法外的变量
   - 局部变量：声明方法，构造器内部的变量

2. 在内存中分配的位置不同
   - 属性：随着对象的创建，存储在堆空间中
   - 局部变量：存储在栈空间中

3. 生命周期
   - 属性：随着对象的创建消亡而创建消亡
   - 局部变量： 随着方法对应的栈帧入栈，随着栈帧出栈
4. 作用域
   - 属性：在整个类的内部有效
   - 局部变量：仅限于声明此局部变量所在的方法(构造器或代码块内)
5. 是否可以使用权限修饰符
   - 可以使用权限修饰符
   - 不可以
6. 是否有默认值
   - 都有默认初始化值
   - 没有：所以在使用局部变量之前，必须要显性的赋值 

#### 方法

![](https://pic2.imgdb.cn/item/645f8a1d0d2dde5777186e10.jpg)

对象调用方法的过程：先调用main函数，进入内存，然后eat函数进入内存，用完之后退出内存，然后sleep方法进入，如果mian中有一个String接收方法interests方法的返回值，则调用者为main函数，就和打子弹一样，一个新方法要进入，旧方法如果后续没有调用，旧方法就直接出去，main方法出去后，堆中的实例没有指针指向则被垃圾回收器回收

## 多线程

### 实现方法

如果要实现多线程开发有三种形式：

1. 继承Tread类

   ```java
   public static void main(String[] args) {
               /**
                * 多线程第一种启动方法
                * 1.自己定义一个类继承Thread
                * 2.重写run方法
                * 3.创建子类对象并启动线程
                *
                */
           MyThread myThread1 = new MyThread();
           MyThread myThread2 = new MyThread();
           myThread1.setName("线程1");
           myThread2.setName("线程2");
   
           myThread1.start();
           myThread2.start();
   //MyThread类
   public class MyThread extends Thread{
       @Override
       public void run() {
           for (int i = 0; i <10 ; i++) {
               System.out.println(getName()+"HelloWorld");
           }
       }
   }
   ```

   

2. 实现Runnable接口

   ```java
   public static void main(String[] args) {
       /**
        * 多线程第二种启动方法
        *
        * 1.定义一个类实现Runable接口
        * 2.重写里面的run方法
        * 3.创建自己的类的对象
        * 4.创建一个Thread类的对象，并开启线程
        * */
   
       MyRun myRun = new MyRun();
       Thread t1 = new Thread(myRun);
       Thread t2 = new Thread(myRun);
       t1.setName("线程1");
       t2.setName("线程2");
       t1.start();
       t2.start();
   }
   //MyRun类
   public class MyRun implements Runnable{
       @Override
       public void run() {
           for (int i = 0; i < 10; i++) {
               System.out.println(Thread.currentThread().getName()+"HelloWorld");
           }
       }
   }
   
   ```

3. 实现Callable接口

   待续，还没掌握

### 运行过程

1. 新建
2. 就绪
3. 执行(通过资源调度，获取资源)
4. 阻塞
5. 死亡

待补充

## Socket

### 服务器模块设计

服务端功能主要如下

1. 能够开启和关闭服务器
2. 等待客户端从特殊端口发送的请求
3. 监听的端口并不是固定的，服务端的端口能够自定义的
4. 能够广播消息或者向所有连接到服务器的用户

客户端和服务器进行socket套接字连接，socket的使用，API提供了一个专门的类来处理，让编写程序变得简单，多线程技术在服务端得到了充分的体现，服务器能够同时处理不同IP的客户端的请求，通过循环调用serversocket对象的方法来监听是否来自客户端请求。

### Socket的常用方法

| 常用方法                            | 说明                                                         |
| ----------------------------------- | :----------------------------------------------------------- |
| InetAddress<br />getInetAddress()   | 返回与当前Socket对象关联的InetAddress                        |
| void shutdownInput()                | 此套接字的输入流置于"流的末尾"                               |
| void shutdownOutput()               | 禁止此套接字的输出流                                         |
| InputStream<br />getInputStream()   | 返回当前Socket对象的inputStream对象，它是服务器端向客户端发送回来的数据流 |
| OutputStream<br />getOutputStream() | 返回当时Socket对象关联的OutputStream对象，他是客户端向服务器端发送的数据流 |
| void close()                        | 关闭该socket建立的连接                                       |

[^InetAddress]: 表示互联网协议地址，包含IP地址，实际上就是java对IP地址的封装

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class Server {
    public static void main(String[] args) {
        ServerSocket server = null;
        Socket socket= null;
        try {
            //准备服务器端用的通讯对象(套接字),指明端口号为8888
            server = new ServerSocket(8888);
            //到指定端口取阻塞监听，一旦有客户端请求发送过来，那么立即自动与客户端建立连接
            socket = server.accept();
            System.out.println("服务端准备就绪");
            //发送到客户端的内容
            String msg="你好，我是服务器，这是我的第一次测试";
            //准备输出对象
            OutputStream outputStream = socket.getOutputStream();
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(outputStream));
            bw.write(msg);
            bw.newLine();
            //刷新缓冲区
            bw.flush();
            //接收客户端发送的消息
            InputStream is = socket.getInputStream();
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            String reply = br.readLine();
            System.out.println("我是服务器，接收到消息"+reply);
            br.close();
            is.close();
            bw.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```



### 使用多线程和Socket实现简单的聊天

Server:

```java
import java.io.IOException;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;

/**
 * @author lenovo
 * @remark 服务器对象类
 * @creatTime 2023年6月1日10:14:10
 * @version 1.0
 * */
public class MyServer {
    public static void main(String[] args) throws IOException {
        //创建ServerSocket对象类
        ServerSocket serverSocket = new ServerSocket(8888);
        //声明键盘输入工具类对象
        Scanner input =null;
        //声明PrintWriter输出对象
        PrintWriter out =null;
        Scanner sc = new Scanner(System.in);
        int password = sc.nextInt();
        System.out.println("服务器准备就绪-------");
        System.out.println("请输入用户名：");
        String userName =sc.next();
        System.out.println("请输入密码: ");
        int pwd = sc.nextInt();
        //使用循环等待侦听到有客户端连接上
        while (true){
            //创建Socket对象,调用服务类中的侦听方法
            Socket socket = serverSocket.accept();
            //打印客户端设备连接信息
            System.out.println(socket.getInetAddress()+":该设备成功连接服务器");
            //实例化创建键盘录入工具类,从对应客户端获取输入流信息
             input = new Scanner(socket.getInputStream());
            //实例化输出对象类,从对应客户端获取输出流信息
             out = new PrintWriter(socket.getOutputStream());
             //调用读取信息的线程类，进行启动线程
            new MySocketReader(input).start();
            //调用写入信息的线程类，进行启动线程
            new MySocketWriter(out).start();
        }
    }
}
```

Client:

```java
import com.guangsha.thread.MySocketReader;
import com.guangsha.thread.MySocketWriter;

import java.io.IOException;
import java.io.PrintWriter;
import java.net.Socket;
import java.util.Scanner;

/**
 * @author lenovo
 * @remark 客户端对象类
 * @creatTime 2023年6月1日10:14:10
 * @version 1.0
 * */
public class Myclient {
    public static void main(String[] args) throws IOException {
        //实例化创建Socket对象类,与服务器进行连接
        Socket socket = new Socket("10.12.25.78",8888);
        //实例化创建键盘录入工具类,从对应服务器获取输入流信息
        Scanner input = new Scanner(socket.getInputStream());
        //实例化输出对象类,从对应服务器端获取输出流信息
        PrintWriter out = new PrintWriter(socket.getOutputStream());
        //调用读取信息的线程类，进行启动线程
        new MySocketReader(input).start();
        //调用写入信息的线程类，进行启动线程
        new MySocketWriter(out).start();
    }
}

```

MySocketReader:

```java
package com.guangsha.thread;

import java.net.Socket;
import java.util.Scanner;

/**
 * @author lenovo
 * @remark 读取消息线程对象类
 * @creatTime 2023年6月1日10:14:10
 * @version 1.0
 * */
public class MySocketReader extends Thread{
    //声明键盘录入工具类
    private Scanner input;
    public MySocketReader(Scanner input){
        this.input =input;//进行赋值处理
    }
    /**
     * 核心方法
     *  */
    @Override
    public void run() {
        while (true) {
            try {
                //打印输出读取到的信息
                System.out.println("读取对端消息:"+input.nextLine());
            }catch (Exception e){
                System.out.println("已断开连接!");
            }
        }
    }
}
```

MySocketWriter

```java
import java.io.IOException;
import java.io.PrintWriter;
import java.net.Socket;
import java.util.Scanner;

/**
 * @author lenovo
 * @remark 写入消息线程对象类
 * @creatTime 2023年6月1日10:14:10
 * @version 1.0
 * */
public class MySocketWriter extends Thread{
    private Socket socket;
    private String host;//记录客户端的IP地址信息
    //声明键盘录入工具类
    private Scanner input;
    //声明打印输出对象类
    private PrintWriter out;
    /**
     * 有参构造方法
     * @param out 打印输出对象
     * */

    public MySocketWriter(PrintWriter out){
        this.out= out;
        this.socket = socket;
        //通过socket获取远端计算机地址信息
        host =socket.getInetAddress().getHostAddress();
    }
    /**
     * 核心方法
     * */
    @Override
    public void run() {
        //输出提示连接成功,可以进行聊天
        out.println("与服务端连接成功!,开始聊天吧！");
        //刷新缓冲区
        out.flush();
        //使用循环进行聊天处理
        while(true){
            try {
                //从键盘接受输入信息
                input = new Scanner(System.in);
                //定义String变量接受消息内容
                String msg = input.nextLine();
                
                //使用PrintWriter对象进行输出
                out.println(msg);
                //刷新缓冲区
                out.flush();
            }catch (Exception e) {
                System.out.println("已断开连接!");
            }
        }
    }
}

```

## 集合

### 什么是集合

1. 集合是Java API所提供的一系列类，可以用于动态存放多个对象。--`集合只能存对象`
2. 集合与数组的不同在于，集合是大小可变的序列，而且元素类型可以不受限定，只要是引用类型。(集合中不能放基本数据类型，但可以放基本数据类型的包装类)
3. 集合类可以自动扩容。
4.  集合类全部支持**泛型，**是一种数据安全的用法。

### 集合框架总体结构

​	java.util.Set接口及其子类，set提供的是一个无序，且不能重复的集合；

​	java.util.List接口及其子类，List提供的是一个有序，可以重复的集合；

​	 java.util.Map接口及其子类，Map提供了一个映射（对应）关系的集合数据结构；

Java的集合框架从整体上可以分为两大家族。

1. Collection(接口)家族。该接口下的所有子孙均存储的是单一对象， 描述所有集合共性的接口
2. Map(接口)家族。该接口下的所有子孙均存储的是key-value(键值对)形式的数据

### Collection接口

#### 常用的方法

1. int size();　返回此collection中的元素数。

2. boolean isEmpty();　判断此collection中是否包含元素。

3. boolean contains(Object obj);　判断此collection是否包含指定的元素。

4. boolean contains(Collection c);　判断此collection是否包含指定collection中的所有元素。

5. boolean add(Object element);　向此collection中添加元素。

6. boolean addAll(Collection c);将指定collection中的所有元素添加到此collection中

7. boolean remove(Object element); 从此collection中移除指定的元素。

8.  boolean removeAll(Collection c);　移除此collection中那些也包含在指定collection中的所有元素。

9. void clear();　移除些collection中所有的元素。

10. boolean retainAll(Collectionc);　方法用于保留 arraylist 中在指定集合中也存在的那些元素，也就是删除指定集合中不存在的那些元素。

11. Iterator iterator();　返回在此collection的元素上进行迭代的迭代器。(详情看后面迭代器部分)

12. Object[]toArray();把此collection转成数组。

#### List接口

List接口：存放的元素**有序且允许有重复**的集合接口。

```java
// 示例
package cn.sz.gl.test01;

import java.util.ArrayList;
import java.util.List;

public class Test02 {

	public static void main(String[] args) {
		ArrayList list = new ArrayList();

		list.add("zhangsan");
		list.add("lisi");
		list.add("wangwu");

		System.out.println("集合长度："+list.size());
		//list.clear();//清空集合中的所有数据
		System.out.println("调用clear方法后，集合长度："+list.size());

		boolean flag = list.contains("lisis");//判断集合中是否包含指定的数据
		System.out.println("判断集合中是否包含指定的数据:"+flag);

		int index = list.indexOf("wangwu");//用来获取元素保存在list集合中的索引号
		System.out.println("获取指定数据的索引号:"+index);

		boolean isempty = list.isEmpty();
		System.out.println("判断集合是否有元素："+isempty);

		int index2 = list.lastIndexOf("wangwu");
		System.out.println("从后往前查找指定的元素，并返回元素的索引号："+index2);

		Object obj = list.remove(2);//用来删除指定位置的元素， 并返回该已经删除的元素
		System.out.println("删除第2位的数据后，集合的长度："+list.size());
		System.out.println("删除的元素是："+obj);

		boolean isRemove = list.remove("lisi");//删除指定的元素，并返回是否删除成功
		System.out.println("删除lisi后，集合的长度："+list.size());
		System.out.println("删除lisi后，返回值是："+isRemove);

		list.set(0, "zhangsans");//修改指定位置的元素
		System.out.println("集合长度："+list.size());
		System.out.println("set之后，元素内容："+list.get(0));



		list.add("111");
		list.add("222");
		list.add("333");
		list.add("xxxx");
		list.add("yyyy");
		list.add("zzz");
		List list2 = list.subList(2, 5);
        //获取原list集合中的某一部分元素(获取子集)，这里包含第2个，但不包含第5个
		for (int i = 0; i < list2.size(); i++) {
			System.out.print(list2.get(i)+"\t");
		}
		System.out.println();
		System.out.println("***************************************");

		Object os [] = list.toArray();//把集合转变为一个数组
		for (int i = 0; i < os.length; i++) {
			System.out.print(os[i]+",");
		}

	}
}
```

ArrayList和LinkedList的区别

| ArrayList                                    | LinkedList                                 |
| -------------------------------------------- | ------------------------------------------ |
| 底层封装数组实现，分配的是一块连续的内存空间 | 底层封装链表实现，分配的是不连续的内存空间 |
| 读取数据效率更高                             | 读取数据效率相对较低                       |
| 增，删等操作效率相对偏低                     | 增，删等操作效率高                         |

#### Set接口

1. HashSet

   a. 实现了 Set 接口

   b. “它不保证 set 的迭代顺序；特别是它不保证该顺序恒久不变”

   c. 允许使用 null 元素

2. LinkedHashSet

   a.HashSet的子类

   b.由于该实现类对象维护着一个运行于所有元素的双重链接列表，由于该链接列表定义了迭代顺序，所以在遍历该实现类集合时按照元素的**插入顺序**进行遍历

3. TreeSet

   a.既实现Set接口，同时也实现了SortedSet接口，具有排序功能

   b.存入TreeSet中的对象元素需要实现Comparable接口

Set集合没有提供get方法，所以对Set集合的遍历只能通过加强for循环和迭代器进行遍历

## 反射

获取Class对象的三方式

```java
//类名.class
Class user1 = User.class;
//对象.getClass()
Class user2 = new User().getClass;
//Class.forName("全路径")
Class user3 = Class.forName("com.jason.test.bean.auto.User");
//实例化
User user4 = (User) user.getDeclaredConstructor().newInstance();
```

获取构造方法

```java
  @Test
    public void test01() throws Exception {
        Class userClass = User.class;
        //getConstructors获取所有公共的构造
        for (Constructor c : userClass.getConstructors()) {
            System.out.println("参数名称"+c.getName()+"参数个数"+c.getParameterCount());
        }
        //getDeclaredConstructors获取所有构造方法(包含私有)
        for (Constructor dc : userClass.getDeclaredConstructors()) {
            System.out.println("参数名称"+dc.getName()+"参数个数"+dc.getParameterCount());
        }
    }
```

指定有参构造创造对象

```java
    @Test
    public void test01() throws Exception {
        Class userClass = User.class;
        //指定public有参构造创建对象
//        Constructor constructor = userClass.getConstructor(String.class, int.class);
//        User user = (User) constructor.newInstance("milet", 18);
       //指定private有参构造创建对象
        Constructor constructor1 = userClass.getDeclaredConstructor(String.class, int.class);
        //设置允许访问
        constructor1.setAccessible(true);
        User user2 =(User) constructor1.newInstance("milet", 18);
        System.out.println(user2);
    }
```

获取属性

```java
   @Test
    public void test01() throws Exception {
        Class userClass = User.class;
        User user = (User ) userClass.getDeclaredConstructor().newInstance();
        //获取public属性
        //Field[] field = userClass.getFields();
        //获取所有属性(包含私有属性) 这里获取的是类的自身声明的属性，不包括从父类继承的属性
       Field[] fileds = userClass.getDeclaredFields();
        for (Field filed : fileds) {
           if(filed.getName().equals("Nickname")) {
               //设置允许访问
               filed.setAccessible(true);
               filed.set(user, "milet");
           }
            System.out.println(user);
        }
        System.out.println(user);
    }
```

获取方法

```java
 @Test
    public void test01() throws Exception {
        User user = new User("milet", 19);
        Class userClass = user.getClass();
        //1.获取所有public方法
        Method[] methods = userClass.getMethods();
        for (Method method : methods) {
            //执行指定的方法
            if(method.getName().equals("toString")){
                //用invoke去执行方法
                System.out.println(method.invoke(user));
            }
        }
        /*私有方法 用getDeclareMethods()方法获取，
         然后在invoke之前用setAccessible允许访问 大致相同 */
    }
```

