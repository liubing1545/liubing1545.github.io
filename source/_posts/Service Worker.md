---
title: Service Worker
date: 2020-04-28 21:09:10
tags: [Service Worker]
---

## Service Worker

### 概述

Service Worker本质上也是浏览器缓存资源用的，只不过他不仅仅是cache，也是通过worker的方式来进一步优化。

他基于h5的web worker，所以绝对不会阻碍当前js线程的执行，sw最重要的工作原理就是

1. 后台线程：独立于当前网页线程；

2. 网络代理：在网页发起请求时代理，来缓存文件；

### 使用条件

sw 是基于 HTTPS 的，因为service worker中涉及到请求拦截，所以必须使用HTTPS协议来保障安全。如果是本地调试的话，localhost是可以的。

### PWA

PWA是Progressive Web App的英文缩写， 翻译过来就是渐进式增强WEB应用。

#### PWA中所包含的核心功能及特性

1. Web App Manifest
2. Service Worker
3. Cache API缓存
4. Push & Notification 推送与通知
5. Background Sync 后台同步
6. 响应式设计

在service worker忠，可以监听install、activate，message，fetch，sync，push等事件。

### WorkBox

在service worker中通过监听事件，然后编写对应逻辑，缓存文件都不是很容易。并且webpack build之后，js名称随时会变。因此chrome推出了workbox框架。

**workbox 是用于向web应用程序添加离线支持的JavaScript库。**

在wokbox对象中，包含很多模块，比如 workbox.routing模块，workbox.precaching模块，workbox.strategies模块，workbox.expiration模块等等，它们分别负责处理不同的逻辑。

1. workbox缓存/预缓存
2. workbox路由
3. workbox插件

[workbox的使用介绍](https://segmentfault.com/a/1190000019281388?utm_source=tag-newest)

另外npm中也已经有这个包了[https://www.npmjs.com/package/web-pwa](https://link.jianshu.com/?t=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fweb-pwa) ，可以玩玩。









