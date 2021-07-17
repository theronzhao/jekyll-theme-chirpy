---
title: Django基础
date: 2019-01-06 19:14:46 +0800
categories: [Web开发]
tags: [Web开发,Django]
---
Web框架的意义

- 用于搭建web应用程序
- 免去不同 Web 应用相同代码部分的重复编写，只需关心 Web 应用核心的业务逻辑实现

Web应用程序的本质

- 接收并解析 HTTP 请求 ，获取具体的请求信息
- 处理本次 HTTP 请求 ，即完成本次请求的业务逻辑处理
- 构造并返回处理结果 —— HTTP 响应

# 1.Django介绍

- Django 是一款重量级框架, 内部架构使用的是 MVT 设计模式.
    对比 Flask 框架，Django 原生提供了众多的功能组件，让开发更简便快速。
- 提供项目工程管理的自动化脚本工具( 脚手架工具 )
- 数据库 ORM 支持（对象关系映射，英语：Object Relational Mapping）
- 模板
- 表单
- Admin 管理站点
- 文件管理
- 认证权限
- session 机制
- 缓存

**MVC 模式说明**

- M 全拼为 Model，主要封装对数据库层的访问，对数据库中的数据进行增、删、改、查操作。
- V 全拼为 View，用于封装结果，生成页面展示的 html 内容。
- C 全拼为 Controller，用于接收请求，处理业务逻辑，与 Model 和 View 交互，返回结果。

**MVT 模式说明**

- M 全拼为 Model，与 MVC 中的 M 功能相同，负责和数据库交互，进行数据处理。
- V 全拼为 View，与 MVC 中的 C 功能相同，接收请求，进行业务处理，返回应答。
- T 全拼为 Template，与 MVC 中的 V 功能相同，负责封装构造要返回的 html。

两种程序设计模式，其核心思想是分工, 解耦，让不同的代码块之间降低耦合，增强代码的可扩展性和可移植性，实现向后兼容。

# 2.工程搭建

## 2.1.环境安装

	创建虚拟环境	mkvirtualenv 环境名 -p python3
	
	删除虚拟环境	rmvirtualenv 环境名
	
	进入虚拟环境、查看所有虚拟环境	workon
	
	退出虚拟环境	deactivate
	
	安装Django	pip install django==版本
	
	pip相关命令：
		查看：pip list/freeze

## 2.2.创建工程

```python
	创建项目工程	django-admin startproject 工程名称

	django提供了一个轻量级web服务器供开发阶段使用，运行命令：
	python manage.py runserver
	# 默认运行在 127.0.0.1:8000 的 IP 和 端口上
	# 我们也可以在刚刚的命令后面增加 IP:PORT 参数, 指定特定的 IP 和 端口号运行:
	python manage.py runserver IP地址:端口
```

## 2.3.创建子应用

```python
# 创建子应用的常见命令: 
	python manage.py startapp 子应用名称

	创建子应用完成后，在项目工程的 settings.py 文件中进行注册配置， 将子应用的配置信息文件 apps.py 中的 Config 类添加到 INSTALLED_APPS 列表中。
	- 例如：users.apps.UsersConfig 
```

## 2.4.创建视图

- 在子应用的views.py编写视图代码  
    视图函数的第一个传入参数必须定义，用于接收 Django 构造的包含了请求数据的 HttpReqeust 对象，通常名为 request。

    视图函数的返回值必须为一个响应对象, 可以将要返回的字符串数据放到一个 HTTPResponse 对象中。  

- 定义路由url  
    在子应用中新建一个 urls.py 文件保存该应用的路由  
    在 users / urls.py 文件中定义路由信息  
    在工程总路由 urls.py 中添加子应用的路由数据   

```python
---子路由：urlpattern = [
	url(r'^index/$', views.index)
]
	---总路由：urlpattern = [
	url(r'^users/', include('users.urls'))
]
    
使用 include 来将子应用 users 里的路由文件( urls.py )包含到工程总路由中 r'^users/' 决定了 users 子应用的所有路由都以 /users/ 开头，如我们刚定义的视图 index， 其最终的完整访问路径为 /users/index/。    
```

# 3.配置/静态文件/路由

## 3.1.配置文件

BASE_DIR

- 指当前工程的根目录，Django 会依此来定位工程内的相关文件，我们也可以使用该参数来构造文件路径。
- BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
- ___file___ : 可以理解为当前的文件
- os.path.abspath ( 文件 ) : 获取这个文件的绝对路径
- os.path.dirname( 路径 ) : 获取这个路径的上一级路径

## 3.2.静态文件

一般把项目中前端写的 CSS、图片、js 以及 html 等看做静态文件 。

为了提供静态文件访问路径，我们需要在项目的 settings.py 文件中配置两个参数：

- STATICFILES_DIRS : 存放查找静态文件的目录
- STATIC_URL : 访问静态文件的 URL 前缀

```python
1）在项目根目录下创建 static_files 目录来保存静态文件。
2）在 demo / settings.py 中修改静态文件的两个参数为
# 这个参数默认存在
STATIC_URL = '/static/'
# 我们可以添加这个参数, 用于补全静态文件路径
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static_files'),
]
3）此时在 static_files 添加的任何静态文件都可以使用网址 /static/ 文件在 static_files 中的路径 来访问了。
```

Django 仅在调试模式下（DEBUG=True）能对外提供静态文件。

当DEBUG=False工作在生产模式时，Django不再对外提供静态文件，需要是用collectstatic命令来收集静态文件并交由其他静态文件服务器来提供。

## 3.3.路由说明

http://www.baidu.com:80/users/index/?a=1&b=2&c=3#box

	http:// 代表使用的协议
	www.baidu.com 指的是访问的域名
	80 指的是调用的端口
	/users/index/ 指的是路由部分
	?a=1&b=2&c=3 指的是查询字符串
	#box 指的是锚点

**路由解析顺序**
	Django 在接收到一个请求时，从主路由文件中的 urlpatterns 列表中以由上至下的顺序查找对应路由规则，如果发现规则为 include 包含，则再进入被包含的 urls 中的 urlpatterns 列表由上至下进行查询。有可能会使上面的路由屏蔽掉下面的路由，带来非预期结果。

**路由命名**

```python
1) 在使用 include 函数定义路由时，可以使用 namespace 参数定义路由的命名空间
	- include('子应用.urls', namespace="名称" )
2) 在定义普通路由时，可以使用 name 参数指明路由的名字
	- url('正则', 视图函数, name="名称")
```

**reverse 反解析**

可以根据路由名称，返回具体的路径： reverse( '总路由名称 : 子路由名称' )

路径中的 / 代表路径分隔符, 一般我们发请求时最好携带.

如果发请求时, 没有在最后携带 / , 则浏览器会帮我们进行一次重定向, 再次访问携带 / 的路径

# 4.请求与响应

## 4.1.请求Request

QueryDict 对象是 django 中一种特殊类型的字典，可以存储 一对一类型的数据, 也可以存储 一对多类型的数据，一般用来存储浏览器传递过来的参数，可以默认把他看成是一个字典

获取 QueryDict 中的数据:

```python
- 一键 ===> 一值: QueryDict 这个字典可以调用 get( ) 函数来获取.
- 一键 ===> 多值: QueryDict 这个字典的获取方法:
		如果获取一个值: QueryDict.get( key ) 获取的是所有值的最后一个.
		如果获取多个值: QueryDict.getlist( key ) 获取这个键对应的所有值, 存放在列表中返回.得到的值都是字符串类型
```

前端传参的四种方式：

1、查询字符串QueryString 传参，通过url的参数部分传参，取值：request.GET.get('key')

- 通过 request.GET 获取的是 QueryDict 类型
- 通过 QueryDict 的 get( ) 和 getlist( ) 方法来获取某个键对应的数据
- 通过QueryDict的dict()方法获取所有的kv查询参数

2、路径传参，url/(分组1正则式)/(分组2正则式)，取值：视图函数定义形参接收，若未对正则式进行命名，则安定依顺序传递给形参

3、请求体传参，可以发送请求体数据的请求方式有 POST、PUT、PATCH、DELETE。

表单类型参数：通过 request.POST 属性获取，返回 QueryDict 对象，通过 QueryDict 的 get( ) 和 getlist( ) 函数, 获取数据

非表单类型参数：通过 request.body 属性获取最原始的请求体数据，返回 bytes 类型

请求对象中的请求头信息：

- 获取请求头数据可以通过 request.META 属性获取请求头中的数据 request.META 为字典类型
- 具体使用 request.META['CONTENT_LENGTH']

请求对象中的其他信息：

- request.属性名，user/method/path/FILES/encoding

## 4.2.响应Response

```python
	HttpResponse的三个参数：
	HttpResponse(
    content=响应体, 
    content_type=响应体数据类型, 
    status=状态码
)
    
如果需要在响应头添加自定义的键值对内容,可以把 HttpResponse 对象当做字典进行响应头键值对的设置：
# 创建一个 response 对象
response = HttpResponse()
# 在对象中添加一个新的 键值对
response['Itcast'] = 'Python'

JsonResponse 类
	- 帮助我们将字典数据转换为 json 字符串
	- 设置响应头 Content-Type 为 application/json
    
redirect 重定向
	- 使用格式:redirect('想要跳转的路径')，一般搭配reverse使用  
```

## 4.3.Cookie

指的是由服务端生成, 保存在客户端的一种数据存储形式, 内部以 key/value 键值对形式存储. 一般不存储敏感信息. 内容有时会被加密.

Cookie 的特点

- Cookie 以键值对 Key-Value 形势进行信息的存储
- Cookie 基于域名安全，不同域名的 Cookie 是不能互相访问的

设置Cookie

- 通过 HttpResponse 对象中的 set_cookie 方法来设置 cookie。

```python
response.set_cookie(key,  value,  max_age)
- max_age 单位为秒, 默认为 None. 如果设置 None 值, 则关闭浏览器失效.
```

读取Cookie

- 通过 HttpRequest 对象( request )的 COOKIES 属性来读取本次请求携带的 cookie 值。request.COOKIES 为字典类型

```python
value = request.COOKIES.get('key')
```

删除Cookie

- 删除的服务器的,游览器的还是没有删掉`del request.COOKIES['my']`
- cookie对应的值删了,键还是存在的`response.delete_cookie('my')`
- 这个是删除所有cookie`response.flush()`

## 4.4.Session

Session 是一种会话控制方式. 由服务端创建, 并且保存在服务端的数据存储形式. 内部以 key/value 键值对的形式存储. 可以存储敏感信息, 内容有些会被加密. 一般生成 session 后, 我们会把 sessionid 传递给 cookie 保存.

Session 的作用

- 在服务器上保存用户状态信息, 以供前端页面访问
- 因为数据保存在服务器端, 所以可以保存敏感信息. 每次前端发送请求, 可以随时获取对应的信息. 保持会话状态

Session 的特点

- 依赖 cookies
- 存储敏感、重要的信息
- 支持更多字节
- Session 共享问题

Session 配置和存储

settings.py 文件中，可以设置 session 数据的存储方式:

- 数据库：默认存储方式，SESSION_ENGINE='django.contrib.sessions.backends.db'
- 本地缓存：存储在本机内存中，如果丢失则不能找回，比数据库的方式读写更快SESSION_ENGINE='django.contrib.sessions.backends.cache'
- 混合存储：优先从本机内存中存取，如果没有则从数据库中存取。SESSION_ENGINE='django.contrib.sessions.backends.cached_db'

在 redis 中保存 session，需要引入第三方扩展，我们可以使用 django-redis 来解决。

```python
1）安装扩展 pip install django-redis
2）配置：settings.py 文件中
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    }
}
SESSION_ENGINE = "django.contrib.sessions.backends.cache"
SESSION_CACHE_ALIAS = "default"
```

Session 操作：

```python
通过 HttpRequest 对象( request ) 的 session 属性进行会话的读写操作。
	1）往 session 中写入键值对:
		request.session['键'] = 值
	2）根据 key 读取 session 中的数据:
		value = request.session.get('键',默认值)
	3）清除所有 session，在存储中删除值部分:
		request.session.clear()
	4）清除 session 数据，在存储中删除 session 的整条数据:
		request.session.flush()
	5）删除 session 中的指定键及值，在存储中只删除某个键及对应的值。
		del request.session['键']
	6）设置 session 的有效期
		request.session.set_expiry(value)
			- 如果 value 是一个整数，session 将在 value 秒没有活动后过期。
			- 如果 value 为 0，那么用户 session 的 Cookie 将在用户的浏览器关闭时过期。
			- 如果 value 为 None，那么 session 有效期将采用系统默认值，默认为两周，可以通过在 settings.py 中设置 SESSION_COOKIE_AGE 来设置全局默认值。
```

# 5.类视图与中间件

## 5.1.类视图

定义类视图需要继承自 Django 提供的父类 View ，配置路由时，需要在类名后面增加 as_view( ) 函数，类视图中的函数名称是规定死的请求方法名

类视图添加装饰器--

- 以下my_decorate维自定义的闭包装饰器

1、在urls.py里在类视图的前面添加一个装饰器，my_decorate(views.RegisterView.as_view())--不推荐。此种方式会为类视图中的所有请求方法都加上装饰器行为

2、使用method_decorator

​		1> 在views.py的类视图上面添加@methos_decorator(my_decorate,name="dispatch")，为全部请求方法添加装饰器

​		2> 在views.py的类视图上面添加@methos_decorator(my_decorate,name="get")，为特定请求方法添加装饰器

​		3> 在views.py的类视图里对应的方法上面添加@methos_decorator(my_decorate)，为特定请求方法添加装饰器  

类视图Mixin扩展类

使用 Mixin 扩展类时有两个注意点:  
		扩展类需要继承自 object  
		类视图调用时, 需要把 View 类添加到最后  
	在 Mixin 扩展类中, 我们一般会重写 as_view( ) 函数. 在函数内添加过滤  
	一个类视图可以继承多个扩展类, 每个扩展类中都可以添加装饰器.  

## 5.2.中间件

中间件是一个轻量级、底层的插件系统，可以介入请求和响应处理过程，在 Django 视图的不同阶段对输入或输出进行处理.

中间件的定义方法--

```python
def simple_middleware(get_response):
    # 此处编写的代码仅在 Django 第一次配置和初始化的时候执行一次。

    def middleware(request):

        # 此处编写的代码会在每个请求处理视图前被调用。

        response = get_response(request)

        # 此处编写的代码会在每个请求处理视图之后被调用。

        return response

    return middleware

定义好中间件后，需要在 settings.py 文件中添加注册中间件：
	settings里MIDDLEWARE配置中间件，如'users.middleware.my_middleware'，
    
注意：多个中间件的执行顺序，视图处理前和视图处理后的调用顺序不一致
	- 在请求视图被处理前，中间件由上至下依次执行
	- 在请求视图被处理后，中间件由下至上依次执行
```

# 6.模板

## Django模板

略

## CSRF攻击

定义：CSRF全拼为 Cross Site Request Forgery， 译为跨站请求伪造。CSRF指攻击者盗用了你的身份，以你的名义发送恶意请求。

防止 CSRF 攻击：

1、在客户端向后端请求界面数据的时候，后端会往响应中的 cookie 中设置 csrf_token 的值

2、在 Form 表单中添加一个隐藏的的字段，值也是 csrf_token

3、在用户点击提交的时候，会带上这两个值向后台发起请求

4、后端接受到请求，以会以下几件事件：

- 从 cookie 中取出 csrf_token
- 从 表单数据中取出来隐藏的 csrf_token 的值
- Django 进行对比 (这里的对比不要求一定一致, 有时候两个值不一致)

5、如果对比能够通过, 则是正常的请求

6、如果对比不能够通过，代表不是正常的请求，不执行下一步操作

# 7.数据库

ORM框架

- O是 object，也就类对象的意思
- R 是 relation，翻译成中文是关系, 也就是关系数据库中数据表的意思.
- M 是 mapping，是映射的意思

通过类和类对象就能操作它所对应的表格中的数据;根据我们设计的类自动帮我们生成数据库中的表格

## 7.1. 配置

```python
默认使用sqlite
	1\安装pip install PyMySQL
	2\在 Django 的工程同名子目录的 __init__.py 文件中添加如下语句:
	from pymysql import install_as_MySQLdb
	install_as_MySQLdb()
	3\修改 DATABASES 配置信息
	DATABASES = {
		'default': {
			'ENGINE': 'django.db.backends.mysql',
			'HOST': '127.0.0.1',  # 数据库主机
			'PORT': 3306,  # 数据库端口
			'USER': 'root',  # 数据库用户名
			'PASSWORD': 'mysql',  # 数据库用户密码
			'NAME': 'django_demo'  # 数据库名字
		}
	}
	4\在 MySQL 中创建数据库
	create database django_demo default charset=utf8;
```

## 7.2.定义模型类

类中类可以定义abstract=True表示抽象类，不会在迁移的时候生成迁移文件，managed=False表示django只用这个模型类，但是不管理这个模型类

生成迁移文件：python manage.py makemigrations

迁移：python manage.py migrate

## 7.4.数据库操作

```python
- 增加
	save  通过创建模型类对象，执行对象的save()方法保存到数据库中
	create  通过 模型类.objects.create() 保存.

	- 删除
	使用对象删除，对象.delete()
	使用查询集删除，模型类.objects.filter().delete()

	- 修改
	save  修改模型类对象的属性，然后执行 save() 方法
	update  查询集.update()，使用 模型类.objects.filter().update()，会返回受影响的行数
		HeroInfo.objects.filter(hname='沙悟净').update(hname='沙僧')

	- 查询
	get 查询单一结果，如果不存在会抛出 模型类.DoesNotExist 异常.
	all 查询多个结果
	count 查询结果数量

	- 过滤查询
	filter 过滤出多个结果
	exclude 排除掉符合条件剩下的结果
	get 过滤单一结果
	过滤条件的表达语法如下：属性名称__比较运算符 = 值
		- 相等exact：表示判等
			BookInfo.objects.filter(id__exact=1)
		- 模糊查询contains：是否包含，startswith、endswith：以指定值开头或结尾
			BookInfo.objects.filter(btitle__contains='传')
		以上运算符都区分大小写，在这些运算符前加上i表示不区分大小写，如iexact、icontains、istartswith、iendswith.
		- 空查询isnull：是否为null
			BookInfo.objects.filter(btitle__isnull=False)
		- 范围查询in和range：是否包含在范围内
			BookInfo.objects.filter(id__in=[1, 3, 5])
			Entry.objects.filter(pub_date__range=(start_date, end_date))
		- 比较查询gt 大于 (greater then)，gte 大于等于 (greater then equal)，lt 小于 (less then)，lte 小于等于 (less then equal)
			BookInfo.objects.filter(id__gt=3)
		- 日期查询year、month、day、week_day、hour、minute、second：对日期时间类型的属性进行运算
			BookInfo.objects.filter(bpub_date__year=1980)


	- F 对象 和 Q 对象
	两个属性比较，使用 F对象：F(属性名)：
		BookInfo.objects.filter(bread__gte=F('bcomment')*2)
	多个过滤器逐个调用表示 逻辑与 关系：
		BookInfo.objects.filter(bread__gt=20).filter(id__lt=3)
	使用 Q( ) 对象结合|运算符实现 逻辑或(or) 的查询：
		BookInfo.objects.filter(Q(bread__gt=20) | Q(pk__lt=3))
		&表示逻辑与，|表示逻辑或，Q对象 前可以使用~操作符，表示非not.
		BookInfo.objects.filter(~Q(pk=3))

	- 聚合查询
	使用 aggregate( ) 过滤器调用聚合函数.
	聚合函数包括：Avg 平均，Count 数量，Max 最大，Min 最小，Sum 求和，StdDev 标准差，Variance 方差

	BookInfo.objects.aggregate(Sum('bread'))

	注意 aggregate 的返回值是一个字典类型，格式如下：{'属性名__聚合类小写':值}，如:{'bread__sum':3}

	使用 count 时一般不使用 aggregate( ) 过滤器，BookInfo.objects.count()

	- 排序
	使用 order_by 对结果进行排序
	BookInfo.objects.all().order_by('bread')  # 升序
	BookInfo.objects.all().order_by('-bread')  # 降序

	- 关联查询
	1、由一到多的访问：
		一对应的模型类对象.多对应的模型类名小写_set，如果有related_name约束，可以直接用related_name替换多对应的模型类名小写_set
	2、由多到一的访问
		多对应的模型类对象.外键名
	3、访问一对应的模型类关联对象的 id
		多对应的模型类对象.关联类属性_id
	4、关联过滤查询
		由多模型类条件查询一模型类数据:
			关联模型类名小写__属性名__条件运算符=值，BookInfo.objects.filter(heroinfo__hcomment__contains='八')
		由一模型类条件查询多模型类数据:
			一模型类关联属性名__一模型类属性名__条件运算符=值，HeroInfo.objects.filter(hbook__bread__gt=30)

	- 其他方法
	get_or_create(),首先尝试获取对象，不存在就创建，可以防止重复.返回的是一个元组，(object, True/False)，创建时返回 True, 已经存在时返回 False

	reverse(),反向排序一个查询集，他只能对已经排了序的查询集进行反排

	distinct(),返回一个在SQL 查询中使用SELECT DISTINCT的新QuerySet。它将去除查询结果中重复的行。

	values(),返回一个ValuesQuerySet ——QuerySet 的一个子类，迭代时返回字典而不是模型实例对象。values() 接收可选的位置参数*fields，它指定SELECT 应该限制哪些字段。如果指定字段，每个字典将只包含指定的字段的键/值。如果没有指定字段，每个字典将包含数据库表中所有字段的键和值。
		# 返回对象
		>>> Blog.objects.filter(name__startswith='Beatles')
		[<Blog: Beatles Blog>]
		# 将对象转成字典再返回.
		>>> Blog.objects.filter(name__startswith='Beatles').values()
		[{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]
		# 查询指定字段并返回
		>>> Blog.objects.values()
		[{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}],
		>>> Blog.objects.values('id', 'name')
		[{'id': 1, 'name': 'Beatles Blog'}]
	
	values_list(), 获取元组形式结果
		>>>Author.objects.values_list('name', 'qq')
		>>> <QuerySet [(u'WeizhongTu', u'336643078'), (u'twz915', u'915792575'), (u'wangdachui', u'353506297'), (u'xiaoming', u'004466315')]>
		>>>list(Author.objects.values_list('name', 'qq'))
		>>>[(u'WeizhongTu', u'336643078'),(u'twz915', u'915792575'),(u'wangdachui', u'353506297'),(u'xiaoming', u'004466315')]

	latest(条件),返回最近的一条记录，使用条件作为依据，如Entry.objects.latest('pub_date')

	earliest(),同上，只不过是最早的

	first(),返回结果集的第一个对象, 当没有找到时返回None.如果QuerySet 没有设置排序,则将会自动按主键进行排序
		p = Article.objects.order_by('title', 'pub_date').first()

	last(),同上，只不过是最后一个

	exists()：判断查询集中是否有数据，如果有则返回 True，没有则返回 False。
		some_queryset.filter(pk=entry.pk).exists()
	
	select_related(self, *fields):性能相关：表之间进行join连表操作，一次性获取关联的数据。
		model.tb.objects.all().select_related()
		model.tb.objects.all().select_related('外键字段')
		model.tb.objects.all().select_related('外键字段__外键字段')

	prefetch_related(self, *lookups)：性能相关：多表连表操作时速度会慢，使用其执行多次SQL查询  在内存中做关联，而不会再做连表查询;相当于进行子语句查询
		# 第一次 获取所有用户表
		# 第二次 获取用户类型表where id in (用户表中的查到的所有用户ID)
		models.UserInfo.objects.prefetch_related('外键字段')

	annotate(self, *args, **kwargs)：用于实现聚合group by查询
		qs = Question.objects.annotate(thumbs = F('thumbups') - F('thumbdowns')).order_by("-thumbs")

	extra(),实现 别名，条件，排序等
		SELECT name AS tag_name FROM blog_tag;
		tags = Tag.objects.all().extra(select={'tag_name': 'name'})
		tags[0].name或者tags[0].tag_name都能取出值

	下面两个 取到的是对象，并且注意 取到的对象可以 获取其他字段（这样会再去查找该字段降低性能
	defer(self, *fields)：排除不需要的字段
		UserInfo.objects.defer('username','id')  # 查出的字段不包含username和id
		UserInfo.objects.filter(...).defer('username','id')
	
	only(self, *fields)：仅选择需要的字段
		UserInfo.objects.only('username','id')  # 查出的字段只包含username和id
		UserInfo.objects.filter(...).only('username','id')
```

## 7.5.查询集QuerySet

查询集，也称查询结果集、QuerySet，表示从数据库中获取的对象集合。

all(),filter(),exclude(),order_by()会返回查询集

判断某一个查询集中是否有数据：

exists()：判断查询集中是否有数据，如果有则返回 True，没有则返回 False。   查询集.exists()

两大特性

- 惰性执行:创建查询集不会访问数据库，直到调用数据时，才会访问数据库，调用数据的情况包括迭代、序列化、与if合用
- 缓存:使用同一个查询集，第一次使用时会发生数据库的查询，然后Django会把结果缓存下来，再次使用这个查询集时会使用缓存的数据，减少了数据库的查询次数。

限制查询集：可以对查询集进行取下标或切片操作，注意：不支持负数索引。对查询集进行切片后返回一个新的查询集，不会立即执行查询。

## 7.6.管理器Manager

管理器是 Django 的模型进行数据库操作的接口

在通过模型类的 objects 属性提供的方法操作数据库时，即是在使用一个管理器对象 objects

自定义管理器：

```python
1. 修改原始查询集，重写 all() 方法。
	a）打开 booktest / models.py 文件，定义类 BookInfoManager
	#图书管理器
	class BookInfoManager(models.Manager):
		def all(self):
			#默认查询未删除的图书信息
			#调用父类的成员语法为：super().方法名
			return super().filter(is_delete=False)
	b）在模型类BookInfo中定义管理器
	class BookInfo(models.Model):
		...
		books = BookInfoManager()
	c）使用方法
	BookInfo.books.all()

2. 在管理器类中补充定义新的方法
	a）打开 booktest / models.py 文件，定义方法 create
	class BookInfoManager(models.Manager):
		#创建模型类，接收参数为属性赋值
		def create_book(self, title, pub_date):
			#创建模型类对象self.model可以获得模型类
			book = self.model()
			book.btitle = title
			book.bpub_date = pub_date
			book.bread=0
			book.bcommet=0
			book.is_delete = False
			# 将数据插入进数据表
			book.save()
			return book
	b）为模型类 BookInfo 定义管理器 books 语法如下
	class BookInfo(models.Model):
		...
		books = BookInfoManager()
	c）调用语法如下：
	book=BookInfo.books.create_book("abc",date(1980,1,1))
```

