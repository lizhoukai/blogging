---
title: 理解作用域
date: 2015-11-01
tags: JavaScript
---

# 什么是作用域
- 域：空间、范围、区域
- 作用：读、写

----



# 浏览器的“JS解析器”
“JS解析器”：浏览器中专门用来读JS的程序。它至少做下面两件事（当然还有其他事情）：

1. 准备工作（“找一些东西”）：根据`var、function、`参数找东西 —— **JS的预解析**
   - 所有的变量，在正式运行代码之前，都提前赋了一个值：未定义（`a = 未定义`）
   - 所有的函数，在正式运行代码之前，都是整个函数块（`fn1 = function fn1(){alert(2);}`）
   - 遇到重名的，只留一个

2. 逐行解读代码
    - 每读一行，就会回到预解析的库里面去看一眼。
    - 碰上表达式（带有 `= + - * / % ++ -- ! `参数……都是表达式）能够改变值的是表达式。碰上表达式，就到库里面去修改值。

<!-- more -->

```js
alert(a); //未定义
var a=1; //在库里面a的值由未定义换成1
function fn1(){alert(2);}
```

```js
alert(a); //弹出：function a(){alert(4);}
var a=1; //预解析中的a改为了1
alert(a);  //弹出1
function a(){alert(2);}//函数声明，没有改变a的值。什么也没发生。
alert(a); //继续弹出1，因为a在预处理库里面的值没有被改过。
var a=3; //预处理中a的值变为3
alert(a); //弹出3
function a(){alert(4);} //函数声明，什么也没有发生
alert(a); //继续弹出3
a(); //报错 a is not a function
```

> 以上代码在预解析中的过程如下：

** 1）**预解析： `var function` 参数 ……
读到 `var a = 1` => `a` 未定义
读到 `function a(){alert(2);}` => `a = functiona(){alert(2);}`
变量与函数重名了，就只留下函数：
`a = function(){alert(2);}`
读到 `var a = 3` => `a` 未定义，这里与上面名为`a`的函数重名了，所以还是保留`a = function(){alert(2);}`
读到`function a(){alert(4);}` => `a = function a(){alert(4);}` 与前面的 `a = function(){alert(2);}`重名，根据上下文，后面的函数覆盖了前面的函数，所以预解析就只留下了 `a = function(){alert(4);}`

** 2）**逐行解读代码
读到表达式，表达式可以修改预解析的值。参数也可以改变预解析的值。
遇到函数调用，又要做两件事：预解析、逐行解析代码
函数声明，不是表达式，不改变预解析里面的值。

----

# 作用域

- 在 `<script>` 标签里面声明的函数和变量，都是全局函数和全局变量（但是 ** js解析器 **要把一块的事情处理完，然后处理另外一块事情）自上而下
- 函数：函数也是个域，也是先找后执行。由里到外
- 对象 {}

> 作用域链：从子级作用域跳到父级作用域的过程

```js
<script>
alert(a); //报错：a is not defined。因为在预解析里面压根就没有a这个东西
</script>

...

<script>
var a = 1;
</script
````

```js
<script>
alert(a); //不报错，会弹出undefined。因为预解析里面有a这个东西，但是由于执行的时候，还没有改变a的值，因此a的值为undefined
var a = 1;
</script>

...

<script>
</script>
```

```js
<script>
var a = 1; //预解析里面存了a，并且a的值存为1 并且a是全局变量
</script>

...

<script>
alert(a); //弹出1
</script>
```


```js
var a = 1; //预处理中的全局变量a的值改为1
function fn1(){
    alert(a);
    var a = 2;
} //函数声明，什么也不做
fn1(); //遇到函数调用，开始进行预解析和逐行解读代码。在函数内，先预解析出一个局部的 a 是未定义（局部的a与全局的a一点关系都没有）；然后读代码，alert(a)弹出的是undefined；然后继续执行，遇到表达式，将局部的变量a的值改为2。这时fn1的函数执行已经完成了。
alert(a); //弹出全局变量a为1
```

> 以上代码在js解析器中的模拟如下：

**1）**预解析 var function 参数……
```js
a = ...
fn1 = function fn1(){
alert(a);
var a=2;
}
```
**2) **逐行解析代码

> 另外一段代码：

```js
var a = 1; //预处理中的全局变量a的值改为1
function fn1(){
    alert(a);
    a = 2;
} //函数声明，什么也不做
fn1(); //遇到函数调用，开始进行预解析和逐行解读代码。在函数内，没有任何函数和变量声明，因此预解析里面没东西；然后读代码，alert(a)，在局部没有找到预解析的a，于是从子级作用域跳到父级作用域去找，找到了全局的a，所以弹出的是全局变量a的值1；然后继续执行，遇到表达式，将全局变量a的值改为2。这时fn1的函数执行已经完成了。
alert(a); //弹出全局变量a为2
```

```js
var a = 1; //预处理中的全局变量a的值改为1
function fn1(a){ //参数本质上就是一个局部变量
    alert(a);
    a = 2;
} //函数声明，什么也不做
fn1(); //遇到函数调用，开始进行预解析和逐行解读代码。在函数内，找到参数a，因此预处理里面有个局部的a是未定义。；然后读代码，alert(a)，弹出的是局部的a为undefined；然后继续执行，遇到表达式，将局部变量a的值改为2。这时fn1的函数执行已经完成了。
alert(a); //弹出全局变量a为1
```

```js
var a = 1; //预处理中的全局变量a的值改为1
function fn1(a){ //参数本质上就是一个局部变量
    alert(a);
    a = 2;
} //函数声明，什么也不做
fn1(a); //遇到函数调用，开始进行预解析和逐行解读代码。在函数内，找到局部参数a，因此预处理里面有个局部的a是未定义。；然后读代码，读到第一行function fn1(a)，这时有参数进来，把全局的a的值1赋给了局部变量a，这时局部变量a的值变为1，alert(a)，弹出的是局部的a为1；然后继续执行，遇到表达式，将局部变量a的值改为2。这时fn1的函数执行已经完成了。
alert(a); //弹出全局变量a为1
```

----

# 获取函数内的值
- 方法一、把全局变量用到函数里面。因为函数可以读写全局变量。见下面的代码：
```js
var str = '';
function fn1(){
    var a = '大鸡腿~';
    str = a;
}
fn1();
```
- 方法二，在函数里面调用全局函数，通过传参实现，见下面代码：
```js
function fn2(){
    var a = '99999999克拉钻石';
    fn3(a);
}
fn2();
function fn3(a){ //注意这个a是fn3里面的，与fn2里面的a毫无关系
    alert(a);//99999999克拉钻石
}
```

## 注意事项

函数的大括号是作用域；`if(){}`不是作用域；`for(){}`也不是作用域。只有作用域是先预解析、再执行。`if`和`for`的大括号没有预解析过程。
```js
alert(a);
//弹出undefined。这说明if大括号中的a已经在全局就被预解析了，因此if的大括号并不是作用域

if(true){
    var a=1;
}
```

但是，需要注意：

```js
alert(fn1);
//在火狐浏览器里面会报错说fn1 is not defined。但是在chrome浏览器里可以正常弹出fun1函数。

if(true){
    var a = 1;
    function fn1(){
        alert(123);
    }
}
```
只有火狐浏览器预解析时，不能对`if`或`for`的括号里括起来的函数进行预解析。因此存在兼容性问题。因此，如果想要定义`全局函数`或`全局变量`，尽量不要放到`if`和`for`的大括号里面，放到大括号外面。

```js
for(var i=0; i<aBtn.length; i++){
    aBtn[i].onclick = function(){
        alert(i);
        //弹出3。因为必须for循环全部执行完成之后，aBtn才能够点击。
        //但是for循环执行完了，外面的i已经变成3；但是在function里面又没有i，自然找到了外面的i，也就是3.
    }
}
```

```js
for(var i=0; i<aBtn.length; i++){
    aBtn[i].onclick = function(){
        alert(i); //这时候弹出的就不是3了，是undefined
        //函数是作用域那就要预解析，alert(i) 找i的申明处，下面的var i= 0；所以是undefined
        for(var i=0; i<aBtn.legnth; i++){
            aBtn[i].style.background = 'yellow';
        }
    }
}
```
```js
for(var i=0; i<aBtn.length; i++){
    aBtn[i].onclick = function(){
        alert(i);
        //下面for循环的括号中的var删去了，function里面又找不到i了，所以到父级找到了i，又是个3。因此，此处会弹出3。
        for (i = 0; i<aBtn.length; i++){
            aBtn[i].style.background = 'yellow';
        }
    }
}
```

