---
layout: post
title: with语句和上下文管理器
Author: TheronZhao
tags: Python
---

任何实现了 \_\_enter\_\_() 和 \_\_exit\_\_() 方法的对象都可称之为上下文管理器，上下文管理器对象可以使用 with 关键字。\_\_enter\_\_() 方法返回资源对象，这里就是你将要打开的那个文件对象，\_\_exit\_\_() 方法处理一些清除工作。

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

- `__enter__`表示上文方法，需要返回一个操作文件对象
- `__exit__`表示下文方法，with语句执行完成会自动执行，即使出现异常也会执行该方法

Python 提供了一个 contextmanager 的装饰器，更进一步简化了上下文管理器的实现方式。通过 yield 将函数分割成两部分，yield 之前的语句在 \_\_enter\_\_ 方法中执行，yield 之后的语句在 \_\_exit\_\_ 方法中执行。紧跟在 yield 后面的值是函数的返回值。  

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

