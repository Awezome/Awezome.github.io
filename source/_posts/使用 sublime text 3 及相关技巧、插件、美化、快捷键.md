---
title: 使用 sublime text 3 及相关技巧、插件、美化、快捷键
tags:
  - sublime
id: 1939
categories:
  - Other
date: 2014-05-07 10:17:35
---

第一次用sublime时好像还是 2 的 20xx 的某个版本，当时就是看着比 nodepad++ 漂亮，不过用了几天因为处理大文件和对编码处理不好放弃了，后来就是出了个 ConvertToUTF8 也不怎么理想。直到现在，再度拿出，按自己的需求跑了一遍，以前遇到的问题都ok了，果断把它作为默认的编辑器。

下面是一些相关技巧、插件、美化，会不定时更新。

**安装 Sublime Text 3**
```
http://www.sublimetext.com/3
```

**字体大小**
在这里修改配置preferences -&gt; settings-user
```json
"font_size": 14
```

**不记住最后打开的文件**
```json
"hot_exit": false,
"remember_open_files": false
```

**安装 Package Control**
按下 Control + ` 调出 Console
将以下代码粘贴进命令行中并回车，注意网络通畅
```json
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

**安装插件及主题**
按下 Shift + Ctrl + P 调出命令面板。
输入 install 或者 pi 调出 Package Control: Install Package 选项，按下回车。
在列表中查找相应插件按下回车进行安装。

**推荐的主题 Theme - Soda**
使用，两选一，重启sublime查看效果
```json
"theme": "Soda Light 3.sublime-theme"
"theme": "Soda Dark 3.sublime-theme" 
```

**推荐的主题 spacegray 搜索 “ spacegray ”**
使用，选一，重启sublime查看效果，color_scheme可以选别的，我用的是Obsidian
```json
"theme": "Spacegray.sublime-theme",
"color_scheme": "Packages/Theme - Spacegray/base16-ocean.dark.tmTheme"
```

```json
"theme": "Spacegray Light.sublime-theme",
"color_scheme": "Packages/Theme - Spacegray/base16-ocean.light.tmTheme"
```

```json
"theme": "Spacegray Eighties.sublime-theme",
"color_scheme": "Packages/Theme - Spacegray/base16-eighties.dark.tmTheme"
```

**推荐配色方案 Obsidian**
这有两个，一个是 Obsidian 另一个是是它孙子 Grandson-of-Obsidian ，它孙子可以直接用 PI 安装，它爷爷要手动安装。
两个地址
```
https://github.com/mekwall/obsidian-color-scheme
https://github.com/jfromaniello/Grandson-of-Obsidian
```

**推荐的插件**
ConvertToUTF8 支持UTF-8编码的插件
Emmet (Zen Coding) 快速生成HTML代码段的插件，不知道的自行google

**通用快捷键**
Ctrl+Shift+P 打开Package Control
Ctrl+P 根据文件名打开文件
Ctrl+G 定位行数
Ctrl+D 多处同步编辑

**我的最终的配置文件**
```json
{
"color_scheme": "Packages/Grandson-of-Obsidian/GrandsonOfObsidian.tmTheme",
"font_size": 14,
"hot_exit": false,
"ignored_packages":
[
"Vintage"
],
"remember_open_files": false,
"theme": "Soda Dark 3.sublime-theme"
}
```