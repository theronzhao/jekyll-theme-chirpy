---
title: pyecharts饼图使用笔记
date: 2021-07-18 18:58:32 +0800
categories: [数据可视化]
tags: [数据可视化, pyecharts]
---


## InitOpts：初始化配置项

```python
class InitOpts(
    # 图表画布宽度，css 长度单位。
    width: str = "900px",

    # 图表画布高度，css 长度单位。
    height: str = "500px",

    # 图表 ID，图表唯一标识，用于在多图表时区分。
    chart_id: Optional[str] = None,

    # 渲染风格，可选 "canvas", "svg"
    # # 参考 `全局变量` 章节
    renderer: str = RenderType.CANVAS,

    # 网页标题
    page_title: str = "Awesome-pyecharts",

    # 图表主题
    theme: str = "white",

    # 图表背景颜色
    bg_color: Optional[str] = None,

    # 远程 js host，如不设置默认为 https://assets.pyecharts.org/assets/"
    # 参考 `全局变量` 章节
    js_host: str = "",

    # 画图动画初始化配置，参考 `global_options.AnimationOpts`
    animation_opts: Union[AnimationOpts, dict] = AnimationOpts(),
)
```

## Pie：饼图

```python
class Pie(
    # 初始化配置项，参考 `global_options.InitOpts`
    init_opts: opts.InitOpts = opts.InitOpts()
)
```

```python
def add(
    # 系列名称，用于 tooltip 的显示，legend 的图例筛选。
    series_name: str,

    # 系列数据项，格式为 [(key1, value1), (key2, value2)]
    data_pair: types.Sequence[types.Union[types.Sequence, opts.PieItem, dict]],

    # 系列 label 颜色
    color: Optional[str] = None,

    # 饼图的半径，数组的第一项是内半径，第二项是外半径
    # 默认设置成百分比，相对于容器高宽中较小的一项的一半
    radius: Optional[Sequence] = None,

    # 饼图的中心（圆心）坐标，数组的第一项是横坐标，第二项是纵坐标
    # 默认设置成百分比，设置成百分比时第一项是相对于容器宽度，第二项是相对于容器高度
    center: Optional[Sequence] = None,

    # 是否展示成南丁格尔图，通过半径区分数据大小，有'radius'和'area'两种模式。
    # radius：扇区圆心角展现数据的百分比，半径展现数据的大小
    # area：所有扇区圆心角相同，仅通过半径展现数据大小
    rosetype: Optional[str] = None,

    # 饼图的扇区是否是顺时针排布。
    is_clockwise: bool = True,

    # 标签配置项，参考 `series_options.LabelOpts`
    label_opts: Union[opts.LabelOpts, dict] = opts.LabelOpts(),

    # 提示框组件配置项，参考 `series_options.TooltipOpts`
    tooltip_opts: Union[opts.TooltipOpts, dict, None] = None,

    # 图元样式配置项，参考 `series_options.ItemStyleOpts`
    itemstyle_opts: Union[opts.ItemStyleOpts, dict, None] = None,

    # 可以定义 data 的哪个维度被编码成什么。
    encode: types.Union[types.JSFunc, dict, None] = None,
)
```

## PieItem：饼图数据项

```python
class PieItem(
    # 数据项名称。
    name: Optional[str] = None,

    # 数据值。
    value: Optional[Numeric] = None,

    # 该数据项是否被选中。
    is_selected: bool = False,

    # 标签配置项，参考 `series_options.LabelOpts`
    label_opts: Union[LabelOpts, dict, None] = None,

    # 图元样式配置项，参考 `series_options.ItemStyleOpts`
    itemstyle_opts: Union[ItemStyleOpts, dict, None] = None,

    # 提示框组件配置项，参考 `series_options.TooltipOpts`
    tooltip_opts: Union[TooltipOpts, dict, None] = None,
)
```

## 饼图标签的视觉引导线样式

```python
class PieLabelLineOpts(
    # 是否显示视觉引导线。
    is_show: bool = True,

    # 视觉引导线第一段的长度。
    length: Numeric = None,

    # 视觉引导项第二段的长度。
    length_2: Numeric = None,

    # 是否平滑视觉引导线，默认不平滑，可以设置成 true 平滑显示。
    # 也可以设置为 0 到 1 的值，表示平滑程度。
    smooth: Union[bool, Numeric] = False,

    # 线条样式，参考 `LineStyleOpts`
    linestyle_opts: Union[LineStyleOpts, dict, None] = None,
)
```

## demo



```python
from pyecharts.charts import Pie
from pyecharts.render import make_snapshot
from snapshot_phantomjs import snapshot
from pyecharts import options as opts

data = {"very_danger": 7, "more_danger": 8, "more_safe": 4, "very_safe": 9}
host_level = ["非常危险", "比较危险", "比 较安全", "非常安全"]
host_data = [int(data["very_danger"]), int(data["more_danger"]), int(data["more_safe"]), int(data["very_safe"])]

pie = Pie()
pie.add("",[list(z) for z in zip (host_level,host_data)])
pie.set_series_opts(label_opts=opts.LabelOpts(is_show=True,color=["#F36266", "#F9A84A", "#3ECBF7", "#19C868"],formatter="{b}: {d}%"))

pie.render("/root/123.html")
```

![](/refer/pyecharts-pie.png)

## 渲染图片

使用 pyecharts 渲染成图片一直是开发者比较关心的功能，pyecharts 提供了 `selenium`, `phantomjs` 和 `pyppeteer` 三种方式。

## [make_snapshot](https://pyecharts.org/#/zh-cn/render_images?id=make_snapshot)

> make_snapshot 用于 pyecharts 直接生成图片

### [引入](https://pyecharts.org/#/zh-cn/render_images?id=引入)

```python
from pyecharts.render import make_snapshot
```

### [API](https://pyecharts.org/#/zh-cn/render_images?id=api)

```python
def make_snapshot(
    # 渲染引擎，可选 selenium 或者 phantomjs
    engine: Any,

    # 传入 HTML 文件路径
    file_name: str,

    # 输出图片路径
    output_name: str,

    # 延迟时间，避免图还没渲染完成就生成了图片，造成图片不完整
    delay: float = 2,

    # 像素比例，用于调节图片质量
    pixel_ratio: int = 2,

    # 渲染完图片是否删除原 HTML 文件
    is_remove_html: bool = False,

    # 浏览器类型，目前仅支持 Chrome, Safari，使用 snapshot-selenium 时有效
    browser: str = "Chrome",
    **kwargs,
)
```

## [snapshot-phantomjs](https://pyecharts.org/#/zh-cn/render_images?id=snapshot-phantomjs)

pyecharts生成饼图，基于pyecharts生成的饼图，想要生成图片，需要安装snapshot-phantomjs，而snapshot-phantomjs基于phantomjs，环境中需要安装phantomjs并为phantomjs配置好环境变量

### [安装](https://pyecharts.org/#/zh-cn/render_images?id=安装-1)

```shell
$ pip install snapshot-phantomjs
```

[snapshot-phantomjs](https://github.com/pyecharts/snapshot-phantomjs) 是 pyecharts + phantomjs 渲染图片的扩展，需要先安装 phantomjs，安装方法请参照官网 [phantomjs.org/download.html](http://phantomjs.org/download.html)

```python
from pyecharts.charts import Pie
from pyecharts.render import make_snapshot
from snapshot_phantomjs import snapshot
from pyecharts import options as opts

data = {"very_danger": 7, "more_danger": 8, "more_safe": 4, "very_safe": 9}
host_level = ["非常危险", "比较危险", "比 较安全", "非常安全"]
host_data = [int(data["very_danger"]), int(data["more_danger"]), int(data["more_safe"]), int(data["very_safe"])]

pie = Pie()
pie.add("",[list(z) for z in zip (host_level,host_data)])
pie.set_series_opts(label_opts=opts.LabelOpts(is_show=True,color=["#F36266", "#F9A84A", "#3ECBF7", "#19C868"],formatter="{b}: {d}%"))

make_snapshot(snapshot, pie.render(), "/root/bar0.png")
```

## 资源引用

pyecharts 使用的所有静态资源文件存放于 [pyecharts-assets](https://github.com/pyecharts/pyecharts-assets) 项目中，

默认挂载在 `https://assets.pyecharts.org/assets/`

### [Localhost-Server](https://pyecharts.org/#/zh-cn/assets_host?id=localhost-server)

pyecharts 提供了更改全局 HOST 的快捷方式，下面以开发者启动本地 FILE SERVER 为例，操作如下。

1. 获取 pyecharts-assets 项目

    ```shell
     $ git clone https://github.com/pyecharts/pyecharts-assets.git
    ```

2. 启动 HTTP file server

    ```shell
     $ cd pyecharts-assets
     $ python -m http.server
     # Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
     # 默认会在本地 8000 端口启动一个文件服务器
    ```

3. 配置 pyecharts 全局 HOST

    ```python
     # 只需要在顶部声明 CurrentConfig.ONLINE_HOST 即可
     from pyecharts.globals import CurrentConfig
     CurrentConfig.ONLINE_HOST = "http://127.0.0.1:8000/assets/"
    
     # 接下来所有图形的静态资源文件都会来自刚启动的服务器
     from pyecharts.charts import Bar
     bar = Bar()
    ```

### [Notebook-Server](https://pyecharts.org/#/zh-cn/assets_host?id=notebook-server)

pyecharts v1.5.1+ 起开始支持 Notebook 插件作为静态资源服务。

1. 获取 pyecharts-assets 项目

    ```shell
     $ git clone https://github.com/pyecharts/pyecharts-assets.git
    ```

2. 安装扩展插件

    ```shell
     $ cd pyecharts-assets
     # 安装并激活插件
     $ jupyter nbextension install assets
     $ jupyter nbextension enable assets/main
    ```

3. 配置 pyecharts 全局 HOST

    ```python
     # 只需要在顶部声明 CurrentConfig.ONLINE_HOST 即可
     from pyecharts.globals import CurrentConfig, OnlineHostType
    
     # OnlineHostType.NOTEBOOK_HOST 默认值为 http://localhost:8888/nbextensions/assets/
     CurrentConfig.ONLINE_HOST = OnlineHostType.NOTEBOOK_HOST
    
     # 接下来所有图形的静态资源文件都会来自刚启动的服务器
     from pyecharts.charts import Bar
     bar = Bar()
    ```

