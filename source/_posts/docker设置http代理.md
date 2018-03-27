---
title: docker设置http代理
date: 2017-07-24 16:51:51
tags: [docker, proxy]
---
#### 创建目录
```
mkdir /etc/systemd/system/docker.service.d
```

#### 创建http-proxy.conf文件
```
touch /etc/systemd/system/docker.service.d/http-proxy.conf
```

#### 在http-proxy.conf文件中记入

```
[Service]
Environment="HTTP_PROXY=http://username:passwd@hostname:port/" "NO_PROXY=localhost,127.0.0.1"
```

#### reload配置及重启docker服务
```
systemctl daemon-reload
systemctl show docker --property Environment
systemctl restart docker.service
```
