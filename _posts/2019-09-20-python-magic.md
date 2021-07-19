---
title: Python 魔法方法 指南
date: 2019-09-20 17:41:00 +0800
categories: [Python,Python进阶]
tags: [Python进阶]
---

什么是魔法方法呢？魔法方法就是可以给你的类增加魔力的特殊方法，如果你的对象实现（重载）了这些方法中的某一个，那么这个方法就会在特殊的情况下被 Python 所调用，你可以定义自己想要的行为，而这一切都是自动发生的。它们经常是两个下划线包围来命名的（比如\_\_init\_\_， \_\_lt\_\_ ）


- zzz
{:toc}

## Ⅰ.	构造方法

\_\_init\_\_指明一个对象初始化的行为，然而，当我们调用 x = SomeClass() 的时候，\_\_init\_\_并不是第一个被调用的方法。事实上，第一个被调用的是\_\_new\_\_，这个 方法才真正地创建了实例。当这个对象的生命周期结束的时候， \_\_del\_\_会被调用。

###  \_\_new\_\_（cls, [...]）

\_\_new\_\_是对象实例化时第一个调用的方法，它只取下 cls 参数，并把其他参数传给\_\_init\_\_。单例类场景可以重写此方法，美多项目短信验证功能有用到

### \_\_init\_\_（self, [...]）

类的初始化方法。它获取任何传给构造器的参数（比如我们调用 x = SomeClass(10, ‘foo’) ，\_\_init\_\_就会接到参数 10 和 ‘foo’ 。\_\_init\_\_ 在Python的类定义中用的最多。

### \_\_del\_\_（self）

\_\_new\_\_ 和 \_\_init\_\_ 是对象的构造器， \_\_del\_\_ 是对象的销毁器。它并非实现了语句 del x (因此该语句不等同于 x.\_\_del\_\_())。而是定义了当对象被垃圾回收时的行为。 当对象需要在销毁时做一些处理的时候这个方法很有用，比如 socket 对象、文件对象。但是需要注意的是，当Python解释器退出但对象仍然存活的时候， \_\_del\_\_ 并不会 执行。 所以养成一个手工清理的好习惯是很重要的，比如及时关闭连接。

```python
from os.path import join

class FileObject:
    '''文件对象的装饰类，用来保证文件被删除时能够正确关闭。'''

    def __init__(self, filepath='~', filename='sample.txt'):
        # 使用读写模式打开filepath中的filename文件
        self.file = open(join(filepath, filename), 'r+')

    def __del__(self):
        self.file.close()
        del self.file
```



## Ⅱ.	比较操作符

### \_\_cmp\_\_(self, other)

\_\_cmp\_\_ 是所有比较魔法方法中最基础的一个，它实际上定义了所有比较操作符的行为（<,==,!=,等等），但是它可能不能按照你需要的方式工作（例如，判断一个实例和另一个实例是否相等采用一套标准，而与判断一个实例是否大于另一实例采用另一套）。 \_\_cmp\_\_ 应该在 self < other 时返回一个负整数，在 self == other 时返回0，在 self > other 时返回正整数。最好只定义你所需要的比较形式，而不是一次定义全部。 如果你需要实现所有的比较形式，而且它们的判断标准类似，那么 \_\_cmp\_\_ 是一个很好的方法，可以减少代码重复，让代码更简洁。python3中被取消

### \_\_eq\_\_(self, other)

定义等于操作符(==)的行为。

### \_\_ne\_\_(self, other)

定义不等于操作符(!=)的行为。

### \_\_lt\_\_(self, other)

定义小于操作符(<)的行为。

### \_\_gt\_\_(self, other)

定义大于操作符(>)的行为。

### \_\_le\_\_(self, other)

定义小于等于操作符(<)的行为。

### \_\_ge\_\_(self, other)

定义大于等于操作符(>)的行为。

```python
class Word(str):
    '''单词类，按照单词长度来定义比较行为'''

    def __new__(cls, word):
        # 注意，我们只能使用 __new__ ，因为str是不可变类型
        # 所以我们必须提前初始化它（在实例创建时）
        if ' ' in word:
            print "Value contains spaces. Truncating to first space."
            word = word[:word.index(' ')]
            # Word现在包含第一个空格前的所有字母
        return str.__new__(cls, word)

    def __gt__(self, other):
        return len(self) > len(other)
    def __lt__(self, other):
        return len(self) < len(other)
    def __ge__(self, other):
        return len(self) >= len(other)
    def __le__(self, other):
        return len(self) <= len(other)
```



## Ⅲ.	一元操作符

### \_\_pos\_\_(self)

实现取正操作，例如 +some_object。

### \_\_neg\_\_(self)

实现取负操作，例如 -some_object。

### \_\_abs\_\_(self)

实现内建绝对值函数 abs() 操作。

### \_\_invert\_\_(self)

实现取反操作符 ~

### \_\_round\_\_(self， n)

实现内建函数 round() ，n 是近似小数点的位数。

### \_\_floor\_\_(self)

实现 math.floor() 函数，即向下取整。

### \_\_ceil\_\_(self)

实现 math.ceil() 函数，即向上取整

### \_\_trunc\_\_(self)

实现 math.trunc() 函数，即距离零最近的整数。



## Ⅳ.	算术操作符

### \_\_add\_\_(self, other)

实现加法操作。

### \_\_sub\_\_(self, other)

实现减法操作。

### \_\_mul\_\_(self, other)

实现乘法操作。

### \_\_floordiv\_\_(self, other)

实现使用 // 操作符的整数除法。

### \_\_div\_\_(self, other)

实现使用 / 操作符的除法。python3中被取消

### \_\_truediv\_\_(self, other)

实现 _true_ 除法，这个函数只有使用 from \_\_future\_\_ import division 时才有作用。

### \_\_mod\_\_(self, other)

实现 % 取余操作。

### \_\_divmod\_\_(self, other)

实现 divmod 内建函数。

### \_\_pow\_\_

实现  操作符。

### \_\_lshift\_\_(self, other)

实现左移位运算符 << 。

### \_\_rshift\_\_(self, other)

实现右移位运算符 >> 。

### \_\_and\_\_(self, other)

实现按位与运算符 & 。

### \_\_or\_\_(self, other)

实现按位或运算符 \| 。

### \_\_xor\_\_(self, other)

实现按位异或运算符 ^ 。



## Ⅴ.	反射算数运算符

### \_\_radd\_\_(self, other)

实现反射加法操作。

### \_\_rsub\_\_(self, other)

实现反射减法操作。

### \_\_rmul\_\_(self, other)

实现反射乘法操作。

### \_\_rfloordiv\_\_(self, other)

实现使用 // 操作符的整数反射除法。

### \_\_rdiv\_\_(self, other)

实现使用 / 操作符的反射除法。

### \_\_rtruediv\_\_(self, other)

实现 _true_ 反射除法，这个函数只有使用 from \_\_future\_\_ import division 时才有作用。

### \_\_rmod\_\_(self, other)

实现 % 反射取余操作符。

### \_\_rdivmod\_\_(self, other)

实现调用 divmod(other, self) 时 divmod 内建函数的操作。

### \_\_rpow\_\_

实现  反射操作符。

### \_\_rlshift\_\_(self, other)

实现反射左移位运算符 << 的作用。

### \_\_rshift\_\_(self, other)

实现反射右移位运算符 >> 的作用。

### \_\_rand\_\_(self, other)

实现反射按位与运算符 & 。

### \_\_ror\_\_(self, other)

实现反射按位或运算符 | 。

### \_\_rxor\_\_(self, other)

实现反射按位异或运算符 ^ 。



## Ⅵ.	增强赋值运算符

### \_\_iadd\_\_(self, other)

实现加法赋值操作。

### \_\_isub\_\_(self, other)

实现减法赋值操作。

### \_\_imul\_\_(self, other)

实现乘法赋值操作。

### \_\_ifloordiv\_\_(self, other)

实现使用 //= 操作符的整数除法赋值操作。

### \_\_idiv\_\_(self, other)

实现使用 /= 操作符的除法赋值操作。

### \_\_itruediv\_\_(self, other)

实现 _true_ 除法赋值操作，这个函数只有使用 from \_\_future\_\_ import division 时才有作用。

### \_\_imod\_\_(self, other)

实现 %= 取余赋值操作。

### \_\_ipow\_\_

实现 = 操作。

### \_\_ilshift\_\_(self, other)

实现左移位赋值运算符 <<= 。

### \_\_irshift\_\_(self, other)

实现右移位赋值运算符 >>= 。

### \_\_iand\_\_(self, other)

实现按位与运算符 &= 。

### \_\_ior\_\_(self, other)

实现按位或赋值运算符 | 。

### \_\_ixor\_\_(self, other)

实现按位异或赋值运算符 ^= 。



## Ⅶ.	类型转换操作符

### \_\_int\_\_(self)

实现到int的类型转换。

### \_\_long\_\_(self)

实现到long的类型转换。

### \_\_float\_\_(self)

实现到float的类型转换。

### \_\_complex\_\_(self)

实现到complex的类型转换。

### \_\_oct\_\_(self)

实现到八进制数的类型转换。

### \_\_hex\_\_(self)

实现到十六进制数的类型转换。

### \_\_index\_\_(self)

实现当对象用于切片表达式时到一个整数的类型转换。如果你定义了一个可能会用于切片操作的数值类型，你应该定义 \_\_index\_\_。

### \_\_trunc\_\_(self)

当调用 math.trunc(self) 时调用该方法， \_\_trunc\_\_ 应该返回 self 截取到一个整数类型（通常是long类型）的值。

### \_\_coerce\_\_(self)

该方法用于实现混合模式算数运算，如果不能进行类型转换， \_\_coerce\_\_ 应该返回 None 。反之，它应该返回一个二元组 self 和 other ，这两者均已被转换成相同的类型。python3中被取消



## Ⅷ.	类的表示

### \_\_str\_\_(self)

定义对类的实例调用 str() 时的行为。

### \_\_repr\_\_(self)

定义对类的实例调用 repr() 时的行为。 str() 和 repr() 最主要的差别在于“目标用户”。 repr() 的作用是产生机器可读的输出（大部分情况下，其输出可以作为有效的Python代码），而 str() 则产生人类可读的输出。

### \_\_unicode\_\_(self)

定义对类的实例调用 unicode() 时的行为。 unicode() 和 str() 很像，只是它返回unicode字符串。注意，如果调用者试图调用 str() 而你的类只实现了 \_\_unicode\_\_() ，那么类将不能正常工作。所有你应该总是定义 \_\_str\_\_() ，以防有些人没有闲情雅致来使用unicode。python3中\_\_unicode\_\_被取消掉了

### \_\_format\_\_(self)

定义当类的实例用于新式字符串格式化时的行为，例如， “Hello, 0:abc!”.format(a) 会导致调用 a.\_\_format\_\_(“abc”) 。当定义你自己的数值类型或字符串类型时，你可能想提供某些特殊的格式化选项，这种情况下这个魔法方法会非常有用。

### \_\_hash\_\_(self)

定义对类的实例调用 hash() 时的行为。它必须返回一个整数，其结果会被用于字典中键的快速比较。同时注意一点，实现这个魔法方法通常也需要实现 \_\_eq\_\_ ，并且遵守如下的规则： a == b 意味着 hash(a) == hash(b)。

### \_\_nonzero\_\_(self)

定义对类的实例调用 bool() 时的行为，根据你自己对类的设计，针对不同的实例，这个魔法方法应该相应地返回True或False。python3中被重命名成\_\_bool\_\_

### \_\_dir\_\_(self)

定义对类的实例调用 dir() 时的行为，这个方法应该向调用者返回一个属性列表。一般来说，没必要自己实现 \_\_dir\_\_ 。但是如果你重定义了 \_\_getattr\_\_ 或者 \_\_getattribute\_\_ （下个部分会介绍），乃至使用动态生成的属性，以实现类的交互式使用，那么这个魔法方法是必不可少的。



## Ⅸ.	访问控制

### \_\_getattr\_\_(self, name)

当用户试图访问一个根本不存在（或者暂时不存在）的属性时，你可以通过这个魔法方法来定义类的行为。这个可以用于捕捉错误的拼写并且给出指引，使用废弃属性时给出警告（如果你愿意，仍然可以计算并且返回该属性），以及灵活地处理AttributeError。只有当试图访问不存在的属性时它才会被调用，所以这不能算是一个真正的封装的办法。

### \_\_setattr\_\_(self, name, value)

和 \_\_getattr\_\_ 不同， \_\_setattr\_\_ 可以用于真正意义上的封装。它允许你自定义某个属性的赋值行为，不管这个属性存在与否，也就是说你可以对任意属性的任何变化都定义自己的规则。然后，一定要小心使用 \_\_setattr\_\_ ，这个列表最后的例子中会有所展示。

### \_\_delattr\_\_(self, name)

这个魔法方法和 \_\_setattr\_\_ 几乎相同，只不过它是用于处理删除属性时的行为。和 _setattr\_\_ 一样，使用它时也需要多加小心，防止产生无限递归（在 \_\_delattr\_\_ 的实现中调用 del self.name 会导致无限递归）。

### \_\_getattribute\_\_(self, name)

\_\_getattribute\_\_看起来和上面那些方法很合得来，但是最好不要使用它。 \_\_getattribute\_\_ 只能用于新式类。在最新版的Python中所有的类都是新式类，在老版Python中你可以通过继承 object 来创建新式类。 \_\_getattribute\_\_ 允许你自定义属性被访问时的行为，它也同样可能遇到无限递归问题（通过调用基类的 \_\_getattribute\_\_ 来避免）。 \_\_getattribute\_\_ 基本上可以替代 \_\_getattr\_\_ 。只有当它被实现，并且显式地被调用，或者产生 AttributeError 时它才被使用。 这个魔法方法可以被使用（毕竟，选择权在你自己），我不推荐你使用它，因为它的使用范围相对有限（通常我们想要在赋值时进行特殊操作，而不是取值时），而且实现这个方法很容易出现Bug。

自定义这些控制属性访问的魔法方法很容易导致问题，考虑下面这个例子:

```python
def __setattr__(self, name. value):
    self.name = value
    # 因为每次属性幅值都要调用 __setattr__()，所以这里的实现会导致递归
    # 这里的调用实际上是 self.__setattr('name', value)。因为这个方法一直
    # 在调用自己，因此递归将持续进行，直到程序崩溃

def __setattr__(self, name, value):
    self.__dict__[name] = value # 使用 __dict__ 进行赋值
    # 定义自定义行为
```

下面的例子展示了实际应用中某些特殊的属性访问方法(注意我们之所以使用 super 是因为不是所有的类都有 __dict__ 属性)

```python
class AccessCounter(object):
    ''' 一个包含了一个值并且实现了访问计数器的类
    每次值的变化都会导致计数器自增'''

    def __init__(self, val):
            super(AccessCounter, self).__setattr__('counter', 0)
            super(AccessCounter, self).__setattr__('value', val)

    def __setattr__(self, name, value):
            if name == 'value':
                    super(AccessCounter, self).__setattr_('counter', self.counter + 1)
        # 使计数器自增变成不可避免
        # 如果你想阻止其他属性的赋值行为
        # 产生 AttributeError(name) 就可以了
        super(AccessCounter, self).__setattr__(name, value)

    def __delattr__(self, name):
            if name == 'value':
                    super(AccessCounter, self).__setattr('counter', self.counter + 1)
                    super(AccessCounter, self).__delattr(name)
```



## Ⅹ.	自定义序列

既然讲到创建自己的序列类型，就不得不说一说协议了。协议类似某些语言中的接口，里面包含的是一些必须实现的方法。在Python中，协议完全是非正式的，也不需要显式的声明，事实上，它们更像是一种参考标准。

为什么我们要讲协议？因为在Python中实现自定义容器类型需要用到一些协议。首先，不可变容器类型有如下协议：想实现一个不可变容器，你需要定义 \_\_len\_\_ 和 \_\_getitem\_\_ (后面会具体说明）。可变容器的协议除了上面提到的两个方法之外，还需要定义 \_\_setitem\_\_ 和 \_\_delitem\_\_ 。最后，如果你想让你的对象可以迭代，你需要定义 \_\_iter\_\_ ，这个方法返回一个迭代器。迭代器必须遵守迭代器协议，需要定义 \_\_iter\_\_ （返回它自己）和 next 方法。

### \_\_len\_\_(self)

返回容器的长度，可变和不可变类型都需要实现。

### \_\_getitem\_\_(self, key)

定义对容器中某一项使用 self[key] 的方式进行读取操作时的行为。这也是可变和不可变容器类型都需要实现的一个方法。它应该在键的类型错误式产生 TypeError 异常，同时在没有与键值相匹配的内容时产生 KeyError 异常。

### \_\_setitem\_\_(self, key)

定义对容器中某一项使用 self[key] 的方式进行赋值操作时的行为。它是可变容器类型必须实现的一个方法，同样应该在合适的时候产生 KeyError 和 TypeError 异常。

### \_\_iter\_\_(self, key)

它应该返回当前容器的一个迭代器。迭代器以一连串内容的形式返回，最常见的是使用 iter() 函数调用，以及在类似 for x in container: 的循环中被调用。迭代器是他们自己的对象，需要定义 \_\_iter\_\_ 方法并在其中返回自己。

### \_\_reversed\_\_(self)

定义了对容器使用 reversed() 内建函数时的行为。它应该返回一个反转之后的序列。当你的序列类是有序时，类似列表和元组，再实现这个方法，

### \_\_contains\_\_(self, item)

\_\_contains\_\_ 定义了使用 in 和 not in 进行成员测试时类的行为。你可能好奇为什么这个方法不是序列协议的一部分，原因是，如果 \_\_contains\_\_ 没有定义，Python就会迭代整个序列，如果找到了需要的一项就返回 True 。

### \_\_missing\_\_(self ,key)

\_\_missing\_\_ 在字典的子类中使用，它定义了当试图访问一个字典中不存在的键时的行为（目前为止是指字典的实例，例如我有一个字典 d ， “george” 不是字典中的一个键，当试图访问 d[“george’] 时就会调用 d.\_\_missing\_\_(“george”) ）。

```python
# 一个实现了一些函数式结构的列表
class FunctionalList:
    '''一个列表的封装类，实现了一些额外的函数式
    方法，例如head, tail, init, last, drop和take。'''

    def __init__(self, values=None):
        if values is None:
            self.values = []
        else:
            self.values = values

    def __len__(self):
        return len(self.values)

    def __getitem__(self, key):
        # 如果键的类型或值不合法，列表会返回异常
        return self.values[key]

    def __setitem__(self, key, value):
        self.values[key] = value

    def __delitem__(self, key):
        del self.values[key]

    def __iter__(self):
        return iter(self.values)

    def __reversed__(self):
        return reversed(self.values)

    def append(self, value):
        self.values.append(value)

    def head(self):
        # 取得第一个元素
        return self.values[0]

    def tail(self):
        # 取得除第一个元素外的所有元素
        return self.valuse[1:]

    def init(self):
        # 取得除最后一个元素外的所有元素
        return self.values[:-1]

    def last(self):
        # 取得最后一个元素
        return self.values[-1]

    def drop(self, n):
        # 取得除前n个元素外的所有元素
        return self.values[n:]

    def take(self, n):
        # 取得前n个元素
        return self.values[:n]
```



## Ⅺ.	反射

### \_\_instancecheck\_\_(self, instance)

检查一个实例是否是你定义的类的一个实例（例如 isinstance(instance, class) ）。

### \_\_subclasscheck\_\_(self, subclass)

检查一个类是否是你定义的类的子类（例如 issubclass(subclass, class) ）。



## Ⅻ.	抽象基类

### 见参考文档	http://docs.python.org/2/library/abc.html



## XIII.	可调用的对象

Python中一个特殊的魔法方法允许你自己类的对象表现得像是函数，然后你就可以“调用”它们，把它们传递到使用函数做参数的函数中，等等等等。这是另一个强大而且方便的特性

### \_\_call\_\_(self, [args...])

允许类的一个实例像函数那样被调用。本质上这代表了 x() 和 x.\_\_call\_\_() 是相同的。注意 \_\_call\_\_ 可以有多个参数，这代表你可以像定义其他任何函数一样，定义 \_\_call\_\_ ，喜欢用多少参数就用多少。

\_\_call\_\_ 在某些需要经常改变状态的类的实例中显得特别有用。“调用”这个实例来改变它的状态，是一种更加符合直觉，也更加优雅的方法。

```python
class Entity:
        '''表示一个实体的类，调用它的实例
        可以更新实体的位置'''

        def __init__(self, size, x, y):
                self.x, self.y = x, y
                self.size = size

        def __call__(self, x, y):
                '''改变实体的位置'''
                self.x, self.y = x, y
```



## XIV.	上下文管理器

当对象使用 with 声明创建时，上下文管理器允许类做一些设置和清理工作。上下文管理器的行为由两个魔法方法所定义：

### \_\_enter\_\_(self)

定义使用 with 声明创建的语句块最开始上下文管理器应该做些什么。注意 \_\_enter\_\_ 的返回值会赋给 with 声明的目标，也就是 as 之后的东西。

### \_\_exit\_\_(self, exception_type, exception_value, traceback)

定义当 with 声明语句块执行完毕（或终止）时上下文管理器的行为。它可以用来处理异常，进行清理，或者做其他应该在语句块结束之后立刻执行的工作。如果语句块顺利执行， exception_type , exception_value 和 traceback 会是 None 。否则，你可以选择处理这个异常或者让用户来处理。如果你想处理异常，确保 \_\_exit\_\_ 在完成工作之后返回 True 。如果你不想处理异常，那就让它发生吧。

```python
class Closer:
    '''一个上下文管理器，可以在with语句中
    使用close()自动关闭对象'''

    def __init__(self, obj):
        self.obj = obj

    def __enter__(self, obj):
        return self.obj # 绑定到目标

    def __exit__(self, exception_type, exception_value, traceback):
        try:
                self.obj.close()
        except AttributeError: # obj不是可关闭的
                print 'Not closable.'
                return True # 成功地处理了异常

            
>>> from magicmethods import Closer
>>> from ftplib import FTP
>>> with Closer(FTP('ftp.somesite.com')) as conn:
...         conn.dir()
...
# 为了简单，省略了某些输出
>>> conn.dir()
# 很长的 AttributeError 信息，不能使用一个已关闭的连接
>>> with Closer(int(5)) as i:
...         i += 1
...
Not closable.
>>> i
6    
```



## XV.	创建描述符对象

描述符是一个类，当使用取值，赋值和删除 时它可以改变其他对象。描述符不是用来单独使用的，它们需要被一个拥有者类所包含。描述符可以用来创建面向对象数据库，以及创建某些属性之间互相依赖的类。描述符在表现具有不同单位的属性，或者需要计算的属性时显得特别有用（例如表现一个坐标系中的点的类，其中的距离原点的距离这种属性）。

要想成为一个描述符，一个类必须具有实现 \_\_get\_\_ , \_\_set\_\_ 和 \_\_delete\_\_ 三个方法中至少一个。

### \_\_get\_\_(self, instance, owner)

定义当试图取出描述符的值时的行为。 instance 是拥有者类的实例， owner 是拥有者类本身。

### \_\_set\_\_(self, instance, owner)

定义当描述符的值改变时的行为。 instance 是拥有者类的实例， value 是要赋给描述符的值。

### \_\_delete\_\_(self, instance, owner)

定义当描述符的值被删除时的行为。 instance 是拥有者类的实例

```python
class Meter(object):
    '''米的描述符。'''

    def __init__(self, value=0.0):
        self.value = float(value)
    def __get__(self, instance, owner):
        return self.value
    def __set__(self, instance, owner):
            self.value = float(value)

class Foot(object):
    '''英尺的描述符。'''

    def __get(self, instance, owner):
            return instance.meter * 3.2808
    def __set(self, instance, value):
            instance.meter = float(value) / 3.2808

class Distance(object):
    '''用于描述距离的类，包含英尺和米两个描述符。'''
    meter = Meter()
    foot = Foot()
```



## XVI.	拷贝

有些时候，特别是处理可变对象时，你可能想拷贝一个对象，改变这个对象而不影响原有的对象。这时就需要用到Python的 copy 模块了。然而（幸运的是），Python模块并不具有感知能力， 因此我们不用担心某天基于Linux的机器人崛起。但是我们的确需要告诉Python如何有效率地拷贝对象。

### \_\_copy\_\_(self)

定义对类的实例使用 copy.copy() 时的行为。 copy.copy() 返回一个对象的浅拷贝，这意味着拷贝出的实例是全新的，然而里面的数据全都是引用的。也就是说，对象本身是拷贝的，但是它的数据还是引用的（所以浅拷贝中的数据更改会影响原对象）。

### \_\_deepcopy\_\_(self, memodict=)

定义对类的实例使用 copy.deepcopy() 时的行为。 copy.deepcopy() 返回一个对象的深拷贝，这个对象和它的数据全都被拷贝了一份。 memodict 是一个先前拷贝对象的缓存，它优化了拷贝过程，而且可以防止拷贝递归数据结构时产生无限递归。当你想深拷贝一个单独的属性时，在那个属性上调用 copy.deepcopy() ，使用 memodict 作为第一个参数。



## XVII.	Pickling

Pickling是Python数据结构的序列化过程，当你想存储一个对象稍后再取出读取时，Pickling会显得十分有用。然而它同样也是担忧和混淆的主要来源。

Pickling是如此的重要，以至于它不仅仅有自己的模块（ pickle ），还有自己的协议和魔法方法。

Pickle不仅仅可以用于内建类型，任何遵守pickle协议的类都可以被pickle。Pickle协议有四个可选方法，可以让类自定义它们的行为

### \_\_getinitargs\_\_(self)

如果你想让你的类在反pickle时调用 \_\_init\_\_ ，你可以定义 \_\_getinitargs\_\_(self) ，它会返回一个参数元组，这个元组会传递给 \_\_init\_\_ 。注意，这个方法只能用于旧式类。

### \_\_getnewargs\_\_(self)

对新式类来说，你可以通过这个方法改变类在反pickle时传递给 \_\_new\_\_ 的参数。这个方法应该返回一个参数元组。

### \_\_getstate\_\_(self)

你可以自定义对象被pickle时被存储的状态，而不使用对象的 \_\_dict\_\_ 属性。 这个状态在对象被反pickle时会被 \_\_setstate\_\_ 使用。

### \_\_setstate\_\_(self)

当一个对象被反pickle时，如果定义了 \_\_setstate\_\_ ，对象的状态会传递给这个魔法方法，而不是直接应用到对象的 \_\_dict\_\_ 属性。这个魔法方法和 \_\_getstate\_\_ 相互依存：当这两个方法都被定义时，你可以在Pickle时使用任何方法保存对象的任何状态。

### \_\_reduce\_\_(self)

当定义扩展类型时（也就是使用Python的C语言API实现的类型），如果你想pickle它们，你必须告诉Python如何pickle它们。 \_\_reduce\_\_ 被定义之后，当对象被Pickle时就会被调用。它要么返回一个代表全局名称的字符串，Pyhton会查找它并pickle，要么返回一个元组。这个元组包含2到5个元素，其中包括：一个可调用的对象，用于重建对象时调用；一个参数元素，供那个可调用对象使用；被传递给 \_\_setstate\_\_ 的状态（可选）；一个产生被pickle的列表元素的迭代器（可选）；一个产生被pickle的字典元素的迭代器（可选）；

### \_\_reduce_ex\_\_(self)

\_\_reduce_ex\_\_ 的存在是为了兼容性。如果它被定义，在pickle时 \_\_reduce_ex\_\_ 会代替 \_\_reduce\_\_ 被调用。 \_\_reduce\_\_ 也可以被定义，用于不支持 \_\_reduce_ex\_\_ 的旧版pickle的API调用。

```python
import time

class Slate:
        '''存储一个字符串和一个变更日志的类
        每次被pickle都会忘记它当前的值'''

        def __init__(self, value):
                self.value = value
                self.last_change = time.asctime()
                self.history = {}

        def change(self, new_value):
                # 改变当前值，将上一个值记录到历史
                self.history[self.last_change] = self.value
                self.value = new_value)
                self.last_change = time.asctime()

        def print_change(self):
                print 'Changelog for Slate object:'
                for k,v in self.history.items():
                        print '%s\t %s' % (k,v)

        def __getstate__(self):
                # 故意不返回self.value或self.last_change
                # 我们想在反pickle时得到一个空白的slate
                return self.history

        def __setstate__(self):
                # 使self.history = slate，last_change
                # 和value为未定义
                self.history = state
                self.value, self.last_change = None, None
```



## 附录1.	如何调用魔法方法

一些魔法方法直接和内建函数对应，这种情况下，如何调用它们是显而易见的。然而，另外的情况下，调用魔法方法的途径并不是那么明显。这个附录旨在展示那些不那么明显的调用魔法方法的语法。

| 魔法方法                            | 什么时候被调用                   | 解释                         |
| :---------------------------------- | :------------------------------- | ---------------------------- |
| \_\_new\_\_(cls [,...])             | instance = MyClass(arg1, arg2)   | \_\_new\_\_在实例创建时调用  |
| \_\_init\_\_(self [,...])           | instance = MyClass(arg1,arg2)    | \_\_init\_\_在实例创建时调用 |
| \_\_cmp\_\_(self)                   | self == other, self > other 等   | 进行比较时调用               |
| \_\_pos\_\_(self)                   | +self                            | 一元加法符号                 |
| \_\_neg\_\_(self)                   | -self                            | 一元减法符号                 |
| \_\_invert\_\_(self)                | ~self                            | 按位取反                     |
| \_\_index\_\_(self)                 | x[self]                          | 当对象用于索引时             |
| \_\_nonzero\_\_(self)               | bool(self)                       | 对象的布尔值                 |
| \_\_getattr\_\_(self, name)         | self.name #name不存在            | 访问不存在的属性             |
| \_\_setattr\_\_(self, name)         | self.name = val                  | 给属性赋值                   |
| \_\_delattr_(self, name)            | del self.name                    | 删除属性                     |
| \_\_getattribute\_\_(self,name)     | self.name                        | 访问任意属性                 |
| \_\_getitem\_\_(self, key)          | self[key]                        | 使用索引访问某个元素         |
| \_\_setitem\_\_(self, key)          | self[key] = val                  | 使用索引给某个元素赋值       |
| \_\_delitem\_\_(self, key)          | del self[key]                    | 使用索引删除某个对象         |
| \_\_iter\_\_(self)                  | for x in self                    | 迭代                         |
| \_\_contains\_\_(self, value)       | value in self, value not in self | 使用in进行成员测试           |
| \_\_call\_\_(self [,...])           | self(args)                       | “调用”一个实例               |
| \_\_enter\_\_(self)                 | with self as x:                  | with声明的上下文管理器       |
| \_\_exit\_\_(self, exc, val, trace) | with self as x:                  | with声明的上下文管理器       |
| \_\_getstate\_\_(self)              | pickle.dump(pkl_file, self)      | Pickling                     |
| \_\_setstate\_\_(self)              | data = pickle.load(pkl_file)     | Pickling                     |



## 附录2.	Python 3中的变化

在这里，我们记录了几个在对象模型方面 Python 3 和 Python 2.x 之间的主要区别。

◆	Python 3中string和unicode的区别不复存在，因此 \_\_unicode\_\_ 被取消了， \_\_bytes\_\_ 加入进来（与Python 2.7 中的 \_\_str\_\_ 和 \_\_unicode\_\_ 行为类似），用于新的创建字节数组的内建方法。

◆	Python 3中默认除法变成了 true 除法，因此 \_\_div\_\_ 被取消了。

◆	\_\_coerce\_\_ 被取消了，因为和其他魔法方法有功能上的重复，以及本身行为令人迷惑。

◆	\_\_cmp\_\_ 被取消了，因为和其他魔法方法有功能上的重复。

◆	\_\_nonzero\_\_ 被重命名成 \_\_bool\_\_ 。



整理并转自:https://pyzh.readthedocs.io/en/latest/python-magic-methods-guide.html