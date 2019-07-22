---
title: gitignore更新后，删除未来不需要管控的程序
date: 2019-07-22 14:30:11
tags: [git, gitignore]
---

## 提交更新后的gitignore文件，删除本地缓存再提交
```shell
git rm -r --cached .
git add .
git commit -m "update .gitignore"
```