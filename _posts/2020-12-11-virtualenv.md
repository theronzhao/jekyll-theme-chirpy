---
title: 虚拟环境
date: 2020-12-11 20:22:41 +0800
categories: [环境配置]
tags: [环境配置]
---

## 虚拟环境依赖安装
### **Linux/MacOS**

1.安装虚拟环境
```python
pip install virtualenv 
pip install virtualenvwrapper
```
2.修改环境变量

修改~/.bash_profile或其它环境变量相关文件(如 .bashrc 或用 ZSH 之后的 .zshrc)，添加以下语句
```shell
export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/workspace
source /usr/local/bin/virtualenvwrapper.sh
```
修改后使之立即生效(也可以重启终端使之生效)：
```bash
source ~/.bash_profile
```

### **Windows 下**

```python
pip install virtualenv 
pip install virtualenvwrapper-win 
```
【可选】Windows下默认虚拟环境是放在用户名下面的Envs中的，与桌面，我的文档，下载等文件夹在一块的。更改方法：计算机，属性，高级系统设置，环境变量，添加WORKON_HOME，如图（WORKON_HOME=D:\virenv）


## 虚拟环境使用方法
```shell
mkvirtualenv test：创建运行环境 test

workon test: 工作在 test 环境 或 从其它环境切换到 test 环境

deactivate: 退出终端环境


rmvirtualenv ENV：删除运行环境ENV

mkproject mic：创建mic项目和运行环境mic

mktmpenv：创建临时运行环境

lsvirtualenv: 列出可用的运行环境

lssitepackages: 列出当前环境安装了的包
```