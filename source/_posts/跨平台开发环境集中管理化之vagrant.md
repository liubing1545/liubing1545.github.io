---
title: 开发团队跨平台开发环境集中管理化之vagrant
date: 2016-08-12 09:58:01
tags: [vagrant, 跨平台]
---
**vagrant**是一款用来构建虚拟开发环境的工具，非常适合各类开发语言的web应用，因统一安装本地开发环境浪费的人力成本及时间成本，以及不可避免的“work on my machine”错误，将永久告别～  

## 安装virtualbox及vagrant
vagrant对virtualbox支持的非常好。但不匹配的版本，加载box会发生或多或少的问题。    

我的工作环境的软件版本是：  
vagrant 1.6.3  
virtualbox 4.2.12-84980

## 添加镜像
安装好后运行以下命令可以添加vagrant官方的box镜像。  

    $ vagrant box add hashicorp/precise64
这是一个标准的64bit的ubuntu系统。  
如果要下载其他系统的镜像，可以在这里下载:<a href="https://atlas.hashicorp.com/boxes/search">https://atlas.hashicorp.com/boxes/search</a>   

## 初始化开发环境
切换到box文件所在目录，加载box文件及初始化 
   
    $ cd ~/dev 
    $ vagrant box add test test.box 
    $ vagrant init test
    $ vagrant up

## ssh登录
mac下ssh登录，虚拟机目录 /vagrant 就是宿主机的 ~/dev  
    
    $ vagrant ssh
    $ cd /vagrant
**windows用户注意:** windows终端需要使用ssh客户端，比如putty等。

## 其他设置
vagrant 初始化成功后，会在初始化的目录里生成一个 vagrantfile 的配置文件，可以修改配置文件进行个性化的定制。  

vagrant 默认是使用端口映射的方式将虚拟机的端口映射本地从而实现类似 http://localhost:80 这种访问方式。  
相比之下，host-only模式显得非常方便。打开 vagrantfile，将下面的注释去掉，便可以访问192.168.33.10机器上的服务了。 
 
    config.vm.network "private_network", ip:"192.168.33.10"

## 打包分发
当配置好开发环境后，退出并关闭虚拟机。对开发环境进行打包。
    
    $ vagrant package

打包后，就会在当前目录下生成一个 package.box 的文件。可以分发这个文件给其他开发者。

## 集成预安装
vagrant 还提供预安装定制，打开 vagrantfile, 可以放开这些在文件末尾处有被注释的代码：

    config.vm.provision "shell", inline: <<-SHELL
    	apt-get update
    	apt-get install -y apache2
    SHELL

可以把需要安装的软件应用全部写在里面，在初次 vagrant up 的时候，虚拟机会预先执行这些命令。  

如果不是初次运行，但又修改了这些命令。则可以进行重载vagrant。  

    $ vagrant reload --provision

也可以把这些配置写在shell脚本里面，让vagrant加载运行这些脚本。因此整个团队可以维护一个 vagrantfile 或者 shell 脚本，把这个文件放在github上，还可以监控它的版本，多么简单和容易啊！哈哈

## 常用命令
	$ vagrant init  # 初始化
	$ vagrant up  # 启动
	$ vagrant halt  # 关闭
	$ vagrant reload  # 重启
	$ vagrant ssh  # ssh
	$ vagrant status  # 查看状态
	$ vagrant destroy  # 销毁
