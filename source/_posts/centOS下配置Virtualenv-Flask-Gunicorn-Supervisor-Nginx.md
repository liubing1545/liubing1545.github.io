---
title: centOS下配置Virtualenv+Flask+Gunicorn+Supervisor+Nginx
date: 2016-07-18 16:59:19
tags: [centOS,Virtualenv,flask,Gunicorn,Supervisor]
---
在阿里云上部署flask环境。  

## 安装virtualenv并创建工程  

    $ pip install virtualenv
    $ virtualenv stooge
    $ cd stooge
    $ source bin/activate

## 安装flask并创建一个服务  

    $ pip install flask
    $ touch runserver.py
    $ vim runserver.py
    $ chmod a+x runserver.py
        
### runserver.py  

    from flask import Flask
    
    app = Flask(__name__)
    
    @app.route('/')
    def hello_world():
    	return 'Hello World!'
    
    if __name__ == '__main__':
    	app.run()
 
## 安装Gunicorn  
 
 Gunicorn是一个开源Python WSGI UNIX的HTTP服务器，默认是同步工作，支持Gevent，Eventlet异步。  

    $ pip install gunicorn

### gunicorn.conf  

在项目stooge根目录下, 配置gevent workers数处理并发，及绑定本地的端口号。 

    #worker process number
    workers = 3
    #bind local port
    bind = '127.0.0.1:8000'

## 安装Supervisor    

    $ sudo pip install supervisor

supervisor是用python实现的一款进程管理工具。supervisor会帮你把管理的应用程序转成daemon程序，而且可以方便的通过命令开启，关闭，重启等操作。而且它管理的进程一旦崩溃会自动重启，这样就可以保证程序在执行中断后自我修复。  

### supervisor配置

supervisor包括两部分：   

* supervisord  (server端)
* supervisorctl  (client端)

重定向配置文件  

    $ sudo echo_supervisord_conf > /etc/supervisord.conf
    
虽然可以把所有的配置项都写到supervisord.conf文件里，但并不推荐这样做。而是通过include的方式把不同的程序组写到不同的配置文件里。  

修改supervisord.conf的include section  

    [include]
    files = /etc/supervisor/conf.d/*.conf
    
添加program配置  

新建目录/etc/supervisor/conf.d，并创建stooge.conf

    $ touch stooge.conf
    $ vim stooge.conf
    [program:stooge]
    command=/home/$username/stooge/bin/gunicorn runserver:app -c /home/$username/stooge/gunicorn.conf
    directory=/home/$username/stooge
    user=$username
    autostart=true
    autorestart=true
    stdout_logfile=/home/$username/stooge/logs/gunicorn_supervisor.log

这样的配置，通过[program:stooge]来告诉supervisord需要管理哪个进程。可以在client端(supervisorctl或web页面)显示，并对该进程start,restart,stop。

### supervisorctl

supervisorctl 是 supervisord 的一个命令行客户端工具，启动时需要指定配置文件。  

    $ supervisord -c /etc/supervisord.conf

shell 命令  

    $ supervisorctl status #查看程序状态
    $ supervisorctl stop stooge
    $ supervisorctl start stooge
    $ supervisorctl restart stooge
    $ supervisorctl reread #读取有更新的配置文件
    $ supervisorctl update #重启配置文件修改过的进程

在这里我们启动stooge  

    $ supervisorctl start stooge

## 安装nginx
    $ yum install nginx

centos7下的nginx1.6.3没有sites-available和sites-enabled子目录。但是有conf.d子目录。和配置supervisord一样，也是可以通过include的方式把不同的程序组写到不同的配置文件里。

### nginx配置

确保/etc/nginx/nginx.conf中，http模块中引入conf.d子目录。  
  
    include /etc/nginx/conf.d/*.conf

在/etc/nginx/conf.d下创建stooge.conf  

    server {
    	listen 80;
    	server_name xx.xx.xx.xx;
    	
    	root /home/$username/stooge/;
    	access_log /home/$username/stooge/access.log;
    	error_log /home/$username/stooge/error.log;
    	
    	location / {
    		proxy_set_header X-Forword-For $proxy_add_x_forwarded_for;
    		proxy_set_header Host $http_host;
    		proxy_redirect off;
    		if (!-f $request_filename) {
    			proxy_pass http://127.0.0.1:8000;
    			break;
    		}
    	}
    }

重启nginx服务  

    $sudo service nginx restart

## 配置firewalld  

安装完nginx后，80端口是没有开放的，外网无法访问。  
增加http,https到/etc/firewalld/zones/public.xml文件。  
    
    <service name="http"/>
    <service name="https"/>
    
## 访问helloworld服务

外网访问ip，显示helloworld了！



    



