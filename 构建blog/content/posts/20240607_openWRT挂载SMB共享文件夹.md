---
title: openWRT（Linux）挂载共享文件夹
date: 2024-06-07T22:48:00+08:00
categories: ["旁路由"]
tags: ["OpenWRT","Linux","NAS","旁路由"]
draft: false
---


# openWRT（Linux）挂载共享文件夹
两个家中都有NAS，用ZerotierOne组网在了一起。其中一个通过SMB协议共享了电影，想在另一个家里的NAS上（openWRT系统）把这个电影文件夹挂载到本地NAS里，并再次通过SMB共享出去供电视盒子使用。

但是在openWRT系统中，启用SMB共享简单，但是“挂载 SMB/CIFS 网络共享文件夹”这个选项怎么设置都不起作用。折腾很久，终于通过命令行搞定。

1. 首先安装相关套件cifsmount
```bash
opkg update         # 更新软件列表
opkg install cifsmount          # 安装cifsmount
```
2. 之后，创建一个挂载点（创建挂载点的文件夹）。用命令行或者FinalShell之类的工具新建文件夹都行。例如```/mnt/nas100```。

3. 挂载SMB目录
```bash
mount.cifs //192.168.193.1/nas/media /mnt/nas100 -o user=username,pass=password,iocharset=utf8
```
  + //192.168.193.1/nas/media SMB的路径
  + /mnt/nas100 主机挂载点的路径
  + user=username username是SMB访问的用户名
  + pass=password password是SMB访问密码
  + iocharset=utf8 指定字符集，以确保中文文件名和路径在Linux系统上正确显示，如果还是乱码，先卸载掉挂载点，再删掉这个参数，或者把utf8替换为cp936或gbk重新执行上述命令试试。

4. 卸载挂载点
```bash
umount /mnt/nas100
```
  + /mnt/nas100 是要卸载的挂载点路径

