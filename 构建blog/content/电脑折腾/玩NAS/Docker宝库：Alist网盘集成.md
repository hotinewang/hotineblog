---
title: Docker宝库：Alist网盘集成
date: 2024-11-27T15:51:00+08:00
categories:
  - NAS
tags:
  - Docker
  - NAS
  - blog
draft: false
lastmod: 2025-01-15T03:55:13.443Z
---
[Alist](https://alist.nn.ci/zh/)是一个支持多种存储的文件列表程序。包括本地存储、阿里云盘、OneDrive、Google Drive 等，且易于拓展。支持视频、音频、文档、PDF、图片预览等。

## 安装

在控制台输入以下命名来创建Docker容器：

```bash
docker run -d \
--restart=unless-stopped \
-v /etc/alist:/opt/alist/data \
-v /opt:/srv \
-p 5244:5244 \
-e PUID=0 -e PGID=0 -e UMASK=022 \
--name="alist" \
xhofe/alist:latest
```

* `-v /etc/alist:/opt/alist/data`  /etc/alist 需要修改成存放数据文件的位置
* `-v /opt:/srv` 把宿主机的opt文件夹映射到容器的srv文件夹（可选），方便把srv文件夹挂在Alist中管理。
* \`-p 5244:5244 前边的5244可以修改为想使用的端口号；右边的5244为内部端口号不能修改。\
  之后即可通过服务器IP:5244访问Alist网站。

用户名是admin，密码呢？

一是通过Portainer打开alist镜像的log文件，查看密码，如下图涂黄处\
![](../../Pasted%20image%2020241127172906.png)\
另一个方式是直接在控制台设置新密码：

```bash
docker exec -it alist ./alist admin set NEW_PASSWORD
```

* alist 是前边docker run 命令中--name参数中设置的名称
* NEW\_PASSWORD 需要替换成要设置的密码

## 配置

登录后，点击页面最下方的“管理”即可进入后台。点击后台的“存储”即可添加本地目录或网盘目录。不同的网盘添加方式不同。可以参照[官方文档-添加存储](https://alist.nn.ci/zh/guide/drivers/common.html)进行设置。

设置完毕后，点击左侧菜单最后一个“主页”可回到网盘列表。

***

**自用备份：**

```bash
docker run -d \
--restart=unless-stopped \
-v /opt/alist:/opt/alist/data \
-p 5244:5244 \
-e PUID=0 -e PGID=0 -e UMASK=022 \
--name="alist" \
xhofe/alist:latest
```
