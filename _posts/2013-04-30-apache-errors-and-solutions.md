---
layout: post
title: Apache常见错误及处理
---

* ##Option FollowSymlinks not allowed here  
解决方法：`FollowSymLinks`替换成`SymLinksIfOwnerMatch`
