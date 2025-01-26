---
title: Docker镜像导出与导入
date: 2024-06-12T16:00:00.000Z
categories:
  - 玩NAS
tags:
  - Docker
  - Linux
  - NAS
draft: false
---

Docker被Ban，因此可以通过导入、导出镜像文件的方式来分享、安装镜像。

## 镜像导出
```bash
docker save -o <导出路径与文件名> <镜像名>:<标签>
#例如
docker save -o /images/nginx.tar nginx:latest
```

## 镜像导入
```bash
docker load -i <导入路径与文件名>
#例如
docker load -i  /images/nginx.tar
```
