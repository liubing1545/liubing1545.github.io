---
title: IPFS-Cluster搭建(ubuntu)
date: 2020-03-14 16:16:10
tags: [git, blockchain, ipfs]
---

## IPFS-Cluster搭建(ubuntu)

## Update Linux packages and dependencies:

```shell
sudo apt-get update
sudo apt-get -y upgrade
```

## Install IPFS

准备好的go-ipfs资源

```shell
tar -zxf go-ipfs_v0.4.23_linux-amd64.tar.gz
cd go-ipfs
sudo ./install.sh //安装文件自动mv /usr/local/bin
ipfs init //ipfs初始化
ipfs version
```

## Creating a Private network

拷贝准备好的swarm.key到集群各个节点

```shell
cp swarm.key ~/.ipfs/swarm.key
```

## Bootstrapping IPFS nodes

```shell
ipfs bootstrap rm --all
ipfs bootstrap add /ip4/启动节点的ip地址/tcp/4001/ipfs/启动节点的id的hash
```

也可以配置环境变量强制为private mode:

```shell
export LIBP2P_FORCE_PNET=1
```

## Run IPFS daemon as a service in the background

```shell
sudo touch /etc/systemd/system/ipfs.service
```

添加配置

```shell
[Unit]
Description=IPFS Daemon
After=syslog.target network.target remote-fs.target nss-lookup.target
[Service]
Type=simple
ExecStart=/usr/local/bin/ipfs daemon --enable-namesys-pubsub
User=$rootuser
[Install] 
WantedBy=multi-user.target
```

Apply新service。

```shell
sudo systemctl daemon-reload
sudo systemctl enable ipfs
sudo systemctl start ipfs
sudo systemctl status ipfs
```

## Deploying IPFS-Cluster

```shell
tar -zxf ipfs-cluster-service_v0.12.1_linux-amd64.tar.gz
tar -zxf ipfs-cluster-ctl_v0.12.1_linux-amd64.tar.gz
```

## Generate and set up CLUSTER_SECRET variable

在主节点，生成cluster-secret

```shell
export CLUSTER_SECRET=$(od -vN 32 -An -tx1 /dev/urandom | tr -d ' \n') 
echo $CLUSTER_SECRET
```

把生成的cluster_secret设置入所有节点的.bashrc中再source

## Run IPFS-Cluster daemon as a service

```shell
sudo touch /etc/systemd/system/ipfs-cluster.service
```

主节点添加配置

```shell
[Unit]
Description=IPFS-Cluster Daemon
Requires=ipfs
After=syslog.target network.target remote-fs.target nss-lookup.target ipfs
[Service]
Type=simple
ExecStart=/home/$rootuser/ipfs-tools/ipfs-cluster-service/ipfs-cluster-service daemon
User=$rootuser
[Install]
WantedBy=multi-user.target
```

子节点添加配置

```shell
[Unit]
Description=IPFS-Cluster Daemon
Requires=ipfs
After=syslog.target network.target remote-fs.target nss-lookup.target ipfs
[Service]
Type=simple
ExecStart=/home/$rootuser/ipfs-tools/ipfs-cluster-service/ipfs-cluster-service daemon --bootstrap /ip4/38.91.120.173/tcp/9096/ipfs/12D3KooWSL6aP7UqkV8YruT99htjjdHQq4rHFvsRTrmdpSnt9cSL
User=$rootuser
[Install]
WantedBy=multi-user.target
```

Apply新service。

```shell
sudo systemctl daemon-reload
sudo systemctl enable ipfs-cluster
sudo systemctl start ipfs-cluster
sudo systemctl status ipfs-cluster
```

## 

