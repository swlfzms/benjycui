---
layout: post
title: Linux下遇到的问题及解决方法备忘
---

## Virtualbox启动虚拟机报"kernel driver not installed"错误

启动虚拟机，报以下错误

<pre>
Kernel driver not installed (rc=-1908)

The VirtualBox Linux kernel driver (vboxdrv) is either not loaded or there is a permission problem with /dev/vboxdrv. Please reinstall the kernel module by executing

'/etc/init.d/vboxdrv setup'

as root. If it is available in your distribution, you should install the DKMS package first. This package keeps track of Linux kernel changes and recompiles the vboxdrv kernel module if necessary.
</pre>

解决方法（[来源](http://askubuntu.com/questions/205154/virtualbox-etc-init-d-vboxdrv-setup-issue)）：

{% highlight bash %}
sudo apt-get install linux-headers-`uname -r`

dpkg-reconfigure virtualbox-dkms  
modprobe vboxdrv
{% endhighlight %}
