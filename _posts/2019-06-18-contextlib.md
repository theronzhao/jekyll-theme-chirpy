---
layout: post
title: contextlib模块
Author: TheronZhao
tags: Python标准库
---


# contextlib模块

**上下文管理器**

任何实现了 __enter__() 和 __exit__() 方法的对象都可称之为上下文管理器，上下文管理器对象可以使用 with 关键字。__enter__() 方法返回资源对象，这里就是你将要打开的那个文件对象，__exit__() 方法处理一些清除工作。

```python
class File():

    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode

    def __enter__(self):
        print("entering")
        self.f = open(self.filename, self.mode)
        return self.f

    def __exit__(self, *args):
        print("will exit")
        self.f.close()

# 调用
with File('out.txt', 'w') as f:
    print("writing")
    f.write('hello, python') 
```

**@contextmanager**

编写__enter__和__exit__仍然很繁琐，因此Python的标准库contextlib提供了更简单的写法。

通过 yield 将函数分割成两部分，yield 之前的语句在 __enter__ 方法中执行，yield 之后的语句在 __exit__ 方法中执行。紧跟在 yield 后面的值是函数的返回值。   

```python
from contextlib import contextmanager

@contextmanager
def my_open(path, mode):
    f = open(path, mode)
    yield f
    f.close()   

# 调用
with my_open('out.txt', 'w') as f:
    f.write("hello , the simplest context manager")
```

**@closing**

如果一个对象没有实现上下文，我们就不能把它用于with语句。这个时候，可以用closing()来把该对象变为上下文对象。例如，用with语句使用urlopen()：

```python
from contextlib import closing
from urllib.request import urlopen

with closing(urlopen('https://www.python.org')) as page:
    for line in page:
        print(line)
```

# 