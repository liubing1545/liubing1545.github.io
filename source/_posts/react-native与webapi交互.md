---
title: react native与webapi交互
date: 2016-08-08 11:56:15
tags: [react native,Runtime is not ready for debugging,Network request failed]
---
react native坑太大了！！！
既然跳进去了，就想办法填坑呗～

## webapi
#### 本地开启webAPI
![webapi](http://obksgg9lx.bkt.clouddn.com/webapi.png)

#### curl测试webAPI
![test webapi](http://obksgg9lx.bkt.clouddn.com/getToken.png)

## react native如何debug
#### command + d
在ios的simulator上，command+d调出菜单，选择Debug JS Remotely

![debug](http://obksgg9lx.bkt.clouddn.com/debugJS.png)  

#### Runtime is not ready for debugging.
如果报出该错，则需要下载chrome浏览器，mac自带的safari无法载入本地的reactJS文件。

![error](http://obksgg9lx.bkt.clouddn.com/debugging.png)

#### Network request failed.
如果报出该错，需要配置xcode里对于http请求的设置。  

![error](http://obksgg9lx.bkt.clouddn.com/networkRequestFail.png)

这个错误搞的我烦躁了一个周末，根本没有头绪。  
首先，要确认RCTWebSocketExecutor.m文件中host = @"localhost";

![host](http://obksgg9lx.bkt.clouddn.com/RCTWebSocketExecutor.png)

其次，要在plist的APP Transport Security Settings中添加Allow Arbitrary Loads为True。这个设置把request请求不仅仅局限于https安全模式，http也被允许了。

![plist](http://obksgg9lx.bkt.clouddn.com/plist.png)

#### chrome debug
测试一下，与本地webapi的交互，在chrome浏览器下可以打断点，查看变量了。

![chrome](http://obksgg9lx.bkt.clouddn.com/chromeTool.png)

webapi的监听也收到了request。

![webapi listen](http://obksgg9lx.bkt.clouddn.com/requestForWebAPI.png)

## 退出debug模式

#### command + d
在ios的simulator上，command+d调出菜单，选择Stop Remote JS Debugging

![stop debug](http://obksgg9lx.bkt.clouddn.com/debugJSStop.png)

参照<a href="http://facebook.github.io/react-native/releases/0.31/docs/debugging.html#debugging">facebook debugging文档</a>
