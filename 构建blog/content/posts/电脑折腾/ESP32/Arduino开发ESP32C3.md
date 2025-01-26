---
title: Arduino开发ESP32C3
date: 2025-01-26T13:00:21.000Z
categories:
  - ESP32
tags:
  - ESP32
featuredImage: null
draft: false
---
## 配置ArduinoIDE开发环境：
1. 安装IDE，从[[https://www.arduino.cc/]]下载即可。
2. 在IDE中 文件-首选项-其他开发板管理地址 中新增一行`https://arduino.me/packages/esp32.json`
3. 之后在 工具-开发板-开发板管理器... 中搜索ESP32，找到esp32 by Espressif Systems,点安装（需要挂梯子）
4. 之后，便可以在 工具-开发板 中找到esp32的选项并选择对应的开发板了。

## ESP32C3开发板区分与选择
常见的ESP32C3开发板有3种：
- 经典款：有串口芯片，支持IDE调试
- 简约款：无串口芯片，不支持IDE调试
- mini款：尺寸比经典款更短，无串口芯片，不支持IDE调试

## 编写HelloWorld
- 查询芯片端口号：通过USB把芯片接入电脑，在 设备管理器 （本电脑右击-管理-设备管理器）中，展开端口（COM和LPT），查看芯片使用的端口号，例如COM3（如果有多个，可以拔掉芯片看哪个没了就是哪个）
- 打开ArduinoIDE，选择 工具-开发板-esp32-ESP32C3 Dev Module 。
- 工具-端口 选择COM3
- 工具-Flash Mode 选DIO （选此项可以控制开发板上的两个板载LED）
- 工具-USB CDC ON BOOT 选Disabled
- 工具-其他选项默认即可
- 开始编写代码：
```cpp
//定义两个板载LED的PIN值
const int led0 = 13;
const int led1 = 12;

void setup() {
  // put your setup code here, to run once:
  pinMode(led0, OUTPUT);
  pinMode(led1, OUTPUT);
}

void loop() {
  // put your main code here, to run repeatedly:
  digitalWrite(led0, HIGH);
  digitalWrite(led1, LOW);
  delay(1000);
  digitalWrite(led0, LOW);
  digitalWrite(led1, HIGH);
  delay(1000);
}
```
- 点击顶端右箭头图标或 项目-上传 ，即可编译代码，并上传到芯片中。此时可见芯片的两个LED按照1秒的间隔交替闪烁。（注意文件路径不能有中文字符）
