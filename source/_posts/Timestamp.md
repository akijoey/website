---
title: Timestamp
date: 2019-08-04
tags: Javascript
categories: Frontend
keywords: Time Stamp
description: Time Stamp
cover: /img/cover_posts/javascript.jpg
top_img: /img/cover_posts/javascript.jpg
---
## Timestamp

时间戳 (Timestamp) 是指格林威治时间 (GMT) 1970 年 01 月 01 日 00 时 00 分 00 秒 (北京时间 1970 年 01 月 01 日 08 时 00 分 00 秒) 起至现在的总毫秒数.

## Date to Timestamp

日期格式转时间戳.
先将日期格式转为 Date 对象, 再将 Date 对象转为时间戳.
下面三种写法都能获取当前时间戳.

``` javascript
var timestamp = new Date().getTime();
var timestamp = (new Date()).valueOf();
var timestamp = Date.parse(new Date()); //只能精确到秒
```

## Timestamp to Date

时间戳转日期格式.
先将时间戳转为 Date 对象, 再将 Date 对象转为日期格式.
要转为 GMT + 8 (北京时间) 的日期格式, 需要加上 8 * 3600 s.
时间戳的单位是毫秒, 因此需要 * 1000.

``` javascript
var timestamp = new Date().getTime();
var date = new Date(timestamp + 8 * 3600 * 1000).toJSON().substr(0, 19).replace('T', ' ');
```
