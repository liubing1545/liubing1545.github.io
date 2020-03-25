---
title: IPFS-Cluster客户端构建
date: 2020-03-25 20:42:10
tags: [react, node, electron, blockchain, ipfs]
---

## IPFS-Cluster客户端构建

## 安装windows构建插件

需要使用管理员权限安装

```shell
npm install –global –production windows-build-tools
```

保证有python2.7的编译环境

## 下载源码

```shell
git clone --depth 1 --single-branch https://github.com/caorushizi/oss-client.git
# 进入目录
cd oss-client
# 安装依赖
npx cross-env npm_config_electron_mirror="https://npm.taobao.org/mirrors/electron/" npm_config_electron_custom_dir="7.1.9" npm install
# 运行
npm start
```

