---
layout: post
title: 内置数据类型常用方法
Author: TheronZhao
tags: Python
---

- zzz
{:toc}


## 列表 List

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

## 集合 Set

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

## 字典 Dict

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

## 元组 Tuple

元组与列表类似，不同之处在于**元组的元素不能修改**  

tup.index(x)  
查找某个元素，如果数据存在返回对应的下标，否则报错

tup.count(x)  
统计某个元素在当前元组出现的次数。

## 字符串 String

### 格式相关 操作方法

string.capitalize()  
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

### 查询相关  操作方法

string.count(sub[, start[, end]])  
计算子串 sub 出现的次数，可搜索范围限定为 string[start:end]

string.partition(sep)  
在字符串中搜索 sep ，并返回元组 (sep 前面的部分 , sep, sep 后面的部分 )

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

### 切割替换 操作方法

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

### 进行判断  操作方法

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

### 其他   操作方法

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

### 字符串的格式化

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

## 数字 Number

Python 支持四种不同的数值类型:  
    整型(Int) - 通常被称为是整型或整数，是正或负整数，不带小数点。
    长整型(long integers) - 无限大小的整数，整数最后是一个大写或小写的L。
    浮点型(floating point real values) - 浮点型由整数部分与小数部分组成，浮点型也可以使用科学计数法表示（2.5e2 = 2.5 x 102 = 250）
    复数(complex numbers) - 复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型。

int(x [,base ])         将x转换为一个整数  
long(x [,base ])        将x转换为一个长整数  
float(x )               将x转换到一个浮点数  
complex(real [,imag ])  创建一个复数  
str(x )                 将对象 x 转换为字符串  
repr(x )                将对象 x 转换为表达式字符串  
eval(str )              用来计算在字符串中的有效Python表达式,并返回一个对象  
tuple(s )               将序列 s 转换为一个元组  
list(s )                将序列 s 转换为一个列表  
chr(x )                 将一个整数转换为一个字符  
unichr(x )              将一个整数转换为Unicode字符  
ord(x )                 将一个字符转换为它的整数值  
hex(x )                 将一个整数转换为一个十六进制字符串  
oct(x )                 将一个整数转换为一个八进制字符串  



abs(x)	            返回数字的绝对值，如abs(-10) 返回 10  
cmp(x, y)	        如果 x < y 返回 -1, 如果 x == y 返回 0, 如果 x > y 返回 1  
max(x1, x2,...)	    返回给定参数的最大值，参数可以为序列。  
min(x1, x2,...)	    返回给定参数的最小值，参数可以为序列。  
pow(x, y)	        x**y 运算后的值。  
round(x [,n])	    返回浮点数x的四舍五入值，如给出n值，则代表舍入到小数点后的位数。

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