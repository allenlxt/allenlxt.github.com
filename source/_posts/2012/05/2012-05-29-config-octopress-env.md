---
layout: post
title: "配置Octopress环境"
date: 2012-05-29 13:11
comments: true
categories: octopress rvm
---

[Octopress](http://octopress.org)的官方网站上有详细文档，可先参考此网站上的相关文档。

## ubuntu10.04安装rvm ##

### 1. 安装ruby依靠包、curl、ruby ###
	sudo apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion
	sudo apt-get install ruby

### 2. 通过下面指令安装rvm ###
	bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer )

### 3. 环境变量的配置 ###
替换.bashrc：    
	[ -z "$PS1" ] && return
为   
	if [[ -n "$PS1" ]]; then
文件末尾添加内容：    
	if [[ -s $HOME/.rvm/scripts/rvm ]] ; then source $HOME/.rvm/scripts/rvm ; fi
	fi

### 4. 查询安装结果 ###
重新进入终端或使用`. ~/.bashrc `使刚才设置的环境变量生效，然后运行如下命令：    
	rvm list known
	# MRI Rubies
	[ruby-]1.8.6[-p420]
	[ruby-]1.8.7[-p358]
	[ruby-]1.8.7-head
	[ruby-]1.9.1[-p431]
	[ruby-]1.9.2-p180
	[ruby-]1.9.2-p290
	[ruby-]1.9.2-p318
	[ruby-]1.9.2[-p320]
	[ruby-]1.9.2-head
	[ruby-]1.9.3-preview1
	[ruby-]1.9.3-rc1
	[ruby-]1.9.3-p0
	[ruby-]1.9.3-p125
	[ruby-]1.9.3-[p194]
	[ruby-]1.9.3-head
	ruby-head
如上所示，显示出可安装的ruby版本，至此rvm的安装完成。

## ubuntu10.04安装ruby及依靠包 ##

### 1. 安装ruby ###
`Octopress`当前使用的`ruby`版本为`1.9.2`，因此使用如下命令安装`ruby`：    
	rvm install 1.9.2 && rvm use 1.9.2
需要重装时：   
	rvm install 1.9.2 && rvm use 1.9.2

### 2. 克隆`Octopress`的版本库 ###
`Octopress`的版本库托管在`github`，使用如下命令`checkout`：    
	git clone git://github.com/imathis/octopress.git octopress
	cd octopress    # 当使用rvm时，提示是否信任.rvmrc文件，输入yes
	ruby --version  # 显示 Ruby 1.9.2

### 3. 安装依靠包 ###
以下命令需要在`octopress`路径下执行，版本的锁定需要此路径下的`Gemfile`文件：   
	gem install bundler
	bundle install

到此已完成`Octopress`环境的配置，可以使用`Octopress`的相关命令：    
	rake install
	rake generate
	rake preview

<hr />
