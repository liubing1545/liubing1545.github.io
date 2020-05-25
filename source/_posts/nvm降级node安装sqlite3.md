---
title: nvm降级node安装sqlite3
date: 2020-05-22 15:51:10
tags: [nvm, node, npm]
---

## nvm降级node安装sqlite3

### node12.14.0下安装sqlite3@3.1.13失败

> download (403): https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v3.1.13/node-v72-win3 2-x64.tar.gz
>
> binaries not found for sqlite3@3.1.13 and node@12.14.0 (node-v72 ABI) (falling back to source compile with node-gyp)

### 解决方法

安装nvm [下载地址](https://github.com/coreybutler/nvm-windows/releases)

```shell
nvm install 8.9.3
nvm use 8.9.3
```

配置nvm，如果需要proxy的话就配置proxy

``` shell
nvm proxy http://$proxy-url/
nvm node_mirror https://npm.taobao.org/mirrors/node/
nvm npm_mirror https://npm.taobao.org/mirrors/npm/
```

再次安装即能成功

```shell
npm install
```







