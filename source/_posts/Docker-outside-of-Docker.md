---
title: Docker outside of Docker
date: 2017-11-16 16:06:20
tags: [持续集成, CI, DooD, jenkins, docker]
---
## 添加用户jenkins至docker组
```shell
# 如果不存在docker组就groupadd一个
$ sudo groupadd docker

# 添加用户jenkins至docker组 
$ sudo gpasswd -a jenkins docker

# 重启docker服务
$ sudo service docker restart
```


## 自定义jenkins镜像
镜像Dockerfile    

```shell
From jenkins

USER root
AGR dockerGid=995

RUN echo "docker:x:${dockerGid}:jenkins" >> /etc/group

USER jenkins
```

其中的dockerGid是docker组id，可以在宿主机里**cat /etc/group | grep ^docker**查看组id序号。    

### docker:dial unix /var/run/docker.sock:permission denied.
上面的dockerfile内容关键是将宿主机的docker组及用户配置，写入docker镜像里，防止在docker容器里运行docker命令没有权限，会报错。    
**一定要注意，设置的dockerGid一定要与宿主机的组id一致。**

### build自定义镜像
```shell
docker build -t local_jenkins
```

## Docker进程监听的Unix域socket
### /var/run/docker.sock
这个文件是什么呢？简单地说，它是Docker守护进程(Docker daemon)默认监听的Unix域套接字(Unix domain socket)，容器中的进程可以通过它与Docker守护进程进行通信。
### 管理jenkins的docker-compose
```shell
version: '2'
services:
  my_jenkins:
    image: local_jenkins
    volumes:
      - ./jenkins_home:/var/jenkins_home
      - /bin/docker:/usr/bin/docker
      - /usr/bin/docker.sock:/var/run/docker.sock
    ports:
      - "8081:8080"
```
在这个docker-compose中，我们挂载了**/bin/docker**,**/var/run/docker.sock**，实现了DooD(Docker outside of Docker)。在jenkins镜像里run起来的容器实际上就是运行在宿主机上的。




