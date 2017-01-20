## 自定义属性的读写操作
```js
var  oBtn = document.getElementByTagName('input');

oBtn[0].abc = 123;
console.log(oBtn[0].abc);// 自定义属性的读

oBtn[0].abc = 456;// 自定义属性的写
//JS可以为任何HTML元素添加任意个自定义属性
```

## 自定义属性应用
> 用自定义属性可以元素添加开关属性，作为控制开关。如下面这个例子：

```js
var aLi = document.getElementsByTagName('li');
// var onOff = true; 一个onOff只能控制一个开关。要为多个元素设置各自的开关，就要给每个元素加个onOff开关。

for(var i=0; i<aLi.length; i++) {
    aLi[i].onOff = true;
    aLi[i].onclick = function(){
      if (this.onOff){
        this.style.background = 'url(img/active.png)';
        this.onOff = false;
      } else {
        this.style.background = 'url(img/normal.png)';
        this.onOff = true;
      }
    }
}
```

<!-- more -->

----

## 获取自身递增数字及匹配数组内容
> 在网页上设置三个按钮，默认值为0，每点击一次按钮，字母依次ABCD轮换:

```html
<input type="button" value="0" />
<input type="button" value="0" />
<input type="button" value="0" />
```

```js
var arr = ['a','b','c','d'];
var num = 0;
var oBtn = document.getElementsByTagName('input');

for(var i=0;i<oBtn.length;i++){
    oBtn[i].num = 0;
    oBtn[i].onclick = function(){
        this.value = arr[this.num];
        this.num ++;
        if(this.num == arr.length){
            this.num = 0;
        }
    };
}

```

## 添加索引值、匹配数组、HTML元素
> 把三个按钮的value与数组里面的值依次进行匹配。

```html
<input type="button" value="btn1" />
<input type="button" value="btn2" />
<input type="button" value="btn3" />
```

```js
var aBtn = document.getElementsByTagName('input');
var arr = ['莫涛','张森','杜鹏'];

for(var i=0; i<aBtn.length; i++){
    aBtn[i].index = i; //自定义属性index，添加索引值
    aBtn[i].onclick = function(){
      //alert(i); //不会弹出1、2，直接弹出3，也就是aBtn的长度。也就是说，在基础阶段，在for循环所包的函数里面再用i是不靠谱的。
      this.value = arr[this.index];
    };
};
```

##
> 总结：要控制多个相同的`DOM`节点并处理相同的事情，这时候可以考虑用`自定义属性`
> 上面是通过索引值匹配数组值，匹配HTML中的元素也是同理。
> 想建立”匹配“、”对应“关系，就用索引值。


`DEMO：`

- [QQ列表展开收缩扩展(单选)](http://sandbox.runjs.cn/show/rgmjl8nn)
- [QQ列表展开收缩扩展(多选)](http://sandbox.runjs.cn/show/o1lfksh5)
- [带缩略图的图片切换效果](http://sandbox.runjs.cn/show/8hhrz4dy)
- [控制多组图片切换](http://sandbox.runjs.cn/show/csqcitr1)


