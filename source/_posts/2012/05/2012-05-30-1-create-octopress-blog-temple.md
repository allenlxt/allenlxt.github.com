---
layout: post
title: "创建Octopress博客模板"
date: 2012-05-30 15:11
comments: true
categories: octopress
---

开始此节内容之前，参考以下内容：

* [gitweb git使用](http://help.github.com/linux-set-up-git/)
* [gitweb page](http://help.github.com/pages/)
* [Octopress doc](http://octopress.org)
* [配置Octopress环境](/blog/2012/05/29/config-octopress-env)

### 1. 创建个人主页 ###
`github`提供一个二级域名给所有用户，地址为`http://username.github.com`,注册完用户后，创建一个
`username.github.com`的git仓库，在`master`分支下随意写一个`index.html`，上传后约十分钟可以通过二级
域名浏览刚才的`index.html`内容。

### 2. 使用Octopress上传模板 ###
现从[Octopress](https://github.com/imathis/octopress)克隆git版本库，进入该文件夹，查看分支：   
	git branch -a
	* master
	  remotes/octopress/2.1
	  remotes/octopress/HEAD -> octopress/master
	  remotes/octopress/gh-pages
	  remotes/octopress/linklog
	  remotes/octopress/master
	  remotes/octopress/refactor_with_tests
	  remotes/octopress/rubygemcli
	  remotes/octopress/site
	  remotes/origin/source

在`master`分支，执行如下命令：   
	rake setup_github_pages
意义如下：   
1. Ask you for your Github Pages repository url.
2. Rename the remote pointing to imathis/octopress from ‘origin’ to ‘octopress’
3. Add your Github Pages repository as the default origin remote.
4. Switch the active branch from master to source.
5. Configure your blog’s url according to your repository.
6. Setup a master branch in the _deploy directory for deployment.

安装、生成、部署：   
	rake install
	rake generate
	rake deploy

上传源码：   
	git add .
	git commit -m 'your message'
	git push origin source

octopress的工作到此已经完成。

### 3. 使用Octopress生成的模板 ###
先checkout版本，如下：   
	git clone git@github.com:meegoo-tsui/meegoo-tsui.github.com.git
	cd meegoo-tsui.github.com
	rake setup_github_pages
	git branch -a
	  master
	* source
	  remotes/origin/HEAD -> origin/master
	  remotes/origin/master
	  remotes/origin/source
	git checkout source
	rake generate
	rake preview
	rake deploy

`master`分支为主页显示所需要的内容，因此需要在github上配置当前分支为`master`；    
`source`分支为源代码，在此分支上写代码然后使用`rake deploy`就可以修改`master`的内容，也就
更新了主页。

<hr />
