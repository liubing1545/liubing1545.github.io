---
title: git撤销merge操作
date: 2016-09-18 12:02:00
tags: git
---
使用git偶尔会遇到merge错代码的情形，这时需要撤销merge的操作。    

    $ git show bb46d15

commit id 是 bb46d15。  
可以看到merge commit的parents次序。该次序从1开始，想保留哪个parent就指定它的序号。

    $ git revert -m 2 bb46d15

2是想要保留的parent branch序号，其他的parent branch会撤销掉。  
bb46d15是指定对具体的commit id进行操作。