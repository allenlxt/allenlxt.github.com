---
layout: post
title: "linux常用服务器的配置"
date: 2012-05-16 14:11
comments: true
categories: linux
---

安装平台： ubuntu 10.04    
	ssh   - 远程登录    
	samba - 文件共享    
	nfs   - 网络文件系统    
	tftp  - 简单文件传输协议    
	ftp   - 文件传输协议    

## ssh ##
### 1. 安装软件包 ####
	sudo apt-get install openssh-server
### 2. 重启ssh服务 ###
	sudo /etc/init.d/ssh restart
### 3. 查看 ###
	netstat -tlp | grep ssh

## samba ##
### 1. 安装软件包 ####
	sudo apt-get install samba samba-common samba-common-bin system-config-samba
### 2. 运行 ###
	sudo smbd start
### 3. 图形界面配置 ###
	sudo system-config-samba
### 4. 命令行配置 ###
修改配置文件：    
	sudo gedit /etc/samba/smb.conf
修改内容如下：   
	####### Authentication #######
	
	# “security = user” is always a good idea. This will require a Unix account
	# in this server for every user accessing the server. See
	# /usr/share/doc/samba-doc/htmldocs/Samba-HOWTO-Collection/ServerType.html
	# in the samba-doc package for details.
	;  security = user
修改为：    
	security = user
	username map = /etc/samba/smbusers
添加samba用户：    
	sudo smbpasswd -a meegoo
添加到samba用户文件，格式为<ubuntuusername> = “<samba username>”    
	sudo gedit /etc/samba/smbusers

### 5. 添加samba共享路径 ###
修改配置文件：    
	sudo gedit /etc/samba/smb.conf
文件尾添加内容如下：    
	[meegoo]
		path = /home/meegoo
		writeable = yes
	;	browseable = yes
		valid users = meegoo
开始或重启samba服务：    
	sudo smbd start
	sudo smbd restart
测试samba服务器：    
	sudo aptitude install smbfs
	sudo mkdir /mnt/samba
	sudo mount -t smbfs -o username=meegoo //192.168.0.238/meegoo /mnt/samba

## nfs ##
### 1. 安装软件包 ####
	sudo apt-get install nfs-kernel-server
### 2. 修改配置文件 ###
	sudo vim /etc/exports
添加内容如下：    
	/home/ming/work/.out/filesys *(rw,sync,no_root_squash,no_subtree_check)
配置文件生效：   
	sudo /usr/sbin/exportfs -va

## tftp ##
### 1. 安装软件包 ####
	sudo apt-get install tftpd tftp xinetd
### 2. 修改配置文件 ###
	sudo vim /etc/xinetd.d/tftp
添加内容如下：    
	service tftp
	{
		socket_type = dgram
		protocol = udp
		wait = yes
		user = root
		server = /usr/sbin/in.tftpd
		server_args = -s /tftpboot
		disable = no
		per_source = 11
		cps = 100 2
		flags = IPv4
	} 
配置文件生效：   
	sudo chmod ugo+rwx /tftpboot
	sudo /etc/init.d/xinetd restart 

## ftp ##
### 1. 安装软件包 ####
	sudo apt-get install vsftpd
### 2. 创建ftp文件夹 ###
	sudo mkdir /home/ftp
	sudo usermod -d /home/ftp ftp
### 3. ftp操作 ###
	sudo /etc/init.d/vsftpd start
	sudo /etc/init.d/vsftpd restart
	sudo /etc/init.d/vsftpd stop
### 4. 修改配置文件 ###
	sudo gedit /etc/vsftpd.conf
内容如下：    
	local_umask=022
	anon_umask=000
	anonymous_enable=YES
	write_enable=YES
	anon_world_readable_only=NO
	anon_other_write_enable=YES
	anon_upload_enable=YES
	anon_mkdir_write_enable=YES

<hr />
