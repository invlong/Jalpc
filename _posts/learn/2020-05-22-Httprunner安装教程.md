---
layout: post  
title: "Httprunner安装教程"  
date: 2020-05-22 14:27:50 +0800  
categories: Java培训  
tag: "工具"  
---

* content
{:toc}  


> 本篇介绍如何在Ubuntu服务器安装httprunner，介绍Ubuntu安装的详细步骤，并提供通用的思路以供参考。

### 一.名词解释

> 使用Linux系统需要对Linux本身有一定的了解，因此简单介绍一下此次安装时用到的一些工具

#### 1.Python

一种高级编程语言，比C++和Java想比，具有更简洁的语法。

Httprunner是一款基于Python的测试框架，因此安装之前需要安装Python，虽然官网中有讲到支持Python2.x，但是2.x在使用中会遇到中文乱码等各种问题，所以一定要升级Python版本到3.6以上。

**并且，一定一定要在安装httprunner之前升级Python，本人使用的Python版本是3.6.8**

#### 2.pyenv

PythonEnvironment，是Python控制版本的工具。可以灵活的安装多个Python版本，并且可以灵活地切换。

[pyenv-github地址](https://github.com/pyenv/pyenv)

#### 3.pyenv installer

pyenv的一个安装工具，在github上开源，是官方推荐的安装pyenv的工具

[pyenv install 的github地址](https://github.com/pyenv/pyenv-installer)

#### 4.pyenv virtualenv

pyenv自带的一个虚拟环境插件，pyenv提供了很多插件，这个虚拟环境插件可以启动虚拟环境。**强烈建议httprunner安装在虚拟环境中**，因为httprunner我至今不知道怎样卸载和降级😅

#### 5.pip

pip是Python的一个软件包管理系统，通过pip来安装Python的软件，类似于centos的yum，ubutnu的apt等等。本文中的httprunner也是通过pip来安装的。

**但是一定要先升级pip或者安装pip3来安装httprunner，因为pip版本过低会导致一些问题。**

#### 6.httprunner

2018年5月上线的非常新的测试框架，目前作者维护的非常频率非常高，虽然时间不是很久，但是star数量以及教程还算不少，是[霍格沃兹测试学院](https://www.testing-studio.com/)赞助的一款开源框架。

相关链接：

[TesterHome](https://testerhome.com/)

[校验器的设计学习](https://testerhome.com/topics/11207)

[作者在TesterHome的主页](https://testerhome.com/debugtalk/topics)

### 二.安装教程

```flow
st=>start: 使用pyenv install安装ypenv
op=>operation: 使用pyenv安装Python3.6.8
op2=>operation: 切换Python默认版本为3.6.8
op3=>operation: 升级pip为最新版本或者使用pip3
op4=>operation: 使用pyenv virtualenv创建虚拟环境
op5=>operation: 使用虚拟环境安装httprunner
e=>end: End

st->op->op2->op3->op4->op5->e
```



#### 1.pyenv install的使用

[pyenv-github地址](https://github.com/pyenv/pyenv)

github中的readme页面有详细的介绍，我主要做一些翻译和简化的说明。

（1）安装前的必装软件

参考[installation wiki](https://github.com/pyenv/pyenv/wiki/Common-build-problems)，选择自己的操作系统来安装依赖。

（2）安装

```shell
# 在线安装 在线安装可能会存在网络的问题，可能跟修改源有关？
curl https://pyenv.run | bash
# 或者使用下面命令，两个等价，使用一个即可
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
```

（3）重启shell

```shell
 exec $SHELL
```

（4）写入pyenv相关路径到环境变量中

```shell
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
#然后刷新配置
source ~/.bashrc
```

（5）更新

```shell
 pyenv update
```

（6）卸载

想要卸载直接删除即可

```shell
rm -fr ~/.pyenv
```

#### 2.pyenv 安装Python

（1）安装Python

```shell
# 查看可安装的版本列表
pyenv install -l
# 安装指定版本的Python
pyenv install 3.6.8
# 查看安装版本的目录
ls ~/.pyenv/versions/
# 卸载某个版本的Python
pyenv uninstall 3.6.8
# 或者直接删除也可
rm -rf ~/.pyenv/versions/3.6.8
# 查看本机安装的版本
pyenv versions
# 切换版本
pyenv global 3.6.8
pyenv local 3.6.8
```

（2）安装虚拟环境

```shell
# 创建env1环境
pyenv virtualenv env1
# 列出所有的环境
pyenv virtualenvs
# 激活指定的虚拟环境
pyenv activate env
#退出虚拟环境，回到系统环境
pyenv deactivate
```

（3）删除虚拟环境

```shell
pyenv uninstall env-name
rm -rf ~/.pyenv/versions/env-name
```

[参考文章地址](http://einverne.github.io/post/2017/04/pyenv.html#fn:auto)

#### 3.pip换源和升级

（1）换源

新建`vim ~/.pip/pip.conf`，并写入

```
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```

（2）升级pip

```shell
pip install -U pip	
```

#### 4.安装httprunner

（1）切换Python的默认版本为3.6.8

（2）进入虚拟环境

（3）安装指定的httprunner版本

httprunner3.0以上版本太新了，还不是很稳定，而官方给的安装命令是安装最细版的，所以我使用安装制定版本的命令。

```shell
pip install git+https://github.com/HttpRunner/HttpRunner.git@v2.5.7
```

### 三.问题小计

#### 1.使用pyenv install 时提示 `Failed to connect to 127.0.0.1 port 8888: 拒绝连接`

是由于我在bashrc中设置了代理，导致8888端口被占用，可以临时处理这个问题：

```shell
export http_proxy=''
export https_proxy=''
```

#### 2.安装后提示pyenv 命令找不到 

```shell
export PATH="$HOME/.pyenv/bin:$PATH"    
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
# 刷新source
source ~/.bashrc
```
