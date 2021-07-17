---
title: itertools模块
date: 2018-05-21 20:08:47 +0800
categories: [Python标准库]
tags: [Python标准库]
---
# itertools模块

itertools模块包含创建有效迭代器的函数，可以用各种方式对数据进行循环操作，此模块中的所有函数返回的迭代器都可以与for循环语句以及其他包含迭代器（如生成器和生成器表达式）的函数联合使用

## 无穷迭代器

```
itertools.count(start=0, step=1)
    创建一个无穷迭代器，它从 start 值开始，返回均匀间隔的值。常用于 map() 中的实参来生成连续的数据点。此外，还用于 zip() 来添加序列号
    count(10) --> 10 11 12 13 14 ...

itertools.cycle(iterable)
    创建一个无穷迭代器，返回 iterable 中所有元素并保存一个副本。当取完 iterable 中所有元素，返回副本中的所有元素。无限重复.
    cycle('ABCD') --> A B C D A B C D ...

itertools.repeat(object[, times])
    创建一个迭代器，不断重复 object 。除非设定参数 times ，否则将无限重复。可用于 map() 函数中的参数，被调用函数可得到一个不变参数。也可用于 zip() 的参数以在元组记录中创建一个不变的部分。
    repeat(10, 3) --> 10 10 10
    repeat(10) --> 10 10 10 10 10 ...
```

## 根据最短输入序列长度停止的迭代器

```
itertools.accumulate(iterable[, func])
    创建一个迭代器，返回累加和或其他二元函数的累加结果（通过可选参数 func 指定）。如果提供了 func ，它应是2个参数的函数。输入 iterable 元素类型应是 func 能支持的任意类型。
    accumulate([1,2,3,4,5]) --> 1 3 6 10 15
    accumulate()    p [,func]   p0, p0+p1, p0+p1+p2, ...

itertools.chain(*iterables)
    创建一个迭代器，它首先返回第一个可迭代对象中所有元素，接着返回下一个可迭代对象中所有元素，直到耗尽所有可迭代对象中的元素。可将多个序列处理为单个序列
    chain('ABC', 'DEF') --> A B C D E F

itertools.chain.from_iterable(iterable)
    构建类似 chain() 迭代器的另一个选择。从一个单独的可迭代参数中得到链式输入，该参数是延迟计算的。
    chain.from_iterable(['ABC', 'DEF']) --> A B C D E F

itertools.compress(data, selectors)
    创建一个迭代器，它返回 data 中经 selectors 真值测试为 True 的元素。迭代器在两者较短的长度处停止。
    compress('ABCDEF', [1,0,1,0,1,1]) --> A C E F

itertools.dropwhile(predicate, iterable)
    创建一个迭代器，如果 predicate 为true，迭代器丢弃这些元素，然后返回其他元素。注意，迭代器在 predicate 首次为false之前不会产生任何输出，所以可能需要一定长度的启动时间。
    dropwhile(lambda x: x<5, [1,4,6,4,1]) --> 6 4 1

itertools.takewhile(predicate, iterable)
    创建一个迭代器，只要 predicate 为真就从可迭代对象中返回元素。
    takewhile(lambda x: x<5, [1,4,6,4,1]) --> 1 4

itertools.filterfalse(predicate, iterable)
    创建一个迭代器，只返回 iterable 中 predicate 为 False 的元素。如果 predicate 是 None，返回真值测试为false的元素
    filterfalse(lambda x: x%2, range(10)) --> 0 2 4 6 8

itertools.islice(iterable, stop)
itertools.islice(iterable, start, stop[, step])
    创建一个迭代器，返回从 iterable 里选中的元素。如果 start 不是0，跳过 iterable 中的元素，直到到达 start 这个位置。之后迭代器连续返回元素，除非 step 设置的值很高导致被跳过。如果 stop 为 None，迭代器耗光为止；否则，在指定的位置停止。与普通的切片不同，islice() 不支持将 start ， stop ，或 step 设为负值。
    islice('ABCDEFG', 2, None) --> C D E F G

itertools.starmap(function, iterable)
    创建一个迭代器，使用从可迭代对象中获取的参数来计算该函数。当参数对应的形参已从一个单独可迭代对象组合为元组时（数据已被“预组对”）可用此函数代替 map()。map() 与 starmap() 之间的区别可以类比 function(a,b) 与 function(*c) 的区别。
    starmap(pow, [(2,5), (3,2), (10,3)]) --> 32 9 1000

itertools.tee(iterable, n=2):
    从iterable创建n个独立的迭代器，创建的迭代器以n元组的形式返回，n的默认值为2，此函数适用于任何可迭代的对象，但是，为了克隆原始迭代器，生成的项会被缓存，并在所有新创建的迭代器中使用，一定要注意，不要在调用tee()之后使用原始迭代器iterable，否则缓存机制可能无法正确工作。

itertools.zip_longest(*iterables, fillvalue=None)
    创建一个迭代器，从每个可迭代对象中收集元素。如果可迭代对象的长度未对齐，将根据 fillvalue 填充缺失值。迭代持续到耗光最长的可迭代对象。
    zip_longest('ABCD', 'xy', fillvalue='-') --> Ax By C- D-
```

## 排列组合迭代器

```
itertools.product(*iterables, repeat=1)
    可迭代对象输入的笛卡儿积。大致相当于生成器表达式中的嵌套循环。要计算可迭代对象自身的笛卡尔积，将可选参数 repeat 设定为要重复的次数
    product('ABCD', 'xy') --> Ax Ay Bx By Cx Cy Dx Dy
    product(range(2), repeat=3) --> 000 001 010 011 100 101 110 111

itertools.permutations(iterable, r=None)
    连续返回由 iterable 元素生成长度为 r 的排列。如果 r 未指定或为 None ，r 默认设置为 iterable 的长度，这种情况下，生成所有全长排列。排列依字典序发出。因此，如果 iterable 是已排序的，排列元组将有序地产出。.即使元素的值相同，不同位置的元素也被认为是不同的。如果元素值都不同，每个排列中的元素值不会重复。
    permutations('ABCD', 2) --> AB AC AD BA BC BD CA CB CD DA DB DC
    permutations(range(3)) --> 012 021 102 120 201 210

itertools.combinations(iterable, r)
    返回由输入 iterable 中元素组成长度为 r 的子序列。组合按照字典序返回。所以如果输入 iterable 是有序的，生成的组合元组也是有序的。即使元素的值相同，不同位置的元素也被认为是不同的。如果元素各自不同，那么每个组合中没有重复元素。
    combinations('ABCD', 2) --> AB AC AD BC BD CD
    combinations(range(4), 3) --> 012 013 023 123

itertools.combinations_with_replacement(iterable, r)
    返回由输入 iterable 中元素组成的长度为 r 的子序列，允许每个元素可重复出现。组合按照字典序返回。所以如果输入 iterable 是有序的，生成的组合元组也是有序的。不同位置的元素是不同的，即使它们的值相同。因此如果输入中的元素都是不同的话，返回的组合中元素也都会不同
    combinations_with_replacement('ABC', 2) --> AA AB AC BB BC CC
```

