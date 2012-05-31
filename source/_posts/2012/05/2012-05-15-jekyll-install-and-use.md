---
layout: post
title: "jekyll的安装使用"
date: 2012-05-15 14:11
comments: true
categories: jekyll
---

<!---
################################################################################
-->
*    [Windows 环境](#windows)
*    [Linux 环境](#linux)

<!---
################################################################################
-->
<h2 id="windows">jekyll的安装 - Windows 环境</h2>
### 1. 安装RailsInstaller ###
下载[RailsInstaller](http://railsinstaller.org/)，
然后安装，里面包含了Ruby、Rails、Bundler、Git、Sqlite、TinyTDS、SQL Server support和DevKit。

### 2. 安装jekyll和相关的包 ###
把淘宝的镜像加到gem的镜像列表里
	$ gem sources --remove http://rubygems.org/
	$ gem sources -a http://ruby.taobao.org/
查看：
	$ gem sources -l
安装jekyll及相关包：
	$ gem install jekyll
	$ gem install rdiscount kramdown

### 3. 修改jekyll运行语言错误 ###
打开C:\RailsInstaller\Ruby1.9.3\lib\ruby\gems\1.9.1\gems\jekyll-0.11.2\lib\jekyll\tags\include.rb，
将source = File.read(@file)修改为：
	source = File.read(@file, :encoding => "utf-8")

打开C:\RailsInstaller\Ruby1.9.3\lib\ruby\gems\1.9.1\gems\jekyll-0.11.2\lib\jekyll\convertible.rb，
将self.content = File.read(File.join(base, name))修改为：
	self.content = File.read(File.join(base, name), :encoding => "utf-8")

### 4. 语法的高亮显示 ###
a. 下载[wget.exe](http://users.ugent.be/~bpuype/wget/)，copy到C:\windows；    

b. 下载且安装python；    
windows 64位：    
{% highlight bash %}
$ cd /f
# setuptools does not work with Python 3.2
$ wget -nc http://python.org/ftp/python/2.7.3/python-2.7.3.amd64.msi
$ msiexec -qn -i python-2.7.3.amd64.msi ADDLOCAL="DefaultFeature"
{% endhighlight %}
windows 32位：    
{% highlight bash %}
$ cd /f
# setuptools does not work with Python 3.2
$ wget -nc http://python.org/ftp/python/2.7.3/python-2.7.3.msi
$ msiexec -qn -i python-2.7.3.msi ADDLOCAL="DefaultFeature"
{% endhighlight %}

c. 添加路径到PATH，文件D:\Program Files (x86)\Git\etc\profile；    
{% highlight bash %}
# add python path
export PATH="$HOME/bin:/C/Python27:/C/Python27/Scripts:$PATH"
{% endhighlight %}

d. 重新启动git bash，安装Pygments；    
{% highlight bash linenos%}
# Install setuptools 
$ wget -nc peak.telecommunity.com/dist/ez_setup.py
$ ./ez_setup.py
# Pygments
$ easy_install Pygments
{% endhighlight %}

e. 修正bug：D:\RailsInstaller\Ruby1.9.3\lib\ruby\gems\1.9.1\gems\albino-1.3.3\lib\albino.rb；    
{% include_code lang:diff albino.rb.diff %}

<!---
################################################################################
-->
<hr />
<h2 id="linux">jekyll的安装 - linux 环境</h2>
### 1. Install ruby ###
{% highlight bash %}
$ sudo apt-get install ruby1.9.1 ruby1.9.1-dev python-pygments
$ sudo ln -s /usr/bin/ruby1.9.1 /usr/bin/ruby
{% endhighlight %}

### 2. Download and install rubygems ###
{% highlight bash %}
$ wget http://files.rubyforge.vm.bytemark.co.uk/rubygems/rubygems-1.8.24.tgz
$ tar -zxf rubygems-1.8.24.tgz
$ cd rubygems-1.8.24/
$ sudo ruby setup.rb
$ sudo ln -s /usr/bin/gem1.9.1 /usr/bin/gem
{% endhighlight %}

### 3. Install jekyll via gem ###
{% highlight bash %}
$ sudo gem install jekyll --no-rdoc --no-ri
$ sudo gem install rdiscount --no-rdoc --no-ri
$ sudo gem install RedCloth --no-rdoc --no-ri
{% endhighlight %}

### 4. Run jekyll at a jekyll project path ###
{% highlight bash %}
$ jekyll --server
{% endhighlight %}

View http://127.0.0.1:4000/

## 其他 ##
[Liquid-Extensions](https://github.com/mojombo/jekyll/wiki/Liquid-Extensions/)    
[jekyll-Install](https://github.com/mojombo/jekyll/wiki/Install)    

<hr />
