---
title: mac下批量替换文本
date: 2016-06-08 12:02:39
tags: [mac,sed,grep]
---
在mac下使用sed与linux下稍微有一些不同。  
-i 参数可以指定备份源文件名  

    sed -i "bk" "s/Cat/Dog/g" example.txt

替换example.txt文件中的Cat->Dog时，会生成备份文件example.txtbk。也可以指定不生成备份文件，-i参数为“”。

批量替换命令如下：  

    sed -i "" "s/Cat/Dog/g" `grep Cat -rl ./`

用grep查找出当前文件夹下含有Cat的文件，然后替换成Dog。并且不指定备份文件。
