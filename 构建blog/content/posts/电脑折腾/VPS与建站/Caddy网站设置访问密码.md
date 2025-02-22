---
title: Caddy网站设置访问密码
date: 2023-08-23T14:49:00.000Z
categories:
  - VPS与建站
tags:
  - Caddy
  - 建站
draft: false
featuredImage: /blog/电脑折腾/附件/VPS与建站/Caddy建站指南.svg
---
![Caddy建站指南.svg](/blog/电脑折腾/附件/VPS与建站/Caddy建站指南.svg)
**有时候，明明搭建了一个网站，但还不想让无关的人看，咋办？当然是设置访问密码！**

当然有很多这样的应用场景，比方说：
+ 把家里NAS上的某个功能穿透到外网了，但是只想自己能看到。
+ 把宝宝相册放到VPS上了，不想让无关人员看到。
+ 等等

如何设置呢？总不能自己写一个身份验证吧（那也太麻烦了），幸运的是，Caddy自带身份验证的功能。

Caddy支持在访问网站前通过用户名和密码进行身份验证，在配置文件中，密码是以HASH的形式加密的，并不是储存的明文，因此需要先获取自己密码的HASH值：

首先想一个密码，然后在用SSH工具连接上VPS，输入
```
caddy hash-password
```
之后按照提示，输入两遍密码，就会生成一个HASH密码。把这个HASH密码记下来。

修改VPS上的caddyfile（在`/etc/caddy/Caddyfile`），在网站设置里添加basicauth，例如没有身份验证的网站设置是：
```
xl.hotine.wang {
    reverse_proxy 192.168.193.27:2345   #这是一个反向代理
}
```
修改成下边的样子以增加身份验证
```
xl.hotine.wang {
    reverse_proxy 192.168.193.27:2345
    basicauth  {
    admin $2a$14$hWxxxxzQUxzmf0a8pu5NxxxxockwxHxHSjIiQ3KLDnGY67lVXmVw2
    }
}
```
+ admin 是用户名
+ $2a$14$hWxxxxzQUxzmf0a8pu5NxxxxockwxHxHSjIiQ3KLDnGY67lVXmVw2  这一堆是HASH密码
+ HASH通过命令`caddy hash-password yourpassword` 生成

使用以下命令重启caddy即可生效：
```
sudo systemctl stop caddy
sudo systemctl start caddy
```
