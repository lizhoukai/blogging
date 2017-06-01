---
title: JS查找数组中是否存在某个元素
date: 2016-04-12
tags: JavaScript
---

## 1.函数方法定义
```js
function contains(arr, obj) {
    var i = arr.length;
    while (i--) {
        if (arr[i] === obj) {
            return true;
        }
    }
    return false;
}
```

```js
var arr = new Array(1, 2, 3);
contains(arr, 2);//返回true
contains(arr, 4);//返回false
```

## 2.定义原型
```js
Array.prototype.contains = function (obj) {
    var i = this.length;
    while (i--) {
        if (this[i] === obj) {
            return true;
        }
    }
    return false;
}
```
```js
[1, 2, 3].contains(2); //返回true
[1, 2, 3].contains('2'); //返回false
```


