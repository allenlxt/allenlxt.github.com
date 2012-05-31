---
layout: post
title: "octopress使用技巧"
date: 2012-05-31 08:24
comments: true
categories: octopress
---

*   内容列表
    *   [1. 代码块](#codeblock)
    *   [2. 代码包含](#includecode)
    *   [3. 引用](#blockquote)
    *   [4. gist](#gist)
    *   [5. 图片](#img)
    *   [6. 视频](#video)
    *   [7. category](#category)

<h2 id="codeblock">1. 代码块</h2>
语法：    
	{{ "{% codeblock [title] [lang:language] [start:#] [url] [link text]" }} %}
	code snippet
	{{ "{% endcodeblock" }} %}

实例1 - 简单显示：   
	{{ "{% codeblock" }} %}
	需要显示的代码
	{{ "{% endcodeblock" }} %}
{% codeblock %}
需要显示的代码
{% endcodeblock %}

实例2 - 高亮显示：   
	{% raw %}{% codeblock lang:objc %}
	[rectangle setX: 10 y: 10 width: 20 height: 20];
	{% endcodeblock %}{% endraw %}

{% codeblock lang:objc %}
[rectangle setX: 10 y: 10 width: 20 height: 20];
{% endcodeblock %}

实例3 - 高亮显示（交互）：   
	{{ "{% codeblock Time to be Awesome - awesome.rb" }} %}
	puts "Awesome!" unless lame
	{{ "{% endcodeblock" }} %}

{% codeblock Time to be Awesome - awesome.rb %}
puts "Awesome!" unless lame
{% endcodeblock %}

实例4 - 高亮显示（强制）：     
	{{ "{% codeblock Here's an example .rvmrc file. lang:ruby" }} %}
	rvm ruby-1.8.6 # ZOMG, seriously? We still use this version?
	{{ "{% endcodeblock" }} %}

{% codeblock Here's an example .rvmrc file. lang:ruby %}
rvm ruby-1.8.6 # ZOMG, seriously? We still use this version?
{% endcodeblock %}

实例5 - 添加url：     
	{% raw %}{% codeblock Javascript Array Syntax lang:js http://j.mp/pPUUmW MDN Documentation %}
	var arr1 = new Array(arrayLength);
	var arr2 = new Array(element0, element1, ..., elementN);
	{% endcodeblock %}{% endraw %}

{% codeblock Javascript Array Syntax lang:js http://j.mp/pPUUmW MDN Documentation %}
var arr1 = new Array(arrayLength);
var arr2 = new Array(element0, element1, ..., elementN);
{% endcodeblock %}

实例6 - 添加行信息：     
    {% raw %}{% codeblock Coffeescript Tricks lang:coffeescript start:51 %}
    # Given an alphabet:
    alphabet = 'abcdefghijklmnopqrstuvwxyz'

    # Iterate over part of the alphabet:
    console.log letter for letter in alphabet[4..8]
    {% endcodeblock %}{% endraw %}

{% codeblock lang:coffeescript Coffeescript Tricks start:51 %}
# Given an alphabet:
alphabet = 'abcdefghijklmnopqrstuvwxyz'

# Iterate over part of the alphabet:
console.log letter for letter in alphabet[4..8]
{% endcodeblock %}

<h2 id="includecode">2. 代码包含</h2>
语法：   
	{{ "{% include_code [title] [lang:language] path/to/file [start:#] [end:#] [range:#,#]" }} %}

实例1 - 基本用法：     
文件路径`source/downloads/code/test.js`，include_code的默认路径可修改。
    {{ "{% include_code test.js" }} %}
{% include_code test.js %}

实例2 - 定制标题：     
默认情况`<figcaption>` 为文件名称，但可以修改为任意你喜欢的标题，如下：   
    {{ "{% include_code Add to_fraction for floats test.rb" }} %}
{% include_code Add to_fraction for floats test.rb %}

实例3 - 强制高亮：     
	{{ "{% include_code test.coffee lang:coffeescript" }} %}
{% include_code test.coffee lang:coffeescript %}

<h2 id="blockquote">3. 引用</h2>
语法：   
	{{ "{% blockquote [author[, source]] [link] [source_link_title]" }} %}
	Quote string
	{{ "{% endblockquote" }} %}

实例1 - 基本用法：     
	{{ "{% blockquote" }} %}
	Last night I lay in bed looking up at the stars in the sky and I thought to myself, where the heck is the ceiling.
	{{ "{% endblockquote" }} %}

{% blockquote %}
Last night I lay in bed looking up at the stars in the sky and I thought to myself, where the heck is the ceiling.
{% endblockquote %}

实例2 - 名言：     
	{{ "{% blockquote Douglas Adams, The Hichhikers Guide to the Galaxy" }} %}
	Flying is learning how to throw yourself at the ground and miss.
	{{ "{% endblockquote" }} %}

{% blockquote Douglas Adams, The Hitchhikers Guide to the Galaxy %}
Flying is learning how to throw yourself at the ground and miss.
{% endblockquote %}

实例3 - 网络引用：     
    {{ "{% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing" }} %}
    Every interaction is both precious and an opportunity to delight.
    {{ "{% endblockquote" }} %}

{% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing %}
Every interaction is both precious and an opportunity to delight.
{% endblockquote %}

<h2 id="gist">4. gist</h2>
语法：   
    {{ "{% gist gist_id [filename]" }} %}
实例：
	{{ "{% gist 996818" }} %}
{% gist 996818 %}

包含多个文件：   
	{{ "{% gist 1059334 svg_bullets.rb" }} %}
	{{ "{% gist 1059334 usage.scss" }} %}

<h2 id="img">5. 图片</h2>
语法：   
	{{ "{% img [class names] /path/to/image [width] [height] [title text [alt text]]" }} %}
实例：    
	{{ "{% img http://placekitten.com/890/280" }} %}
	{{ "{% img left http://placekitten.com/320/250 Place Kitten #2" }} %}
	{{ "{% img right http://placekitten.com/300/500 150 250 Place Kitten #3" }} %}
	{{ "{% img right http://placekitten.com/300/500 150 250 'Place Kitten #4' 'An image of a very cute kitten'" }} %}

效果如下：    
{% img http://placekitten.com/890/280 %}
Bacon ipsum dolor sit amet exercitation ball tip consectetur tempor. 
Biltong exercitation aliqua, ribeye consequat veniam consectetur.

{% img left http://placekitten.com/320/250 Place Kitten #2 %}
{% img right http://placekitten.com/300/500 150 250 Place Kitten #3 %}
Aliquip nulla do tempor, ball tip dolore anim esse strip steak nisi nostrud. 
Tri-tip mollit deserunt ut duis, commodo brisket short loin est hamburger sunt 
consequat rump meatloaf. Exercitation enim aliqua tempor dolore. Non eu venison, 
officia boudin tri-tip enim beef ribs flank cupidatat in aute. 
Tail voluptate fugiat aute flank, venison sint.

{% img right http://placekitten.com/300/500 150 250 "Place Kitten #4" "An image of a very cute kitten" %}

Brisket quis velit bresaola. Pork loin pork chop beef duis. 
Short loin fugiat officia short ribs magna. Ullamco eu proident jerky, 
fugiat chuck nostrud ham rump meatloaf eiusmod adipisicing. Qui et reprehenderit, 
magna biltong consequat short ribs pancetta. Tail tenderloin sausage, 
hamburger corned beef drumstick ad. Eu labore enim velit.

Filler text courtesy of [Bacon Ipsum](http://baconipsum.com), 
Images courtesy of [Place Kitten](http://placekitten.com).

<h2 id="video">6. 视频</h2>
语法：   
	{{ "{% video url/to/video [width height] [url/to/poster]" }} %}
实例：    
	{{ "{% video http://s3.imathis.com/video/zero-to-fancy-buttons.mp4 640 320 http://s3.imathis.com/video/zero-to-fancy-buttons.png" }} %}
效果如下： 
{% video http://s3.imathis.com/video/zero-to-fancy-buttons.mp4 640 320 http://s3.imathis.com/video/zero-to-fancy-buttons.png %}

<h2 id="category">7. category</h2>
修改plugin，生成categories，补丁文件如下：    
{% include_code lang:diff category_generator.rb.diff %}

Category列表添加到右侧，跟最近博客显示在一起，补丁文件如下：    
{% include_code lang:diff recent_posts.html.diff %}

<hr />

