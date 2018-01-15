---
title: 'React Native : Mac 下 Android 环境及IDE配置'
tags:
  - react
  - react-native
id: 2584
categories:
  - React
date: 2016-06-09 14:13:46
---

一时感兴趣，想玩一把React Native，花了好几个时间搭环境，主要坑在网速上，简单做下总结。

### 基本工具

Mac开发的话首先要有Homebrew和Node.js环境，这个安装教程很多就不多说了。

```bash
brew install watchman
brew install Flow
```

*针对iOS开发，我们只需要安装Xcode 7.0或者7.0以后版本，该可以通过App Store进行下载安装

### For Android

安装最新版的JDK 1.8 javac -version
安装Genymotion android模拟器，https://www.genymotion.com/

安装Android SDK: 当然也可以安装Android Studio来进行安装，不过Android Studio不是必须的。http://www.androiddevtools.cn/这个地方下载现成的包也行。
```bash
brew install android-sdk
```

Android SDK在线更新镜像服务器更换源，不然各种慢
南阳理工学院镜像服务器地址:mirror.nyist.edu.cn 端口：80
腾讯Bugly 镜像:android-mirror.bugly.qq.com 端口：8080

![](http://www.androiddevtools.cn/static/image/sdk-manager-proxy-settings.png)


安装SDK
Android SDK Build-tools version 23.0.1 版本严格匹配23.0.1
Android 6.0 (API 23)
Local Maven repository for Support Libraries(之前叫做Android Support Repository)

![QQ20160609-1@2x](https://awezome.net/wp-content/uploads/2016/06/QQ20160609-1@2x.png)

添加环境变量
~/.bashrc, ~/.bash_profile 或者 ~/.zshrc
```
PATH="/usr/local/Cellar/android-sdk/24.4.1_1/tools:/usr/local/Cellar/android-sdk/24.4.1_1/platform-tools:${PATH}"
export ANDROID_HOME=/usr/local/Cellar/android-sdk/24.4.1_1/
```

### React Native安装

vim ~/.npmrc 加入下面内容,不然各种慢
```
registry = https://registry.npm.taobao.org
```

React Native安装
```
npm install -g react-native-cli
```

建立工程
```
react-native init AwesomeProject
```

### IDE

开发工具有sublime,webstorm以及官网推荐的Nuclide，不过我喜欢Jetbrains系统，平时也用PHPStrom，这个和webstorm一样。打开AwesomeProject工程后改下JavaScript版本
```
Languages&Frameworks -> JavaScript -> JavaScript language version -> Harmony
```
同时也可装个提示插件
https://github.com/virtoolswebplayer/ReactNative-LiveTemplate
如果用sublime的话推荐安装几个插件：JSX、React Teamplate、react-native-snippets、ReactJS Snippets、SublimeLinter-jsxhint、Babel、Babel Snippets、Emmet、React ES6-Snippets，这几个插件可以支持React的JSX语法，并进行高亮提示，同时也可进行排版