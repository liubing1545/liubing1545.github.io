---
title: IPFS-Cluster搭建
date: 2020-03-14 16:16:10
tags: [git, blockchain, ipfs]
---

## IPFS-Cluster搭建

私有网络集群允许IPFS节点只连接到拥有共享密钥的其他对等节点，网络中的节点不响应来自网络外节点的通信请求。IPFS-Cluster是一个独立的应用程序和一个CLI客户端 ，它跨一组IPFS守护进程分配、复制和跟踪pin。它使用基于Raft一致性算法来协调存储，将数据集合分布到参与的节点上。

目前在搭建分布式存储系统时，需要将一个peer上存储的文件同步备份到所有集群上的其他的peer上，或者对集群的节点管理，就需要IPFS-Cluster来实现。

## 下载

[ipfs官网](https://dist.ipfs.io/#go-ipfs)

进入官网选择合适的版本。

```shell
wget https://dist.ipfs.io/ipfs-cluster-ctl/v0.12.1/ipfs-cluster-ctl_v0.12.1_linux-amd64.tar.gz
wget https://dist.ipfs.io/ipfs-cluster-service/v0.12.1/ipfs-cluster-service_v0.12.1_linux-amd64.tar.gz
```



## 安装ipfs-cluster-service

```shell
tar -zxf ipfs-cluster-service_v0.12.1_linux-amd64.tar.gz
cd ipfs-cluster-service
./ipfs-cluster-service init //初始化
```

初始化之后会生成两个文件 

> .ipfs-cluster/service.json
>
> > 含有secret和监听端口，需要把secret配置到其他节点的service.json里，保证一样。

> .ipfs-cluster/identity.json
>
> > 含有cluster的节点id



## 启动

主节点

```shell
./ipfs-cluster-service daemon &
```

其他节点启动--bootstrap添加主节点

```shell
./ipfs-cluster-service daemon --bootstrap /ip4/$主节点ip/tcp/9096/ipfs/$cluster id
```



## 安装ipfs-cluster-ctl

```shell
tar -zxf ipfs-cluster-ctl_v0.12.1_linux-amd64.tar.gz
cd ipfs-cluster-ctl
./ipfs-cluster-ctl peers ls
```



## 默认端口

一定需要保证所有服务器的9094端口、9095端口、9096端口开放input的安全组。

* 9094-HTTP API endpoint
* 9095-IPFS proxy endpoint
* 9096-Cluster swarm