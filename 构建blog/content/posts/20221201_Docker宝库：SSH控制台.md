---
title: Docker宝库：SSH控制台
date: 2022-12-01T20:00:00+08:00
categories: [
  "NAS"
]
tags: [
  "Docker",
  "NAS"
]
draft: false
---

SSH控制台：如果电脑上不想安装相关的SSH控制台程序，就可使用这个网页版的SSH控制台。
只需输入要连接的IP、端口号、用户名和密码，即可完成连接。
Docker安装也非常简单，使用以下命令：
```
docker run -d --net=host --log-driver json-file --log-opt max-file=1 --log-opt max-size=100m --restart always --name webssh -e TZ=Asia/Shanghai -e savePass=true jrohy/webssh
```
参数解释：
+ --name webssh “webssh”是容器名称，可以自己修改
+ --restart always 容器挂掉了会自动重启
+ 其他都不用动

本容器默认端口号为5032，也就是说使用的时候，直接浏览器输入主机地址或域名:5032即可。
例如：`192.168.X.X:5032` 打开页面之后，输入NAS主机IP地址，SSH端口号（默认是22），控制台用户名，密码连接即可。