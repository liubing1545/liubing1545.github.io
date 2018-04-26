---
title: 利用AppVeyor做GithubIO的CI
date: 2018-04-24 11:45:11
tags: [AppVeyor, github, hexo, CI, 持续集成, 持续部署]
---
## 建立两个仓库
 我基于hexo搭建的github.io，在更换电脑时无法随意的提交blog。（因每个机器都需要部署node+hexo的环境）    
 于是，我将我的github.io建立两个仓库。
 1. master仓库（hexo generate后生成的html等文件）
 2. hexo仓库（原始文件，包含md文档及主题等）
 并将hexo仓库设置为这个repo的主仓库。

## AppVeyor
[appveyor](https://www.appveyor.com/)是支持windows OS做CI的持续集成工具，可以使用GitHub账号登陆。绑定我的github.io项目。
![appveyor-setting1](http://obksgg9lx.bkt.clouddn.com/appveyor-setting1.png)
![appveyor-setting2](http://obksgg9lx.bkt.clouddn.com/appveyor-setting2.png)
![appveyor-setting3](http://obksgg9lx.bkt.clouddn.com/appveyor-setting3.png)
![github-access-token](http://obksgg9lx.bkt.clouddn.com/github-access-token.png)
![github-webhook](http://obksgg9lx.bkt.clouddn.com/github-webhook.png)
