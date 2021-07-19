---
title: collections模块
date: 2018-06-26 21:17:22 +0800
categories: [Python,标准库]
tags: [Python标准库]
---


# collections模块

`class collections.deque([iterable[,maxlen]])`

- 返回一个新的双向队列对象，从左到右初始化(用方法 append()) ，从 iterable （迭代对象) 数据创建。如果 iterable 没有指定，新队列为空。如果 maxlen 没有指定或者是 None ，deques 可以增长到任意长度。否则，deque就限定到指定最大长度。一旦限定长度的deque满了，当新项加入时，同样数量的项就从另一端弹出。
- Deque队列是由栈或者queue队列生成的.在队列两端插入或删除元素时间复杂度都是 O(1) ，而在列表的开头插入或删除元素的时间复杂度为 O(N) 。

append(x)   添加 x 到右端。

appendleft(x)   添加 x 到左端。

clear() 移除所有元素，使其长度为0.

copy()  创建一份浅拷贝。

count(x)    计算deque中个数等于 x 的元素。

extend(iterable)    扩展deque的右侧，通过添加iterable参数中的元素。

extendleft(iterable)    扩展deque的左侧，通过添加iterable参数中的元素。注意，左添加时，在结果中iterable参数中的顺序将被反过来添加。

index(x[, start[, stop]])   返回第 x 个元素（从 start 开始计算，在 stop 之前）。返回第一个匹配，如果没找到的话，升起 ValueError 。

insert(i, x)    在位置 i 插入 x 。如果插入会导致一个限长deque超出长度 maxlen 的话，就升起一个 IndexError 。

pop()   移去并且返回一个元素，deque最右侧的那一个。如果没有元素的话，就升起 IndexError 索引错误。

popleft()   移去并且返回一个元素，deque最左侧的那一个。如果没有元素的话，就升起 IndexError 索引错误。

remove(value)   移去找到的第一个 value。 如果没有的话就升起 ValueError 。

reverse()   将deque逆序排列。返回 None 。

rotate(n=1)    向右循环移动 n 步。 如果 n 是负数，就向左循环。如果deque不是空的，向右循环移动一步就等价于 d.appendleft(d.pop()) ， 向左循环一步就等价于 d.append(d.popleft()) 。

```python
>>> q = deque(maxlen=3)
>>> q.append(1)
>>> q.append(2)
>>> q.append(3)
>>> q
deque([1, 2, 3], maxlen=3)
>>> q.append(4)
>>> q
deque([2, 3, 4], maxlen=3)
>>> q.append(5)
>>> q
deque([3, 4, 5], maxlen=3)
```

