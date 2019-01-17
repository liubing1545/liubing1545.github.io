---
title: aws爬梯子
date: 2019-01-17 14:30:11
tags: [aws, 梯子]
---

## ubuntu下亲测有效
```shell
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh

chmod +x shadowsocks-all.sh

./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```