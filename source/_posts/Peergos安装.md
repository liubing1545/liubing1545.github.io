---
title: Peergos安装记
date: 2020-03-02 14:30:11
tags: [git, peergos, ipfs]
---

## Peergos安装记

## 环境准备

作者在github里记录，为了跑tests，需要安装ant-optional这个插件，根据调查，ant-optional这个插件只有debian/ubuntu提供。因此准备一台ubuntu的机器。

## 安装工具

```shell
sudo apt-get install openjdk-11-jdk
sudo apt-get install ant
```

* ubuntu：Ubuntu 18.04.3 LTS
* java：openjdk 11.0.6
* ant：1.10.5

## 下载peergos

```shell
git clone https://github.com/Peergos/Peergos.git
git clone https://github.com/Peergos/web-ui.git
```

## 网络要求

必须翻墙！！！

## build

根据github执行各种ant命令，启动后台就是下面的命令

```shell
ant update_and_run
```
考虑有两种方式运行web-ui。

1. 在ubuntu客户端通过ide运行整体系统。
2. 前后端分离部署，前端通过命令ant ui来build静态资源文件，要修改vue的代码，在post/get等http请求中加入8000端口的后端url。通过nginx代理别的端口即可。