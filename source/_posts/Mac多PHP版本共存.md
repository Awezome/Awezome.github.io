---
title: Mac多PHP版本共存
date: 2018-01-22 16:52:15
tags: PHP
---

先安装 php

```shell
brew install php56
brew unlink php56
brew install php71
```

主要用到 php-version 扩展，执行php-version可以查看和切换当前使用的版本和未使用的版本

```shell
brew install php-version
source $(brew --prefix php-version)/php-version.sh

php-version
```

进入PHP的配置目录把php-fpm的端口默认端口修改掉。

5.6的配置文件在 /usr/local/etc/php/5.6/php-fpm.conf

7.1的配置文件在 /usr/local/etc/php/7.1/php-fpm.d/

