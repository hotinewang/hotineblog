---
title: Docker宝库：青龙面板&NTFY消息推送
date: 2025-01-05T23:28:00+08:00
categories:
  - Docker
tags:
  - 消息推送
  - Docker
  - 脚本自动化
  - blog
  - 青龙
  - ntfy
draft: false
lastmod: 2025-01-07T01:48:20.889Z
---
青龙面板是一款基于Docker技术构建的支持 Python3、JavaScript、Shell、Typescript 的定时任务管理平台。

简单来说就是可以定时跑脚本，实现一些自动化任务，例如签到打卡、定时爬虫等

## 安装：

```bash
docker run -dit \  
--name QingLong \  
--hostname QingLong \  
--restart always \  
-p 5700:5700 \  
-v path/to/ql/data:/ql/data \
-v path/to/ql/config:/ql/config \  
-v path/to/ql/log:/ql/log \  
-v path/to/ql/db:/ql/db \  
-v path/to/ql/scripts:/ql/scripts \  
-v path/to/ql/jbot:/ql/jbot \
whyour/qinglong:latest
```

之后访问IP地址+5700端口号即可访问青龙面板。

## 青龙面板系统使用NTFY消息推送的设置

在青龙面板后台：系统设置-通知设置 中，选择 “自定义通知”，其他按图中设置即可。注意webhookBody那里的topic:"hotine"，把hotine改成实际的话题名称。\
![../附件/玩NAS/Docker宝库：青龙面板\&NTFY消息推送1.png](/%E7%94%B5%E8%84%91%E6%8A%98%E8%85%BE/%E9%99%84%E4%BB%B6/%E7%8E%A9NAS/Docker%E5%AE%9D%E5%BA%93%EF%BC%9A%E9%9D%92%E9%BE%99%E9%9D%A2%E6%9D%BF\&NTFY%E6%B6%88%E6%81%AF%E6%8E%A8%E9%80%811.png)

## 青龙面板的京东Faker脚本的NTFY消息推送设置

在菜单 环境变量 中，新建以下变量、填入对应的值：

```
变量名  变量值
WEBHOOK_BODY  topic:hotine（另起一行）title:$title（另起一行）message:$content
WEBHOOK_CONTENT_TYPE  application/json
WEBHOOK_METHOD  POST
WEBHOOK_URL  https://ntfy.sh
```

记得把WEBHOOK\_BODY中的hotine换成实际的话题名。\
就像下图这样：\
![../附件/玩NAS/Docker宝库：青龙面板\&NTFY消息推送2.png](/%E7%94%B5%E8%84%91%E6%8A%98%E8%85%BE/%E9%99%84%E4%BB%B6/%E7%8E%A9NAS/Docker%E5%AE%9D%E5%BA%93%EF%BC%9A%E9%9D%92%E9%BE%99%E9%9D%A2%E6%9D%BF\&NTFY%E6%B6%88%E6%81%AF%E6%8E%A8%E9%80%812.png)
