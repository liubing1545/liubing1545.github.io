layout: testhexo
title: mac下搭建hexo至github
date: 2016-05-22 11:28:02
tags:
---

**hexo**是一款基于Node.js的静态博客框架:<a href="https://github.com/hexojs/hexo">hexo github</a>
  
## 安装nodejs

    brew install node
    
## 安装hexo

    npm install -g hexo
    hexo init <folder>
    npm install

## hexo语法

    hexo new "postName" #新建文章
    hexo generate #生成静态页面至public目录
    hexo server #本地预览，默认4000端口
    hexo deploy ＃发布

## 配置

修改hexo根目录下的_config.yml文件,xxx为github的账户名称。  

    deploy:
      type: git
      repository: https://github.com/xxx/xxx.github.io.git
      branch: master
     
在github创建xxx.github.io

## 安装主题

个人比较喜欢<a href="https://github.com/litten/hexo-theme-yilia">yilia github</a>

## 发布

    $ hexo d -g