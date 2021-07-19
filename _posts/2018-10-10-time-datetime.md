---
title: Time,Datetime,Calendar模块
date: 2018-10-10 19:07:44 +0800
categories: [Python,标准库]
tags: [Python标准库]
---
# datetime模块

## 一. datetime模块介绍

datetime模块包含如下类：

- date：日期对象,属性有year, month, day
- time：时间对象, 属性有hour, minute, second, microsecond,和tzinfo。
- datetime：日期时间对象,属性：year, month, day, hour, minute, second, microsecond,和tzinfo.
- timedelta：表示两个时间对象的时间间隔，即两个时间点之间的长度,精确到微秒
    datetime模块中包含的常量
- MAXYEAR：返回能表示的最大年份，datetime.MAXYEAR=9999
- MINYEAR：返回能表示的最小年份，datetime.MINYEAR=1

## 二.date类

date对象表示一个日期，由year年份、month月份及day日期三部分构成

datetime.date（year，month，day)

所有的参数都是必需的，参数可以是整数，并且在以下范围内：

MINYEAR <= year <= MAXYEAR（也就是 1 ~ 9999）

1 <= month <= 12

1 <= day <= 根据 year 和 month 来决定（例如 2015年2月 只有 28 天）

**date类方法**

```
date.today() - 返回一个表示当前本地日期的 date 对象
date.fromtimestamp(timestamp) - 根据给定的时间戮，返回一个 date 对象
```

**date 实例方法**

```
date.replace(year, month, day)
- 生成一个新的日期对象，用参数指定的年、月、日代替原有对象中的属性

date.timetuple()
- 返回日期对应的 time.struct_time 对象（类似于 time 模块的 time.localtime()）

date.isocalendar()
- 返回一个三元组格式 (year, month, day)

date.isoformat()
- 返回一个 ISO 8601 格式的日期字符串，如 "YYYY-MM-DD" 的字符串

date.ctime()
- 返回一个表示日期的字符串，相当于 time 模块的 time.ctime(time.mktime(d.timetuple()))

date.strftime(format)
- 返回自定义格式化字符串表示日期
```

## 三.time类

time 对象表示一天中的一个时间，并且可以通过 tzinfo 对象进行调整

datetime.time(hour=0, minute=0, second=0, microsecond=0, tzinfo=None)

所有的参数都是可选的；tzinfo 可以是 None 或者 tzinfo 子类的实例对象；其余的参数可以是整数，并且在以下范围内：

0 <= hour < 24

0 <= minute < 60

0 <= second < 60

0 <= microsecond < 1000000

注：如果参数超出范围，将引发 ValueError 异常

**time 实例方法**

```
time.replace([hour[, minute[, second[, microsecond[, tzinfo]]]]])
- 生成一个新的时间对象，用参数指定时间代替原有对象中的属性

time.isoformat()
- 返回一个 ISO 8601 格式的日期字符串，如 "HH:MM:SS.mmmmmm" 的字符串

time.strftime(format)
- 返回自定义格式化字符串表示时间
```

## 四.datetime类

datetime 对象是 date 对象和 time 对象的结合体，并且包含他们的所有信息

datetime.datetime(year, month, day, hour=0, minute=0, second=0, microsecond=0, tzinfo=None)

必须的参数是 year（年）、month（月）、day（日）

**datetime 类方法**

```
datetime.today()
- 返回一个表示当前本地时间的 datetime 对象

datetime.combine(date, time)
- 根据参数 date 和 time，创建一个 datetime 对象

datetime.fromtimestamp(timestamp, tz=None)
- 根据时间戮创建一个 datetime 对象，参数 tz 指定时区信息

datetime.date()
- 返回一个 date 对象datetime.time() - 返回一个 time 对象（tzinfo 属性为 None）

datetime.timestamp()
- 返回当前时间的时间戳（类似于 time 模块的 time.time()）

datetime.isocalendar()
- 返回一个三元组格式 (year, month, day)

datetime.datetime.now()
- 获取当前时间

datetime.datetime.utcnow()
- 获取当前的UTC时间

datetime.datetime(year,month,day,hour,minute,second)
- 获取指定时间

datetime.datetime.now().timetuple()
- 转换成元组
```

**计算时间差**

```python
直接将两个datetime类型时间相减，能够得到时间差
>>>a = datetime.datetime.now()
>>>b = datetime.datetime.now()
>>>c = b - a
datetime.timedelta(0, 8, 336790)        # 0天8秒336790毫秒
```

```python
datetime.strftime()
- 转换成格式化时间字符串
>>>datetime.datetime.now().strftime('%a, %b %d %H:%M')
'Tue, Oct 30 15:46'
```

```python
datetime.strptime()
- 格式化时间字符串转换成datetime类型
>>>datetime.datetime.now().strptime('2018-10-30 15:49:00', '%Y-%m-%d %H:%M:%S')
datetime.datetime(2018, 10, 30, 15, 49)


年  %y	用两个数字表示年份（例如 2014年 == 14）
    %Y	用四个数字表示年份
月  %m	月份（01, 02, ..., 12）
    %b	月份的简写（一月 ~ 十二月：Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec）
    %B	月份的全写（一月 ~ 十二月：January, February, March, April, May, June, July, August, September,October, November, December）
日  %d	在一个月中的第几天（01, 02, ..., 31）
周  %a	星期的简写（星期一 ~ 天：Mon, Tue, Wed, Thu, Fri, Sat, Sun）
    %A	星期的全写（星期一 ~ 天：Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday）
    %w	在一个星期中的第几天（ 0 表示星期天 ... 6 表示星期六）
时  %H	二十四小时制（00, 01, ..., 23）
    %I	十二小时制（01, 02, ..., 11）
    %p	AM 或者 PM
分  %M	分钟（00, 01, ..., 59）
秒  %S	秒（00, 01, ..., 59） 
```

## 五.timedelta 类

timedelta 对象表示两个 date 或者 time 的时间间隔。

```python
>>>from datetime import timedelta
>>>d = datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)  # 所有的参数都是可选的并且默认为 0。这些参数可以是整数或者浮点数，也可以是正数或者负数。
```

支持时间对象的加减乘除操作,返回的还是时间对象

# time模块

属性：

- time.timezone是当地时区（未启动夏令时）距离格林威治的偏移秒数（>0，美洲;<=0大部分欧洲，亚洲，非洲）。
- time.tzname包含一对根据情况的不同而不同的字符串，分别是带夏令时的本地时区名称，和不带的。

时间间隔以秒为单位

时间戳都以自从 1970 年 1 月 1 日午夜（历元）经过了多长时间来表示。如1540808367.8872325

时间元组struct_time(tm_year, tm_mon, tm_mday, tm_hour, tm_min, tm_sec, tm_wday, tm_yday, tm_isdst)

```
0	tm_year 2008
1	tm_mon	1 到 12
2	tm_mday	1 到 31
3	tm_hour	0 到 23
4	tm_min	0 到 59
5	tm_sec	0 到 61 (60或61 是闰秒)
6	tm_wday	0到6 (0是周一)
7	tm_yday	一年中的第几天，1 到 366
8	tm_isdst	是否为夏令时，值有：1(夏令时)、0(不是夏令时)、-1(未知)，默认 -1
```

```python
time.time()
    该函数返回当前时间的时间戳，也就是距离1970年1月1日00:00:00的差值。
>>>import time
>>>time.time()
1540808367.8872325
```

```python
time.localtime()
    该函数能将一个时间戳转换成元组的形式，如果没有指定时间戳，默认使用当前时间的时间戳。需要注意的是返回的时间是当地时间。
>>>import time
>>>time.localtime(1540808367.8872325)
time.struct_time(tm_year=2018, tm_mon=10, tm_mday=29, tm_hour=18, tm_min=19, tm_sec=27, tm_wday=0, tm_yday=302, tm_isdst=0)
>>>time.localtime()
time.struct_time(tm_year=2018, tm_mon=10, tm_mday=29, tm_hour=18, tm_min=26, tm_sec=10, tm_wday=0, 
```

```python
time.gmtime()
    该函数和localtime()的功能一样，只是它返回的时间是格林威治天文时间（UTC），也就是世界标准时间。中国时间为UTC+8。
```

```python
time.mktime()
    该函数将一个元组转换成时间戳
>>>import time
>>>time.mktime(time.localtime())
1540809214.0
```

```python
time.asctime()
    该函数将一个元组转换成格式化时间。如果没有传入参数，默认传入time.localtime()
>>>import time
>>>time.asctime()
'Mon Oct 29 18:39:10 2018'
```

```python
time.ctime()
    该函数将一个时间戳转换成格式化时间。如果没有传入参数，默认传入time.time()。
>>>import time
>>>time.ctime()
'Mon Oct 29 18:41:04 2018'
```

```python
time.strftime()
    该函数按照格式化字符把一个元组转换成格式化时间字符串。如果没有传入参数，默认传入time.localtime()。
>>>import time
>>>time.strftime("%Y-%m-%d %X", time.localtime())
'2018-10-29 18:46:14'
```

```python
time.strptime()
    该函数按照格式化字符把一个格式化时间字符串转成元组。
>>>import time
>>>time.strptime('2018-10-29 18:46:14', '%Y-%m-%d %X')
time.struct_time(tm_year=2018, tm_mon=10, tm_mday=29, tm_hour=18, tm_min=46, tm_sec=14, tm_wday=0, tm_yday=302, tm_isdst=-1)
```

```python
time.sleep()
    该函数能让程序线程暂停休息，传入几秒，休息几秒
import time
print(time.time())
time.sleep(3)
print(time.time())
```

python中时间日期格式化符号：

```
%a     本地星期名称的简写（如星期四为Thu）      
%A  本地星期名称的全称（如星期四为Thursday）      
%b  本地月份名称的简写（如八月份为agu）    
%B  本地月份名称的全称（如八月份为august）       
%c  本地相应的日期和时间的字符串表示（如：15/08/27 10:20:06）       
%d  一个月中的第几天（01 - 31）  
%f  微妙（范围0.999999）    
%H  一天中的第几个小时（24小时制，00 - 23）       
%I  第几个小时（12小时制，0 - 11）       
%j  一年中的第几天（001 - 366）     
%m  月份（01 - 12）    
%M  分钟数（00 - 59）       
%p  本地am或者pm的相应符      
%S  秒（00 - 61）    
%U  一年中的星期数(00 - 53星期天是一个星期的开始)第一个星期天之    前的所有天数都放在第0周。     
%w  一个星期中的第几天（0 - 6，0是星期天）    
%W  和%U基本相同，不同的是%W以星期一为一个星期的开始。    
%x  本地相应日期字符串（如15/08/01）     
%X  本地相应时间字符串（如08:08:10）     
%y  去掉世纪的年份（00 - 99）两个数字表示的年份       
%Y  完整的年份（4个数字表示年份）
%z  与UTC时间的间隔（如果是本地时间，返回空字符串）
%Z  时区的名字（如果是本地时间，返回空字符串）       
%%  ‘%’字符 
```

# calendar模块

此模块的函数都是日历相关的，例如打印某月的字符月历。星期一是默认的每周第一天，星期天是默认的最后一天。

```python
calendar.calendar(year,w=2,l=1,c=6)
    该函数返回某年的日历，3个月一行，间隔距离为c。 每日宽度间隔为w字符。每行长度为21* W+18+2* C。l是每星期行数。

calendar.month(year,month,w=2,l=1)
    该函数返回某年某月的日历。两行标题，一周一行。每日宽度间隔为w字符。每行的长度为7* w+6。l是每星期的行数。

calendar.isleap(year)
    该函数可以判断某年是不是闰年

calendar.leapdays(year1， year2)
    该函数返回某两年之间的闰年总数

calendar.monthcalendar(year, month)
    该函数以嵌套列表的形式返回某年某个月的日历
>>>import calendar
>>>calendar.monthcalendar(2018, 10)
[[1, 2, 3, 4, 5, 6, 7],
 [8, 9, 10, 11, 12, 13, 14],
 [15, 16, 17, 18, 19, 20, 21],
 [22, 23, 24, 25, 26, 27, 28],
 [29, 30, 31, 0, 0, 0, 0]]

calendar.monthrange(year, month)
    该函数以元组形式返回两个整数，第一个数为某月第一天为星期几，第二个数为该月有多少天
>>>import calendar
>>>calendar.monthrange(2018, 10)
(0, 31)  # 0表示星期一，31表示有31天

calendar.timegm()
    该函数将一个元组时间变成时间戳
>>>import time
>>>import calendar
>>>calendar.timegm(time.localtime())
1540926234

calendar.firstweekday( )
    返回当前每周起始日期的设置。默认情况下，首次载入caendar模块时返回0，即星期一。

calendar.weekday(year,month,day)
    返回给定日期的日期码。0（星期一）到6（星期日）。月份为 1（一月） 到 12（12月）。
```



