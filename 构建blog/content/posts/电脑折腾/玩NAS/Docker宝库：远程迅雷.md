---
title: Docker宝库：远程迅雷
date: 2022-12-01T12:00:05.000Z
categories:
  - 玩NAS
tags:
  - Docker
  - NAS
draft: false
---

迅雷下载网页版，和PC版用起来差不多，每日支持添加3个下载连接，如果超过3个，可以使用迅雷手机app的远程下载功能添加下载。
```bash
docker run -d --name=xunlei  \
-v /opt/xunlei/data:/xunlei/data \
-v /path/to/your/downloadfolder:/xunlei/downloads  \
-p 2345:2345 \
-e XL_UID=1000 \
-e XL_GID=1000 \
--privileged \
--restart=unless-stopped \
cnk3x/xunlei:latest

```
参数解释：
+ --name=xunlei “xunlei”是容器名称，可以自己修改
+ -v /opt/xunlei/data:/xunlei/data  把服务器（宿主机）的`/opt/xunlei/data`挂载到容器内的`/xunlei/data`路径上。这样容器访问`/xunlei/data`目录实际上访问的是服务器的`/opt/xunlei/data`目录。
+ -v /path/to/your/downloadfolder:/xunlei/downloads  原理同上，把宿主机的`/path/to/your/downloadfolder`（此路径可改）文件挂载到容器内的`/xunlei/downloads`文件夹。这样做可以把下载的文件存放在宿主机的`/path/to/your/downloadfolder`文件夹中。
+ 其他都不用动

使用方式为直接访问主机地址+端口号，例如192.168.X.X:2345
如需邀请码，输入：迅雷牛通

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

<!--自用
docker run -d \
--name=xunlei  \
-v /opt/xunlei/data:/xunlei/data \
-v /srv/dev-disk-by-uuid-1ad0ebcf-0133-47df-af50-2cb54d10baa6/downloads:/xunlei/downloads  \
-p 2350:2345 \
--privileged \
--restart=unless-stopped \
cnk3x/xunlei:latest
-->
