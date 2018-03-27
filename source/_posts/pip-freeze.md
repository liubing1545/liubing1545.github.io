---
title: pip freeze
date: 2016-05-31 10:15:40
tags: python
---
在本地python开发时，可以生成requirements.txt文件，用于记录所有依赖包及其精确的版本号。  

    pip freeze >requirements.txt
可创建与本地完全一致的副本环境。  

    pip install -r requirements.txt
