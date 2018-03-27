---
title: >-
  YAMLException: can not read a block mapping entry; a multiline key may not be
  an implicit key at line 4, column 1
date: 2017-03-28 19:35:15
tags: hexo
---
在hexo generate时，报错:

    YAMLException: can not read a block mapping entry; a multiline key may not be an implicit key at line 4, column 1
    
原因是在tags:hexo之间hexo之前要加一个空格！！！