---
title: WIFI棒子电脑驱动
date: 2024-07-30T01:52:00.000Z
categories:
  - 其他
tags:
  - WIFI棒子
draft: false
---

WIFI棒子刷DEBIAN系统后，直接插电脑不会被自动识别。在设备管理器中会显示为：`其他设备-RNDIS Communications Control`

只需在该设备上点击右键，`更新驱动程序`–>`浏览我的电脑以查找驱动程序`–>`让我从计算机上的可用驱动程序列表中选取`–>`显示所有设备`，等待列表加载完成。左侧选择 `Microsoft`，右侧选择`**基于远程 NDIS 的 Internet 共享设备`，在弹出窗口中选择`是`，并确定。至此，RNDIS 驱动安装成功。
