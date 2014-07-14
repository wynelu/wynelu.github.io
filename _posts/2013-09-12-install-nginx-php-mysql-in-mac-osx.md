---
layout: post
date: 2013-09-12 16:20
title: "在Mac OSX上安装Nginx、PHP、Mysql"
description: "在Mac OSX上安装Nginx、PHP、Mysql的完美教程，不断更新。 " 
---

###安装Homebrew
{% highlight ruby %}
 ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"
{% endhighlight %}
记得运行brew doctor命令，来确认Homebrew安装正确。如果已经安装过MacPorts或者RVM，会有提示将MacPorts挪走。
{% highlight ruby %}
sudo mv /opt/local ~/macports
{% endhighlight %}


###安装和调试Nginx
{% highlight ruby %}
brew install nginx
{% endhighlight %}

安装完成之后，进入/usr/local/etc/nginx目录，这里是相关的配置文件目录。在下面建立一个vhost文件夹，用来放置单独的网站配置。
{% highlight ruby %}
mkdir vhost
{% endhighlight %}

在vhost目录里建立一个default.conf，内容类似如下
{% highlight ruby %}
    server {
        listen       8080;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
          #这里是网站文件的目录，以下另外一处相同处理
            root   /Users/feir/sites; 
            index  index.html index.htm index.php;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /Users/feir/sites;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        location ~ \.php$ {
                root   /Users/feir/sites;
                fastcgi_pass   127.0.0.1:9000;
                fastcgi_index  index.php;
                fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
                include        fastcgi_params;
        }

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
{% endhighlight %}

将nginx.conf里面的 server { ... }部分全部注销或者删除掉，同时在http {...}模块里增加以下内容
{% highlight ruby %}
include /usr/local/etc/nginx/vhost/*.conf;
{% endhighlight %}

OK，现在已经完成了Nginx的安装，以下为Nginx的管理命令：
{% highlight ruby %}
sudo nginx // 启动Nginx
#重新加载|重启|停止|退出 nginx
nginx -s reload|reopen|stop|quit
{% endhighlight %}

**将Nginx加入开机启动**
{% highlight ruby %}
ln -sfv /usr/local/opt/nginx/*.plist ~/Library/LaunchAgents
//启动
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist
{% endhighlight %}

###安装MySQL
{% highlight ruby %}
brew install mysql
{% endhighlight %}

初始化Mysql数据库
{% highlight ruby %}
#修改 feir 为自己的用户名
mysql_install_db --verbose --user='feir' --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp
{% endhighlight %}

启动Mysql
{% highlight ruby %}
mysql.server start
{% endhighlight %}

目前访问mysql是不需要密码的，如果需要的话，得运行配置下
{% highlight ruby %}
mysql_secure_installation
{% endhighlight %}
按照提示进行操作就可以。

**将mysql加入开机启动**
{% highlight ruby %}
ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
//启动
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
{% endhighlight %}

**管理数据库**
如果喜欢界面化管理mysql，在Mac上可以用[Sequel Pro](http://www.sequelpro.com)，比PHPmyadmin好用。

###安装PHP
{% highlight ruby %}
brew tap homebrew/dupes
brew tap josegonzalez/homebrew-php
brew install php55 --with-fpm --with-mysql
{% endhighlight %}

**将PHP加入开机启动**
{% highlight ruby %}
mkdir -p ~/Library/LaunchAgents
cp /usr/local/Cellar/php54/5.4.19/homebrew-php.josegonzalez.php54.plist ~/Library/LaunchAgents/
launchctl load -w ~/Library/LaunchAgents/homebrew-php.josegonzalez.php54.plist
{% endhighlight %}

###各类配置文件
**nginx**
/usr/local/etc/nginx/nginx.conf

**PHP (FPM)**
/usr/local/etc/php/5.4/php-fpm.conf

**PHP (CLI)**
/usr/local/etc/php/5.4/php.ini

**Mysql**
/usr/local/opt/mysql/my-new.cnf







