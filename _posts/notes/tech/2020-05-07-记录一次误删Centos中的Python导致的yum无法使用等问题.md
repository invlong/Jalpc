---
layout: post  
title: "记录一次误删Centos中的Python导致的yum无法使用等问题"  
date: 2020-05-07 14:27:50 +0800  
categories: 个人笔记  
tag: "tech"  
---

* content
{:toc}  

> 最近在服务器上安装HttpRunner时，想统一Python版本，删除了系统的一些旧版本，导致了一些问题，记录修复过程

#### 1.删除现有Python

`yum和Python版本不兼容会导致一系列的问题，所以要先卸载旧版本`

```sh
rpm -qa|grep python|xargs rpm -ev --allmatches --nodeps #卸载python
whereis python |xargs rm -frv ##删除所有残余文件
whereis python ##验证删除，返回无结果
```

#### 2.删除现有的yum

`yum和Python版本不兼容会导致一系列的问题，所以要先卸载旧版本`

```sh
rpm -qa|grep yum|xargs rpm -ev --allmatches --nodeps #删除yum
whereis yum |xargs rm -frv #删除残留文件
whereis yum #验证删除完成
```

#### 3.安装对应Centos版本的Python包

**下载并安装，注意顺序，先安装python 然后 yum。不然安装后还会报错，重新来一遍。**

首先查询系统版本

```sh
cat /etc/redhat-release # 我的版本是：CentOS Linux release 7.6.1810 (Core)
```

根据系统版本，下载Python包

[官网地址](http://vault.centos.org/) 例：`http://vault.centos.org/7.6.1810/os/x86_64/Packages/`

对国内流量支持不是很好，建议使用翻墙访问，我遇到搜索包名找不到，以为是没有，实际是国内流量无法完全加载所有的包。

执行下面的命令来安装Python包

```sh
rpm -ivh --nodeps http://vault.centos.org/7.6.1810/os/x86_64/Packages/python-2.7.5-76.el7.x86_64.rpm
rpm -ivh --nodeps http://vault.centos.org/7.6.1810/os/x86_64/Packages/python-devel-2.7.5-76.el7.x86_64.rpm
rpm -ivh --nodeps http://vault.centos.org/7.6.1810/os/x86_64/Packages/python-iniparse-0.4-9.el7.noarch.rpm
rpm -ivh --nodeps http://vault.centos.org/7.6.1810/os/x86_64/Packages/python-libs-2.7.5-76.el7.x86_64.rpm
rpm -ivh --nodeps http://vault.centos.org/7.6.1810/os/x86_64/Packages/python-pycurl-7.19.0-19.el7.x86_64.rpm
rpm -ivh --nodeps http://vault.centos.org/7.6.1810/os/x86_64/Packages/python-urlgrabber-3.10-9.el7.noarch.rpm
rpm -ivh --nodeps http://vault.centos.org/7.6.1810/os/x86_64/Packages/rpm-python-4.11.3-35.el7.x86_64.rpm
```

上面是在线安装，如果你的服务器没有翻墙的话，下载会比较慢，可以使用浏览器下载后拷贝到服务器，使用离线安装，命令例如：

```sh
rpm -ivh --nodeps python-2.7.5-76.el7.x86_64.rpm # 到对应的目录执行
```

**--nodeps表示忽略依赖关系，不然安装时可能导致不成功**

#### 4.安装对应Centos版本的yum包

```sh
rpm -ivh --nodeps http://vault.centos.org/7.6.1810/os/x86_64/Packages/yum-3.4.3-161.el7.centos.noarch.rpm
rpm -ivh --nodeps http://vault.centos.org/7.6.1810/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
rpm -ivh --nodeps http://vault.centos.org/7.6.1810/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-50.el7.noarch.rpm
```

同上，也可以离线安装

```sh
rpm -ivh --nodeps yum-3.4.3-161.el7.centos.noarch.rpm
```

#### 5.验证

```sh
python # 输出下面参数，ctrl + D退出
Python 2.7.5 (default, Oct 30 2018, 23:45:53) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-36)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>>

yum # 输出下面参数
Loaded plugins: fastestmirror
You need to give some command
Usage: yum [options] COMMAND

List of Commands:
...
```

