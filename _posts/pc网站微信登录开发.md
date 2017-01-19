title: pc网站微信登录开发
date: 2015-11-25 15:46:27
tags: [微信开发,JavaScript]
---

网站需要接入一个微信登录的功能。于是知乎了下，了解了下原理

（1）每打开一次微信网页版页面的时候会随机生成一个含有唯一`uuid`的二维码，每次刷新页面都会不一样（这个可以保证一个 `uuid`只可以绑定一个账号和密码，如果一个`uuid`可以绑定多个账号和密码，那么很可能你的电脑会登陆别人的微信）；

确实返回了唯一 id，但目的是为了识别用户身份，而且实际上打开这个页面的时候浏览器已经和 Server 创建了一个长连接等待确认信息。查看 https://open.weixin.qq.com/connect/qrconnect?appid=wx4b58644b2d59e8c9&redirect_uri=http%3A%2F%2Fapi.date.tm&response_type=code&scope=snsapi_login&state=STATE#wechat_redirect 的源码可以看到，这个页面在加载完毕时，也已经把很多登录后才需要的相关资源都预先加载进来了，所以长连接等待登录用户得到确认后展示用户信息的速度很快，因为无需刷页面和加载头像外的其他资源。

<!-- more -->
![微信图片](https://ohv0hyr4v.qnssl.com/weixin.png)

（2）当用户使用登陆后的微信扫描该二维码的时候，会将这个id和手机上的微信账号及密码绑定，并上传到微信网页版服务器；

- 1.在浏览器生成二维码，二维码中包含登录信息和服务端给它生成了一个唯一标识码UUID，同时服务端监听服务端登录请求；
- 2.在客户端使用扫一扫登录网页版时，此时uid已经登录且有访问授权码access_token信息
- 3.扫描网页的二维码，获取到服务器生成的UUID，然后将access_token及UUID发送给服务端
服务端验证通过后，生成登录授权码并且通知网页端
- 4.网页端获得授权码后即可向服务器申请用户登录信息，完成登录

**具体流程:**

![登录](https://res.wx.qq.com/open/zh_CN/htmledition/res/img/pic/web-wxlogin/12168b9.png)

**官网提供的URL：**
```html
https://open.weixin.qq.com/connect/qrconnect?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=snsapi_login&state=STATE#wechat_redirect
```



`appid`：应用唯一标识
`redirect_uri` ：重定向地址，需要进行UrlEncode
`response_type`：填code
`scope        `：应用授权作用域，拥有多个作用域用逗号（,）分隔，网页应用目前仅填写snsapi_login即可
`state`        ：用于保持请求和回调的状态，授权请求后原样带回给第三方。该参数可用于防止csrf攻击（跨站请求伪造攻击），建议第三方带上该参数，可设置为简单的随机数加session进行校验

注意：**redirect_uri**（重定向地址必须开发者信息审核通过的网站URL）

**Demo:**
```js
var redirect  =  'http://api.date.tm';
var RedirectUrlEncode = encodeURIComponent(redirect);
var wecaht_entryUrl = 'https://open.weixin.qq.com/connect/qrconnect?appid=wx4b58644b2d59e8c9&redirect_uri='+ RedirectUrlEncode +'&response_type=code&scope=snsapi_login&state=STATE#wechat_redirect'
```




