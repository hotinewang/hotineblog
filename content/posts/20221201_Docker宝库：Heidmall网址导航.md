---
title: Docker宝库：Heidmall网址导航
date: 2022-12-01T20:00:01+08:00
categories: [
  "NAS"
]
tags: [
  "Docker",
  "NAS"
]
draft: false
---
NAS中安装的容器（程序）太多，端口号不好记，因此可以通过一个网址导航来省去记端口的烦恼。
Docker安装命令非常简单，使用SSH工具连接服务器，输入以下命令：
```
docker run -d --name=heimdall -p 8080:80 -p 8081:443 -v /opt/Heimdall/config:/config --restart unless-stopped linuxserver/heimdall
```
参数解释：
+ --name heimdall  “heimdall”是容器名称，可以自己修改
+ -p 8080:80 -p 8081:443  通过8080端口来访问容器的80端口，8081端口访问容器的443端口（HTTPS协议），8080和8081均可修改。
+ -v /opt/Heimdall/config:/config  挂载服务器路径/opt/Heimdall/config到容器内/config路径，也就是说容器内的配置文件实际上是保存到服务器的/opt/Heimdall/config中的。
+ 其他都不用动

本容器默认端口号为8080，也就是说使用的时候，直接浏览器输入主机地址或域名:8080即可。
例如：`192.168.X.X:8080` 
这样一个网址导航页面就做好了，里面具体怎么用很简单，可以自行研究。