---
title: 迭代器和生成器
date: 2018-05-05 20:23:49 +0800
categories: [Python,Python进阶]
tags: [Python进阶]
---


## 迭代器

可以直接作用于for循环的对象统称为**可迭代对象**：Iterable

可以使用isinstance()判断一个对象是否是Iterable对象

```python
In [50]: from collections import Iterable

In [51]: isinstance([], Iterable)
Out[51]: True
    
In [55]: isinstance(100, Iterable)
Out[55]: False
```



可以被next()函数调用并不断返回下一个值的对象称为**迭代器**：Iterator

可以使用isinstance()判断一个对象是否是Iterator对象

```python
In [56]: from collections import Iterator

In [57]: isinstance([], Iterator)
Out[57]: False

In [58]: isinstance(iter([]), Iterator)
Out[58]: True
```

### 可迭代对象的本质

每迭代一次（即在for...in...中每循环一次）都会返回对象中的下一条数据，一直向后读取数据直到迭代了所有数据后结束。那么，在这个过程中就应该有一个“人”去记录每次访问到了第几条数据，以便每次迭代都可以返回下一条数据。我们把这个能帮助我们进行数据迭代的“人”称为**迭代器(Iterator)**。

可迭代对象的本质就是可以向我们提供一个这样的中间“人”即迭代器帮助我们对其进行迭代遍历使用。

可迭代对象通过`__iter__`方法向我们提供一个迭代器，我们在迭代一个可迭代对象的时候，实际上就是先获取该对象提供的一个迭代器，然后通过这个迭代器来依次获取对象中的每一个数据.

把一个类作为一个迭代器使用需要在类中实现两个方法 _\_iter_\_() 与 _\_next_\_():

_\_iter_\_() 方法返回一个特殊的迭代器对象， 这个迭代器对象实现了 _\_next_\_() 方法并通过 StopIteration 异常标识迭代的完成。

_\_next_\_() 方法会返回下一个迭代器对象。

StopIteration 异常用于标识迭代的完成，防止出现无限循环的情况

```python
# 创建一个迭代器
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    if self.a <= 20:
      x = self.a
      self.a += 1
      return x
    else:
      raise StopIteration
      
myclass = MyNumbers()
myiter = iter(myclass)
print(next(myiter))  # 输出1
print(next(myiter))  # 输出2
```

### 自定义可迭代对象

```python
>>> class MyList(object):
...     def __init__(self):
...             self.container = []
...     def add(self, item):
...             self.container.append(item)
...     def __iter__(self):
...             """返回一个迭代器"""
...             # 我们暂时忽略如何构造一个迭代器对象
...             pass
...
>>> mylist = MyList()
>>> from collections import Iterable
>>> isinstance(mylist, Iterable)
True
>>>
# 这回测试发现添加了__iter__方法的mylist对象已经是一个可迭代对象了
```



迭代器是一个可以记住遍历的位置的对象。

迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。

迭代器有两个基本的方法：iter() 和 next()。

list、dict、str虽然是Iterable，却不是Iterator。可以使用iter()函数将它们变成迭代器

```python
>>> from collections import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
```

### 自定义迭代器

**要想构造一个迭代器，就要实现它的`__next__`方法**。但这还不够，python要求迭代器本身也是可迭代的，所以我们还要为迭代器实现`__iter__`方法，而`__iter__`方法要返回一个迭代器，迭代器自身正是一个迭代器，所以迭代器的`__iter__`方法返回自身即可。

**一个实现了`__iter__`方法和`__next__`方法的对象，就是迭代器。**

```python
class MyList(object):
    """自定义的一个可迭代对象"""
    def __init__(self):
        self.items = []

    def add(self, val):
        self.items.append(val)

    def __iter__(self):
        myiterator = MyIterator(self)
        return myiterator


class MyIterator(object):
    """自定义的供上面可迭代对象使用的一个迭代器"""
    def __init__(self, mylist):
        self.mylist = mylist
        # current用来记录当前访问到的位置
        self.current = 0

    def __next__(self):
        if self.current < len(self.mylist.items):
            item = self.mylist.items[self.current]
            self.current += 1
            return item
        else:
            raise StopIteration

    def __iter__(self):
        return self


if __name__ == '__main__':
    mylist = MyList()
    mylist.add(1)
    mylist.add(2)
    mylist.add(3)
    mylist.add(4)
    mylist.add(5)
    for num in mylist:
        print(num)
```



Python的Iterator对象表示的是一个数据流，Iterator对象可以被next()函数调用并不断返回下一个数据，直到没有数据时抛出StopIteration错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过next()函数实现按需计算下一个数据，所以Iterator的计算是惰性的，只有在需要返回下一个数据时它才会计算。

### for...in...循环的本质

`for item in Iterable` 循环的本质就是先通过iter()函数获取可迭代对象Iterable的迭代器，然后对获取到的迭代器不断调用next()方法来获取下一个值并将其赋值给item，当遇到StopIteration的异常后循环结束。



## 生成器

### 概念

创建一个包含100万个元素的列表，占用很大的存储空间，生成器可以在需要用到的时候生成一个值给我们，节省空间

在 Python 中，使用了 **yield** 的函数被称为**生成器**（generator）。生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。

在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。

调用一个生成器函数，返回的是一个迭代器对象。

### 创建生成器

**创建生成器-方式1**

```python
第一种方法很简单，只要把一个列表生成式的[]改成()，就创建了一个generator：
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
>>> next(g)
0
```

可以通过next()函数获得generator的下一个返回值，每次调用next(g)，就计算出g的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误。也可以用for 循环遍历

**创建生成器-方式2**

如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator.generator的函数在每次调用next()的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。

```python
def fib(max):
    """斐波那契数列"""
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
```

可以用next()调用生成器对象，也可以用for循环，用for循环调用generator时，拿不到generator的return语句的返回值，如果想要拿到返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中：

```python
>>> g = fib(6)  # g 是一个迭代器，由生成器返回生成
>>> while True:
...     try:
...         x = next(g)
...         print('g:', x)
...     except StopIteration as e:
...         print('Generator return value:', e.value)
...         break
```

