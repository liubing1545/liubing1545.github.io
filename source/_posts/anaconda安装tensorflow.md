---
title: anaconda安装tensorflow
date: 2017-08-29 11:30:25
tags: [anaconda, conda, tensorflow]
---
## 下载anaconda

anaconda是一个用于科学计算的python发行版，支持linux、mac、windows系统。    
[anaconda官方下载地址](https://www.continuum.io/downloads)    
我下载的版本是python27的anaconda2-4.4.0-Windows-x86_64.exe    

## 创建python3的anaconda环境
```shell
conda create -n python3 python=3.5.2 anaconda
```

## 安装tensorflow
```shell
# 查看当前版本分支
conda info -e

# 切换python3的环境
activate python3

# 安装tensorflow
conda install tensorflow
```

## 启动spyder
```shell
# 在python3的虚拟环境下
spyder
```
