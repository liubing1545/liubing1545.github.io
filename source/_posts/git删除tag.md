---
title: git删除tag
date: 2016-11-23 15:07:14
tags: git
---
同事在git上误打了tag，并且只删除了本地的tag，没有删除origin上的tag。管理员账户登录了git仓库的web页面，找到了这个tag但还是没有办法删掉。  
ok！只有命令行开搞！  
#### 配置ssh config
![webapi](http://obksgg9lx.bkt.clouddn.com/ssh-config.png)
#### 删除tag
![webapi](http://obksgg9lx.bkt.clouddn.com/delete-tag.png)
#### 主要命令
    ssh-agent
    eval `ssh-agent`
    ssh-add $git_rsa
    git tag -d $tag_name
    git push origin :refs/tags/tag_name
