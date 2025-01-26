---
title: Docker宝库：文件管理器
date: 2022-12-01T12:00:04.000Z
categories:
  - 玩NAS
tags:
  - Docker
  - NAS
draft: false
---

文件管理器FileBrowser：如果想管理服务器中的文件，不依赖软件还是挺麻烦的。FileBrowser提供了一个网页版的文件管理器，让用户可以方便的管理服务器中的文件，而且支持文件预览、多用户管理和权限设置。
Docker安装命令非常简单，使用SSH工具连接服务器，输入以下命令：
```bash
docker run -d --name filebrowser \
-v /:/srv \
-v /opt/filebrowser/filebrowserconfig.json:/etc/config.json \
-v /opt/filebrowser/database.db:/etc/database.db \
-p 2038:80 \
filebrowser/filebrowser
```
参数解释：
+ --name filebrowser “filebrowser”是容器名称，可以自己修改
+ -v /:/srv  把服务器（宿主机）的根目录（此路径可改）挂载到容器内的/srv路径上。这样容器访问/srv目录实际上访问的是服务器的根目录。这个设置代表了FileBrowser可以管理的最顶层目录。
+ -v /opt/filebrowser/filebrowserconfig.json:/etc/config.json  原理同上，把宿主机的/opt/filebrowser/filebrowserconfig.json（此路径可改）文件挂载到容器内的/etc/config.json文件。这样做可以把配置文件保存到宿主机上，这样重启或者重建容器时，只要还绑定这个目录，就能自动读取之前的配置，免于重新设置。
+ -v /opt/filebrowser/database.db:/etc/database.db  原理同上
+ -p 2038:80  把宿主机的2038端口映射到80端口，这样访问2038端口时，实际上访问的是容器的80端口。
+ 其他都不用动

本容器默认端口号为2038，可以修改，也就是说使用的时候，直接浏览器输入主机地址或域名:2038即可。
例如：`192.168.X.X:2038` 
登录时默认用户名/密码是`admin/admin`
登录后settings中，可以将系统界面设置成中文，以及修改密码。也可以添加其他用户，并设置其他用户的访问权限。
