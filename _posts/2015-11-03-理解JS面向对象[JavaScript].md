# 什么是面向对象编程

- 用对象的思想去写代码，就是面向对象编程
    - 过程式写法
    - 面向对象写法
- 我们一直都在使用对象
    - 数组Array 时间Date

## 面向对象编程（OOP）的特点

- 抽象：抓住核心问题
- 封装：只能通过对象来访问方法
- 继承：从已有对象上继承出新的对象
- 多态：多对象的不同形态

## 对象的组成

方法（行为、操作） —— 对象下面的函数：过程、动态的
属性 —— 对象下面的变量：状态、静态的

## 创建第一个面向对象程序

```js
//var obj = {};
var obj = new Object(); //创建了一个空的对象

obj.name = '小明'; //属性
obj.showName = function(){ //方法
    alert(this.name);
}

obj.showName();
```

- 为对象添加属性和方法
    - Object对象
    - this指向
    - 创建两个对象：重复代码过多

```js
//var obj = {};
var obj = new Object(); //创建了一个空的对象

obj.name = '小明'; //属性
obj.showName = function(){ //方法
    alert(this.name);
}

obj.showName();
var obj2 = new Object();

obj2.name = '小强';
obj.showName = function(){
    alert(this.name);
}

obj2.showName();
```

## 工厂方式

> 面向对象中的封装函数

```js
//工厂方式：封装函数
function createPerson(name){
    //1. 原料
    var obj = new Object();

    //2. 加工
    obj.name = name;
    obj.showName = function(){
        alert(this.name);
    };

    //3. 出厂
    return obj;
}

var p1 = createPerson('小明');
p1.showName();
var p2 = createPerson('小强');
p2.showName();
```


