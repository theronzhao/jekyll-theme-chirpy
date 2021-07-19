---
title: sys模块
date: 2019-01-01 20:33:05 +0800
categories: [Python,标准库]
tags: [Python标准库]
---
# OS模块

该模块提供了一些方便使用操作系统相关功能的函数。

## 模块变量

os.name

  导入依赖操作系统模块的名字，指示你正在使用的平台。如果是posix，说明系统是Linux、Unix或Mac OS X，如果是nt，就是Windows系统。

os.linesep

  当前平台上的换行符字符串. 在POSIX上是'\n',或者 在Windows上是'\r\n' . 不要使用 os.linesep作为换行符，当写入文本文件时 (默认); 使用'\n'代替, 在所有平台上。

os.environ

  一个mapping对象表示操作系统中定义的环境变量。例如，environ['HOME'] ，表示的你自己home文件夹的路径(某些平台支持，windows不支持) ，它与C中的getenv("HOME")一致。

## 文件和文件夹

os.access(path, mode)

  使用现在的uid/gid尝试访问path。注大部分操作使用有效的uid/gid, 因此运行环境可以在 suid/sgid环境尝试，如果用户有权访问path。 mode为F_OK，测试存在的path,或者它可以是包含R_OK, W_OK和X_OK或者R_OK, W_OK和X_OK其中之一或者更多。如果允许访问返回 True , 否则返回False。

os.chdir(path)

  改变当前工作目录。 在unix，Windows中有效。

os.getcwd()

  返回当前工作目录的字符串， 在unix，Windows中有效。

os.getcwdu()

  返回一个当前工作目录的Unicode对象。在unix，Windows中有效。

os.chroot(path)

  改变根目录为path。在unix中有效

os.chmod(path, mode)

  改变path的mode到数字mode。在unix，Windows中有效。

os.chown(path, uid, gid)

  改变path的所属用户和组。在unix中有效

os.listdir(path)

  返回path指定的文件夹包含的文件或文件夹的名字的列表。

os.remove(path)

  删除路径为path的文件。

os.rename(需要修改的文件名, 新的文件名)

  可以完成对文件的重命名操作

os.mkdir(path)

  创建文件夹到指定路径

os.rmdir(path)

  删除指定路径的文件夹

## 获取进程ID

os.getppid()

  返回当前父进程的id。在unix中有效

os.getpid()

  返回当前进程的id。在unix，Windows中有效。

## os子模块：Path模块

os.path.abspath(path) #返回绝对路径

os.path.basename(path) #返回文件名

os.path.commonprefix(list) #返回list(多个路径)中，所有path共有的最长的路径。

os.path.dirname(path) #返回文件路径

os.path.exists(path) #路径存在则返回True,路径损坏返回False

os.path.lexists #路径存在则返回True,路径损坏也返回True

os.path.expanduser(path) #把path中包含的"~"和"~user"转换成用户目录

os.path.expandvars(path) #根据环境变量的值替换path中包含的”$name”和”${name}”

os.path.getatime(path) #返回最后一次进入此path的时间。

os.path.getmtime(path) #返回在此path下最后一次修改的时间。

os.path.getctime(path) #返回path的大小

os.path.getsize(path) #返回文件大小，如果文件不存在就返回错误

os.path.isabs(path) #判断是否为绝对路径

os.path.isfile(path) #判断路径是否为文件

os.path.isdir(path) #判断路径是否为目录

os.path.islink(path) #判断路径是否为链接

os.path.ismount(path) #判断路径是否为挂载点（）

os.path.join(path1[, path2[, ...]]) #把目录和文件名合成一个路径

os.path.normcase(path) #转换path的大小写和斜杠

os.path.normpath(path) #规范path字符串形式

os.path.realpath(path) #返回path的真实路径

os.path.relpath(path[, start]) #从start开始计算相对路径

os.path.samefile(path1, path2) #判断目录或文件是否相同

os.path.sameopenfile(fp1, fp2) #判断fp1和fp2是否指向同一文件

os.path.samestat(stat1, stat2) #判断stat tuple stat1和stat2是否指向同一个文件

os.path.split(path) #把路径分割成dirname和basename，返回一个元组

os.path.splitdrive(path)  #一般用在windows下，返回驱动器名和路径组成的元组

os.path.splitext(path) #分割路径，返回路径名和文件扩展名的元组

os.path.splitunc(path) #把路径分割为加载点与文件

os.path.walk(path, visit, arg) #遍历path，进入每个目录都调用visit函数，visit函数必须有3个参数(arg, dirname, names)，dirname表示当前目录的目录名，names代表当前目录下的所有文件名，args则为walk的第三个参数

os.path.supports_unicode_filenames #设置是否支持unicode路径名

# sys模块

sys.argv

  一个列表，其中包含了被传递给 Python 脚本的命令行参数。

sys.builtin_module_names

  一个元素为字符串的元组。包含了所有的被编译进 Python 解释器的模块。（这个信息无法通过其他的办法获取）

sys.modules

  是一个字典, 将模块名映射到模块（仅限于当前已导入的模块）。key 是模块名，value 是模块

sys.exc_info() 

  返回一个元组,获取当前正在处理的异常类,包含三个元素exc_type、exc_value、exc_traceback 当前处理的异常详细信息

sys.exc_clear() 

  用来清除当前线程所出现的当前的或最近的错误信息

sys.exec_prefix 

  返回平台独立的 python 文件安装的位置

sys.exit(n) 

  退出程序，正常退出时 exit(0), 非正常退出exit(1)

sys.hexversion 

  获取 Python 解释程序的版本值，16 进制格式如：0x020403F0

sys.version 

  获取 Python 解释程序的版本信息

sys.maxint 

  最大的 Int 值

sys.maxunicode 

  最大的 Unicode 值

sys.path 

  返回模块的搜索路径，初始化时使用 PYTHONPATH 环境变量的值

sys.platform 

  返回操作系统平台名称

sys.stdout 

  标准输出

sys.stdin 

  标准输入

sys.stderr 

  错误输出

sys.byteorder 

  本地字节规则的指示器，big-endian 平台的值是'big',little-endian 平台的值是'little'

sys.copyright 

  记录 python 版权相关的东西

sys.api_version 

  解释器的 C 的 API 版本

sys.version_info 

  元组则提供一个更简单的方法来使你的程序具备 Python 版本要求功能