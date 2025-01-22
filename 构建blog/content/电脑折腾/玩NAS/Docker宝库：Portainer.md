---
title: Docker宝库：Docker管理工具Portainer
date: 2024-03-11T21:16:00+08:00
categories:
  - NAS
tags:
  - Docker
  - NAS
  - blog
draft: false
lastmod: 2025-01-06T14:32:17.953Z
---
# Docker宝库：Docker管理工具Portainer

Portainer是一款轻量级且功能强大的Docker可视化管理工具，其主要用途包括：创建与删除容器、查看容器信息、操作容器、进入容器、管理镜像等等。通过Portainer，可以直观地管理和监控Docker环境，降低了学习成本和操作复杂度。

控制台输入：

```bash
docker run -d -p 9000:9000 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /dockerData/portainer:/data \
--restart=always \
--name portainer  \
portainer/portainer-ce:latest
```

回车即可。\
访问主机IP+9000进入管理页面。
