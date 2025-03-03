---
title: Docker宝库：文档处理OnlyOffice
date: 2025-01-26T12:55:48.000Z
categories:
  - 玩NAS
tags:
  - NAS
featuredImage: null
draft: false
---
OnlyOffice 是一款由 Ascensio System SIA开发的开源办公套件，提供了文档编辑、协作、管理和存储功能。它支持多种文件格式，包括DOCX、XLSX、PPTX、PDF等，并且可以在线编辑文档、表格和演示文稿，支持多人实时协作。

下面是OnlyOffice安装的docker run命令的示例：

```bash
docker run -d \
  --name onlyoffice-document-server \
  -p 80:80 \
  -p 443:443 \
  -e "JWT_ENABLED=true" \
  -e "JWT_SECRET=your_jwt_secret" \
-v /path/to/document_data:/var/www/onlyoffice/Data \
-v /path/to/document_lib:/var/lib/onlyoffice \
-v /path/to/document_logs:/var/log/onlyoffice \
  onlyoffice/documentserver:latest
```

解释一下各个参数的含义：


• `-d`：表示以守护进程模式运行容器，即在后台运行。

• `--name`：指定容器的名称，这里命名为`onlyoffice-document-server`。

• `-p`：端口映射，将宿主机的端口映射到容器的端口。这里将宿主机的80端口和443端口分别映射到容器的80端口和443端口。

• `-e`：设置环境变量。这里设置了两个环境变量，`JWT_ENABLED`用于启用JWT，`JWT_SECRET`用于设定JWT密钥(可选)。

• `-v`：挂载卷。这里挂载了三个卷，分别用于持久化数据、库文件和日志文件。将`/path/to/document_data`、`/path/to/document_lib`、`/path/to/document_logs`替换为你在宿主机上实际的目录路径。

• `onlyoffice/documentserver:latest`：指定使用的Docker镜像名称。



<!--
自用：
```bash
docker run -d \
  --name onlyoffice \
  -p 8100:80 \
  -e "JWT_ENABLED=true" \
  -e "JWT_SECRET=hotineonlyofficejwt" \
-v /srv/dev-disk-by-uuid-1ad0ebcf-0133-47df-af50-2cb54d10baa6/documents:/var/www/onlyoffice/Data \
-v /opt/onlyoffice/lib:/var/lib/onlyoffice \
-v /opt/onlyoffice/log:/var/log/onlyoffice \
  onlyoffice/documentserver:latest
```
-->
