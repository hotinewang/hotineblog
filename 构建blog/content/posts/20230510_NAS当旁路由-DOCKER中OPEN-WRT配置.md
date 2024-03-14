---
title: NAS当旁路由-DOCKER中OPEN-WRT配置
date: 2023-05-10T00:00:00+08:00
categories: [
  "旁路由"
]
tags: [
  "旁路由",
  "OpenWRT",
  "Docker"
]
featuredImage: /images/posts/20230510_NAS当旁路由-DOCKER中OPEN-WRT配置.jpg
---
家里NAS上为啥要弄个旁路由，当然是为了能更好的优化网络，美美的上网了。

## 安装流程：
### **网卡开启混杂模式**
用SSH工具连接服务器,需要先查看是否打开了网卡的混杂模式。
查看网卡是否打开混杂模式（使用以下两个命令都可以）
```
ip addr
或者
ifconfig
```
会显示类似于下边的信息：
```
1: lo: .LOOPBACK,UP,LOWER_UP. mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
inet 127.0.0.1/8 scope host lo
valid_lft forever preferred_lft forever
inet6 ::1/128 scope host
valid_lft forever preferred_lft forever
2: enp1s0: .BROADCAST,MULTICAST,PROMISC,UP,LOWER_UP. mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
link/ether 68:1d:ef:16:9d:16 brd ff:ff:ff:ff:ff:ff
inet 192.168.31.74/24 brd 192.168.31.255 scope global dynamic enp1s0
valid_lft 33867sec preferred_lft 33867sec
inet6 fe80::6a1d:efff:fe16:9d16/64 scope link
valid_lft forever preferred_lft forever
```
找到ip和主机地址一致的那个网卡，例如enp1s0，看后边有没有`PROMISC`字样，没有就是没开启的意思，使用以下命令开启混杂模式：
```
sudo ip link set enp1s0 promisc on
```
然后再通过`ip addr`或者`ifconfig`命令检查是否开启成功。


### **Docker安装OpenWRT**
创建docker网卡
```
docker network create -d macvlan --subnet=192.168.31.0/24 --gateway=192.168.31.1 -o parent=enp1s0 macnet
```
其中--subnet=192.168.31.0/24是网段和子网掩码，--gateway=192.168.31.1是网关（一般是路由器管理页面的ip地址）
可以用以下命令查看是否创建成功：
```
docker network list
```
查看自己系统的架构
```
uname -a
```
安装dock镜像，项目地址：[https://hub.docker.com/r/sulinggg/openwrt](https://hub.docker.com/r/sulinggg/openwrt)，从网址中找到对应自己系统架构的镜像名称(dockerhub或阿里云镜像仓库 (上海))。并安装对应的镜像：
```
docker run --restart always --name openwrt -d --network macnet --privileged registry.cn-shanghai.aliyuncs.com/suling/openwrt:x86_64 /sbin/init
```
上述命令中，`--name openwrt`是指镜像名称，可修改；`--network macnet`中的macnet就是对应的新建网卡名称；`registry.cn-shanghai.aliyuncs.com/suling/openwrt:x86_64`是要安装镜像名称，用dockerhub或阿里云镜像仓库 (上海)都可以。
安装完毕后，开始设置openwrt的ip。进入portainer管理界面，在容器列表里找到openwrt，点击QuickAction列表下的>_，在下一个页面点击连接，进入命令行模式,输入:
```
vim /etc/config/network
```
修改以下部分
```
option ipaddr '192.168.123.100'
option gateway '192.168.123.1'
option dns '192.168.123.1'
```
为：
```
option ipaddr '192.168.31.5'
option gateway '192.168.31.1'
option dns '192.168.31.1'
```
按i进入编辑模式，修改完以后按esc退出，输入以下命令保存
```
:wq
```
重启网络:
```
/etc/init.d/network restart
```
随后访问`http://192.168.31.5/`即可进入openwrt界面，默认密码是：root或者password或者随便输一个也能进入。先修改密码。

### **设置openwrt**
+ 网络-接口-LAN-修改，在基本设置里：把“使用自定义的DNS”清空，基本设置里勾选“忽略此接口”，IPv6设置里，全部禁用，之后保存应用。之后切到物理设置：取消勾选“桥接接口”。
+ 网络-防火墙-自定义规则，在最后添加一行代码：
 `iptables -t nat -I POSTROUTING -j MASQUERADE`
  并用`#`注释掉：
  ```
  #iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 53
  #iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 53
  ```
+ 网络-Turbo ACC 网络加速设置：勾选“DNS缓存”，保存并应用。
+ 到此，整个docker运行openWRT的过程就结束了。


## **如何使用胖路由**

+ 方案1：修改主路由器设置（局域网内全部设备均通过旁路由）
  进入路由器后台中，将主路由的 DHCP 的默认网关和 DNS 服务器设置为openWRT的 IP。完成以上操作后，断开设备（如手机，电脑）与路由器的连接，重新连接路由器。(此方法如果旁路由挂了，会导致整个家里网络都瘫掉)
+ 方案2：修改联网设备设置（特定联网设备通过旁路由）
  以安卓机为例，将IP设置修改为“静态”，IP地址填写一个未被占用的IP，前缀长度填24，DNS1填写openwrt的ip，例如上文中的192.168.31.5。保存，断开网络再重新联网即可（次方法仅设置过的设备使用旁路由，旁路由挂掉对家中其他设备无影响）。

参考文献：https://mlapp.cn/376.html