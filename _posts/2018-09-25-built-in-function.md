---
title: python3内置函数
date: 2018-09-25 22:16:07 +0800
categories: [Python]
tags: [Python]
---

abs(number)  
返回数字的绝对值  

all(iterable)  
如果 iterable 的所有元素都为真值，就返回 True ；否则返回 False  

any(iterable)  
如果 iterable 的所有元素都为假值，就返回 False ；否则返回 True 

ascii(object)  
ascii() 函数类似 repr() 函数, 返回一个表示对象的字符串, 但是对于字符串中的非 ASCII 字符则返回通过 repr() 函数使用 \x, \u 或 \U 编码的字符。  

bin(integer)  
将整数转换为以字符串表示的二进制字面量  

bool(x)  
将 x 解读为布尔值，并返回 True 或 False  

bytearray([string,[encoding[,errors]]] )  
创建一个 bytearray ，可根据指定的字符串给它赋值，还可指定编码和错误处理方式  

bytes([string, [encoding[, errors]]] )  
类似于 bytearray ，但返回一个可修改的 bytes 对象  

callable(object)  
检查对象是否是可调用的.callable() 函数用于检查一个对象是否是可调用的。如果返回 True，object仍然可能调用失败；但如果返回 False，调用对象 object 绝对不会成功。对于函数、方法、lambda函式、 类以及实现了 __call__ 方法的类实例, 它都返回 True。  

chr(number)  
返回一个字符，其Unicode码点为指定的数字  

classmethod(func)  
根据实例方法创建一个类方法  

complex(real[, imag] )  
返回一个复数，其实部和虚部分别为指定的值,例如complex(1,2) -->  1+2j  

dict([mapping-or-sequence] )  
创建一个字典。可根据另一个映射或 (key, value) 列表来创建，也可使用关键字参数来调用  

dir([object] )  
列出当前可见作用域中的（大部分）命令，或列出指定对象的（大部分）属性  

divmod(a, b)  
返回 (a // b, a % b)  （对于浮点数，有一些特殊规则）,python3该函数不支持复数  

enumerate(iterable)  
迭代 iterable 中所有项的 (index, item) 。可提供关键字参数 start ，以便不从开头开始迭代  

eval(string[, globals[, locals]])  
计算以字符串表示的表达式，还可在指定的全局和局部作用域内进行  

float(object)  
将字符串或数字转换为浮点数  

format(value[, format_spec])  
返回对指定字符串设置格式后的结果。格式设置规范的作用与字符串方法 format 中相同  

frozenset([iterable])  
创建一个不可修改的集合，这意味着可将其添加到其他集合中  

globals()  
返回一个表示当前全局作用域的字典  

help([object])  
调用内置的帮助系统，或打印有关指定对象的帮助信息  

hex(number)  
将数字转换为十六进制字符串  

id(object)  
返回指定对象的独一无二的ID  

input([prompt])  
以字符串的方式返回用户输入的数据，还可显示指定的提示语  

int(object[, radix])  
将字符串或数字转换为整数，还可指定进制单位,默认十进制  

isinstance(object, classinfo)  
检查 object 是否是 classinfo 的实例，其中参数 classinfo 可以是类对象、类型对象或类和类型对象元组  

issubclass(class1, class2)  
检查 class1 是否是 class2 的子类（每个类都被视为是它自己的子类）  

iter(object[, sentinel])  
返回一个迭代器对象，即 object.__iter__() 。这个迭代器对象用于迭代序列（如果 object 支持 __getitem__ ）。如果指定了 sentinel ，这个迭代器将不断调用 object ，直到返回的是 sentinel  

len(object)  
返回指定对象的长度（包含的项数）  

list([sequence])  
创建一个列表，也可根据指定的序列创建列表  

locals()  
返回一个表示当前局部作用域的字典（请不要修改这个字典）  

max(object1, [object2, ...])  
如果 object1 不是空序列，就返回其中最大的元素；否则返回提供的参数（ object1 、 object2 等）中最大的那个  

min(object1, [object2, ...])  
如果 object1 不是空序列，就返回其中最小的元素；否则返回提供的参数（ object1 、 object2 等）中最小的那个  

next(iterator[, default])  
返回 iterator.__next__() 的值，还可指定默认值，它指定在到达了迭代器末尾时将返回的值  

object()  
返回一个 object 实例； object 是所有新式类的基类  

oct(number)  
将整数转换为八进制字符串  

open(filename[, mode[, bufsize]])  
打开一个文件并返回一个文件对象（还有其他的可选参数，如指定编码和错误处理方式的参数）  

ord(char)  
返回指定字符的Unicode码点  

pow(x, y[, z])  
返回 x 的 y 次方，还可将结果对 z 求模  

print(x, ...)  
将0个或更多参数作为一行打印到标准输出，并用空格分隔参数。可使用关键字参数 sep 、 end 、 file 和 flush 调整这种行为  

property([fget[, fset[, fdel[, doc]]]])  
根据一组存取函数创建一个特性。  

range([start, ]stop[, step])  
根据参数 start （包含，默认为0）、 stop （不包含）和 step （默认为1）以序列的方式返回指定范围内的一系列值  

repr(object)  
返回对象的字符串表示，通常用作 eval 的参数,返回的是解释器易读的，类似于"hello\n",str()返回的是用户易读的，会将"\n"翻译成换行  

reversed(sequence)  
返回一个反向迭代序列的迭代器  

round(float[, n])  
将指定的浮点数圆整到小数点后 n 位（默认为零位）。关于详尽的圆整规则，请参阅官方文档  

set([iterable])  
返回一个集合；如果指定了 iterable ， 该集合的元素将是从中取得的  

staticmethod(func)  
根据实例方法创建一个静态（类）方法  

str(object)  
返回指定对象的格式良好的字符串表示  

sum(iterable[, start])  
计算数字序列中所有元素的总和;iterable -可迭代对象，如：列表、元组、集合,start -指定起始相加的参数，如果没有设置这个值，默认为0,然后返回结果  

super([type[, obj/type]])  
返回一个将方法调用委托给超类的代理  

tuple([sequence])  
创建一个元组，如果指定了可选参数 sequence ，该元组包含的项将与该参数指定的序列相同  

type(object）  
返回指定对象的类型  

type(name, bases, dict)  
返回一个新的类型对象，其名称、基类和作用域由相应的参数指定  

vars([object])  
返回一个表示局部作用域的字典或一个包含指定对象的属性的字典（请不要修改这个字典）  

zip(sequence1, ...)  
返回一个元组迭代器，其中每个元组都包含提供序列的相应项。返回的列表与提供的最短序列等长  

map(function, sequence, ...)  
接收两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回。  
```python
>>> def f(x):
...     return x * x
...
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

reduce(function, sequence)  
把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算  
```python
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> reduce(fn, [1, 3, 5, 7, 9])
13579
```

filter(function, sequence)  
用于过滤序列。接收一个函数和一个序列。filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。返回的是一个Iterator  
```python
>>> def is_odd(n):
    	return n % 2 == 1
>>> list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))  
>>> # 结果: [1, 5, 9, 15]
```

sorted(iterable[, key][, reverse])   
返回一个排序后的列表，其中的元素来自 iterable 。key指定的函数将作用于list的每一个元素上，并根据key函数返回的结果进行排序。第三个参数reverse=True 表示反向排序，默认是False .注意点是：key处理元素之后对处理的元素排序，排完序之后会让原始的元素按照这个顺序排列，即原始的序列中元素没变成处理之后的值  
```python
>>> sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```

hasattr(object, name)  
判断一个对象里面是否有name属性或者name方法，返回BOOL值，有name特性返回True， 否则返回False。需要注意的是name要用引号包起来  

```python
>>> class test():
...     name="xiaohua"
...     def run(self):
...             return "HelloWord"
...
>>> t=test()
>>> hasattr(t, "name") #判断对象有name属性
True
>>> hasattr(t, "run")  #判断对象有run方法
True
```

getattr(object, name[,default])  
获取对象object的name属性或者方法，如果存在打印出来，如果不存在，打印出默认值，默认值可选。如果不提供该参数，在没有对应属性时，将触发 AttributeError。
需要注意的是，如果是返回的对象的方法，返回的是方法的内存地址，如果需要运行这个方法，可以在后面添加一对括号。  

```python
>>> class test():
...     name="xiaohua"
...     def run(self):
...             return "HelloWord"
...
>>> t=test()
>>> getattr(t, "name") #获取name属性，存在就打印出来。
'xiaohua'
>>> getattr(t, "run")  #获取run方法，存在就打印出方法的内存地址。
<bound method test.run of <__main__.test instance at 0x0269C878>>
>>> getattr(t, "run")()  #获取run方法，后面加括号可以将这个方法运行。
'HelloWord'
>>> getattr(t, "age")  #获取一个不存在的属性。
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: test instance has no attribute 'age'
>>> getattr(t, "age","18")  #若属性不存在，返回一个默认值。
'18'
```

setattr(object, name, values)  
给对象的属性赋值，若属性不存在，先创建再赋值。  

```python
>>> class test():
...     name="xiaohua"
...     def run(self):
...             return "HelloWord"
...
>>> t=test()
>>> hasattr(t, "age")   #判断属性是否存在
False
>>> setattr(t, "age", "18")   #为属相赋值，并没有返回值
>>> hasattr(t, "age")    #属性存在了
True
```

delattr(object, name)  
删除指定对象的指定属性，相等于 del x.foobar。name -- 必须是对象的属性，如果没有name属性会引发异常  