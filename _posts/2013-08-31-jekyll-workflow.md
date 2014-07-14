---
layout: post
title: "Jekyll Workflow"
description: "使用Sublime Text 2、Rakefile以及alias简化jekyll发布过程。"
---
一直是个喜欢折腾的人，所以前两天又折腾了下jekyll弄下博客，不知道这次又能折腾多久。

为了提升使用jekyll写博客的速度，给自己弄个了自动化流程，分享如下：

###配置Sublime Text 2 的Project设置

仿照[官方的Project说明文档](http://www.sublimetext.com/docs/2/projects.html)，将博客设置为project，并且默认隐藏了部分文件。现在在ST2里面打开blog项目，默认只显示_post目录了。

###使用rake命令
偷取的[jekyllbootstrap](http://jekyllbootstrap.com)的Rakefile文件，做了精简并符合我的jekyll配置。直接使用rake post title=标题名，就会自动在_posts目录里生成一个符合jekyll url要求的文档，并且文档默认加入post格式规范。

然后再配合[TextExpander](http://smilesoftware.com/TextExpander/index.html)创建个新个针对这个rake命令的如下Snipet。使用后，会自动进入到博客所在目录，并且按照jekyll的文章格式要求创建出新的文章文件。

{% highlight ruby %}
cd /Users/feir/websites/blog; \
rake post title=
{% endhighlight %}

###与sublime text结合
如果你使用sublime text编辑器，则更可以通过以下办法，在上面步骤创建了文章之后，自动打开sublime text加入编辑界面。

{% highlight ruby %}
sudo ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/subl
{% endhighlight %}

然后再rakefile里面创建new post模块的end前面加上这段代码
{% highlight ruby %}
system("subl #{filename}")
{% endhighlight %}

###自动化git命令
原先每次修改博客或者添加文章后，都需要经过git的add、commit、push步骤，每次输入这些命令还是很麻烦的。直接修改了~/.bash_profile文件，添加了快捷方式
{% highlight ruby %}
alias build_blog="cd /Users/feir/Documents/Website/myblog; jekyll;git add .;git commit -am 'Everything to be Rebuilt.';git push";
alias bb="build_blog";
{% endhighlight %}

再重新加载下~/.bash_profile 或者 ~/.profile
{% highlight ruby %}
source ~/.bash_profile
{% endhighlight %}
以后在博客有更新后，直接在命令行输入bb，就OK了。