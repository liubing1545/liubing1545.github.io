---
title: supervisor管理服务器进程
date: 2017-07-07 16:57:57
tags: [supervisor,sprintboot,web.py]
---
在服务器上同时管理多个java进程和python进程，我使用supervisor。
在/etc/supervisor/conf.d/路径下创建xxx.conf    
在xxx.conf下配置如下    

```+shell
[program:gzh]
command=/root/python-gzh/venv/bin/gunicorn main:application -c /root/python-gzh/gunicorn.conf
directory=/root/python-gzh
user=root
autostart=true
autorestart=true
stdout_logfile=/root/python-gzh/logs/gzh.log

[program:provider]
command=java -jar /root/springboot/platform-system-provider.jar --spring.profiles.active=prod
directory=/root/springboot
user=root
autostart=true
autorestart=true
stdout_logfile=/root/springboot/logs/provider.log

[program:webapi]
command=java -jar /root/springboot/platform-mobile-client.jar --spring.profiles.active=prod
directory=/root/springboot
user=root
autostart=true
autorestart=true
stdout_logfile=/root/springboot/logs/webapi.log

[program:web]
command=java -jar /root/springboot/platform-admin-web.jar --spring.profiles.active=prod
directory=/root/springboot
user=root
autostart=true
autorestart=true
stdout_logfile=/root/springboot/logs/web.log
```
通过supervisorctl可以监控管理各种进程的状态了。
