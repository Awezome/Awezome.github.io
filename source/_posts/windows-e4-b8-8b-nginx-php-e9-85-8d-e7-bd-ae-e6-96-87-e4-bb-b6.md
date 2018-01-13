---
title: windows 下 Nginx + PHP 配置文件
tags:
  - nginx
  - php
id: 2191
categories:
  - PHP
date: 2014-09-13 17:18:35
---

`server {
        listen       9999;
        server_name  localhost;
        #charset koi8-r;
        #access_log  logs/host.access.log  main;

        location / {
            root   C:/www/cms/;
            index  index.php index.html index.htm;
        }

        #error_page  404              /404.html;
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
        location ~ \.php$ {
			root           C:/www/cms/;
			fastcgi_pass   127.0.0.1:9000;
			fastcgi_index  index.php;
			fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
			include        fastcgi_params;
        }
    }`
<!--more-->

`@echo off
REM Windows 下无效
REM set PHP_FCGI_CHILDREN=5

REM 每个进程处理的最大请求数，或设置为 Windows 环境变量
set PHP_FCGI_MAX_REQUESTS=1000

echo Starting PHP FastCGI...
RunHiddenConsole D:/xampp/php/php-cgi.exe -b 127.0.0.1:9000 -c D:/xampp/php/php.ini

echo Starting nginx..
RunHiddenConsole D:/nginx/nginx.exe -p D:/nginx/`

`@echo off
echo Stopping nginx...  
taskkill /F /IM nginx.exe > nul
echo Stopping PHP FastCGI...
taskkill /F /IM php-cgi.exe > nul
exit`