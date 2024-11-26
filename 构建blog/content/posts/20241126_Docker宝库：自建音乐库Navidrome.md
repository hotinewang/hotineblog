---
title: Docker宝库：自建音乐库Navidrome
date: 2024-11-26T11:00:00+08:00
categories: ["NAS"]
tags: ["Docker","NAS"]
featuredImage: /images/posts/2024/Docker宝库：自建音乐库Navidrome2.PNG
draft: false
---


自己收藏音乐如何漂漂亮亮的整理在一起，做一个自己的音乐网站？又或是怕音乐App会员过期好歌就不能听了？不想看APP广告，只想要极简的音乐播放与歌单？

使用Navidrome搭建自己的音乐库，通过music TAG整理、编辑音乐的封面、歌词等标签信息。

## 安装Navidrome音乐库
控制台直接输入命令：
```bash
docker run -d \
--name navidrome \
--restart=unless-stopped \
--user $(id -u):$(id -g) \
-v /path/to/music:/music \
-v /path/to/data:/data \
-p 4533:4533 \
-e ND_LOGLEVEL=info \
-e ND_DEFAULTLANGUAGE=zh-Hans \
deluan/navidrome:latest
```
+ `-v /path/to/music:/music`  /path/to/music 需要修改成音乐文件夹所在的位置
+ `-v /path/to/data:/data` /path/to/data 需要修改成程序的数据文件存放的位置，比方说/opt/navidrome
+ `-p 4533:4533` 前边的4533可以修改为想使用的端口号；右边的4533为内部端口号不能修改。
之后即可通过IP:4533访问Navidrome网站。
注册账户后，如果不是中文界面，可在右上角点击头像，设置为中文界面。
![设置中文](/images/posts/2024/Docker宝库：自建音乐库Navidrome1.PNG)

这个网站能够展示出音乐文件夹中全部的音乐，并能自建歌单。
但是，展示的可能比较乱：有的音乐没有封面，有的同一专辑的歌曲被识别为多个专辑等。
因为，Navidrome的显示规则是，按照：专辑+艺术家进行归类。也就是说，如果一张专辑中有不同艺术家参与，Navidrome会识别为多张专辑，这时可以通过编辑元数据的“专辑艺术家”标签将它们组合在一起。
因此，需要有一个刮削器，去给音乐文件归类、添加封面与歌词、修改专辑等。

## 安装music TAG 刮削器
控制台直接输入命令：
```bash
docker run -d -p 4534:8002 \
--name musictag \
-v /path/to/your/music:/app/media \
-v /path/to/your/config:/app/data \
--restart=always \
xhongc/music_tag_web:latest
```
+ `/path/to/your/music:/app/media`  /path/to/your/music 需要修改成音乐文件夹所在的位置
+ `-v /path/to/your/config:/app/data` /path/to/your/config 需要修改成程序的数据文件存放的位置，比方说/opt/musictag
+ `-p 4534:8002 前边的4534可以修改为想使用的端口号；右边的8002为内部端口号不能修改。
之后即可通过IP:4534访问MusicTag网站进行音乐管理。

![MusicTag](/images/posts/2024/Docker宝库：自建音乐库Navidrome3.PNG)
单击一个音频文件，并点击中间面板音乐标题右边的放大镜图标，可以搜索网上的对应音乐，并在最右边的搜索结果中，点击对应的信息，可以替换本地音乐的相应信息。如封面、歌词、标题等。
勾选多个文件，可以进行批量修改信息等。

除此之外，在电脑端安装一个MusicTag软件，也可以方便的管理音乐文件。

最终，通过MusicTag整理后，在Navidrome中的音乐库如下图：
![Navidrome](/images/posts/2024/Docker宝库：自建音乐库Navidrome2.PNG)


**自用备份：**

```bash
docker run -d \
   --name navidrome \
   --restart=unless-stopped \
   --user $(id -u):$(id -g) \
   -v /srv/dev-disk-by-uuid-1ad0ebcf-0133-47df-af50-2cb54d10baa6/media/music:/music \
   -v /opt/navidrome:/data \
   -p 4533:4533 \
   -e ND_LOGLEVEL=info \
   -e ND_DEFAULTLANGUAGE=zh-Hans
   deluan/navidrome:latest
```

```bash
docker run --name musictag  \
-d -p 4534:8002 \
-v /srv/dev-disk-by-uuid-1ad0ebcf-0133-47df-af50-2cb54d10baa6/media/music:/app/media \
-v /opt/music_tag:/app/data \
--restart=always \
xhongc/music_tag_web:latest
```
