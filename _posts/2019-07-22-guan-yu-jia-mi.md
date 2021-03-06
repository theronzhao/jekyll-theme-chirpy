---
title: 关于加密
date: 2019-07-22 12:44:00 +0800
categories: [计算机基础]
tags: [计算机基础]
---

## 加密

保障信息安全的一部分

- 信息安全主要包括：
- 防止窃听
- 防止篡改
- 防止抵赖
- 防止回放

## 加密算法的分类

两类半-摘要算法严格来说不是

- 对称加密：加密和解密的时候用到的密钥是一样的(无法防止抵赖)
- 非对称加密：加密和解密的时候用到的密钥是不一样的(密钥是成对的) 
- 密钥：公钥(公开)，私钥(私有)
- 密钥空间：密文的可能值，表示一种密文的所有可能性

非对称加密算法：RSA，DSA/DSS     在客户端与服务端相互验证的过程中用的是对称加密 
对称加密算法：AES，RC4，3DES     客户端与服务端相互验证通过后，以随机数作为密钥时，就是对称加密
HASH算法：MD5，SHA1，SHA256  在确认握手消息没有被篡改时 

## 非对称加密主要有两个用途：

① 密钥交换：公钥加密，私钥解密
② 签名：私钥加密-->摘要-->公钥解密-->还原摘要-->证明是你发出的信息

## 摘要算法

- 可以把不同长度的内容变为特定长度的内容，相同的内容得到相同的结果，如果内容发生变化，则得到完全不同的内容

- 可以判断内容是否被篡改过

## 常见的各类加密算法名称

- 对称加密
DES  3DES  AES   美国政府以前用的

- 非对称加密：慢，明文只能在一定的长度，超过这个长度就无法加密了
RSA  ECC(椭圆加密算法)

- 摘要算法
MD5  SHA-1  SHA-256  SHA-512  SHA-384  CRC
每个算法最后获得的摘要长度可能不同 

## 作用

▶防止窃听
使用对称加密即可
使用非对称加密算法 来进行密钥交换

▶防止篡改
明文-->摘要算法-->摘要值-->密文-->发过去

▶防止抵赖--签名
摘要值   私钥加密 (摘要值)  => 签名

▶防止回放
在原始信息中添加额外的信息，时间戳


https：参见后文

## django中的密码是如何保存的


django中密码的保存是摘要值

盐 salt  一个若干位的10+随机字符串

10^14


用户的真实密码  + 盐   => 摘要值1

用户的真实密码  +  摘要值1  => 摘要值2

用户的真实密码  +  摘要值2  => 摘要值36000



## https验证流程

- 1 客户端发起一个https的请求，把自身支持的一系列Cipher Suite（密钥算法套件，简称Cipher）发送给服务端

- 2  服务端，接收到客户端所有的Cipher后与自身支持的对比，如果不支持则连接断开，反之则会从中选出一种加密算法和HASH算法     

    以证书的形式返回给客户端 证书中还包含了 公钥 颁证机构 网址 失效日期等等。

- 3 客户端收到服务端响应后会做以下几件事

    ​	3.1 验证证书的合法性    
    ​	颁发证书的机构是否合法与是否过期，证书中包含的网站地址是否与正在访问的地址一致等;证书验证通过后，在浏览器的地址栏会加上一把小锁(每家浏览器验证通过后的提示不一样 不做讨论)  

    ​	3.2 生成随机密码  
    ​		如果证书验证通过，或者用户接受了不授信的证书，此时浏览器会生成一串随机数，然后用证书中的公钥加密。  

    ​	3.3 HASH握手信息  
    ​	用最开始约定好的HASH方式，把握手消息取HASH值，  然后用 随机数加密 “握手消息+握手消息HASH值(签名)”  并一起发送给服务端  

    ​	在这里之所以要取握手消息的HASH值，主要是把握手消息做一个签名，用于验证握手消息在传输过程中没有被篡改过。  
    
- 4 服务端拿到客户端传来的密文，用自己的私钥来解密握手消息取出随机数密码，再用随机数密码 解密 握手消息与HASH值，并与传过来的HASH值做对比确认是否一致。

    ​	然后用随机密码加密一段握手消息(握手消息+握手消息的HASH值 )给客户端

- 5 客户端用随机数解密并计算握手消息的HASH，如果与服务端发来的HASH一致，此时握手过程结束，之后所有的通信数据将由之前浏览器生成的随机密码并利用对称加密算法进行加密

    因为这串密钥只有客户端和服务端知道，所以即使中间请求被拦截也是没法解密数据的，以此保证了通信的安全



