---
title: 配置gitbucket的webhook触发jenkins自动构建
date: 2017-11-20 12:10:22
tags: [gitbucket, webhook, jenkins, 持续部署]
---

## jenkins安装gitbucket插件
在jenkins中安装插件:**Gitbucket Plugin**   
安装好了之后配置构建触发器。    
![构建触发器](http://obksgg9lx.bkt.clouddn.com/triggle.png)

## gitbucket设置webhook
使用root管理员账户进入需要设置webhook的具体项目中Settings->Service Hooks菜单下，配置gitbucket的webhook。    
![配置webhook](http://obksgg9lx.bkt.clouddn.com/gitbucket.png)

## 自动触发
source提交git。    
![push状态](http://obksgg9lx.bkt.clouddn.com/push-status.png)

自动触发构建。    
![自动构建](http://obksgg9lx.bkt.clouddn.com/console.png)