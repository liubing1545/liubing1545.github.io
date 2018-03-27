---
title: docker创建私有仓库
date: 2017-07-24 16:50:48
tags: [docker, registry]
---

## 拉取registry的镜像
```
docker pull registry:2.1.1
```
## 启动容器
```
docker run -d -v /root/compose-docker-registry:/var/lib/registry -p 5000:5000 --restart=always --name registry2 --privileged registry:2.1.1
```
## 打包镜像push到本地仓库
```
docker tag postgres $registry-address:5000/postgres
docker push $registry-address:5000/postgres
```
   
## list仓库中的镜像
在client端执行
```
curl -XGET http://$registry-address:5000/v2/_catalog    
```

## 可能问题

#### docker received unexpected HTTP status:501 Not Implemented
这个问题是client端配置了docker的http-proxy，没有将docker的私有仓库registry所在ip设为例外导致的。
#### server gave HTTP response to HTTPS client
registry默认的是https协议，因此需要在client端配置insecure-registries。    
我的docker版本是1.12.1，配置文件是/etc/docker/daemon.json    
```
{"insecure-registries":["$registry-address:5000"]}
```
        