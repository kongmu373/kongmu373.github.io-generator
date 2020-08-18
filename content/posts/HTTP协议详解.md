---
title: "HTTP协议"
date: 2020-05-28T14:28:21+08:00
draft: true
tags: ["HTTP"]
series: ["HTTP"]
categories: ["Java体系"]
---
# HTTP协议

> 互联网所有东西都是基于HTTP协议


## HTTP 方法
+ GET 和 POST 的区别
  + GET 所有数据全部都放在 HTTP Request header 里面
  + POST 真正的数据放在 HTTP Request body 里面 (登录，上传文件)
  + DELETE
  + PUT
  + ...

## HTTP 状态码
+ 2XX 成功
+ 3XX 重定向
+ 4XX 客户端出问题
  + 405 客户端请求的方法被禁止
+ 5XX  
[HTTP状态码的博客](https://http.cat/)

## HTTP 请求header
+ 重要的header
  + Accept*
  + Cookie
  + User-Agent
  + Referer 当前页面是从哪里跳转过来的(正确拼写Referrer)防盗链
  + Content-type 

## HTTP响应header
+ 重要的hedaer
  + Content-type
  + Set-Cookie

## Accept -- Content-type
+ 一对经常使用的header
+ accept:application/json,application/xml...
  + 请求的是json,xml数据,...列举所能接受的数据
   
+ Content-type
  + application/zip, /img 浏览器默认下载文件
  + application/json, application/xml 返回数据
  + 不但能用在响应，还能用在请求

## HTTP body
+ HTTP request body
  + x-www-form-urlencoded
  + form-data
  + raw
    + 添加Header Content-Type:...
  + binary
    + 传文件
  + k-v对
+ HTTP response body
  + JSON
  + HTML/XML
  + 二进制(图片/下载文件)

## Cookie
> HTTP协议是无状态的

+ 怎么样维持登录状态？
  + 使用Cookie
    + 登录成功时，添加Response的Header为,"set-Cookie:XXXX"
    + 然后每次客户端向服务器发送请求，Request的Header都会有"Cookie:XXX"
  + cookie只对当前域名有效
  + cookie是很重要的凭证，不能泄露，黑客攻击也多数针对cookie