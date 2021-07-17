---
title: docker容器启动iptables报错
date: 2017-03-28 20:22:32
tags: [docker,iptables]
---
### 错误信息
```
docker0: iptables: No chain/target/match by that name
```
### 重启docker
```
service docker restart
```
### 列出iptables的所有规则
```
iptables -L
```
可以看到iptables里面多出了Chain Docker的选项。    
经验为：在启动firewalld之后，iptables被激活，此时没有docker chain，重启docker后被加入到iptable里面。
