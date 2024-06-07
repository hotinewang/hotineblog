---
title: Docker宝库：JELLYFIN家庭影院
date: 2022-12-01T20:00:02+08:00
categories: [
  "NAS"
]
tags: [
  "Docker",
  "NAS"
]
draft: false
---

家里的电影比较多，想搭一个个人影音库，让每个电影都有专属的封面、介绍、评分等等，更重要的是，只要在同一网络下的用户，都可以直接访问，实现全家共享。
Docker安装命令非常简单，使用SSH工具连接服务器，输入以下命令：
```bash
docker run -d \
--name=jellyfin \
-e PUID=1000 \
-e PGID=1000 \
-e TZ=Asia/Shanghai \
-p 8096:8096 \
-p 8920:8920 `#optional` \
-p 7359:7359/udp `#optional` \
-p 1900:1900/udp `#optional` \
-v /opt/jellyfin/config:/config \
-v /path/to/your/media:/data/media \
--restart unless-stopped \
nyanmisaka/jellyfin:latest
```
原版镜像是（不建议用，对中文字符支持不好）：
```
lscr.io/linuxserver/jellyfin:latest
```
参数解释：
+ -p 8096:8096 宿主机的8096端口（：前边的）映射到容器的8096端口（：后边那个）（如果宿主机8096端口没有被占用，可以不改）
+ -v /opt/jellyfin/config:/config  挂载服务器路径/opt/jellyfin/config（这个路径可修改）到容器内/config路径，也就是说容器内的配置文件实际上是保存到服务器的/opt/Heimdall/config中的。
+ -v /path/to/your/media:/data/media  把服务器上的/path/to/your/media（影视资源存放的实际路径，需要根据实际情况修改）挂载到容器的/data/media目录（media目录下可以再建立电影、剧集等子文件夹），这样容器可以正常的访问影视资源。
+ -v /path/to/your/tvshows:/data/tvshows （电视剧存放的实际路径，需要根据实际情况修改）挂载到容器的/data/tvshows目录，这样容器可以正常的访问电视剧资源。如若没有电视剧，这一条参数可以直接省略不写。
+ nyanmisaka/jellyfin:latest 是中国特供版镜像，原版镜像对中文字体支持不好，影集封面中文会出现很多方框、字幕显示也会有问题。
+ 其他都不用动

本容器默认端口号为8096，也就是说使用的时候，直接浏览器输入主机地址或域名:8096即可。
例如：`192.168.X.X:8096` 
这样家庭影音库就搭建好了，登录进入根据提示进行设置即可。
需要**注意**的是，在影音库中添加视频库时，输入的路径应该是容器内路径，比方说电影路径是`/data/media`而不是电影在服务器上的路径。因为容器是没有办法直接访问容器外部的资源。
另外，进入影音库会发现电影都没有海报和简介，那是因为能刮削海报和简介等信息的网站都被墙掉了。因此，可以下载Tiny Media Manager进行刮削。


### 后记：适用于N100 CPU的硬件解码设置(2024-6-6)
1. 开启集成显卡：在主机控制台输入`modprobe i915`并回车（不会有回显信息），加载名为 i915 的内核模块（i915 是 Intel 的集成显卡驱动模块，用于支持 Intel 的集成图形硬件。当你执行 modprobe i915 命令时，系统会加载这个模块，从而允许你的 Linux 系统使用 Intel 集成显卡的功能。）
之后输入`ls /dev/dri`并回车，如果显示有renderD128则说明加载成功。（每次重启主机后都需要重复这个步骤）
2. 配置Jellyfin硬解：通过以下docker run命令创建jellyfin镜像
```bash
docker run -d \
  --name=jellyfin \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Shanghai \
  -p 8096:8096 \
  --device /dev/dri:/dev/dri `#区别在这里`\
  -v /opt/jellyfin/config:/config \
  -v /path/to/your/movies:/data/movies \
  --restart unless-stopped \
  nyanmisaka/jellyfin:latest
```
之后访问Jellyfin，在 管理-控制台-播放-转码 中，硬件加速选择“Video Acceleration API(VAAPI)”,下边的VA-API设备会显示`/dev/dri/renderD128`,启用硬件解码下方的选项全部勾选，启用 VPP 色调映射也勾选上，保存即可。