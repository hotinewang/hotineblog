---
title: Hugo修改发布位置
date: 2024-02-07T12:58:00.000Z
categories:
  - VPS与建站
tags:
  - Hugo
  - 博客
  - 建站
draft: false
featuredImage: /blog/电脑折腾/附件/VPS与建站/Hugo个人博客搭建.jpg
---
![Hugo个人博客搭建.jpg](/blog/电脑折腾/附件/VPS与建站/Hugo个人博客搭建.jpg)
Hugo默认发布文件是保存在Hugo文件夹下的public文件夹中，如果需要修改发布的位置，例如直接发布在github的项目文件中，可以通过修改配置文件来实现。

**打开配置文件：** 找到你的 Hugo 项目文件夹中的配置文件。默认情况下，配置文件名为 hugo.toml。

**找到发布路径设置** ：在配置文件中找到类似于 publishDir 的设置。这个设置指定了 Hugo 编译生成的静态网页的位置。如果没有publishDir，可以自己添加一个

如果你使用的是 hugo.toml，你可能会看到像下面这样的设置：

```
baseURL = "https://blog.hotine.wang/"
# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"
# 网站标题
title = "Hotine的小窝"
# 网站语言, 仅在这里 CN 大写 ["en", "zh-CN", "fr", "pl", ...]
languageCode = "zh-CN"
# 语言名称 ["English", "简体中文", "Français", "Polski", ...]
languageName = "简体中文"
```

指定publishDir的位置：

```
baseURL = "https://blog.hotine.wang/"

# 设置新的发布目录！！！！！！！！
publishDir = "../hotineblog"   #当前目录的上一级的hotineblog文件夹

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"
# 网站标题
title = "Hotine的小窝"
# 网站语言, 仅在这里 CN 大写 ["en", "zh-CN", "fr", "pl", ...]
languageCode = "zh-CN"
# 语言名称 ["English", "简体中文", "Français", "Polski", ...]
languageName = "简体中文"
```

之后，在控制台重新使用`hugo`命令发布博客即可。
