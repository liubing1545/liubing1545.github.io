---
title: win10启动vagrant
date: 2017-07-18 16:37:55
tags: [vagrant,win10]
---
win10环境下vagrant启动会报错。    
## 正常启动步骤：
### 设置virtual box的adapter网卡    
打开Preferences -> Network -> Host-only Networks Tab    

* 将默认的adapter网卡的ipv4的地址，改写成192.168.xx.1
* 将mask改为255.255.255.0

### 启动vagrant

    $ vagrant up 
    
如果启动时报错    
    
    Bringing machine 'default' up with 'virtualbox' provider...
    There was an error while executing `VBoxManage`, a CLI used by Vagrant
    for controlling VirtualBox. The command and stderr is shown below.

    Command: ["hostonlyif", "create"]

    Stderr: 0%...
    Progress state: E_FAIL
    VBoxManage.exe: error: Failed to create the host-only adapter
    VBoxManage.exe: error: Code E_FAIL (0x80004005) - Unspecified error (extended info not available)
    VBoxManage.exe: error: Context: "int __cdecl handleCreate(struct HandlerArg ,int,int )" at line 68 of file VBoxManageHostonly.cpp
    
1. check下virtual box有没有创建一个新的adapter网卡。
2. check该网卡的ipv4的地址是以192.168开头的。
3. 如果成功创建了，则再次vagrant up。就启动成功了。