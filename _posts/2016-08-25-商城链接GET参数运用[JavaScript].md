---
title: 商城链接GET参数运用
date: 2016-08-25
tags: JavaScript
---

![2016082523067mall.gif](https://ohv0hyr4v.qnssl.com/2016082523067mall.gif)

<!-- more -->

# 需求分析
商城页的分页组件和顶部的TAG选项的筛选条件。

当分页器默认点击所有商品页下一页的时候，`url`后会添加一个`page=2`翻页。然后点击 **TAG** 的时候，`url`也随应也加上参数`categories=1`

现在遇到的问题:
> 1.如果当前**TAG**筛选的商品比较少，只够展示一页的数据，那么当前`page=2`，是不是应该只展示第一页的数据。
> 2.每次点击翻页，分页器都会**JS**重新获取 `window.location.search` **URl ？**号后面的`get`参数，但是分页器翻页的时候，下一页比如`page=3`，当前 **URL** 的也包含了上一次的`page=2`，这时候要过滤之前的参数。


# 思路
`window.location.search`获取当前**URL**后面的`get`参数，然后转换成**JS对象**，列如：

```js
{
	page:1,
	categories:[
		"1",
		"2"
	],
	producer:[
		"2",
		"3"
	]
}
```
然后对这个**对象**进行参数修改
接着再把这个对象转换成`url`中的`get`字符串，成为一个新的链接地址

# 需要的知识点
对象转JSON，JSON转对象：

```js
var a={"name":"tom","sex":"男","age":"24"};
var b='{"name":"Mike","sex":"女","age":"29"}';

var aToStr=JSON.stringify(a);
var bToObj=JSON.parse(b);

alert(typeof(aToStr));  //string
alert(typeof(bToObj));//object
```

分页取余：
`Math.random()`
**random()**
返回 0 ~ 1 之间的随机数。

`Math.ceil()`
**ceil()** 方法可对一个数进行上舍入。
参数必须是一个数值。返回值大于等于 x，并且与它最接近的整数。

`Math.floor()`
**floor()** 方法可对一个数进行下舍入。
参数可以是任意数值或表达式。返回值小于等于 x，且与 x 最接近的整数。

`Math.round()`
**round()** 方法可把一个数字舍入为最接近的整数


# 实践

```js
var getArgs = function() {
  var tmpArray = {};
  var currentArgsStr = window.location.search;
  var tmp, tmpKey, tmpVal;
  if (!currentArgsStr) {
    return {};
  }

  currentArgsStr = currentArgsStr.replace('?', '');
  $.each(currentArgsStr.split("&"), function(index, ele) {
    var array = [];
    tmp = ele.split("=");
    tmpKey = tmp[0];
    tmpVal = tmp[1];
    if (tmpArray[tmpKey]) {
      tmpArray[tmpKey].push(tmpVal);
    } else {
      array.push(tmpVal);
      tmpArray[tmpKey] = array;
    }
  });
  return tmpArray;
}
```

