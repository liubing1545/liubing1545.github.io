---
title: flask.exthook.ExtDeprecationWarning警告的消除
date: 2016-06-17 09:12:32
tags: [flask,shell,flask-ext-migrate]
---
flask升级到0.11版后，弃用了以 **flask.ext.xxx** 导入扩展模块的形式，改为 **flask_xxx**。
如果仍然沿用原来的形式，flask会报警告flask.exthook.ExtDeprecationWarning。  

flask团队提供了<a href="https://github.com/pallets/flask-ext-migrate">flask-ext-migrate</a>的转换工具。  

pip安装：  

    $ pip install flask-ext-migrate

转换：  

    $ flask_ext_migrate xxx.py
 
<mark>但目前pip上的版本上存在bug，最新的github上fix了这个问题。</mark>  
这个转换工具一次只能转换一个python文件。  
随便写个shell批量处理吧。  

    $ for f in `find . -name "*.py"`
    > {
    > flask_ext_migrate $f
    > }








