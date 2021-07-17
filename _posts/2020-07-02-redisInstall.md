---
layout: post
title: Ubuntu16.04安装Redis
Author: TheronZhao
tags: 生产工具
---
## 安装

step1: 下载

> wget http://download.redis.io/releases/redis-4.0.9.tar.gz

step2: 解压

> tar xzf redis-x.x.x.tar.gz

step3: 移动，放到 usr/local ⽬录下

> sudo mv ./redis-x.x.x /usr/local/redis/

step4: 进⼊ redis ⽬录

> cd /usr/local/redis/

step5: 生成

> sudo make

step6: 测试,这段运⾏时间会较⻓

> sudo make test

step7: 安装,将 redis 的命令安装到 `/usr/local/bin/` ⽬录

> sudo make install

step8: 安装完成后，我们进入目录 `/usr/local/bin` 中查看

> cd /usr/local/bin
> ls -all

step9: 配置⽂件，移动到 `/etc/` ⽬录下

- 配置⽂件⽬录为 `/usr/local/redis/redis.conf`

    > sudo cp /usr/local/redis/redis.conf /etc/redis/

## 配置

- Redis 的配置信息在 `/etc/redis/redis.conf` 中

- 查看

    > sudo vim /etc/redis/redis.conf

- 绑定 ip：如果需要远程访问，可将此⾏注释，或绑定⼀个真实 ip

    > bind 127.0.0.1

- 端⼝，默认为: 6379

    > port 6379

- 是否以守护进程运⾏

    - 如果以守护进程运⾏，则不会在命令⾏阻塞，类似于服务
    - 如果以⾮守护进程运⾏，则当前终端被阻塞
    - 设置为 yes 表示守护进程，设置为 no 表示⾮守护进程
    - 推荐设置为 yes

    > daemonize yes

- 数据⽂件( 持久化时生成文件的名称 )

    > dbfilename dump.rdb

- 数据⽂件存储路径

    > dir /var/lib/redis

- ⽇志⽂件路径

    > logfile "/var/log/redis/redis-server.log"

- 数据库，默认有16个

    > database 16

- 主从复制，类似于双机备份

    > slaveof