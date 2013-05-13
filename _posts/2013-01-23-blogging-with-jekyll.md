---
layout: post
title: 用Jekyll搭建静态博客
---

Jekyll是一个用Ruby写的静态网站生成工具。Jekyll支持使用[Markdown](http://zh.wikipedia.org/wiki/Markdown)语法进行写作，让你更专注于写作而非排版，你可以使用你熟悉的文本编辑轻松完成写作。写毕，在本地通过Jekyll生成静态网站，将生成的静态网站上传到你的服务器上即可。如果你会使用Git，可将博客托管至GitHub，省去租用服务器的费用，还可绑定自己的域名。

如果你打算使用GitHub托管自己的博客，本地安装是非必需的。因为GitHub服务器装有Jekyll，在你将符合Jekyll规范的文件上传到GitHub，Github会自动帮你生成对应的静态网站。如果你有自己的服务器，则需要在本地生成静态网站后上传。当然你可以在你的服务器安装并运行Jekyll。不管怎样，还是建议在本地安装Jekyll，这样让你本地即可预览博客的样子。

## 本地安装Jekyll

RubyGems是一个Ruby程序包管理器，类似Debian的apt-get和RedHat的RPM，可简单地通过RubyGems来安装Jekyll。Jekyll官方也建议使用这种方法进行安装。

Linux用户参考 [https://github.com/mojombo/jekyll/wiki/install](https://github.com/mojombo/jekyll/wiki/install)  
Windows用户参考 [Running Jekyll on Windows](http://www.madhur.co.in/blog/2011/09/01/runningjekyllwindows.html)

## 下载Jekyll实例

[这个页面](https://github.com/mojombo/jekyll/wiki/Sites)包含很多用Jekyll构建的博客，上面也提供了源代码的下载地址，看到喜欢的博客可以下载下来研究，个人认为是学习Jekyll最佳的方法。会Git的朋友也可以直接`git clone`下来。

## 本地运行Jekyll实例

在下载的Jekyll实例所在的目录，执行以下语句

{% highlight bash %}
# 其中--auto选项可以让内容有更新时自动更新静态页面
jekyll --server --auto
{% endhighlight %}

语句执行后，会生成一个_site目录，该目录就是所生成的静态网站。  
可以通过http://localhost:4000访问本地网站。

## 代码高亮

安装Pygments语法高亮插件，如果系统是Debian/Ubuntu，可以通过新立得安装。

{% highlight bash %}
sudo apt-get install python-pygments
{% endhighlight %}

博客根目录的_config.yml为博客的配置文件，我的配置如下

<pre>
markdown: rdiscount
pygments: true
</pre>

最后附上写这篇博文的截图：

![blogging with jekyll](/images/blogging_with_jekyll.png)

2013/4/23注：

这两天重装了系统，同时也需要重装Jekyll。  
今天发现在终端下执行了`jekyll --server --auto`语句，并对博文进行改动后，jekyll并不会自动重新生成对应的html文件。
在Stack Overflow上找到了原因，说是最近directory_watcher gem的改动导致的问题，将directory_watcher gem降一下版本即可解决问题。
<pre>
sudo gem uninstall directory_watcher
 && sudo gem install directory_watcher -v 1.4.1
</pre>

2013/4/25注：  

1.对php进行语法高亮时，代码段中要有<?php开始标签，否则Pygments无法php代码。

2.要为一段代码标上行号，类似下面

{% highlight php linenos %}
<?php
function hello_world() {
    echo 'Hello World!';
}
{% endhighlight %}

只需这样，注意**lineos**属性

<pre>
{{'{%'}} highlight languange linenos {{'%'}}}
&lt;?php
function hello_world() {
    echo 'Hello World!';
}
</pre>

理论上上面那样是可以正确显示行号的，但貌似是Jekyll版本问题导致不能正确显示行号，如果你也不能正确显示行号，可以继续往下看

在 **_plugins** 目录下新建一个文件 ** highlight.rb **，方法来源于[这里](https://github.com/a24kaku/scala-platea/blob/1ae1870b94a37e2f3da5a9fbc63cd55907b0ee86/src/jekyll/_plugins/highlight.rb)

文件内容如下

{% highlight ruby %}
module Jekyll

  class HighlightBlock < Liquid::Block
    include Liquid::StandardFilters

    # The regular expression syntax checker. Start with the language specifier.
    # Follow that by zero or more space separated options that take one of two
    # forms:
    #
    # 1. name
    # 2. name=value

    # SYNTAX is already defined
    #SYNTAX = /^([a-zA-Z0-9.+#-]+)((\s+\w+(=\w+)?)*)$/

    def initialize(tag_name, markup, tokens)
      super
      if markup.strip =~ SYNTAX
        @lang = $1
        @options = {}
        if defined?($2) && $2 != ''
          $2.split.each do |opt|
            key, value = opt.split('=')
            if value.nil?
              if key == 'linenos'
                value = 'inline'
              else
                value = true
              end
            end
            @options[key] = value
          end
        end
      else
        raise SyntaxError.new("Syntax Error in 'highlight' - Valid syntax: highlight <lang> [linenos]")
      end
    end

    def render(context)
      if context.registers[:site].pygments
        render_pygments(context, super)
      else
        render_codehighlighter(context, super)
      end
    end

    def render_pygments(context, code)
      @options[:encoding] = 'utf-8'

      output = add_code_tags(
        Pygments.highlight(code, :lexer => @lang, :options => @options),
        @lang
      )

      output = context["pygments_prefix"] + output if context["pygments_prefix"]
      output = output + context["pygments_suffix"] if context["pygments_suffix"]
      output
    end

    def render_codehighlighter(context, code)
      #The div is required because RDiscount blows ass
      <<-HTML
<div>
  <pre><code class='#{@lang}'>#{h(code).strip}</code></pre>
</div>
      HTML
    end

    def add_code_tags(code, lang)
      # Add nested <code> tags to code blocks
      code = code.sub(/<pre>/,'<pre><code class="' + lang + '">')
      code = code.sub(/<\/pre>/,"</code></pre>")
    end

  end

end

Liquid::Template.register_tag('highlight', Jekyll::HighlightBlock)
{% endhighlight %}

大功告成！行号正确显示。

2013/4/30注：  

[遍历博客所有文章](/images/loop-site-posts.png)  
[遍历某分类下的文章](/images/loop-category-posts.png)
