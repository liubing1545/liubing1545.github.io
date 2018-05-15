---
title: celery介绍
date: 2018-05-10 14:47:11
tags: [celery, 分布式]
---
## Celery架构设计
Celery是一个用python编写的分布式的任务调度模块，它有着简明的 API，并且有丰富的扩展性，适合用于构建分布式的 Web 服务。celery的架构包括三个部分：消息中间件（message broker），任务执行单元（worker）和任务执行结果的存储（task result store）。
#### 消息中间件
celery本身不提供消息服务，但可以和第三方提供的消息中间件（**RabbitMQ**,**Redis**等）集成。
#### 任务执行单元
worker是celery提供的任务执行单元，worker并发的运行在分布式的系统节点中。
#### 任务结果存储
task result store用来存储worker执行的任务的结果，celery支持以不同方式存储任务的结果（**AMQP**,**Redis**,**MongoDB**）。

另外，celery还支持不同的并发（**Prefork**,**Eventlet**,**gevent**,**threads**）和序列化的手段（**pickle**,**json**,**yaml**等）。

## 概图
![celery](http://obksgg9lx.bkt.clouddn.com/celery.png)

## 安装
```shell
pip install celery
pip install redis
```

## 应用

#### 消息中间件redis的配置,及任务结果存储
```python
CELERY_BROKER_URL = 'redis://localhost:6379/0'
CELERY_RESULT_BACKEND = 'redis://localhost:6379/0'
# 使用 update设置多个celery的参数
celery.conf.update(app.config)
```

#### 消费者task 
```python
@celery.task(bind=True)
def long_task(self):
    # 执行耗费资源的算法
    algorithm.reset_data()

    app = create_app(os.getenv('FLASK_CONFIG') or 'default')
    with app.app_context():
        front_data = algorithm.run_search(algorithm.TestData.requirement, algorithm.TestData.disabled_devices)
        import json
        
        data = json.dumps(front_data)
        return {'current': 100, 'total': 100, 'status': 'Task completed!', 'result': data}
```

#### 通过task.apply_async将任务压入消息队列broker中
```python
@energy_island.route('/get_graph_data', methods=['POST'])
@login_required
def get_graph_data():
    id = request.values.get('id')
    print(id)

    task = tasks.long_task.apply_async()
    return jsonify({}), 202, {'Location': url_for('energy_island.taskstatus', task_id=task.id)}
```

#### 使用backend异步获取结果task.AsyncResult
```python
@energy_island.route('/status/<task_id>')
def taskstatus(task_id):
    task = tasks.long_task.AsyncResult(task_id)
    if task.state == 'PENDING':
        response = {
            'state': task.state,
            'current': 0,
            'total': 1,
            'status': 'Pending...'
        }
    elif task.state != 'FAILURE':
        response = {
            'state': task.state,
            'current': task.info.get('current', 0),
            'total': task.info.get('total', 1),
            'status': task.info.get('status', ' ')
        }
        if 'result' in task.info:
            response['result'] = task.info['result']
    else:
        response = {
            'state': task.state,
            'current': 1,
            'total': 1,
            'status': str(task.info),
        }
    return jsonify(response)
```

#### 生产者get消费者完成的状态
```javascript
var update_progress = function(status_url) {

    // send GET request to status URL
    $.getJSON(status_url, function(data) {
        // update UI
        percent = parseInt(data['current'] * 100 / data['total']);
        if (data['state'] != 'PENDING' && data['state'] != 'PROGRESS') {
            if ('result' in data) {

                setGraphDataRun(JSON.parse(data['result']), 1);
                // show result
                alert('result-liubing: ' + data['result']);
            }
            else {
                alert('state-liubing: ' + data['state']);
            }
        }
        else {
            // rerun in 2 seconds
            setTimeout(function() {
                update_progress(status_url);
            }, 2000);
        }
    });
}
```

#### 启动celery
```shell
celery -A app.celery worker -l info -P eventlet
```
