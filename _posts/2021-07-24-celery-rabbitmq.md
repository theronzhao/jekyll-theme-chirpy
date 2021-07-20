---
title: Celery与RabbitMQ
date: 2021-06-24 19:22:56 +0800
categories: [Python应用框架]
tags: [celery,rabbitmq]
---


## 生产者消费者设计模式

在工作中，大家可能会碰到这样一种情况：某个模块负责产生数据，这些数据由另一个模块来负责处理（此处的模块是广义的，可以是类、函数、线程、进程等）。

产生数据的模块，就形象地称为生产者；而处理数据的模块，就称为消费者。

在生产者与消费者之间在加个缓冲区，我们称之为消息队列，生产者负责往消息队列中写入消息，而消费者负责从消息队列里读取消息，这就构成了生产者消费者模型。

![](.\images\批注 2020-08-04 224559.jpg)

**生产者消费者模型的优点：**

**1、解耦**

假设生产者和消费者分别是两个类。

如果让生产者直接调用消费者的某个方法，那么生产者对于消费者就会产生依赖（也就是耦合）。

将来如果消费者的代码发生变化， 可能会影响到生产者。而如果两者都依赖于某个缓冲区，两者之间不直接依赖，耦合也就相应降低了。

**2、支持并发**

由于生产者与消费者是两个独立的并发体，他们之间是用缓冲区作为桥梁连接，生产者只需要往缓冲区里丢数据，

就可以继续生产下一个数据，而消费者只需要从缓冲区了拿数据即可，这样就不会因为彼此的处理速度而发生阻塞。

**3、支持忙闲不均**

缓冲区还有另一个好处。如果制造数据的速度时快时慢，缓冲区的好处就体现出来了。

当数据制造快的时候，消费者来不及处理，未处理的数据可以暂时存在缓冲区中。 等生产者的制造速度慢下来，消费者再慢慢处理掉。

## 分布式队列服务Celery

### 1.celery简介

Celery是一个简单，灵活且可靠的分布式系统，可以处理大量消息，同时为操作提供维护该系统所需的工具。 这是一个任务队列，着重于实时处理，同时还支持任务调度。

- 简单：celery易于使用和维护，不需要配置文件。

- 高可用度：如果连接丢失或者失败，辅助服务器和客户端将自动重试，并且某些代理以主/主或主/从方式支持HA
- 快速：一个celery进程每分钟可以处理数百万个任务，往返延迟不到毫秒

- 灵活：celery几乎所有部分都可以扩展或单独使用。可以自制连接池、序列化、压缩模式、日志、调度器、消费者、生产者、自动扩展、中间人传输或者更多
- 单个 Celery 进程每分钟可处理数以百万计的任务。
- 通过消息进行通信，使用`消息队列（broker）`在`客户端`和`消费者`之间进行协调。

![](.\images\1.jpg)

**Application**：负责产生计算任务，交给任务队列去处理

**Celery Beat**：以独立进程的形式存在，会读取配置文件的内容，周期性地将执行任务的请求发送给任务队列

**Broker**：负责接受任务生产者发送过来的任务处理消息，存进队列之后再进行调度，分发给任务消费方

**Celery Worker**：负责接收任务处理中间方发来的任务处理请求，完成这些任务，并且返回任务处理的结果

**Backend**：处理完后将状态信息和结果的保存，以供查询。框架内置支持 rpc，Redis，RabbitMQ 等方式来保存任务处理后的状态信息

### 2.简单入门celery

安装Celery：

```bash
$ pip install -U Celery
```

```python
from celery import Celery

app = Celery('hello', broker='amqp://guest@localhost//')

@app.task
def hello():
    return 'hello world'
```

**启动Celery服务**

```bash
$ celery -A celery_tasks.main worker -l info

-A指对应的应用程序, 其参数是项目中 Celery实例的位置。
worker指这里要启动的worker。
-l指日志等级，比如info等级。
```

**celery worker的工作模式**

- 默认是进程池方式，进程数以当前机器的CPU核数为参考，每个CPU开四个进程。
- 指定进程数：`celery worker -A your_items --concurrency=4`
- 改变进程池方式为协程方式：`celery worker -A your_items --concurrency=1000 -P eventlet -c 1000`

## RabbitMQ介绍和使用

### 1. RabbitMQ介绍

RabbitMQ是一个遵循AMQP协议的消息中间件，它从生产者接受消息并传递给消费者

AMQP ：Advanced Message Queue，高级消息队列协议。它是应用层协议的一个开放标准，为面向消息的中间件设计，基于此协议的客户端与消息中间件可传递消息，并不受产品、开发语言等条件的限制。

RabbitMQ 最初起源于金融系统，用于在分布式系统中存储转发消息，在易用性、扩展性、高可用性等方面表现不俗。具体特点包括：

1. 可靠性（Reliability）

    RabbitMQ 使用一些机制来保证可靠性，如持久化、传输确认、发布确认。

2. 灵活的路由（Flexible Routing）

    在消息进入队列之前，通过 Exchange 来路由消息的。对于典型的路由功能，RabbitMQ 已经提供了一些内置的 Exchange 来实现。针对更复杂的路由功能，可以将多个 Exchange 绑定在一起，也通过插件机制实现自己的 Exchange 。

3. 消息集群（Clustering）

    多个 RabbitMQ 服务器可以组成一个集群，形成一个逻辑 Broker 。

4. 高可用（Highly Available Queues）

    队列可以在集群中的机器上进行镜像，使得在部分节点出问题的情况下队列仍然可用。

5. 多种协议（Multi-protocol）

    RabbitMQ 支持多种消息队列协议，比如 STOMP、MQTT 等等。

6. 多语言客户端（Many Clients）

    RabbitMQ 几乎支持所有常用语言，比如 Java、.NET、Ruby 等等。

7. 管理界面（Management UI）

    RabbitMQ 提供了一个易用的用户界面，使得用户可以监控和管理消息 Broker 的许多方面。

8. 跟踪机制（Tracing）

    如果消息异常，RabbitMQ 提供了消息跟踪机制，使用者可以找出发生了什么。

9. 插件机制（Plugin System）

    RabbitMQ 提供了许多插件，来从多方面进行扩展，也可以编写自己的插件。

![](.\images\图片2.png)

### 2. 安装RabbitMQ

> **1.安装Erlang**
>
> - 由于 RabbitMQ 是采用 Erlang 编写的，所以需要安装 Erlang 语言库。

```bash
# 1. 在系统中加入 erlang apt 仓库
$ wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb
$ sudo dpkg -i erlang-solutions_1.0_all.deb

# 2. 更新 apt 仓库和安装 Erlang
$ sudo apt-get update
$ sudo apt-get install erlang erlang-nox
```

> **2.安装RabbitMQ**
>
> - 安装成功后，默认就是启动状态。

```bash
# 1. 先在系统中加入 rabbitmq apt 仓库，再加入 rabbitmq signing key
$ echo 'deb http://www.rabbitmq.com/debian/ testing main' | sudo tee /etc/apt/sources.list.d/rabbitmq.list
$ wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | sudo apt-key add -

# 2. 更新 apt 仓库和安装 RabbitMQ
$ sudo apt-get update
$ sudo apt-get install rabbitmq-server
```

```bash
# 重启
$ sudo systemctl restart rabbitmq-server
# 启动
$ sudo systemctl start rabbitmq-server
# 关闭
$ sudo systemctl stop rabbitmq-server
```

> **3.Python访问RabbitMQ**
>
> - RabbitMQ提供默认的administrator账户。
> - 用户名和密码：`guest`、`guest`
> - 协议：`amqp`
> - 地址：`localhost`
> - 端口：`5672`
> - 查看队列中的消息：`sudo rabbitctl list_queues`

```bash
# Python3虚拟环境下，安装pika
$ pip install pika
```

```bash
# 生产者代码：rabbitmq_producer.py
import pika


# 链接到RabbitMQ服务器
credentials = pika.PlainCredentials('guest', 'guest')
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost',5672,'/',credentials))
#创建频道
channel = connection.channel()
# 声明消息队列
channel.queue_declare(queue='test123')
# routing_key是队列名 body是要插入的内容
channel.basic_publish(exchange='', routing_key='test123', body='Hello RabbitMQ!')
print("开始向 'test123' 队列中发布消息 'Hello RabbitMQ!'")
# 关闭链接
connection.close()
```

```python
# 消费者代码：rabbitmq_customer.py 
import pika


# 链接到rabbitmq服务器
credentials = pika.PlainCredentials('guest', 'guest')
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost',5672,'/',credentials))
# 创建频道，声明消息队列
channel = connection.channel()
channel.queue_declare(queue='test123')
# 定义接受消息的回调函数
def callback(ch, method, properties, body):
    print(body)
# 告诉RabbitMQ使用callback来接收信息
channel.basic_consume(callback, queue='test123', no_ack=True)
# 开始接收信息
channel.start_consuming()
```

### 3. 新建administrator用户

```bash
# 新建用户，并设置密码
$ sudo rabbitmqctl add_user admin your_password 
# 设置标签为administrator
$ sudo rabbitmqctl set_user_tags admin administrator
# 设置所有权限
$ sudo rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
# 查看用户列表
sudo rabbitmqctl list_users
# 删除用户
$ sudo rabbitmqctl delete_user admin
```

### 4. RabbitMQ配置远程访问

> **1.准备配置文件**
>
> - 安装好 `RabbitMQ` 之后，在 `/etc/rabbitmq` 目录下面默认没有配置文件，需要单独下载。

```bash
$ cd /etc/rabbitmq/
$ wget https://raw.githubusercontent.com/rabbitmq/rabbitmq-server/master/docs/rabbitmq.config.example
$ sudo cp rabbitmq.config.example rabbitmq.config
```

> **2.设置配置文件**

```bash
$ sudo vim rabbitmq.config

# 设置配置文件结束后，重启RabbitMQ服务端
$ sudo systemctl restart rabbitmq-server
```