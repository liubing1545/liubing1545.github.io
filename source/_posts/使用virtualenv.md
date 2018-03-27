---
title: 使用virtualenv
date: 2016-05-27 22:00:05
tags: python
---

虚拟环境非常有用。可以在系统的python解释器中避免包的混乱和版本的冲突。

## 安装virtualenv

    sudo pip install virtualenv

## 创建python虚拟环境

在工程文件夹下创建python虚拟环境。创建虚拟环境后，当前文件夹中会出现一个子文件夹，名字为下述命令中指定的参数venv。  

    virtualenv venv     
    
## 激活

    source venv/bin/activate
    
## 退出

    deactivate