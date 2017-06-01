---
title: new Date()格式问题
date: 2015-10-25
tags: JavaScript
---

最近遇到一个问题，查不到原因。卡在那里，所幸在`stackoverflow`找到了答案；
后端提供的接口，json其中一条格式是这样的 `"start_date": "2015-10-10 19:11"`

而然我从发送请求拉取json数据的时候，前台有这么一个事件是通过new Date()填充数据。
格式如下：
```js
        scheduler.addEvent({
            start_date: new Date(dataList.events[i].start_date), //事件开始时间
            end_date: new Date(allDayEndDate),//事件结束时间
            text: dataList.events[i].title, //日历事件主题内容
            eid: dataList.events[i].eid, //eid
            start_time:dataList.events[i].day_num //事件开始时间
        });
```
这边的`start_date` 和 `end_date`的格式都是 **2015-10-10 19:11**
没错，写完在我的chrome上数据显示正常。
<!-- more -->
然后我就开始测浏览器兼容性问题，火狐，ie8，360,都不能显示。
但我仔细看了下，如果有的事件格式为`"start_date": "2015-10-10"` ie，火狐都正确显示了。
那问题应该就是日期格式了！！！

然后我就测试
```js
var time = new Date("2010-08-17 12:09:36");
alert(time)//Invalid date
```
测试的结果是**无效的日期？？**

于是我找到了答案，显然这种日期格式不适用于所有浏览器。
如果想适用于所有浏览器，
格式应该为：
```javascript
new Date("2010/08/17 12:09:36");
```

于是就改了下如下：
```js
scheduler.addEvent({
    start_date: new Date(dataList.events[i].start_date.replace(/-/g, "/")), //事件开始时间
    end_date: new Date(allDayEndDate.replace(/-/g, "/")),//事件结束时间
    text: dataList.events[i].title, //日历事件主题内容
    eid: dataList.events[i].eid, //eid
    start_time:dataList.events[i].day_num //事件开始时间
});
```
`replace()`函数：用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。
