---
title: NAS(OpenWRT)设置端口转发
date: 2024-10-19T20:55:00+08:00
categories: ["NAS"]
tags: ["OpenWRT","NAS","端口转发"]
draft: false
---
场景：
在城市A有台NAS-1，部署了Jellyfin服务。在城市B也有个NAS-2,并且通过ZerotireOne进行了虚拟组网，使两个NAS在一个虚拟局域网中。现在，想在城市B的智能电视安装Jellyfin，添加城市A的NAS上的Jellyfin服务。因此需要通过端口映射，使智能电视访问NAS-2的IP：端口号就能转发的NAS-1的对应IP和端口号上。

看了很多教程说是在openWRT系统菜单的``网络->防火墙->端口转发->添加``里进行设置。但是试了很多次不行。最后发现应该在Socat里进行设置（我的openWRT里已经预装了Socat）。

设么是Socat？
```
Socat（SOcket CAT）是一个功能强大的工具，用于在两个双向字节流之间建立通道，并支持多种协议和传输方式。它广泛应用于网络调试、端口转发、文件传输、串口通信等多种场景。Socat 可以被看作是 netcat（nc）的超级集，因为它不仅支持 TCP/UDP 流，还支持多种其他类型的连接，如 UNIX 域套接字、管道、文件、命名管道、串行端口、伪终端（pty）、OpenSSL 加密的套接字等。Socat 的强大之处在于它能够几乎在任意两个数据流之间创建双向通道，从而提供了极高的灵活性和强大的功能。
```
只需要在openWRT的``网络->soCat->添加``中，填入要监听的端口、目标地址和目标端口，保存并应用即可。
![设置1](/images/posts/2024/20241019_NAS（openWRT）设置端口转发1.jpg)
![设置2](/images/posts/2024/20241019_NAS（openWRT）设置端口转发2.jpg)