---
title: Docker添加镜像源及自行搭建方法
date: 2024-10-22T21-40-14+08:00
categories:
  - Docker
tags:
  - NAS
  - blog
  - Docker
featuredImage: /images/posts/封面图.jpg
draft: false
lastmod: 2025-01-06T13:41:35.634Z
---
## 添加镜像源

目前（2024年10月22日）暂时可用的镜像源：

```
https://jockerhub.com,
https://dockerpull.com,
https://docker.m.daocloud.io,
https://noohub.ru,
https://huecker.io,
https://dockerhub.timeweb.cloud
```

修改本地的Docker`/etc/docker/deamon.json `deamon.json文件，添加"registry-mirrors"字段，例如：

```
{  
  "data-root": "/var/lib/docker",  
  "registry-mirrors": [  
    "https://jockerhub.com",  
    "https://dockerpull.com",  
    "https://docker.m.daocloud.io",  
    "https://noohub.ru",  
    "https://huecker.io",  
    "https://dockerhub.timeweb.cloud"  
  ]  
}
```

## 重启Docker服务

最后，重启docker服务即可生效。

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 使用镜像源拉取镜像

如果镜像服务其地址是`https://hub.docker.hotine.wang`，要拉取`filebrowser/filebrowser:latest`镜像，就这样使用:

```bash
docker pull hub.docker.hotine.wang/filebrowser/filebrowser:latest
```
