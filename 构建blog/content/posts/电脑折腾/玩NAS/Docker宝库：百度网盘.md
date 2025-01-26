---
title: Docker宝库：百度网盘
date: 2022-12-01T12:00:06.000Z
categories:
  - 玩NAS
tags:
  - Docker
  - NAS
draft: false
---
百度网盘资源的远程下载，可以登录会员，且能长期保持登录状态。

2024年1月22日更新-发现一个更好用的百度网盘：
```
docker run -d \
--name=baidunetdisk \
-p 15811:5800 \
-p 15911:5900 \
-v /opt/baidunetdisk/config:/config \
-v /path/to/your/downloadfolder:/config/downloads \
-e VNC_PASSWORD=yourpassword \
--restart unless-stopped \
johngong/baidunetdisk:latest
```
参数解释：
+ --name=baidunetdisk baidunetdisk是容器名称，可以自己修改
+ -p 15811:5800  宿主机的15811端口（：前边的）映射到容器的5800端口（：后边那个）（如果宿主机15811端口没有被占用，可以不改）。
+ -v /path/to/your/downloadfolder:/config/downloads \  把宿主机的`/path/to/your/downloadfolder`（此路径可改）文件挂载到容器内的`/config/downloads`文件夹。这样做可以把下载的文件存放在宿主机的`/path/to/your/downloadfolder`文件夹中。在首次下载时，会提示选择保存位置，选择保存到`/config/downloads`即可
+ -e VNC_PASSWORD=yourpassword yourpassword改成进入网盘界面前的验证密码，随便设。
+ 其他都不用动

使用方式为直接访问主机地址+端口号，例如192.168.X.X:15811

原文：
```bash
docker pull johnshine/baidunetdisk-crossover-vnc:latest
docker run -d -p 6080:6080 \
--name=BaiduNetDisk \
-v /path/to/your/downloadfolder:/home/baidu/baidunetdiskdownload/ \
johnshine/baidunetdisk-crossover-vnc:latest
```

参数解释：
+ --name=BaiduNetDisk “BaiduNetDisk”是容器名称，可以自己修改
+ -p 6080:6080  宿主机的6080端口（：前边的）映射到容器的6080端口（：后边那个）（如果宿主机6080端口没有被占用，可以不改）。
+ -v /path/to/your/downloadfolder:/home/baidu/baidunetdiskdownload/  把宿主机的`/path/to/your/downloadfolder`（此路径可改）文件挂载到容器内的`/home/baidu/baidunetdiskdownload/`文件夹。这样做可以把下载的文件存放在宿主机的`/path/to/your/downloadfolder`文件夹中。
+ 其他都不用动

使用方式为直接访问主机地址+端口号，例如192.168.X.X:6080


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
