---
title: Docker宝库：Visual Studio Code
date: 2024-02-07T20:32:00+08:00
categories: [
  "NAS"
]
tags: [
  "Docker",
  "NAS",
  "Python",
  "Nodejs",
  "VSCode"
]
draft: false
---

# 远程开发环境，通过Docker安装Visual Studio Code

通过Docker，可以在远程服务器上安装并运行Visual Studio Code，实现远程编程和开发。这对于需要在不同设备上工作或者希望在安全环境中进行代码开发的开发者来说非常有用。

## 安装

以下是更新后的安装命令：

```bash
docker run -it -d --name vscode -p 8082:8080 \
  -v "/opt/vscode/.config:/home/coder/.config" \
  -v "/opt/vscode/project:/home/coder/project" \
  -u "$(id -u):$(id -g)" \
  -e "DOCKER_USER=$USER" \
  -e PASSWORD='yourpassword' \
  codercom/code-server:latest
```

注意：

* -v "/opt/vscode/.config:/home/coder/.config" 是指将服务器上的 /opt/vscode/.config 目录挂载到Docker容器中的 /home/coder/.config 目录。这样，你可以在容器中使用服务器上的用户配置。
* -v "/opt/vscode/project:/home/coder/project" 是将服务器上的 /opt/vscode/project 目录挂载到容器中的 /home/coder/project 目录，以便在容器中访问和编辑项目文件。
* -u "$(id -u):$(id -g)" 是设置容器内的用户ID和组ID，以匹配服务器上的当前用户，确保文件权限正确。
* -e "DOCKER_USER=$USER" 是设置环境变量，以便在容器内部使用与服务器上相同的用户名。
* -e PASSWORD='yourpassword' 是设置环境变量，用于设置访问code-server的密码。请确保使用安全的密码。

访问 http://VPS_IP:8082 即可远程访问Visual Studio Code。

## 设置中文

进入VSCode之后，界面是英文，需要设置为中文的可以参照以下步骤：

1. 点击最左侧侧边栏的“插件”图标（四个方块），在插件搜索框输入“chinese”并回车，从下方出现的插件列表中选择“Chinese (Simplified) (简体中文) Language Pack for Visual S...”并点击Install安装。
2. 安装完毕后，界面右下角会弹出提示“Would you like to change code-server's display language to Chinese Simplified and restart?”,点击“change language and restart”即可。


## 初始化Git和GitHub
首先进入欢迎界面，如果当前不在欢迎界面，则点击左上角的三道横线-帮助-欢迎

欢迎界面中有一个“克隆Git仓库”，点击后，在上方搜索栏会出现“Clone From Github ...”点击，并在弹出的对话框“扩展"GitHub"希望使用GitHub登录。”点“允许”。之后复制显示的验证码，进入Github网站，登录并输入验证码，并授权后，就OK了。

回到VSCode，在搜索框中选择需要从Github克隆的项目，路径选择到“/home/coder/project/”就可以了。之后，便将这个项目拉取到的本地。修改后，只需要点击界面最左侧的“源码管理器”图标，即可进行修改确认、同步到GitHub。

如果在Commit项目时提示“Make sure you configure your "user.name" and "user.email" in git.”，只需在控制台分别输入以下命令设置下名称与邮箱即可
```bash
git config --global user.name "你的名字"

git config --global user.email "你的邮箱@example.com"
```

至此，VSCode已经基本配置好了。

因为每一个docker容器都相当于是一个微型linux系统，因此可以在VSCode中调出控制台，并在里面安装NodeJS、Python等。

## 安装Hugo
直接在左资源管理器的任一文件右键->在集成终端中打开。或者Ctrl+J打开终端，并依次输入以下三个命令即可
```bash
apt update
apt upgrade
apt install hugo
```

## 安装Python
直接在左资源管理器的任一文件右键->在集成终端中打开。或者Ctrl+J打开终端，并依次输入以下三个命令即可
```bash
apt update
apt upgrade
apt install python3
```
之后在终端输入以下命令测试是否安装成功(安装成功会显示版本号等信息)
```bash
python3 --version

quit()
```
这样可以在终端使用python3来运行py文件。如果想使用python来运行py文件，只需要增加一个软链接：
```bash
sudo ln -s /usr/bin/python3 /usr/bin/python
```

## 安装Nodejs
分别输入以下命令
```bash
sudo apt update

sudo apt install nodejs

sudo apt install npm
```
之后在控制台输入
```bash
node

npm
```
如果能正常显示版本号，那就说明装好了。可以跳过本文后边的安装过程了。

## 其他

由于Visual Studio Code的远程开发特性，你可以在本地机器上通过浏览器访问远程服务器上的VS Code实例，进行代码编辑、运行和调试，就像在本地开发一样。

剪贴板的内容无法直接在远程VS Code中使用，但你可以通过VS Code的剪贴板共享功能来实现复制和粘贴。



