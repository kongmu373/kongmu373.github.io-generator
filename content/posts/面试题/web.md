---
title: "Web"
date: 2020-07-07T14:54:54+08:00
draft: true
tags: ["面试题"]
series: ["面试题"]
categories: ["面试题"]
---

## 常见的HTTP状态码
+ 2XX 一切正常
+ 3XX 代表跳转
+ 4XX 代表客户端异常
+ 5XX 代表服务端异常
[常用状态码](https://http.cat/)
+ 200/201 created / 301 永久/ 302 临时
+ 403/403 , 500/502/503/504

## get/post的区别?
+ get获取资源，而post用于发送数据
+ get参数在请求字符串里，post参数放在request body中
+ GET: 只有header
+ POST: 把数据放在body
+ get是幂等的(多次发送和一次发送的结果完全相同),post不一定

## Cookie和Session有什么区别?
+ Cookie是服务器写入浏览器的一小段信息，只有同源的网页才能共享
+ Session: 放在服务端的，用于通过Cookie和用户身份对应关系来鉴权的数据

## 什么是跨域,怎么解决?
+ 同源策略：同一个域名包括协议以及端口号,才能携带对应的Cookie
  + 目的是为了保证用户信息的安全，防止恶意的网站窃取数据。
+ 解决方法:
  + jsonp
    + 它的基本思想是，网页通过添加一个<script>元素
  + CORS
    + ![跨域资源共享 CORS 详解](https://www.ruanyifeng.com/blog/2016/04/cors.html)
  + nginx反向代理
