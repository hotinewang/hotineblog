---
title: Linux上做个流量统计器
date: 2025-01-14T22-17-47+08:00
categories:
  - VPS与建站
tags:
  - 脚本自动化
  - 消息推送
  - blog
  - VPS
featuredImage: 
draft: false
lastmod: 2025-01-16T09:12:00.709Z
---
VPS流量竟然用超了，于是打算做个流量统计与监控的脚本来自动统计每月和每天的流量使用情况。

查了下资料，Linux系统中，有一个虚拟文件，它提供了系统中所有网络接口的详细统计信息。这个文件就是`/proc/net/dev`。\
打开这个文件，内容大致像：

```
Inter-|   Receive                                                |  Transmit
 face |bytes    packets errs drop fifo frame compressed multicast|bytes    packets errs drop fifo colls carrier compressed
    lo: 3304479918 6432909    0    0    0     0          0         0 3304479918 6432909    0    0    0     0       0          0
enp1s0: 118598215588 160465104    0 10015148 1390     0          0   9098966 139289884592 177958470    0    0    0    14       0          0
enp2s0:       0       0    0    0    0     0          0         0        0       0    0    0    0     0       0          0
...
```

用Excel表格整理下，就是下表的样子（展示了两个设备中的`/proc/net/dev`文件的样子）\
![Linux上做个流量统计器.png](/%E7%94%B5%E8%84%91%E6%8A%98%E8%85%BE/%E9%99%84%E4%BB%B6/VPS%E4%B8%8E%E5%BB%BA%E7%AB%99/Linux%E4%B8%8A%E5%81%9A%E4%B8%AA%E6%B5%81%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%99%A8.png)

* **Inter-face**是接口名称。
* **Receive**：接收数据的统计信息。
  * **bytes**：接收的字节数。
  * **packets**：接收的数据包数。
  * **errs**：接收时出现的错误数。
  * **drop**：接收时丢弃的数据包数。
  * **fifo**：接收时因FIFO（先进先出）队列溢出而丢弃的数据包数。
  * **frame**：接收时因帧错误而丢弃的数据包数。
  * **compressed**：接收时压缩的数据包数。
  * **multicast**：接收的多播数据包数。
* **Transmit**：发送数据的统计信息。
  * **bytes**：发送的字节数。
  * **packets**：发送的数据包数。
  * **errs**：发送时出现的错误数。
  * **drop**：发送时丢弃的数据包数。
  * **fifo**：发送时因FIFO队列溢出而丢弃的数据包数。
  * **colls**：发送时发生的碰撞数（仅适用于以太网）。
  * **carrier**：发送时因载波问题而丢弃的数据包数。
  * **compressed**：发送时压缩的数据包数。

一般设备中，enth0或者enp1s0就是网卡的接口名称。`lo`是本地回环接口（loopback interface），也被称为环回接口。

要分析流量只需解析出Receive中bytes 和 Transmit中bytes 的数值即可。
