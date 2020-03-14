---
title: IPFS搭建
date: 2020-03-06 14:30:11
tags: [git, blockchain, ipfs]
---

## IPFS搭建

## 下载工具

[ipfs官网](https://dist.ipfs.io/#go-ipfs)

进入官网选择合适的版本。**go-ipfs_v0.4.23_linux-amd64.tar.gz**

```shell
tar -zxf  go-ipfs.tar.gz
cd go-ipfs
./install.sh //安装文件自动mv /usr/local/bin
ipfs init //ipfs初始化
```

[下载go](https://dl.google.com/go/go1.14.windows-amd64.msi)

我下载的是windows版本。

## 私有网络

### 清除缺省启动节点

默认会连接进ipfs的公链

```shell
ipfs bootstrap rm --all
```

### 生成秘钥

```shell
git clone https://github.com/Kubuxu/go-ipfs-swarm-key-gen.git //下载秘钥生成工具源码
go build ipfs-swarm-key-gen/main.go //编译得到main.exe
main.exe>swarm.key //执行得到秘钥文件swarm.key
```

### 拷贝秘钥

将生成的swarm.key文件拷贝进所有的节点服务器中**~.ipfs**文件夹下

### 启动

选择一台稳定的公网ip地址为启动节点

```shell
ipfs id //查看该节点生成的hash
```

添加该节点地址到所有节点的bootstrap列表中

```shell
ipfs bootstrap add /ip4/启动节点的ip地址/tcp/4001/ipfs/启动节点的id的hash
```

启动。

```shell
ipfs daemon & //启动，后台运行
ipfs bootstrap list  //查看ipfs bootstrap列表
```

在各个节点查看别的节点信息

```shell
ipfs swarm peers
```

## 上传下载

```shell
ipfs add xxx.png //上传到私链上，会返回hash码
ipfs get hash码 // 下载
```

## 默认端口

一定需要保证所有服务器的4001端口、5001端口、8080端口开放input的安全组。

* 4001-与其他节点通信
* 5001-API server
* 8080-Gateway server