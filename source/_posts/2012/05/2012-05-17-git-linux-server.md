---
layout: post
title: "linux git服务器的配置"
date: 2012-05-17 13:11
comments: true
categories: linux git
---

安装平台： ubuntu 10.04    
搭建git服务器，需要如下软件：    
	$ sudo apt-get install openssh-server openssh-client
	$ sudo apt-get install git-core
	$ sudo apt-get install python-setuptools

## 源码安装gitosis ##
使用git下载源码：    
	$ git clone https://github.com/ossxp-com/gitosis.git
	$ cd gitosis
	$ sudo python setup.py install

## 账户相关信息 ##
创建用户git    
	$ sudo adduser --system --shell /bin/bash --disabled-password --group git
创建git仓库存储目录    
	$ sudo mkdir /home/git/repositories
设置git仓库权限    
	$ sudo chown git:git /home/git/repositories
	$ sudo chmod 755 /home/git/repositories
初始化全局设置    
	$ git config --global usre.name     "meegoo tsui"
	$ git config --global usre.email    meegoo.tsui@gmail.com
	$ git config --global core.fileMode false
	$ git config --global core.editor   vim
* 配置用户名称
* 配置用户邮件
* 配置忽略文件的执行权限
* 配置编辑器

## 初使化gitosis ##
生成 SSH key：    
	$ ssh-keygen -t rsa -C "meegoo.tsui@gmail.com"
拷贝到服务器/tmp下：   
	$ cp  ~/.ssh/id_rsa.pub /tmp
或：    
	$ scp ~/.ssh/id_rsa.pub 用户名@主机:/tmp
初使化gitosis，切回到服务器：   
	$ sudo -H -u git gitosis-init < /tmp/id_rsa.pub
修改post-update权限:    
	$ sudo chmod 755 /home/git/repositories/gitosis-admin.git/hooks/post-update

clone gitosis管理平台，切换到本地：    
	$ git clone git@192.168.0.2:gitosis-admin.git
或
	$ git clone git@lenovo:gitosis-admin.git
用户key在gitosis-admin/keydir/下，权限文件在gitosis-admin/gitosis.conf,添加*.pub且修改gitosis.conf，
上传即可。

## 创建版本库 ##
客服端修改权限文件gitosis-admin.git/gitosis.conf，关键字admin允许创建，添加内容如下：    
	[group my_project-admin]
	members = meegoo.tsui@gmail.com
	admin = my_project
创建版本项目：    
	$ mkdir my_project;cd my_project
	$ git init
	$ git commit --allow-empty -m "create the repos."
	$ git remote add origin git@192.168.0.2:my_project.git
	$ git push -u origin master

## gitweb安装及配置 ##
安装gitweb：     
	$ sudo apt-get install gitweb
配置apache服务器:
	$ sudo gedit /etc/apache2/mods-available/dav_svn.conf
添加内容如下：      
	Alias /gitweb /usr/share/gitweb
	<Directory /usr/share/gitweb>
		Options FollowSymLinks +ExecCGI
		AddHandler cgi-script .cgi
		DirectoryIndex index.cgi gitweb.cgi
		order Allow,Deny
		Allow from all
	</Directory>
或：    
	<VirtualHost *:80>
		ServerName localhost
		ScriptAlias /cgi-bin/ /usr/lib/cgi-bin/
		<Directory /usr/lib/cgi-bin/>
			Options ExecCGI +FollowSymLinks +SymLinksIfOwnerMatch
			AllowOverride All
			order allow,deny
			Allow from all
			AddHandler cgi-script cgi .pl .py
			DirectoryIndex gitweb.cgi
		</Directory>
	</VirtualHost>

修改gitweb配置文件：   
	$ sudo vim /etc/gitweb.conf
	$ sudo /etc/init.d/apache2 restart
	# 修改内容如下：
	$projectroot = "/home/git";
	$home_text = "/home/git/indextext.html";
客服端修改权限文件gitosis-admin.git/gitosis.conf：      
	[repo gitosis-admin.git]
	gitweb      = yes
	description = git server admin
服务器修改项目的配置文件，提供显示信息给gitweb，gitosis-admin.git/config：    
	[gitweb]
		owner = meegoo tsui <meegoo.tsui@gmail.com>
		url   = git@192.168.0.2:gitosis-admin.git
注：其他项目一样修改服务器xxx.git/config文件。   
访问： http://192.168.0.2/gitweb

<hr />
