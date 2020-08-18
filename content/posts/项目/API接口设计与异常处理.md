---
title: "API接口设计"
date: 2020-07-15T16:54:08+08:00
draft: true
categories: ["项目"]
---


----
### API接口设计
----
REST: Representational State Transfer [表述性状态转移](http://en.wikipedia.org/wiki/Representational_state_transfer)
[Microsoft REST API Guidelines](https://github.com/Microsoft/api-guidelines/blob/vNext/Guidelines.md)

+ URL结构
  + REST API 不出现动词
  + url都是对资源的描述(描述应该易于理解)
  + 最好带版本控制信息
  + 例子:`api.kongmu373.com/v1/users/1`
+ 支持的方法

| 方法 | 描述 | 幂等性 |
|---- | ---- | ---- |
| GET | 返回一个对象的当前值 | True |
| PUT | 替换一个对象，或者创建命名对象(如果适用) | True |
| DELETE | 删除一个对象 | True |
| POST | 根据提供的数据创建新对象，或者提交一个命令 | False |
| HEAD | 返回一个元数据对象以获取GET想象 支持GET方法的资源，也支持HEAD方法的资源 | True |
| PATCH | 更新对象的一部分 | False |
| OPTIONS | 获取关于request的信息 | True | 


### 微软指南的Standard request headers,以及Standard response headers

### Response格式
+ 基本要求
  + JSON属性应该用camelCased 
  + Controller层应该将JSON作为默认编码
  + Controller层必须要支持application/json, 并且将application/json作为默认response format
  + 错误请求
    + 用HTTP status code
      + OK 200 201.
      + 客户端错误: 4XX
      + 服务器错误: 5XX

+ ErrorResponse 例子:
```json
HttpStatusCode

Body: {
    "status": 404, // optional
    "message": "", // must
    "errorCode": "PASSWORD_INVALID",   // 业务错误
    "action": "",
    "detail":[
        {},
        {},
        ...
    ]
}
```  
