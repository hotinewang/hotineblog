---
title: Docker宝库：SunPanel网址导航
date: 2024-10-27T12:00:01.000Z
categories:
  - 玩NAS
tags:
  - Docker
  - NAS
  - 网址导航
draft: false
featuredImage: /blog/电脑折腾/附件/玩NAS/Docker宝库：SunPanel网址导航.png
---
Docker开的程序多了，端口号记不过来，就需要专门有个网址导航去记录。

SunPanel比Heidmall好用100倍！

最大的亮点是，这个网址导航支持内外网切换，也就是说同一个网站可以填写两个网址，一个内网用，一个外网用。

而且还可以显示NAS的CPU、内存、磁盘使用信息。

```bash
docker run -d --restart=always -p 3002:3002 \
-v /opt/sun-panel/conf:/app/conf \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /srv:/srv  \
--name sun-panel \
hslr/sun-panel:latest
```
+ -v /srv:/srv SunPanel可以显示磁盘使用情况，挂载这个srv目录主要是为了在容器内获得挂载的磁盘的信息。

![Docker宝库：SunPanel网址导航.png](/blog/电脑折腾/附件/玩NAS/Docker宝库：SunPanel网址导航.png)
