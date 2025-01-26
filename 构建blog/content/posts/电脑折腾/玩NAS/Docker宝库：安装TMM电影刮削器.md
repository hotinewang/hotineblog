---
title: Docker宝库：安装TMM电影刮削器
date: 2024-10-20T01:57:00.000Z
categories:
  - 玩NAS
tags:
  - Docker
  - NAS
  - 刮削
draft: false
---
组建影音库离不开电影信息刮削，平常都是在电脑端安装TMM，但是，每次还要开电脑这么麻烦，所以，打算把TMM直接按在NAS上。

这里使用`dzhuang/tinymediamanager5-docker`版本，地址：[https://github.com/dzhuang/tinymediamanager-docker](https://github.com/dzhuang/tinymediamanager-docker )而非官方版，主要是因为：

此版本的Docker镜像体积显著小于官方Docker镜像(150M vs. 300M+，解压后230M vs. 870M)。这种精简的体积直接导致了系统资源消耗的降低，为用户提供了更高效、更流畅的体验。
建议选用TMM V3版本而非V5版本。因为V3版本完全免费，V5版本只能刮削TheMovieDb的数据，刮削其他网站数据

V3版安装命令如下：

```bash
docker run -d --name=tmm \
-v /opt/tmm/config:/config \
-v /srv/dev-disk-by-uuid-1ad0ebcf-0133-47df-af50-2cb54d10baa6/media:/media \
-e GROUP_ID=0 -e USER_ID=0 -e TZ=Asia/Shanghai \
-p 5810:5800 \
-p 5811:5900 \
dzhuang/tinymediamanager:latest
```
如果需要安装V5版，只需把最后一个latest替换成latest-v5即可

说明：

+ 将任意本地端口映射到5800以进行网页访问。
+ 将任意本地端口映射到5900以进行VNC访问。
+ 将本地卷映射到/config（存储配置数据）。`/share/Container/tinymediamanager/config`需替换为实际目录
+ 将本地卷映射到/media（访问媒体文件）。`/share/Container/tinymediamanager/media`需替换为实际目录
