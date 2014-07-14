---
layout: post
title:  "使用ImageMagick批量压缩图片"
date:   2013-08-30 13:54:23
---

今天跟同事遇到一个问题，需要在Centos下面将大量的图片进行压缩，同时要求保持原有图片名不变。在安装了ImageMagick之后，网上找的一些方法都没办法实现需求。临近吃饭时候忽然发现[一篇文章](http://www.charry.org/docs/linux/ImageMagick/ImageMagick.html)，非常详细的介绍了ImageMagick的使用办法。

{% highlight ruby %}
mogrify -sample 80x60 *.jpg
{% endhighlight %}
这条命令会将文件夹下所有.jpg图片都压缩为80x60大小，同时图片名保持不变。