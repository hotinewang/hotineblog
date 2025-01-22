---
title: Docker宝库：SunPanel网址导航
date: 2024-10-27T20:00:01+08:00
categories:
  - NAS
tags:
  - Docker
  - NAS
  - blog
  - 网址导航
draft: false
lastmod: 2025-01-07T01:48:20.875Z
---
Docker开的程序多了，端口号记不过来，就需要专门有个网址导航去记录。

SunPanel比Heidmall好用100倍！![../附件/玩NAS/Docker宝库：SunPanel网址导航.png](/%E7%94%B5%E8%84%91%E6%8A%98%E8%85%BE/%E9%99%84%E4%BB%B6/%E7%8E%A9NAS/Docker%E5%AE%9D%E5%BA%93%EF%BC%9ASunPanel%E7%BD%91%E5%9D%80%E5%AF%BC%E8%88%AA.png)

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

* -v /srv:/srv SunPanel可以显示磁盘使用情况，挂载这个srv目录主要是为了在容器内获得挂载的磁盘的信息。
