---
layout: post
title: Laravel 4环境设置
---

`bootstrap/start.php`文件中有如下代码：

{% highlight php %}
<?php
$env = $app->detectEnvironment(array(

	'local' => array('your-machine-name'),

));
{% endhighlight %}

这段代码这样解释，通过满足数组array('your-machine-name')中定义的url格式的url来访问应用的时候，环境名设定为对应键值，在这段代码中环境名即`local`。

一般本地都是用localhost访问本地网站，所以设置如下：

{% highlight php %}
<?php
$env = $app->detectEnvironment(array(

	'local' => array('localhost'),
    // 可使用正则表达式，如
    // 'local' => array('local.*'),

    // 本地访问也可能是
    // 'local' => array('127.0.0.1'),

));
{% endhighlight %}

接下来在`app/config`文件夹中新建一个名为上述环境名的文件夹，如上即local文件夹。

最后，想覆盖原来配置，就新建对应的同名的文件，在新文件中设置对应的配置项即可覆盖原配置。

[方法来源](http://andrewelkins.com/programming/php/how-to-set-laravel-4-environments/)

Google是个好东西。
