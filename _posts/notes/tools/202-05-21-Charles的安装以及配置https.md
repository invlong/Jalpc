---
layout: post  
title: "Charles的安装以及配置https"  
date: 2020-05-21 14:27:50 +0800  
categories: 技术文章  
tag: "工具"  
---

* content
{:toc}  


## 一.下载

直接去官网下载即可：

[官网地址](https://www.charlesproxy.com/download/)

注：没有历史版本的下载，只能下载最新版本，或者下载3的一个发布版，目前最新的是4.5.6

## 二.破解

### 1.直接破解码

Registered Name: `https://zhile.io`

License Key: `48891cf209c6d32bf4`

### 2.在线破解

[破解地址](https://www.zzzmode.com/mytools/charles/)

在线破解我试过最新版本和另外几个版本，都没有成功，不知道是不是和我是Ubuntu系统有关系？

## 三. 配置https

### 1.配置chrome抓包

> 本来chrome是有控制台的，用不到，但是我要做自动化测试脚本，需要用到抓包生成脚本。

#### ①配置浏览器代理

**可以直接配置浏览器代理**（网络方法，没有具体尝试）

- 访问: chrome://settings/

- 然后下拉到最后的高级，下来在“系统”（倒数第二个）的条目下找到“打开代理设置”，然后双击打开之后，打开之后找到代理的tab点开，点开之后可以看到请选择一个协议进行配置，这个时候找到“网页代理(http)”和“安全网页代理(https)”，进行相应的配置就可以了，一般来说自己不做其他处理，直接配置代理服务器为“127.0.0.1”，端口(就是冒号:)后是“8888”。

- 如何抓https网站, 在charles左侧该网址右键 Enable SSL Proxying

**配置SwitchyOmega代理**（本人使用的方式）

![代理配置](/styles/images/teach/TIM_20200521170230.png)

#### ②从chareles中导出证书，导入到chrome中

`Help | SSL Prosying | Save Charles Root Certificate`

文件名为`charles-ssl-proxying-certificate.pem`

然后将这个文件导入到chrome浏览器中

打开`chrome://settings`

`隐私和安全 | 更多 | 管理证书 | 授权中心 | 导入`

![](/styles/images/teach/2020-05-21 17-17-18 的屏幕截图.png)

#### ③配置charles开启抓包

`Proxy | Proxy Settins`

![](/styles/images/teach/2020-05-21 17-20-33 的屏幕截图.png)

`Proxy | SSL Proxy Settings`

这里是配置的抓取所有的https请求，根据需求变更`*.*.*`

![](/styles/images/teach/2020-05-21 17-22-26 的屏幕截图.png)

