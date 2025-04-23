---
title: Docker宝库：PDF编辑器Stirling-PDF
date: 2025-04-23T01:08:24.000Z
categories:
  - 玩NAS
tags:
  - NAS
  - Docker
featuredImage: null
draft: false
---

[Stirling-PDF](https://github.com/Stirling-Tools/Stirling-PDF) 是一个功能强大的开源 PDF 操作工具，采用 MIT 开源协议，用户可以自由使用、修改和分发，无需支付任何费用。
![Docker宝库：PDF编辑器Stirling-PDF.png](/blog/电脑折腾/附件/玩NAS/Docker宝库：PDF编辑器Stirling-PDF.png)
### 功能特点

- **页面操作** ：支持查看和编辑 PDF，包括注释、绘图、添加文本和图像等；可合并多个 PDF 文件，或将一个 PDF 文件拆分成多个；还能旋转页面、删除页面、裁剪页面、调整页面顺序、自动分割 PDF、提取页面、将 PDF 转换为单页等。
    
- **转换操作** ：能够将 PDF 与图像相互转换，如将 PDF 转换为图片，或将图片转换为 PDF；支持将常见文件格式如 Word、PowerPoint 等转换为 PDF，也可将 PDF 转换为 Word、PowerPoint 等格式；此外，还能实现 HTML、URL、Markdown 到 PDF 的转换。
    
- **安全与权限** ：可添加或删除 PDF 密码，更改或设置 PDF 权限，如限制打印、复制等；支持添加水印、认证或签署 PDF、净化 PDF、自动删减文本等，满足用户对 PDF 文件安全性和权限管理的需求。
    
- **其他操作** ：具备添加签名、修复 PDF、检测并删除空白页、比较两个 PDF 文件显示文本差异、从 PDF 中提取图像、添加页码、通过检测 PDF 头部文本自动重命名文件、对 PDF 进行 OCR 文字识别、将 PDF 转换为 PDF/A 格式、编辑元数据、展平 PDF 等功能。

### Docker部署

Stirling-PDF提供了Docker镜像，方便用户快速部署和运行。以下是通过`docker run`命令运行Stirling-PDF容器的示例：
```bash
docker run -d \
  -p 8099:8080 \
  -v /opt/s-pdf/trainingData:/usr/share/tessdata \
  -v /opt/s-pdf/extraConfigs:/configs \
  -v /opt/s-pdf/customFiles:/customFiles \
  -v /opt/s-pdf/logs:/logs \
  -e DOCKER_ENABLE_SECURITY=false \
  -e INSTALL_BOOK_AND_ADVANCED_HTML_OPS=false \
  -e LANGS=zh_CN \
  --name s-pdf \
  frooodle/s-pdf:latest
```

运行后，可以通过`http://localhost:8099`访问Stirling-PDF的界面。
