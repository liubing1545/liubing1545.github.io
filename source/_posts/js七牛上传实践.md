---
title: js七牛上传实践
date: 2016-06-24 17:22:14
tags: [js,七牛,flask]
---
* 七牛有免费的配额可以使用，在测试开发时，将图片，视频流等多种媒体文件可以上传到七牛上。  
* 其次，七牛可以绕过搭载应用的server，手机端或者pc端可以直接上传下载媒体资源至七牛云。只是在上传时，需要先向应用server要求访问七牛的token，拿到这个token后直接与七牛交互。  
* 最后，七牛云支持cdn加速，即使对成熟的应用来说，也是不错的选择。 

![七牛](http://developer.qiniu.com/article/developer/img/upload-with-callback.png)

下面讲解flask作为业务服务器，进行七牛云存储的过程。  


