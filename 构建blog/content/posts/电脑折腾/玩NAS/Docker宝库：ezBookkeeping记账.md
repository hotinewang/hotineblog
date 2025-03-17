---
title: Docker宝库：ezBookkeeping记账
date: 2025-03-17T10:16:47.000Z
categories:
  - 玩NAS
tags:
  - NAS
  - Docker
featuredImage: null
draft: false
---

ezBookkeeping 是一个自托管 (self-hosted) 的轻量个人账本应用。它可以部署在几乎所有平台上，包括 x86、x64 以及 ARM 架构的 Windows、macOS 和 Linux。您甚至可以将它部署在树莓派 (Raspberry) 设备中。此外，它支持多种不同数据库，包括 SQLite、MySQL 和 PostgreSQL 等。借助 Docker，您甚至只需要一条命令即可将其部署，而无需复杂的配置。

项目地址：[https://github.com/mayswind/ezbookkeeping](https://github.com/mayswind/ezbookkeeping)

容器中的默认路径：
- **配置文件**：`/ezbookkeeping/conf/ezbookkeeping.ini`
- **数据库文件（使用 `sqlite3` 数据库）**：`/ezbookkeeping/data/ezbookkeeping.db`
- **日志文件**：`/ezbookkeeping/log/ezbookkeeping.log`
- **对象存储根路径（使用 `local_filesystem` 对象存储）**: `/ezbookkeeping/storage/`


使用持久化数据卷运行容器:
```bash
# 在宿主机创建数据路径并修改 UID/GID
$ mkdir -p /opt/ezbookkeeping/data
$ mkdir -p /opt/ezbookkeeping/storage
$ chown 1000:1000 /opt/ezbookkeeping/data
$ chown 1000:1000 /opt/ezbookkeeping/storage
```

```bash
docker run -d \
-p 9080:8080 \
--name ezbookkeeping \
-v /opt/ezbookkeeping/data:/ezbookkeeping/data \
-v /opt/ezbookkeeping/storage:/ezbookkeeping/storage \
mayswind/ezbookkeeping
```

第一次使用需要注册账号，如需导入数据。可以先随意添加两条记账记录，然后导出。根据导出的格式填写导入模板即可。

记账类别、标签等信息需要提前在平台中设置好，否则导入时会要求强制修改成已有的分类或标签。
