---
title: Linux修改SSH端口号
date: 2025-01-14T15-03-58+08:00
categories:
  - VPS与建站
tags:
  - Linux
  - blog
draft: true
lastmod: 2025-01-15T02:31:29.659Z
---
在Debian系统中，修改SSH端口号到2077的步骤如下：

### 一、备份配置文件

首先，备份`sshd_config`文件，以防止修改过程中出现错误导致无法通过SSH连接到服务器。

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config_backup
```

### 二、修改配置文件

使用文本编辑器（如`nano`或`vim`）打开`sshd_config`文件。

```bash
sudo nano /etc/ssh/sshd_config
```

或者

```bash
sudo vim /etc/ssh/sshd_config
```

在文件中找到以`#Port 22`开头的行（这行表示SSH默认的端口号是22，并且前面的`#`表示注释）。将其修改为：

```bash
Port 2077
```

如果文件中没有`Port`配置项，直接在文件末尾添加`Port 2077`即可。

如果想开启root登录SSH的权限，需要找到`#PermitRootLogin prohibit-password`的位置，添加：

```
PermitRootLogin yes
```

### 三、重启SSH服务

保存并关闭配置文件后，重启SSH服务使配置生效。

```bash
sudo systemctl restart sshd
```

或者

```bash
sudo service ssh restart
```

### 四、检查防火墙设置

确保防火墙允许2077端口的流量。如果使用的是`iptables`，可以添加如下规则：

bash复制

```bash
sudo iptables -A INPUT -p tcp --dport 2077 -j ACCEPT
```

如果是`ufw`防火墙，可以使用：

bash复制

```bash
sudo ufw allow 2077/tcp
```

然后检查防火墙状态，确保规则已经生效：

bash复制

```bash
sudo ufw status
```

### 五、测试连接

在另一台计算机上或者使用SSH客户端，使用新的端口号尝试连接到Debian服务器。例如，如果你的服务器IP地址是`192.168.1.100`，可以使用以下命令：

bash复制

```bash
ssh -p 2077 username@192.168.1.100
```

其中`username`是你的Debian服务器上的用户名。如果能够成功连接，说明端口号修改成功。
