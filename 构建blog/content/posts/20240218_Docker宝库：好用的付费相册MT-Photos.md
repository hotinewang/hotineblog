---
title: Docker宝库：好用的付费相册MT-Photos
date: 2024-02-18T12:20:00+08:00
categories: ["NAS"]
tags: ["Docker","相册","NAS"]
draft: false
---

# Docker宝库：好用的付费相册MT-Photos

## 简介

**MT Photos**（付费）是一款为NAS用户量身打造的照片管理系统。可自动将照片整理、分类，包括但不限于时间、地点、人物、照片类型。
同时也提供网页、安卓、苹果app，可以方便的管理服务器上的照片，也可以设置备份手机端相册。

**官网上描述的功能特性包括：**

* 以**时间线** 的方式，展示您的照片
* 以**文件夹** 的方式，展示您的照片
* 通过**图库** 您可以方便地将现有存储中的照片，提供给用户
* 手机App**备份照片、视频支持丰富的命名规则** （按年月分文件夹、按拍摄日期命名文件等）；
* 通过人脸识别，自动生成**人物相册**
* 基于照片的GPS信息，自动生成**地点相册**
* 支持**Live Photos** 的无损备份、下载、显示
* 按**文件类型** ，自动分类屏幕截图、自拍照、视频、动态照片
* 基于照片内容，自动生成**场景相册**
* 通过**影集分享** ，您可以方便地将照片分享给家人
* **图片、视频查重**

**使用费用为：**

* 免费试用一个月
* 3元/月    25元/年    99元/终身

## mtphotos安装流程

### 1. 准备工作

确保您的系统上已经安装了Docker。如果还没有安装，您可以访问Docker官网下载并安装适合您操作系统的Docker版本。

### 2. 确定必要的目录

在您的系统中创建并确定三个目录：用于存放配置文件（以及缓存文件）、上传/备份的照片以及照片库（可以是多个）。这些目录需要根据您的实际路径进行调整。

```bash
mkdir -p /opt/mtphotos/config
mkdir -p /path/to/Backups/Photos
mkdir -p /path/to/PhotoAlbum
mkdir -p /path/to/PhotoAlbum2
```

### 3. 运行Docker容器

使用以下Docker命令来启动mtphotos容器。这个命令会创建一个名为mt-photos的容器，并将您的本地目录挂载到容器内部，同时设置时区为中国上海时间，并在容器停止时自动重启。

```bash
docker run -d \
--name="mt-photos" \
-v /opt/mtphotos/config:/config \
-v /path/to/Backups/Photos:/upload \
-v /path/to/PhotoAlbum:/photoalbum \
-v /path/to/PhotoAlbum2:/photoalbum2 \
-p 8063:8063 \
-e TZ="Asia/Shanghai" \
--restart="unless-stopped" \
mtphotos/mt-photos:latest
```

代码解释：

* `-d`：后台运行容器。
* `--name="mt-photos"`：给容器命名为mt-photos（可自行修改）。
* `-v /opt/mtphotos/config:/config`：将本地的/opt/mtphotos/config目录挂载到容器的/config目录,默认情况下，这个目录中会有一个cache文件夹，用于存放缩略图等文件可能会比较大，可以通过环境变量CACHE_DIR_PATH来单独指定cache的存放位置：`-e CACHE_DIR_PATH="/photoalbum/cache/"``* -v /path/to/Backups/Photos:/upload`：将本地的/path/to/Backups/Photos目录挂载到容器的/upload目录，用于存放上传的照片。
* `-v /path/to/PhotoAlbum:/photoalbum`：将本地的/path/to/PhotoAlbum目录挂载到容器的/photoalbum目录，用于指定一个照片库。容器内的目录名可以自定义。
* `-v /path/to/PhotoAlbum2:/photoalbum2`：将本地的/path/to/PhotoAlbum2目录挂载到容器的/photoalbum2目录，用于指定另一个照片库。容器内的目录名可以自定义。
* `-p 8063:8063`：将容器的8063端口映射到宿主机的8063端口。
* `-e TZ="Asia/Shanghai"`：设置容器的时区为上海时间。
* `--restart="unless-stopped"`：除非手动停止，否则容器在退出时会自动重启。
* `mtphotos/mt-photos:latest`：指定要运行的Docker镜像和标签。

## 访问与配置mtphotos

启动容器后，您可以通过浏览器访问http://localhost:8063来查看和使用mtphotos。您可能需要根据实际情况调整端口号。

在浏览器中访问mtphotos后，您可能需要进行一些基本的配置，比如设置用户权限、相册等。请根据mtphotos的界面提示进行操作。

## 进阶设置

* 可以在系统配置里，根据需求，按提示增加智能识别API和GPS信息识别，便于对照片进行归档整理。
* 人脸识别在nas中处理会比较慢，大批量处理照片时可以在系统配置里直接关闭人脸识别选项，在需要使用时，在系统配置-人脸识别（保持关闭）-辅助处理浏览器端。使用手机或电脑的浏览器进行辅助处理，速度很快。
