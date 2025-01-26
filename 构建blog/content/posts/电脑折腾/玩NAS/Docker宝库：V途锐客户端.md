---
title: Docker宝库：V途锐客户端
date: 2024-06-12T13:44:00.000Z
categories:
  - 玩NAS
tags:
  - Docker
  - V途锐
  - NAS
  - VPN
draft: false
---
## 国内Docker被Ban，是时候在NAS上魔法上网了
通过一下命令，在Docker安装V2RayA

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
  -v /opt/v2raya:/etc/v2raya \
  mzz2017/v2raya:latest
```
访问localhost:2017即可进入管理页面，注册账号、添加节点并启用后，便可实现NAS全局魔法。

同时，该客户端可提供给同局域网内的其他设备使用。
## 开启局域网共享

在管理界面右上角，点击齿轮设置图标进入设置界面，点击“开启端口分享”。接着点击左下方“地址与端口”，在弹出的界面中可以找到“http端口(带分流规则)”对应的端口是20172。

打开手机WIFI，连接到同局域网的WIFI后，在手机WIFI中找到已连接信号的网络详情，其中“代理”改成“手动”，在“主机名”填入V2RayA所在设备的IP，端口填写20172，确认即可。

