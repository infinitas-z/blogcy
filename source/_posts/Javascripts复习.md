---
title: Javascripts复习
date: 2023-07-25 17:32:18
categories: 学习笔记
tags: [JavaScript]
top_img: https://pic.imgdb.cn/item/64c24b2d1ddac507cc509e09.png
cover: https://pic.imgdb.cn/item/64c24b2d1ddac507cc509e09.png
---

## 基础

### 语句

HTML 输出流中使用 document.write，相当于添加在原有html代码中添加一串html代码。而如果在文档加载后使用（如使用函数），会覆盖整个文档。

```html
<body>
    <p> 未被覆盖的文本</p>
    <button type="button" onclick="myfunction()">点击</button>
    <script>
        function myfunction(){
            document.write("覆盖文本")
        }
        document.write("<h1>这是一个标题<h1>")
    </script>
</body>
```

- 使用 **window.alert()** 弹出警告框
- 使用 **document.write()** 方法将内容写到 HTML 文档中。
- 使用 **innerHTML** 写入到 HTML 元素，使用 document.getElementById(*id*) 方法。请使用 "id" 属性来标识 HTML 元素，并 innerHTML 来获取或插入元素内容
- 使用 **console.log()** 写入到浏览器的控制台。主要是方便你调式javascript用的, 你可以看到你在页面中输出的内容

如果重新声明 JavaScript 变量，该变量的值不会丢失：

```javascript
var nickname ="张三";
var nickname; //输出的值还是张三
console.log(nickname);
```

字符串赋值可以是单引号，也可以是双引号，利用toString可以把数值转换为字符串,toString将值转换为字符串，number()转换为数字，null和undefined一个是没有值，一个是未初始化，两个的值一样，类型不一样，toUpperCase()方法将字符全部大写，toLowerCase()全部小写

创建数组，判断是否为数组用isArray，如果用typeof判断数组返回的是object

```javascript
var cars = new Array("a","b","c");
var cars02 = ["a","v","c"];
cars.push("cc")//在数组后面添加元素
cars.unshift("aa")//在数组头部添加元素
cars.pop()//删除数组末尾的额元素
cars.indexof("a")//获取元素所对应的下标
```

### 变量

const，let，var

块内用var再声明块外的变量，会改变块外的变量，let就可以解决，在相同作用域下不能使用 **let** 关键字来重置let和var声明的变量

```javascript
var x = 10;
// 这里输出 x 为 10
{ 
    //var x= 2; 如果使用var，下面输出就是2
    let x = 2;
    // 这里输出 x 为 2
}
// 这里输出 x 为 10
```

const 用于声明一个或多个常量，声明时必须进行初始化，且初始化后值不可再修改,我们可以使用const定义对象或者数组，这些数组是可以被修改或者添加属性的，但是不能重新赋值，var

```javascript
const PI = 3.141592653589793;
PI = 3.14;      // 报错
PI = PI + 10;   // 报错
const cars = ["Saab", "Volvo", "BMW"];
cars[0] = "Toyota" //允许
cars = ["Toyota", "Volvo", "Audi"];    // 错误
```



### 创建对象(常用)

第一种

```javascript
function Demo(){
    var obj = new Object();
    obj.name ="张三";
    obj.age = 12;
    obj.firstF =function(){}
    return obj;
}
var one = Demo();
console.log(one.name);
```

第二种

```javascript
function Demo(){
    this.name="张思";
    this.age=12;
    this.firstF=function(){
    }
}
var one=new Demo
// 调用输出
document.write(one.age);
```

对象属性获取

```javascript
const person={
    nickName:"nick",
    age:30,
    hobbies:["music","movies"],
    address:{
        street:"50 st",
        city:"China",
    },
};
const {nickName ,age,address:{city},} =person //可以对同名的对象属性取值
console.log(nickName,age,city);
person.email ="asudhaos@gmail.com"//添加新的属性
console.log(person)
```



示例 js实现简单的加减运算

```html
<body>
    <table border="1" style="position: center;">
        <tr>
            <td>第一个数</td>
            <td><input type="text" id="first"></td>
        </tr>
        <tr>
            <td>第二个数</td>
            <td><input type="text" id="twice"></td>
        </tr>
        <tr>
            <td colspan="2">
                &nbsp;
                <button style="width: inherit;" onclick="add()">+</button>
                &nbsp;
                <button style="width: inherit;" onclick="subtract()">-</button>
                &nbsp;
                <button style="width: inherit;" onclick="ride()">*</button>
                &nbsp;
            </td>
        </tr>
        <tr>
            <td colspan="2" rowspan="1">
                <p id="result"></p>
            </td>
        </tr>
    </table>
    <script>
        var result_1;
        //获取第一个，第二个值
        function getFirstNum(){
            var firstnum = document.getElementById("first").value;
            return firstnum;
        }
        function getSecoundNum(){
            var Secoundnum = document.getElementById("twice").value;
            return Secoundnum;
        }
        //加法
        function add(){
            var num1 = getFirstNum();
            var num2 = getSecoundNum();
            sendRequest(Number(num1)+Number(num2));
        }
        //减法
        function subtract(){
            var num1 = getFirstNum();
            var num2 = getSecoundNum();
            sendRequest(num1-num2)
        }
        //乘法
        function ride(){
            var num1 = getFirstNum();
            var num2 = getSecoundNum();
            sendRequest(num1*num2)
        }
        function sendRequest(result){
            document.getElementById("result").innerHTML=result
        }
    </script>
</body>
```

如果变量在函数内没有声明（没有使用 var 关键字），该变量为全局变量

### 事件

| 事件        | 描述                               |
| ----------- | ---------------------------------- |
| onchange    | HTML元素改变                       |
| onclick     | 用户点击HTML元素                   |
| onmouseover | 指针移动到指定的元素上时发生       |
| onmouseout  | 用户从一个HTML元素上一开鼠标时发生 |
| onkeydown   | 用户按下键盘按键                   |
| onload      | 浏览器已完成页面的加载             |

举例，点击的点击事件

```html
<button id="test" onclick="changeContent()">更换内容</button>
<script>
var test = document.getElementById("test");
test.onclick = function changeContent(){
    document.getElementById("result").innerHTML=result
}
</script>
```

添加事件句柄示例

```javascript
element.addEventListener(event, function, [useCapture])
//删除句柄
element.removeEventListener(event, function, [useCapture])
```

```html
<input id="test" type="button" value="提交"/>
<script>
window.onload = function(){
    var test = document.getElementById("test");
    /*为元素添加事件句柄或者删除元素事件句柄的过程中，
    不要将 event 参数设置为 onclick，而必须写成 click*/
    test.addEventListener("click",myfun2);   
    test.addEventListener("click",myfun1);
}
function myfun1(){  
    alert("你好1");
}
function myfun2(){ 
    alert("你好2");
}
```

###  == 与 === 

1、对于 string、number 等基础类型，== 和 === 是有区别的

- 不同类型间比较，== 之比较 "转化成同一类型后的值" 看 "值" 是否相等，=== 如果类型不同，其结果就是不等。
-  同类型比较，直接进行 "值" 比较，两者结果一样。

2、对于 Array,Object 等高级类型，== 和 === 是没有区别的

进行 "指针地址" 比较

3、基础类型与高级类型，== 和 === 是有区别的

- 对于 ==，将高级转化为基础类型，进行 "值" 比较
-  因为类型不同，=== 结果为 false

4、!= 为 == 的非运算，!== 为 === 的非运算

```javascript
var num=1；
var str="1"；
var test=1；
test == num   //true　相同类型　相同值 
test === num  //true　相同类型　相同值 
test !== num  //false test与num类型相同，其值也相同,　非运算肯定是false 
num == str   //true 　把str转换为数字，检查其是否相等。 
num != str   //false  == 的 非运算 
num === str  //false  类型不同，直接返回false 
num !== str  //true   num 与 str类型不同 意味着其两者不等　非运算自然是true啦
```

### 模板字符串

模板字符串使用反引号 **``** 作为字符串的定界符分隔的字面量,允许你在字符串中嵌入表达式和变量。除了普通字符串外，模板字面量还可以包含占位符——一种由美元符号和大括号分隔的嵌入式表达式：**${expression}**

```javascript
const name = 'Runoob';
const age = 30;
const message = `My name is ${name} and I'm ${age} years old.`;
```

### switch

```
switch(n)
{
    case 1:
        执行代码块 1
        break;
    case 2:
        执行代码块 2
        break;
    default:
        与 case 1 和 case 2 不同时执行的代码
}
```

### for

for...in 循环会自动跳过那些没被赋值的元素，而 for 循环则不会，它会显示出 undefined。

### 正则表达式

匹配一系列符合某个句法规则的字符串搜索模式。搜索模式可用于文本搜索和文本替换。

```
/正则表达式主体/修饰符(可选)
```

正则表达式模式

方括号用于查找某个范围的字符

| 表达式 | 描述                     |
| ------ | ------------------------ |
| [abc]  | 查找方括号之间的任意字符 |
| [0-9]  | 查找0到9                 |
| (x\|y) | 查找任何以\|分隔的选项   |

元字符是拥有特殊含义的字符

| 元字符 | 描述                                |
| ------ | ----------------------------------- |
| \d     | 查找数字                            |
| \s     | 查找空白字符                        |
| \b     | 匹配单元边界                        |
| \uxxxx | 查找以十六进制xxxx规定的Unicode字符 |

量词:

| 量词 | 描述·                                 |
| ---- | ------------------------------------- |
| n+   | 匹配任何包括至少一个n的字符串         |
| n*   | 匹配任何包含零个或多个 *n* 的字符串。 |
| n?   | 匹配任何包含零个或一个 *n* 的字符串。 |



正则表达式通常用于两个字符串方法 : search() 和 replace()。

- **search()** 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，并返回子串的起始位置。

  ```javascript
  var str = "Visit Runoob!"; 
  var n = str.search("Runoob");//6
  ```

- **replace()** 方法用于在字符串中用一些字符串替换另一些字符串，或替换一个与正则表达式匹配的子串。返回字符串

  ```javascript
  var str = document.getElementById("demo").innerHTML; 
  var txt = str.replace("Microsoft","Runoob");
  ```


- test() 方法，用于检测一个字符串是否匹配某个模式,返回布尔值

  ```javascript
  var patt = /e/;
  patt.test("The best things in life are free!");//true
  //或者合并
  /e/.test("The best things in life are free!")//true
  ```

- exec() 方法，用于检索字符串中的正则表达式的匹配返回数组，存放匹配结果，如果未找到显示null

  ```javascript
  /e/.exec("The best things in life are free!");//e
  ```


示例：

```javascript
/*是否带有小数*/
function    isDecimal(strValue )  {  
   var  objRegExp= /^\d+\.\d+$/;
   return  objRegExp.test(strValue);  
}  
/*校验是否中文名称组成 */
function ischina(str) {
    var reg=/^[\u4E00-\u9FA5]{2,4}$/;   /*定义验证表达式*/
    return reg.test(str);     /*进行验证*/
}
/*校验是否全由8位数字组成 */
function isStudentNo(str) {
    var reg=/^[0-9]{8}$/;   /*定义验证表达式*/
    return reg.test(str);     /*进行验证*/
}
/*校验电话码格式 */
function isTelCode(str) {
    var reg= /^((0\d{2,3}-\d{7,8})|(1[3584]\d{9}))$/;
    return reg.test(str);
}
/*校验邮件地址是否合法 */
function IsEmail(str) {
    var reg=/^\w+@[a-zA-Z0-9]{2,10}(?:\.[a-z]{2,4}){1,3}$/;
    return reg.test(str);
}
```

### 表单验证

手动验证

```html
 <form name="myForm" onsubmit="return validateForm(this)">
        <input type="text" name="nickname">
        <input type="submit" name="submit" value="提交">
    </form>
    <script>
        function validateForm(obj){
            var x = obj.nickname.value
            if(x == null || x==""){
                alert("请输入");
                return false;
            }
        }
    </script>
```

**onsubmit="return validateForm()"** 为什么不是 **onsubmit="validateForm()"** ？？

**onsubmit="validateForm()"** 能够调用 **validateForm()** 对表单进行验证，但是在验证不通过的情况下，并不能阻止表单提交，onsumbit默认返回true

浏览器自动验证

```html
<form action="demo_form.php" method="post">
  <input type="text" name="fname" required="required">
  <input type="submit" value="提交">
</form>
```

约束验证

| 属性     | 描述                     |
| -------- | ------------------------ |
| disabled | 规定输入的元素不可用     |
| max      | 规定输入元素的最大值     |
| min      | 规定输入元素的最小值     |
| pattern  | 规定输入元素值的模式     |
| required | 规定输入元素字段是必需的 |
| type     | 规定输入元素的类型       |

Js中的this关键字

在方法中，this 表示该方法所属的对象。如果单独使用，this 表示全局对象，在函数中，this 表示全局对象。在事件中，this 表示接收事件的元素

void关键字仅仅是代表不返回任何值，但是括号内的表达式还是要运行，用法

```javascript
// 阻止链接跳转，URL不会有任何变化
<a href="javascript:void(0)" rel="nofollow ugc">点击此处</a>

// 虽然阻止了链接跳转，但URL尾部会多个#，改变了当前URL。（# 主要用于配合 location.hash）
<a href="#" rel="nofollow ugc">点击此处</a>

```

## JavaScript 异步编程

线程作为一个线程，不能够同时接受多方面的请求。所以，当一个事件没有结束时，界面将无法处理其他请求。现在有一个按钮，如果我们设置它的 onclick 事件为一个死循环，那么当这个按钮按下，整个网页将失去响应。

为了避免这种情况的发生，我们常常用子线程来完成一些可能消耗时间足够长以至于被用户察觉的事情，比如读取一个大文件或者发出一个网络请求。因为子线程独立于主线程，所以即使出现阻塞也不会影响主线程的运行。但是子线程有一个局限：一旦发射了以后就会与主线程失去同步，我们无法确定它的结束,JavaScript 中的异步操作函数往往通过回调函数来实现异步任务的结果处理。

### 回调函数

回调函数就是一个函数，它是在我们启动一个异步任务的时候就告诉它：等你完成了这个任务之后要干什么。这样一来主线程几乎不用关心异步任务的状态了，他自己会善始善终。

setTimeout就是个比较消耗时间的过程，经过3000毫秒之后，打印输出RUNOOB!

```javascript
setTimeout(function () {
    document.getElementById("demo").innerHTML="RUNOOB!";
}, 3000);
```

案例2：settimeout执行时，主程序并没有结束，主程序先执行

```javascript
setTimeout(function () {
    document.getElementById("demo1").innerHTML="RUNOOB-1!";  // 三秒后子线程执行
}, 3000);
document.getElementById("demo2").innerHTML="RUNOOB-2!";      // 主线程先执行
```

### 异步AJAX

数据转换：https://www.freeformatter.com/json-formatter.html

JSON.stringify(对象),可以将对象数据变为json格式

XMLHttpRequest 常常用于请求来自远程服务器上的 XML 或 JSON 数据。一个标准的 XMLHttpRequest 对象往往包含多个回调

```javascript
var xhr = new XMLHttpRequest();
 
xhr.onload = function () {
    // 输出接收到的文字数据
    document.getElementById("demo").innerHTML=xhr.responseText;
}
 
xhr.onerror = function () {
    //请求失败时调用
    document.getElementById("demo").innerHTML="请求出错";
}
 
// 发送异步 GET 请求
xhr.open("GET", "https://www.runoob.com/try/ajax/ajax_info.txt", true);
xhr.send();
```

示例：发送一个 HTTP GET 请求并获取返回结果,需导入jquery包

```html
<script>
$(document).ready(function(){
	$("button").click(function(){
		$.get("/try/ajax/demo_test.php",function(data,status){
			alert("数据: " + data + "\n状态: " + status);
		});
	});
});
</script>
<button>发送一个 HTTP GET 请求并获取返回结果</button>
```

### Promise

如果要实现多异步任务，想要隔1s输出某个信息，隔2s输出xx...。使用函数瀑布过于繁琐,难维护，可以通过新建Promise对象更好的去写

```javascript
setTimeout(function () { //难以维护
    console.log("First");
    setTimeout(function () {
        console.log("Second");
        setTimeout(function () {
            console.log("Third");
        }, 3000);
    }, 4000);
}, 1000);

//Promise对象，起始函数，resolve和reject分别代表成功和失败状态
new Promise(function (resolve, reject) { 
    setTimeout(function () {
        console.log("First");
        resolve();
    }, 1000);
//then用于处理成功状态
//catch用于处理失败状态
//finally无论如何都会执行
}).then(function () {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            console.log("Second");
            resolve();
        }, 4000);
    });
}).then(function () {
    setTimeout(function () {
        console.log("Third");
    }, 3000);
});
```

Promise对象代表一个异步操作，有三种状态：Pending（进行中）、Resolved（已完成 ，又称Fulfilled）和 Rejected（已失败）。resolve(data)将这个promise标记为resolved，然后进行下一步then((data)=>{//do something})，resolve里的参数就是传入then的数据

- 执行到 resolve()这个方法的时候，就改变promise的状态为resolved，当状态为 resolved的时候就可以执行.then()

- 当执行到 reject() 这个方法的时候，就改变 promise的状态为 reject，当promise为reject就可以.catch()这个promise了

优化计时器程序可以将核心写成一个promise函数 在需要使用的时候调用

```javascript
function print(time,message){
    return new Promise((resolve,reject) =>{
        setTimeout(() =>{
            console.log(message);
            resolve();
        },time)
    })
}

print(1000,"Frist").then(()=>{
     print(2000,"secound");
}).then(()=>{
    print(3000,"Third");
});
```

也可以使用async，await来变得更好看，将异步变为同步

```javascript
function print(delay, message) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            console.log(message);
            resolve();
        }, delay);
    });
}
async function asyncFunc() {
    await print(1000, "First");
    await print(4000, "Second");
    await print(3000, "Third");
}
asyncFunc();
```

异步函数 async function 中可以使用 await 指令，await 指令后必须跟着一个 Promise，异步函数会在这个 Promise 运行中暂停，直到其运行结束再继续运行

```javascript
async function asyncFunc() {
    try {
        await new Promise(function (resolve, reject) {
          reject("Some error");
        });
    } catch (err) {
        console.log(err);
        // 会输出 Some error
    }
}
asyncFunc();
```



示例，如果异步操作成功 则食醋胡success反之 error

```javascript
 const promise = new Promise((resolve, reject) => {
  // 异步操作
  setTimeout(() => {
    if (Math.random() < 0.5) {
      resolve('success');
    } else {
      reject('error');
    }
  }, 1000);
});
 
promise.then(result => {
  console.log(result);
}).catch(error => {
  console.log(error);
});

//或者使用then，catch方法处理失败的回调函数
new Promise((resolve, reject) =>{
    var a = 0;
    var b = 1;
    if (b == 0) reject("Divide zero");
    //resolve() 中可以放置一个参数用于向下一个 then 传递一个值
    else resolve(a / b);
}).then(value => {
    console.log("a / b = " + value);
}).catch(err =>{
    console.log(err);
}).finally(() => {
    console.log("End");
});
```



