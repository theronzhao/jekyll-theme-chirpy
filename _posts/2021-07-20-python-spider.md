---
title: 爬虫与requests库
date: 2021-06-20 21:11:21 +0800
categories: [网络爬虫]
tags: [网络爬虫,requests]
---

# HTTP基础

网络爬虫(网页蜘蛛，网络机器人)，就是模拟客户端(主要指的的是浏览器)发送网络请求，接收请求响应，一种按照一定规则，自动的抓取互联网信息的程序

HTTP请求头：

- User-Agent：用户代理，提供浏览器信息和系统信息 
- Referer：页面跳转处，指的是这个请求从哪个url页面跳转过来的
- Cookie：cookie状态保持
- Authorization：用于表示HTTP协议中需要认证资源的认证信息，如jwt token
- Host：域名
- Connection：长链接keep-alive
- Updrade-Insecure-Requests：升级为https请求
- Content-Type:内容类型

Response响应头：

- Set-Cookie：对方服务器设置cookie到用户浏览器的缓存
- Retry-After：有时候服务器识别爬虫程序之后虽然正常返回响应数据，但是会给一个503状态码(服务器由于维护或者负载过重未能应答),让爬虫以为爬取失败

所有的状态码都不可信，一切以是否从抓包得到的响应中获取到数据为准

network中抓包得到的源码才是判断依据，elements中的源码时渲染之后的源码，不能作为判断标准

http请求的过程

> 1. 浏览器将域名发送给dns解析成对应的ip地址，拿到对应的ip地址后先向地址栏中的url发起请求，并获取响应
> 2. 返回的响应内容html中，会有css,js,图片等url地址，以及ajax代码，浏览器会按照响应内容中的顺序依次发送其他的请求，并获取相应的响应
> 3. 浏览器每获取一个响应就对展示出的结果进行添加(加载)，js,css等内容会修改页面的内容，js也可以重新发送请求，获取响应
> 4. 从获取第一个响应并在浏览器中展示，直到最终获取全部响应，并在展示的结果中添加内容或修改----这个过程叫做浏览器的渲染

# requests库

## 基本方法

```python 
requests.request(method, url, **kwargs)

requests.get(url, params=None, **kwargs)
requests.head(url, **kwargs)
requests.post(url, data=None, json=None, **kwargs)
requests.put(url, data=None, **kwargs)
requests.patch(url, data=None, **kwargs)
requests.delete(url, **kwargs)
```

- method：请求方式，有GET，HEAD， POST， PUT, PATCH, delete, OPTIONS。传参的时候以字符串写，各个方法的名字分别对应于http请求的对应请求方式
- url：要请求的网址
- **kwargs:控制访问参数，如下：

> params：字典或字节序列，作为参数增加到url中.对应于查询字符串参数
> data：字典/字节序列或文件对象形式，对应于请求体传参
> json：json格式的数据
> headers：字典，http请求头信息
> cookies：字典或cookie Jar
> auth：元组，支持http认证功能
> files：字典类型，传输文件
> timeout：设定超时时间，以秒为单位,规定时间内没有响应就会报错
> proxies：字典类型，设定访问代理服务器，可以增加登录认证
> allow_redirects：True/False，默认位True，重定向开关
> strea：True/False，默认True，获取内容立即下载开关
> verify：True/False,默认True,认证SSL证书开关。设置为False时可以忽略CA证书的认证
> cert：本地SSL证书路径

```python
proxies = {
    "http": "http://1.1.1.1:8888",
    "https": "http://1.1.1.1:8888",  # 代理按协议分为http代理，https代理，socks代理
}
代理使用成功不会有任何报错，能成功获取响应；如果失败，要么程序一直卡滞要么报错      
```

关于post请求的表单数据来源：

1. 固定值：抓包比较不变值
2. 输入值：抓包比较根据自身变化值
3. 预设值-静态文件：需要提前从html静态文件中获取
4. 预设值-发请求：需要对指定地址发请求以获取
5. 在客户端生成的：分析js代码，模拟生成数据

## requests.session

该类能够自动处理发送请求获取响应过程中产生的cookie，进而达到状态保持的目的

requests.session自动处理cookie，下一次请求会带上前一次的cookie。session实例在请求了一个网站后，对方服务器设置在本地的cookie会保存再session中，下一次请求的时候会自动带上

```python
session = requests.session()  # 实例化session对象
response = session.get(url, headers, ...)
response = session.post(url, data, ...)
```

## requests库的异常

```python
requests.ConnectionError  网络连接错误异常，如DNS查询失败，拒绝连接
requests.HTTPError  HTTP错误异常
requests.URLRequired  URL缺失异常
requests.TooManyRedirects  超过最大重定向次数，产生重定向异常
requests.ConnectTimeout  连接远程服务器超时异常
requests.Timeout  请求url超时，产生超时异常
```

## Response对象

- response.text是str类型，requests模块按照chardet模块推测出的编码字符集进行解码的结果

- response.content是bytes类型，解码没有指定，可以自行指定，response.text = response.content.decode("推测出的编码字符集")
- response.encoding获取根据头部信息推测出来的编码格式，可以自行赋值指定response对象的编码格式
- response.url:响应的url，有时候响应的url和请求的url并不一致
- response.status_code:响应状态码
- response.request.headers:响应对应的请求头，返回一个字典
- response.headers:响应头
- response.request._cookies:响应对应请求的cookie；返回cookieJar类型
- response.cookies：响应的cookie(经过了set-cookie动作；返回cookieJar类型)
- response.json():自动将json字符串类型的响应内容转换为python对象(dict或者list)

## 代理请求

透明代理：透明代理可以直接隐藏浏览器的ip地址(你的ip)，但是还是可以查到你是谁。

```python
目标服务器接收到的请求头：
REMOTE_ADDR = 代理ip
HTTP_VIA = 代理ip
HTTP_X_FORWARDED_FOR = 浏览器的ip(你的ip)
```

匿名代理：别人只能知道你用了代理，无法知道你是谁。

```python
REMOTE_ADDR = 代理ip
HTTP_VIA = 代理ip
HTTP_X_FORWARDED_FOR = 代理ip
```

高匿代理：别人无法发现你是在用代理。毫无疑问使用高匿代理效果最好

```python
REMOTE_ADDR = 代理ip
HTTP_VIA = not determined
HTTP_X_FORWARDED_FOR = not determined
```

## 网络爬虫的限制

来源审查：判断User-Agent进行限制，检查来访http协议头的user-agent阈，只响应浏览器或友好爬虫的访问

发布公告：Robots协议，告知所有爬虫网站的爬取策略，要求爬虫遵守

Robots协议：Robots Exclusion Standard，网络爬虫排除标准。作用，网站告知网路爬虫哪些页面可以抓取，哪些不行；形式，在网站根目录瞎的robots.txt文件

# 数据提取

## xml和html

| 数据格式 | 描述                                     | 设计目标                                   |
| -------- | ---------------------------------------- | ------------------------------------------ |
| XML      | Extensible Markup Language可扩展标记语言 | 被设计为传输和存储数据，其焦点是数据的内容 |
| HTML     | HyperText Markup Language超文本标记语言  | 显示数据以及如何更好的显示数据             |

## jsonpath模块

```python
from jsonpath import jsonpath
ret = jsonpath(a, 'jsonpath语法规则字符串')
```

| JsonPath | 描述                                 |
| -------- | ------------------------------------ |
| $        | 根节点                               |
| @        | 现行节点                             |
| . 或 []  | 取子节点                             |
| ..       | 就是不管位置，选择所有符合条件的元素 |
| *        | 匹配所有元素节点                     |
| []       | 迭代器标示（可以做取下标操作）       |
| [,]      | 迭代器多选，放入多个下标             |
| ?()      | 过滤操作                             |
| ()       | 表达式计算                           |

## LXML模块

lxml模块可以利用XPath规则语法，来快速的定位HTML\XML 文档中特定元素以及获取节点信息（文本内容、属性值）

XPath (XML Path Language) 是一门在 HTML\XML 文档中查找信息的**语言**，可用来在 HTML\XML 文档中对**元素和属性进行遍历**

- XPath语法参考：http://www.w3school.com.cn/xpath/index.asp

| 表达式                             | 描述                                                       |
| ---------------------------------- | ---------------------------------------------------------- |
| nodename                           | 选中该元素。                                               |
| /                                  | 从根节点选取、或者是元素和元素间的过渡。                   |
| //                                 | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。 |
| .                                  | 选取当前节点。                                             |
| ..                                 | 选取当前节点的父节点。                                     |
| @                                  | 选取属性。                                                 |
| text()                             | 选取文本。                                                 |
| //*[starts-with(@attribute,'xxx')] | 属性以xxx开头的元素                                        |
| //*[contains(@attribute,'Sxxx')]   | 属性中含有xxx的元素                                        |

## BeautifulSoup模块

```python
from bs4 import BeautifulSoup

html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class ="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class ="story"> Once upon a time there were three little s isters; and their names were
<a href ="http://example.com/elsie" class="sister" id="linkl"><!-Elsie … >< la>,
<a href ="http://example.com/lacie" class="sister" id="link2">Lacie</a>and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well. </p>
<p class =" story "> ... </p>
"""

soup = BeautifulSoup(html, 'lxml')  # 第一个参数是需要解析的html文本字符串，第二个参数是bs支持的解析器。选用lxml比较好

soup.title  # 选取title节点
soup.title.string  # 选取节点的文本信息
soup.title.name  # 获取节点的名称
soup.title.sttrs  # 获取节点的属性值，返回一个字典

# 嵌套选择
soup.head.title  # 获取head标签下的title标签

# 获取直接子节点
soup.p.contents  # 返回列表，包含p节点内的所有直接子节点
soup.p.children  # 返回生成器

# 获取所有的子孙节点
soup.p.descendants  # 返回生成器，包含p标签内的所有子孙节点

# 父节点
soup.a.parent
# 祖先节点
soup.a.parents

# 兄弟节点
soup.a.next_sibling  # 后一个兄弟节点
soup.a.previous_sibling # 前一个兄弟节点
soup.a.next_siblings  # 后面的兄弟节点们
soup.a.previous_siblings  # 前面的兄弟节点

# find_all()  # 寻找符合的标签，返回符合条件的元素，是一个列表
find_all(name, attrs, recursive, text, **kwargs)
	name:节点名称
    attrs:节点属性值，字典类型
    text:匹配节点的文本，字符串或者正则表达式对象
     
# find()  返回第一个匹配的元素    
# fing_parents()  返回所有祖先节点
# fing_parent()  返回直接父节点
# find_next_siblings()  返回后面的所有的兄弟节点
# find_next_sibling()  返回后面第一个兄弟节点
# find_previous_siblings()
# find_previous_sibling()
# find_all_next()  # 返回节点后所有符合条件的节点
# find_next()  # 返回符合条件的节点后边的第一个节点
# find_all_previous()
# find_previous()
```

