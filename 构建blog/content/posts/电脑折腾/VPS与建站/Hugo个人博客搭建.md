---
title: Hugo个人博客搭建
date: 2023-08-17T08:11:23.000Z
categories:
  - VPS与建站
tags:
  - Hugo
  - 博客
  - 建站
featuredImage: /blog/电脑折腾/附件/VPS与建站/Hugo个人博客搭建.jpg
---
![Hugo个人博客搭建.jpg](/blog/电脑折腾/附件/VPS与建站/Hugo个人博客搭建.jpg)
## 前言

一直想弄一个轻量级的博客，操作简单，便于迁移。试过wordpress和自建静态博客，最后发现还有hugo这类好东西。整体对比起来就是：

+ wordpress虽然很强大，但也比较复杂，需要php和sqlserver,对于我这种不想太折腾的人来说，部署太过麻烦。而且迁移也让人头大，或者说如果有一天我不玩vps了，难道要在自己电脑上弄一套wordpress才能建站？
+ 自建静态博客就是用现成的静态网页模板修改里边的内容。虽然好看，方便部署，而且在本地直接打开html文件也能看，迁移性好一些，但是如果想换模板，那就是噩梦。而且写文章时，需要修改html代码，也挺麻烦。
+ 突然有一天，发现了Hugo这类静态博客生成器，打开了博客建站新世界。原始博文使用markdown文档保存，类似于txx文本文档，简单的几个语法即可实现排版，兼容性非常好，也方便迁移。Hugo的安装也简单，主题设置也简单，生成网站速度快，而且网站是静态的，部署成也本低，直接扔到github pages这类免费网页托管上就行。简直是我这种轻量级博主的福音。

## 开始安装配置Hugo
### Windows系统下
在hugo官网[gohugo.io](http://gohugo.io)有全部的文档教程。
+ 下载hugo∶在github上[gohugoio/hugo](https://github.com/gohugoio/hugo)下载最新版Hugo程序(下载地址在页面右侧的Releases处)。注意需要下载extended版，因为非extended版的很多主题都不支持使用。下载后解压，可以把hugo.exe程序放在c∶/program files/hugo/hugo.exe的位置。
+ 添加环境变量，方便使用Hugo。从开始菜单的搜索中搜索“编辑系统环境变量”并打开窗口，或者在“此电脑”上点右键，选择“属性”-“高级系统设置”。之后切换到“高级”页签，点击右下方“环境变量...”。此时会有“xxx的用户变量”和“系统变量”。区别是前者只能让当前用户使用Hugo，后者能让此电脑的所有用户都是用Hugo。
  + 比方说选择了“系统变量”，则在系统变量下边找到PATH并双击，再弹出的窗口中点“新建”，输入放置Hugo.exe的路径，例如刚才的c∶/program files/hugo/，点确定关闭设置PATH的窗口，再点确定关闭设置环境变量的窗口，再点确定使设置生效。
+ 测试环境变量是否生效：再任意位置打开CMD或者WindowsPowerShell，输入`hugo version`或者`hugo -help`查看hugo是否能运行。正常运行的话会输出hugo的版本号或者帮助说明。
### Debian系统下
分别输入以下3个命令。分别是更新软件包列表、更新已安装的软件包、安装hugo
```bash
sudo apt update

sudo apt upgrade

sudo apt install hugo
```

## 开始建站
+ 首先新建一个博客，打开CMD或者WindowsPowerShell，用cd命令定位到要建站的目录，比方说:
    ```cd d:/```
    开始创建网站：
    ```hugo new site myblog ```
    myblog可以替换成自己想用的文件夹名（也是默认的网站名）。之后定位到网站文件夹内部(把下边命令的myblog替换成自己网站文件夹的名称)：
    ```cd myblog```
+ 创建第一篇文章：再Powershell中定位到网站目录里，使用`hugo new posts/文章名.md`命令在content/posts目录下生成一个"文章名.md"的博客markdown文章。打开文件，可以进行修改编辑。注意文件头的格式，建议修改成：
    ```
    ---
    title: 文章主题
    date: 2023-08-17T16:11:23+08:00
    categories: [
        "这里是文章分类",
        "多个分类可用引文逗号隔开"
    ]
    tags: [
    "这里是文章标签",
    "多个标签可用逗号隔开",
    "就像这样"
    ]
    draft: true
    ---
    ```
    PS：当draft为false时，视文章为草稿，默认不会生成网页。
+ 下载一个主题：Hugo默认是没有任何主题的。去官[网主题板块](https://themes.gohugo.io/)找一款主题，然后下载，解压放在网站目录里的themes文件夹下。根据主题的说明文档，对网站目录的hugo.toml文件（网站设置文件）进行修改。
+ 生成预览网站：执行命令`hugo server -D --disableFastRender`，会提示通过http://localhost:1313/来预览网站。此时，再对文章的md文件进行修改，保存后网站内容会同步刷新，十分方便。
  + 命令中的`-D`是指包括草稿（draft）在内的所有页面都会被渲染。
  + 命令中的`--disableFastRender`是禁用快速渲染，这可能会更好地适应一些更复杂的主题和网站结构。
+ 生成正式网站：执行`hugo`命令，网站将根据配置和内容生成到网站目录中的 public 文件夹中。
+ 发布网站：把public文件夹中的全部文件复制到服务器中，或使用github同步到githubpages中，及完成了网站的发布。可以输入网址进行访问了。

