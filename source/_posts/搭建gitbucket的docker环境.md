---
title: 搭建gitbucket的docker环境
date: 2016-12-27 13:49:09
tags: [gitbucket,git,docker]
---
为了更方便简洁的部署各种服务器应用，我在社内环境安装了docker。  
从社内直连github有时极不稳定。社内团队协同开发，搭建本地的git仓库，我选择搭建gitbucket。  
  
    $ docker search gitbucket

虽然没有docker官方放出的gitbucket镜像，但从列表里选stars最多也算靠谱点儿吧。  

    $ docker pull f99aq8ove/gitbucket

github也不网络稳定啊。。。下载两次都失败，寻找国内的镜像找到了daocloud。去daocloud官网去注册一下，然后配置docker加速器。  

    $ curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://5706b345.m.daocloud.io

该脚本可以将 --registry-mirror 加入到你的docker配置文件 /etc/default/docker 中。

配置好了之后，再次docker pull。成功！   
启动镜像。  

    $ docker run -d -p 8080:8080 -p 29418:29418 -v ${PWD}/gitbucket-data:/gitbucket f99aq8ove/gitbucket
配置了映射端口8080是gitbucket的网页入口，映射端口29418是ssh的端口。  
正常启动后，通过网页打开，显示正常。  
创建用户组，创建新的repository。  
在客户端local创建开发环境。  

    $ mkdir xxxdir
    $ cd xxxdir
    $ touch README.md
    $ git add .
    $ git commit -m "first commit"
    $ git remote add origin [URL]
    $ git push origin master
在进行push的时候，失败了，报错如下。  

    fatal: unable to access 'http://xxx.git/': The requested URL returned error: 503
    
查了一下，有可能是在gitbucket服务器之前，设置了http代理所导致的。设置git http操作的debug。  

    $ export GIT_CURL_VERBOSE = 1
再次git push查看，的确有从http代理向gitbucket服务器发出的请求。但我不知道在哪里配置了http代理了。。。  
使用git config命令查看配置文件  

查看仓库级的config  

    $ git config -local -l
查看全局级的config

    $ git config -global -l
查看系统级的config

    $ git config -system -l
查看当前生效的配置

    $ git config -l
ok！将设置的http代理全部注释掉，git push成功啦！  
社内gitbucket搭建成功！

