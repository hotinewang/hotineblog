---
title: Linux中安装NodeJS并用Express搭建静态网站
date: 2023-05-10T16:00:00+08:00
categories: [
  "VPS与建站"
]
tags: [
  "NodeJS",
  "Express",
  "建站"
]

---

## NODEJS部署
### 下载Nodejs
先在服务器上下载nodejs包，在控制台通过`cd`命令进入usr/local目录：
```
cd /usr/local
```
在nodejs.org官网找到nodejs的linux LTS（长期支持）版本下载地址，并复制，在控制台输入:
```
wget https://nodejs.org/dist/nodejsxxxxxxx.tar.xz
```
解压tar文件:
```
tar -xvf nodejsxxxxxxx.tar.zx
```
修改目录名称为node
```
mv /usr/local/nodejsxxxxxxx /usr/local/node
```
可以尝试查看nodejs版本号确定是否能正常运行
```
/usr/local/node/bin/node -v
```
回显：V版本号说明node能正常使用。
### 配置环境变量
向/etc/profile文件中写入nodejs模块所在目录路径
```
cat >> /etc/profile >>EOF
export PATH=$PATH:/usr/local/node/bin
```
也可以用SSH工具把/etc/profile文件下载到电脑桌面，用记事本在最后一行加入下面的内容然后再上传回去覆盖原文件：
```
export PATH=$PATH:/usr/local/node/bin
```
最后执行source命令让配置生效
```
source /etc/profile
```
配软连接，相当于全局变量
```
ln -s /usr/local/node/bin/node /usr/local/bin/
ln -s /usr/local/node/bin/npm /usr/local/bin/
```
然后在控制台输入以下两个命令，测试是否配置nodejs成功。配置成功的话会回显版本号
```
node -v
npm -v
```
此时，NodeJS就完全配置好了。
## Express搭建最简单的静态网站
chatGPT对Express的介绍为：Express.js是一种用于构建Web应用程序和API的开源Web应用程序框架。它基于Node.js平台，使用JavaScript语言编写。Express提供了一组简化和抽象的工具，使开发者能够更轻松地处理路由、中间件、请求和响应等方面的任务。虽然Express本身是一个精简的框架，但通过结合其他Node.js模块和工具，开发者可以构建出功能强大且高性能的Web应用程序。它在Node.js开发社区中非常流行，被广泛用于构建各种规模和类型的Web项目。
在配置好NODEJS的基础上，在服务器上新建一个文件夹，作为网站目录，并创建app.js、和index.html两个文件。
+ index.html是网站的主页文件，内容任意。
+ app.js内容如下：
    ```
    let express = require("express"), app = express();
    // 可以同时设置多个别名访问路径：
    // app.use("/", express.static(__dirname + "/"));//配置静态网页访问目录【前端代码就放这个文件夹】（用于外部访问http://127.0.0.1/）
    // app.use("/upload", express.static(__dirname + "/upload"));//配置静态资源文件目录路径（用于外部访问http://127.0.0.1/upload/资源文件）
    // 上面的写法等同于↓
    app.use(express.static("./"));
    const port = 8080;
    //端口8080可根据实际情况修改
    app.listen(port, () => console.log(`网站服务器启动，访问端口${port}`));
    ```
通过cd命令定位到网站目录，例如：`cd /path/to/code/`,然后在控制台运行:
```
npm i express &npm i forever -g & forever start app.js
```
这段代码会自动使用npm安装express插件和forever插件，并用forever插件对app.js进程进行守护，防止app.js程序进程被结束。
之后在浏览器打开ip地址+端口号即可访问到index.html文件的内容，例如192.168.X.X:8080
一个最简单的静态网站搭建完成。