---
layout: post
date: 2013-09-18 13:13
title: "使用rsync进行跨服务器备份"
description: "linux上利用rsync来实现跨服务器间的数据同步和备份。" 
---
在两台服务器上都安装rsync
{% highlight ruby %}
sudo apt-get install rsync
{% endhighlight %}

在备份服务器上生成SSH密钥，一路enter，表输入密码。
{% highlight ruby %}
ssh-keygen
{% endhighlight %}

使用以下命令，从备份服务器复制密钥到主服务器。相关目录不存在的话，可以创建。
{% highlight ruby %}
scp ~/.ssh/id_rsa.pub user@production_server:~/.ssh/uploaded_key.pub
ssh user@production_server 'echo `cat ~/.ssh/uploaded_key.pub` >> ~/.ssh/authorized_keys'
{% endhighlight %}

或者直接在主服务器相应目录下运行以下命令
{% highlight ruby %}
cat uploaded_key.pub >> authorized_keys
{% endhighlight %}

在备份服务器上进行测试，如果不需要输入密码正常显示就成功。
{% highlight ruby %}
ssh user@production_server 'ls -al'
{% endhighlight %}

在备份服务器上创建存储备份的文件夹
{% highlight ruby %}
mkdir ~/backups/
{% endhighlight %}

在主服务器上，创建一个文件夹放要备份的东西，然后运行类似命令，检查是否成功。
{% highlight ruby %}
rsync -ahvz user@production_server:~/public ~/backups/public_orig/
{% endhighlight %}

最后添加自动运行到crontab
{% highlight ruby %}
rsync -ahvz --delete user@production_server:~/public ~/backups/public_orig
{% endhighlight %}
这回我们引入一个 –delete 选项，表示客户端上的数据要与服务器端完全一致，如果备份服上有主服务器上不存在的文件，则删除。最终目的是让数据完全与主服务器上保持一致。

