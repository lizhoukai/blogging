title: '使用手淘[Flexible]实现h5页面的终端适配'
date: 2015-12-05 21:41:28
tags: [移动端,适配解决方案]
---
双11已过，手淘前端团队大漠老湿分享的一个基于手淘html5终端适配的移动端解决方案；
之前买过大漠老湿写的书`图解CSS3`，写的很用心，很棒 😄

喜欢移动端，是因为可以大胆的尝试一些新的写法，搭配各种不一样的框架，不用在考虑为向下兼容ie8啊 为了这些操蛋的写多余的代码，而一直用不到一些便捷新奇的方法属性，但是坑是挺多的。

<!-- more -->

**先来看看移动需要适配的各种尺寸：**
![适配图片参数](https://camo.githubusercontent.com/9598a107e7f7029717f52192c90dcaf7008e49c1/687474703a2f2f7777772e773363706c75732e636f6d2f73697465732f64656661756c742f66696c65732f626c6f67732f323031352f313531312f72656d2d342e706e67)

在来看看淘宝需要适配的终端设备数据：
![数据](https://camo.githubusercontent.com/ab4450a21060ca291fc6b7ddc9592c94467d6bd6/687474703a2f2f7777772e773363706c75732e636f6d2f73697465732f64656661756c742f66696c65732f626c6f67732f323031352f313531312f72656d2d372e706e67)

- -，-还好苹果垄断了大半江山，各种杂七杂八的机型，就不说，各种原生改过的系统，可想而知兼容性多差了~如为了适配全部手机，那得多累，大致适配一些主流，不冷门的手机，不过大多基于`webkit`内核的兼容性都还可以。


## 1.淘宝的适配协作流程
- 选择一种尺寸作为设计和开发基准
- 定义一套适配规则，自动适配剩下的两种尺寸(其实不仅这两种，你懂的)
- 特殊适配效果给出设计效果

![流程](https://camo.githubusercontent.com/8e69ed933a0eff873d4a2b3667461d1e3ec2d790/687474703a2f2f7777772e773363706c75732e636f6d2f73697465732f64656661756c742f66696c65732f626c6f67732f323031352f313531312f72656d2d362e6a7067)


## 2.基本概念
### 2.1视窗 viewport
简单的理解，**viewport**是严格等于浏览器的窗口。在桌面浏览器中，**viewport**就是浏览器窗口的宽度高度。但在移动端设备上就有点复杂。

移动端的**viewport**太窄，为了能更好为CSS布局服务，所以提供了两个**viewport**:虚拟的`viewportvisualviewport`和布局的`viewportlayoutviewport`。

### 2.2物理像素(physical pixel)
物理像素又被称为设备像素，他是显示设备中一个最微小的物理部件。每个像素可以根据操作系统设置自己的颜色和亮度。正是这些设备像素的微小距离欺骗了我们肉眼看到的图像效果。

### 2.3设备独立像素(density-independent pixel)
设备独立像素也称为密度无关像素，可以认为是计算机坐标系统中的一个点，这个点代表一个可以由程序使用的虚拟像素(比如说CSS像素)，然后由相关系统转换为物理像素。

### 2.4设备像素比(device pixel ratio)
设备像素比简称为dpr，其定义了物理像素和设备独立像素的对应关系。它的值可以按下面的公式计算得到：

    设备像素比 ＝ 物理像素 / 设备独立像素


在JavaScript中，可以通过`window.devicePixelRatio`获取到当前设备的**dpr**。而在CSS中，可以通过`-webkit-device-pixel-ratio，-webkit-min-device-pixel-ratio`和 `-webkit-max-device-pixel-ratio`进行媒体查询，对不同**dpr**的设备，做一些样式适配(这里只针对webkit内核的浏览器和webview)。


部分代码逻辑：
```js
if (!dpr && !scale) {
    var isAndroid = win.navigator.appVersion.match(/android/gi);
    var isIPhone = win.navigator.appVersion.match(/iphone/gi);
    var devicePixelRatio = win.devicePixelRatio;
    if (isIPhone) {
        // iOS下，对于2和3的屏，用2倍的方案，其余的用1倍方案
        if (devicePixelRatio >= 3 && (!dpr || dpr >= 3)) {
            dpr = 3;
        } else if (devicePixelRatio >= 2 && (!dpr || dpr >= 2)){
            dpr = 2;
        } else {
            dpr = 1;
        }
    } else {
        // 其他设备下，仍旧使用1倍的方案
        dpr = 1;
    }
    scale = 1 / dpr;
}
```

## 3.flexible的原理
flexible实际上就是能过JS来动态改写meta标签，代码类似这样：
```js
var metaEl = doc.createElement('meta');
var scale = isRetina ? 0.5:1;
metaEl.setAttribute('name', 'viewport');
metaEl.setAttribute('content', 'initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
if (docEl.firstElementChild) {
    document.documentElement.firstElementChild.appendChild(metaEl);
} else {
    var wrap = doc.createElement('div');
    wrap.appendChild(metaEl);
    documen.write(wrap.innerHTML);
}
```

**原理流程：**

- 动态改写<meta>标签
- 给<html>元素添加`data-dpr`属性，并且动态改写`data-dpr`的值
- 给<html>元素添加`font-size`属性，并且动态改写`font-size`的值

## 4.把视觉稿中的px转换成rem
`CSSREM`是一个CSS的px值转rem值的Sublime Text3自动完成插件
![cssrem](https://camo.githubusercontent.com/7bc50fa37be4ada5d263152a107125a216a6936c/687474703a2f2f7777772e773363706c75732e636f6d2f73697465732f64656661756c742f66696c65732f626c6f67732f323031352f313531312f63737372656d2e676966)

## 5.文本字号不建议使用rem
前面大家都见证了如何使用rem来完成H5适配。那么文本又将如何处理适配。是不是也通过rem来做自动适配。

显然，我们在iPhone3G和iPhone4的Retina屏下面，希望看到的文本字号是相同的。也就是说，我们不希望文本在Retina屏幕下变小，另外，我们希望在大屏手机上看到更多文本，以及，现在绝大多数的字体文件都自带一些点阵尺寸，通常是16px和24px，所以我们不希望出现13px和15px这样的奇葩尺寸。

如此一来，就决定了在制作H5的页面中，rem并不适合用到段落文本上。所以在Flexible整个适配方案中，考虑文本还是使用px作为单位。只不过使用[data-dpr]属性来区分不同dpr下的文本字号大小。
```css
div {
    width: 1rem;
    height: 0.4rem;
    font-size: 12px; // 默认写上dpr为1的fontSize
}
[data-dpr="2"] div {
    font-size: 24px;
}
[data-dpr="3"] div {
    font-size: 36px;
}
```

最终适配的效果图：
![gif](https://ohv0hyr4v.qnssl.com/2.gif)


