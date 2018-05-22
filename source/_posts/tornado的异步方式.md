---
title: tornado的异步方式
date: 2018-05-22 17:44:11
tags: [tornado, 多并发]
---

## 介绍
Tornado是Facebook主要为HTTP客户端和服务器端开发的异步I/O包。Tornado主要应对的是I/O密集型应用，Tornado可以使用协成和回调两种方式。

## 协成

```python
from tornado import gen

@gen.coroutine
def fetch_coroutine(url):
    http_client = AsyncHTTPClient()
    response = yield http_client.fetch(url)
    #为了返回值，python2中需要生成一个特殊的异常；在python3.3以后直接用return
    raise gen.Return(response.body)
```
* **@gen.coroutine**和**yield**搭配使用，代表的是协成。
* **yield**之后挂起耗时操作，耗时操作完成时从挂起的位置接着往下执行。
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
