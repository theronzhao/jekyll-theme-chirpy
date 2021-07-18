---
title: Flask程序运行报错signal only works in main thread
date: 2020-10-29 19:44:55 +0800
categories: [BUG专栏]
tags: [Flask, BUG]
---



**报错信息:**

```python
...此处省略
File "/usr/lib/python3.5/signal.py", line 47, in signal
    handler = _signal.signal(_enum_to_int(signalnum), _enum_to_int(handler))
ValueError: signal only works in main thread
```

推测与项目中socketio有关
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405200759785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RlcmVrVGhlcm9u,size_16,color_FFFFFF,t_70)
**解决方案:**
**1**.socketio 插件在使用flask run启动，出现 ValueError: signal only works in main thread 异常的问题。如果非要flask run启动，flask run --no-reload 切记一定要带 --no-reload 参数，本人测试已经成功。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405201705196.png)
**2**.如果是使用app.run启动,run()指定参数use_reloader=False, 这个方法我还没进行尝试

```python
app.run(debug=True, use_reloader=False)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405201224621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RlcmVrVGhlcm9u,size_16,color_FFFFFF,t_70)

