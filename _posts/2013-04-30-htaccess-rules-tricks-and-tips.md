---
layout: post
title: 常用.htaccess规则整理
---

## www域名跳转到无www域名
<pre>
RewriteEngine On
RewriteCond %{HTTP_HOST} !^my-domain\.com$ [NC]
RewriteRule ^(.*)$ http://my-domain.com/$1 [R=301,L]
</pre>

## 无www域名跳转到www域名
<pre>
RewriteEngine On
RewriteCond %{HTTP_HOST} !^www\.
RewriteRule ^(.*)$ http://www.%{HTTP_HOST}/$1 [R=301,L]
</pre>
