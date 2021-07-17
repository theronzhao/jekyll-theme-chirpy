---
title: 文件操作
date: 2018-10-24 22:38:16 +0800
categories: [Python]
tags: [Python]
---



## open()函数

open() 方法用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数,如果该文件无法被打开，会抛出 OSError。基本语法格式如下:

open(file, mode)  # 简写语法

open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

>**file**：一个 path-like object，表示将要打开的文件的路径。  
>**mode**：决定了打开文件的模式：只读，写入，追加等。这个参数是非强制的，默认文件访问模式为只读(r)；指针，相当于打字的光标；r,读，指针在开始；r+,读写，指针在开始；w,写，创建，覆盖，指针在开始；w+,读写创建覆盖，指针在开始；a,写创建，指针在结尾；a+读写创建，指针在结尾; x,排它性创建，如果文件已存在则失败; b, 二进制模式; t, 文本模式（默认值，与其他模式结合使用）  
>**encoding**: 一般使用utf8  
>**errors**: 是一个可选的字符串参数，用于指定如何处理编码和解码错误 - 这不能在二进制模式下使用。  
>**newline**: 区分换行符,,默认根据平台来辨别换行符  
>**closefd**: 传入的file参数类型  
>**buffering**: 设置缓冲.传递0以切换缓冲关闭（仅允许在二进制模式下），1选择行缓冲（仅在文本模式下可用），并且>1的整数以指示固定大小的块缓冲区的大小（以字节为单位）。如果没有给出 buffering 参数，则默认缓冲策略的工作方式如下:  
>
>>二进制文件以固定大小的块进行缓冲；使用启发式方法选择缓冲区的大小，尝试确定底层设备的“块大小”或使用 io.DEFAULT_BUFFER_SIZE。在许多系统上，缓冲区的长度通常为4096或8192字节。
>>“交互式”文本文件（ isatty() 返回 True 的文件）使用行缓冲。其他文本文件使用上述策略用于二进制文件。

文件打开方式1  

```python
f = open("/tmp/foo.txt", "w")  # 打开一个文件  
f.write( "非常好的语言。\n是的，的确非常好!!\n" )  
f.close()  # 关闭打开的文件  
```

文件打开方式2  

```python
with open("/tmp/foo.txt", "w") as file:
    # with语句块内会自动调用f.close()帮助关闭文件
    f.write( "Python 是一个非常好的语言。\n是的，的确非常好!!\n" )
```

## 文件对象的方法

f.read([,size])

\- 读取一定数目的数据, 然后作为字符串或字节对象返回。默认读取整个文件.在文本模式下size指的是多少个字符,在二进制模式下是多少个字节

f.readline()

\- 从文件中读取单独的一行。换行符为 '\n'。f.readline() 如果返回一个空字符串, 说明已经已经读取到最后一行。

f.readlines() 

\- 将返回该文件中包含的所有行。如果设置可选参数 sizehint, 则读取指定长度的字节, 并且将这些字节按行分割。

f.write(string) 

\- 将 string 写入到文件中, 然后返回写入的字符数

f.tell() 

\- 返回文件对象当前所处的位置, 它是从文件开头开始算起的字节数/字符数。

f.seek(offset, from_what)

\- 如果要改变文件指针当前的位置, from_what 的值, 如果是 0 表示开头, 如果是 1 表示当前位置, 2 表示文件的结尾，from_what 值为默认为0，即文件开头。offset表示偏移当前指针的字节数

 	seek(x,0) ： 从起始位置即文件首行首字符开始移动 x 个字符

​	 seek(x,1) ： 表示从当前位置往后移动x个字符

​	 seek(-x,2)：表示从文件的结尾往前移动x个字符

f.flush()

\- 将缓冲区中的数据立刻写入文件，同时清空缓冲区，不需要是被动的等待输出缓冲区写入。

一般情况下，文件关闭后会自动刷新缓冲区，但有时你需要在关闭前刷新它，这时就可以使用 flush() 方法。

f.truncate([size])

\- 从文件的首行首字符开始截断，截断文件为 size 个字符，无 size 表示从当前位置截断；截断之后后面的所有字符被删除，其中 Widnows 系统下的换行代表2个字符大小。

f.writelines(sequence)

\- 向文件写入一个序列字符串列表，如果需要换行则要自己加入每行的换行符。

f.close() 

\- 关闭文件并释放系统的资源

## 三个标准流

变量 sys.stdin 、 sys.stdout 和 sys.stderr 是类似于文件的流对象，表示标准的UNIX概念：标准输入、标准输出和标准错误。一个标准数据输入源是 sys.stdin 。当程序从标准输入读取时，你可通过输入来提供文本，也可使用管道将标准输入关联到其他程序的标准输出.你提供给 print 的文本出现在 sys.stdout 中，向 input 提供的提示信息也出现在这里。写入到 sys.stdout 的数据通常出现在屏幕上，但可使用管道将其重定向到另一个程序的标准输入。错误消息（如栈跟踪）被写入到 sys.stderr ，但与写入到 sys.stdout 的内容一样，可对其进行重定向。

## 序列化和反序列化

pickle模块实现了基本的数据序列和反序列化。通过pickle模块的序列化操作我们能够将程序中运行的对象信息保存到文件中去，永久存储。通过pickle模块的反序列化操作，我们能够从文件中创建上一次程序保存的对象。

提示：注意区分pickle.dumps()和pickle.dump(),前者操作的不是文件

提示：注意区分pickle.loads()和pickle.load(),前者操作的不是文件

**序列化操作**

```python
pickle.dump(obj, file, [,protocol=None])
```

将序列化后的对象obj以二进制形式写入文件file中，进行保存。关于参数file，有一点需要注意，必须是以二进制的形式进行操作（写入）。file为'svm_model_iris.pkl'，并且以二进制的形式（'wb'）写入。可选参数 protocol 是一个整数，告知 pickler 使用指定的协议，可选择的协议范围从 0 到 HIGHEST_PROTOCOL(4)。如果没有指定，这一参数默认值为 DEFAULT_PROTOCOL。指定一个负数就相当于指定 HIGHEST_PROTOCOL

```python
import pickle
# 使用pickle模块将数据对象保存到文件
data1 = {'a': [1, 2.0, 3, 4+6j],
         'b': ('string', u'Unicode string'),
         'c': None}
selfref_list = [1, 2, 3]
selfref_list.append(selfref_list)
output = open('data.pkl', 'wb')
# Pickle dictionary using protocol 3.
pickle.dump(data1, output)
# Pickle the list using the highest protocol available.
pickle.dump(selfref_list, output, -1)
output.close()
```

**反序列化操作**

```python
pickle.load(file, *,fix_imports=True, encoding=”ASCII”. errors=”strict”)
```

该方法实现的是将序列化的对象从文件file中读取出来。关于参数file，有一点需要注意，必须是以二进制的形式进行操作（读取）。

```python
#使用pickle模块从文件中重构python对象
pkl_file = open('data.pkl', 'rb')

data1 = pickle.load(pkl_file)
pprint.pprint(data1)

data2 = pickle.load(pkl_file)
pprint.pprint(data2)

pkl_file.close()
```

