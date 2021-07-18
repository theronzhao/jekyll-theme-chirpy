---
title: PostgreSql连接池
date: 2020-11-20 15:42:25 +0800
categories: [数据库]
tags: [PostgreSql]
---

### 连接池的作用及原理

正常访问数据库的过程中，每次访问都需要创建数据库的连接，这会消耗大量的资源；连接池的就是为数据库连接建立一个“缓冲区”，预先在缓冲池中放入一定数量的连接对象，当需要建立数据库连接时，只需从“缓冲池”中取出一个，使用完毕之后再放回去；且连接池允许多个客户端使用缓存起来的连接对象，这些对象可以连接数据库，它们是共享的、可被重复使用的；使用连接池可以节省大量资源，提高程序运行速度。

连接池的基本原理是：先初始化一定的数据库连接对象，并且把这些连接保存在连接池中。这些数据库连接的数量是由最小数据库连接数来设定的。连接池的最大数据库连接数量限定了这个连接池能占有的最大连接数，当应用程序向连接池请求的连接数超过最大连接数量时，这些请求将被加入到等待队列中。当程序需要访问数据库的时候，如果连接池中有空闲的连接，可直接得到一个连接；如果连接池对象中没有空闲的连接，且连接数没有达到最大，会创建一个新的连接从连接池中取出一个连接，数据库操作结束后，再把这个用完的连接重新放回连接池。

### Python连接PostgreSql

首先安装 psycopg2模块，pycopg2是Python编程语言的PostgreSQL数据库的适配器。



```undefined
pip install psycopg2
```

查阅[psycopg2官方手册](https://links.jianshu.com/go?to=http%3A%2F%2Finitd.org%2Fpsycopg%2Fdocs%2Findex.html)可以找到psycopg2.pool连接池



对PostgreSql连接池进行封装，代码如下：

```python
#!/usr/bin/env python
# -*- coding=utf-8 -*-
import traceback
from traceback import format_exc

import psycopg2
from DBUtils.PooledDB import PooledDB
from psycopg2.extras import RealDictCursor

from config.conf import PG_CONFIG
from logger.log import savelog


class PostgreSqlPool(object):
    """单例模式PostgreSql数据库连接池"""

    # __instance_lock = threading.Lock()
    #
    # def __new__(cls, *args, **kwargs):
    #     if not hasattr(PostgreSqlPool, "_instance"):
    #         with cls.__instance_lock:
    #             cls._instance = super(PostgreSqlPool, cls).__new__(cls, *args, **kwargs)
    #     return cls._instance

    def __init__(self, config, maxcached, cursor_factory=None):
        self.__psycopg_pool = None
        self.__CONFIG = config
        self.__maxcached = maxcached
        self.cursor_factory = cursor_factory

    def _get_pool(self):
        """
        创建连接池，retry三次
        :return:
        """
        for i in range(3):
            if self.__psycopg_pool is not None:
                break
            try:
                self.__psycopg_pool = PooledDB(creator=psycopg2, mincached=1, maxcached=self.__maxcached,
                                               cursor_factory=self.cursor_factory, **self.__CONFIG)
                return
            except Exception as e:
                self.__psycopg_pool = None
                self._deal_with_error(e)
            if self.__psycopg_pool is None and i == 2:
                raise Exception("Failed to Create Psycopg_pool")

    def _connect(self):
        """连接数据库"""
        if self.__psycopg_pool is None:
            self._get_pool()
        try:
            self.conn = self.__psycopg_pool.connection()
            self.cursor = self.conn.cursor()
        except Exception as e:
            self._deal_with_error(e)
            raise Exception("Failed to Connect to Database")

    def _disconnect(self):
        """释放数据库连接"""
        try:
            self.cursor.close()
            self.conn.close()
        except Exception as e:
            self._deal_with_error(e)

    def _execute(self, sql, param):
        """执行sql语句"""
        try:
            self._connect()
            self.cursor.execute(sql, param)
            self.conn.commit()
            result = self.cursor.rowcount
        except Exception as e:
            self.conn.rollback()
            savelog.error("error sql: {}".format(sql))
            savelog.error("error data: {}".format(param))
            self._deal_with_error(e)
            result = None
        finally:
            self._disconnect()
        return result

    def get_conn(self):
        """
        获取连接池的连接
        :return:
        """
        if self.__psycopg_pool is None:
            self._get_pool()
        return self.__psycopg_pool.connection()

    def get_one(self, sql, param=None):
        """
        查询一条数据
        :param sql:
        :param param: list or tuple
        :return: 查询结果 --> dict
        """
        try:
            self._connect()
            self.cursor.execute(sql, param)
            result = self.cursor.fetchone()
        except Exception as e:
            savelog.error("error sql: %s" % sql)
            self._deal_with_error(e)
            result = None
        finally:
            self._disconnect()
        return result

    def get_all(self, sql, param=None):
        """
        查询所有数据
        :param sql:
        :param param: list or tuple
        :return: 查询结果 --> [dict, dict, ...]
        """
        try:
            self._connect()
            self.cursor.execute(sql, param)
            result = self.cursor.fetchall()
        except Exception as e:
            savelog.error("error sql: %s" % sql)
            self._deal_with_error(e)
            result = None
        finally:
            self._disconnect()

        return result

    def insert_one(self, sql, param=None):
        """
        插入一条数据
        :param sql:
        :param param: list or tuple
        :return: 受影响的行数 --> int
        """
        return self._execute(sql, param)

    def insert_many(self, sql, param=None):
        """
        插入多条数据
        :param sql:
        :param param: [(v1,v2...), (v1,v2...)]
        :return: 受影响的行数 --> int
        """
        try:
            self._connect()
            self.cursor.executemany(sql, param)
            self.conn.commit()
            result = self.cursor.rowcount
        except Exception as e:
            savelog.error("error sql: %s" % sql)
            self.conn.rollback()
            self._deal_with_error(e)
            result = None
        finally:
            self._disconnect()
        return result

    def insert_and_get(self, sql, param=None):
        """
        插入数据的同时，获取数据
        :param sql:
        :param param:
        :return:
        """
        try:
            self._connect()
            self.cursor.execute(sql, param)
            self.conn.commit()
            result = self.cursor.fetchone()
        except Exception as e:
            savelog.error("error sql: %s" % sql)
            self.conn.rollback()
            self._deal_with_error(e)
            result = None
        finally:
            self._disconnect()
        return result

    def delete(self, sql, param=None):
        """
        删除
        :param sql:
        :param param:
        :return: 受影响的行数 --> int
        """
        return self._execute(sql, param)

    def update(self, sql, param=None):
        """
        更新
        :param sql:
        :param param:
        :return: 受影响的行数 --> int
        """
        return self._execute(sql, param)

    def do(self, sql, param=None):
        """
        执行其他sql
        :param sql:
        :param param: list or tuple
        :return:
        """
        return self._execute(sql, param)

    @staticmethod
    def transaction_do(param_list):
        """
        事务操作，伴随更新和插入
        :param param_list:
        :return:
        """
        conn, cur, count = None, None, None
        try:
            conn = mdb.get_conn()
            cur = conn.cursor()
            for param_dict in param_list:
                single, sql, param = param_dict.get('single'), param_dict.get('sql'), param_dict.get('param')
                if single:
                    cur.execute(sql, param)
                else:
                    cur.executemany(sql, param)
            count = cur.rowcount
            conn.commit()
        except Exception:
            savelog.error('transaction_do data_into_db error:{}'.format(traceback.format_exc()))
            count = None
            if conn:
                conn.rollback()
        finally:
            if cur:
                cur.close()
            if conn:
                conn.close()
        return count

    @staticmethod
    def _deal_with_error(e):
        savelog.error(format_exc())


db = PostgreSqlPool(PG_CONFIG, 30, RealDictCursor)


if __name__ == '__main__':
    pass

```





