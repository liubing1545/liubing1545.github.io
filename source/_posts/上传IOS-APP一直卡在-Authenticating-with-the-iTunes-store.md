---
title: 上传IOS APP一直卡在'Authenticating with the iTunes store'
date: 2017-09-29 13:32:42
tags: [app, ios]
---
### 打开终端：
```shell
cd ~
mv .itmstransporter/ .old_itmstransporter/
"/Applications/Xcode.app/Contents/Applications/Application Loader.app/Contents/itms/bin/iTMSTransporter"
```
当Transporter更新完成之后，再上传ios app就好了。
[参考stackoverflow](https://stackoverflow.com/questions/22443425/application-loader-stuck-at-authenticating-with-the-itunes-store-when-uploadin/40423739#40423739)