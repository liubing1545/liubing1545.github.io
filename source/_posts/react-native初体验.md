---
title: react native初体验
date: 2016-07-07 14:21:11
tags: [react native,npmjs.org:443]
---
今天想体验一下react native

    $ brew install node
    $ brew install watchman
    $ brew install flow
    $ sudo npm install -g react-native-cli
    
结果报错如下

    $ network getaddrinfo ENOTFOUND registry.npmjs.org registry.npmjs.org:443
    
给npm设置proxy翻墙安装就成功了。

    $ npm config set proxy http://address:8080

初始化一个project

    $ react-native init HelloWorld

成功后，新new的project里，会建立好ios和android的初始工程。
运行IOS应用程序：

    $ react-native run-ios

运行Android应用程序：

    $ react-native run-android

IOS环境配置很快啊，只要网络没有问题，就很快可以run成功。
Android环境配了三天啊。。。各种坑，各种查资料，还好最后也run好了。

开始实战吧。
