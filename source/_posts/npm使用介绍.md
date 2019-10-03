---
title: npm使用介绍
date: 2017-06-03 09:17:45
tags:
    - npm
mp3: http://music.163.com/song/media/outer/url?id=451703096&userid=1918806314.mp3
cover: /img/ml02-knn.jpeg
---

## What

npm（全称Node Package Manager，即node包管理器）是Node.js默认的、以JavaScript编写的软件包管理系统。

## 安装

npm 是随同node.js一起安装的，所以安装node.js即可。

## 使用

```js
# 查看版本
$ npm -v

# 升级
$ sudo npm install npm -g

# 安装模块
$ npm install <Module Name>    #本地安装
# 本地安装：安装到./node_modules(命令运行目录)
$ npm install <Module Name> -g #全局安装
# 全局安装：放在 /usr/local 下或者你 node 的安装目录。

# 卸载模块
$ npm uninstall <Module Name>

# 更新模块
$ npm update <Module Name>

# 查看所有安装的模块
$ npm ls     #所有本地模块
$ npm ls -g  #所有全局模块

# 搜索模块
$ npm search <Module Name>
```