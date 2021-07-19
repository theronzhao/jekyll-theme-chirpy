---
title: Python笔记
date: 2019-02-15 23:37:41 +0800
categories: [Python,Python基础]
tags: [Python基础,Python进阶]
---

# 列表 List

aList.append(obj)  
在列表末尾追加元素，等同于 aList[len(aList）:len(aList)] = [obj]

aList.clear()  
删除 aList 的所有元素

aList.count(obj)  
返回 aList 中与 obj 相等的元素个数

aList.copy()  
返回 aList 的副本。请注意，这是浅复制，即不会复制元素

aList.extend(sequence)  
将sequence中的元素逐个追加到列表当中，等同于 aList[len(aList):len(aList)] = sequence

aList.index(obj)  
返回 aList 中第一个与 obj 相等的元素的索引；如果没有这样的元素，就引发ValueError 异常

aList.insert(index, obj)  
如果 index >= 0 ，就等同于 aList[index:index] = [obj] ；如果 index < 0 ，就将指定的对象加入到列表尾端索引前一位

aList.pop([index])  
删除并返回指定索引（默认为-1）处的元素

aLiast.remove(obj)  
等同于 del aList[aList.index(obj)], 删除第一个与 obj 相等的元素,如果没有则引发ValueError异常

aList.reverse()  
就地按相反的顺序排列列表的元素

aList.sort([key，[,reverse]）  
就地对 aList 的元素进行排序（稳定排序）。可通过提供键函数key （创建用户排序的键）和降序标志 reverse （一个布尔值）进行定制.*key* 指定带有一个参数的函数，用于从每个列表元素中提取比较键 (例如 `key=str.lower`)。 对应于列表中每一项的键会被计算一次，然后在整个排序过程中使用。 默认值 `None` 表示直接对列表项排序而不计算一个单独的键值。

**列表推导式**

```python
>>> freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
>>> [weapon.strip() for weapon in freshfruit]
>>> list1 = [(i, j) for i in range(1, 3) for j in range(3)]
>>> list2 = [i for i in range(10) if i % 2 == 0]
```

# 集合 Set

aSet.add(x)  
将元素 x 添加到集合 s 中，如果元素已存在，则不进行任何操作。x是单一元素

aSet.update(seq)  
添加 seq 中的每一个元素到集合中，参数可以是列表，元组，字典等序列

aSet.remove( x )  
将元素 x 从集合 s 中移除，如果元素不存在，则会发生错误。

aSet.discard( x )  
移除集合中的元素，且如果元素不存在，不会发生错误。

aSet.pop（）  

随机删除集合中的一个元素并返回

aSet.clear()  
清空集合中的元素

aSet.copy()  
复制集合

aSet.union(set1, set2...)  
返回两个集合的并集，即包含了所有集合的元素，重复的元素只会出现一次。原集合未改变

aSet.difference(bset)  
返回集合的差集，即返回的集合元素包含在第一个集合中，但不包含在第二个集合(方法的参数)中

aSet.difference_update(set)  
用于移除a中两个集合都存在的元素。difference_update() 方法与 difference() 方法的区别在于 difference() 方法返回一个移除相同元素的新集合，而 difference_update() 方法是直接在原来的集合中移除元素，没有返回值。

aSet.intersection(set1, set2 ... etc)  
返回两个或更多集合中都包含的元素，即交集。

aSet.intersection_update(set1, set2 ... etc)  
用于获取两个或更多集合中都重叠的元素，即计算交集.intersection_update() 方法不同于 intersection() 方法，因为 intersection() 方法是返回一个新的集合，而 intersection_update() 方法是在原始的集合上移除不重叠的元素。

aSet.isdisjoint(set)  
判断两个集合是否包含相同的元素，如果没有返回 True，否则返回 False。

aSet.issubset(set)  
判断集合的所有元素是否都包含在指定集合中，如果是则返回 True，否则返回 False。

aSet.issuperset(set)  
判断指定集合的所有元素是否都包含在原始的集合中，如果是则返回 True，否则返回 False。

aSet.symmetric_difference(set)  
返回两个集合中不重复的元素集合，即会移除两个集合中都存在的元素。

aSet.symmetric_difference_update(set)  
移除当前集合中在另外一个指定集合相同的元素，并将另外一个指定集合中不同的元素插入到当前集合中。

**集合推导式**

```python
>>> a = {x for x in 'abracadabra' if x not in 'abc'}
>>> a
{'r', 'd'}
```

**两个集合间的运算**

```python
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  
{'a', 'r', 'b', 'c', 'd'}
>>> a -b                              # 集合a中包含而集合b中不包含的元素
{'r', 'd', 'b'}
>>> a | b                              # 集合a或b中包含的所有元素
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # 集合a和b中都包含了的元素
{'a', 'c'}
>>> a ^ b                              # 不同时包含于a和b的元素
{'r', 'd', 'b', 'm', 'z', 'l'}
```

# 字典 Dict

aDict.clear()  
删除 aDict 的所有项

aDict.copy()  
返回 aDict 的副本，是浅复制

aDict.fromkeys(seq[,val])  
返回一个字典，其中的键来自 seq ，而值都被设置为 val （默认为 None ）。可直接使用字典类型 dict 将其作为类方法来调用

aDict.get(key[,default])  
如果 aDict[key] 存在，就返回它；否则返回指定的默认值（默认为 None ）

aDict.setdefault(key[,default])  
如果 aDict[key] 存在，就返回它；否则就返回指定的默认值（默认为 None ），并将 aDict[key] 设置为指定的默认值

aDict.items()  
返回一个迭代器（实际上是一个视图），其中包含表示 aDict 各项的 (key, value) 对

aDict.keys()  
返回一个迭代器（视图），其中包含 aDict 中所有的键

aDict.values()  
返回一个迭代器（视图），其中包含 aDict 中所有的值（可能有重复的）

aDict.pop(key[, default])  
如果key存在于字典中则将其移除并返回其值，否则返回default。如果default未给出且key不存在于字典中，则会引发KeyError。

aDict.popitem()  
从 aDict 随机的删除一项，并将其以 元组(key, value) 对的方式返回

aDict.update(otherDict)  
将 otherDict 中的每项都添加到 aDict （可能覆盖既有的项）。也可以像调用字典构造函数那样指定类似的参数

```python
D = {'one': 1, 'two': 2}
D.update({'three': 3, 'four': 4})  # 传一个字典
D.update(five=5, six=6)  # 传关键字
D.update([('seven', 7), ('eight', 8)])  # 传一个包含一个或多个元祖的列表
```

**字典推导式**

```python
counts = {'MBP': 268, 'HP': 125, 'DELL': 201, 'Lenovo': 199, 'acer': 99}
# 需求：提取上述电脑数量量⼤大于等于200的字典数据
count1 = {key: value for key, value in counts.items() if value >= 200}

>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
{'sape': 4139, 'jack': 4098, 'guido': 4127}
```

# 元组 Tuple

元组与列表类似，不同之处在于**元组的元素不能修改**  

tup.index(x)  
查找某个元素，如果数据存在返回对应的下标，否则报错

tup.count(x)  
统计某个元素在当前元组出现的次数。

# 字符串 String

## 格式相关

`string.capitalize()`  
返回字符串的副本，但将第一个字符大写

string.title()  
将字符串中所有单词的首字母都大写，并返回结果

string.upper()  
将字符串中所有的字母都转换为大写，并返回结果

string.lower()  
将字符串中所有的字母都转换为小写，并返回结果。只对ASCII字符有效，跟casefold相比范围窄

string.casefold()  
返回经过标准化（normalize）后的字符串，标准化类似于转换为小写，但更适合用于对Unicode字符串进行不区分大小写的比较

string.swapcase()  
将字符串中所有字母的大小写都反转，并返回结果

string.expandtabs([tabsize])  
返回将字符串中的制表符展开为空格后的结果，可指定可选参数 tabsize（默认为8）

string.encode([encoding[,errors]])  
返回使用指定编码和errors指定的错误处理方式对字符串进行编码的结果,参数errors的可能取值包含'strict','ignore','replace'

string.strip([chars])  
将字符串开头和结尾的所有 chars 字符（默认为所有空白字符，如空格、制表符和换行符）都删除，并返回结果

string.rstrip([chars])  
将字符串末尾所有的 chars 字符（默认为所有的空白字符，如空格、制表符和换行符）都删除，并返回结果

string.lstrip([chars])  
将字符串开头所有的 chars （默认为所有的空白字符，如空格、制表符和换行符）都删除，并返回结果

string.zfill(width)  
在字符串左边填充0（但将原来打头的 + 或 - 移到开头），使其长度为 width

string.center(width[, fillchar])  
返回一个指定的宽度 width 居中的字符串，fillchar 为填充的字符，默认为空格。

string.ljust(width[, fillchar])  
返回一个长度为 max(len(string), width) 的字符串，其开头是当前字符串的副本，而末尾是使用fillchar 指定的字符（默认为空格）填充的

string.rjust(width[,fillchar])  
返回一个长度为 max(len(string), width) 的字符串，其末尾为当前字符串的拷贝，而开头是使用fillchar 指定的字符（默认为空格）填充的

## 查询相关

string.count(sub[, start[, end]])  
计算子串 sub 出现的次数，可搜索范围限定为 string[start:end]

string.partition(sep)  
在字符串中搜索 sep ，并返回元组 (sep 前面的部分 , sep, sep 后面的部分 ),如果sep不在字符串中,

string.rpartition(sep)  
与 partition 相同，但从右往左搜索

string.index(sub[, start[, end]])  
返回找到的第一个子串 sub 的索引，如果没有找到将引发ValueError 异常；还可将搜索范围限制为string[start:end]

string.rindex(sub[,start[,end]])  
从右开始找子串 sub 的索引，如果没有找到这样的子串，就引发ValueError 异常；还可将搜索范围限定为 string[start:end]

string.find(sub[, start[, end]])  
返回找到的第一个子串 sub 的索引，如果没有找到就返回 -1 ；还可将搜索范围限制为string[start:end]

string.rfind(sub[,start[,end]])  
从右开始找子串的索引，如果没有找到这样的子串，就返回 -1 ；还可将搜索范围限定为 string[start:end]

## 切割替换

string.replace(old,new[,max])  
将字符串中的子串 old 替换为 new ，并返回结果；还可将最大替换次数限制为 max

string.join(sequence)  
以指定字符串string作为分隔符，将 seq 中所有的元素(的字符串表示)合并为一个新的字符串

string.split([sep[, maxsplit]])  
返回一个列表，其中包含以 sep 为分隔符对字符串进行划分得到的结果（参数 sep 默认空白字符）；还可将最大划分次数限制为 maxsplit

string.rsplit([sep[, maxsplit]])  
与 split 相同，但指定了参数 maxsplit ，从右往左计算划分次数

string.splitlines([keepends])  
返回一个列表，其中包含字符串中的所有行；如果参数 keepends 为 True ，将包含换行符在前一个元素中

## 进行判断

string.endswith(suffix[,start[,end]])  
检查字符串是否以 suffix 结尾，还可使用索引 start 和 end 来指定匹配范围

string.startswith(prefix[,start[,end]])  
检查字符串是否以 prefix 打头,是则返回 True，否则返回 False；还可将匹配范围限制在索引 start 和end之间

string.isalnum()  
如果字符串至少有一个字符并且所有字符都是字母或数字则返 回 True,否则返回 False

string.isalpha()  
如果字符串至少有一个字符并且所有字符都是字母则返回 True, 否则返回 False

string.isdecimal()  
检查字符串中的字符是否都是十进制数

string.isdigit()  
如果字符串只包含数字则返回 True 否则返回 False

string.islower()  
如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False

string.isupper()  
如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False

string.isnumeric()  
如果字符串中只包含数字字符，则返回 True，否则返回 False.数字可以是： Unicode 数字，全角数字（双字节），罗马数字，汉字数字。指数类似 ² 与分数类似 ½ 也属于数字。

string.isprintable()  
检查字符串中的字符是否都是可打印的

string.isspace()  
如果字符串中只包含空白，则返回 True，否则返回 False.

string.istitle()  
如果字符串是标题化的(见 title())则返回 True，否则返回 False.位于非字母后面的字母都是大写的，且其他所有字母都是小写的

string.isidentifier()  
检查字符串是否可用作Python标识符

## 其他

string.format(...)  
实现了标准的Python字符串格式设置。将字符串中用大括号分隔的字段替换为相应的参数，再返回结果

string.format_map(mapping)  
类似于使用关键字参数调用 format ，只是参数是以映射的方式提供的

```python
>>>"{} {}".format("hello", "world")    # 不设置指定位置，按默认顺序
'hello world'
>>> "{1} {0} {1}".format("hello", "world")  # 设置指定位置
'world hello world'
>>>print("网站名：{name}, 地址 {url}".format(name="百度", url="www.baidu.com"))  # 通过参数指定
```

str.maketrans(x[,y[,z]])  
一个静态方法，它创建一个供 translate 使用的转换表。一个参数，该参数必须为字典；两个参数 x 和 y，x、y 必须是长度相等的字符串，并且 x 中每个字符映射到 y 中相同位置的字符；还可提供第三个参数 z 必须是字符串，其字符将被映射为 None，即删除该字符

string.translate(table)  
根据转换表 table （这是使用 maketrans 创建的）对字符串中的所有字符都进行转换，并返回结果

```python
>>> intab = "aeiou"
>>> outtab = "12345"
>>> trantab = str.maketrans(intab, outtab)   # 制作翻译表
>>> str = "this is string example....wow!!!"
>>> print (str.translate(trantab))
th3s 3s str3ng 2x1mpl2....w4w!!!
```

## 字符串的格式化

1. 最基本的用法是将一个值插入到一个有字符串格式符 %s 的字符串中

```python
%s	 格式化字符串
%d	 格式化整数
%f	 格式化浮点数字，可指定小数点后的精度

格式化操作符辅助指令：
    *	    定义宽度或者小数点精度
    -	    用做左对齐
    +	    在正数前面显示加号( + )
    <sp>	在正数前面显示空格
    #	    在八进制数前面显示零('0')，在十六进制前面显示'0x'或者'0X'(取决于用的是'x'还是'X')
    0	    显示的数字前面填充'0'而不是默认的空格
    %	    '%%'输出一个单一的'%'
    (var)	映射变量(字典参数)
    m.n.	m 是显示的最小总宽度,n 是小数点后的位数(如果可用的话)
```

2. Python2.6 开始新增了一种格式化字符串的函数 str.format()，它增强了字符串格式化的功能。基本语法是通过 {} 和 : 来代替以前的 % 。format 函数可以接受不限个参数，位置可以不按顺序。

```python
>>>"{} {}".format("hello", "world")    # 不设置指定位置，按默认顺序
'hello world'
>>> "{1} {0} {1}".format("hello", "world")  # 设置指定位置
'world hello world'

print("百度：{name}, 地址 {url}".format(name="百度", url="www.baidu.com"))

# 通过字典设置参数
site = {"name": "百度", "url": "www.baidu.com"}
print("网站名：{name}, 地址 {url}".format(**site))
 
# 通过列表索引设置参数
my_list = ['百度', 'www.baidu.com']
print("网站名：{0[0]}, 地址 {0[1]}".format(my_list))  # "0" 是必须的

# 通过传入对象设置参数
class AssignValue(object):
    def __init__(self, value):
        self.value = value
my_value = AssignValue(6)
print('value 为: {0.value}'.format(my_value))  # "0" 是可选的
```

3. F字符串- f-string 是 python3.6 之后版本添加的，称之为字面量格式化字符串，是新的格式化字符串的语法。

    f-string 格式化字符串以 f 开头，后面跟着字符串，字符串中的表达式用大括号 {} 包起来，它会将变量或表达式计算后的值替换进去

    执行效率比前两种快

    ```python
    >>> name = 'baidu'
    >>> f'Hello {name}'  # 替换变量
    'Hello baidu'
    >>> f'{1+2}'         # 使用表达式
    '3'
    >>> w = {'name': 'baidu', 'url': 'www.baidu.com'}
    >>> f'{w["name"]}: {w["url"]}'
    'baidu: www.baidu.com'
    ```

# 数字 Number

Python 支持三种不同的数值类型：

- **整型(Int)** - 通常被称为是整型或整数，是正或负整数，不带小数点。Python3 整型是没有限制大小的，可以当作 Long 类型使用，所以 Python3 没有 Python2 的 Long 类型。
- **浮点型(float)** - 浮点型由整数部分与小数部分组成，浮点型也可以使用科学计数法表示（2.5e2 = 2.5 x 102 = 250）
- **复数( (complex))** - 复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型。

## 按位运算符

``` python
按位运算符是把数字看作二进制来进行计算的。  
a = 0011 1100   # a为60  
b = 0000 1101   # b为13 
--------------
a & b   =   0000 1100  # 输出结果 12
a | b   =   0011 1101  # 输出结果 61
a ^ b   =   0011 0001  # 输出结果 49
~a      =   1100 0011  # 输出结果 -61
a << 2  =   1111 0000  # 输出结果 240 
a >> 2  =   0000 1111  # 输出结果 15 

&	按位与运算符：参与运算的两个值,如果两个相应位都为1,则该位的结果为1,否则为0
|	按位或运算符：只要对应的两个二进位有一个为1时，结果位就为1
^	按位异或运算符：当两对应的二进位相异时，结果为1	
~	按位取反运算符：对数据的每个二进制位取反,即把1变为0,把0变为1;~x 类似于 -x-1
<<	左移动运算符：二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0。	
>>	右移动运算符：把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数
```

## Python运算符优先级

以下表格列出了从最高到最低优先级的所有运算符：

| 运算符                   | 描述                                                   |
| :----------------------- | :----------------------------------------------------- |
| **                       | 指数 (最高优先级)                                      |
| ~ + -                    | 按位翻转, 一元加号和减号 (最后两个的方法名为 +@ 和 -@) |
| * / % //                 | 乘，除，求余数和取整除                                 |
| + -                      | 加法减法                                               |
| >> <<                    | 右移，左移运算符                                       |
| &                        | 位 'AND'                                               |
| ^ \|                     | 位运算符                                               |
| <= < > >=                | 比较运算符                                             |
| == !=                    | 等于运算符                                             |
| = %= /= //= -= += *= **= | 赋值运算符                                             |
| is is not                | 身份运算符                                             |
| in not in                | 成员运算符                                             |
| not and or               | 逻辑运算符                                             |

## 常用的数据类型转换


|          函数          |                        说明                         |
| :--------------------: | :-------------------------------------------------: |
|    int(x [,base ])     |                  将x转换为一个整数                  |
|       float(x )        |                 将x转换为一个浮点数                 |
| complex(real [,imag ]) |        创建一个复数，real为实部，imag为虚部         |
|        str(x )         |                将对象 x 转换为字符串                |
|        repr(x )        |             将对象 x 转换为表达式字符串             |
|       eval(str )       | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
|       tuple(s )        |               将序列 s 转换为一个元组               |
|        list(s )        |               将序列 s 转换为一个列表               |
|        chr(x )         |           将一个整数转换为一个Unicode字符           |
|        ord(x )         |           将一个字符转换为它的ASCII整数值           |
|        hex(x )         |         将一个整数转换为一个十六进制字符串          |
|        oct(x )         |          将一个整数转换为一个八进制字符串           |
|        bin(x )         |          将一个整数转换为一个二进制字符串           |

## 数学函数

| 函数            | 返回值 ( 描述 )                                              |
| :-------------- | :----------------------------------------------------------- |
| abs(x)          | 返回数字的绝对值，如abs(-10) 返回 10                         |
| ceil(x)         | 返回数字的上入整数，如math.ceil(4.1) 返回 5                  |
| cmp(x, y)       | 如果 x < y 返回 -1, 如果 x == y 返回 0, 如果 x > y 返回 1。 **Python 3 已废弃，使用 (x>y)-(x。 |
| exp(x)          | 返回e的x次幂(ex),如math.exp(1) 返回2.718281828459045         |
| fabs(x)         | 返回数字的绝对值，如math.fabs(-10) 返回10.0                  |
| floor(x)        | 返回数字的下舍整数，如math.floor(4.9)返回 4                  |
| log(x)          | 如math.log(math.e)返回1.0,math.log(100,10)返回2.0            |
| log10(x)        | 返回以10为基数的x的对数，如math.log10(100)返回 2.0           |
| max(x1,x2,...) | 返回给定参数的最大值，参数可以为序列。                       |
| min(x1,x2,...) | 返回给定参数的最小值，参数可以为序列。                       |
| modf(x)         | 返回x的整数部分与小数部分，两部分的数值符号与x相同，整数部分以浮点型表示。 |
| pow(x, y)       | x**y 运算后的值。                                            |
| round(x [,n\])  | 返回浮点数 x 的四舍五入值，如给出 n 值，则代表舍入到小数点后的位数。**其实准确的说是保留值将保留到离上一位更近的一端。** |
| sqrt(x)         | 返回数字x的平方根。                                          |



# 简单语句&复合语句

## 简单语句

简单语句只包含一个逻辑行。

1. 表达式语句

    表达式本身可以为语句。这在表达式为函数调用或文档字符串时特别有用。

    示例：

    "This module contains SPAM-related functions."

2. 断言语句

    断言语句检查条件是否满足，如果不满足，就引发 AssertionError 异常（并可提供错误消息）。

    示例：

    assert age >= 12, 'Children under the age of 12 are not allowed'

3. 赋值语句

    赋值语句将变量与值关联起来。可通过序列解包同时给多个变量赋值，还可进行链式赋值。

    示例：

    x = 42 # 简单赋值

    name, age = 'Gumby', 60 # 序列解包

    x = y = z = 10 # 链式赋值

4. 增强赋值语句

    可使用运算符来增强赋值。在这种情况下，将对变量的当前值和指定的值执行运算符指定的运算，并将变量重新关联到结果。如果原来的值是可变的，可能修改原来的值（并让变量依然关联到原来的值）。

5. pass 语句

    pass 语句不执行任何操作，可用作占位符。在语法要求的代码块中，如果你不想执行任何操作，可让它只包含 pass 语句。

6. del 语句

    del 语句用于解除变量和属性与值的关联以及将数据结构（映射或序列）的一部分（如（位置、切片或存储槽）删除。不能直接使用它来删除值，因为值只能通过垃圾收集来删除。

7. return 语句

    return 语句结束函数的执行并返回一个值。如果没有指定值，将返回 None 。

8. yield 语句

    yield 语句暂停执行生成器，并返回一个值。生成器是一种迭代器，可用于 for 循环中。

9. raise 语句

    raise 语句引发异常。调用它时可不提供任何参数（在 except 子句中用于重新引发当前捕获的异常），提供 Exception 的一个子类和一个可选参数（在这种情况下，将创建一个实例）或提供Exception 子类的一个实例。

10. break 语句

    break 语句结束它所属的循环语句（ for 或 while 语句），并接着执行该循环语句后面的语句。

11. continue 语句

    continue 语句类似于 break 语句，但结束所属循环的当前迭代而不是整个循环，即跳到下一次迭代开头继续执行。

12. import 语句

    import 语句用于从外部模块导入名称（与函数、类或其他值相关联的变量）。这也包括 from__future__ import 语句，它们用于导入在未来的Python版本中将包含在标准中的功能。

13. global 语句

    global 语句用于将变量标记为全局的。在函数中，可使用它给全局变量重新赋值。使用 global语句通常被视为糟糕的编程风格，因此应尽可能避免。

14. nonlocal 语句

    类似于 global 语句，但引用内部函数（闭包）的外部作用域。换而言之，如果你在一个函数内定义了另一个函数并返回它，这个函数就可引用并修改外部函数中的变量，条件是使用nonlocal 来标记它。

## 复合语句

复合语句包含一组其他的语句（代码块）。

1. if 语句

    if 语句用于有条件地执行，可包含 elif 和 else 子句。
    示例：

    ```python
    if x < 10:
    	print('Less than ten')
    elif 10 <= x < 20:
    	print('Less than twenty')
    else:
    	print('Twenty or more')
    ```

2. while 语句

    while 语句用于在指定条件为真时反复地执行（循环），可包含 else 子句［这种子句将在循环正常结束（如没有执行任何 break 和 return 语句）时执行］。

    示例：

    ```python
    x = 1
    while x < 100:
    	x *= 2
    	print(x)
    ```

3. for 语句

    for 语句用于对序列的元素或其他可迭代对象（包含返回迭代器的方法 __iter__ 的对象）反复地执行（循环），可包含 else 子句［这种子句将在循环正常结束（如没有执行任何 break 和 return语句）时执行］。

    ```python
    for i in range(10, 0, -1):
    print(i)
    print('Ignition!')
    ```

4. try 语句

    try 语句用于执行可能发生异常的代码段，让程序能够捕获这些异常并执行异常处理代码。try 语句可包含多个 except 子句（用于处理异常）和 finally 子句（这种子句不管情况如何都将执行，可用于执行清理工作）。

    ```python
    try:
    	1 / 0
    except ZeroDivisionError:
    	print("Can't divide anything by zero.")
    else:
        print("OK")
    finally:
    	print("Done trying to calculate 1 / 0")
    ```

5. with 语句

    with 语句用于包装使用上下文管理器的代码块，让管理器能够执行一些设置和清理操作。例如，可将文件用作上下文管理器，这样它们将在执行清理工作时关闭自己。

    ```python
    with open("somefile.txt") as myfile:
    	dosomething(myfile)
    # 到这里时文件已关闭
    ```

6. 函数定义

    函数定义用于创建函数对象以及将全局或局部变量与函数对象关联起来。

    ```python
    def double(x):
    	return x * 2
    ```

7. 类定义

    类定义用于创建类对象以及将全局或局部变量与类对象关联起来。

    ```python
    class Doubler:
    	def __init__ (self, value):
    		self.value = value
    	def double(self):
    		self.value *= 2
    ```


# 内置函数

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

print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)    
- objects -- 复数，表示可以一次输出多个对象。输出多个对象时，需要用 , 分隔。
- sep -- 用来间隔多个对象，默认值是一个空格。
- end -- 用来设定以什么结尾。默认值是换行符 \n，我们可以换成其他字符串。
- file -- 要写入的文件对象。
- flush -- 输出是否被缓存通常决定于 file，但如果 flush 关键字参数为 True，流会被强制刷新。  

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

sorted(iterable[, key]\[, reverse])   
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

# 匿名函数

匿名函数的语法结构:

```python
lambda [形参1], [形参2], ... : [单行表达式] 或 [函数调用]
```

```python
>>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

关键字lambda表示匿名函数，冒号前面的x表示函数参数

匿名函数有个限制，就是只能有一个表达式，不用写return，返回值就是该表达式的结果。匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数

```python
>>> f = lambda x: x * x
>>> f
<function <lambda> at 0x101c6ef28>
>>> f(5)
25
```

1. 匿名函数中**不能使用 if 语句、while 循环、for 循环**, 只能编写单行的表达式，或函数调用, 普通函数都可以.
2. 匿名函数中返回结果**不需要使用 return**, 表达式的运行结果就是返回结果, 普通函数返回结果必须 return.
3. 匿名函数中也可以不返回结果. 例如: `lambda : print('hello world')`

# 面向对象

⾯面向对象就是将编程当成是⼀一个事物，对外界来说，事物是直接使⽤用的，不不⽤用去管他内部
的情况。⽽而编程就是设置事物能够做什什么事。

**面向对象三⼤大特性: 封装, 继承, 多态**

**封装**
将属性和⽅方法书写到类的⾥里里⾯面的操作即为封装
封装可以为属性和⽅方法添加私有权限

**继承**

- ⼦子类默认拥有⽗父类的所有属性和⽅方法
- ⼦子类重写⽗父类同名⽅方法和属性
- ⼦子类调⽤用⽗父类同名⽅方法和属性

**多态**
传⼊入不不同的对象，产⽣生不不同的结果。多个子类中虽然都具有同一个方法，但是这些子类实例化的对象调用这些相同的方法后却可以获得完全不同的结果，多态性增强了软件的灵活性。（多态的概念依赖于继承）。

**类属性**

类属性就是 类对象 所拥有的属性，它被 该类的所有实例例对象 所共有。
类属性可以使⽤用 类对象 或 实例例对象 访问。

实例属性 要求 每个对象 为其 单独开辟一份内存空间 来记录数据，⽽而 类属性 为全类所共有
，仅占⽤用一份内存，更更加节省内存空间。

类属性只能通过类对象修改，不不能通过实例例对象修改，如果通过实例例对象修改类属性，表示的是创建了了
一个实例例属性。

**实例例属性**

实例属性不不能通过类访问

**类的私有属性**

**__private_attrs**：两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问。在类内部的方法中使用时 **self.__private_attrs**。

**类方法**

第一个形参是类对象的⽅方法
需要用装饰器器 @classmethod 来标识其为类⽅方法，对于类⽅方法，第一个参数必须是类对象，一般以
cls 作为第一个参数。

当方法中 需要使⽤用类对象 (如访问私有类属性等)时，定义类⽅方法

**静态方法**

需要通过装饰器器 @staticmethod 来进⾏行行修饰，静态⽅方法既不不需要传递类对象也不不需要传递实例例对象
（形参没有self/cls）。
静态方法 也能够通过 实例例对象 和 类对象 去访问。

当方法中 既不不需要使⽤用实例例对象(如实例例对象，实例例属性)，也不需要使⽤用类对象 (如类属性、类方
法、创建实例等)时，定义静态方法
取消不需要的参数传递，有利利于 减少不不必要的内存占⽤用和性能消耗

**类的私有方法**

**__private_method**：两个下划线开头，声明该方法为私有方法，只能在类的内部调用 ，不能在类的外部调用。**self.__private_methods**。

**在类定义中，对所有以两个下划线打头的名称都进行转换，即在开头加上一个下划线和类名。**

# 异常处理

**常见异常**

| 异常名称          | 描述                         |
| :---------------- | :--------------------------- |
| BaseException     | 所有异常的基类               |
| Exception         | 常规错误的基类               |
| StopIteration     | 迭代器没有更多的值           |
| ZeroDivisionError | 除(或取模)零 (所有数据类型)  |
| AssertionError    | 断言语句失败                 |
| AttributeError    | 对象没有这个属性             |
| IOError           | 输入/输出操作失败            |
| OSError           | 操作系统错误                 |
| ImportError       | 导入模块/对象失败            |
| IndexError        | 序列中没有此索引(index)      |
| KeyError          | 映射中没有这个键             |
| NameError         | 未声明/初始化对象 (没有属性) |
| SyntaxError       | Python 语法错误              |
| IndentationError  | 缩进错误                     |
| TabError          | Tab 和空格混用               |
| TypeError         | 对类型无效的操作             |
| ValueError        | 传入无效的参数               |

**`assert`断言**

用于判断一个表达式，在表达式条件为 false 的时候触发异常。断言可以在条件不满足程序运行的情况下直接返回错误，而不必等待程序运行后出现崩溃的情况

**`try/except`异常捕捉**

try 语句按照如下方式工作；

- 首先，执行 try 子句（在关键字 try 和关键字 except 之间的语句）。
- 如果没有异常发生，忽略 except 子句，try 子句执行后结束。
- 如果在执行 try 子句的过程中发生了异常，那么 try 子句余下的部分将被忽略。如果异常的类型和 except 之后的名称相符，那么对应的 except 子句将被执行。
- 如果一个异常没有与任何的 excep 匹配，那么这个异常将会传递给上层的 try 中。

一个 try 语句可能包含多个except子句，分别来处理不同的特定的异常。最多只有一个分支会被执行。

处理程序将只针对对应的 try 子句中的异常进行处理，而不是其他的 try 的处理程序中的异常。

```python
try:
    # 执行代码
    # TODO
except oneException as error:
    # 发生异常时执行代码
    # TODO
else:
    # 没有异常时执行代码
    # TODO
finally:
    # 不管有没有异常都会执行代码
    # TODO
```

**抛出异常**

```python
raise Exception(args)
```

```python
>>>try:
        raise NameError('HiThere')
    except NameError:
        print('An exception flew by!')
        raise
   
An exception flew by!
Traceback (most recent call last):
  File "<stdin>", line 2, in ?
NameError: HiThere
```

**自定义异常**

自定义异常类继承自 Exception 类，可以直接继承，或者间接继承

```python
>>>class MyError(Exception):
        def __init__(self, value):
            super().__init()
            self.value = value
        def __str__(self):
            return repr(self.value)
   
>>> try:
        raise MyError(2*2)
    except MyError as e:
        print('My exception occurred, value:', e.value)
   
My exception occurred, value: 4
>>> raise MyError('oops!')
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
__main__.MyError: 'oops!'
```

**预定义的清理行为**

一些对象定义了标准的清理行为，无论系统是否成功的使用了它，一旦不需要它了，那么这个标准的清理行为就会执行

```python 
# 迭代行为
for line in open("myfile.txt"):
    print(line, end="")

# 上下文管理
with open("myfile.txt") as f:
    for line in f:
        print(line, end="")
```

# 模块与包

**模块**

Python 模块(Module)，是一个 Python 文件，以 .py 结尾，包含了 Python 对象定义和Python语句。模块能定义函数，类和变量，模块里也能包含可执行的代码。

```python
import 模块名
from 模块名 import 功能名
from 模块名 import *
import 模块名 as 别名
from 模块名 import 功能名 as 别名
```

当你导入一个模块，Python解析器对模块位置的搜索顺序是：

1. 当前目录
2. 如果不在当前目录，Python则搜索在shell变量PYTHONPATH下的每个目录。
3. 如果都找不到，Python会察看默认路径。UNIX下，默认路径一般为/usr/local/lib/python/
4. 模块搜索路径存储在system模块的sys.path变量中。变量里包含当前目录，PYTHONPATH和由安装过程决定的默认目录。

如果一个模块文件中有 \_\_all\_\_(一个列表,元素是当前文件的变量名的字符串形式)变量，当使用 from xxx import * 导入时，只能导入这个列表中的元素。

当前文件有一个变量\_\_name\_\_,如果是在本文件中执行,\_\_name\_\_值为"\_\_main\_\_",如果在其他模块导入值为文件名

**包**

包将有联系的模块组织在一起，即放到同一个文件夹下，并且在这个文件夹创建一个名字为 \_\_init\_\_.py 文件，那么这个文件夹就称之为包。

# 文件操作

## open()函数

open() 方法用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数,如果该文件无法被打开，会抛出 OSError。基本语法格式如下:

open(file, mode)  # 简写语法

open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

>**file**：一个 path-like object，表示将要打开的文件的路径。  
>**mode**：打开文件的模式  
>**encoding**: 一般使用utf8 ,不指定编码无法识别汉字 
>**errors**: 是一个可选的字符串参数，用于指定如何处理编码和解码错误 - 这不能在二进制模式下使用。  
>**newline**: 区分换行符,,默认根据平台来辨别换行符  
>**closefd**: 传入的file参数类型  
>**buffering**: 设置缓冲.传递0以切换缓冲关闭（仅允许在二进制模式下），1选择行缓冲（仅在文本模式下可用），并且>1的整数以指示固定大小的块缓冲区的大小（以字节为单位）。如果没有给出 buffering 参数，则默认缓冲策略的工作方式如下:  
>
>>二进制文件以固定大小的块进行缓冲；使用启发式方法选择缓冲区的大小，尝试确定底层设备的“块大小”或使用 io.DEFAULT_BUFFER_SIZE。在许多系统上，缓冲区的长度通常为4096或8192字节。
>>“交互式”文本文件（ isatty() 返回 True 的文件）使用行缓冲。其他文本文件使用上述策略用于二进制文件。

## 文件打开模式

| 模式 | 描述                                                         |
| :--- | :----------------------------------------------------------- |
| t    | 文本模式 (默认)。                                            |
| x    | 写模式，新建一个文件，如果该文件已存在则会报错。             |
| b    | 二进制模式。                                                 |
| +    | 打开一个文件进行更新(可读可写)。                             |
| U    | 通用换行模式（不推荐）。                                     |
| r    | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。 |
| rb   | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。一般用于非文本文件如图片等。 |
| r+   | 打开一个文件用于读写。文件指针将会放在文件的开头。           |
| rb+  | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。一般用于非文本文件如图片等。 |
| w    | 打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| wb   | 以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。 |
| w+   | 打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。 |
| a    | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| ab   | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| a+   | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
| ab+  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |

|    模式    |  r   |  r+  |  w   |  w+  |  a   |  a+  |
| :--------: | :--: | :--: | :--: | :--: | :--: | :--: |
|     读     |  +   |  +   |      |  +   |      |  +   |
|     写     |      |  +   |  +   |  +   |  +   |  +   |
|    创建    |      |      |  +   |  +   |  +   |  +   |
|    覆盖    |      |      |  +   |  +   |      |      |
| 指针在开始 |  +   |  +   |  +   |  +   |      |      |
| 指针在结尾 |      |      |      |      |  +   |  +   |

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

## 文件对象的属性

| 属性                | 描述                                      |
| :------------------ | :---------------------------------------- |
| file.closed         | 返回true如果文件已被关闭，否则返回false。 |
| file.mode           | 返回被打开文件的访问模式。                |
| file.name           | 返回文件的名称。                          |
| file.encoding       | 返回文件的编码                            |
| file.newlines       | 返回文件的换行符                          |
| file.line_buffering | 返回bool.是否设置了缓冲                   |

## 文件对象的方法

f.read([,size])

\- 读取一定数目的数据, 然后作为字符串或字节对象返回。默认读取整个文件.在文本模式下size指的是多少个字符,在二进制模式下是多少个字节

f.readline(size)

\- 从文件中读取单独的一行。换行符为 '\n'。f.readline() 如果返回一个空字符串, 说明已经已经读取到最后一行。如果指定size而一行未读完,再次调用readline时会接着上次的继续读取直到遇到换行符,如果指定的size大于一行数据,则读到行尾就不再继续读取

```python
# 文本
1:www.ru\noob.com
2:www.runoob.com
3:www.runoob.com
4:www.runoob.com
5:www.runoob.com

# 代码
fo = open("runoob.txt", "r+")
line = fo.readline()
print ("读取第一行 %s" % (line))  # 文本中写的\n会被转义为\\\\n,不会被认为是换行符
line = fo.readline(5)
print ("读取的字符串为: %s" % (line))
print(fo.readline(9))
print(fo.readline(9))
fo.close()

# 输出
读取第一行 1:www.runoob.com

读取的字符串为: 2:www
.runoob.c
om
```



f.readlines() 

\- 将返回该文件中包含的所有行。如果设置可选参数 sizehint, 则读取指定长度的字节, 并且将这些字节按行分割。

f.write(string) 

\- 将 string 写入到文件中, 然后返回写入的字符数

f.tell() 

\- 返回文件对象当前所处的位置, 它是从文件开头开始算起的字节数/字符数。

f.seek(offset, from_what)

\- 如果要改变文件指针当前的位置, from_what 的值, 如果是 0 表示开头, 如果是 1 表示当前位置, 2 表示文件的结尾，from_what 值为默认为0，即文件开头。offset表示偏移当前指针的字节数.只对"b"模式有用,"t"模式只允许从文件头开始计算位置

 	seek(x,0) ： 从起始位置即文件首行首字符开始移动 x 个字符

​	 seek(x,1) ： 表示从当前位置往后移动x个字符

​	 seek(-x,2)：表示从文件的结尾往前移动x个字符

```python 
>>> f = open("a.txt", "r+", encoding="utf8")       
>>> f.read(8)       
'zxy\n#abc'
>>> f.tell()	       
8
>>> f.write("nihao") # "nihao"被写入文件末尾       
5
>>> f.close()

# seek改变指针位置之后,如果进行写操作,将会覆盖指针后边相应长度的字符/字节
>>> f = open("a.txt", "r+", encoding="utf8")	       
>>> f.seek(8)	       
8
>>> f.write("youdu")  # "youdu"从第八个字符位置覆盖五个字符写入       
5
>>> f.close()
```

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

# 生成器和迭代器

## 迭代器

可以直接作用于for循环的对象统称为**可迭代对象**：Iterable

可以使用isinstance()判断一个对象是否是Iterable对象



可以被next()函数调用并不断返回下一个值的对象称为**迭代器**：Iterator

可以使用isinstance()判断一个对象是否是Iterator对象



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

迭代器是一个可以记住遍历的位置的对象。

迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。

迭代器有两个基本的方法：iter() 和 next()。



list、dict、str虽然是Iterable，却不是Iterator。可以使用iter()函数将它们变成迭代器

```python
>>> from collections import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
```



Python的Iterator对象表示的是一个数据流，Iterator对象可以被next()函数调用并不断返回下一个数据，直到没有数据时抛出StopIteration错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过next()函数实现按需计算下一个数据，所以Iterator的计算是惰性的，只有在需要返回下一个数据时它才会计算。

## 生成器

创建一个包含100万个元素的列表，占用很大的存储空间，生成器可以在需要用到的时候生成一个值给我们，节省空间

在 Python 中，使用了 **yield** 的函数被称为**生成器**（generator）。生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。

在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。

调用一个生成器函数，返回的是一个迭代器对象。

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

yield关键字有两点作用：

- 保存当前运行状态（断点），然后暂停执行，即将生成器（函数）挂起
- 将yield关键字后面表达式的值作为返回值返回，此时可以理解为起到了return的作用

Python3中的生成器可以使用return返回最终运行的返回值，而Python2中的生成器不允许使用return返回一个返回值（即可以使用return从生成器中退出，但return后不能有任何表达式）

除了可以使用next()函数来唤醒生成器继续执行外，还可以使用send()函数来唤醒执行。使用send()函数的一个好处是可以在唤醒的同时向断点处传入一个附加数据。

```python
In [10]: def gen():
   ....:     i = 0
   ....:     while i<5:
   ....:         temp = yield i
   ....:         print(temp)
   ....:         i+=1
   ....:
    In [43]: f = gen()

In [44]: next(f)
Out[44]: 0

In [45]: f.send('haha')
haha
Out[45]: 1

In [46]: next(f)
None
Out[46]: 2

In [47]: f.send('haha')
haha
Out[47]: 3
    
执行到yield时，gen函数作用暂时保存，返回i的值; temp接收下次c.send("python")发送过来的值; c.next()等价c.send(None)   
注意：send()作为生成器的起始唤醒函数时，得传一个None值 ，不能传别的值 也不能不传值
```



## for...in...循环的本质

`for item in Iterable` 循环的本质就是先通过iter()函数获取可迭代对象Iterable的迭代器，然后对获取到的迭代器不断调用next()方法来获取下一个值并将其赋值给item，当遇到StopIteration的异常后循环结束。

# 命名空间和作用域

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

# 闭包与装饰器

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

# property属性

Python内置的@property装饰器就是负责把一个方法变成属性调用

property属性的定义和调用要注意一下几点：

定义时，在实例方法的基础上添加 @property 装饰器；并且仅有一个self参数

调用时，无需括号

两种方式：

装饰器 即：在方法上应用装饰器

类属性 即：在类中定义值为property对象的类属性

## 装饰器方式

在类的实例方法上应用@property装饰器

```python
class Goods:
    """python3中默认继承object类
        以python2、3执行此程序的结果不同，因为只有在python3中才有@xxx.setter  @xxx.deleter
    """
    @property
    def price(self):
        print('@property')

    @price.setter
    def price(self, value):
        print('@price.setter')

    @price.deleter
    def price(self):
        print('@price.deleter')

obj = Goods()
obj.price          # 自动执行 @property 修饰的 price 方法，并获取方法的返回值
obj.price = 123    # 自动执行 @price.setter 修饰的 price 方法，并将  123 赋值给方法的参数
del obj.price      # 自动执行 @price.deleter 修饰的 price 方法
```
经典类中的属性只有一种访问方式，其对应被 @property 修饰的方法
新式类中的属性有三种访问方式，并分别对应了三个被@property、@方法名.setter、@方法名.deleter修饰的方法

## 类属性方式

创建值为property对象的类属性，当使用类属性的方式创建property属性时，经典类和新式类无区别

property方法中有个四个参数

第一个参数是方法名，调用 对象.属性 时自动触发执行方法

第二个参数是方法名，调用 对象.属性 ＝ XXX 时自动触发执行方法

第三个参数是方法名，调用 del 对象.属性 时自动触发执行方法

第四个参数是字符串，调用 对象.属性.\_\_doc\_\_ ，此参数是该属性的描述信息

```python
#coding=utf-8
class Foo(object):
    def get_bar(self):
        print("getter...")
        return 'laowang'

    def set_bar(self, value): 
        """必须两个参数"""
        print("setter...")
        return 'set value' + value

    def del_bar(self):
        print("deleter...")
        return 'laowang'

    BAR = property(get_bar, set_bar, del_bar, "description...")

obj = Foo()

obj.BAR  # 自动调用第一个参数中定义的方法：get_bar
obj.BAR = "alex"  # 自动调用第二个参数中定义的方法：set_bar方法，并将“alex”当作参数传入
desc = Foo.BAR.__doc__  # 自动获取第四个参数中设置的值：description...
print(desc)
del obj.BAR  # 自动调用第三个参数中定义的方法：del_bar方法
```
property 其实并不是函数，而是一个类。它的实例包含一些魔法方法，而所有的魔法都

是由这些方法完成的。这些魔法方法为 \_\_get\_\_ 、 \_\_set\_\_ 和 \_\_delete\_\_ ，它们一道定义了所谓

的描述符协议。只要对象实现了这些方法中的任何一个，它就是一个描述符。描述符的独特

之处在于其访问方式。例如，读取属性（具体来说，是在实例中访问类中定义的属性）时，如

果它关联的是一个实现了 \_\_get\_\_ 的对象，将不会返回这个对象，而是调用方法 \_\_get\_\_ 并将

其结果返回。

# 魔法方法

```python
1. __doc__
表示类的描述信息
class Foo:
    """ 描述类信息，这是用于看片的神奇 """
    def func(self):
        pass

print(Foo.__doc__)
#输出：类的描述信息

2. __module__ 和 __class__
__module__ 表示当前操作的对象在那个模块
__class__ 表示当前操作的对象的类是什么
test.py

# -*- coding:utf-8 -*-

class Person(object):
    def __init__(self):
        self.name = 'laowang'

5. __call__
对象后面加括号，触发执行。
注：__init__方法的执行是由创建对象触发的，即：对象 = 类名() ；而对于 __call__ 方法的执行是由对象后加括号触发的，即：对象() 或者 类()()

class Foo:
    def __init__(self):
        pass

    def __call__(self, *args, **kwargs):
        print('__call__')


obj = Foo()  # 执行 __init__
obj()  # 执行 __call__

6.__getitem__、__setitem__、__delitem__
用于索引操作，如字典。以上分别表示获取、设置、删除数据
# -*- coding:utf-8 -*-

class Foo(object):

    def __getitem__(self, key):
        print('__getitem__', key)

    def __setitem__(self, key, value):
        print('__setitem__', key, value)

    def __delitem__(self, key):
        print('__delitem__', key)


obj = Foo()

result = obj['k1']      # 自动触发执行 __getitem__
obj['k2'] = 'laowang'   # 自动触发执行 __setitem__
del obj['k1']           # 自动触发执行 __delitem__

7.__getslice__、__setslice__、__delslice__
该三个方法用于分片操作，如：列表
# -*- coding:utf-8 -*-

class Foo(object):

    def __getslice__(self, i, j):
        print('__getslice__', i, j)

    def __setslice__(self, i, j, sequence):
        print('__setslice__', i, j)

    def __delslice__(self, i, j):
        print('__delslice__', i, j)

obj = Foo()

obj[-1:1]                   # 自动触发执行 __getslice__
obj[0:1] = [11,22,33,44]    # 自动触发执行 __setslice__
del obj[0:2]                # 自动触发执行 __delslice__

8.__str__
该方法需要 return 一个数据，并且只有self一个参数，当在类的外部 print(对象) 则打印这个数据;如果没有定义该方法,则默认打印对象的内存地址

9.__del__()
当删除对象时，python解释器会默认调用__del__()方法
```
# with与上下文管理器

对于系统资源如文件、数据库连接、socket 而言，应用程序打开这些资源并执行完业务逻辑之后，必须做的一件事就是要关闭（断开）该资源。

**上下文管理器**

任何实现了 \_\_enter\_\_() 和 \_\_exit\_\_() 方法的对象都可称之为上下文管理器，上下文管理器对象可以使用 with 关键字。\_\_enter\_\_() 方法返回资源对象，这里就是你将要打开的那个文件对象，\_\_exit\_\_() 方法处理一些清除工作。

```python
class File():

    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode

    def __enter__(self):
        print("entering")
        self.f = open(self.filename, self.mode)
        return self.f

    def __exit__(self, *args):
        print("will exit")
        self.f.close()

# 调用
with File('out.txt', 'w') as f:
    print("writing")
    f.write('hello, python') 
```

- `__enter__`表示上文方法，需要返回一个操作文件对象
- `__exit__`表示下文方法，with语句执行完成会自动执行，即使出现异常也会执行该方法

Python 提供了一个 contextmanager 的装饰器，更进一步简化了上下文管理器的实现方式。通过 yield 将函数分割成两部分，yield 之前的语句在 \_\_enter\_\_ 方法中执行，yield 之后的语句在 \_\_exit\_\_ 方法中执行。紧跟在 yield 后面的值是函数的返回值。  

```python
from contextlib import contextmanager

@contextmanager
def my_open(path, mode):
    f = open(path, mode)
    yield f
    f.close()   

# 调用
with my_open('out.txt', 'w') as f:
    f.write("hello , the simplest context manager") 
```

# 元类

元类就是用来创建类的类。在大多数编程语言中，类就是一组用来描述如何生成一个对象的代码段，但是，Python中的类还远不止如此。类同样也是一种对象。当你使用关键字class时Python在幕后做的事情，而这就是通过元类来实现的。它是创建类对象的

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

# PEP8

PEP8 提供了 Python 代码的编写约定. 本节知识点旨在提高代码的可读性, 并使其在各种 Python 代码中编写风格保持一致.

1. 缩进使用4个空格, 空格是首选的缩进方式. Python3 不允许混合使用制表符和空格来缩进.

2. 每一行最大长度限制在79个字符以内.

3. 顶层函数、类的定义, 前后使用两个空行隔开.

4. import 导入

    - 导入建议在不同的行, 例如:

    ```python
    import os
    import sys
    # 不建议如下导包
    import os, sys
    # 但是可以如下:
    from subprocess import Popen, PIPE
    ```

    - 导包位于文件顶部, 在模块注释、文档字符串之后, 全局变量、常量之前. 导入按照以下顺序分组:

        ​	标准库导入

        ​	相关第三方导入

        ​	本地应用/库导入

        ​	在每一组导入之间加入空行

5. Python 中定义字符串使用双引号、单引号是相同的, 尽量保持使用同一方式定义字符串. 当一个字符串包含单引号或者双引号时, 在最外层使用不同的符号来避免使用反斜杠转义, 从而提高可读性.

6. 表达式和语句中的空格:

    1. 避免在小括号、方括号、花括号后跟空格.
    2. 避免在逗号、分好、冒号之前添加空格.
    3. 冒号在切片中就像二元运算符, 两边要有相同数量的空格. 如果某个切片参数省略, 空格也省略.
    4. 避免为了和另外一个赋值语句对齐, 在赋值运算符附加多个空格.
    5. 避免在表达式尾部添加空格, 因为尾部空格通常看不见, 会产生混乱.
    6. 总是在二元运算符两边加一个空格, 赋值（=），增量赋值（+=，-=），比较（==,<,>,!=,<>,<=,>=,in,not,in,is,is not），布尔（and, or, not

7. 避免将小的代码块和 if/for/while 放在同一行, 要避免代码行太长.

    ```python
    if foo == 'blah': do_blah_thing()
    for x in lst: total += x
    while t < 10: t = delay()
    ```

8. 永远不要使用字母 'l'(小写的L), 'O'(大写的O), 或者 'I'(大写的I) 作为单字符变量名. 在有些字体里, 这些字符无法和数字0和1区分, 如果想用 'l', 用 'L' 代替.

9. 类名一般使用首字母大写的约定.

10. 函数名应该小写, 如果想提高可读性可以用下划线分隔.

11. 如果函数的参数名和已有的关键词冲突, 在最后加单一下划线比缩写或随意拼写更好. 因此 class_ 比 clss 更好.(也许最好用同义词来避免这种冲突).

12. 方法名和实例变量使用下划线分割的小写单词, 以提高可读性.