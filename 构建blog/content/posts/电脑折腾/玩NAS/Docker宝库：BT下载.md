---
title: Docker宝库：BT下载
date: 2022-12-01T12:00:06.000Z
categories:
  - 玩NAS
tags:
  - Docker
  - NAS
draft: false
---

BT下载可以用TRANSMISSION，也可以用QBITTORENT。
+ TRANSMISSION：更傻瓜化，界面更简单，但是资源不好的时候，下载速度慢。
+ QBITTORENT：设置很复杂，但是只要设置得当，下载速度很快。

## TRANSMISSION安装(功能单一，不推荐)
```bash
docker pull linuxserver/transmission
docker create --name=transmission -e PUID=0 -e PGID=0 \
-e TRANSMISSION_WEB_HOME=/combustion-release \
-e TZ=Asia/Shanghai -p 9091:9091 -p 51413:51413 -p 51413:51413/udp \
-v /opt/transmission/config:/config \
-v /path/to/your/downloadfolder:/downloads \
--restart unless-stopped linuxserver/transmission
```
参数解释：
+ --name=transmission “transmission”是容器名称，可以自己修改
+ -p 9091:9091  宿主机的9091端口（：前边的）映射到容器的9091端口（：后边那个）（如果宿主机9091端口没有被占用，可以不改）。
+ -v /opt/transmission/config:/config  把宿主机的`/opt/transmission/config`文件夹挂在到容器内的`config`文件夹，这样配置文件可以保存在宿主机上。
+ -v /path/to/your/downloadfolder:/downloads  原理同上，把宿主机的`/path/to/your/downloadfolder`（此路径可改）文件挂载到容器内的`/downloads/`文件夹。这样做可以把下载的文件存放在宿主机的`/path/to/your/downloadfolder`文件夹中。
+ 其他都不用动
因为BT下载比较伤硬盘，所以建议下载位置设置成外挂硬盘。
使用方式为直接访问主机地址+端口号，例如192.168.X.X:9091


## QBITTORENT安装（功能强大，推荐使用）
```bash
docker run -d \
--name=qbittorrent \
-e PUID=1000 \
-e PGID=1000 \
-e TZ=Asia/Shanghai \
-e WEBUI_PORT=8088 \
-p 8088:8088 \
-p 6881:6881 \
-p 6881:6881/udp \
-p 10004:8080 \
-v /opt/qbit/config:/config \
-v /path/to/your/downloadfolder:/downloads \
--restart unless-stopped \
ghcr.io/linuxserver/qbittorrent
```
参数解释：
+ --name=qbittorrent “qbittorrent”是容器名称，可以自己修改
+ -e WEBUI_PORT=8088 设置网页版下载页面的端口号为8088，这个可以按需修改，且下方的-p 8088:8088 必须保持一致
+ -v /opt/qbit/config:/config  把服务器（宿主机）的`/opt/qbit/config`挂载到容器内的`config`路径上。这样容器访问`/config`目录实际上访问的是服务器的`/opt/qbit/config`目录。
+ -v /path/to/your/downloadfolder:/downloads  原理同上，把宿主机的`/path/to/your/downloadfolder`（此路径可改）文件挂载到容器内的`/downloads`文件夹。这样做可以把下载的文件存放在宿主机的`/path/to/your/downloadfolder`文件夹中。
+ 其他都不用动

因为BT下载比较伤硬盘，所以建议下载位置设置成外挂硬盘。
默认监听端口是6881，即使想要映射监听端口，也不要监听默认端口，大多数PT站都是禁了的，用6881端口进行PT下载就会遇到以下问题：“Port 6881 is blacklisted！”，端口被黑名单了。
使用方式为直接访问主机地址+端口号，例如192.168.X.X:8088

进入管理界面后，密码可在log文件中查看（使用portainer查看很方便）需要设置Tracker才能获得较好菜单下载速度，点击最上方“工具”-“选项”-“BitTorrent”-拉倒最下方勾选“自动添加一下tracker到新的torrent”，并在下方加入Tracker，比如从以下三个网站获取的的Tracker地址：
```
https://trackerslist.com/all.txt
https://github.com/ngosang/trackerslist/blob/cd664002b9d6f1a023713bb2034925029704a522/blacklist.txt
http://github.itzmx.com/1265578519/OpenTracker/master/tracker.txt

```



## 异常处理
PS:有时候可能出现下载存储位置异常（比方说外接硬盘掉线导致挂载的路径失效），也就是说下载的内容没保存到挂载的文件夹内，而是存在了Docker容器内部。
查找文件的步骤：
+ 用`docker ps`命令查看容器ID
+ 然后用`docker exec -it 容器ID bash`（把容器ID替换成实际的ID）命令进入容器，随后可以用linux命令查看内部文件夹和文件。
+ 可用：
  + `ls`` 查看当前目录文件
  + `cd 文件夹路径` 进入文件夹
  + `rm -rf 文件/文件夹名` 强制删除（可用*代替文件名，能强制删除目录下全部文件/文件夹）
  + `exit` 退出docker容器
  + `docker cp 容器名:/容器内目录或文件路径 /要复制的目标容器外路径`把文件拷贝出来
+ 如果确实发生了此问题，则停止并删除容器
  ```
  docker stop 容器id
  docker rm 容器id
  ```
  然后重新安装容器即可。
