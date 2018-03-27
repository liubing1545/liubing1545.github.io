---
title: curl测试restful服务
date: 2016-07-19 16:00:06
tags: [curl,restful]
---
利用curl，可以很方便的测试restful服务，发送HTTP GET，POST，PUT，DELETE请求。也可以改变HTTP header来指定特别条件。

### curl参数
> -X/--request [GET|POST|PUT|DELETE|...]  <mark>指定http request方式</mark>    
> -H/--header                             <mark>设定request请求的header</mark>  
> -i/--include                            <mark>显示response响应的header</mark>  
> -d/--data                               <mark>设定request的参数</mark>  
> -v/--verbose                            <mark>输出更多的信息</mark>  
> -u/--user                               <mark>使用者账号，密码</mark>  
> -b/--cookie                             <mark>cookie</mark>

### GET
    $ curl -i http://localhost:5000/rest/api/v1.0/tasks

### POST
    $ curl -i -H "Content-Type: application/json" -X POST -d '{"title":"learn python"}' http://localhost:5000/rest/api/v1.0/tasks  

### PUT
    $ curl -i -H "Content-Type: application/json" -X PUT -d '{"title":"learn nodejs"}' http://localhost:5000/rest/api/v1.0/tasks/2

### DELETE
    $ curl -i -X DELETE http://localhost:5000/rest/api/v1.0/tasks/1
  