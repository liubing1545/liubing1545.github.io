---
title: MultiHash和Base58
date: 2020-05-23 11:42:10
tags: [MultiHash, Base58, ipfs]
---

## MultiHash和Base58

### MultiHash

目前IPFS使用SHA2-256的加密方式，目前来说比较安全，但随着科技发展，说不定哪天就会被破解。因此IPFS在制定协议时，采用了MultiHash这种方式，可扩展支持多种HASH算法。如果未来改用了别的算法，用的仍然是MultiHash，也就保持了加密方式的持续性。

#### 格式

* HASH算法编码
* HASH值长度（字节数）
* HASH值

SHA2-256的编码为0x12，其Hash摘要长度为32字节（十六进制为0x20）。

所以将 **1220** 添加到所有的Hash值前。

### Base58

Base64有一些缺点，就是某些字符容易混淆，比如**O**和**0**等。很容易将字符串认错，造成阅读上的障碍。Base58将Hash值压缩，并且剔除了干扰字符。

因此**1220**开头的Hash值都被编码成了**Qm**开头。

这就是ipfs add时，返回的CID都是Qm开头的原因。