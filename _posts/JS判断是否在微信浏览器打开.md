title: 用JS判断是否在微信浏览器打开
date: 2015-10-25 22:31:06
tags: [JavaScript,微信开发]
---
最近做了挺多针对微信浏览器的H5页面开发。总结下：
思路：获取当前浏览器ua，再做判断
<!-- more -->
```js
    var browser = {
        versions: function () {
            var u = navigator.userAgent.toLowerCase();
            return {         //移动终端浏览器版本信息
                txt: u, // 浏览器版本信息
                version: (u.match(/.+(?:rv|it|ra|ie)[\/: ]([\d.]+)/) || [])[1], // 版本号
                msie: /msie/.test(u) && !/opera/.test(u), // IE内核
                mozilla: /mozilla/.test(u) && !/(compatible|webkit)/.test(u), // 火狐浏览器
                safari: /safari/.test(u) && !/chrome/.test(u), //是否为safair
                chrome: /chrome/.test(u), //是否为chrome
                opera: /opera/.test(u), //是否为oprea
                presto: u.indexOf('presto/') > -1, //opera内核
                webKit: u.indexOf('applewebkit/') > -1, //苹果、谷歌内核
                gecko: u.indexOf('gecko/') > -1 && u.indexOf('khtml') == -1, //火狐内核
                mobile: !!u.match(/applewebkit.*mobile.*/), //是否为移动终端
                ios: !!u.match(/\(i[^;]+;( u;)? cpu.+mac os x/), //ios终端
                android: u.indexOf('android') > -1, //android终端
                iPhone: u.indexOf('iphone') > -1, //是否为iPhone
                iPad: u.indexOf('ipad') > -1, //是否iPad
                webApp: !!u.match(/applewebkit.*mobile.*/) && u.indexOf('safari/') == -1 //是否web应该程序，没有头部与底部
            };
        }(),
        // language: (navigator.browserLanguage || navigator.language).toLowerCase()
    }

    if (browser.versions.mobile) {//判断是否是移动设备打开
        var ua = navigator.userAgent.toLowerCase();//获取判断用的对象
        if (ua.match(/MicroMessenger/i) == "micromessenger") {//微信端
                 window.location.href = 'http://m.amap.com/?mk=30.36244801,120.09799636,杭州市西湖区三墩镇三墩公平路';
        }else if (browser.versions.ios) {//ios终端
                window.location="iosamap://viewMap?sourceApplication=applicationName&poiname=杭州市西湖区三墩镇三墩公平路&lat=30.36244801&lon=120.09799636&dev=1";
        }else if (browser.versions.android) {//android终端
            window.location.href = 'http://m.amap.com/?mk=30.36244801,120.09799636,杭州市西湖区三墩镇三墩公平路';
        } else {
        //否则就是PC浏览器打开
        }
    }
```
