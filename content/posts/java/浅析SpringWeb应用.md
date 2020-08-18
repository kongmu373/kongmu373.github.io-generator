---
title: "浅析SpringWeb应用"
date: 2020-06-14T09:51:00+08:00
draft: true
tags: ["Spring"]
series: ["Spring"]
categories: ["Spring"]
---

## Web应用
+ 处理HTTP请求
  + 从HTTP请求中提取query string(查询字符串)
  + 从HTTP请求中接收payload(负载)中的参数
+ 返回HTTP响应
  + status code
  + HTTP resonse header
  + HTTP response body
    + JSON
    + HTML
    + ...

## HTTP GET
+ Query string
  + ?param1=value1&param2=value2
  + 通常用来传递非敏感信息
+ 使用@RequestParam进行接收

## RESTful API
+ 使用HTTP动词来代表动作
  + GET: 获取资源
  + POST: 新建资源
  + PUT: 更新资源
  + DELETE: 删除资源
+ 使用URL (名词) 来代表资源
  + 资源里面没有动词
  + 使用复数来代表资源列表
![RESTFUL API](/img/RESFULAPI.jpeg)

## @RestController
+ 使用RESTful风格的参数
  + @PathVariable 
  + @RequestBody
  + ...
```java
@RestController
@RequestMapping("repos")
public class IssueController {
    @DeleteMapping("/{owner}/{repo}/issues/{issueNumber}/lock")
    public void unlock(
        @PathVairable("owner") String owner,
        @PathVairable("repo") String repo,
        @PathVairable("issueNumber") String issueNumber) {
            // do something...
    }

    @PostMapping("/{owner}/{repo}/{issues}")
    public void create(
        @PathVariable("owner" String owner,
        @PathVariable("repo") String repo, @RequestBody Object object) {
            // do something...
        }
    )
}
```

## @PostMapping
+ 处理post请求
+ 从HTTP POST请求中提取body

| 场景 | Content-Type | 使用注解 | 适用于|
| ----| ---- | ---- | ----|
| 提取整个body中的对象| application/json | @RequestBody| JSON|
|提取body中的参数|application/x-www-form-urlencoded|@RequestParam|表单|