---
title: 理解函数返回值-return详解及应用
date: 2015-11-01
---

- `return` 返回值：函数名+括号（例如fn1())就相当于得到了该函数里面return后面的值
- 所有函数默认返回值是：undefined
- 可返回：数字、字符串、布尔、函数、对象（元素、[]、{}、null）、未定义
- `return` 后面的任何代码都不执行了

<!-- more -->

## return实例

```js
alert fn1();
function fn1(){
    return function(){
        alert(1);
    }
}
//此时弹出的是：function(){alert(1);}
```

```js
alert fn1()();
function fn1(){
    return function(){
        alert(1);
    }
}
//此时弹出的是1，也就是return的匿名函数执行的结果。
```

```js
alert fn1()(10);
function fn1(){
    return function(a){
        alert(a);
    }
}
//此时弹出的是10。
```

## arguments

`fn1(1, 2, 3)` //实参——实际传递的参数

`function fn1(a, b, c)` //形参——形式上，a、b、c这些名代表1、2、3

如果不写形参，1、2、3也能够传进来，都存在arguments的肚子里。

function fn1(){
//arguments => [1, 2, 3] —— 实参的集合（不是数组，但是类似数组，有length，也可以用下标找到其中的数据）
}

> 当函数参数个数无法确定的时候，用arguments：

```js
alert(sum(1,2,3));
function sum(){
    var n=0;
    for(var i=0; i<arguments.length; i++){
        n += arguments[i];
    }
    return n;
}
```

```js
var a=1;
function fn2(a){ //arguments的某个数值就相当于某个形参
    arguments[0]=3;
    alert(a); //弹出3
    var a=2;
    alert(arguments[0]); //弹出2
}
fn2(a);
alert(a); //弹出1
```

```js
var a=1;
function fn2(a){ //arguments的某个数值就相当于某个形参
    arguments[0]=3;
    alert(a); //弹出3
    var a=2;
    alert(arguments[0]); //弹出2
}
fn2(a);
alert(a); //弹出1
```

## currentStyle与getComputedStyle应用
- `getComputedStyle`获取的是计算机（浏览器）计算后的样式，但是不兼容IE6、7、8
- `currentStyle`方法兼容IE6、7、8，但是不兼容标准浏览器

属性判断法、版本检测法来解决浏览器间的兼容性问题

```js
function getStyle( obj, attr) {
    return obj.currentStyle ? obj.currentStyle[attr]:getComputedStyle( obj )[attr];
}
```

`注意事项：`

- 如果用以上的方法获取某个元素的复合样式，例如`background`，那么就不要用上面那种方式获取，在不同浏览器间有兼容性问题。用上面的方法获得单一样式，而不要用来获取复合样式。
- 使用以上方法，注意不要多按空格
- 使用以上方法，不要获取未设置后的样式，因为浏览器间不兼容
- 在火狐4.0之前，有个`bug`，如果`getComputedStyle`后面不跟参数，会出现问题，所以有些人写成`getComputedStyle(obj, false)`，那个`false`就是为了解决这个`bug`。这里的bug也可以写成0，或者其他任何参数都可以。不过目前火狐的浏览器都比较高，因此这个问题已经不是很常见了。

