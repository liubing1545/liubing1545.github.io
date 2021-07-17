---
title: java工程启动 No Route to host
date: 2017-03-28 20:17:35
tags: [docker,java,No Route to host]
---
1. 检查宿主机防火墙是否开启    
```
firewall-cmd --state #查看默认防火墙状态（关闭后显示not running，开启后显示running）    
systemctl stop firewalld.service #停止firewall    
systemctl disable firewalld.service #禁止firewall开机启动    
```
2. 确认project工程连接的postgresql服务是映射到宿主机的ip及port，而不是postgresql docker自身的ip及port
3. 确认postgresql服务docker的pg_hba.conf文件，是否设置接收任意的ip发来的请求    
pg_hba.conf:    
```
host    all             all             0.0.0.0/0               md5    
```
4. 确认postgresql服务docker的postgres.conf文件的监听端口    
postgres.conf:    
```
listen_addresses = '*'      # what IP address(es) to listen on;    
```
**netstat -tunlp命令**查看下监听状态是否正常    
```
Active Internet connections (only servers)                                 
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:5432            0.0.0.0:*               LISTEN      816/postgres         
tcp6       0      0 :::5432                 :::*                    LISTEN      816/postgres
```
### 参考
[stackoverflow解决方法](http://stackoverflow.com/questions/25069832/docker-tomcat-and-postgresql-containers-in-same-host-no-route-to-host)