---
title: "TCP与socket"
date: 2020-06-22T20:29:53+08:00
draft: true
tags: ["日志系统"]
series: ["Java体系"]
categories: ["Java体系"]
---


## 通信协议没有100%可靠的
+ 经典的红军/蓝军问题
    - 红军 - 敌人  - 蓝军 
    - 红军与蓝军交换信息都要经过敌人
    - 因此无论怎么发，都没有100%可靠的通信协议

## 计算机网络与传统电信网络
+ 传统电信网络 (电话网络)
  + 电路交换网路 (震动-电信号)
+ 计算机网络
  + 目标:实现全球的连接，并且物理无法毁坏的网络
  + 分组交换(在不同的中间设备中交换,每台设备都遵守IP协议)
  + 虚电路 (感觉报文沿着某条电路传输，实际该电路不存在)

## IP协议
+ IP长度4个字节(1个字节 0-255,总共32位)

## 计算机网络的分层模型
+ 广泛使用的 -- TCP/IP五层模型
  + 应用层 qq brower steam ..
  + 传输层 (transport layer)  TCP/UDP
  + 网络层  IP
  + 数据链路层
  + 物理层
+ QQ Browser Steam 的通信
    - 每个机器只有一个ip 
    - 数据包抵达网络层 通过端口分发到不同的应用中(多路复用 multiplexing)
+ 端口
  + 0 - 65535

## TCP 
+ Transport Control Protocol
+ 特点:
  + 面向连接
  + 点对点
  + 可靠交付
  + 面向字节流
+ TCP的握手与断开
  + 三次握手
    + CONNECT (x,y位随机数)
    + client  发送 SYN x 
    + server  发送 ACK x+1 y 
    + client  发送 ACK y+1   
    + ESTABLISHED
  + 建立了一条基于流的双向高速通路(TCP)
  + 断开
    + client FIN seq = x+2, ACK = y+1
    + server ack = x + 3
    + server FIN seq = y+ 1
    + client ACK = y+2
  + 不正常断开 （措施:超时断开 心跳检测)
+ 粘包/拆包方式
    + 定长
    + 设计协议
    + 规定结束符
+ HTTP(S) FTP DUBBO 都建立在TCP之上

## UDP (用户数据包协议)
+ 把数据分成一个个数据包，然后丢出去(不管是否可达)
+ 特点:
  + 无连接的
  + 尽最大可能交付
  + 面向报文的
  + 比TCP速度快
+ 场景:
  + 视频,直播


## 从数据链路层到传输层
+ 以太网首部  + (IP包首部) + (TCP包首部) + (数据)      -- 以太网数据
+ (IP包首部) + (TCP包首部) + (数据)   -- IP数据
+ (TCP包首部) + (数据) -- TCP数据


## Socket
+ 翻译: 插座,套接字
+ 代表建立从client到server的这条TCP通道
+ 四元组:
  + src ip 
  + src port 
  + des ip
  + des port 
+ 四元组确定一个Socket, 确定一个程序
+ 如何建立一个Socket连接?
```java
// 106.11.208.37:443
    @Test
    public void testSocket() throws IOException {
        try (Socket socket = new Socket()) {
            socket.connect(new InetSocketAddress("106.11.208.37", 443));
            System.out.println("连接成功");
        } catch (IOException e) {
            System.out.println("连接失败");
        }
    }
```

+ 展示从socket 到http 协议
```java
// 浏览器访问 localhost:8080 返回一个 `hello` 页面
@Test
    public void writeAHttpServer() throws IOException, InterruptedException {
        int port = 8080;
        ServerSocket serverSocket = new ServerSocket();
        serverSocket.bind(new InetSocketAddress("127.0.0.1", port));
        Socket from = serverSocket.accept();

        BufferedReader reader = new BufferedReader(new InputStreamReader(from.getInputStream()));
        String s = reader.readLine();
        System.out.println(s);
        // HTTP response header
        from.getOutputStream().write("HTTP/1.1 200 OK\r\n".getBytes());
        // 分隔符
        from.getOutputStream().write("\r\n".getBytes());
        // HTTp response body
        from.getOutputStream().write("Hello".getBytes());
        // 清空缓冲区
        from.close();
        socket.close();
    }
```