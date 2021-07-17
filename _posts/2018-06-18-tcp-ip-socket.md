---
layout: post
title: TCP与UDP
Author: TheronZhao
tags: 网络通信
---
- zzz
{:toc}
# 简介

## IP地址

用来在网络中标记一台电脑，比如192.168.1.1；在本地局域网上是唯一的。

IP地址实际上是一个32位整数（称为IPv4），以字符串表示的IP地址如`192.168.0.1`实际上是把32位整数按8位分组后的数字表示，目的是便于阅读。

IPv6地址实际上是一个128位整数，它是目前使用的IPv4的升级版，以字符串表示类似于`2001:0db8:85a3:0042:1000:8a2e:0370:7334`。

## 端口

端口是通过**端口号**来标记的，端口号只有整数，范围是**从0到65535**（2的16次方）。

**知名端口**是众所周知的端口号，范围从0到1023；80端口分配给HTTP服务，21端口分配给FTP服务；

**动态端口**的范围是从1024到65535，当一个系统程序或应用程序程序需要网络通信时，它向主机申请一个端口，主机从可用的端口号中分配一个供它使用。

## NAT

网络地址转换器，当在专用网内部的一些主机本来已经分配到了本地IP地址（即仅在本专用网内使用的专用地址），但现在又想和因特网上的主机通信（并不需要加密）时，可使用NAT方法

1. 当在家里用宽带链接上网时，会把电话线(今天很多地方都是光纤)---->调制解调制(简称猫)------->电脑等设备

2. 电脑会得到来自电信服务商的一个公网ip地址（切记只有公网ip地址才能上网），此时可以直接上网happy...

3. 为了能够让多台设备都可以上网，需要将数据进行“分流” 电话线(今天很多地方都是光纤)---->调制解调制(简称猫)------->路由器------>电脑等设备

4. 此时路由器的一端有一个公网ip地址，剩下的4个（路由器型号不同个数不同）可以接入电脑等设备 并且 它们的ip是私有ip(例如 192.168.1.2)

5. 当一个电脑（192.168.1.2）上网时，先通过DNS协议解析出某个域名对应的ip，然后

    > - 发送数据时,在经过路由器时转换为公网ip以及路由器自己分配的临时端口
    >
    > 192.168.1.2:6789----->192.168.1.1 路由器 116.226.52.212:6539------->猫---->万维网
    >
    > - 接收数据时,在经过路由器时转换为路由器之前记录的ip以及port
    >
    > 万维网------->猫----->116.226.52.212:6539 路由器 192.168.1.1 ---->192.168.1.2:6789



## 通信概述

为了把全世界的所有不同类型的计算机都连接起来，就必须规定一套全球通用的协议，为了实现互联网这个目标，**互联网协议簇（Internet Protocol Suite）**就是通用协议标准.

互联网协议包含了上百种协议标准，但是最重要的两个协议是TCP和IP协议，所以，大家把互联网的协议简称TCP/IP协议。

**IP协议**	负责把数据从一台计算机通过网络发送到另一台计算机。数据被分割成一小块一小块，然后通过IP包发送出去。由于互联网链路复杂，两台计算机之间经常有多条线路，因此，路由器就负责决定如何把一个IP包转发出去。IP包的特点是按块发送，途径多个路由，但不保证能到达，也不保证顺序到达

**TCP协议**	则是建立在IP协议之上的。TCP协议负责在两台计算机之间建立可靠连接，保证数据包按顺序到达。TCP协议会通过握手建立连接，然后，对每个IP包编号，确保对方按顺序收到，如果包丢掉了，就自动重发。

许多常用的更高级的协议都是建立在TCP协议基础上的，比如用于浏览器的HTTP协议、发送邮件的SMTP协议等。

一个TCP报文除了包含要传输的数据外，还包含源IP地址和目标IP地址，源端口和目标端口。

端口有什么作用？在两台计算机通信时，只发IP地址是不够的，因为同一台计算机上跑着多个网络程序。一个TCP报文来了之后，到底是交给浏览器还是QQ，就需要端口号来区分。每个网络程序都向操作系统申请唯一的端口号，这样，两个进程在两台计算机之间建立网络连接就需要各自的IP地址和各自的端口号。

## socket简介

网络层的“ip地址”可以唯一标识网络中的主机，而传输层的“协议+端口”可以唯一标识主机中的应用进程（进程）。

socket(简称 `套接字`) 是进程间通信的一种方式，它与其他进程间通信的一个主要不同是：
它能实现不同主机间的进程间通信，我们网络上各种各样的服务大多都是基于 Socket 来完成通信的

```python
# 在 Python 中 使用socket 模块的函数 socket 就可以创建socket
import socket
socket.socket(AddressFamily, Type)
```

- AddressFamily：可以选择 AF_INET（用于 Internet 进程间通信） 或者 AF_UNIX（用于同一台机器进程间通信）,实际工作中常用AF_INET
- Type：套接字类型，可以是 SOCK_STREAM（流式套接字，主要用于 TCP 协议）或者 SOCK_DGRAM（数据报套接字，主要用于 UDP 协议）

```python
import socket

# 创建tcp的套接字
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# ...这里是使用套接字的功能（省略）...

# 不用的时候，关闭套接字
s.close()
```

```python
import socket

# 创建udp的套接字
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# ...这里是使用套接字的功能（省略）...

# 不用的时候，关闭套接字
s.close()
```

## Socket 对象方法

| 函数                                 | 描述                                                         |
| :----------------------------------- | :----------------------------------------------------------- |
| 服务器端套接字                       |                                                              |
| s.bind()                             | 绑定地址（host,port）到套接字， 在AF_INET下,以元组（host,port）的形式表示地址。 |
| s.listen()                           | 开始TCP监听。backlog指定在拒绝连接之前，操作系统可以挂起的最大连接数量。该值至少为1，大部分应用程序设为5就可以了。 |
| s.accept()                           | 被动接受TCP客户端连接,(阻塞式)等待连接的到来                 |
| 客户端套接字                         |                                                              |
| s.connect()                          | 主动初始化TCP服务器连接，。一般address的格式为元组（hostname,port），如果连接出错，返回socket.error错误。 |
| s.connect_ex()                       | connect()函数的扩展版本,出错时返回出错码,而不是抛出异常      |
| 公共用途的套接字函数                 |                                                              |
| s.recv()                             | 接收TCP数据，数据以字符串形式返回，bufsize指定要接收的最大数据量。flag提供有关消息的其他信息，通常可以忽略。 |
| s.send()                             | 发送TCP数据，将string中的数据发送到连接的套接字。返回值是要发送的字节数量，该数量可能小于string的字节大小。 |
| s.sendall()                          | 完整发送TCP数据。将string中的数据发送到连接的套接字，但在返回之前会尝试发送所有数据。成功返回None，失败则抛出异常。 |
| s.recvfrom()                         | 接收UDP数据，与recv()类似，但返回值是（data,address）。其中data是包含接收数据的字符串，address是发送数据的套接字地址。 |
| s.sendto()                           | 发送UDP数据，将数据发送到套接字，address是形式为（ipaddr，port）的元组，指定远程地址。返回值是发送的字节数。 |
| s.close()                            | 关闭套接字                                                   |
| s.getpeername()                      | 返回连接套接字的远程地址。返回值通常是元组（ipaddr,port）。  |
| s.getsockname()                      | 返回套接字自己的地址。通常是一个元组(ipaddr,port)            |
| s.setsockopt(level,optname,value)    | 设置给定套接字选项的值。                                     |
| s.getsockopt(level,optname[.buflen]) | 返回套接字选项的值。                                         |
| s.settimeout(timeout)                | 设置套接字操作的超时期，timeout是一个浮点数，单位是秒。值为None表示没有超时期。一般，超时期应该在刚创建套接字时设置，因为它们可能用于连接的操作（如connect()） |
| s.gettimeout()                       | 返回当前超时期的值，单位是秒，如果没有设置超时期，则返回None。 |
| s.fileno()                           | 返回套接字的文件描述符。                                     |
| s.setblocking(flag)                  | 如果flag为0，则将套接字设为非阻塞模式，否则将套接字设为阻塞模式（默认值）。非阻塞模式下，如果调用recv()没有发现任何数据，或send()调用无法立即发送数据，那么将引起socket.error异常。 |
| s.makefile()                         | 创建一个与该套接字相关连的文件                               |

# UDP

## udp简介

Internet 协议集支持一个无连接的传输协议，该协议称为**用户数据报协议**（UDP，User Datagram Protocol）。UDP 为应用程序提供了一种**无需建立连接**就可以发送封装的 IP 数据包的方法。

UDP是一个无连接协议，传输数据之前源端和终端不建立连接，当它想传送时就简单地去抓取来自应用程序的数据，并尽可能快地把它扔到网络上。在发送端，UDP传送数据的速度仅仅是受应用程序生成数据的速度、计算机的能力和传输带宽的限制；在接收端，UDP把每个消息段放在队列中，应用程序每次从队列中读一个消息段。

## udp网络程序-发送数据

创建一个基于udp的网络程序流程很简单，具体步骤如下：

1. 创建客户端套接字
2. 发送/接收数据
3. 关闭套接字

<div  align="left">
    <img src="/refer/UDP开发流程.jpg"  />
</div>


```python
#coding=utf-8

from socket import *

# 1. 创建udp套接字
udp_socket = socket(AF_INET, SOCK_DGRAM)

# 2. 准备接收方的地址
# '192.168.1.103'表示目的ip地址
# 8080表示目的端口
dest_addr = ('192.168.1.103', 8080)  # 注意 是元组，ip是字符串，端口是数字

# 3. 从键盘获取数据
send_data = input("请输入要发送的数据:")

# 4. 发送数据到指定的电脑上的指定程序中
udp_socket.sendto(send_data.encode('utf-8'), dest_addr)

# 5. 关闭套接字
udp_socket.close()
```

## udp网络程序-发送、接收数据

```python
#coding=utf-8

from socket import *

# 1. 创建udp套接字
udp_socket = socket(AF_INET, SOCK_DGRAM)

# 2. 准备接收方的地址
dest_addr = ('192.168.236.129', 8080)

# 3. 从键盘获取数据
send_data = input("请输入要发送的数据:")

# 4. 发送数据到指定的电脑上
udp_socket.sendto(send_data.encode('utf-8'), dest_addr)

# 5. 等待接收对方发送的数据
recv_data = udp_socket.recvfrom(1024)  # 1024表示本次接收的最大字节数

# 6. 显示对方发送的数据
# 接收到的数据recv_data是一个元组
# 第1个元素是对方发送的数据
# 第2个元素是对方的ip和端口
print(recv_data[0].decode('gbk'))
print(recv_data[1])

# 7. 关闭套接字
udp_socket.close()
```

## udp绑定信息

一般情况下，在一台电脑上运行的网络程序有很多，为了不与其他的网络程序占用同一个端口号，往往在编程中，udp的端口号一般不绑定，但是如果需要做成一个服务器端的程序的话，是需要绑定的

- 一个udp网络程序，可以不绑定，此时操作系统会随机进行分配一个端口，如果重新运行此程序端口可能会发生变化
- 一个udp网络程序，也可以绑定信息（ip地址，端口号），如果绑定成功，那么操作系统用这个端口号来进行区别收到的网络数据是否是此进程的

```python
#coding=utf-8

from socket import *

# 1. 创建套接字
udp_socket = socket(AF_INET, SOCK_DGRAM)

# 2. 绑定本地的相关信息，如果一个网络程序不绑定，则系统会随机分配
local_addr = ('', 7788) #  ip地址和端口号，ip一般不用写，表示本机的任何一个ip
udp_socket.bind(local_addr)

# 3. 等待接收对方发送的数据
recv_data = udp_socket.recvfrom(1024) #  1024表示本次接收的最大字节数

# 4. 显示接收到的数据
print(recv_data[0].decode('gbk'))

# 5. 关闭套接字
udp_socket.close()
```

## 应用：udp聊天器

```python
import socket


def send_msg(udp_socket):
    """获取键盘数据，并将其发送给对方"""
    # 1. 从键盘输入数据
    msg = input("\n请输入要发送的数据:")
    # 2. 输入对方的ip地址
    dest_ip = input("\n请输入对方的ip地址:")
    # 3. 输入对方的port
    dest_port = int(input("\n请输入对方的port:"))
    # 4. 发送数据
    udp_socket.sendto(msg.encode("utf-8"), (dest_ip, dest_port))


def recv_msg(udp_socket):
    """接收数据并显示"""
    # 1. 接收数据
    recv_msg = udp_socket.recvfrom(1024)
    # 2. 解码
    recv_ip = recv_msg[1]
    recv_msg = recv_msg[0].decode("utf-8")
    # 3. 显示接收到的数据
    print(">>>%s:%s" % (str(recv_ip), recv_msg))


def main():
    # 1. 创建套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # 2. 绑定本地信息
    udp_socket.bind(("", 7890))
    while True:
        # 3. 选择功能
        print("="*30)
        print("1:发送消息")
        print("2:接收消息")
        print("="*30)
        op_num = input("请输入要操作的功能序号:")

        # 4. 根据选择调用相应的函数
        if op_num == "1":
            send_msg(udp_socket)
        elif op_num == "2":
            recv_msg(udp_socket)
        else:
            print("输入有误，请重新输入...")

if __name__ == "__main__":
    main()
```

# TCP

## TCP 简介

TCP协议，**传输控制协议**（英语：Transmission Control Protocol，缩写为 TCP）是一种**面向连接**的、**可靠**的、**基于字节流**的传输层通信协议，由IETF的RFC 793定义。

TCP通信需要经过**创建连接、数据传送、终止连接**三个步骤。

 **TCP 的特点**

1. 面向连接
    - 通信双方必须先建立好连接才能进行数据的传输，数据传输完成后，双方必须断开此连接，以释放系统资源。
2. 可靠传输
    - TCP 采用发送应答机制
    - 超时重传
    - 错误校验
    - 流量控制和阻塞管理

**TCP与UDP的不同点**

- 面向连接（确认有创建三方交握，连接已创建才作传输。）
- 有序数据传输
- 重发丢失的数据包
- 舍弃重复的数据包
- 无差错的数据传输
- 阻塞/流量控制

## TCP 客户端程序开发

1. 创建客户端套接字对象
2. 和服务端套接字建立连接
3. 发送数据
4. 接收数据
5. 关闭客户端套接字

<div  align="left">
    <img src="/refer/TCP开发流程.png"  />
</div>

**socket 类的介绍**

- 导入 socket 模块
    **import socket**创建客户端 socket 对象
    **socket.socket(AddressFamily, Type)**

    **参数说明:**

- AddressFamily 表示IP地址类型, 分为IPv4和IPv6

    - Type 表示传输协议类型

    **方法说明:**

- connect((host, port)) 表示和服务端套接字建立连接, host是服务器ip地址，port是应用程序的端口号

    - send(data) 表示发送数据，data是二进制数据
    - recv(buffersize) 表示接收数据, buffersize是每次接收数据的长度

```python
import socket


if __name__ == '__main__':
    # 创建tcp客户端套接字
    # 1. AF_INET：表示ipv4
    # 2. SOCK_STREAM: tcp传输协议
    tcp_client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 和服务端应用程序建立连接
    tcp_client_socket.connect(("192.168.131.62", 8080))
    # 代码执行到此，说明连接建立成功
    # 准备发送的数据
    send_data = "你好服务端，我是客户端小黑!".encode("gbk")
    # 发送数据
    tcp_client_socket.send(send_data)
    # 接收数据, 这次接收的数据最大字节数是1024
    recv_data = tcp_client_socket.recv(1024)
    # 返回的直接是服务端程序发送的二进制数据
    print(recv_data)
    # 对数据进行解码
    recv_content = recv_data.decode("gbk")
    print("接收服务端的数据为:", recv_content)
    # 关闭套接字
    tcp_client_socket.close()
```

1. 导入socket模块
2. 创建TCP套接字‘socket’
    - 参数1: ‘AF_INET’, 表示IPv4地址类型
    - 参数2: ‘SOCK_STREAM’, 表示TCP传输协议类型
3. 发送数据‘send’
    - 参数1: 要发送的二进制数据， 注意: 字符串需要使用encode()方法进行编码
4. 接收数据‘recv’
    - 参数1: 表示每次接收数据的大小，单位是字节
5. 关闭套接字‘socket’表示通信完成

## TCP 服务端程序开发

1. 创建服务端端套接字对象
2. 绑定端口号
3. 设置监听
4. 等待接受客户端的连接请求
5. 接收数据
6. 发送数据
7. 关闭套接字

<div  align="left">
    <img src="/refer/TCP开发流程.png"  />
</div>


**socket 类的介绍**

导入 socket 模块
**import socket**

创建服务端 socket 对象
**socket.socket(AddressFamily, Type)**

**参数说明:**

- AddressFamily 表示IP地址类型, 分为IPv4和IPv6
- Type 表示传输协议类型

**方法说明:**

- bind((host, port)) 表示绑定端口号, host 是 ip 地址，port 是端口号，ip 地址一般不指定，表示本机的任何一个ip地址都可以。
- listen (backlog) 表示设置监听，backlog参数表示最大等待建立连接的个数。
- accept() 表示等待接受客户端的连接请求
- send(data) 表示发送数据，data 是二进制数据
- recv(buffersize) 表示接收数据, buffersize 是每次接收数据的长度

```python
import socket

if __name__ == '__main__':
    # 创建tcp服务端套接字
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 设置端口号复用，让程序退出端口号立即释放
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True) 
    # 给程序绑定端口号
    tcp_server_socket.bind(("", 8989))
    # 设置监听
    # 128:最大等待建立连接的个数， 提示： 目前是单任务的服务端，同一时刻只能服务与一个客户端，后续使用多任务能够让服务端同时服务与多个客户端，
    # 不需要让客户端进行等待建立连接
    # listen后的这个套接字只负责接收客户端连接请求，不能收发消息，收发消息使用返回的这个新套接字来完成
    tcp_server_socket.listen(128)
    # 等待客户端建立连接的请求, 只有客户端和服务端建立连接成功代码才会解阻塞，代码才能继续往下执行
    # 1. 专门和客户端通信的套接字： service_client_socket
    # 2. 客户端的ip地址和端口号： ip_port
    service_client_socket, ip_port = tcp_server_socket.accept()
    # 代码执行到此说明连接建立成功
    print("客户端的ip地址和端口号:", ip_port)
    # 接收客户端发送的数据, 这次接收数据的最大字节数是1024
    recv_data = service_client_socket.recv(1024)
    # 获取数据的长度
    recv_data_length = len(recv_data)
    print("接收数据的长度为:", recv_data_length)
    # 对二进制数据进行解码
    recv_content = recv_data.decode("gbk")
    print("接收客户端的数据为:", recv_content)
    # 准备发送的数据
    send_data = "ok, 问题正在处理中...".encode("gbk")
    # 发送数据给客户端
    service_client_socket.send(send_data)
    # 关闭服务与客户端的套接字， 终止和客户端通信的服务
    service_client_socket.close()
    # 关闭服务端的套接字, 终止和客户端提供建立连接请求的服务
    tcp_server_socket.close()
```

1. 导入socket模块
2. 创建TCP套接字‘socket’
    - 参数1: ‘AF_INET’, 表示IPv4地址类型
    - 参数2: ‘SOCK_STREAM’, 表示TCP传输协议类型
3. 绑定端口号‘bind’
    - 参数: 元组, 比如:(ip地址, 端口号)
4. 设置监听‘listen’
    - 参数: 最大等待建立连接的个数
5. 等待接受客户端的连接请求‘accept’
6. 发送数据‘send’
    - 参数: 要发送的二进制数据， 注意: 字符串需要使用encode()方法进行编码
7. 接收数据‘recv’
    - 参数: 表示每次接收数据的大小，单位是字节，注意: 解码成字符串使用decode()方法
8. 关闭套接字‘socket’表示通信完成

## TCP网络应用程序的注意点

1. 当 TCP 客户端程序想要和 TCP 服务端程序进行通信的时候必须要先建立连接
2. TCP 客户端程序一般不需要绑定端口号，因为客户端是主动发起建立连接的。
3. TCP 服务端程序必须绑定端口号，否则客户端找不到这个 TCP 服务端程序。
4. listen 后的套接字是被动套接字，只负责接收新的客户端的连接请求，不能收发消息。
5. 当 TCP 客户端程序和 TCP 服务端程序连接成功后， TCP 服务器端程序会产生一个新的套接字，收发客户端消息使用该套接字。
6. 关闭 accept 返回的套接字意味着和这个客户端已经通信完毕。
7. 关闭 listen 后的套接字意味着服务端的套接字关闭了，会导致新的客户端不能连接服务端，但是之前已经接成功的客户端还能正常通信。
8. 当客户端的套接字调用 close 后，服务器端的 recv 会解阻塞，返回的数据长度为0，服务端可以通过返回数据的长度来判断客户端是否已经下线，反之服务端关闭套接字，客户端的 recv 也会解阻塞，返回的数据长度也为0。

## socket之send和recv原理剖析

1. 认识TCP socket的发送和接收缓冲区
    当创建一个TCP socket对象的时候会有一个发送缓冲区和一个接收缓冲区，这个发送和接收缓冲区指的就是内存中的一片空间。

2. send原理剖析
    send是不是直接把数据发给服务端?
    不是，要想发数据，必须得通过网卡发送数据，应用程序是无法直接通过网卡发送数据的，它需要调用操作系统接口，也就是说，应用程序把发送的数据先写入到发送缓冲区(内存中的一片空间)，再由操作系统控制网卡把发送缓冲区的数据发送给服务端网卡 。

3. recv原理剖析
    recv是不是直接从客户端接收数据?
    不是，应用软件是无法直接通过网卡接收数据的，它需要调用操作系统接口，由操作系统通过网卡接收数据，把接收的数据写入到接收缓冲区(内存中的一片空间），应用程序再从接收缓存区获取客户端发送的数据。

不管是recv还是send都不是直接接收到对方的数据和发送数据到对方，发送数据会写入到发送缓冲区，接收数据是从接收缓冲区来读取，发送数据和接收数据最终是由操作系统控制网卡来完成。

## 三次握手与四次挥手

<div  align="left">
    <img src='/refer/TCP的三次握手和四次挥手.png' />
</div>
- 三次握手

> 1. 客户端对服务端说，我的序号是x，我要向你请求连接。（第一次握手，SYN=1，SYN表示发起一个新连接，seq=x，然后进入SYN-SEND同步发送状态）
> 2. 服务端收到报文之后对客户端说，我的序号是y，期待你下一句序号为x+1的话（意思就是收到了序号为x的话，即ack=x+1，ack是确认号，确认序号字段有效），同意建立连接。（第二次握手，SYN=1，ACK=1，seq=y，ack=x+1，然后进入SYN-RCVD同步接收状态，大写的ACK是标识确认序号有效的）
> 3. 客户端听到(收到报文)服务端说同意建立连接之后，对服务端说：已确认你同意与我连接(ack=y+1，ACK=1，seq=x+1）。（第三次握手，客户端已进入ESTABLISHED状态）
> 4. 服务端听到客户端的确认之后，也进入ESTABLISHED状态。

- 四次挥手

> 1. 客户端与服务端交谈结束之后，客户端要结束此次会话，对服务端说：我要关闭连接了（seq=u,FIN=1,FIN标识释放一个连接）。（第一次挥手，客户端进入FIN-WAIT-1半关闭阶段，停止在客户端到服务器端方向上发送数据，但是客户端仍然能接收从服务器端传输过来的数据。）
>
> 2. 服务端收到客户端的消息后说：确认，你要关闭连接了。（seq=v,ack=u+1,ACK=1）（第二次挥手，确认了客户端想要释放连接，服务端进入CLOSE-WAIT半关闭状态）。客户端收到服务端的确认后，确认了服务器收到了客户端发出的释放连接请求（此时客户端进入FIN-WAIT-2）
> 3. 服务端说完了他要说的话（只是可能还有话说）之后，对客户端说，我要关闭连接了。（seq=w, ack=u+1,FIN=1，ACK=1）(第三次挥手，进入LAST-ACK最后确认阶段，停止在服务器端到客户端的方向上发送数据)
> 4. 客户端收到服务端要结束连接的消息后说：已收到你要关闭连接的消息。（seq=u+1,ack=w+1,ACK=1）(第四次挥手，确认了服务器端已做好释放连接的准备，，然后客户端进入TIME-WAIT时间等待阶段，是4分钟)
> 5. 服务端收到客户端的确认后，进入CLOSED。正式确认关闭服务器端到客户端方向上的连接。
> 6. 客户端结束TIME-WAIT阶段，进入CLOSED阶段，由此完成“四次挥手”。
>
> 前"两次挥手"既让服务器端知道了客户端想要释放连接，也让客户端知道了服务器端了解了自己想要释放连接的请求。于是，可以确认关闭客户端到服务器端方向上的连接了；
>
> 后“两次挥手”既让客户端知道了服务器端准备好释放连接了，也让服务器端知道了客户端了解了自己准备好释放连接了。于是，可以确认关闭服务器端到客户端方向上的连接了，由此完成“四次挥手”。

## 长连接和短连接

短链接：
建立连接——数据传输——关闭连接...建立连接——数据传输——关闭连接

长链接：
建立连接——数据传输...（保持连接）...数据传输——关闭连接

**长/短连接的优点和缺点**

- 长连接可以省去较多的TCP建立和关闭的操作，减少浪费，节约时间。
    对于频繁请求资源的客户来说，较适用长连接。
- client与server之间的连接如果一直不关闭的话，会存在一个问题，
    随着客户端连接越来越多，server早晚有扛不住的时候，这时候server端需要采取一些策略，
    如关闭一些长时间没有读写事件发生的连接，这样可以避免一些恶意连接导致server端服务受损；
    如果条件再允许就可以以客户端机器为颗粒度，限制每个客户端的最大长连接数，
    这样可以完全避免某个蛋疼的客户端连累后端服务。
- 短连接对于服务器来说管理较为简单，存在的连接都是有用的连接，不需要额外的控制手段。
- 但如果客户请求频繁，将在TCP的建立和关闭操作上浪费时间和带宽。

# TCP与UDP的区别

- tcp是面向连接的协议，udp是无连接协议
- tcp是可靠的，它会保证数据传输给客户端，丢失重发；udp是不可靠的，它发送的数据可能会在传输过程中丢失，且不会重发
- tcp传输的数据是有序的，在报文中会给每个包一个序号，客户端根据序号按序接收；udp不提供有序性的保证
- tcp是基于字节流的，udp是基于数据报的
- tcp速度慢，它传输数据要做的事情很多，要建立连接，要保证数据完整传到等等；udp的话做的事情就比较少，所以它的速度比udp快
- tcp有流量控制和阻塞控制；udp没有

