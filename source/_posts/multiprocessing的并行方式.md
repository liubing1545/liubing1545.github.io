---
title: multiprocessing的并行方式
date: 2018-05-24 10:35:11
tags: [multiprocessing, 并行]
---

## 介绍
multiprocessing模块让你使用基于进程和基于线程的并行处理，在队列上共享任务，以及在进程间共享数据。它主要是集中于单机多核的并行（对多机并行来说，有更好的选择）。    
一个很普遍的用法就是针对CPU密集型的问题，在一个进程集上并行化一个任务。

## 并行运算
* 避免共享状态会让并行运算变得简单很多。
* 每一个进程需要和其他进程来通信的话，那么通信开销将减慢整体的性能。

Python中的线程是OS原生的，它们被全局解释锁(GIL)所束缚，所以同一时刻只有一个线程可以被交互。因此Python都会并行一定数量的进程，每一个进程都有私有的内存空间与GIL。如果需要共享状态，就需要增加一些通信的开销。

## multiprocessing模块
* 用进程或者池对象来并行化一个CPU密集型任务。
* 用dummy模块在线程池中并行化一个I/O密集型任务。
* 由队列来共享任务。
* 在并行工作者之间共享状态，包括字节、原生数据类型、字典和列表。

### 进程
一个当前进程的forked拷贝，创建了一个新的进程标识符，在OS中以一个独立的子进程运行。
### 池
包装了进程和线程。在工作者线程池中共享了工作块并返回聚合后的结果。
### 队列
一个FIFP（先进先出）的队列允许多个生产者和消费者。
### 管理者
一个单向或双向的在两个进程间的通信渠道。
### ctypes
允许在进程派生后，在父子进程间共享原生数据类型（整数型、浮点型和字节型）
### 同步
锁和信号量在进程间同步。


## 例子
```python
import time
from multiprocessing import Process
from multiprocessing.pool import ThreadPool, Pool


def slp(a):
    print "received: %s" % (a)
    time.sleep(2)
    return a

if __name__ == '__main__':
	pool = Pool(processes=4)
	start_time = time.clock()
	d_l = []
	p_list = []
	for i in range(0, 4):
		async_result = pool.apply_async(slp, [i,])
		p_list.append(async_result)
		# d_l.append(async_result.get())
	pool.close()
	pool.join()
	for p in p_list:
		d_l.append(p.get())
	# p_list = []
	# for i in range(0, 4):
	#     p = Process(target=slp)
	#     p.start()
	#     p_list.append(p)
	# for p in p_list:
	#     p.join()
	end_time = time.clock()
	duration = round(end_time - start_time, 2)
	print 'time: %s' % duration
	print d_l

```
结果：
```shell
received: 0
received: 1
received: 2
received: 3
time: 2.12
[0, 1, 2, 3]
```
