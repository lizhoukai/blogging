---
title: JS控制cookie，选中复选框下次不再提醒
date: 2016-01-08
tags: [JavaScript]
---

应用场景：
每次按钮点击弹出一个对话框，选中复选框下次不再提醒：

解决思路：
获取需要点击的DOM节点为`checkbox`，判断`checkbox`选中后是否为`true`,
当为`true`,当前内容存入一个`cookie`值，
用`if`语句判断，如果下次有`cookie`值，不再提示，否则反之。

html代码：
```html
    <label>
        <input id="result_checkbox" type="checkbox" name="" value="按钮" placeholder="">下次不提醒我
    </label>
    <div id="create_result">刷新在看看</div>
```

JS代码：
```javascript
    //设置cookie
    function setCookie(name, value) {
        var exp = new Date();
        exp.setTime(exp.getTime() + 1 * 60 * 60 * 1000); //有效期1小时
        document.cookie = name + "=" + escape(value) + ";expires=" + exp.toGMTString();
    }
    //取cookies函数
    function getCookie(name) {
        var arr = document.cookie.match(new RegExp("(^| )" + name + "=([^;]*)(;|$)"));
        if (arr != null) return unescape(arr[2]);
        return null;
    }

    $(function() {
        var checkbox = document.getElementById('result_checkbox');

        if (getCookie("closepop") == 0) {
            $("#create_result").hide();
        } else {
            $("#create_result").show();
        }

        $('#result_checkbox').click(function() {
            if (checkbox.checked) {
                setCookie("closepop","0")
                console.log('选中')
            } else {
                console.log('没选中')
            }
        })
    })
```


