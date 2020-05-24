---
layout: post  
title: "Python Flask 安装"  
date: 2020-05-22 14:27:50 +0800  
categories: Java培训  
tag: "工具"  
---


Flask是用于Python的免费开放源微型Web框架，旨在帮助开发人员构建安全，可扩展和可维护的Web应用程序。 Flask基于[ Werkzeug ](http://werkzeug.pocoo.org/)，并使用[ Jinja2 ](http://jinja.pocoo.org/)作为模板引擎。

有多种安装Flask的方法，具体取决于您的需求。它可以安装在系统范围内，也可以使用pip安装在Python虚拟环境中。

## 一. 安装步骤

### 1.安装Python以及依赖

建议安装到Python的虚拟环境中。

Ubuntu默认安装的Python，验证命令：

```shell
python3 -V
# 输出如下：
Python 3.6.6
```

从Python 3.6开始，创建虚拟环境的推荐方法是使用`venv`模块。要安装提供`venv`模块的`python3-venv`软件包，请运行以下命令：

```shell
sudo apt install python3-venv
```

上面的命令创建一个名为`venv`的目录，该目录包含Python二进制文件的副本，[ Pip包管理器](https://www.myfreax.com/how-to-install-pip-on-ubuntu-18.04/)，标准Python库和其他支持文件。您可以为虚拟环境使用任何名称。

要开始使用此虚拟环境，您需要通过运行`activate`脚本将其激活：

```shell
source venv/bin/activate
```

一旦激活，虚拟环境的bin目录将添加到[ `$PATH` ](https://www.myfreax.com/how-to-add-directory-to-path-in-linux/)变量的开头。此外，您的Shell提示符也会更改，并且会显示您当前正在使用的虚拟环境的名称。在我们的例子中是`venv`：

### 2.安装Flask

现在已激活虚拟环境，您可以使用Python包管理器pip安装Flask：

```shell
pip install Flask
```

在虚拟环境中，可以使用命令`pip`代替`pip3`，使用`python`代替`python3`。

使用以下命令验证安装，该命令将显示Flask版本：

```shell
python -m flask --version
# 输出如下信息
Flask 1.0.2
Python 3.6.6 (default, Sep 12 2018, 18:26:19)
[GCC 8.0.1 20180414 (experimental) [trunk revision 259383]]
```

