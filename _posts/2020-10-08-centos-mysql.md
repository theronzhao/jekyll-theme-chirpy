---
title: centos 7.6安装MySQL5.7
date: 2020-10-08 21:34:55 +0800
categories: [环境配置]
tags: [环境配置]
---




1.卸载默认安装的mariadb

```
yum search mysql

yum remove mariadb.x86_64
```

2.安装

```
wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
```

3.本地yum源安装

```
yum localinstall mysql57-community-release-el7-11.noarch.rpm
```

4.检测是否已安装

```
yum search mysql57-community-release-el7-11.noarch.rpm
```

5.安装

```
yum install mysql-community-server.x86_64
```

6.启动mysql

```
service mysqld start
```

7.修改默认密码

```
cat /var/log/mysqld.log | grep password
```

8.修改密码
登录MySQL后

```
SET PASSWORD = PASSWORD(' 1234567 ');
```

9.修改密码策略

```
set global validate_password_policy=0
set global validate_password_length=5;
```