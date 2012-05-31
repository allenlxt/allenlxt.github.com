---
layout: post
title: "ubuntu更新问题解决"
date: 2012-05-21 13:11
comments: true
categories: ubuntu
---

ubuntu server 10.04 和 ubuntu desktop 10.04源设置及更新错误解决。

## 修改 sources.list ##
	$ sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
下载[sources.list](http://mirrors.163.com/.help/sources.list.lucid)，
保存于/etc/apt/sources.list，运行命令更新：    
	$ sudo apt-get update
	$ sudo spt-get upgrade

## 修改更新错误 ##
### error 1 ###
E: Could not get lock /var/lib/apt/lists/lock - open (11: Resource temporarily unavailable)     
E: Unable to lock the list directory    
解决方法：     
	$ ps -e | grep apt
删除相关进程：    
	$ sudo kill xxxx

<hr />
