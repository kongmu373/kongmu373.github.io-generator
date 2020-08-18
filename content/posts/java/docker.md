---
title: "Docker"
date: 2020-06-17T13:59:25+08:00
draft: true
tags: ["docker"]
series: ["docker"]
categories: ["Java体系"]
---

## Docker能做什么?
+ 保证开发，测试，交付，部署的环境完全一致
+ 保证资源的隔离
+ 启动临时的，用完即弃的环境，例如测试
+ 迅速 (秒级)超大规模和扩容
+ Docker 与 Virtual Machines 相比
![docker](/img/docker.jpg)

## Docker的基本概念
+ 镜像 imgage
  + 一个预先定义好的模板文件， Docker引擎可以按照这个摸板文件启动无数个一模一样，互不干扰的容器
+ 容器 container
    + 一台独立的，虚拟计算机
+ 默认没有持久化

## docker pull/images
+ docker pull <镜像仓库:镜像名:tag>
+ docker images 查看本地已有的镜像

## docker run/ps
+ docker run 装载镜像成为一个容器
    + 每个容器有独立的id, 支持缩写
+ docker run -it <镜像名> <镜像中要运行的命令和参数>
  + 交互式命令
+ docker run -d <镜像名> <镜像中要运行的命令和参数>
  + -d daemon模式，后台运行
+ --name 为容器指定一个名字
+ --restart=always 遇到错误自动重启
+ -v <本地文件>:<容器文件>
+ -p <本地端口>:<容器端口>
+ -e NAME=VALUE

## docker start/stop
+ 启动/停止一个容器

## docker rm
+ docker rm -f 容器名
+ 删除一个容器

## docker exec 
+ 指定目标容器，进入容器执行命令
  + docker run -it <目标容器ID> <目标命令 通常为(bash)>
+ 调试，解决问题的必备

## doker logs
+ 查看目标容器的输出
+ docker logs <容器ID或容器名>
+ docker logs -f <容器ID或容器名>

## docker inspect
+ 查看容器状态

## dockefile
+ 指定镜像如何生成 
+ 编写完运行 docker build .
+ 每个镜像都有一个唯一ID
