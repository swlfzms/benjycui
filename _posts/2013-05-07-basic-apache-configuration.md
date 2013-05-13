---
layout: post
title: Apache常用配置整理
---

## 基于端口新建虚拟主机（Ubuntu下）

{% highlight bash %}
cd /etc/apache2/
vi ports.conf
{% endhighlight %}

修改文件如下

{% highlight bash %}
# 一个端口对应一个虚拟主机
NameVirtualHost *:80
NameVirtualHost *:591
NameVirtualHost *:8008
NameVirtualHost *:8080
Listen 80
Listen 591
Listen 8008
Listen 8080
{% endhighlight %}

然后

{% highlight bash %}
cd /etc/apache2/sites-available/
cp default new-site
vi new-site
{% endhighlight %}

对应apache配置如下

{% highlight bash %}
<VirtualHost *:8080>
        ServerAdmin webmaster@localhost

        DocumentRoot /var/www/folder-name
        <Directory />
                Options FollowSymLinks
                AllowOverride All
        </Directory>
        <Directory /var/www/folder-name>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride All
                Order allow,deny
                allow from all
        </Directory>

        ScriptAlias /cgi-bin/ /usr/lib/cgi-bin/
        <Directory "/usr/lib/cgi-bin">
                AllowOverride All
                Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
                Order allow,deny
                Allow from all
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log

        # Possible values include: debug, info, notice, warn, error, crit,
        # alert, emerg.
        LogLevel warn

        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
{% endhighlight %}

启用/停止虚拟主机

{% highlight bash %}
# 启用
a2ensite new-site # 虚拟主机名即sites-available内的文件名
# 停止
a2dissite new-site
{% endhighlight %}

重载apache配置

{% highlight bash %}
/etc/init.d/apache2 reload
{% endhighlight %}
