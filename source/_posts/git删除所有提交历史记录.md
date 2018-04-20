---
title: git删除所有提交历史记录
date: 2018-04-20 10:53:25
tags: [git]
---
## checkout

   ```shell
   git checkout --orphan latest_branch
   ```

## 添加所有文件

   ```shell
   git add -A
   ```
   ​

## 提交变更

   ```shell
   git commit -am "something about message"
   ```
   ​

## 删除分支

   ```shell
   git branch -D master
   ```
   ​

## 重命名当前branch为目标名

   ```shell
   git branch -m master
   ```

   ​
## 强制push

   ```shell
   git push -f origin master
   ```
   ​