---
title: gunicorn与uwsgi项目部署
date: 2019-07-11 15:21:00 +0800
categories: [Web开发]
tags: [Web开发,项目部署]
---

## gunicorn

Gunicorn（绿色独角兽）是一个Python WSGI的HTTP服务器。从Ruby的独角兽（Unicorn ）项目移植。该Gunicorn服务器与各种Web框架兼容，实现非常简单，轻量级的资源消耗。Gunicorn直接用命令启动，不需要编写配置文件

### 安装gunicorn

```
pip install gunicorn
```

**查看命令行选项：** 安装gunicorn成功后，通过命令行的方式可以查看gunicorn的使用信息。

```bash
$ gunicorn -h
```

**直接运行：**

```bash
#直接运行，默认启动的127.0.0.1::8000
gunicorn 运行文件名称:程序实例名
```

**指定进程和端口号：** -w: 表示进程（worker）。 -b：表示绑定ip地址和端口号（bind）。

```bash
$gunicorn -w 4 -b 127.0.0.1:5001 运行文件名称:程序实例名
```

### 运行 flask 程序

```bash
$ gunicorn myproject.main:app

$ gunicorn -b 0.0.0.0:8000 -w 运行的进程数 --access-logfile 记录请求访问的日志文件  --error-logfile 记录gunicorn运行错误的日志 myproject.main:app

# -w 运行的进程数 最多一般采用CPU核心数 或核心数乘以1.2
```

### 运行 django 程序

最简单地，gunicorn 的调用只需要在其调用位置具有一个包含WSGI application 对象的模块，该对象的名称必须为名为application。 所以在一个Django 项目中，调用gunicorn 就像这样：

```bash
$ gunicorn myproject.wsgi
```

它将启动一个进程，它运行一个线程并监听在`127.0.0.1:8000`。 它要求您的项目在Python路径上；最简单的方法是确保从与`manage.py`文件相同的目录运行此命令

在django的项目目录中有一个wsgi.py文件,其内容如下:

```python
import os

from django.core.wsgi import get_wsgi_application

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "malonghui.settings")

application = get_wsgi_application()  # django实例application
```

## uWSGI

`uWSGI`是一个`web`服务器，实现了`WSGI`协议，`uwsgi`协议，`http`协议等。
 `uWSGI`的主要特点是：

- 超快的性能
- 低内存占用
- 多`app`管理
- 详尽的日志功能（可以用来分析`app`的性能和瓶颈）
- 高度可定制（内存大小限制，服务一定次数后重启等）

`uWSGI`服务器自己实现了基于`uwsgi`协议的`server`部分，我们只需要在`uwsgi`的配置文件中指定`application`的地址，`uWSGI`就能直接和应用框架中的`WSGI application`通信。

```python
# uwsgi.ini
[uwsgi]
# 使用nginx连接时使用，Django程序所在服务器地址
socket=127.0.0.1:8001
# 项目目录
chdir=/myproject
# 项目中wsgi.py文件的目录，相对于项目目录
wsgi-file=myproject/wsgi.py
# 进程数
processes=4
# 线程数
threads=2
# uwsgi服务器的角色
master=True
# 存放进程编号的文件
pidfile=uwsgi.pid
```

## 二者对比

Gunicorn是使用Python实现的WSGI服务器, 直接提供了http服务, 并且在woker上提供了多种选择, gevent, eventlet这些都支持, 在多worker最大化里用CPU的同时, 还可以使用协程来提供并发支撑, 对于网络IO密集的服务比较有利.

uWSGI不同于Gunicorn, uWSGI是使用C写的, 它的socket fd创建, worker进程的启动都是使用C语言系统接口来实现的, 在worker进程处理循环中, 解析了http请求后, 使用python的C接口生成environ对象, 再把这个对象作为参数塞到暴露出来的WSGI application函数中调用. 而这一切都是在C程序中进行, 只是在处理请求的时候交给python虚拟机调用application. 完全使用C语言实现的好处是性能会好一些.

除了支持http协议, uWSGI还实现了uwsgi协议, 一般我们会在uWSGI服务器前面使用Nginx作为负载均衡, 如果使用http协议, 请求在转发到uWSGI前已经在Nginx这里解析了一遍, 转发到uWSGI又会重新解析一遍. uWSGI为了追求性能, 设计了uwsgi协议, 在Nginx解析完以后直接把解析好的结果通过uwsgi协议转发到uWSGI服务器, uWSGI拿到请求按格式生成environ对象, 不需要重复解析请求. 如果用Nginx配合uWSGI, 最好使用uwsgi协议来转发请求.