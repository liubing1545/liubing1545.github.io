---
title: docker-compose的使用
date: 2017-07-27 12:09:33
tags: [docker, docker-compose]
---
docker compose是一个用来定义和运行复杂应用的docker工具。使用compose，你可以在一个文件中定义一个多容器应用，然后使用一条命令来启动应用。    

## 下载docker-compose

```shell
curl -L https://github.com/docker/compose/releases/download/1.3.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```
## 创建应用目录
```shell
mkdir compose-gitbucket
touch docker-compose.yml
```

## docker-compose.yml配置文件
```yml
gitbucket:
  image: f99aq8ove/gitbucket
  volumes:
    - ./gitbucket:/gitbucket
  ports:
    - "8080:8080"
    - "29418:29418"
  restart: always
```

## docker-compose命令
> build 构建或重建服务    
> help 命令帮助    
> kill 杀掉容器    
> logs 显示容器的输出内容    
> port 打印绑定的开放端口    
> ps 显示容器    
> pull 拉取服务镜像    
> restart 重启服务    
> rm 删除停止的容器    
> run 运行一个一次性命令    
> scale 设置服务的容器数目    
> start 开启服务    
> stop 停止服务    
> up 创建并启动容器    

一般up之后，又改写了yml配置文件，则需要rm掉旧的容器，再up。

## 后台启动
在应用目录下启动

```shell
docker-compose up -d
```
