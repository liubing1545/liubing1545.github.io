---
title: gevent的异步方式
date: 2018-05-23 17:23:11
tags: [gevent, 多并发]
---

## 介绍
gevent是基于协程的高性能Python网络库，相比Twisted、Stackless等，gevent使用libev事件循环，因此速度很快、性能很好，使用greenlet提供高层的同步API，因此非常轻量。    
作者曾经是另一个Python网络库Eventlet的开发者之一，2009年因 为Eventlet无法满足项目需要，他决定另外开发一个更轻量的库，这就是gevent。某种意义上，gevent是从Eventlet项目派生出来的。    

## 异步方式
* gevent对标准I/O函数做了猴子补丁，把它们变成了异步。
* 并且gevent有greenlet对象能够被用于并发执行。

### greenlet
greenlet是一种协程。gevent的调度器在I/O等待期间使用一个事件循环在所有的greenlets间来回切换，而不是用多个CPU来运行它们。    
gevent通过使用wait函数来设法尽可能透明化地处理事件循环。wait函数将启动一个事件循环，直到所有的greenlets结束。    
greenlet由gevent.spawn(func, para)来创建，func是需要执行的函数，para是func的参数。一旦声明的函数执行完成，它的值就会包含在greenlet的value域中。    

### 猴子补丁
Monkey Patch就是在运行时对已有的代码进行修改，达到hot patch的目的。

## 例子
```python
from gevent import monkey; monkey.patch_all()
import gevent
import requests

def f(url):
	print('GET: %s' % url)
	headers = {'User-Agent': 'Mozilla/5.0'}
	r = requests.get(url, headers = headers)
	print('%d bytes received from %s.' % (r.status_code, url))
 

gevent.joinall([
	gevent.spawn(f, 'https://www.baidu.com'), 
	gevent.spawn(f, 'https://www.sina.com'),
	gevent.spawn(f, 'http://www.douban.com')
])

```
结果：
```shell
(venv2) E:\github\concurrent>python gevent2.py
GET: https://www.baidu.com
GET: https://www.sina.com
GET: http://www.douban.com
200 bytes received from https://www.baidu.com.
200 bytes received from http://www.douban.com.
200 bytes received from https://www.sina.com.
```
## 其他
在部署web app的时候，用一个支持gevent的WSGI服务器，立刻就获得了数倍的性能提升。
