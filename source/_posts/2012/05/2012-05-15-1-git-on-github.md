---
layout: post
title: "在github上使用git"
date: 2012-05-15 13:11
comments: true
categories: github git
---

## git工具的安装 - Windows 7 ##
### 1. 安装命令行工具 msysgit ###
msysgit为google上的托管项目，可以从[google](http://code.google.com/p/msysgit/downloads/list)下载安装包，
选择最新版本(Git-1.7.8-preview20111206.exe)进行安装，安装时无特别要求，选择默认选项，一直“next”完成安装。

### 2. 安装图形工具 - tortoise git ###
tortoisegit为google上的托管项目，可以从[google](http://code.google.com/p/tortoisegit/downloads/list)下载安装包，
选择最新版本(TortoiseGit-1.7.8.0-32bit.msi或TortoiseGit-1.7.8.0-64bit.msi)进行安装，安装时无特别要求，
选择默认选项，一直“next”完成安装。

### 3. git配置 ###
配置git账户信息，包括用户名和邮件地址等，一般以命令行形式配置，使用git config配置时:

* 无参数   - 参数保存在.git/config
* --global - 参数保存在/home/user_name/.gitconfig
* --system - 参数保存在/etc/gitconfig

执行配置脚本：
{% highlight bash %}
#!/bin/sh

# name and email
git config --global user.name "meegoo tsui"
git config --global user.email "meegoo.tsui@gmail.com"

# chinese font
git config --global gui.encoding utf-8
git config --global i18n.commitencoding utf-8
git config --global i18n.logoutputencoding gbk

# fie mode and edit
git config --global core.fileMode false
git config --global core.editor vim

# diff
git config --global diff.external git-diff-wrapper.sh
git config --global diff.tool tortoise
git config --global difftool.tortoise.cmd 'TortoiseMerge -base:"$LOCAL" -theirs:"$REMOTE"'
git config --global difftool.prompt false

# merge
git config --global merge.tool tortoise
git config --global mergetool.tortoise.cmd 'TortoiseMerge -base:"$BASE" -theirs:"$REMOTE" -mine:"$LOCAL" -merged:"$MERGED"'
git config --global mergetool.prompt false
{% endhighlight %}

### 4. 中文输入支持 ###
修改D:\Program Files (x86)\Git\etc\profile,添加内容如下：
	# ls 显示中文
	alias ls='ls --show-control-chars --color=auto'
	
	# more 显示中文
	export LESSCHARSET=utf-8
中文输入支持，修改文件D:\Program Files (x86)\Git\etc\inputrc：
	set output-meta on
	set convert-meta off
使用SVN的diff、merge工具TortoiseMerge.exe、git-diff-wrapper.sh，拷贝到D:\Program Files (x86)\Git\bin，git-diff-wrapper.sh
内容如下：
{% highlight bash %}
#!/bin/sh

# diff is called by git with 7 parameters:
# path old-file old-hex old-mode new-file new-hex new-mode

TortoiseMerge -base:"$2" -theirs:"$5"
{% endhighlight %}

## git工具的安装 - Ubuntu 10.04 ##
### 1. 安装 openssh ###
如果不安装openssh，在导入ssh id_rsa私匙时，认证老是不成功，安装命令如下：
	$ sudo apt-get install openssh-serve
	$ sudo /etc/init.d/ssh restart

### 2. 安装 git ###
安装命令如下：
	$ sudo apt-get install git-core

### 3. 安装图形工具 rabbitvcs-nautilus ###
与Windows下的tortoisegit的差不多，安装命令如下：
	$ sudo add-apt-repository ppa:rabbitvcs/ppa
	$ sudo apt-get update
	$ sudo apt-get install rabbitvcs-nautilus

## SSH key生成和使用 ##
### 1. 生成 SSH key ###
私匙id_rsa和公匙id_rsa.pub配对生成，配对使用，生成时可设置密码，命令如下：
	$ ssh-keygen -t rsa -C "meegoo.tsui@gmail.com"
上传公匙id_rsa.pub到github.com的个人repos，测试命令如下：
	$ ssh -T git@github.com

### 2. 导入 SSH key ###
复制私匙id_rsa和公匙id_rsa.pub到/home/user_name/.ssh，然后修改文件属性：
	$ chmod 600 .ssh/id_rsa .ssh/id_rsa.pub

### 3. Windows生成*.ppk格式的SSH key ###
下载[puttygen.exe](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)，然后导入
id_rsa文件，Conversions，输入密码，即可生成ppk(putty pravite key)文件。

## 创建repos ##
在github创建repos的本地操作，基本命令如下：
### Create A Repos ###
	$ git init
	$ git add README
	$ git commit -m 'first commit'
	$ git remote add origin git@github.com:meegootsui/meegootsui.github.com.git
	$ git push -u origin master

### Checkout Repos ###
	$ git clone git@github.com:meegootsui/meegootsui.github.com.git

### Commit ###
	$ git commit -m ""
	$ git push -u origin master

<hr />
