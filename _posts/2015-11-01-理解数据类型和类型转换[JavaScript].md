---
title: 理解数据类型和类型转换
date: 2015-11-01
tags: JavaScript
---

# 数据类型
- HTML标签类型：
    - block
    - inline
    - inline-block
    - table
    - ......
- JS中的数据类型（** 五种基本数据类型 ** 和 ** 一种复杂数据类型 **：object）：
    - `Undefined`（未申明，或者变量的值即为undefined或者未初始化）
    - `Null`（值为null）
    - `Number`（值是数字类型）
    - `String`（值是字符串类型）
    - `Boolean`（值是布尔类型）
    - `object` (对象或者值为null)
        - obj
        - []
        - {}
        - null （空是不能添加自定义属性的）


`typeof` 用来判断数据类型。
字符串方法 `charAt() `：通过字符串的下标获取子字符串。

<!-- more -->

----

# 数据类型转换
  把字符串转成数字的方法：

## Number()
```js
var a = '100';
alert(a + 100); //'100100'

var b = '00100';
alert(Number(b)); //100

var c = '+100';
alert(Number(c)); //100

var d = '';
alert(Number(d)); //0

var e = '   ';
alert(Number(e)); //0

var f = true;
alert(Number(f)); //1

var g = false;
alert(Number(g)); //0

var i = [];
alert(Number(i)); //0 空数组用Number转出来是0

var j = [''];
alert(Number(j)); //0

var k = [123];
alert(Number(k)); //123

var l = ['123'];
alert(Number(l)); //123

var o = null;
alert(Number(o)); //0

----------------------------

var m = [1,2,3];
alert(Number(m)); //NaN

var json = {abc:123};
alert(Number(json)); //NaN

var json2 = {};
alert(Number(json2)); //NaN 即使是空json，Number方法也转不了

var h = function(){};
alert(Number(h)); //NaN

var p;
alert(Number(p)); //undefined

var q = '100px';
alert(Number(q)); // NaN
```

## parseInt与parseFloat

```js
var a = '100px';
alert(parseInt(a)); //100

var b = '100px123456789';
alert(parseInt(b)); //100

var d = '-100px';
alert(parseInt(d)); //-100 parseInt还是认加减号的

var e = '   100px';
alert(parseInt(e)); //100 parseInt也是认空格的

var f = '000000100px';
alert(parseInt(f)); //100

var g = '12.34元';
alert(parseInt(g)); //12 parseInt只能转出整数

------------------------------

var c = true;
alert(parseInt(c)); //NaN

//parseInt也不能用于转函数等

```
> `parseInt()` 方法还有一个参数，就是按照几进制来转，比如`parseInt(a, 10)`表示按照十进制来转；`parseInt(b, 2)`表示按照二进制来转

```js
var a = '12.34元';
alert(parseFloat(a)); //12.34

var b = '12.3.4元';
alert(parseFloat(b)); //12.3 parseFloat只认第一个小数点
```

> `parseInt` 和 `parseFloat` 的配合使用，可以来判断某个值是整数还是小数，如下：


```js
var num = '200.345';

if(parseInt(num) == parseFloat(num)) {
  alert(num + '是整数');
} else {
  alert(num + '是小数');
}
```
----


# 隐式类型转换
显式类型转换（强制类型转换）：
- Number();
- parseInt();
- parseFloat();

隐式类型转换：
- `- * / % `减、乘、除、取模可以将字符串转成数字
- `+` 加号可以将数字转成字符串
- `++ --` 加加、减减运算符可以把字符串转成数字
- `< >` 大于号、小于号可以把字符串转成数字，一定要注意是进行数字的比较还是字符串的比较
- `! `取反 把右边的数据类型转成布尔值
- `==` 判断是否等于

```js
alert('200' - 3); //197
alert(200 + '3'); //2003

var a = '10';
a++;
alert(a); //11

alert('10'>9); //true
alert('1000000'>'9'); //false
//注意：数字的比较和字符串的比较不同；字符串的比较是一位一位的比较。

alert(!'ok'); //false
alert(!100); //false

alert('2' == 2); //true
alert('2' === 2); //false 三个等号不仅判断值，还会先判断两者的类型
```
隐式类型转换转不出来，也会返回一个NaN，例如：alert('......' - 9);就会弹出NaN。

----

# isNaN应用实例
`NaN`: (not a number)。`NaN`是个不是数字的数字类型。所有的数字都是数字类型，但不是所有的数字类型都是数字。
- `NaN`是个数字类型，但它不是个数字。
- 一旦写程序中出现了`NaN`，肯定说明进行了非法的运算操作。
- `NaN`是个`false`。
- `NaN`与自己都不相等。

```js
var a = Number('abc');
//alert(a); //NaN
//alert(typeof(a)); //number

if(a) { //会弹出‘假’，说明NaN是false
  alert('真');
} else {
  alert('假');
}

//alert(a === a); //false NaN比较，比出false
```

** isNaN：** is not a Number 是不是不是个数字（不是数字）。

`isNaN()` 是个方法，用来干活的。

- 帮助判断某些值是不是数字。
- 不喜欢数字、讨厌数字(不是讨厌数字类型，`isNaN`遇到`NaN`就很喜欢，会返回`true`，但是`NaN`就是个数字类型) 用来判断数字。如alert(isNaN(2)); 返回`false`。alert(isNaN('我爱你')); 返回`true`。alert(isNaN(function(){})); 返回`true`。

`isNaN` 的判断过程，将括号里面的东西扔给`Number`，`Number`转出来数字，就返回`false`；转不出来就返回`true`。

```js
alert(isNaN('250'));
//Number() '250' => 250 => false

alert(isNaN(true));
//Number() true => 1 => false

alert(isNaN([]));
//Number() [] => 0 => false
```
> 下面是一个isNaN的小应用：

```html
<input type="text" />
<input type="button" value="判断输入值是不是数字" />
```


```js
window.onload = function(){
  var aInp = document.getElementsByTagName('input');
  var str = '';

  aInp[1].onclick = function(){
    str = aInp[0].value
    //alert(type of str); //总是返回string 从HTML中拿到的内容，类型都是字符串

    if(isNaN(str)){
      alert(str + '不是数字');
    } else {
      alert(str + '是数字');
    }
  }
}
```


