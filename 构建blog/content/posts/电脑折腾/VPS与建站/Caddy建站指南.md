---
title: Caddy建站指南
date: 2023-08-10T16:00:00.000Z
categories:
  - VPS与建站
tags:
  - Caddy
  - 建站
  - 反向代理
  - VPS
featuredImage: /blog/电脑折腾/附件/VPS与建站/Caddy建站指南.svg
---
![Caddy建站指南.svg](/blog/电脑折腾/附件/VPS与建站/Caddy建站指南.svg)

对于在VPS或者家里的小服务器上搭建网站，最轻量级的可以使用基于NodeJS的Express,也可以使用功能强大且出名的Nginx，本文主要讲解Caddy快速在VPS上搭建HTTPS的个人网站。因为Caddy可以自动申请SSL证书，搭建完成直接就是HTTPS网站。

官网：[Caddy官网](https://caddyserver.com/)
GitHub：[caddyserver/caddy](https://github.com/caddyserver/caddy)
GitHub：[caddy官方文档](https://caddyserver.com/docs/)

**部署流程如下：**
+ **安装**：根据官方文档安装就可以，很简单。详见这里：[快速安装](https://caddyserver.com/docs/install)
debian或ubuntu参考以下代码：
    ```bash
    sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
    curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
    curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
    sudo apt update
    sudo apt install caddy
    ```
    当然也可以试一下docker部署，官方也有教程。
+ **配置**：最关键的当然是安装后的配置，Candy的默认配置文件是`/etc/caddy/Caddyfile`,可以直接记事本打开修改。下边是配置示例，实际使用请删除#和后边的注释：
    ```
    hotine.wang { #配置hotine.wang域名的设置
      encode gzip #启用gzip压缩
      log /opt/www/log/caddy.log #日志保存的位置（非必要）
      root * /opt/www #网页文件所在的路径，根据实际情况修改
      tls xxxxx@163.com #通知邮件地址（非必须）
      file_server
    }
    
    movie.hotine.wang { #对这个域名进行重定向
      redir https://hotine.wang/movie #网页重定向（跳转）到指定URL（任URL均可）
    }
    
    pan.hotine.wang,
    cloud.hotine.wang { #这两个域名都使用同样的配置，多个域名中间用逗号隔开
      reverse_proxy 192.168.193.27:5700  #反向代理到192.168.193.27的5700端口
    }
    ```
    修改完毕，重启下caddy即可。在控制台使用以下命令的其中一个。
    ```
    systemctl restart caddy
    或者
    sudo systemctl restart caddy
    ```
    在浏览器输入上述设置完毕的网址，查看是否生效。

+ **测试**：根据caddyfile指定的网页目录`/opt/www`，在这个路径下上传一个index.html网页文件，如果访问hotine.wang能打开，则说明配置成功。同理，如果打开movie.hotine.wang和pan.hotine.wang显示内容都如预期，那就说明，配置都成功了。


其他相关：
[Caddy网站设置访问密码](/posts/电脑折腾/vps与建站/caddy网站设置访问密码)
