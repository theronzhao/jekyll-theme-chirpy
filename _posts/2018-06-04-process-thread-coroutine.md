---
title: 进程·线程·协程
date: 2018-06-04 21:26:44 +0800
categories: [计算机基础]
tags: [网络通信,计算机基础,Python进阶]
---
# 多任务编程

多任务执行方式有两种方式：**并发**和**并行**

## 并发

指的是任务数多余cpu核数，通过操作系统的各种任务调度算法，实现用多个任务“一起”执行（实际上总有一些任务不在执行，因为切换任务的速度相当快，看上去一起执行而已）

## 并行

指的是任务数小于等于cpu核数，即任务真的是一起执行的

# 多任务-进程

进程是操作系统进行资源分配的基本单位，  
一个程序运行后至少有一个进程，一个进程默认有一个线程，进程里面可以创建多个线程，线程是依附在进程里面的，没有进程就没有线程。

## Process进程类的说明

1.Process([group [, target [, name [, args [, kwargs]]]]])  
    target：如果传递了函数的引用，可以任务这个子进程就执行这里的代码  
    args：给target指定的函数传递的参数，以元组或者列表的方式传递  
    kwargs：给target指定的函数传递命名参数  
    name：给进程设定一个名字，可以不设定  
    group：指定进程组，大多数情况下用不到  

2.Process创建的实例对象的常用方法:  
    start()：启动子进程实例（创建子进程）  
    join([timeout])：是否等待子进程执行结束，或等待多少秒，起到阻塞作用  
    terminate()：不管任务是否完成，立即终止子进程  
    is_alive()：判断进程子进程是否还在活着  

3.Process创建的实例对象的常用属性:  
    name：当前进程的别名，默认为Process-N，N为从1开始递增的整数  
	daemon：设置守护主进程，默认为False  
    pid：获取当前进程的编号  

## 进程需要注意的点

1.进程之间不共享全局变量  
    创建子进程会对主进程资源进行拷贝，也就是说子进程是主进程的一个副本，好比是一对双胞胎，之所以进程之间不共享全局变量， 是因为操作的不是同一个进程里面的全局变量，只不过不同进程里面的全局变量名字相同而已。  

2.主进程会等待所有的子进程执行结束再结束的小结  
  **为了保证子进程能够正常的运行，即使主进程比子进程先执行完，主进程会等所有的子进程执行完成以后再销毁，设置守护主进程的目的是主进程退出子进程销毁，不让主进程再等待子进程去执行。
  **设置守护主进程方式：   
    子进程对象.daemon = True  
    销毁子进程方式： 子进程对象.terminate()  

## 代码示例

```python
import multiprocessing
import time
import os

"""
    os.getpid() 表示获取当前进程编号
    os.getppid() 表示获取当前父进程编号
"""

lst = []


def aaa(num):
    print("aaa：", os.getpid())

    # 获取当前进程名:multiprocessing.current_process()
    print(multiprocessing.current_process())
    print("aaa父进程：", os.getppid())
    for i in range(num):
        lst.append(i)
        print('---aaa---')
        time.sleep(0.5)
    print(lst)


# def bbb(num):
# print("bbb：", os.getpid())
# print(multiprocessing.current_process())
# print("bbb父进程：", os.getppid())
# for i in range(num):
#     lst.append(i)
#     print('---bbb---')
#     time.sleep(1)
# print(lst)


if __name__ == '__main__':
    aaa_process = multiprocessing.Process(target=aaa, name='aaa_process', args=(5,))
    # bbb_process = multiprocessing.Process(target=bbb, name='bbb_process', kwargs={'num': 5})

    print('开始')
    print('主进程', os.getpid())
    print('主进程的父进程：', os.getppid(), multiprocessing.current_process())

    # 守护主进程主进程退出，子进程直接销毁。守护主进程必须在子进程开启之前设置
    aaa_process.daemon = True
    aaa_process.start()
    time.sleep(1)

    # aaa_process.terminate()   # 让子进程销毁
    # aaa_process.join()
    # bbb_process.start()
    # time.sleep(0.9)
    print(lst)
    print('结束')
    exit()  # 主进程退出
```

## 进程间通信-Queue

Python的`multiprocessing`模块包装了底层的机制，提供了`Queue`、`Pipes`等多种方式来实现多进程之间的数据传递。

**1. Queue的使用**

以`Queue`为例，Queue本身是一个消息列队程序。

初始化Queue()对象时（例如：q=Queue()），若括号中没有指定最大可接收的消息数量，或数量为负值，那么就代表可接受的消息数量没有上限（直到内存的尽头）；

Queue 模块中的常用方法:

- Queue.qsize() 返回队列的大小
- Queue.empty() 如果队列为空，返回True,反之False
- Queue.full() 如果队列满了，返回True,反之False
- Queue.FULL 与 maxsize 大小对应
- Queue.put(item,[block[, timeout]])：将item消息写入队列，block默认值为True；
- Queue.put_nowait(item) 相当Queue.put(item, False)

1）如果block使用默认值，且没有设置timeout（单位秒），消息列队如果已经没有空间可写入，此时程序将被阻塞（停在写入状态），直到从消息列队腾出空间为止，如果设置了timeout，则会等待timeout秒，若还没空间，则抛出"Queue.Full"异常；

2）如果block值为False，消息列队如果没有空间可写入，则会立刻抛出"Queue.Full"异常；

- Queue.task_done() 在完成一项工作之后，Queue.task_done()函数向任务已经完成的队列发送一个信号
- Queue.join() 实际上意味着等到队列为空，再执行别的操作
- Queue.get([block[, timeout]])：获取队列中的一条消息，然后将其从列队中移除，block默认值为True；
- Queue.get_nowait() 相当Queue.get(False)

1）如果block使用默认值，且没有设置timeout（单位秒），消息列队如果为空，此时程序将被阻塞（停在读取状态），直到从消息列队读到消息为止，如果设置了timeout，则会等待timeout秒，若还没读取到任何消息，则抛出"Queue.Empty"异常；

2）如果block值为False，消息列队如果为空，则会立刻抛出"Queue.Empty"异常；

首先用一个小实例来演示一下Queue的工作原理：

```python
#coding=utf-8
from multiprocessing import Queue
q=Queue(3) #初始化一个Queue对象，最多可接收三条put消息
q.put("消息1") 
q.put("消息2")
print(q.full())  #False
q.put("消息3")
print(q.full()) #True

#因为消息列队已满下面的try都会抛出异常，第一个try会等待2秒后再抛出异常，第二个Try会立刻抛出异常
try:
    q.put("消息4",True,2)
except:
    print("消息列队已满，现有消息数量:%s"%q.qsize())

try:
    q.put_nowait("消息4")
except:
    print("消息列队已满，现有消息数量:%s"%q.qsize())

#推荐的方式，先判断消息列队是否已满，再写入
if not q.full():
    q.put_nowait("消息4")

#读取消息时，先判断消息列队是否为空，再读取
if not q.empty():
    for i in range(q.qsize()):
        print(q.get_nowait())
        
        
# 执行结果
False
True
消息列队已满，现有消息数量:3
消息列队已满，现有消息数量:3
消息1
消息2
消息3
```

**2. Queue实例**

在父进程中创建两个子进程，一个往Queue里写数据，一个从Queue里读数据：

```python
from multiprocessing import Process, Queue
import os, time, random

# 写数据进程执行的代码:
def write(q):
    for value in ['A', 'B', 'C']:
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())

# 读数据进程执行的代码:
def read(q):
    while True:
        if not q.empty():
            value = q.get(True)
            print('Get %s from queue.' % value)
            time.sleep(random.random())
        else:
            break

if __name__=='__main__':
    # 父进程创建Queue，并传给各个子进程：
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    # 启动子进程pw，写入:
    pw.start()    
    # 等待pw结束:
    pw.join()
    # 启动子进程pr，读取:
    pr.start()
    pr.join()
    # pr进程里是死循环，无法等待其结束，只能强行终止:
    print('')
    print('所有数据都写入并且读完')
```

## 进程池 Pool

如果要启动大量的子进程，可以用进程池的方式批量创建子进程

初始化Pool时，可以指定一个最大进程数，当有新的请求提交到Pool中时，如果池还没有满，那么就会创建一个新的进程用来执行该请求；但如果池中的进程数已经达到指定的最大值，那么该请求就会等待，直到池中有进程结束，才会用之前的进程来执行新的任务

multiprocessing.Pool常用函数解析：

- apply_async(func[, args[, kwds]]) ：使用非阻塞方式调用func（并行执行，堵塞方式必须等待上一个进程退出才能执行下一个进程），args为传递给func的参数列表，kwds为传递给func的关键字参数列表；
- close()：关闭Pool，使其不再接受新的任务；
- terminate()：不管任务是否完成，立即终止；
- join()：主进程阻塞，等待子进程的退出， 必须在close或terminate之后使用；

```python
# -*- coding:utf-8 -*-
from multiprocessing import Pool
import os, time, random

def worker(msg):
    t_start = time.time()
    print("%s开始执行,进程号为%d" % (msg,os.getpid()))
    # random.random()随机生成0~1之间的浮点数
    time.sleep(random.random()*2) 
    t_stop = time.time()
    print(msg,"执行完毕，耗时%0.2f" % (t_stop-t_start))

po = Pool(3)  # 定义一个进程池，最大进程数3
for i in range(0,10):
    # Pool().apply_async(要调用的目标,(传递给目标的参数元祖,))
    # 每次循环将会用空闲出来的子进程去调用目标
    po.apply_async(worker,(i,))

if __name__ == '__main__':
    print("----start----")
    po.close()  # 关闭进程池，关闭后po不再接收新的请求
    po.join()  # 等待po中所有子进程执行完成，必须放在close语句之后
    print("-----end-----")


# 运行结果:
----start----
0开始执行,进程号为21466
1开始执行,进程号为21468
2开始执行,进程号为21467
0 执行完毕，耗时1.01
3开始执行,进程号为21466
2 执行完毕，耗时1.24
4开始执行,进程号为21467
3 执行完毕，耗时0.56
5开始执行,进程号为21466
1 执行完毕，耗时1.68
6开始执行,进程号为21468
4 执行完毕，耗时0.67
7开始执行,进程号为21467
5 执行完毕，耗时0.83
8开始执行,进程号为21466
6 执行完毕，耗时0.75
9开始执行,进程号为21468
7 执行完毕，耗时1.03
8 执行完毕，耗时1.05
9 执行完毕，耗时1.69
-----end-----
```

**进程池中的Queue**

如果使用Pool创建进程，通信就需要使用multiprocessing.Manager()中的Queue()，而不是multiprocessing.Queue()

```python
# -*- coding:utf-8 -*-

# 修改import中的Queue为Manager
from multiprocessing import Manager,Pool
import os,time,random

def reader(q):
    print("reader启动(%s),父进程为(%s)" % (os.getpid(), os.getppid()))
    for i in range(q.qsize()):
        print("reader从Queue获取到消息：%s" % q.get(True))

def writer(q):
    print("writer启动(%s),父进程为(%s)" % (os.getpid(), os.getppid()))
    for i in "itcast":
        q.put(i)

if __name__=="__main__":
    print("(%s) start" % os.getpid())
    q = Manager().Queue()  # 使用Manager中的Queue
    po = Pool()
    po.apply_async(writer, (q,))

    time.sleep(1)  # 先让上面的任务向Queue存入数据，然后再让下面的任务开始从中取数据

    po.apply_async(reader, (q,))
    po.close()
    po.join()
    print("(%s) End" % os.getpid())
    
    
    
# 运行结果:

(11095) start
writer启动(11097),父进程为(11095)
reader启动(11098),父进程为(11095)
reader从Queue获取到消息：i
reader从Queue获取到消息：t
reader从Queue获取到消息：c
reader从Queue获取到消息：a
reader从Queue获取到消息：s
reader从Queue获取到消息：t
(11095) End    
```

# 多任务-线程

线程是进程中执行代码的一个分支，每个执行分支（线程）要想工作执行代码需要cpu进行调度 ，也就是说线程是cpu调度的基本单位，每个进程至少都有一个线程，而这个线程就是我们通常说的主线程

## 线程类Thread参数说明

Thread([group [, target [, name [, args [, kwargs]]]]])  
    group: 线程组，目前只能使用None  
    target: 执行的目标任务名  
    args: 以元组的方式给执行任务传参  
    kwargs: 以字典方式给执行任务传参  
    name: 线程名，一般不用设置  

```python
#coding=utf-8
import threading
import time

class MyThread(threading.Thread):
    def run(self):
        for i in range(3):
            time.sleep(1)
            msg = "I'm "+self.name+' @ '+str(i)
            print(msg)
def test():
    for i in range(5):
        t = MyThread()
        t.start()
if __name__ == '__main__':
    test()
```

threading.Thread类有一个run方法，用于定义线程的功能函数，可以在自己的线程类中覆盖该方法。而创建自己的线程实例后，通过Thread类的start方法，可以启动该线程，交给python虚拟机进行调度，当该线程获得执行的机会时，就会调用run方法执行线程。



## 线程的注意点介绍

1.线程之间执行是无序的  
  	线程之间执行是无序的，它是由cpu调度决定的 ，cpu调度哪个线程，哪个线程就先执行，没有调度的线程不能执行。  
      进程之间执行也是无序的，它是由操作系统调度决定的，操作系统调度哪个进程，哪个进程就先执行，没有调度的进程不能执行。  

2.主线程会等待所有的子线程执行结束再结束  
    守护主线程:守护主线程就是主线程退出子线程销毁不再执行  
    设置守护主线程有两种方式：  
        threading.Thread(target=show_info, daemon=True)  
        线程对象.setDaemon(True)     

3.线程之间共享全局变量  
    好处是可以对全局变量的数据进行共享，坏处是可能会导致数据出现错误问题  
    全局变量数据错误的解决办法:  
    线程同步: 保证同一时刻只能有一个线程去操作全局变量 同步: 就是协同步调，按预定的先后次序进行运行。线程同步的两种方式：线程等待(join)和互斥锁
       

## 线程等待代码示例

```python
import threading
import time

lst = []


def aaa(num1):
    # 获取当前线程名
    print('aaa线程:', threading.current_thread())

    for i in range(num1):
        lst.append(i)
        print('---aaa---', i)
        time.sleep(0.5)
    print(lst)


def bbb(num2):
    # 获取当前线程名
    print('bbb线程:', threading.current_thread())

    for i in range(num2):
        print('---bbb---', i)
        time.sleep(0.5)
    print(lst)


if __name__ == '__main__':
    # 设置守护主线程
    # aaa_thread = threading.Thread(target=aaa, name='aaa_thread', args=(6,)， daemon=True)
    aaa_thread = threading.Thread(target=aaa, name='aaa_thread', args=(6,))
    bbb_thread = threading.Thread(target=bbb, name='bbb_thread', kwargs={'num2': 6})

    print('开始')
    print('主线程：', threading.current_thread())
    
    print(threading.enumerate())  # 返回一个线程列表

    # aaa_thread.setDaemon(True)  # 设置守护主线程
    aaa_thread.start()
    bbb_thread.start()

    time.sleep(0.5)
    print(lst)
    print('结束')
```

## 互斥锁

1.互斥锁的概念  
对共享数据进行锁定，保证同一时刻只能有一个线程去操作。  
互斥锁是多个线程一起去抢，抢到锁的线程先执行，没有抢到锁的线程需要等待，等互斥锁使用完释放后，其它等待的线程再去抢这个锁。  

2.互斥锁的使用  
acquire和release方法之间的代码同一时刻只能有一个线程去操作  
如果在调用acquire方法的时候 其他线程已经使用了这个互斥锁，那么此时acquire方法会堵塞，直到这个互斥锁释放后才能再次上锁。  

```python 
# 创建锁
mutex = threading.Lock()

# 上锁
mutex.acquire()

...这里编写代码能保证同一时刻只能有一个线程去操作, 对共享数据进行锁定...

# 释放锁
mutex.release()
```

**示例代码**

```python
import threading

lock = threading.Lock()
global_num = 0


def add_num1():
    lock.acquire()
    global global_num
    for i in range(1000000):
        global_num += 1
    print(threading.current_thread(), global_num)
    lock.release()


def add_num2():
    lock.acquire()
    global global_num
    for i in range(1000000):
        global_num += 1
    print(threading.current_thread(), global_num)
    lock.release()


if __name__ == '__main__':
    num1_thread = threading.Thread(target=add_num1, name='add_num1_thread')
    num2_thread = threading.Thread(target=add_num2, name='add_num2_thread')

    num1_thread.start()
    num2_thread.start()

    print(threading.current_thread(), global_num)
```

3.总结：  
互斥锁的作用就是保证同一时刻只能有一个线程去操作共享数据，保证共享数据不会出现错误问题  
使用互斥锁的好处确保某段关键代码只能由一个线程从头到尾完整地去执行  
使用互斥锁会影响代码的执行效率，多任务改成了单任务执行  
互斥锁如果没有使用好容易出现**死锁**的情况

## 死锁

  若干子线程在资源竞争时,都在等待对方对某部分资源解除占用状态,结果是谁也不愿先解锁,互相干等着,程序无法执行下去,这就是死锁.

```python
import threading
import time

# 创建互斥锁
lock = threading.Lock()


# 根据下标去取值， 保证同一时刻只能有一个线程去取值
def get_value(index):

    # 上锁
    lock.acquire()
    print(threading.current_thread())
    my_list = [3,6,8,1]
    # 判断下标释放越界
    if index >= len(my_list):
        print("下标越界:", index)
        # lock.release() 需要加一行代买释放锁
        return  # 这里退出函数了，函数执行已经结束，但是没有释放锁，造成了死锁
    value = my_list[index]
    print(value)
    time.sleep(0.2)
    # 释放锁
    lock.release()


if __name__ == '__main__':
    # 模拟大量线程去执行取值操作
    for i in range(30):
        sub_thread = threading.Thread(target=get_value, args=(i,))
        sub_thread.start()
```

## 可重入锁

为了支持在同一线程中多次请求同一资源，python提供了“可重入锁”：threading.RLock。RLock内部维护着一个Lock和一个counter变量，counter记录了acquire的次数，从而使得资源可以被多次require。直到一个线程所有的acquire都被release，其他的线程才能获得资源。

# 多任务-协程

协程，又称微线程，纤程。英文名Coroutine。

协程是python个中另外一种实现多任务的方式，只不过比线程更小占用更小执行单元（理解为需要的资源）

通俗的理解：在一个线程中的某个函数，可以在任何地方保存当前函数的一些临时变量等信息，然后切换到另外一个函数中执行，注意不是通过调用函数的方式做到的，并且切换的次数以及什么时候再切换到原来的函数都由开发者自己确定

## 特点

1. 无需线程上下文切换的开销  
2. 无需原子操作锁定及同步的开销
3. 方便切换控制流，简化编程模型
4. 无法利用多核资源：协程的本质是个单线程,它不能同时将 单个CPU 的多个核用上,协程需要和进程配合才能运行在多CPU上.

## 协程-yield

```python
import time

def work1():
    while True:
        print("----work1---")
        yield
        time.sleep(0.5)

def work2():
    while True:
        print("----work2---")
        yield
        time.sleep(0.5)

def main():
    w1 = work1()
    w2 = work2()
    while True:
        next(w1)
        next(w2)

if __name__ == "__main__":
    main()
    
# 输出
----work1---
----work2---
----work1---
----work2---
----work1---
----work2---
----work1---
等等
```

## 协程-greenlet

```python
# 安装greenlet模块
# sudo pip3 install greenlet

from greenlet import greenlet
import time

def test1():
    while True:
        print "---A--"
        gr2.switch()  # 切换执行点到 greenlet对象 gr2 上
        time.sleep(0.5)

def test2():
    while True:
        print "---B--"
        gr1.switch()  # # 切换执行点到 greenlet对象 gr1 上
        time.sleep(0.5)

gr1 = greenlet(test1)
gr2 = greenlet(test2)

#切换到gr1中运行
gr1.switch()


# 执行结果
---A--
---B--
---A--
---B--
---A--
---B--
...
```

## 协程-gevent

模块gevent更强大的并且能够自动切换任务。

其原理是当一个greenlet遇到IO(指的是input output 输入输出，比如网络、文件操作等)操作时，比如访问网络，就自动切换到其他的greenlet，等到IO操作完成，再在适当的时候切换回来继续执行。

由于IO操作非常耗时，经常使程序处于等待状态，有了gevent为我们自动切换协程，就保证总有greenlet在运行，而不是等待IO

由于切换是在IO操作时自动完成，所以gevent需要修改Python自带的一些标准库，这一过程在启动时通过monkey patch完成：

```python
from gevent import monkey
import gevent
import random
import time

# 有耗时操作时需要
monkey.patch_all()  # 将程序中用到的耗时操作的代码，换为gevent中自己实现的模块

def coroutine_work(coroutine_name):
    for i in range(10):
        print(coroutine_name, i)
        time.sleep(random.random())

gevent.joinall([
        gevent.spawn(coroutine_work, "work1"),
        gevent.spawn(coroutine_work, "work2")
])


# 输出
work1 0
work2 0
work1 1
work1 2
work1 3
work2 1
work1 4
work2 2
work1 5
work2 3
work1 6
work1 7
work1 8
work2 4
work2 5
work1 9
work2 6
work2 7
work2 8
work2 9
```

# 进程、线程、协程区别

## 进程和线程的对比

1. 关系对比  
    线程是依附在进程里面的，没有进程就没有线程。  
    一个进程默认提供一条线程，进程可以创建多个线程。  
2. 区别对比  
    进程之间不共享全局变量  
    线程之间共享全局变量，但是要注意资源竞争的问题，解决办法: 互斥锁或者线程同步  
    创建进程的资源开销要比创建线程的资源开销要大   
    进程是操作系统资源分配的基本单位，线程是CPU调度的基本单位  
    线程不能够独立执行，必须依存在进程中  
    多进程开发比单进程多线程开发稳定性要强  
3. 优缺点对比  
    进程优缺点:  
        优点：可以用多核  
        缺点：资源开销大  
    线程优缺点:  
        优点：资源开销小  
        缺点：不能使用多核  

## 线程和协程的比较

- 极高的执行效率：因为子程序切换不是线程切换，而是由程序自身控制，因此，没有线程切换的开销，和多线程比，线程数量越多，协程的性能优势就越明显；
- 不需要多线程的锁机制：因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多。
- 多进程、多线程根据cpu核数不一样可能是并行的，但是协程是在一个线程中 所以是并发

## 简单总结

1. 进程是资源分配的单位

2. 线程是操作系统调度的单位

3. 进程切换需要的资源很最大，效率很低

4. 线程切换需要的资源一般，效率一般（当然了在不考虑GIL的情况下）

5. 协程切换任务资源很小，效率高

6. 多进程、多线程根据cpu核数不一样可能是并行的，但是协程是在一个线程中 所以是并发


# 进程、线程和协程的使用场景

## 进程

多进程适合在**CPU 密集**型操作(cpu 操作指令比较多，计算密集型，如科学计算，位数多的浮点运算)
计算密集型任务的特点是要进行大量的计算，消耗CPU资源，比如计算圆周率、对视频进行高清解码等等，全靠CPU的运算能力。这种计算密集型任务虽然也可以用多任务完成，但是任务越多，花在任务切换的时间就越多，CPU执行任务的效率就越低，所以，要最高效地利用CPU，计算密集型任务同时进行的数量应当等于CPU的核心数。
计算密集型任务由于主要消耗CPU资源，因此，代码运行效率至关重要。Python这样的脚本语言运行效率很低，完全不适合计算密集型任务。对于计算密集型任务，最好用C语言编写。

## 线程

多线程适合在**IO 密集**型操作(读写数据操作较多的，比如爬虫)
IO密集型，涉及到网络、磁盘IO的任务都是IO密集型任务，这类任务的特点是CPU消耗很少，任务的大部分时间都在等待IO操作完成（因为IO的速度远远低于CPU和内存的速度）。对于IO密集型任务，任务越多，CPU效率越高，但也有一个限度。常见的大部分任务都是IO密集型任务，比如Web应用。
IO密集型任务执行期间，99%的时间都花在IO上，花在CPU上的时间很少，因此，用运行速度极快的C语言替换用Python这样运行速度极低的脚本语言，完全无法提升运行效率。对于IO密集型任务，最合适的语言就是开发效率最高（代码量最少）的语言，脚本语言是首选，C语言最差。

## 协程

Python中的多线程是假的多线程，协程+线程=真的多线程

# GIL锁

## GIL面试题

> 描述Python GIL的概念， 以及它对python多线程的影响？编写一个多线程抓取网页的程序，并阐明多线程抓取程序是否可比单线程性能有提升，并解释原因。

## 参考答案

1. GIL并不是Python的特性，它是在实现Python解析器(CPython)时所引入的一个概念。
2. GIL：全局解释器锁。每个线程在执行的过程都需要先获取GIL，保证同一时刻只有一个线程可以执行代码。
3. 线程释放GIL锁的情况： 在IO操作等可能会引起阻塞的system call之前,可以暂时释放GIL,但在执行完毕后,必须重新获取GIL Python 3.x使用计时器（执行时间达到阈值15ms后，当前线程释放GIL）或Python 2.x，tickets计数达到100
4. Python使用多进程是可以利用多核的CPU资源的。
5. 多线程爬取比单线程性能有提升，因为遇到IO阻塞会自动释放GIL锁

## 总结

Python GIL其实是功能和性能之间权衡后的产物，它尤其存在的合理性，也有较难改变的客观因素。从本分的分析中，我们可以做以下一些简单的总结：

- 因为GIL的存在，只有IO Bound场景下得多线程会得到较好的性能
- 如果对并行计算性能较高的程序可以考虑把核心部分也成C模块，或者索性用其他语言实现
- GIL在较长一段时间内将会继续存在，但是会不断对其进行改进
- 可以使用多进程+协程