---
title: VPS套娃，Linux装Docker，Docker再跑Ubuntu
date: 2023-09-15T00:00:00+08:00
categories: ["VPS与建站"]
tags: ["Docker","VPS","NAS"]
draft: false
---

## VPS套娃，Linux装Docker，Docker再跑Ubuntu

有一个VPS，想弄一个云端工作区。考虑过VPS直接装远程桌面之类的，但是过程比较复杂，而且万一弄不好，把整个VPS弄瘫痪了就麻烦了，所以最终还是选择了Docker安装Ubuntu，干干净净，不影响原VPS系统使用。

本次使用的是 Kasm 的docker镜像，这家公司把Ubuntu打包成了一个可以远程桌面连接，也可以远程网页桌面连接的集成环境。安装十分方便。

可以选择使用以下两者其一： 
+kasmweb/desktop：相当于最小安装，包含Ubuntu系统、远控程序、浏览器
+kasmweb/ubuntu-jammy-desktop：集成工作空间，在desktop版的基础上，还增加了VSCode、办公软件、绘图软件等工具。

安装使用以下命令：
desktop版
```
docker run -d --restart=unless-stopped \
--name ubuntu \
-p 6901:6901 \
-e VNC_PW=wbb@147258369 \
-v /mnt/ubuntushare:/mnt/share \
--shm-size=512m \
kasmweb/desktop:1.14.0
```
集成工作空间版
```
docker run -d --restart=unless-stopped \
--name ubuntu  \
-p 6901:6901 \
-e VNC_PW=147258369 \
-e LANG=zh_CN.UTF-8 \
-e LANGUAGE=zh_CN:zh \
-e LC_ALL=zh_CN.UTF-8 \
-v /mnt/ubuntushare:/mnt/share \
--shm-size=512m \
kasmweb/ubuntu-jammy-desktop:1.14.0
```
注意：
+ `-e VNC_PW=147258369`中的147258369修改成自己的密码
+ `-v /mnt/ubuntushare:/mnt/share \`是指把VPS中 /mnt/ubuntushare目录挂载到docker容器中的/mnt/share目录。这样在容器中的Ubuntu系统中，文件只要放在/mnt/share目录下，就可以在VPS的/mnt/ubuntushare目录中访问到。
+ 最后，通过https://VPS的IP或者域名:6901进行访问即可，用户名是kasm_user，密码是上文中设置的密码。

如需设置Ubuntu的root密码，可以使用以下命令
```
docker exec -u root -it ubuntu-desktop passwd
```
