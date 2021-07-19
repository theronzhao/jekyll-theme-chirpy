---
title: Json模块
date: 2018-08-27 20:37:06 +0800
categories: [Python,标准库]
tags: [Python标准库]
---

JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。它使得人们很容易的进行阅读和编写。同时也方便了机器进行解析和生成。它是基于 [JavaScript Programming Language](http://www.crockford.com/javascript) , [Standard ECMA-262 3rd Edition - December 1999](http://www.ecma-international.org/publications/files/ecma-st/ECMA-262.pdf) 的一个子集。 JSON采用完全独立于程序语言的文本格式，但是也使用了类C语言的习惯（包括C, C++, C#, Java, JavaScript, Perl, Python等）。这些特性使JSON成为理想的数据交换语言。

**JSON基于两种结构**

json简单说就是javascript中的对象和数组，所以这两种结构就是对象和数组两种结构，通过这两种结构可以表示各种复杂的结构

- 1、对象：对象在js中表示为“{}”括起来的内容，数据结构为 {key：value,key：value,...}的键值对的结构，在面向对象的语言中，key为对象的属性，value为对应的属性值，所以很容易理解，取值方法为 对象.key 获取属性值，这个属性值的类型可以是 数字、字符串、数组、对象几种。
- 2、数组：数组在js中是中括号“[]”括起来的内容，数据结构为 ["java","javascript","vb",...]，取值方式和所有语言中一样，使用索引获取，字段值的类型可以是 数字、字符串、数组、对象几种。

```python
{"name":"tom", "age":18}
```

json中的(key)属性名称和字符串值需要用**双引号**引起来，用单引号或者不用引号会导致读取数据错误。

```python
["tom",18,"programmer"]
```

经过对象、数组2种结构就可以组合成复杂的数据结构了。

“名称/值”对的集合（A collection of name/value pairs）。不同的编程语言中，它被理解为对象（object），纪录（record），结构（struct），字典（dictionary），哈希表（hash table），有键列表（keyed list），或者关联数组 （associative array）。 值的有序列表（An ordered list of values）。在大部分语言中，它被实现为数组（array），矢量（vector），列表（list），序列（sequence）。

这些都是常见的数据结构。目前，绝大部分编程语言都以某种形式支持它们。这使得在各种编程语言之间交换同样格式的数据成为可能。

**和XML的比较**

可读性

JSON和XML的可读性可谓不相上下，一边是简易的语法，一边是规范的标签形式，很难分出胜负。

可扩展性

XML天生有很好的扩展性，JSON当然也有，没有什么是XML可以扩展而JSON却不能扩展的。不过JSON在Javascript主场作战，可以存储Javascript复合对象，有着xml不可比拟的优势。

编码难度

XML有丰富的编码工具，比如Dom4j、JDom等，JSON也有提供的工具。无工具的情况下，相信熟练的开发人员一样能很快的写出想要的xml文档和JSON字符串，不过，xml文档要多很多结构上的字符。

解码难度

- XML的解析方式有两种：

- 一是通过文档模型解析，也就是通过父标签索引出一组标记。例如：xmlData.getElementsByTagName("tagName")，但是这样是要在预先知道文档结构的情况下使用，无法进行通用的封装。
- 另外一种方法是遍历节点（document 以及 childNodes）。这个可以通过递归来实现，不过解析出来的数据仍旧是形式各异，往往也不能满足预先的要求。

JSON和XML还有另外一个很大的区别在于有效数据率。JSON作为数据包格式传输的时候具有更高的效率，这是因为JSON不像XML那样需要有严格的闭合标签，这就让有效数据量与总数据包比大大提升，从而减少同等数据流量的情况下，网络的传输压力。

**Python3 中对 JSON 数据处理**

Python3 中可以使用 json 模块来对 JSON 数据进行编解码，它包含了两个函数：

> json.dumps(): 对数据进行编码。
> json.loads(): 对数据进行解码。

**编码和解码**  
编码时，python的字典转换成json的object对象，元组和列表转换成array数组,其他数据类型一一对应，没有集合  
解码时，json的object对象转换成python的字典，array数组转换成列表    

python3.6版本开始支持json.loads(bytes类型)
```python
import json

# Python 字典类型转换为 JSON 对象
data1 = {
    'no' : 1,
    'name' : 'baidu',
    'url' : 'http://www.baidu.com'
}

json_str = json.dumps(data1)
print ("Python 原始数据：", repr(data1))
print ("JSON 对象：", json_str)

# 将 JSON 对象转换为 Python 字典
data2 = json.loads(json_str)
print ("data2['name']: ", data2['name'])
print ("data2['url']: ", data2['url'])
```

如果要处理的是文件而不是字符串，可以使用 json.dump() 和 json.load() 来编码和解码JSON数据。例如：
```python
# 写入 JSON 数据
with open('data.json', 'w') as f:
    json.dump(data, f)

# 读取数据
with open('data.json', 'r') as f:
    data = json.load(f)
```