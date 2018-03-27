---
title: shell反向删除文件
date: 2016-06-24 17:12:07
tags: [linux,shell]
---
除了filename文件外，全部rm掉。

    $ shopt -s extglob
    $ rm -rf !(filename)
 
