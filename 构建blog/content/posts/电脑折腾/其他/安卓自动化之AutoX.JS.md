---
title: 安卓手机自动化神器AutoX.JS
date: 2023-09-17T13:36:00.000Z
categories:
  - 其他
tags:
  - AutoJS
  - VSCode
  - 脚本自动化
draft: false
---

## 安卓手机自动化神器AutoX.JS
Auto.js 是一款基于 Android 平台的自动化测试和脚本编写工具。它允许用户编写脚本来模拟用户在 Android 设备上的操作，例如点击、滑动、输入文字等，以便自动执行各种任务或测试应用程序。
因为功能太过强大，被一些灰色产业所利用，最后导致Auto.js被强制关停下架。而Autox.js就是根据Auto.js的源码，被继承并二次开发的接替版APP。
### AutoX.js安装
Autox.js在GitHub上地址为[kkevsekk1/AutoX](https://github.com/kkevsekk1/AutoX),右侧Releases中可以找到apk安装包下载地址。
### 初始设置
在手机完成之后，按照提示，打开“无障碍服务”，在APP左侧菜单中，开启“悬浮窗”（这个用起来特别方便）。
悬浮窗点开后，有5个按钮，分别是：
+ 运行脚本：这里可以快速查看已有的脚本，并选择运行、编辑等操作
+ 录制脚本：需要root
+ 布局分析：包括布局范围分析和布局层次分析。尤其是布局范围分析这个功能会经常用到，在编写脚本时，如果需要获取页面中一个元件的位置、id等信息，可以是用这个功能。
+ 停止当前全部脚本：在脚本出现问题或者想关闭正在运行的脚本时，可以快速停止。
+ 更多：包括打开APP主页面、关闭悬浮窗等功能。
### 编写脚本
直接点击APP右上角+即可新建脚本或者新建项目。之后点开，即可编辑、运行脚本。
不过在手机上直接编写脚本效率还是比较低，因此，最好是在电脑上进行脚本编写。这就需要大名鼎鼎的VSCode！

## VisualStudioCode安装与配置开发环境
### 安装
直接去官网下载安装就好啦，地址是[code.visualstudio.com/](https://code.visualstudio.com/)。
### 设置中文界面
默认安装完毕是英文界面的，需要按快捷键“Ctrl+Shift+P”,在出现的搜索款中输入`configure display language`并回车，选择中文并安装。重启VSCode后，即可生效。
### 安装AutoJS开发插件
在VS Code中菜单"查看"->"扩展"->输入"Autox.js"搜索，即可看到"Autox.js-VSCodeExt"插件，安装即可。
### 激活AutoX.JS插件服务
按 Ctrl+Shift+P 或点击"查看"->"命令面板"可调出命令面板，输入 Autoxjs 可以看到几个命令，移动光标到命令Auto.js Autoxjs: Start All Server，按回车键执行该命令。
此时VS Code会在右下角显示 "Auto.js server running..." ，即开启服务成功。
### USB连接手机进行开发调试
+ 首先需要先启动或安装ADB服务。ADB驱动的下载页面是：[ADB(中国站)](https://developer.android.google.cn/studio/releases/platform-tools) 或 [ADB(国际站)](https://developer.android.com/studio/releases/platform-tools)，下载最新版后，解压到电脑目录，然后在adb所在的目录打开终端或power shell，运行命令`./adb start-server`。回显`* daemon started successfully`即是启动ADB驱动成功。
+ 并在手机中打开“开发者模式”,随后在开发者选项中，启动“USB调试模式”（在搭载 Android 4.2 及更高版本的设备上，“开发者选项”屏幕默认情况下处于隐藏状态。如需将其显示出来，请依次转到设置 > 关于手机，然后点按版本号七次。返回上一屏幕，在底部可以找到开发者选项。在某些设备上，“开发者选项”屏幕所在的位置或名称可能有所不同。）
+ 在确保手机已经在开发者选项中打开USB调试后，在Autox.js的侧拉菜单中启用ADB调试，再使用数据线连接电脑，插件会自动识别设备。
### 无线连接手机进行开发调试
+ 将手机连接到电脑启用的Wifi或者同一局域网中。在Autox.js的侧拉菜单中启用“连接电脑”，并输入电脑上VS Code右下角铃铛图标上显示的IP地址和端口号，等待连接成功。也可以点击VS Code右下角"Auto.js server running..."通知的下方按钮 "Show QR code" 或按 Ctrl+Shift+P 搜索执行Show qr code命令，然后用Autox.js扫码连接。
### 编辑并运行脚本
在VS Code中编辑JavaScript文件，并通过命令Run或者按键F5在手机上运行测试。
例如，在JS文件中输入：
```
toast("RunRunRun!")
```
保存后，按F5运行。
如果此时手机上有气泡显示`RunRunRun!`,即表示环境配置成功，可以开始编码了！
