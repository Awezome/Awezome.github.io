---
title: Lavarel 增加全局函数文件
tags:
  - lavarel
id: 2484
categories:
  - PHP
date: 2015-09-03 10:34:02
---

1.生成文件 app\Helpers\helper.php

2.在composer.json 增加 

```json
autoload": {
        "files": [
            "app/Helpoers/helper.php"
        ]
    }
```
3.运行 composer dump-autoload
这样就可以在 helper.php 里写函数，整个project都可以调用了。