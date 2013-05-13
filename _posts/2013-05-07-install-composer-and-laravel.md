---
layout: post
title: Composer和Laravel的安装
---

## 安装Composer

{% highlight bash %}
curl -sS https://getcomposer.org/installer | php
# 或者
php -r "eval('?>'.file_get_contents('https://getcomposer.org/installer'));"
{% endhighlight %}

得到`composer.phar`文件

## 安装Laravel
[下载Laravel](https://github.com/laravel/laravel/archive/develop.zip)

解压，将上面得到的`composer.phar`文件移动到Laravel根目录  
执行`php composer.phar install`，自动安装所有框架依赖

注：想让Composer全局可用，只需移动`composer.phar`文件到`/usr/local/bin`
{% highlight bash %}
mv composer.phar /usr/local/bin/composer
{% endhighlight %}
然后可简单使用`composer install`安装依赖。
