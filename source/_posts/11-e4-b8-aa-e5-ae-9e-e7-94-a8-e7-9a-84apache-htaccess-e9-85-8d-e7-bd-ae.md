---
title: 11个实用的Apache .htaccess配置
id: 1220
categories:
  - Linux
date: 2014-03-15 12:00:12
tags:
---

> 用阿里测时发现网站设置静态内容缓存时间有问题，搜了一下看.htaccess可以配置，顺便整理一下其它功能。
1\. 强制后缀反斜杠
在URL的尾部加上反斜杠似乎对SEO有利 ：）
&lt;IfModule mod_rewrite.c&gt;
RewriteCond %{REQUEST_URI} /+[^\.]+$
RewriteRule ^(.+[^/])$ %{REQUEST_URI}/ [R=301,L]
&lt;/IfModule&gt;
2\. 防盗链
节省你宝贵的带宽吧！
RewriteEngine On
#Replace ?mysite\.com/ with your blog url
RewriteCond %{HTTP_REFERER} !^http://(.+\.)?mysite\.com/ [NC]
RewriteCond %{HTTP_REFERER} !^$
#Replace /images/nohotlink.jpg with your "don't hotlink" image url
RewriteRule .*\.(jpe?g|gif|bmp|png)$ /images/nohotlink.jpg [L]
3\. 重定向移动设备<!--more-->
加入你的网站支持移动设备访问的话，最好还是重定向移动设备的访问到专门定制的页面

RewriteEngine On
RewriteCond %{REQUEST_URI} !^/m/.*$
RewriteCond %{HTTP_ACCEPT} "text/vnd.wap.wml|application/vnd.wap.xhtml+xml" [NC,OR]
RewriteCond %{HTTP_USER_AGENT} "acs|alav|alca|amoi|audi|aste|avan|benq|bird|blac|blaz|brew|cell|cldc|cmd-" [NC,OR]
RewriteCond %{HTTP_USER_AGENT} "dang|doco|eric|hipt|inno|ipaq|java|jigs|kddi|keji|leno|lg-c|lg-d|lg-g|lge-" [NC,OR]
RewriteCond %{HTTP_USER_AGENT} "maui|maxo|midp|mits|mmef|mobi|mot-|moto|mwbp|nec-|newt|noki|opwv" [NC,OR]
RewriteCond %{HTTP_USER_AGENT} "palm|pana|pant|pdxg|phil|play|pluc|port|prox|qtek|qwap|sage|sams|sany" [NC,OR]
RewriteCond %{HTTP_USER_AGENT} "sch-|sec-|send|seri|sgh-|shar|sie-|siem|smal|smar|sony|sph-|symb|t-mo" [NC,OR]
RewriteCond %{HTTP_USER_AGENT} "teli|tim-|tosh|tsm-|upg1|upsi|vk-v|voda|w3cs|wap-|wapa|wapi" [NC,OR]
RewriteCond %{HTTP_USER_AGENT} "wapp|wapr|webc|winw|winw|xda|xda-" [NC,OR]
RewriteCond %{HTTP_USER_AGENT} "up.browser|up.link|windowssce|iemobile|mini|mmp" [NC,OR]
RewriteCond %{HTTP_USER_AGENT} "symbian|midp|wap|phone|pocket|mobile|pda|psp" [NC]
#------------- The line below excludes the iPad
RewriteCond %{HTTP_USER_AGENT} !^.*iPad.*$
#-------------
RewriteCond %{HTTP_USER_AGENT} !macintosh [NC] #*SEE NOTE BELOW
RewriteRule ^(.*)$ /m/ [L,R=302]
4\. 强制浏览器下载指定的文件类型
你可以强制浏览器下载某些类型的文件，而不是读取并打开这些文件，例如MP3、XLS。
&lt;Files *.xls&gt;
ForceType application/octet-stream
Header set Content-Disposition attachment
&lt;/Files&gt;
&lt;Files *.eps&gt;
ForceType application/octet-stream
Header set Content-Disposition attachment
&lt;/Files&gt;
5\. 火狐的跨域名字体嵌入
火狐不允许嵌入一个外站的字体，下面的.htaccess片段可以绕过这个限制
&lt;FilesMatch "\.(ttf|otf|eot|woff)$"&gt;
&lt;IfModule mod_headers.c&gt;
Header set Access-Control-Allow-Origin "http://yourdomain.com"
&lt;/IfModule&gt;
&lt;/FilesMatch&gt;
6\. 使用.htaccess缓存 给网站提速
恐怕这个是最有用的代码片段了。这段代码能帮你极大的提高网站的速度！
# 1 YEAR
&lt;FilesMatch "\.(ico|pdf|flv)$"&gt;
Header set Cache-Control "max-age=29030400, public"
&lt;/FilesMatch&gt;
# 1 WEEK
&lt;FilesMatch "\.(jpg|jpeg|png|gif|swf)$"&gt;
Header set Cache-Control "max-age=604800, public"
&lt;/FilesMatch&gt;
# 2 DAYS
&lt;FilesMatch "\.(xml|txt|css|js)$"&gt;
Header set Cache-Control "max-age=172800, proxy-revalidate"
&lt;/FilesMatch&gt;
# 1 MIN
&lt;FilesMatch "\.(html|htm|php)$"&gt;
Header set Cache-Control "max-age=60, private, proxy-revalidate"
&lt;/FilesMatch&gt;
7\. 阻止WordPress博客的垃圾评论
还在为垃圾评论头疼吗？你可以用Akismet插件来解决这个问题，但是.htaccess文件来的更直接：阻止垃圾评论机器人访问wp-comments-post.php文件。
&lt;IfModule mod_rewrite.c&gt;
RewriteEngine On
RewriteCond %{REQUEST_METHOD} POST
RewriteCond %{REQUEST_URI} .wp-comments-post\.php*
RewriteCond %{HTTP_REFERER} !.*yourdomainname.* [OR]
RewriteCond %{HTTP_USER_AGENT} ^$
RewriteRule (.*) ^http://%{REMOTE_ADDR}/$ [R=301,L]
&lt;/IfModule&gt;
8.重定向不同的feed格式到统一的格式
很多年前，有很多不同的feed格式，例如RSS、Atom、RDF等等。但是现在RSS已经占了绝对的主导地位。下面这段代码可以让你重定向不同的feed格式到同一个feed。这段代码可以直接在WordPress博客上使用。
&lt;IfModule mod_alias.c&gt;
RedirectMatch 301 /feed/(atom|rdf|rss|rss2)/?$ http://example.com/feed/
RedirectMatch 301 /comments/feed/(atom|rdf|rss|rss2)/?$ http://example.com/comments/feed/
&lt;/IfModule&gt;
9\. 配置网站的HTML5视频
HTML5为我们带来了不用Flash的视频播放功能，但是你必须配置你的服务器来提供最新的HTML5视频播放功能。
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_URI} !=/favicon.ico
AddType video/ogg .ogv
AddType video/ogg .ogg
AddType video/mp4 .mp4
AddType video/webm .webm
AddType application/x-shockwave-flash swf
10\. 记录PHP错误
在页面上显示PHP错误是很尴尬的事情，也不安全，下面这段代码可以把PHP错误记录到.log文件中而不在页面显示。
# display no errs to user
php_flag display_startup_errors off
php_flag display_errors off
php_flag html_errors off
# log to file
php_flag log_errors on
php_value error_log /location/to/php_error.log
11\. 在JavaScript代码中运行PHP
在JS中插入PHP代码有时候是很有用的，例如读取数据库。下面这段代码可以让你在JS中运行PHP。
AddType application/x-httpd-php .js
AddHandler x-httpd-php5 .js
&lt;FilesMatch "\.(js|php)$"&gt;
SetHandler application/x-httpd-php
&lt;/FilesMatch&gt;

from:http://www.open-open.com/bbs/view/1319588511749