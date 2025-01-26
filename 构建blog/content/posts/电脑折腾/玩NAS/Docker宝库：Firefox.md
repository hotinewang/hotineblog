---
title: 远程浏览器，通过Docker安装Firefox远程版
date: 2023-09-15T16:00:00.000Z
categories:
  - 玩NAS
tags:
  - Docker
  - VPS
  - NAS
draft: false
---
## 远程浏览器，通过Docker安装Firefox远程版

通过VPS，可以挂一个远程浏览器，这样就可以变相实现魔法上网。

同时，也可以在远程浏览器中登录借来的百度云账号，只要不退出，就可以一直保持登录状态，这样就不用来回的给朋友要登录验证码了。

VPS远程浏览器+NAS百度网盘=百度下载黄金搭档：Docker版浏览器用于资源添加到网盘，NAS里的Docker网盘用来操作资源下载，简直是绝配！

以下是安装命令：
```bash
docker run -d \
    --name=firefox \
    -p 5800:5800 \
    -v /opt/firefox:/config:rw \
    -e ENABLE_CJK_FONT=1 \
    jlesage/firefox
```
注意：
+ `-v /opt/firefox:/config:rw`是指把VPS中 /opt/firefox目录挂载到docker容器中的/config目录。这样在容器中/config目录的配置文件就可以在 /opt/firefox目录下存储。
+ `-e ENABLE_CJK_FONT=1`主要是为了防止浏览器中文乱码。
+ 访问http://VPS IP:5800即可远程访问浏览器。
+ 剪贴板的内容无法直接粘贴到远程浏览器中，需要在左侧的剪贴板面板中先粘贴，然后再从远程浏览器的输入框中粘贴。
