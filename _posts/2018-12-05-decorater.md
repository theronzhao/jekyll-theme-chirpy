---
title: 闭包与装饰器
date: 2018-12-05 19:23:41 +0800
categories: [Python,Python进阶]
tags: [Python进阶]
---

## 闭包

在函数嵌套的前提下，内部函数使用了外部函数的变量，并且外部函数返回了内部函数，我们把这个使用外部函数变量的内部函数称为闭包。

## 闭包的形成条件

 - 在函数嵌套(函数里面再定义函数)的前提下

 - 内部函数使用了外部函数的变量(还包括外部函数的参数)

 - 外部函数返回了内部函数

```python
# 定义一个外部函数
def func_out(num1):

    # 定义一个内部函数
    def func_inner(num2):
        nonlocal num1  # 告诉解释器，此处使用的是 外部变量a
        # 修改外部变量num1
        num1 = 10
        # 内部函数使用了外部函数的变量(num1)
        result = num1 + num2
        print("结果是:", result)

    print(num1)
    func_inner(1)
    print(num1)

    # 外部函数返回了内部函数，这里返回的内部函数就是闭包
    return func_inner
```

## 装饰器

就是给已有函数增加额外功能的函数，它本质上就是一个闭包函数。

```python
# 装饰器的基本雏形
def decorator(fn): # fn:目标函数.
    def inner():
        '''执行函数之前'''
        fn() # 执行被装饰的函数
        '''执行函数之后'''
    return inner

def myfunc():
    print("执行")

# 使用装饰器来装饰函数
myfunc = decorator(myfunc)
myfunc()
```

每次都需要编写func = decorator(func)这样代码对已有函数进行装饰，这种做法还是比较麻烦。简单的写法: @装饰器名字

```python
@decorator  # 等于myfunc = decorator(myfunc)，在加载模块时就会执行装饰器
def myfunc():
    print("执行")
myfunc()
```

## 装饰带有参数的闭包函数(通用)

```python
# 通用装饰器
def logging(fn):
  def inner(*args, **kwargs):
      print("--正在努力计算--")
      result = fn(*args, **kwargs)
      return result

  return inner
```

## 多个装饰器的使用

多个装饰器的装饰过程是，离函数最近的装饰器先装饰，然后外面的装饰器再进行装饰，由内到外的装饰过程

```python
def make_div(func):
    """对被装饰的函数的返回值 div标签"""
    print(1)
    def inner(*args, **kwargs):
        print(2)
        return "<div>" + func() + "</div>"
    return inner

def make_p(func):
    """对被装饰的函数的返回值 p标签"""
    print(3)
    def inner(*args, **kwargs):
        print(4)
        return "<p>" + func() + "</p>"
    return inner

# 装饰过程: 1 content = make_p(content) 2 content = make_div(content)
# content = make_div(make_p(content))
@make_div
@make_p
def content():
    return "人生苦短"

result = content()

print(result)

# 注意
# 执行顺序是  3，1，2，4
```

## 带有参数的装饰器

带有参数的装饰器就是使用装饰器装饰函数的时候可以传入指定参数，语法格式: @装饰器(参数,...)

装饰器只能接收一个参数，并且还是函数类型。

可以在装饰器外面再包裹上一个函数，让最外面的函数接收参数，返回的是装饰器，因为@符号后面必须是装饰器实例。

```python
# 添加输出日志的功能
def logging(flag):

    def decorator(fn):
        def inner(num1, num2):
            if flag == "+":
                print("--正在努力加法计算--")
            elif flag == "-":
                print("--正在努力减法计算--")
            result = fn(num1, num2)
            return result
        return inner

    # 返回装饰器
    return decorator


# 使用装饰器装饰函数
@logging("+")
def add(a, b):
    result = a + b
    return result
```

## 类装饰器

```python
class Check(object):
    def __init__(self, fn):
        # 初始化操作在此完成
        self.__fn = fn

    # 实现__call__方法，表示对象是一个可调用对象，可以像调用函数一样进行调用。
    def __call__(self, *args, **kwargs):
        # 添加装饰功能
        print("请先登陆...")
        self.__fn()

@Check
def comment():
    print("发表评论")

comment()
```

@Check 等价于 comment = Check(comment), 所以需要提供一个init方法，并多增加一个fn参数。

要想类的实例对象能够像函数一样调用，需要在类里面使用\_\_call\_\_方法，把类的实例变成可调用对象(callable)，也就是说可以像调用函数一样进行调用。

在\_\_call\_\_方法里进行对fn函数的装饰，可以添加额外的功能。