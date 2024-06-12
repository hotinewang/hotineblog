---
title: Docker宝库：智能家居集成HomeAssistant
date: 2024-08-17T00:00:00+08:00
categories: ["NAS"]
tags: ["Docker","智能家居","NAS"]
featuredImage: /images/posts/封面图.jpg
draft: true
---


# 这里是正文标题
通过一下命令，在Docker安装HomeAssitant

```bash
docker run -d \
--restart always \
--name hass \
-v /opt/homeassistant/config:/config \
-e TZ=Asia/Shanghai \
-p 8123:8123 \
homeassistant/home-assistant:latest

`#或者指定从阿里镜像源拉取镜像：`
docker run -d \
--restart always \
--name hass \
-v /opt/homeassistant/config:/config \
-e TZ=Asia/Shanghai \
-p 8123:8123 \
docker.m.daocloud.io/homeassistant/home-assistant:latest
```

链接[hotine.wang](https://hotine.wang/)
+ 列表1
 + 次级1
 + 次级2
 + 次级3
  +次次级
+ 列表2
+ 列表3


| 虫害类型 | 推荐药剂（已测试） | 可用药剂 |
| --- | --- | --- |
| 红蜘蛛 | 丁氟螨酯-悬浮剂（没有味道） | 呋虫胺 |
| 蚜虫 | 吡虫啉-可湿性粉剂（没有味道） | 呋虫胺、氟氯菊酯 |
| 白粉虱 | 吡虫啉-可湿性粉剂（没有味道） | 呋虫胺、氟氯菊酯 |
| 小黑飞 | 吡虫啉-可湿性粉剂（没有味道） | 呋虫胺、氟氯菊酯 |
| 蓟马 | 吡虫啉-可湿性粉剂（没有味道） | 呋虫胺、啶虫脒 |
| 蚧壳虫 | 未测试过 | 呋虫胺、吡虫啉、氟氯菊酯 |

![温泉](/images/posts/20240512_匆忙的云南之行.day4.温泉.jpg)

行内代码`这样子`
代码块：
  ```
  这样子
  和这样子
  整个代码块可用缩进控制在正文中的位置
  ```
  注意把join后边的那串文字替换成自己网络的NetworkID

docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
