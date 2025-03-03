---
title: FRP内网穿透
date: 2024-03-12T10:01:00.000Z
categories:
  - VPS与建站
tags:
  - FRP
  - 内网穿透
  - NAS
  - 反向代理
  - Docker
draft: false
---
# FRP内网穿透
内网穿透的方案，除了用Zerotier虚拟组网外，还可以使用FRP进行端口映射，实现内网穿透。**FRP（Fast Reverse Proxy）:** FRP 是一种用于内网穿透的工具，它允许您通过公网访问位于私有网络内的服务器和服务。它支持多种协议，如 HTTP、HTTPS、TCP 和 UDP。

这种方式，不需要访问者设备上安装特别的应用程序，只需要在内网机和服务器上分别安装FPR服务端可客户端即可。访问者通过“服务器域名或IP+映射端口”号来访问内网特定端口。


## 下载安装FRP

### 方法1：使用Docker安装
分别在服务器和内网机的控制台中新建`/opt/frps/`目录和`/opt/frpc/`目录。
服务器端在`/opt/frps/`目录中新建`frps.toml`文件。
之后在服务器控制台运行以下命令：
```bash
docker run \
-d \
--name frps  \
--network host \
-v /opt/frps/frps.toml:/etc/frp/frps.toml \
snowdreamtech/frps
```
内网机在`/opt/frpc/`目录中新建frpc.toml文件。
之后在内网机的控制台运行以下命令：
```bash
docker run \
-d \
--name frpc  \
--network host \
-v /opt/frpc/frpc.toml:/etc/frp/frpc.toml \
snowdreamtech/frpc
```


### 方法2：直接安装
首先从Github上下载FRP的最新版，(fatedier/frp)[https://github.com/fatedier/frp/releases] ,解压缩后会有4个比较重要的文件：
```
frpc		客户端程序
frpc.toml		客户端配置文件

frps		服务端程序（公网主机用，比如VPS）
frps.toml		服务端配置文件
```
之后把frps、frps.toml上传到服务器；frpc、frpc.toml上传到要穿透的内网机。比方说分别放在服务器和内网机的`/opt/frps/`和`/opt/frps/`目录中。

PS：此方法后续需要额外设置frp程序跟随系统自启动。否则系统重启后，服务将停止。建议使用docker部署。


## 配置FRP服务端与客户端文件

### 服务端配置文件设置
修改服务端frps.toml：
```bash
[common]
bind_port = 7000 #frp通信用的监听端口
vhost_http_port = 7080 #frp在代理内网机http协议80端口时对应的映射端口
vhost_https_port = 7443 #frp在代理内网机https协议443端口时对应的映射端口
subdomain_host= on.hotine.wang #要绑定的子域名，需要在DNS中提前设置好这个域名解析到服务器上。同时，还需设置*.on.hotine.wang也解析到服务器上。

token = hotinefrp #frps和frpc建立连接时的密码

dashboard_port = 7001 #frps管理面板的对应端口。可以使用服务器IP:7001进行访问
dashboard_user = admin #管理面板的用户名
dashboard_pwd = password #管理面板的密码

```
保存并关闭配置文件。

如果是Docker部署的，直接重启容器即可`docker restart frps`。

如果是直接安装的，在控制台中导航到 FRP 服务器端目录，运行 `./frps -c frps.toml` 启动 FRP 服务器端。

修改frps.toml文件后，均需要重启frps。

### 客户端配置文件设置
比如想把内网SMB协议、SSH协议、9009端口的网页穿透到公网，则修改客户端frpc.ini为：
```bash
[common]
server_addr = 199.115.230.45 #服务器地址，可以用IP或者域名
server_port = 7000 #frp通信用的监听端口，需要和服务端设置一致
token = hotinefrp #frps和frpc建立连接时的密码，需要和服务端设置一致

log_file = /etc/frp/frpc.log
log_level = info
log_max_days = 3


# 客户端配置web页面，配置后可以在localhost:7001页面修改本地配置
admin_addr = 0.0.0.0
admin_port = 7001
admin_user = admin
admin_pwd = password

[ssh] #SSH连接所需设置。外网可通过server_addr指定的地址、端口号7022进行SSH连接内网机。
name = "SSH" #标识名称
type = "tcp"
local_ip = "127.0.0.1"
local_port = 22
remote_port = 7022

[sync] #http穿透演示
name = "test" #标识名称
type = "http"
local_ip = "127.0.0.1"
local_port = 8384 #内网中要穿透的端口号
subdomain = test #映射到subdomain_host（例如上文中的on.hotine.wang）时所用的子域名。之后可以通过test.on.hotine.wang:7080访问。

```

之后重启docker，`docker restart frpc`。
如果是直接安装的，在控制台中导航到 FRP 内网机目录，运行 `./frpc -c frpc.toml` 启动 FRP 客户端。

修改frpc.toml文件后，均需要重启frpc。或者使用网页管理面板修改，可免除重启。

完成上述步骤后，就可以通过`test.on.hotine.wang:7080`访问内网对应资源了。

请确保网络配置正确，端口映射和防火墙设置没有问题。另外，务必考虑安全性和身份验证等问题，以确保共享文件不会暴露在公开网络中。


## 其他事项

+ 虽然说`server_addr`可以使用IP或者域名，但是如果启用了Cloudflare IP保护功能的话，使用域名将无法连接。

+ 上文中的子域名泛解析`*.on.hotine.wang`,也不能启动IP保护功能。

