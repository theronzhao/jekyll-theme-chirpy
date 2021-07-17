---
layout: post
title: 元类
Author: TheronZhao
tags: Python
---


## 使用type创建类

type还有一种完全不同的功能，动态的创建类。

type可以接受一个类的描述作为参数，然后返回一个类。（要知道，根据传入参数的不同，同一个函数拥有两种完全不同的用法是一件很傻的事情，但这在Python中是为了保持向后兼容性）

type可以像这样工作：

type(类名, 由父类名称组成的元组(针对继承的情况，可以为空)，包含属性的字典(名称和值))

```python
# 示例
class A(object):
    num = 100

def print_b(self):
    """准备的实例方法"""
    print(self.num)

@staticmethod
def print_static():
    """准备的静态方法"""
    print("----haha-----")

@classmethod
def print_class(cls):
    """准备的类方法"""
    print(cls.num)

# 如果添加属性，那么加的都是类属性
B = type("B", (A,), {"print_b": print_b, "print_static": print_static, "print_class": print_class})
b = B()
b.print_b()
b.print_static()
b.print_class()
# 结果
# 100
# ----haha-----
# 100
```

元类就是用来创建类的“东西”。元类就是用来创建这些类（对象）的，元类就是类的类，你可以这样理解为：

```python
MyClass = MetaClass() # 使用元类创建出一个对象，这个对象称为“类”
my_object = MyClass() # 使用“类”来创建出实例对象
```

函数type实际上是一个元类。type就是Python在背后用来创建所有类的元类，str是用来创建字符串对象的类，而int是用来创建整数对象的类。type就是创建类对象的类。Python中所有的东西，注意，我是指所有的东西——都是对象。这包括整数、字符串、函数以及类。它们全部都是对象，而且它们都是从一个类创建而来，这个类就是type。

```python
>>> age = 35
>>> age.__class__
<type 'int'>
>>> a.__class__.__class__
<type 'type'>

>>> class Bar(object): pass
>>> b = Bar()
>>> b.__class__
<class '__main__.Bar'>
>>> b.__class__.__class__
<type 'type'>
```

## \_\_metaclass\_\_属性

也可以创建自己的元类，在定义一个类的时候为其添加\_\_metaclass\_\_属性。

```python
class Foo(Bar):
    __metaclass__ = something…
    ...省略...
```

Python做了如下的操作：

- Foo中有\_\_metaclass\_\_这个属性吗？如果是，Python会通过\_\_metaclass\_\_创建一个名字为Foo的类(对象)

- 如果Python没有找到\_\_metaclass\_\_，它会继续在Bar（父类）中寻找\_\_metaclass\_\_属性，并尝试做和前面同样的操作。

- 如果Python在任何父类中都找不到\_\_metaclass\_\_，它就会在模块层次中去寻找\_\_metaclass\_\_，并尝试做同样的操作。

- 如果还是找不到\_\_metaclass\_\_,Python就会用内置的type来创建这个类对象。

现在的问题就是，你可以在\_\_metaclass\_\_中放置些什么代码呢？答案就是：可以创建一个类的东西。那么什么可以用来创建一个类呢？type，或者任何使用到type或者子类化type的东东都可以。

## 自定义元类

元类的主要目的就是为了当创建类时能够自动地改变类,假设模块里所有的类的属性都应该是大写形式，案例如下，

```python
class UpperAttrMetaClass(type):
    # __new__ 是在__init__之前被调用的特殊方法
    # __new__是用来创建对象并返回之的方法
    # 而__init__只是用来将传入的参数初始化给对象
    # 你很少用到__new__，除非你希望能够控制对象的创建
    # 这里，创建的对象是类，我们希望能够自定义它，所以我们这里改写__new__
    # 如果你希望的话，你也可以在__init__中做些事情
    # 还有一些高级的用法会涉及到改写__call__特殊方法，但是我们这里不用
    def __new__(cls, class_name, class_parents, class_attr):
        # 遍历属性字典，把不是__开头的属性名字变为大写
        new_attr = {}
        for name, value in class_attr.items():
            if not name.startswith("__"):
                new_attr[name.upper()] = value

        # 方法1：通过'type'来做类对象的创建
        return type(class_name, class_parents, new_attr)

        # 方法2：复用type.__new__方法
        # 这就是基本的OOP编程，没什么魔法
        # return type.__new__(cls, class_name, class_parents, new_attr)

# python3的用法
class Foo(object, metaclass=UpperAttrMetaClass):
    bar = 'bip'

# python2的用法
# class Foo(object):
#     __metaclass__ = UpperAttrMetaClass
#     bar = 'bip'


print(hasattr(Foo, 'bar'))
# 输出: False
print(hasattr(Foo, 'BAR'))
# 输出:True

f = Foo()
print(f.BAR)
# 输出:'bip'
```
