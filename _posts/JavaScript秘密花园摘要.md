title: JavaScript秘密花园摘要
date: 2015-11-07 15:53:32
tags: [JavaScript]
---
## 1.对象
—————
### 对象使用和属性
JavaScript 中所有变量都可以当作对象使用，除了两个例外 `null` 和 `undefined`。

null表示"没有对象"，即该处不应该有值:
(1）作为函数的参数，表示该函数的参数不是对象。
(2）作为对象原型链的终点。

undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义:
(1）变量被声明了，但没有赋值时，就等于undefined。
(2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
(3）对象没有赋值的属性，该属性的值为undefined。
(4）函数没有返回值时，默认返回undefined。

字面值对象展示：
```javascript
false.toString(); // 'false'
[1, 2, 3].toString(); // '1,2,3'

function Foo(){}
Foo.bar = 1;
Foo.bar; // 1
```

<!-- more -->
> 一个常见的误解是数字的**字面值**（literal）不能当作对象使用。这是因为 JavaScript 解析器的一个错误， 它试图将点操作符解析为浮点数字面值的一部分。

```javascript
2.toString(); // 出错：SyntaxError
```
有很多变通方法可以让数字的字面值看起来像对象。

```javascript
2..toString(); // 第二个点号可以正常解析 结果为2
2 .toString(); // 注意点号前面的空格 结果为2
(2).toString(); // 2先被计算 结果为2
```
#### 对象作为数据类型
JavaScript 的对象可以作为哈希表使用，主要用来保存命名的键与值的对应关系。

使用对象的字面语法 **{}** 可以创建一个简单对象。这个新创建的对象从 **Object.prototype** `继承`下面，没有任何`自定义属性`。

```javascript
var foo = {}; // 定义了一个空对象

// 一个新对象，拥有一个值为24的自定义属性'age'
var kevinli = {age: 24};
```
#### 访问属性
有两种方式来访问对象的属性，点操作符或者中括号操作符。

```javascript
var kevinli = {
    age:'24',
    1234:'5678',
    sex :'男'//这边注意sex后面有个空格
}
kevinli.age;// 24
kevinli['age'];//24

var get = 'age';
kevinli[get];// 24

kevinli.1234; // SyntaxError
kevinli['1234'];//5678

kevinli.sex ;/这里就不能访问了
kevinli['sex '];//男
```

总结：两种语法是等价的，但是中括号操作符在下面两种情况下依然有效
- 动态设置属性
- 属性名不是一个有效的变量名（译者注：比如属性名中包含空格，或者属性名是 JS 的关键词）

#### 删除属性
删除属性的唯一方法是使用 `delete` 操作符；设置属性为 `undefined` 或者 `null` 并不能真正的删除属性， 而仅仅是移除了属性和值的关联。

```javascript
var obj = {
    kevinli: 1,
    tom: 2,
    sum: 3

};
obj.kevinli = undefined;
obj.tom = null;
delete obj.sum;

for(var i in obj) {
    if (obj.hasOwnProperty(i)) {
        console.log(i, '' + obj[i]);
    }
}
```
上面的输出结果有 **kevinli=undefined** 和 **tom=null** - 只有 **sum** 被真正的删除了，所以从输出结果中消失。

#### 属性名的语法
```javascript
var test = {
    'case': '这边的属性名是用引号引起来的',
    delete: '这边会报错,javascript解析错误，用了js自身的关键字' 出错：SyntaxError
};
```

原因:
> 对象的属性名可以使用字符串或者普通字符声明。但是由于 **JavaScript 解析器**的另一个错误设计， 上面的第二种声明方式在 `ECMAScript 5` 之前会抛出 **SyntaxError **的错误。

> 这个错误的原因是 `delete` 是 **JavaScript** 语言的一个关键词；因此为了在更低版本的 JavaScript 引擎下也能正常运行， 必须使用字符串字面值声明方式。

----

## 原型
JavaScript 不包含传统的类继承模型，而是使用 `prototype` 原型模型。

虽然这经常被当作是 JavaScript 的缺点被提及，其实基于原型的继承模型比传统的类继承还要强大。 实现传统的类继承模型是很简单，但是实现 JavaScript 中的原型继承则要困难的多。 (It is for example fairly trivial to build a classic model on top of it, while the other way around is a far more difficult task.)

由于 JavaScript 是唯一一个被广泛使用的基于原型继承的语言，所以理解两种继承模式的差异是需要一定时间的。
第一个不同之处在于 JavaScript 使用原型链的继承方式。
```javascript
function user(){//定义了一个user构造函数（也可以理解为类）
    this.age = 24;
}
user.prototype = {//给这个构造函数添加原型公共属性，不属于某个类的实例，而是直接属于某个类
    method: function(){}
};

function kevinli(){}//新建一个需要继承原型的构造函数

//设置kevinli的prototype属性指向user的实例对象
kevinli.prototype = new user();//user是实例对象
kevinli.prototype.sex = '男';

var tom = new kevinli();//创建kevinli的一个新实例

//原型链
tom > [kevinli的实例]
    kevinli.prototype > [user的实例]
    { sex: '男' }
    user.prototype
        { method: function(){} }
        object.prototype
            {toString: ... /* etc. */};

//
```
> 注意: 简单的使用 `kevinli.prototype = user.prototype`将会导致两个对象共享相同的原型。 因此，改变任意一个对象的原型都会影响到另一个对象的原型，在大多数情况下这不是希望的结果。

> 注意: 不要使用 `kevinli.prototype = user`，因为这不会执行 `user` 的原型，而是指向函数 `user`。 因此原型链将会回溯到 `Function.prototype` 而不是 `user.prototype`，因此 `method` 将不会在 `kevinli` 的原型链上。

上面的例子中，`tom` 对象从 `kevinli.prototype` 和 `user.prototype` 继承下来；因此， 它能访问 `user` 的原型方法 `method`。同时，它也能够访问那个定义在原型上的 `user` 实例属性 `age`。 需要注意的是 `new kevinli()` 不会创造出一个新的 `user` 实例，而是 重复使用它原型上的那个实例；因此，所有的 `kevinli` 实例都会共享相同的 `age` 属性。

### 属性查找
当查找一个对象的属性时，JavaScript 会向上遍历原型链，直到找到给定名称的属性为止。

到查找到达原型链的顶部 - 也就是 `Object.prototype` - 但是仍然没有找到指定的属性，就会返回 **undefined**。

### 原型属性
当原型属性用来创建原型链时，可以把任何类型的值赋给它（prototype）。 然而将原子类型赋给 prototype 的操作将会被忽略
```js
function kevinli() {}
kevinli.prototype = 1; // 无效
```
而将对象赋值给 prototype，正如上面的例子所示，将会动态的创建原型链。

### 性能
如果一个属性在原型链的上端，则对于查找时间将带来不利影响。特别的，试图获取一个不存在的属性将会遍历整个原型链

并且，当使用 `for in` 循环遍历对象的属性时，原型链上的所有属性都将被访问。

### 扩展内置类型的原型
一个错误特性被经常使用，那就是扩展 Object.prototype 或者其他内置类型的原型对象。

这种技术被称之为 **monkey patching** 并且会破坏封装。虽然它被广泛的应用到一些 JavaScript 类库中比如 **Prototype**, 但是我仍然不认为为内置类型添加一些非标准的函数是个好主意。

扩展内置类型的唯一理由是为了和新的 JavaScript 保持一致，比如 `Array.forEach`。

### 总结
在写复杂的 JavaScript 应用之前，充分理解原型链继承的工作方式是每个 JavaScript 程序员必修的功课。 要提防原型链过长带来的性能问题，并知道如何通过缩短原型链来提高性能。 更进一步，绝对不要扩展内置类型的原型，除非是为了和新的 JavaScript 引擎兼容。

----
## `hasOwnProperty` 函数
为了判断一个对象是否包含自定义属性而不是**原型链**上的属性， 我们需要使用继承自 `Object.prototype` 的 `hasOwnProperty` 方法。

`hasOwnProperty` 是 JavaScript 中唯一一个处理属性但是不查找原型链的函数。
```js
// 修改Object.prototype
Object.prototype.bar = 1;
var kevinli = {name: undefined};

kevinli.bar; // 1 任何对象都继承于最高级别的object对象
'bar' in kevinli; // true  “ in ”操作符用来判断某个属性属于某个对象，可以是对象的直接属性，
// 也可以是通过prototype继承的属性

kevinli.hasOwnProperty('bar'); // false hasOwnProperty不包含原型链的属性 所以返回flase
kevinli.hasOwnProperty('name'); // true  这是kevinli的自定义属性
```
只有 `hasOwnProperty` 可以给出正确和期望的结果，这在遍历对象的属性时会很有用。 没有其它方法可以用来排除原型链上的属性，而不是定义在对象自身上的属性。

### `hasOwnProperty` 作为属性
JavaScript 不会保护 `hasOwnProperty` 被非法占用，因此如果一个对象碰巧存在这个属性， 就需要使用外部的 `hasOwnProperty` 函数来获取正确的结果。

```js
var kevinli = {
    hasOwnProperty = function(){
        return false;
    },
    age : '24'
}

kevinli.hasOwnProperty('age');// 总是返回 false

// 使用其它对象的 hasOwnProperty，并将其上下文设置为foo
({}).hasOwnProperty.call(kevinli, 'age'); // true
```

### 结论
当检查对象上某个属性是否存在时，`hasOwnProperty` 是唯一可用的方法。 同时在使用 **for in loop** 遍历对象时，推荐总是使用 `hasOwnProperty` 方法， 这将会避免原型对象扩展带来的干扰。

----

### `for in` 循环
和 `in` 操作符一样，`for in` 循环同样在查找对象属性时遍历原型链上的所有属性。

```js
// 修改 Object.prototype
Object.prototype.age = 24;

var kevinli = {sex: '男'};
for(var i in kevinli) {
    console.log(i); // 输出kevinli的两个属性：age 和 sex
}
```

> 注意: `for in` 循环不会遍历那些 `enumerable` 设置为 `false` 的属性；比如数组的 `length` 属性。

由于不可能改变 `for in` 自身的行为，因此有必要过滤出那些不希望出现在循环体中的属性， 这可以通过 `Object.prototype` 原型上的 `hasOwnProperty` 函数来完成。

#### 使用 `hasOwnProperty` 过滤
```js
Object.prototype.age = 24;
var kevinli = {sex: '男'};
for(var i in kevinli){
    if(kevini.hasOwnProperty(i)){//过滤掉object原型链的属性
        console.log(i) //sex
    }
}
```
> 注意: 由于 `for in` 总是要遍历整个原型链，因此如果一个对象的继承层次太深的话会影响性能。

这个版本的代码是唯一正确的写法。由于我们使用了 `hasOwnProperty`，所以这次只输出 `sex`。 如果不使用 `hasOwnProperty`，则这段代码在原生对象原型（比如 `Object.prototype`）被扩展时可能会出错。

一个广泛使用的类库 **Prototype** 就扩展了原生的 JavaScript 对象。 因此，当这个类库被包含在页面中时，不使用 `hasOwnProperty` 过滤的 `for in` 循环难免会出问题。

#### 总结
推荐总是使用 `hasOwnProperty`。不要对代码运行的环境做任何假设，不要假设原生对象是否已经被扩展了。

----

## 2.函数
—————
### 函数声明与表达式
函数是JavaScript中的一等对象，这意味着可以把函数像其它值一样传递。 一个常见的用法是把匿名函数作为回调函数传递到异步函数中

#### 函数声明
```js
function userName(){}
```
上面的方法会在执行前被 **解析(hoisted)**，因此它存在于当前上下文的任意一个地方， 即使在函数定义体的上面被调用也是对的。
```js
userName();// 正常运行，因为userName在代码运行前已经被创建
function userName(){}
```
#### 函数赋值表达式
```js
    var userName = function(){};
```
这个例子把一个匿名的函数赋值给变量 'userName';
```js
userName; // 'undefined'
userName(); // 出错：TypeError
var userName = function() {};
```

程序执行顺序是从上往下执行的。
由于 `var` 定义了一个声明语句，对变量 `userName` 的解析是在代码运行之前，因此 `userName` 变量在代码运行时已经被定义过了。

但是由于赋值语句只在运行时执行，因此在相应代码执行之前， `userName` 的值缺省为 `undefined`。

#### 命名函数的赋值表达式
另外一个特殊的情况是将命名函数赋值给一个变量。
var kevinli = function userName() {
    userName(); // 正常运行
}
userName(); // 出错：ReferenceError

`userName` 函数声明外是不可见的，这是因为我们已经把函数赋值给了 foo； 然而在 `userName` 内部依然可见。这是由于 JavaScript 的 **命名处理** 所致， 函数名在函数内总是可见的。

----
### `this` 的工作原理
JavaScript 有一套完全不同于其它语言的对 `this` 的处理机制。 在五种不同的情况下 ，`this` 指向的各不相同。

#### 全局范围内
```js
    this;
```
当在全部范围内使用 `this`，它将会指向全局对象。
> 注意：浏览器中运行的 JavaScript 脚本，这个全局对象是 `window`。

#### 函数调用
```js
    userName();
```
这里 `this` 也会指向全局对象。

#### 方法调用
```js
    kevinli.userName();
```
这个例子中，`this` 指向 `kevinli` 对象。
> ES5 注意: 在严格模式下（strict mode），不存在全局变量。 这种情况下 this 将会是 **undefined**。

#### 调用构造函数
```js
    new userName();
```
如果函数倾向于和 `new` 关键词一块使用，则我们称这个函数是 **构造函数**。 在函数内部，`this` 指向新创建的对象。

#### 显式的设置 `this`
```js
    function userName(a,b,c){}

    var kevinli = {};
    userName.apply(kevinli,[1,2,3]);// 数组将会被扩展，如下所示
    userName.call(kevinli,1,2,3);// 传递到userName的参数是：a = 1, b = 2, c = 3

```
当使用 `Function.prototype` 上的 `call` 或者 `apply` 方法时，函数内的 `this` 将会被 显式设置为函数调用的第一个参数。

因此函数调用的规则在上例中已经不适用了，在`userName` 函数内 `this` 被设置成了 `kevinli`。


#### 常见误解
尽管大部分的情况都说的过去，不过第一个规则（译者注：这里指的应该是第二个规则，也就是直接调用函数时，`this` 指向全局对象） 被认为是JavaScript语言另一个错误设计的地方，因为它从来就没有实际的用途。
```js
kevinli.method = function() {
    function test() {
        // this 将会被设置为全局对象（译者注：浏览器环境中也就是 window 对象）
    }
    test();
}
```
一个常见的误解是 `test` 中的 `this` 将会指向 `kevinli` 对象，实际上不是这样子的。

为了在 `test` 中获取对 `kevinli` 对象的引用，我们需要在 method 函数内部创建一个局部变量指向 `kevinli` 对象。

kevinli.

