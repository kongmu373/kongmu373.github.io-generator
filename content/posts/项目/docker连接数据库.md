---
title: "Docker连接数据库以及SpringBoot与Mybatis的集成"
date: 2020-07-15T13:30:15+08:00
draft: true
categories: ["项目"]
---

## 开始连接数据库
----
### mysql安装
----
#### 下载镜像文件
`docker pull mysql:5.7`

#### 创建实例并启动
```sh
# window10 我在Docker Desktop 将D盘设置为挂载文件夹
docker run -p 3306:3306 --name mysql \
-v D:/tmp/mysql/log:/var/log/mysql \
-v D:/tmp/mysql/data:/var/lib/mysql \
-v D:/tmp/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=123 \
-d mysql:5.7
```
参数说明
+ -p 3306:3306: 将容器的3306端口映射到主机的3306端口
+ -v D:/tmp/mysql/log:/var/log/mysql:将日志文件夹挂载到主机
+ -v D:/tmp/mysql/log:/var/lib/mysql: 将lib挂载到主机
+ -v D:/tmp/mysql/log:/etc/mysql: 将排至文件夹挂载到主机
+ -e MYSQL_ROOT_PASSWORD=hardcore: 初始化root用户的密码

### SQL
[阿里MySQL 数据库规约](https://kongmu37301.oss-cn-shenzhen.aliyuncs.com/%E9%98%BF%E9%87%8C%E5%B7%B4%E5%B7%B4%E5%BC%80%E5%8F%91%E8%80%85%E6%89%8B%E5%86%8C.pdf)
```SQL
CREATE DATABASE accounting;
USE accounting;
DROP TABLE IF EXISTS `tally_userinfo`;
CREATE TABLE `tally_userinfo` (
                                  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
                                  `username` varchar(64) NOT NULL COMMENT 'user name',
                                  `password` varchar(64) NOT NULL,
                                  `create_time` datetime NOT NULL,
                                  `update_time` datetime DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP,
                                  PRIMARY KEY `pk_id` (`id`),
                                  UNIQUE KEY `uk_username` (`username`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

INSERT `tally_userinfo` value (NULL, 'admin', '123', NOW(), NULL);

select * from tally_userinfo;

UPDATE `tally_userinfo` SET password='admin' where username = 'admin';
```

### SpringBoot 与 Mybatis 集成 的 sample 
> https://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/

+ 添加依赖
```xml
<!-- mybatis-spring-boot-starter -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>xxx</version>
</dependency>

<!-- Mysql driver -->
<dependency>
    <groupId>mysql<groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>xxx</version>
</dependency>
```

+ 添加注解
    + UserInfoMapper 
      + @Mapper
      + @Select
+ 编写代码
```java
@Mapper
public interface UserInfoMapper {

    @Select("select id, username, password, create_time, update_time from tally_userinfo where id = #{id}")
    UserInfoDO getUserInfoByUserId(@Param("id")long id);
}
```
+ 配置连接
```yml
spring:
  # MySQL
  datasource:
    url: jdbc:mysql://localhost:3306/accounting_test?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: 123
```
