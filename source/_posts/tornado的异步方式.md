---
title: tornado的异步方式
date: 2018-05-22 17:44:11
tags: [tornado, 多并发]
---

## 介绍
Tornado是Facebook主要为HTTP客户端和服务器端开发的异步I/O包。Tornado主要应对的是I/O密集型应用，Tornado可以使用协程和回调两种方式。    
Tornado 优秀的大并发处理能力得益于它的 web server 从底层开始就自己实现了一整套基于 epoll 的单线程异步架构（其他 python web 框架的自带 server 基本是基于 wsgi 写的简单服务器，并没有自己实现底层结构）。

## epoll
ioloop 的实现基于 epoll ，那么什么是 epoll？ epoll 是Linux内核为处理大批量文件描述符而作了改进的 poll 。    
那么什么又是 poll ？ 首先，我们回顾一下， socket 通信时的服务端，当它接受（ accept ）一个连接并建立通信后（ connection ）就进行通信，而此时我们并不知道连接的客户端有没有信息发完。 这时候我们有两种选择：
1. 一直在这里等着直到收发数据结束；
2. 每隔一定时间来看看这里有没有数据；

第二种办法要比第一种好一些，多个连接可以统一在一定时间内轮流看一遍里面有没有数据要读写，看上去我们可以处理多个连接了，这个方式就是 poll / select 的解决方案。     看起来似乎解决了问题，但实际上，随着连接越来越多，轮询所花费的时间将越来越长，而服务器连接的 socket 大多不是活跃的，所以轮询所花费的大部分时间将是无用的。为了解决这个问题， epoll 被创造出来，它的概念和 poll 类似，不过每次轮询时，他只会把有数据活跃的 socket 挑出来轮询，这样在有大量连接时轮询就节省了大量时间。

## tornado.ioloop
ioloop 实际上是对 epoll 的封装，并加入了一些对上层事件的处理和 server 相关的底层处理。

## 协程

```python
from tornado import gen

@gen.coroutine
def fetch_coroutine(url):
    http_client = AsyncHTTPClient()
    response = yield http_client.fetch(url)
    #为了返回值，python2中需要生成一个特殊的异常；在python3.3以后直接用return
    raise gen.Return(response.body)
```
* @gen.coroutine 和 yield 搭配使用，代表的是协成。
* yield之后挂起耗时操作，耗时操作完成时从挂起的位置接着往下执行。
* Tornado的协成是由python的generators来支持的。

## 回调

```python
from tornado.httpclient import AsyncHTTPClient

def asynchronous_fetch(url, callback):
    http_client = AsyncHTTPClient()
    def handle_response(response):
        callback(response.body)
    http_client.fetch(url, callback=handle_response)
```
* http_client第二个参数callback指定了回调函数。
* 多重的显式的回调容易引起“回调地狱”。
