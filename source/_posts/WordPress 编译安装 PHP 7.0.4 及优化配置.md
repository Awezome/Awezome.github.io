---
title: WordPress 编译安装 PHP 7.0.4 及优化配置
tags:
  - php
id: 2506
categories:
  - PHP
date: 2016-03-20 17:18:45
---

好长时间没有理自己的博客了，今天正好有时间，把PHP升级到最新版本PHP 7.0.4。

还是选择编译安装，PHP7 推荐 gcc 4.8以上，gcc -v 看了一下VPS才4.4，所以第一步先升级一下 gcc，这个过程比较长，可以先下部电影等着~~

因为VPS配置太low，所以先准备一些，搞个临时swap 

`$ SWAP=/tmp/swap
$ dd if=/dev/zero of=$SWAP bs=1M count=1000
$ mkswap $SWAP
$ swapon $SWAP
#最后记得关上
$ swapoff -a`

ok , 开始······
`
$ wget http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-5.3.0/gcc-5.3.0.tar.gz
$ tar -xf gcc-5.3.0.tar.gz
$ cd gcc-5.3.0
$ ./contrib/download_prerequisites
$ mkdir gcc_temp  && cd gcc_temp
$ ../configure --enable-checking=release --enable-languages=c,c++ --disable-multilib
$ make && make install`

当然再装gcc时有一些小问题，大多可以google到，这里记一点,如果出现 *** [stage1-bubble] 错误 2 
`$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib64
$ source /etc/profile`

gcc -v 看一下，好了的话就开始PHP

`$ wget http://cn2.php.net/get/php-7.0.4.tar.bz2/from/this/mirror
$ tar xzvf mirror
$ cd php-7.0.4/
$ ./configure --prefix=/data/app/php7 --with-curl --with-freetype-dir --with-gd --with-gettext --with-iconv-dir --with-kerberos --with-libdir=lib64 --with-libxml-dir --with-mysqli --with-openssl --with-pcre-regex --with-pdo-mysql --with-pdo-sqlite --with-pear --with-png-dir --with-xmlrpc --with-xsl --with-zlib --enable-fpm --enable-bcmath --enable-libxml --enable-inline-optimization --enable-gd-native-ttf --enable-mbregex --enable-mbstring --enable-opcache --enable-pcntl --enable-shmop --enable-soap --enable-sockets --enable-sysvsem --enable-xml --enable-zip
$ make && make install
`

完成后再做下配置
`$ cp php.ini-product /data/app/php7/lib/php.ini
$ cp /usr/local/php/etc/php-fpm.conf.default /data/app/php7/etc/php-fpm.conf
$ cp /usr/local/php/etc/php-fpm.d/www.conf.default /data/app/php7/etc/php-fpm.d/www.conf
$ cp -R ./sapi/fpm/init.d.php-fpm /etc/init.d/php7-fpm
$ ln -s /data/app/php7/bin/php /usr/local/bin/php7
$ service php-fpm stop
$ service php7-fpm start
`

到这就完成了安装，如果启用PHP7后发现博客无法打开，那就把DEBUG打开吧，问题很好解决。再进行一些配置。

启用Opcache, 在php.ini中加入:
`zend_extension=opcache.so
opcache.enable=1
opcache.enable_cli=1
opcache.file_cache=/tmp`

最后，又换了一款Cache插件，这个比之前用的WP-cache , Hyper-cache 都好用，叫CometCache，使页面静态化，进一步提高页面打开速度。