---
title: 命名空间和作用域
date: 2018-12-01 23:37:41 +0800
categories: [Python,Python进阶]
tags: [Python进阶]
---


## 三种命名空间

**内置名称**（built-in names），Python 语言内置的名称，比如函数名 abs、char 和异常名称 BaseException、Exception 等等。

**全局名称**(global names),模块中定义的名称，记录了模块的变量,包括函数、类、其它导入的模块、模块级的变量和常量。

**局部名称**(local names),函数中定义的名称，记录了函数的变量，包括函数的参数和局部定义的变量。（类中定义的也是）

命名空间查找顺序:**局部的命名空间去** -> **全局命名空间** -> **内置命名空间**。

如果找不到变量 runoob，它将放弃查找并引发一个 NameError 异常

```python
# var1 是全局名称
var1 = 5
def some_func():
 
    # var2 是局部名称
    var2 = 6
    def some_inner_func():
 
        # var3 是内嵌的局部名称
        var3 = 7
```

## 四种作用域

L（Local）：最内层，包含局部变量，比如一个函数/方法内部。

E（Enclosing）：包含了非局部(non-local)也非全局(non-global)的变量。比如两个嵌套函数，一个函数（或类） A 里面又包含了一个函数 B ，那么对于 B 中的名称来说 A 中的作用域就为 nonlocal。

G（Global）：当前脚本的最外层，比如当前模块的全局变量。

B（Built-in）： 包含了内建的变量/关键字等。，最后被搜索

规则顺序： L –> E –> G –>gt; B。在局部找不到，便会去局部外的局部找（例如闭包），再找不到就会去全局找，再者去内置中找。

```python
g_count = 0  # 全局作用域
def outer():
    o_count = 1  # 闭包函数外的函数中
    def inner():
        i_count = 2  # 局部作用域
```

只有模块（module），类（class）以及函数（def、lambda）才会引入新的作用域，其它的代码块（如 if/elif/else/、try/except、for/while等）是不会引入新的作用域的，也就是说这些语句内定义的变量，外部也可以访问

## 全局变量和局部变量

局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。调用函数时，所有在函数内声明的变量名称都将被加入到作用域中。

```python
total = 0 # 这是一个全局变量
# 可写函数说明
def sum( arg1, arg2 ):
    #返回2个参数的和."
    total = arg1 + arg2 # total在这里是局部变量.
    print ("函数内是局部变量 : ", total)
    return total
 
#调用sum函数
sum( 10, 20 )
print ("函数外是全局变量 : ", total)
```

## global 和 nonlocal关键字

当内部作用域想修改外部作用域的变量时，就要用到global和nonlocal关键字。

```python
num = 1
def fun1():
    global num  # 需要使用 global 关键字声明
    print(num) 
    num = 123
    print(num)
fun1()
print(num)

def outer():
    num = 10
    def inner():
        nonlocal num   # nonlocal关键字声明
        num = 100
        print(num)
    inner()
    print(num)
outer()
```

