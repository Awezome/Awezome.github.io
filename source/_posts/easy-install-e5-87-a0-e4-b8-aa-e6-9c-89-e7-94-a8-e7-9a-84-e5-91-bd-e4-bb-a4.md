---
title: Easy Install 几个有用的命令
tags:
  - easy_install
  - Python
id: 1626
categories:
  - Other
date: 2014-04-07 11:59:14
---

安装
`easy_install SQLObject
easy_install -f http://pythonpaste.org/package_index.html SQLObject
easy_install http://example.com/path/to/MyPackage-1.2.3.tgz
easy_install /my_downloads/OtherPackage-3.2.1-py2.3.egg`

版本
`easy_install "SQLObject==1.13"
easy_install "SQLObject<1.13"`

删除
`easy_install -m SQLObject`

升级
`easy_install --upgrade PyProtocols`

官方手册
`https://pythonhosted.org/setuptools/easy_install.html`