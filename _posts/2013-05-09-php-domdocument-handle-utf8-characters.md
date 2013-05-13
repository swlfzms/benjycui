---
layout: post
title: PHP DOMDocument类处理utf-8字符
---

如下代码，输出会乱码：

{% highlight php %}
<?php
$html = '一些中文';
$dom = new DomDocument();
$dom->loadHTML($html);

echo $dom->saveHTML();
{% endhighlight %}

可以在调用loadHTML方法前，对html进行处理

{% highlight php %}
<?php
$html = preg_replace_callback('/[\x{80}-\x{10FFFF}]/u', function($match) {
    list($utf8) = $match;
    $entity = mb_convert_encoding($utf8, 'HTML-ENTITIES', 'UTF-8');
    // printf("%s -> %s\n", $utf8, $entity);
    return $entity;
}, $html);
{% endhighlight %}

或者

{% highlight php %}
<?php
$html = '<meta http-equiv="content-type" content="text/html; charset=utf-8">'.$html;
{% endhighlight %}

解决乱码问题，方法来源于[Stack Overflow](http://stackoverflow.com/questions/11309194/php-domdocument-failing-to-handle-utf-8-characters)
