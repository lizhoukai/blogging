- `window`是js中的”老大“
- `this`： 指的是调用 当前 方法（函数）的那个对象

默认函数指向的是 `window`
如果对象在执行`语句块内`或`作用域内`用到`this`关键字，指向的是当前对象
但如果执行了匿名函数内执行了另外一个函数内的`this`，指向的是`window`

<!-- more -->

```js
function fn1(){
    alert(this);
}
fn1();// [object Window]

//上述代码中执行函数fn1()相当于window.fn1();因此，在函数体内弹出这个this是window。
```

```js
function fn1(){
    alert(this);
}
oBtn.onclick = fn1; // [object HTMLInputElement]

//上述代码，点击oBtn按钮，弹出的就不再是oBtn这个对象，而是window。
```

```html
<input id="btn2" type="button" value="按钮2" onclick="alert(this)" />// [object HTMLInputElement]

//上述代码是行间事件，onclick的作用域在 " "之间， 弹出的this就是这个按钮btn2。
```

```js
function fn1(){
    alert(this);
}
<input id="btn2" type="button" onclick="fn1();" value="按钮2” /> // [object Window]

//上述代码中，弹出的this就是window。
```

> this: 调用当前方法（函数）的那个对象案例：

```js
function fn1(){
    this
}

fn1(); //this ＝> window

oDiv.onclick = fn1; //this => oDiv

oDiv.onclick = function(){
    this // => oDiv
};

oDiv.onclick = function(){
    fn1(); // fn1()里面的this => window
}

<div onclick="  this  "></div> //this => div

<div onclick = "  fn1();  "></div>  //fn1()里面的this => window
```

```js
fn1(this);
function fn1(obj){
  obj //=> window
}

oDiv.onclick = function(){
  this
  fn1(this);//当前对象参数传递
};

function fn1(obj){ obj //=> oDiv }
```

```js
<input type="button" value="按钮1" />
<input type="button" value="按钮2" />
<input type="button" value="按钮3" />

var aBtn = document.getElementsByTagName('input');
for(var i=0; i<aBtn.length; i++) {
  aBtn[i].onclick = function(){
    this.style.background = 'yellow';
  }
}
```

  上述代码的作用与下面这段代码的作用等价，注意that的使用方法。

```js
<input type="button" value="按钮1" />
<input type="button" value="按钮2" />
<input type="button" value="按钮3" />

var aBtn = document.getElementsByTagName('input');
var that = null;

for(var i=0; i<aBtn.length; i++) {
  aBtn[i].onclick = function(){
    that = this;
    fn1();
  };
}

function fn1(){
  //this => window
  that.style.background = 'yellow';
}
```
  上述代码还等价于下面这段代码：

```js
<input type="button" value="按钮1" />
<input type="button" value="按钮2" />
<input type="button" value="按钮3" />

var aBtn = document.getElementsByTagName('input');

for(var i=0; i<a.aBtn.length; i++) {
  aBtn[i].onclick = fn1; //注意不要在fn1后面加括号
}

function fn1(){
  this.style.background = 'yellow';
}
```

`DEMO：`
- [鼠标经过弹出提示框](http://sandbox.runjs.cn/show/39i8mast)
- [奇偶横竖排判断](view-source:http://sandbox.runjs.cn/show/j4fajpjx)
- [选项卡交叉同步走](http://sandbox.runjs.cn/show/w2k1b77g)


