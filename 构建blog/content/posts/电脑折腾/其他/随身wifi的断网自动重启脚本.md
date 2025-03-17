---
title: 随身wifi的断网自动重启脚本
date: 2025-03-17T10:20:32.000Z
categories:
  - 其他
tags:
  - 脚本自动化
  - WIFI棒子
draft: false
---

### 编写脚本

脚本内容net_check.sh
```bash
#!/bin/bash

# 定义一个函数，用于检测网络连接
check_network() {
    # 使用 ping 命令检测网络连接
    # -c 1 表示发送 1 个 ICMP 数据包
    # -W 5 表示超时时间为 5 秒
    ping -c 1 -W 5 8.8.8.8 &> /dev/null

    # 检查 ping 命令的返回值
    if [ $? -eq 0 ]; then
        echo "网络正常，无需重启。"
        return 0
    else
        echo "网络已断开，准备重启设备。"
        return 1
    fi
}

# 主循环
while true; do
    # 检测网络连接
    if ! check_network; then
        # 如果网络断开，执行重启操作
        echo "正在重启设备..."
        sudo reboot
    fi

    # 每隔一定时间检查一次（例如每 30 秒检查一次）
    sleep 30
done
```
保存在根目录即可。

赋予脚本执行权限：
```bash
chmod +x net_check.sh
```

通过以下命令测试执行：
```bash
/net_check.sh
```

如果希望脚本在后台运行，可以使用 `nohup` 或 `screen` 工具：
```bash
nohup sudo ./net_check.sh &
```

### 进阶：使用 `systemd` 服务

如果希望让脚本在系统启动时自动运行，可以将其配置为一个 `systemd` 服务：

**创建服务文件**：
创建一个服务文件 `/etc/systemd/system/net_check.service`，内容如下：

```ini
[Unit]
Description=Auto Reboot on Network Failure

[Service]
ExecStart=/net_check.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

**启用服务**：
```bash
sudo systemctl daemon-reload
sudo systemctl enable net_check.service
sudo systemctl start net_check.service
```

这样，脚本会在系统启动时自动运行，并在检测到网络断开时重启设备。
