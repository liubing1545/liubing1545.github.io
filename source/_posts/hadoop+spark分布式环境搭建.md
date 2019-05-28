---
title: hadoop+spark分布式环境搭建
date: 2019-05-27 18:50:11
tags: [hadoop, 大数据]
---

## Hadoop整体框架
Hadoop由HDFS、MapReduce、HBase、Hive和ZooKeeper等成员组成，其中最基础最重要的两种组成元素为底层用于存储集群中所有存储节点文件的文件系统HDFS（Hadoop Distributed File System）和上层用来执行MapReduce程序的MapReduce引擎。

### HDFS
HDFS架构采用主从架构（master/slave）。一个典型的HDFS集群包含一个NameNode节点和多个DataNode节点。NameNode节点负责整个HDFS文件系统中的文件的元数据保管和管理，集群中通常只有一台机器上运行NameNode实例，DataNode节点保存文件中的数据，集群中的机器分别运行一个DataNode实例。在HDFS中，NameNode节点被称为名称节点，DataNode节点被称为数据节点。DataNode节点通过心跳机制与NameNode节点进行定时的通信。

### Map/Reduce
MapReduce是一种编程模型，用于大规模数据集的并行运算。Map（映射）和Reduce（化简），采用分而治之思想，先把任务分发到集群多个节点上，并行计算，然后再把计算结果合并，从而得到最终计算结果。多节点计算，所涉及的任务调度、负载均衡、容错处理等，都由MapReduce框架完成，不需要编程人员关心这些内容。

## 环境准备
准备了3台CentOS 6.10 64bit云服务器，1台做master NameNode主节点，2台做DataNode节点。

### 修改hosts文件
在3台服务器上的/etc/hosts，追加以下配置：
```shell
192.168.0.189 master worker0 namenode
192.168.0.233 worker1 datanode1
192.168.0.117 worker2 datanode2
```
统一hosts文件，可以让主机通过host名字来识别彼此。    
**<label style="color:red">ip为内网ip，不能配公网ip，不然hadoop启动异常。9000端口未出现在监听中</label>**

### ssh互信(免密登录)
在master节点执行以下：
```shell
ssh-keygen -t rsa -P '' #生成公钥
scp /root/.ssh/id_rsa.pub root@worker1:/root/.ssh/id_rsa.pub.master #从master节点拷贝id_rsa.pub到worker主机上,并且改名为id_rsa.pub.master
scp /root/.ssh/id_rsa.pub root@worker2:/root/.ssh/id_rsa.pub.master
cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
```
在node节点执行以下：
```shell
cat /root/.ssh/id_rsa.pub.master >> /root/.ssh/authorized_keys
```

## 下载工具
* Java 1.8.0_211
* Hadoop 3.1.2
* Spark 2.4.3
* Scala 2.11.12

### Java安装
```shell
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u112-b15/jdk-8u112-linux-x64.rpm
rpm ivh jdk-8u112-linux-x64.rpm
```

### Hadoop安装
```shell
wget http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-3.1.2/hadoop-3.1.2.tar.gz
tar -xvf hadoop-3.1.2.tar.gz
mv hadoop-3.1.2 /opt
```

### Spark安装
```shell
wget http://mirrors.tuna.tsinghua.edu.cn/apache/spark/spark-2.4.3/spark-2.4.3-bin-hadoop2.7.tgz
tar -xvf spark-2.4.3-bin-hadoop2.7.tgz
mv spark-2.4.3-bin-hadoop2.7 /opt
```

### Scala安装
```shell
wget -O "scala-2.12.8.rpm" "https://downloads.lightbend.com/scala/2.12.8/scala-2.12.8.rpm"
rpm ivh scala-2.12.8.rpm
```

## 配置
配置

### 设定环境变量
在/etc/profile中添加
```shell
#java home
export JAVA_HOME=/usr/java/jdk1.8.0_211-amd64/
#scala home
export SCALA_HOME=/usr/share/scala
#hadoop enviroment
export HADOOP_HOME=/opt/hadoop-3.1.2/
export PATH="$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH"
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export YARN_CONF_DIR=$HADOOP_HOME/etc/hadoop
#spark enviroment
export SPARK_HOME=/opt/spark-2.4.3-bin-hadoop2.7/
export PATH="$SPARK_HOME/bin:$PATH"
```
然后
```shell
source /etc/profile
```

### 配置Hadoop
在**$HADOOP_HOME/etc/hadoop/hadoop-env.sh**中配置如下
```shell
export JAVA_HOME=/usr/java/jdk1.8.0_211-amd64/
```

在**$HADOOP_HOME/etc/hadoop/workers**中配置如下
```shell
worker1
worker2
```

在**$HADOOP_HOME/etc/hadoop/core-site.xml**中配置如下
```shell
<configuration>
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://master:9000</value>
        </property>
        <property>
         <name>io.file.buffer.size</name>
         <value>131072</value>
       </property>
        <property>
                <name>hadoop.tmp.dir</name>
                <value>/opt/hadoop-3.1.2/tmp</value>
        </property>
</configuration>
```

在**$HADOOP_HOME/etc/hadoop/hdfs-site.xml**中配置如下
```shell
<configuration>
    <property>
      <name>dfs.namenode.secondary.http-address</name>
      <value>master:50090</value>
    </property>
    <property>
      <name>dfs.replication</name>
      <value>2</value>
    </property>
    <property>
      <name>dfs.namenode.name.dir</name>
      <value>file:/opt/hadoop-3.1.2/hdfs/name</value>
    </property>
    <property>
      <name>dfs.datanode.data.dir</name>
      <value>file:/opt/hadoop-3.1.2/hdfs/data</value>
    </property>
</configuration>
```

```shell
cp mapred-site.xml.template mapred-site.xml
```

在**$HADOOP_HOME/etc/hadoop/mapred-site.xml**中配置如下
```shell
<configuration>
 <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
  <property>
          <name>mapreduce.jobhistory.address</name>
          <value>master:10020</value>
  </property>
  <property>
          <name>mapreduce.jobhistory.address</name>
          <value>master:19888</value>
  </property>
</configuration>
```

在**$HADOOP_HOME/etc/hadoop/yarn-site.xml**中配置如下
```shell
<!-- Site specific YARN configuration properties -->
     <property>
          <name>yarn.nodemanager.aux-services</name>
          <value>mapreduce_shuffle</value>
     </property>
     <property>
           <name>yarn.resourcemanager.address</name>
           <value>master:8032</value>
     </property>
     <property>
          <name>yarn.resourcemanager.scheduler.address</name>
          <value>master:8030</value>
      </property>
     <property>
         <name>yarn.resourcemanager.resource-tracker.address</name>
         <value>master:8031</value>
     </property>
     <property>
         <name>yarn.resourcemanager.admin.address</name>
         <value>master:8033</value>
     </property>
     <property>
         <name>yarn.resourcemanager.webapp.address</name>
         <value>master:8088</value>
     </property>
```

格式化**namenode**
```shell
hadoop namenode -format
```
**<label style="color:red">若多次格式化namenode，需先清空hadoop/tmp下文件，再格式化</label>**

### 配置Spark
```shell
cp spark-env.sh.template spark-env.sh
```

在**$SPARK_HOME/conf/spark-env.sh**中配置如下
```shell
export SCALA_HOME=/usr/share/scala
export JAVA_HOME=/usr/java/jdk1.8.0_211-amd64/
export SPARK_MASTER_IP=master
export SPARK_WORKER_MEMORY=1g
export HADOOP_CONF_DIR=/opt/hadoop-3.1.2/etc/hadoop
```

```shell
cp slaves.template slaves
```
在**$SPARK_HOME/conf/slaves**文件中配置如下
```shell
master
worker1
worker2
```

## 启动/关闭集群
启动脚本start-cluster.sh如下
```shell
#!/bin/bash
echo -e "\033[31m ========Start The Cluster======== \033[0m"
echo -e "\033[31m Starting Hadoop Now !!! \033[0m"
/opt/hadoop-3.1.2/sbin/start-all.sh
echo -e "\033[31m Starting Spark Now !!! \033[0m"
/opt/spark-2.4.3-bin-hadoop2.7/sbin/start-all.sh
echo -e "\033[31m The Result Of The Command \"jps\" :  \033[0m"
jps
echo -e "\033[31m ========END======== \033[0m"
```

关闭脚本stop-cluster.sh如下
```shell
#!/bin/bash
echo -e "\033[31m ===== Stoping The Cluster ====== \033[0m"
echo -e "\033[31m Stoping Spark Now !!! \033[0m"
/opt/spark-2.4.3-bin-hadoop2.7/sbin/stop-all.sh
echo -e "\033[31m Stopting Hadoop Now !!! \033[0m"
/opt/hadoop-3.1.2/sbin/stop-all.sh
echo -e "\033[31m The Result Of The Command \"jps\" :  \033[0m"
jps
echo -e "\033[31m ======END======== \033[0m"
```

### JPS查看进程
master节点，jps查看进程信息如下
```shell
20069 Jps
8118 NameNode
9096 Worker
8392 SecondaryNameNode
9003 Master
8655 ResourceManager
```

node节点，jps查看进程信息如下
```shell
4210 DataNode
7362 Jps
4328 NodeManager
```

## 问题总结

### could only be written to 0 of the 1 minReplication nodes.
这个问题是由于使用hadoop namenode -format 格式化多次，导致spaceID不一致造成的，解决方法如下：
```shell
bash stop-cluster.sh
rm -rf /tmp/*
rm -rf /opt/hadoop-3.1.2/tmp/*
hadoop namenode -format
bash start-cluster.sh
```

### ipc.Client: Retrying connect to server: localhost/127.0.0.1
配置logger为debugger模式
```shell
export HADOOP_ROOT_LOGGER=DEBUG,console
```
查看具体报错信息

### Unable to load native-hadoop library
去hadoop文件夹里找到libhadoop文件
```shell
ldd libhadoop.so
```

### libc.so.6 version GLIBC2.14 not found
查看系统glibc版本
```shell
strings /lib64/libc.so.6 |grep GLIBC_
```
如果列表中系统支持的glibc的最高版本是GLIBC_2.12，则做升级
```shell
wget http://ftp.gnu.org/gnu/glibc/glibc-2.14.tar.gz
tar zxf glibc-2.14.tar.gz
cd glibc-2.14 && mkdri build
cd build && ../configure --prefix=/opt/glibc-2.14
make && make install
```
安装成功之后
```shell
rm -rf /lib64/libc.so.6
ln -s /opt/glibc-2.14/lib/libc-2.14.so /lib64/libc.so.6
# 如果出问题，执行下面
LD_PRELOAD=/opt/glibc-2.14/lib/libc-2.14.so ln -s /opt/glibc-2.14/lib/libc-2.14.so /lib64/libc.so.6
```

### There are 0 datanode(s) running and no node(s) are excluded in this operation
这个错误是node没有正常设置。其中有可能涉及两个原因：
* 主机在/etc/hosts里配置的是公网ip，改成内网ip。
* hadoop在master和node中的配置保持一样。