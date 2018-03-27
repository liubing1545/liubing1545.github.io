---
title: jenkins自动部署docker应用
date: 2017-11-17 18:29:54
tags: [jenkins, 持续部署, docker]
---
## 项目背景
目前需要部署一个基于python flask的web服务，数据库使用的是postgresql。
 
## 思路&流程
* 准备docker镜像
* jenkins拉取远端源码--git
* 实现应用打包--jenkins本地
* 把应用打包进docker镜像--dockerfile
* 镜像同步到docker私有仓库--shell docker命令
* 删除老的docker容器--shell docker命令
* 运行新的docker容器--shell docker命令

### 准备docker镜像
#### 创建自定义flask镜像
基于**tiangolo/uwsgi-nginx-flask**创建flask的镜像。    
自定义flask镜像的dockerfile:

```shell
FROM tiangolo/uwsgi-nginx-flask:python2.7

COPY ./app /app
COPY ./lib/libseuif97.so /usr/lib
RUN pip install -r requirements.txt
```

因为网络环境不好，安装requirements.txt中python的第三方库时，发生报错，因此我将基本的库事先安装好。    
并且把需要的so静态文件也COPY进镜像中。    
后面如果有变化，可以根据需要在jenkins中再动态生成dockerfile并执行。

```shell
docker build -t test-flask .
```

#### 创建自定义的postgresql
自定义postgresql镜像的dockerfile:

```shell
FROM postgres:9.3

ADD ./sql /docker-entrypoint-initdb.d/
```
我们有一些master表，以及基础数据，我们需要在运行这个容器之前，将这些基础数据insert进db这个docker之中。    
可以把各种sql文件放入/sql路径下。

#### commit到私有docker仓库

```shell
docker commit -m "flask插件安装" -a "liubing" $containId liubing/test-flask
docker tag liubing/$imageName $dockerRegistsryAddress/test-flask
docker push $dockerRegistsryAddress/test-flask
```

db的docker也可提前commit到私有仓库。
### jenkins拉源码

在jenkins中配置“源码管理”，输入source仓库的git地址，并绑定git用户及需要检测状态变化的branch。在构建时会自动下载git源码的。

![配置git地址](http://obksgg9lx.bkt.clouddn.com/git.png)

### 实现应用打包

目前开发的是一个python项目，python不需要打包。源码即可执行。

### 把应用打包进docker镜像

```shell
echo 'From $dockerRegistsryAddress/test-flask
MAINTAINER liubing "lbingg@hotmail.com"

COPY . /app

' > Dockerfile;
```
jenkins中执行的脚本会默认当前脚本处于jenkins环境中的workspace中的当前应用工程下。因此在copy程序时，我们需要将当前目录下的所有文件全部拷贝入新创建docker镜像的/app目录中。

### 镜像同步到docker私有仓库

```shell
docker build -t $dockerRegistsryAddress/test-flask;
```
### 删除老的docker容器

```shell
docker stop postgresql || true;
docker rm postgresql || true;
docker stop test-flask || true;
docker rm test-flask || true;
```

### 运行新的docker容器

```shell
docker run --name=postgresql -itd --restart always --publish 5432:5432 --volume /opt/postgresql/data:/var/lib/postgresql --env 'DB_USER=postgres' --env 'DB_PASS=postgres' --env 'DB_NAME=postgres' $dockerRegistryAddress:5000/test-db-1;

docker run --name test-flask --publish 80:80 --link postgresql:postgres -d $dockerRegistryAddress:5000/test-flask;
```

jenkins中的“构建”配置

![配置构建](http://obksgg9lx.bkt.clouddn.com/structure.png)

### 问题点

需要在jenkins的docker中运行其他的docker命令，可以使用Docker outside of Docker来配置。    
![Docker-outside-of-Docker](http://liubing1545.github.io/2017/11/16/Docker-outside-of-Docker)