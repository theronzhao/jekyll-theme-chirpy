---
title: pickle模块
date: 2018-08-27 20:16:33 +0800
categories: [Python,标准库]
tags: [Python标准库]
---
模块 pickle 实现了对一个 Python 对象结构的二进制序列化和反序列化。 "pickling" 是将 Python 对象及其所拥有的层次结构转化为一个字节流的过程，而 "unpickling" 是相反的操作，会将（来自一个 binary file 或者 bytes-like object 的）字节流转化回一个对象层次结构。



## 与 json 模块的比较

JSON 是一个文本序列化格式（它输出 unicode 文本，尽管在大多数时候它会接着以 utf-8 编码），而 pickle 是一个二进制序列化格式；

JSON 是我们可以直观阅读的，而 pickle 不是；

JSON是可互操作的，在Python系统之外广泛使用，而pickle则是Python专用的；

默认情况下，JSON只能表示Python 内置类型的子集，不能表示自定义的类；但 pickle 可以表示大量的 Python 数据类型



## 常用API

**对文件的操作**

```python
pickle.dump(obj, file, [,protocol=None])
```

  将序列化后的对象obj以二进制形式写入文件file中，进行保存。关于参数file，有一点需要注意，必须是以二进制的形式进行操作（写入）。file为'svm_model_iris.pkl'，并且以二进制的形式（'wb'）写入。

```python
pickle.load(file, *,fix_imports=True, encoding=”ASCII”. errors=”strict”)
```

  该方法实现的是将序列化的对象从文件file中读取出来。关于参数file，有一点需要注意，必须是以二进制的形式进行操作（读取）。

**对内置数据类型操作**

```python
pickle.dumps(obj, protocol=None, *, fix_imports=True)
```

  将 obj 打包以后的对象作为 bytes 类型直接返回，而不是将其写入到文件。参数 protocol 和 fix_imports 的含义与它们在 dump() 中的含义相同。

```python
pickle.loads(bytes_object, *, fix_imports=True, encoding="ASCII", errors="strict")
```

  对于打包生成的对象 bytes_object，还原出原对象的结构并返回。