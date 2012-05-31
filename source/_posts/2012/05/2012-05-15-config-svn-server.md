---
layout: post
title: "svn服务器配置"
date: 2012-05-15 15:11
comments: true
categories: svn
---

<!---
################################################################################
-->
*    [Windows 环境](#windows)
*    [Linux 环境](#linux)

<!---
################################################################################
-->
<h2 id="windows">Windows 环境</h2>
### 一.安装 ###
### 1. 安装apache服务器 ###
下载[httpd-2.2.22-win32-x86-openssl-0.9.8t.msi](http://httpd.apache.org/download.cgi#apache24)，apache占用80端口，
安装完成后，在网页浏览器输入``http://your_ip``,会显示其已经工作，如果80端口端口占用，参考下面内容修改端口；
### 2. 安装svn服务器 ###
下载[Setup-Subversion-1.7.4.msi](http://sourceforge.net/projects/win32svn/files/1.7.4/)，默认安装即可；
### 3. 安装svn客户端 ###
下载[TortoiseSVN-1.7.x.xxxxx-win32-svn-1.7.x.msi](http://tortoisesvn.net/downloads.html)，默认安装即可；

### 二.创建数据仓库 ###
### 1. 确定数据仓库的路径，假设安装在``d:\svn_repos``，dos命令如下: ###

	d:
	mkdir d:\svn_repos

### 2. 创建一个新仓库，dos命令如下： ###

	svnadmin create d:\svn_repos\my_project

### 三.配置数据仓库 ###
### 1. 创建配置文件夹 ###

	mkdir d:\svn_repos\conf

### 2. 添加文件D:\svn_repos\conf\access.auth，添加内容如下：###
	[my_project:/]
	meegoo=rw
	
含义：用户meegoo对my_project有读写权限。

### 3. apache账户管理 ###
初次建立密码文件，前者为用户名称，后者为密码：

	c:
	cd "c:\Program Files\Apache Software Foundation\Apache2.2\bin"
	htpasswd -cb users.auth meegoo 123456

添加其他用户，去掉-c参数：

	htpasswd -b users.auth ming 123456

把users.auth复制到D:\svn_repos\conf\，或复制内容到D:\svn_repos\conf\users.auth。

### 四.配置apache服务器 ###
###  1. 拷贝文件 ###

	cd   "C:\Program Files\Apache Software Foundation\Apache2.2\modules"
	copy "C:\Program Files\Subversion\bin\mod_authz_svn.so"
	copy "C:\Program Files\Subversion\bin\mod_dav_svn.so"

### 2. 修改apache配置文件 ###
修改文件`C:\Program Files\Apache Software Foundation\Apache2.2\conf\httpd.conf`:     
去除“LoadModule dav_module modules/mod_dav.so”前面的“#”；     
在此行后面加内容如下：     
	LoadModule dav_svn_module modules/mod_dav_svn.so    
	LoadModule authz_svn_module modules/mod_authz_svn.so   
添加以下内容到最后:    
	<Location /svn>
	
	DAV svn
	SVNParentPath D:/svn_repos
	
	AuthzSVNAccessFile D:/svn_repos/conf/access.auth
	Satisfy Any
	Require valid-user
	
	AuthType Basic
	AuthName "Subversion repositories"
	AuthUserFile  D:/svn_repos/conf/users.auth
	Require valid-user
	</Location>
修改端口，仅当80端口占用情况，修改“Listen 80”为“Listen 8080”

### 五.使用 ###
* 重启电脑或apache软件；
* 网页访问http://your_ip/svn/my_project或http://your_ip:8080/svn/my_project；
* TortoiseSVN右键checkout；
* 新增类似my_project的仓库，只需修改控件权限即可，文件：D:\svn_repos\conf\access.auth；

<!---
################################################################################
-->
<hr />
<h2 id="linux">linux 环境</h2>
### 一. 安装 ###
安装apache2和subversion相关安装包。    
	sudo apt-get install apache2 subversion libapache2-svn python-subversion sendmail    
添加组subversion及用户模式设置。
	sudo addgroup subversion;sudo usermod -G subversion -a www-data

### 二. 创建数据仓库 ###
创建数据仓库及模式设置。     
方法一（无邮件通知功能）：     
	sudo svnadmin create /opt/svn_repos/my_project
	sudo chown -R root:subversion /opt/svn_repos/my_project;sudo chmod -R g+rws

方法二（有邮件通知功能）：     
下载文件<a href='/downloads/tar/makesvn.tar.bz2' id='blog-link' title='Tar'>makesvn工具</a>,然后解压，
进入文件夹，运行命令如下：
    sudo mkdir /opt/svn_repos;./makesvn.sh /opt/svn_repos/ my_project

### 三. 配置数据仓库 <a href='/downloads/tar/conf.tar.bz2' id='blog-link' title='Tar'>conf</a>###
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

### 四. 配置apache服务器 ###
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

### 五. svn命令 ###
以二进制文件上传：    
	svn propset svn:mime-type application/octet-stream filename
去除文件的执行属性：     
	svn propdel svn:executable filename.ext

<hr />
