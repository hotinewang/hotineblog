---
title: Docker宝库：智能家居集成HomeAssistant
date: 2024-07-30T09:50:00+08:00
categories: ["NAS"]
tags: ["Docker","智能家居","NAS"]
draft: false
---


## 安装HomeAssistant
通过一下命令，在Docker安装HomeAssistant

```bash
docker run -d -it\
--restart always \
--name hass \
-v /opt/homeassistant/config:/config \
-e TZ=Asia/Shanghai \
-p 8123:8123 \
homeassistant/home-assistant:latest
```
之后通过localhost:8123访问管理界面，注册用户等。

## 安装HACS插件
控制台输入以下代码进入容器
```bash
docker exec -it hass bash
````
之后输入以下命令安装hacs
```bash
wget -O - https://get.hacs.xyz | bash -
```
安装完毕后，进入HomeAssistant，点击左下角“配置”，再点击右上角三个点，重新启动HomeAssistant。

启动完毕后，登录HomeAssistant，左下角“配置”-“设备与服务”-“添加集成”，搜索并安装HACS。（5个选项全部打钩，按提示登录Github并输入验证码授权）

此时，菜单栏会出现HACS选项，点击进入，直接在上方搜索栏输入想安装的HACS插件就可以了，记得重启HA生效。

重启成功后，点击“配置”中“设备与服务”选项卡下的“添加集成”按钮，搜索刚刚安装的插件，发现已经有刚刚安装的集成了，点击它进行配置即可。

各类插件推荐：
+ 小米用HACS的Xiaomi Miot Auto
+ 海尔用HACS的Haier(banto6)
+ 涂鸦智能用HACS的Tuya v2
+ OMV系统用HACS的OpenMediaVault
+ 另外可以安装Bar Card插件，用与界面显示百分比进度条（默认插件没有）。
