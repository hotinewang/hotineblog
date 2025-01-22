---
title: Docker使用指南与常用镜像
date: 2025-01-06T19:00:00+08:00
categories:
  - 玩NAS
tags:
  - Docker
  - NAS
  - blog
draft: false
lastmod: 2025-01-07T01:48:20.812Z
---
# 什么是Docker

Docker 是一种开源的容器化平台，用于帮助开发人员构建、交付和运行应用程序。通过使用Docker，开发人员可以将应用程序及其所有依赖项打包到一个称为容器的独立单元中，从而实现环境隔离、易于部署、可移植性和一致性。

简单来说就是，如果想在linux中安装一个文件管理、或者影音库等程序，如果不用Docker，需要手动敲很多命令进行配置、安装，中间过程麻烦不说，还有可能出现缺少依赖、插件版本不兼容等很多的错误。如果想删除程序，也只能手动一点一点清理，非常麻烦。而有了Docker，则可以使用别人已经打包配置好的程序镜像（相当于windows安装盘，是只读的），通过镜像来生成一个程序运行的实例（术语叫做“容器”，相当于一个轻量级虚拟机）。而且Docker容器和容器之间、以及容器和系统之间相互隔离互不影响。删除时，也只是一句代码，即可把程序删的干干净净。\
![](/%E7%94%B5%E8%84%91%E6%8A%98%E8%85%BE/%E9%99%84%E4%BB%B6/%E7%8E%A9NAS/0_Docker%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97%E4%B8%8E%E5%B8%B8%E7%94%A8%E9%95%9C%E5%83%8F.jpg)

# Docker 安装与操作

## 安装Docker

安装流程详见：[Docker宝库：Linux中Docker的安装与操作](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9ALinux%E4%B8%ADDocker%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E6%93%8D%E4%BD%9C)

## 安装可视化管理工具Portainer

Portainer是一款轻量级且功能强大的Docker可视化管理工具，其主要用途包括：创建与删除容器、查看容器信息、操作容器、进入容器、管理镜像等等。通过Portainer，可以直观地管理和监控Docker环境，降低了学习成本和操作复杂度。安装流程见：[Docker宝库：Portainer](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9APortainer)

## Docker无法拉取镜像的解决方案

Docker已经被伟大的GFW给屏蔽掉了，如何拉取镜像，有N多方案。

* 挂梯子，最方便。
* 添加其他镜像源，用于拉取镜像。[Docker添加镜像源及自行搭建方法](/Docker%E6%B7%BB%E5%8A%A0%E9%95%9C%E5%83%8F%E6%BA%90%E5%8F%8A%E8%87%AA%E8%A1%8C%E6%90%AD%E5%BB%BA%E6%96%B9%E6%B3%95.md)
* 导入其他地方提供的镜像文件，参见：[Docker镜像导出与导入](/Docker%E9%95%9C%E5%83%8F%E5%AF%BC%E5%87%BA%E4%B8%8E%E5%AF%BC%E5%85%A5)
* 在VPS上自建镜像源，参见：[Docker私有库搭建](/Docker%E7%A7%81%E6%9C%89%E5%BA%93%E6%90%AD%E5%BB%BA)

# Docker常用镜像

## 自由上网

服务器端可以使用V2ray服务端，参见：\
客户端，可以直接安装V2ray客户端，也可通过Docker或者Linux系统直接安装V2rayA等客户端。Docker客户端安装与使用，参见：[Docker宝库：V途锐客户端](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9AV%E9%80%94%E9%94%90%E5%AE%A2%E6%88%B7%E7%AB%AF)

## 网址导航类

Docker开的程序多了，端口号记不过来，就需要专门有个网址导航去记录。

最简单的办法是使用[iTab](https://go.itab.link/)之类的网站，当然如果要自建一个网址导航，也可以选用一些现有的Docker项目。

* ### SunPanel
  类似于iTab，亮点在于可以监控NAS的状态，包括CPU占用、硬盘使用情况等。而且同一个网站，支持填写内网、外网两个链接，这对使用内网穿透服务的用户非常友好。参见：[Docker宝库：SunPanel网址导航](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9ASunPanel%E7%BD%91%E5%9D%80%E5%AF%BC%E8%88%AA)
* ### Heidmall
  比较简单的网址导航，功能比较少，凑合能用。[Docker宝库：Heidmall网址导航](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9AHeidmall%E7%BD%91%E5%9D%80%E5%AF%BC%E8%88%AA)

## 资源下载类

* ### 迅雷
  迅雷的NAS端版本，可以部署在NAS中远程控制下载文件。[Docker宝库：远程迅雷](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9A%E8%BF%9C%E7%A8%8B%E8%BF%85%E9%9B%B7)

* ### 百度网盘
  百度网盘的VNC版本，部署在NAS上，可以操控下载网盘文件。[Docker宝库：百度网盘](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9A%E7%99%BE%E5%BA%A6%E7%BD%91%E7%9B%98)

* ### qBitorrent
  BT下载的网页版，名气非常大，但实际感觉没有迅雷好用。[Docker宝库：BT下载](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9ABT%E4%B8%8B%E8%BD%BD)

## 影音娱乐类

* ### Jellyfin家庭影库与刮削
  免费的家庭影音库，支持电影、电视剧、视频片段、音乐、IPTV源的展示与播放。配合刮削器，可以生成非常美观的影视墙。Jellyfin安装参见：[Docker宝库：JELLYFIN家庭影院](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9AJELLYFIN%E5%AE%B6%E5%BA%AD%E5%BD%B1%E9%99%A2)，TMM刮削器安装参见：[Docker宝库：安装TMM电影刮削器](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9A%E5%AE%89%E8%A3%85TMM%E7%94%B5%E5%BD%B1%E5%88%AE%E5%89%8A%E5%99%A8)
* ### MTPhotos家庭相册
  一个收费的（但是很便宜）国产家庭相册应用。支持相片（和视频）时间线显示、地理位置显示；支持多用户与相关权限设置；支持人像识别；支持文字查找图片等等，非常良心非常好用。[Docker宝库：好用的付费相册MT-Photos](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9A%E5%A5%BD%E7%94%A8%E7%9A%84%E4%BB%98%E8%B4%B9%E7%9B%B8%E5%86%8CMT-Photos)
* ### Navidrom私有音乐库与刮削
  可以把自己的音乐文件汇总在私有的音乐库中，并自建歌单，非常的清爽。[Docker宝库：Navidrome自建音乐库](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9ANavidrome%E8%87%AA%E5%BB%BA%E9%9F%B3%E4%B9%90%E5%BA%93.md)

## 编程

* ### VSCode
  大名鼎鼎的VSCode，随时随地写代码！[Docker宝库：VSCode](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9AVSCode)

## 智能家居

* ### HomeAssistant
  智能家居集成，支持小米、涂鸦、海尔等等一站式控制。[Docker宝库：智能家居集成HomeAssistant](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9A%E6%99%BA%E8%83%BD%E5%AE%B6%E5%B1%85%E9%9B%86%E6%88%90HomeAssistant)

## 文件管理类

* ### Alist文件管理+网盘整合
  可以把本地文件系统、各家网盘都挂在到Alist中，统一的进行访问、管理。[Docker宝库：Alist网盘集成](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9AAlist%E7%BD%91%E7%9B%98%E9%9B%86%E6%88%90.md)

* ### FileBrowser文件管理器
  一个轻量级的本地文件管理器：[Docker宝库：文件管理器](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9A%E6%96%87%E4%BB%B6%E7%AE%A1%E7%90%86%E5%99%A8)

* ### 文件同步Syncthing
  多设备文件自动同步程序。[Docker宝库：跨设备同步神器Syncthing](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9A%E8%B7%A8%E8%AE%BE%E5%A4%87%E5%90%8C%E6%AD%A5%E7%A5%9E%E5%99%A8Syncthing)

## 其他工具类

* ### Firefox
  如果需要通过VPS浏览网页，或者需要在外网中访问家中局域网的相关网页，再或者想在NAS中挂油猴脚本，可以试试Docker版的Firefox：[Docker宝库：Firefox](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9AFirefox)

* ### SSH网页版
  不安装SSH客户端，在网页也能连接SSH：[Docker宝库：SSH控制台](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9ASSH%E6%8E%A7%E5%88%B6%E5%8F%B0)

* ### 内网穿透ZerotireOne
  将多台设备拉进同一个虚拟局域网，实现互相访问：[Docker宝库：ZeroTier内网穿透](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9AZeroTier%E5%86%85%E7%BD%91%E7%A9%BF%E9%80%8F)

* ### B站视频推流kplayer
  在B站推流。[Docker宝库：B站视频推流kplayer](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9AB%E7%AB%99%E8%A7%86%E9%A2%91%E6%8E%A8%E6%B5%81kplayer)]

* ### 系统套娃
  系统套娃装Ubuntu：[Docker宝库：Linux套娃装Docker，Docker再跑Ubuntu](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9ALinux%E5%A5%97%E5%A8%83%E8%A3%85Docker%EF%BC%8CDocker%E5%86%8D%E8%B7%91Ubuntu)

* ### Docker中的openWRT
  Docker跑个openWRT当旁路由：[Docker宝库：NAS当旁路由-DOCKER中OPEN-WRT配置](/Docker%E5%AE%9D%E5%BA%93%EF%BC%9ANAS%E5%BD%93%E6%97%81%E8%B7%AF%E7%94%B1-DOCKER%E4%B8%ADOPEN-WRT%E9%85%8D%E7%BD%AE)
