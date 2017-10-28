---
layout: post
title:  "ss-clinet-server服务搭建"
date:   2017-10-28
desc: "ss-clinet-server服务搭建"
keywords: "Ubuntn,ssr,clinet,server"
categories: [Linux]
tags: [Ubuntn]
icon: icon-html
---



>受19大的垂爱，我花重金买的expressVPN被墙的厉害，所以我开始寻找方案...  

github上有一个翻墙Repository  
[repository](https://github.com/Alvin9999/new-pac)  
注：下面有些网址是需要翻墙的，所以要搭建的前提是，你在墙外。  

__我考虑的是自建ssr服务器方案__  
使用的ssr服务商是Google Cloud Platform  
免费一年，但是要有visa信用卡。 
服务端  
-- 
__知识点__  
1.注册时我写的是中国地址（有人说需要写美国地址)。  
2.产品与服务->计算引擎->VM实例->创建实例来初始化一个服务。  
3.选择最便宜的方案(微型方案)-5美刀。  
4.我选择的是centos7系统。  
```shell
// 下载脚本(https://teddysun.com/486.html)
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
// 赋予可执行权限
chmod +x shadowsocks-all.sh  
// 执行安装
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
// 成功的提示
Congratulations, ShadowsocksR server install completed!
Your Server IP        :  1.1.1.1 
Your Server Port      :  223 
Your Password         :  **** 
Your Protocol         :  origin 
Your obfs             :  plain 
Your Encryption Method:  aes-256-cfb 
Welcome to visit: https://teddysun.com/486.html
Enjoy it!
```
5.我安装的是ShadowsocksR版本。  
6.在Google Cloud Platform中需要有两点设置  
防火墙  
产品与服务->网络->防火墙规则->创建防火墙规则
来源过滤要选允许任意来源的流量。
允许协议和端口填tcp:1-223; udp:1-223  
静态IP  
同样是在网络里->外部IP地址->保留静态IP  
7.ssr的启动命令
```shell
sudo /etc/init.d/shadowsocks-r start
```
8.加入自动重启shadowsocks  
```shell
sudo vim /etc/rc.local
// 内容
sleep 10
/usr/bin/ssserver /etc/init.d/shadowsocks-r start
exit 0
```

[博客1](https://www.mianao.info/2017/03/16/google-cloud-platform-%E5%85%8D%E8%B4%B9%E6%90%ADshadowsocks%E4%B8%80%E5%B9%B4/)  
[博客2](http://godjose.com/2017/06/14/new-article/)  

-----------------------
客户端  
--
开始我搞混了客户端和服务端。  
在git中Shadowsocks指的是包含客户端和服务端的，看Readme时一定看清哪些代码是客户端的。。  
不过我用的Ubuntu系统，而git中编译ssr客户端问题一堆堆的，浪费了我好多时间，最后也没能编译通过。  
git中的很多仓库迫于国内政府压力(?)下架了很多客户端。  
__最后发现其实有官网的!!!!!__  
[官方地址](https://shadowsocks.org/en/download/clients.html)  
选择Linux版本，好简单啊！  

-------
还要配置自动代理，使用大神给的地址：  
https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt  
[github地址](https://github.com/gfwlist/gfwlist)  

__chrome 插件__  
Proxy SwitchyOmega  
这里就不做详细介绍了  

后来发现这种方式不是很好：  
因为Ubuntu的ss-q5服务启动了以后，实际上所有的网络都无法翻墙的，要么想上面提到的使用SwitchyOmega代理后，浏览器可以翻墙了，但是别的应用没有翻出去。  
网上较复杂的教程为使用iptables配合一些代理，而简单的方式是上文提到的gfwlist.txt  
```shell
#首先安装pip
sudo apt-get install python-pip
#通过pip安装genpac
sudo pip install genpac
```
```shell
genpac -p "SOCKS5 127.0.0.1:1080" --gfwlist-proxy="SOCKS5 127.0.0.1:1080" --gfwlist-url=https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt --output="autoproxy.pac"
```  
此时会生成一个名为autoproxy.pac的文件  
_更改系统代理设置_  
进入代理设置 System settings > Network > Network Proxy  
设置Method为Automatic  
设置Configuration URL为autoproxy.pac文件的路径  
```html
#例如 /home/lckiss/autoproxy.pac
```
__特别注意，客户端有一个配置，要不死活不生效，我找了半天才发现。。。。__  
Local Server Type  一定要选择SOCKs5!!!  
  