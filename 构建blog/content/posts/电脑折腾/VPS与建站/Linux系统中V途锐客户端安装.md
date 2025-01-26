---
title: Linux系统中V途锐客户端安装
date: 2023-06-24T16:00:00.000Z
categories:
  - VPS与建站
tags:
  - V途锐
---
# Linux系统中直接安装：
Linux系统中V途锐客户端的名称叫`V2RAYA`。
官网：[V2RAYA.ORG](https://v2raya.org/)
GitHub：[v2rayA/v2rayA](https://github.com/v2rayA/v2rayA)
官方文档[Debian：Debian/Ubuntu安装](https://v2raya.org/docs/prologue/installation/debian/)

# 安装流程：
根据官方文档安装就可以，很简单。
V途锐客户端的功能依赖于V途锐内核，因此需要安装内核,在控制台直接输入命令即可。
PS:如果以下命令执行中出现问题，建议先看官方安装文档，有可能安装方式有变化。

## 安装V途锐内核
安装客户端提供的镜像脚本
```bash
curl -Ls https://mirrors.v2raya.org/go.sh | sudo bash
```
安装后可以关掉服务，因为客户端不依赖于该systemd服务。
```bash
sudo systemctl disable v2ray --now
```

## 通过软件源安装客户端
添加公钥
```bash
wget -qO - https://apt.v2raya.org/key/public-key.asc | sudo tee /etc/apt/trusted.gpg.d/v2raya.asc
```
添加客户端软件源，更新软件列表
```bash
echo "deb https://apt.v2raya.org/ v2raya main" | sudo tee /etc/apt/sources.list.d/v2raya.list
sudo apt update
```
安装客户端
```bash
sudo apt install v2raya
```
## 启动客户端/设置客户端自动启动
启动客户端
```bash
sudo systemctl start v2raya.service
```
设置开机自动启动
```bash
sudo systemctl enable v2raya.service
```
最后，通过 http://localhost:2017 进入管理界面,添加节点并选择节点，点击左上方“就绪”使其变为“启用”。
右上角设置里，改为“启用：大陆白名单模式”即可。


# Docker安装
```bash
docker run -d \
--restart=always \
--privileged \
--network=host \
--name v2raya \
-e V2RAYA_LOG_FILE=/tmp/v2raya.log \
-e V2RAYA_V2RAY_BIN=/usr/local/bin/v2ray \
-e V2RAYA_NFTABLES_SUPPORT=off \
-e IPTABLES_MODE=legacy \
-v /lib/modules:/lib/modules:ro \
-v /etc/resolv.conf:/etc/resolv.conf \
-v /etc/v2raya:/etc/v2raya \
mzz2017/v2raya
```
通过IP+端口2017可访问
