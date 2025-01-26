---
title: ZeroTier内网穿透教程
date: 2023-08-17T13:20:59.000Z
categories:
  - 玩NAS
tags:
  - ZeroTier
  - Docker
  - 内网穿透
  - NAS
  - 建站
  - 反向代理
featuredImage: /blog/电脑折腾/附件/玩NAS/Docker宝库：ZeroTier内网穿透.jpg
---

## ZeroTier内网穿透简介

家有NAS，如何从外网进行访问或管理，这就需要使用内网穿透。对于家里宽带没有公网IP的人来说，用ZeroTier是个不错的选择。当然，能实现网穿透的还有frp，但是frp需要一个服务端（比方说一个VPS），而ZeroTier则不需要自己拥有服务端。

ZeroTier简单来说，就是通过这个软件，把各个设备接入一个虚拟局域网中，并分配给每个设备一个（或多个）局域网IP，从而实现设备间的互联。

- 优点：安装和设置简单，不需要过多的配置和知识。
- 缺点：需要相关设备下载ZeroTier，网速和稳定性取决于ZeroTier的服务，也可哪天就不能用了。

![Docker宝库：ZeroTier内网穿透.jpg](/blog/电脑折腾/附件/玩NAS/Docker宝库：ZeroTier内网穿透.jpg)

## 开始搭建

+ 注册ZeroTier账号，访问[zerotier.com](https://www.zerotier.com/),正常注册就好。
+ 登录后创建一个网络（虚拟局域网），点击“Create A Network”，下方就会出现一条新的记录，点进入进行设置。
 + Network ID：网络ID，这个很重要，是加入这个网络所必须用到的ID
 + Name和Description：可随意修改，就是这个虚拟局域网的名称和简介（可不填）
 + Access Control：选择Private比较安全，每个加入这个局域网的设备，都需要通过这个网页进行确认批准。而选择Public，则会使每个知道NetworkID的人，都能随意接入这个局域网。
 + 其他设置保持默认即可。

## 设备加入网络
+ windows设备：在[官网下载](https://www.zerotier.com/download/)并安装windows客户端，安装完毕并运行后，在桌面右下角托盘区找到程序图标，点击并选择“Join New Network”（加入新网络），并输入之前在ZeroTier网站创建的网络的NetworkID即可。
+ 安卓设备：在谷歌市场下载APP：ZeroTier One，进入之后，在APP主页面点击“Add Network”,输入NetworkID，其他保持默认，点击最下方“Add”即可。回到主页面，右上方“Settings”中，可勾选“Use Mobile Data”，这样可以在移动网络下访问局域网设备（不勾选则只能在Wifi网络中访问局域网）。最后，回到主页面，点击刚刚新建的网络的右方的开关，即可连入局域网。
+ Docker：如果是NAS设备（Linux系统）想接入局域网，可以安装Docker版的ZeroTier。用SSH工具接入已装好Docker的NAS设备的控制台后，输入以下命令安装ZeroTier镜像：
  ```bash
  docker run -d \
  --name ZeroTier \
  --restart=always \
  --device=/dev/net/tun \
  --net=host \
  --cap-add=NET_ADMIN \
  --cap-add=SYS_ADMIN \
  zerotier/zerotier-synology:latest
  ```
  再输入以下命令加入局域网络：
  ```bash
  docker exec -it ZeroTier zerotier-cli join 233ccaac272dfa91
  ```
  注意把join后边的那串文字替换成自己网络的NetworkID
  
+ 批准设备入网：当有新设备加入网络中后，需要在[zerotier.com](https://www.zerotier.com/)中对应网络的设置页面进行批准。在设置页面往下拉，在Members一节中，会出现新申请入网的设备。在这里勾选这条记录，即可准入网络。此处可以看到自动分配给这个设备的局域网IP，这个很重要，是设备间互相访问所使用的IP。也可以在这里修改设备的备注名称和描述。

## 互相访问
此时，已可以实现各设备之间的互联。比如家中设备的IP是192.168.122.1,在8088端口运行了一个网站。则在已加入这个虚拟局域网的外网设备中，可以通过192.168.122.1:8088访问到家中的网站。访问家中FTP或者SMB服务也是同理。

## 进阶优化
上边的方案，需要每一个设备都加入虚拟局域网才能实现互相访问。自己用比较方便，但是如果想把内容分享给其他人，或者临时使用公共设备，再或者想用家里设备作为服务器工给外网访问，这就不方便了。

如果想解决上述问题，需要拥有一个VPS（虚拟主机，有公网IP），通过反向代理，来实现。

大致原理就是：把家中设备和VPS加入同一个局域网，在VPS上设置反向代理，用户想要访问家中设备上的内容时，由VPS充当中转站，负责用户与家中设备的通信。

开始动手：

+ 把VPS和家中服务器接入局域网，实现内网穿透。参照上文教程即可。一般VPS都是Linux系统，可以通过安装Docker版ZeroTier实现。
+ VPS上设置反向代理。利用Caddy服务，或者Nginx服务进行反向代理设置。（详见Caddy相关博客，也可网络搜索教程）[Caddy建站指南](/posts/电脑折腾/vps与建站/caddy建站指南)
+ 之后只要通过设置好的域名，便可以直接访问家中服务器。
