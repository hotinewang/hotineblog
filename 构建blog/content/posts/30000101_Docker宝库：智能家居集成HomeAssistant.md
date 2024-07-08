---
title: Docker宝库：智能家居集成HomeAssistant
date: 2024-08-17T00:00:00+08:00
categories: ["NAS"]
tags: ["Docker","智能家居","NAS"]
featuredImage: /images/posts/封面图.jpg
draft: true
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

此时，菜单栏会出现HACS选项，点击进入，直接在上方搜索栏输入xiaomi，选择xiaomi miot auto并安装。安装完毕，再次重启HomeAssistant。

重启成功后，点击“配置”中“设备与服务”选项卡下的“添加集成”按钮，搜索xiaomi，发现已经有刚刚安装的集成了，点击它进行配置


https://post.smzdm.com/p/avpn58o9/



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
