---
title: IPFS搭建(windows节点)
date: 2020-03-13 20:15:10
tags: [git, blockchain, ipfs]
---

## IPFS搭建(windows节点)

## 下载

[ipfs官网](https://dist.ipfs.io/#go-ipfs)

进入官网选择合适的版本。**Windows Binary**



## 初始化

在下载好的binary文件夹里执行以下命令后，会在根目录生成一个.ipfs的文件夹存储节点数据。

```shell
ipfs.exe init //初始化ipfs
ipfs bootstrap rm -all //删除默认的bootstrap设置
```

将集群中之前生成好的swarm.key文件拷贝进**~.ipfs**文件夹下



### 启动

添加启动节点地址到当前节点的bootstrap列表中

```shell
ipfs bootstrap add /ip4/启动节点的ip地址/tcp/4001/ipfs/启动节点的id的hash
```

一定需要保证所有服务器的4001端口和5001端口开放input的安全组。

```shell
ipfs daemon & //启动，后台运行
ipfs bootstrap list  //查看ipfs bootstrap列表
```

在各个节点查看别的节点信息

```shell
ipfs swarm peers
```

