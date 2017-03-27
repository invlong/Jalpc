---
layout: post
title:  "ubuntu-ssh-entos服务器保存密码"
date:   2017-03-27
desc: "ssh通过证书登录"
keywords: "Ubuntu,ssh,rsa"
categories: [Linux]
tags: [Jekyll]
icon: icon-html
---



>今天，我开始配置免密码ssh登录远程服务器，虽然网上的教程很详细，但是我也遇到了一些独立的问题，以做记录。  
####1. 客户端    
1.  通过命令生成公私密钥对  

		ssh-keygen      
一路敲回车下去，最后在~/.ssh文件夹下生成id_rsa和id_rsa.pub两个文件。  

2. 改一下.ssh目录的权限  


		chmod 755 ~/.ssh
	
3. 在~/.ssh文件夹下创建config文件  
具体如下  

		Host abc //服务器别名 
		HostName xxx.xxx.xxx.xxx //服务器的ip地址 
		User root   
		Port 22  //ssh服务端口   
_注：如果需要对远程多台机子配置，则config文本文件里面再添加一条记录，格式
和上面的一样。_  

4. 用 ssh-copy-id 把公钥复制到远程主机上  

		ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.0.3 -p 22622
_如果有修改过端口号的话，需要指定-p参数来指定端口号。_  

####2. 远程服务器
1. 用vi打开/etc/ssh/sshd_config这个文件   
将下面几行前面“#”注释取掉   

		RSAAuthentication yes 
		PubkeyAuthentication yes 
		AuthorizedKeysFile .ssh/authorized_keys 
		
2. 在用户根目录下创建.ssh文件夹，如果已经有了就不用创建了.具体路径为(~/.ssh)   
在.ssh文件夹下建立authorized_keys文件，记住authorized_keys是文件，不是文件夹。  
将客户端中的`id_rsa.pub`的内容拷贝到`authorized_keys`中。
3. 赋予权限  

		chmod 600 authorized_keys
		
4. 重启ssh服务。  

		service sshd restar
		
####3.回到本机进行测试

1. _要重新打开一个terminal_  

		ssh abc //注abc 是config文件中配置的服务器别名






