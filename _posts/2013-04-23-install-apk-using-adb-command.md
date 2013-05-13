---
layout: post
title: Linux下通过adb命令安装apk包到Android设备
---

1. Linux下安装好Adb。  
如果你的系统是Debian/Ubuntu，可以直接通过新立得安装android-tools-adb软件包。

2. Android设备开启**USB调试**。

3. 终端下使用`adb install -r someapk.apk`安装apk包到Android设备。  
注：该操作会覆盖Android设备对应的程序
