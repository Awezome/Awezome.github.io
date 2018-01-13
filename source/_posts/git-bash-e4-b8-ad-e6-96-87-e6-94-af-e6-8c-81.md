---
title: git bash  中文支持
tags:
  - git
id: 1740
categories:
  - Other
date: 2014-04-21 16:58:43
---

1. ls 命令
修改%Gi%t\etc\git-completion.bash
加上：
`alias ls='ls --show-control-chars --color=auto'`
2\. 在Git Bash显示中文：
修改%Git%\etc\inputrc
`
# disable/enable 8bitinput
set meta-flag on
set input-meta on
set output-meta on
set convert-meta off
`
3\. 日志
修改%Git%\etc\profile
加上
`export LESSCHARSET=utf-8`
4\. git gui 
修改%Git%\etc\gitconfig
`[gui]
encoding = utf-8`
5。 正常显示推、拉中文修订说明
修改%Git%\etc\gitconfig
`[i18n]
commitencoding = GB2312`
6\. 分页显示
分页显示时不出现乱码，需在 GitBash 中设置属性“LESSCHARSET=utf-8”
`$export LESSCHARSET=utf-8`