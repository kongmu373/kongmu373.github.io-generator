---
title: "浅析URL"
date: 2020-06-14T08:30:32+08:00
draft: true
tags: ["HTML"]
series: ["HTML"]
categories: ["HTML"]
---

> WWW = URL + HTTP + HTML，本篇主要讲的是URL

URL举例:
https://www.baidu.com:443/s?wd=hi&rsv_spt=1#5
+ https 协议
+ www.baidu.com 域名(ip)
+ :443 https的端口 
+ /s 路径
+ ?wd=hi&rsv_spt=1 查询参数
+ #5 锚点 

## 什么是IP
+ Internet Protocal
+ 约定了两件事:
  1. 如何定位一台设备
  2. 如何封装数据报文，以跟其他设备交流 
+ 几个特殊的IP
  + 127.0.0.1表示自己
  + localhost通过hosts文件指定为自己
  + 0.0.0.0不表示任何设备

## 什么是端口(port)?
+ 每个服务一个号码，这个号码就叫端口号 port
+ 为什么需要端口？
  + 一台机器可以提供不同的服务
  + HTTP -> 80 port
  + HTTPS -> 443 port
  + FTP -> 21 port
  + 一台机器一共有65535个端口
+ 端口的使用规则
  + 0-1023 号端口留给系统用
  + 一个端口如果被占用，就只能换一个端口

## 什么是域名?
+ 域名就是对IP的别称
+ 一个域名可以对应不同IP(负载均衡)
+ 一个ip可以对应不同的域名(共享主机)

## DNS(将域名和IP对应起来)
+ 例子(过程)
  + 在浏览器输入baidu.com会向电信/联通提供的DNS服务器询问baidu.com对应什么IP
  + 等DNS服务器返回IP,浏览器才会相对应IP的80/443端口发送请求，得到baidu.com的首页内容

## 路径
+ 在同一个服务器上使用不同的路径就得到不同的页面
+ https://developer.mozilla.org/zh-CN/docs/Web/HTML
+ https://developer.mozilla.org/zh-CN/docs/Web/CSS

## 查询参数
+ 同一个页面，不同的内容
+ www.baidu.com/s?wd=hi
+ www.baidu.com/s?wd=hello

## 锚点
+ 同一个内容的不同位置
+ https://developer.mozilla.org/zh-CN/docs/Web/CSS#参考书
+ https://developer.mozilla.org/zh-CN/docs/Web/CSS#教程
+ 锚点不会传给服务器，锚点只跟浏览器有关

## 注意
+ URL中看起来支持中文，实际不支持中文
+ 中文变成%E5%..

## URL
+ Uniform Resource Locator
+ 协议+域名(ip)+端口+路径+查询参数+锚点

## curl 命令
+ 用curl 可以发送http请求
+ curl -v http://baidu.com·