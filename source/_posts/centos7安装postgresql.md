---
title: centos7安装postgresql
date: 2017-12-07 16:13:46
tags: [centos7, postgresql]
---
## 安装启动postgresql

```shell
yum install postgresql-server
service postgresql initdb
service postgresql start
```
## 修改管理员密码

```shell
su - postgres
psql
$ ALTER USER postgres WITH PASSWORD 'postgres';
```

## 配置远程访问

修改**/var/lib/pgsql/data/postgresql.conf**
```shell
listen_addresses = '*'
```

修改客户端认证配置文件**/var/lib/pgsql/data/pg_hba.conf**,添加

```shell
host  all  all   10.0.0.0/8  md5
```

## 重启服务

```shell
service postgresql restart
```
