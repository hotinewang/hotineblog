---
title: Docker宝库：Docker管理工具Portainer
date: 2024-03-11T21:16:00+08:00
categories: ["NAS"]
tags: ["Docker","NAS"]
draft: false
---


# Docker宝库：Docker管理工具Portainer

控制台输入：
```bash
docker run -d -p 9000:9000 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /dockerData/portainer:/data \
--restart=always \
--name portainer 
portainer/portainer-ce:latest
```
回车即可。
访问主机IP+9000进入管理页面。