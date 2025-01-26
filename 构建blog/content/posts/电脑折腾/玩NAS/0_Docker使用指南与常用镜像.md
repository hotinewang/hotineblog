---
title: Docker使用指南与常用镜像
date: 2025-01-06T11:00:00.000Z
categories:
  - 玩NAS
tags:
  - Docker
  - NAS
draft: false
featuredImage: /blog/电脑折腾/附件/玩NAS/0_Docker使用指南与常用镜像.jpg
---
# 什么是Docker

Docker 是一种开源的容器化平台，用于帮助开发人员构建、交付和运行应用程序。通过使用Docker，开发人员可以将应用程序及其所有依赖项打包到一个称为容器的独立单元中，从而实现环境隔离、易于部署、可移植性和一致性。

简单来说就是，如果想在linux中安装一个文件管理、或者影音库等程序，如果不用Docker，需要手动敲很多命令进行配置、安装，中间过程麻烦不说，还有可能出现缺少依赖、插件版本不兼容等很多的错误。如果想删除程序，也只能手动一点一点清理，非常麻烦。而有了Docker，则可以使用别人已经打包配置好的程序镜像（相当于windows安装盘，是只读的），通过镜像来生成一个程序运行的实例（术语叫做“容器”，相当于一个轻量级虚拟机）。而且Docker容器和容器之间、以及容器和系统之间相互隔离互不影响。删除时，也只是一句代码，即可把程序删的干干净净。

![0_Docker使用指南与常用镜像.jpg](/blog/电脑折腾/附件/玩NAS/0_Docker使用指南与常用镜像.jpg)
# Docker 安装与操作

## 安装Docker

安装流程详见：[Docker宝库：Linux中Docker的安装与操作](/posts/电脑折腾/玩nas/docker宝库linux中docker的安装与操作)
## 安装可视化管理工具Portainer

Portainer是一款轻量级且功能强大的Docker可视化管理工具，其主要用途包括：创建与删除容器、查看容器信息、操作容器、进入容器、管理镜像等等。通过Portainer，可以直观地管理和监控Docker环境，降低了学习成本和操作复杂度。安装流程见：[Docker宝库：Portainer](/posts/电脑折腾/玩nas/docker宝库portainer)

## Docker无法拉取镜像的解决方案
Docker已经被伟大的GFW给屏蔽掉了，如何拉取镜像，有N多方案。
- 挂梯子，最方便。
- 添加其他镜像源，用于拉取镜像。[Docker添加镜像源及自行搭建方法](/posts/电脑折腾/玩nas/docker添加镜像源及自行搭建方法)
- 导入其他地方提供的镜像文件，参见：[Docker镜像导出与导入](/posts/电脑折腾/玩nas/docker镜像导出与导入)
- 在VPS上自建镜像源，参见：[Docker私有库搭建](/posts/电脑折腾/玩nas/docker私有库搭建)

# Docker常用镜像

## 自由上网

服务器端可以使用V2ray服务端，参见：
客户端，可以直接安装V2ray客户端，也可通过Docker或者Linux系统直接安装V2rayA等客户端。Docker客户端安装与使用，参见：[Docker宝库：V途锐客户端](/posts/电脑折腾/玩nas/docker宝库v途锐客户端)

## 网址导航类

Docker开的程序多了，端口号记不过来，就需要专门有个网址导航去记录。

最简单的办法是使用[iTab](https://go.itab.link/)之类的网站，当然如果要自建一个网址导航，也可以选用一些现有的Docker项目。
- ### SunPanel
  类似于iTab，亮点在于可以监控NAS的状态，包括CPU占用、硬盘使用情况等。而且同一个网站，支持填写内网、外网两个链接，这对使用内网穿透服务的用户非常友好。参见：[Docker宝库：SunPanel网址导航](/posts/电脑折腾/玩nas/docker宝库sunpanel网址导航)
- ### Heidmall
  比较简单的网址导航，功能比较少，凑合能用。[Docker宝库：Heidmall网址导航](/posts/电脑折腾/玩nas/docker宝库heidmall网址导航)

## 资源下载类

- ### 迅雷
  迅雷的NAS端版本，可以部署在NAS中远程控制下载文件。[Docker宝库：远程迅雷](/posts/电脑折腾/玩nas/docker宝库远程迅雷)

- ### 百度网盘
  百度网盘的VNC版本，部署在NAS上，可以操控下载网盘文件。[Docker宝库：百度网盘](/posts/电脑折腾/玩nas/docker宝库百度网盘)

- ### qBitorrent
  BT下载的网页版，名气非常大，但实际感觉没有迅雷好用。[Docker宝库：BT下载](/posts/电脑折腾/玩nas/docker宝库bt下载)

## 影音娱乐类

- ### Jellyfin家庭影库与刮削
  免费的家庭影音库，支持电影、电视剧、视频片段、音乐、IPTV源的展示与播放。配合刮削器，可以生成非常美观的影视墙。Jellyfin安装参见：[Docker宝库：JELLYFIN家庭影院](/posts/电脑折腾/玩nas/docker宝库jellyfin家庭影院)，TMM刮削器安装参见：[Docker宝库：安装TMM电影刮削器](/posts/电脑折腾/玩nas/docker宝库安装tmm电影刮削器)
- ### MTPhotos家庭相册
  一个收费的（但是很便宜）国产家庭相册应用。支持相片（和视频）时间线显示、地理位置显示；支持多用户与相关权限设置；支持人像识别；支持文字查找图片等等，非常良心非常好用。[Docker宝库：好用的付费相册MT-Photos](/posts/电脑折腾/玩nas/docker宝库好用的付费相册mt-photos)
- ### Navidrom私有音乐库与刮削
  可以把自己的音乐文件汇总在私有的音乐库中，并自建歌单，非常的清爽。[Docker宝库：Navidrome自建音乐库](/posts/电脑折腾/玩nas/docker宝库navidrome自建音乐库)

## 编程

- ### VSCode
  大名鼎鼎的VSCode，随时随地写代码！[Docker宝库：VSCode](/posts/电脑折腾/玩nas/docker宝库vscode)

## 智能家居

- ### HomeAssistant
  智能家居集成，支持小米、涂鸦、海尔等等一站式控制。[Docker宝库：智能家居集成HomeAssistant](/posts/电脑折腾/玩nas/docker宝库智能家居集成homeassistant)

## 文件管理类

- ### Alist文件管理+网盘整合
  可以把本地文件系统、各家网盘都挂在到Alist中，统一的进行访问、管理。[Docker宝库：Alist网盘集成](/posts/电脑折腾/玩nas/docker宝库alist网盘集成)

- ### FileBrowser文件管理器
  一个轻量级的本地文件管理器：[Docker宝库：文件管理器](/posts/电脑折腾/玩nas/docker宝库文件管理器)
  
- ### 文件同步Syncthing
  多设备文件自动同步程序。[Docker宝库：跨设备同步神器Syncthing](/posts/电脑折腾/玩nas/docker宝库跨设备同步神器syncthing)

## 其他工具类

- ### Firefox
  如果需要通过VPS浏览网页，或者需要在外网中访问家中局域网的相关网页，再或者想在NAS中挂油猴脚本，可以试试Docker版的Firefox：[Docker宝库：Firefox](/posts/电脑折腾/玩nas/docker宝库firefox)
  
- ### SSH网页版
  不安装SSH客户端，在网页也能连接SSH：[Docker宝库：SSH控制台](/posts/电脑折腾/玩nas/docker宝库ssh控制台)

- ### 内网穿透ZerotireOne
  将多台设备拉进同一个虚拟局域网，实现互相访问：[Docker宝库：ZeroTier内网穿透](/posts/电脑折腾/玩nas/docker宝库zerotier内网穿透)

- ### B站视频推流kplayer
  在B站推流。[Docker宝库：B站视频推流kplayer](/posts/电脑折腾/玩nas/docker宝库b站视频推流kplayer)

- ### 系统套娃
  系统套娃装Ubuntu：[Docker宝库：Linux套娃装Docker，Docker再跑Ubuntu](/posts/电脑折腾/玩nas/docker宝库linux套娃装dockerdocker再跑ubuntu)
  
- ### Docker中的openWRT
  Docker跑个openWRT当旁路由：[Docker宝库：NAS当旁路由-DOCKER中OPEN-WRT配置](/posts/电脑折腾/玩nas/docker宝库nas当旁路由-docker中open-wrt配置)
