---
layout: post
title: PHP数组中的陷阱
---

## 数组个数

看一下这段代码，应该输出什么？

{% highlight php %}
<?php
$arr = array(
    'foo' => 'bar',
    'bar' => 'foo',
);

echo count($arr);
{% endhighlight %}

实际上数组的长度为2，而不是3，最后一个数组单元后面的逗号是可以省略的。

## 数组键的类型

{% highlight php %}
<?php
$arr = array(
    'a' => 1,
    '8' => 2,
    true => 4,
    false => 5,
    null => 6,
);
{% endhighlight %}

前两个键的类型分别为string和integer，PHP会将string(8)强制转换成int(8)。  
后四个键的键名会被转换成1、0、''，因为数组的键值必须为 integer 或者 string，所以以上键值会被强制转换。

## 数组添加新元素

一般添加给一个数组添加新元素，都是用下面的方法

{% highlight php %}
<?php
$var[] = 'a';
$var[] = 'b';
{% endhighlight %}

但是如果出现下面这种情况，问题就来了。

{% highlight php %}
<?php
$var = 'abc';
$var[] = 'a';
{% endhighlight %}

$var是一个字符串，而不是数字，[]实际上代表了**字符串访问运算符**。

所以报了一个Fatal error的错误

<pre>
Fatal error: [] operator not supported for strings in xxx on line x
</pre>

## PHP数组数字下标

{% highlight php %}
<?php
$arr = array(1, 2, 3);
print_t($arr);

foreach ($arr as $i => $value) {
    unset($array[$i]);
}
print_r($arr);

$arr[] = 4;
print_r($arr);
{% endhighlight %}

以上将会输出

<pre>

Array
(
    [0] => 1
    [1] => 2
    [2] => 3
)
Array
(
)
Array
(
    [3] => 4
)
</pre>

留意到销毁了数组原来所有的元素后，再添加新元素，下标依然在原来最大的下标的基础上加1。

## 数组元素引用

{% highlight php linenos %}
<?php
$colors = array('red', 'blue', 'green', 'yellow');

foreach ($colors as &$color) {
    $color = strtoupper($color);
}

foreach ($colors as $key => $color) {
    $colors[$key] = strtoupper($color);
}

print_r($colors);
{% endhighlight %}

以上代码的输出
<pre>
Array
(
    [0] => RED
    [1] => BLUE
    [2] => GREEN
    [3] => GREEN
)
</pre>

出问题了吧？数组中第三个元素和第四个元素的值都是 'Green'。

原因就出现在第4行的 `&$color`。

当程序执行到第7行时，$color和$colors[3]指向的是同一个地方。接着第8行的地方没循环一次，$color的值依次被赋值为$colors数组的第1、2、3、4个元素等。

$color的值的每一次改变都会导致$colors[3]的值跟着变，因为它们指向的是同一个地方。

最后一次循环开始时，$colors[3]的值与$colors[2]的值相同，所以以上输出的结果中数组的第3个元素与第四个元素的值相同。

要避免这中意想之外的情况，只需要在程序执行到第七行的时候销毁 $color 变量。

{% highlight php %}
<?php
unset($color);
{% endhighlight %}
