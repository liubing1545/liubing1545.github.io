---
title: 'Write failed: Broken pipe'
date: 2017-07-05 10:52:07
tags: [aliyun]
---
### Write failed: Broken pipe

ssh远程连接阿里云centos服务器时，隔几分钟不操作，就会报错：    
```
Write failed: Broken pipe   
```

解决方法：    
在/etc/ssh/sshd_config文件中，添加如下配置：　    
```
ClientAliveInterval 60   
```
重启一下就生效了。