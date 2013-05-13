---
layout: post
title: Git基本使用入门
---

## 必备软件安装

* Windows下安装msysgit，自行选择是否安装图形化客户端TortoiseGit
* Linux下安装Git软件包或者自行编译。

## 本地生成SSH key

{% highlight bash %}
cd ~/.ssh
ssh-keygen -t rsa -C "your_email@domain.com"
Enter file in which to save the key (/home/you/.ssh/rsa): [随便起一个名，不要和原有的重名即可]
Enter passphrase (empty for no passphrase): [输入密钥，在自己的电脑上建议为空]
Enter same passphrase again: [重复输入上面的密钥]
{% endhighlight %}

复制生成的xxx.pub文件内的内容添加到GitHub/Bitbucket上，此后进行`git push`等操作可免输入帐号密码。

## 基本命令

添加一个远程仓库
{% highlight bash %}
git remote add <name> <url> #name可以是origin等
{% endhighlight %}
