---
title: "Web前后端基础结构原理"
date: 2020-05-27T23:15:03+08:00
draft: false
tags: ["爬虫"]
series: ["HTTP"]
categories: ["Java体系"]
---

## 计算机网络的术语
+ 主机 
  + 每个主机会有一个地址(IPV4地址为32位，4个字节,如115.118.112.3 2^32=42亿个数字,不够用了) (网络，主机，IP地址(Internet Protocol -- 网络协议))
  + IPV6(128位，为地球的每一颗沙子分配地址)
+ 域名与DNS
  + 知道与哪台主机通信，即必须知道门牌号(IP地址,但是 ip地址难记)。
  + 因此我们需要DNS(Domain Name Service)域名服务,当输入taobao.com时，如果计算机不知道该主机在哪，就会向DNS发生请求查询IP地址
  + 如果本机的hosts文件有，就不用到DNS去查询
+ 端口 port
  + 区分不同应用程序的数据
  + 最多有65536个端口
  + 默认端口
    + HTTP协议的默认端口是80 (http://taobao.com 等价于 http://taobao.com:80)
    + HTTPS协议的默认端口是443 (https://taobao.com 等价于 https://taobao.com:443)
+ TCP协议 (Transport Control Protocol)
  + 定义了字节流在网络上如何发生和接收 
  + 双向高速公路(管道，基于流的协议)，跑的是数据
  + 全双工协议，两边可以同时发送数据
+ HTTP协议 (Hyper Text Transport Protocol 超文本传输协议)
  + 在TCP协议之上
  + 可以跑文本之外的数据
  + Http Request
  + Http Response
+ 面试题:浏览器敲了taobao.com按下回车键后中间发生了什么？
  + 首先分析下URL, taobao.com == https://www.taobao.com:443/ 的使用的是HTTPS协议 默认端口是443，域名是taobao.com,访问的是 /
  + 然后会先在本机查找对应的hosts文件，查找是否有对应的IP地址，如果没有就到DNS运行商去查找，一级一级往上查,直到找到对应的ip地址，返回给浏览器(或者域名错误，找不到)
  + 浏览器根据ip地址找到服务器后，与服务器建立双向高速通道，并发送请求request，然后服务器对request进行处理，最后将对应的资源打包起来通过通道返回浏览器。
  + 浏览器收到response后，对其进行加载，根据其中的代码(如 css,js,img等)继续向服务器请求资源
  + 最后对页面进行解释，生成解释树，并对其进行可视化，也叫做渲染，就可以看到taobao的主页了。

## 浏览器是如何工作的
+ 在网络上传输的只是字节流
+ HTTP协议
+ HTML
+ JavaScript
+ CSS

## 用Java发出第一个HTTP请求
1. 使用Java代码访问GitHub的issues
    + 选择一个合适的客户端
      + 如何快速上手使用自己从没用过的库?
    + 设置正确的HTTP header
    + 发送请求，等待响应
    + 解析拿到的响应
```java
public static void main(String[] args) throws IOException {
    CloseableHttpClient httpclient = HttpClients.createDefault();
    HttpGet httpGet = new HttpGet("https://github.com/gradle/gradle/issues");
    CloseableHttpResponse response1 = httpclient.execute(httpGet);
    try {
        // 解析response
        System.out.println(response1.getStatusLine());
        HttpEntity entity1 = response1.getEntity();
        InputStream content = entity1.getContent();
        // inputStream convert to string
        String html = IOUtils.toString(content, "utf-8");
        // jsoup 解析
        Document parse = Jsoup.parse(html);
        Elements select = parse.select(".js-issue-row");
        for (Element element : select) {
            System.out.println(element.child(0).child(1).child(0).text());
            System.out.println("https://www.github.com" + element.child(0).child(1).child(0).attr("href"));
        }
    } finally {
        response1.close();
    }
}
```

## 同步与异步加载
+ 为什么有些数据拿不到？
  + 服务器一次返回所有的数据
  + 服务器返回一部分数据，使用AJAX异步加载
