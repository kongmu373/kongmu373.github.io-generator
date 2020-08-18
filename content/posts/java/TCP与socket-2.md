---
title: "TCP与socket 2"
date: 2020-06-27T20:32:29+08:00
draft: true
tags: ["日志系统"]
series: ["Java体系"]
categories: ["Java体系"]
---


## Socket server与client传输数据
+ 传输数据的例子(html或者js)
```java
    public static void main(String[] args) throws Exception {
        int port = 8080;
        ServerSocket socket = new ServerSocket();
        socket.bind(new InetSocketAddress("127.0.0.1", port));
        while (true) {
            Socket from = socket.accept();

            BufferedReader reader = new BufferedReader(new InputStreamReader(from.getInputStream()));
            String line;
            List<String> headers = new ArrayList<>();
            while (!(line = reader.readLine()).isEmpty()) {
                System.out.println(line);
                headers.add(line);
            }

            String firstLine = headers.get(0);
            String[] s = firstLine.split(" ");
            System.out.println(s[1]);
            from.getOutputStream().write("HTTP/1.1 200 OK\r\n".getBytes());

            if (s[1].endsWith(".js")) {
                from.getOutputStream().write("content-type: application/javascript;charset=utf8\r\n".getBytes());
                from.getOutputStream().write("\r\n".getBytes());
                InputStream is = Main.class.getResourceAsStream(s[1]);
                BufferedReader jsReader = new BufferedReader(new InputStreamReader(is));

                String jsLine;
                while((jsLine = jsReader.readLine()) != null) {
                    from.getOutputStream().write(jsLine.getBytes());
                }

            } else {

                from.getOutputStream().write("content-type: text/html\r\n".getBytes());
                // 分隔符
                from.getOutputStream().write("\r\n".getBytes());
                // HTTp response body
                from.getOutputStream().write(("<html><h1>hello</h1>"
                                                      + "<script src='/myscript.js'/>"
  
    }
```
+ 传输的数据格式
  + Content-Type(在header里,告诉client我们ResponseBody放 是什么数据)
    + Content-Type: text/html 超文本
    + Content-Type: text/plain 原生
    + Content-Type: application/octet-stream 文件
```java
socket.getOutputStream().write("Content-type: text/html\r\n".getBytes());

```

## 渲染更丰富的html页面
+ 动态的数据
  + 后端的动态渲染(new Date()转换成静态,整个页面返回)
```java
        from.getOutputStream().write(("<html><h1>hello</h1>"
                                              + "<script>"
                                              + new Date()
                                              + "</script>"
                                              + "</html>").getBytes());
```
  + 前端的动态渲染(操作dom树，修改dom树)
```java
        from.getOutputStream().write(("<html><h1>hello</h1>"
                                              + "<script>"
                                              + "document.write(new Date())" 
                                              + "</script>"
                                              + "</html>").getBytes());
```

+ 前端的动态渲染的问题
  + 整个页面可能还没加载完成，script语句就可能被加载以及执行
    + 解决:
        ```js
        window.onload = function(){
            alert("页面加载完成====》onload");
        }
        ```
        ```java
        from.getOutputStream().write(("<html><h1>hello</h1>"
                                        + "<script>"
                                        + "        window.onload = function(){\n"
                                        + "            alert(\"onload\");\n"
                                        + "        }"
                                        + "</script>"
                                        + "</html>").getBytes());
        ```



## 一个小的服务器(返回html,js,css,jpg)
+ 代码
```java
    public static void main(String[] args) throws Exception {
        int port = 8080;
        ServerSocket socket = new ServerSocket();
        socket.bind(new InetSocketAddress("127.0.0.1", port));
        while (true) {
            Socket from = socket.accept();

            BufferedReader reader = new BufferedReader(new InputStreamReader(from.getInputStream()));
            String line;
            List<String> headers = new ArrayList<>();
            while ((line = reader.readLine()) != null) {
                if(line.isEmpty()) {
                    break;
                }
                System.out.println(line);
                headers.add(line);
            }
            if(headers.size() == 0) {
                continue;
            }
            String firstLine = headers.get(0);
            String[] s = firstLine.split(" ");
            String resourceName = s[1].substring(1).equals("") ? "index.html" : s[1].substring(1);
            System.out.println(resourceName);
            from.getOutputStream().write("HTTP/1.1 200 OK\r\n".getBytes());
            if(resourceName.endsWith(".js")) {
                from.getOutputStream().write("content-type: application/javascript;charset=utf8\r\n".getBytes());
            } else if( resourceName.endsWith(".css")) {
                from.getOutputStream().write("content-type: text/css;charset=utf8\r\n".getBytes());
            } else if(resourceName.endsWith(".jpg")) {
                from.getOutputStream().write("content-type: image/jpeg\r\n".getBytes());
            } else if(resourceName.endsWith(".html")){
                from.getOutputStream().write("content-type: text/html\r\n".getBytes());
            } else {
                continue;
            }
            from.getOutputStream().write("\r\n".getBytes());
            Path file = Paths.get("C:\\Users\\48011\\Desktop\\temp\\dataStructure\\src\\main\\java\\com\\company\\socket", resourceName);
            byte[] bytes = Files.readAllBytes(file);
            from.getOutputStream().write(bytes);

            // 清空缓冲区
            from.close();
        }
```
+ 预览
![网页](img/socket1.jpg)
![命令行输出](img/socket2.jpg)