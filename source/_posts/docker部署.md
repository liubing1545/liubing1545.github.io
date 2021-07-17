---
title: docker部署web环境
date: 2017-03-28 20:04:17
tags: [docker,postgresql,redis,web]
---
### postgresql
```
1. docker pull sameersbn/postgresql
2. docker run --name=postgresql -itd --restart always \
--publish 5432:5432 \
--volume /opt/postgresql/data:/var/lib/postgresql \
--env 'DB_USER=mymebyo_adm01' --env 'DB_PASS=mymebyo_adm01' --env 'DB_NAME=mymebyo' \
sameersbn/postgresql    
```
### redis
```
1. docker pull redis
2. docker run --name=redis -p 6379:6379 -v /opt/redis/data:/data -d redis redis-server --appendonly yes
```
**redis-server --appendonly yes** :在容器执行redis-server启动命令，并打开redis持久化配置
### java web
```
1. docker pull java
2. docker run -it --volume /var/www:/var/www --publish 8082:8080 --link postgresql:mebyo --link redis:redis java /bin/bash
3. java -jar mebyo-1.0.0.jar
```