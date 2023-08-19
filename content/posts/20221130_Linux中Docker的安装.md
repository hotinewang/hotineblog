---
title: Linux中Docker的安装
date: 2022-11-30T20:00:00+08:00
categories: [
  "NAS","VPS与建站"
]
tags: [
  "建站",
  "Docker",
  "NAS"
]
featuredImage: /images/posts/20221130_Linux中Docker的安装.cover.jpg
draft: false
---

**什么是DOCKER？**
Docker 是一种开源的容器化平台，用于帮助开发人员构建、交付和运行应用程序。通过使用 Docker，开发人员可以将应用程序及其所有依赖项打包到一个称为容器的独立单元中，从而实现环境隔离、易于部署、可移植性和一致性。
简单来说就是，如果想在linux中安装一个文件管理、或者影音库等程序，如果不用Docker，需要手动敲很多命令进行配置、安装，中间过程麻烦不说，还有可能出现缺少依赖、插件版本不兼容等很多的错误。如果想删除程序，也只能手动一点一点清理，非常麻烦。而有了Docker，则可以使用别人已经打包配置好的程序镜像（相当于windows安装盘，是只读的），通过镜像来生成一个程序运行的实例（术语叫做“容器”，相当于一个轻量级虚拟机）。而且Docker容器和容器之间、以及容器和系统之间相互隔离互不影响。删除时，也只是一句代码，即可把程序删的干干净净。
+ 安装docker
  ```
  sudo apt-get install -y docker.io
  ```
+ 启动docker服务
  ```
  systemctl start docker
  ```
+ 设置开机启动
  ```
  systemctl enable docker
  ```
+ 查看状态
  ```
  systemctl status docker
  ```
  到此，docker就安装成功了。后续可以通过`docker run`命令来镜像创建和启动容器。
  可参照其他文章，查看各类镜像的安装使用方法。
+ 停止服务（备用）
  ```
  systemctl stop docker
  ```
+ 其他常见命令：
  ```
  查看docker使用说明：
  docker --help 
  
  下载镜像，xxxxx用镜像名称代替。直接使用docker run命令，在本地没有目标镜像的情况下会自动下载镜像，所以这个pull用处不是特别大。
  docker pull xxxxx
  
  通过镜像运行一个容器。如果本地还没有指定的镜像，则会自动下载，然后再运行。
  docker run <一大堆参数> <镜像名>
  
  查看容器列表。使用-a表示查看全部容器，不使用-a表示只查看运行中的容器
  docker ps -a
 
  停止、启动或重启一个容器。xxxxxx用容器名称或者容器ID代替。名称和ID可用docker ps -a查看
  docker stop xxxxxx
  docker start xxxxxx
  docker restart xxxxxx
  
  删除一个容器。注意：删除容器时，需要先把容器停止掉，否则无法删除。xxxxxx用容器名称或者容器ID代替。
  docker rm xxxxxx
  
  查看已下载的Docker镜像文件
  docker images
  
  删除镜像 xxxxxx用镜像名或者ID代替。注意：需要删除全部基于本镜像的容器之后，才能删除。
  docker rmi xxxxxx
  ```