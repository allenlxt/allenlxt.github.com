---
layout: post
title: "ubuntu配置svn服务器"
date: 2012-05-15 16:11
comments: true
categories: svn
---

安装环境：ubuntu 10.04。

## 一. 安装 ##
安装apache2和subversion相关安装包。    
	sudo apt-get install apache2 subversion libapache2-svn python-subversion sendmail    
添加组subversion及用户模式设置。
	sudo addgroup subversion;sudo usermod -G subversion -a www-data

## 二. 创建数据仓库 ##
创建数据仓库及模式设置。     
方法一（无邮件通知功能）：     
	sudo svnadmin create /opt/svn_repos/my_project
	sudo chown -R root:subversion /opt/svn_repos/my_project;sudo chmod -R g+rws

方法二（有邮件通知功能）：     
下载文件<a href='/tar/2012/05/makesvn.tar.bz2' id='blog-link' title='Tar'>makesvn工具</a>,然后解压，
进入文件夹，运行命令如下：
    sudo mkdir /opt/svn_repos;./makesvn.sh /opt/svn_repos/ my_project

## 三. 配置数据仓库 <a href='/tar/2012/05/conf.tar.bz2' id='blog-link' title='Tar'>conf</a>##
### 1. 创建配置文件夹 ###
    cd /opt/svn_repos/;sudo mkdir conf

### 2. 添加文件D:\svn_repos\conf\access.auth，添加内容如下：###
	[my_project:/]
	meegoo=rw
	
含义：用户meegoo对my_project有读写权限。

### 3. apache账户管理 ###
初次建立密码文件，前者为用户名称，后者为密码：    
	sudo htpasswd -c /opt/svn_repos/conf/dav_svn.passwd meegoo
添加其他用户，去掉-c参数：    
	sudo htpasswd    /opt/svn_repos/conf/dav_svn.passwd ming

## 四. 配置apache服务器 ##
### 1. 修改apache配置文件 ###
	sudo vim /etc/apache2/mods-available/dav_svn.conf
修改内容如下：     
	<Location /svn>
  	DAV svn
  	SVNParentPath /opt/svn_repos
	
	AuthType Basic
	AuthName "Subversion Repository"
	AuthUserFile /opt/svn_repos/conf/dav_svn.passwd
	AuthzSVNAccessFile /opt/svn_repos/conf/access.auth

	#<LimitExcept GET PROPFIND OPTIONS REPORT>
	Require valid-user
	#</LimitExcept> 
	
	</Location>

### 2. 测试svn服务器 ###
重启服务器，使配置文件生效：    
	sudo /etc/init.d/apache2 restart
网页访问http://your_ip/svn/my_project，输入密码、访问。

## 五. svn命令 ##
以二进制文件上传：    
	svn propset svn:mime-type application/octet-stream filename
去除文件的执行属性：     
	svn propdel svn:executable filename.ext

<hr />
