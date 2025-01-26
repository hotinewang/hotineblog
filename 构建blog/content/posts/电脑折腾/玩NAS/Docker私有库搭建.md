---
title: Docker私有仓库搭建
date: 2024-06-12T04:00:00.000Z
categories:
  - 玩NAS
tags:
  - Docker
  - NAS
draft: false
---
6月，中国大陆屏蔽了Docker仓库，NAS上通过Docker装各类应用基本上是白搭了。好在还有VPS，除了通过代理科学上网来访问Docker公共库外，从VPS搭建一个Docker私有仓库，家里NAS使用Docker私有仓库也是一个不错的办法。
## VPS搭建私有仓库
VPS上，控制台安装Docker Registry：
```bash
docker run -id -p \
5000:5000 \
-v /opt/docker_registry/registry/:/var/lib/registry \
--privileged=true \
--restart=always \
--name=registry \
registry:latest
```
+ -v /opt/docker_registry/registry/:/var/lib/registry \ 是将容器内保存镜像的文件夹映射到本地“/opt/docker_registry/registry/”中。
## NAS端配置使用私有仓库
在需要使用私有仓库的机器上配置Docker，时期能够使用私有仓库。编辑(或创建)```etc/docker/daemon.json```
```json
{
  "insecurt-registries":[your-registry-url:5000]
}
```
+ your-registry-url:5000 需要替换成VPS的地址。

重启Docker服务
```bash
systemctl restart docker
```
在本地或其他机器上运行以下命令，确保能够连接到自建的Docker Registry：
```bash
docker login your-registry-domain:5000
```


现在，已经成功搭建了一个私有仓库。

## 如何使用
### 推送镜像
VPS端可以拉取公共库的镜像，再推入私有库供其他设备使用。
推送本地镜像到私有仓库的命令如下：
```bash
# 标记本地镜像 docker tag image-name your-registry-url:5000/image-name
docker tag image-name your-registry-url:5000/image-name 
# 推送镜像到私有仓库 docker push your-registry-url:5000/image-name
docker push your-registry-url:5000/image-name 
```
+ 将image-name替换为要使用的镜像名称，your-registry-url:5000替换为私有仓库的URL

### 拉取镜像
NAS端可以拉取私有库的镜像使用
```bash
docker pull your-registry-url:5000/image-name:latest
```
