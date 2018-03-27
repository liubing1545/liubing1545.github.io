---
title: web.py与gunicorn
date: 2017-07-07 17:02:35
tags: [web.py, gunicorn]
---
web.py的application.py模块，主要实现了WSGI兼容的接口，以便应用程序被WSGI应用服务器调用。   

## WSGI接口的实现
    app = web.application(urls, globals())
    application = app.wsgifunc()

## Gunicorn.conf的配置
    workers = 3
    bind = '127.0.0.1:8000'
    
## Gunicorn的启动方式
    /$path/bin/gunicorn $filename:application -c /$path/gunicorn.conf
