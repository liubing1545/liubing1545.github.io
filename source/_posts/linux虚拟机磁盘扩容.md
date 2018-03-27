---
title: linux虚拟机磁盘扩容
date: 2017-03-28 18:04:05
tags: [linux,磁盘扩容]
---
### 问题点
安装了centos7的虚拟机，yum update时报错空间不足 **No space left on device**。
### VBoxManage modifyhd
在宿主机上的安装virtualBox的根目录执行resize命令进行扩容:    

    C:\Program Files\Oracle\VirtualBox>VBoxManage modifyhd E:\dockerVM\dockerVM.vdi --resize 35000

### CentOS的LVM管理
查看磁盘状况

    $ fdisk -l /dev/sda
将空余磁盘创建为SDA3

    $ fdisk /dev/sda
    n {new partition}
    p {primary partition}
    3 {partition number}
    
    t {change partition id}
    3 {partition number}
    8e {linux LVM partition}
    w
    
重启虚拟机

    $reboot
查看当前Volume group

    $ vgdisplay
创建/dev/sda3，根据VG Name:[centos]，扩展LVM的逻辑卷

    $ lvscan
    $ pvcreate /dev/sda3
    $ vgextend /dev/centos/root /dev/sda3
  
调整逻辑卷文件系统的大小

    $ xfs_growfs /dev/centos/root
    $df -h
OK了！