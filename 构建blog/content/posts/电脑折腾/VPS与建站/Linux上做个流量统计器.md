---
title: Linux上做个流量统计器
date: 2025-01-14T14:17:47.000Z
categories:
  - VPS与建站
tags:
  - 脚本自动化
  - 消息推送
  - VPS
featuredImage: null
draft: false
---
VPS流量竟然用超了，于是打算做个流量统计与监控的脚本来自动统计每月和每天的流量使用情况。

查了下资料，Linux系统中，有一个虚拟文件，它提供了系统中所有网络接口的详细统计信息。这个文件就是`/proc/net/dev`。
打开这个文件，内容大致像：
```
Inter-|   Receive                                                |  Transmit
 face |bytes    packets errs drop fifo frame compressed multicast|bytes    packets errs drop fifo colls carrier compressed
    lo: 3304479918 6432909    0    0    0     0          0         0 3304479918 6432909    0    0    0     0       0          0
enp1s0: 118598215588 160465104    0 10015148 1390     0          0   9098966 139289884592 177958470    0    0    0    14       0          0
enp2s0:       0       0    0    0    0     0          0         0        0       0    0    0    0     0       0          0
...
```
用Excel表格整理下，就是下表的样子（展示了两个设备中的`/proc/net/dev`文件的样子）
![Linux上做个流量统计器.png](/blog/电脑折腾/附件/VPS与建站/Linux上做个流量统计器.png)
- **Inter-face**是接口名称。
- **Receive**：接收数据的统计信息。
    - **bytes**：接收的字节数。
    - **packets**：接收的数据包数。
    - **errs**：接收时出现的错误数。
    - **drop**：接收时丢弃的数据包数。
    - **fifo**：接收时因FIFO（先进先出）队列溢出而丢弃的数据包数。
    - **frame**：接收时因帧错误而丢弃的数据包数。
    - **compressed**：接收时压缩的数据包数。
    - **multicast**：接收的多播数据包数。
- **Transmit**：发送数据的统计信息。
    - **bytes**：发送的字节数。
    - **packets**：发送的数据包数。
    - **errs**：发送时出现的错误数。
    - **drop**：发送时丢弃的数据包数。
    - **fifo**：发送时因FIFO队列溢出而丢弃的数据包数。
    - **colls**：发送时发生的碰撞数（仅适用于以太网）。
    - **carrier**：发送时因载波问题而丢弃的数据包数。
    - **compressed**：发送时压缩的数据包数。

一般设备中，enth0或者enp1s0就是网卡的接口名称。`lo`是本地回环接口（loopback interface），也被称为环回接口。

要分析流量只需解析出Receive中bytes 和 Transmit中bytes 的数值即可。

所以就可以用这个函数

```js
/**
 * 分析各网络接口的上下行流量
 * @returns 返回一个数组，包含每个网络接口的名称、接收的字节数、发送的字节数。类似于[['网络接口1',1029339,3343434],['网络接口2'，3454223,3553211]]
 */

function readNetworkStats() {
    const statsPath = '/proc/net/dev';
    //读取网络接口信息文件
    const stats = fs.readFileSync(statsPath, 'utf-8');
    //按行进行分割
    const lines = stats.split('\n');
    //数组，包含每个网络接口的名称，接收的字节数，发送的字节数。
    const result = [];
    //从第三行开始分析（因为前两行是标题）
    for (let i = 2; i < lines.length; i++) {
        //移除字符串最开头的空白字符
        lines[i] = lines[i].trimStart();
        //按照一个或多个空白字符进行分割，并将分割后的结果存储在数组 parts 中
        const parts = lines[i].split(/\s+/);
        if (parts.length > 1) {
            const interfaceName = parts[0].replace(':', '');
            //这里可以打印出全部的网口，如果不知道改用哪个网口，可以去掉下边的注释查看自己的网口是多少。
            //console.log("[D]网口:", [parts[0].replace(':', ''), parseInt(parts[1]), parseInt(parts[9])]);
            if (interfaceName != netName)
                continue;
            result.push([interfaceName, parseInt(parts[1]), parseInt(parts[9])]);
        }
    }
    return result;
}
```

完整代码：

```js
/**
 * 定时检测主机指定网口的上下行流量情况。
 * 需要再下边配置netName常量，指定网口名称
 * 如果不知道自己的网口名称，可以在38行左右，去掉那个console。log的注释，产看自己的网口名称。
 * 定位到脚本目录下安装执行：
 * npm install schedule
 * 
 * v2025-01-21
*/

const fs = require('fs');
const path = require('path');
const schedule = require('node-schedule')
//要监控的网络名称
const netName = "eth0";

/**
 * 分析各网络接口的上下行流量
 * @returns 返回一个数组，包含每个网络接口的名称、接收的字节数、发送的字节数。类似于[['网络接口1',1029339,3343434],['网络接口2'，3454223,3553211]]
 */
function readNetworkStats() {
    const statsPath = '/proc/net/dev';
    //读取网络接口信息文件
    const stats = fs.readFileSync(statsPath, 'utf-8');
    //按行进行分割
    const lines = stats.split('\n');
    //数组，包含每个网络接口的名称，接收的字节数，发送的字节数。
    const result = [];
    //从第三行开始分析（因为前两行是标题）
    for (let i = 2; i < lines.length; i++) {
        //移除字符串最开头的空白字符
        lines[i] = lines[i].trimStart();
        //按照一个或多个空白字符进行分割，并将分割后的结果存储在数组 parts 中
        const parts = lines[i].split(/\s+/);
        if (parts.length > 1) {
            const interfaceName = parts[0].replace(':', '');
            //这里可以打印出全部的网口，如果不知道改用哪个网口，可以去掉下边的注释查看自己的网口是多少。
            //console.log("[D]网口:", [parts[0].replace(':', ''), parseInt(parts[1]), parseInt(parts[9])]);
            if (interfaceName != netName)
                continue;
            result.push([interfaceName, parseInt(parts[1]), parseInt(parts[9])]);
        }
    }
    return result;
}

function logFilePath() {
    return path.join(__dirname, 'network_traffic.log');
}

/**
 * 记录当前上下行流量到log文件
 * @param {*} trafficData 
 * @returns 
 */
function logTraffic(trafficData) {
    if (trafficData == null || trafficData.length < 1) {
        console.log("[E]:信息为空，未向log文件写入任何内容");
        return;
    }
    const now = new Date();
    const logEntry = `${now.toISOString().slice(0, 19).replace('T', ' ')} ${trafficData[0][1]} ${trafficData[0][2]}\n`;
    fs.appendFileSync(logFilePath(), logEntry);
}

/**
 * 计算每日流量数据。找到日期等于当天的第一条和最后一条数据，进行比对
 * @returns 
 */
function calculateDailyTraffic() {
    let todayReceived = 0;
    let todaySent = 0;
    let monthlyReceived = 0;
    let monthlySent = 0;

    const today = new Date().toISOString().slice(0, 10);
    //读取日志
    const logArray = fs.readFileSync(logFilePath(), 'utf-8').split('\n');
    //读取日期为今天的记录数组
    let logEntries = logArray.filter(line => line.startsWith(today));
    //获取当日最后一条log
    const [lastEntry] = logEntries.slice(-1);
    const [lastReceived, lastSent] = lastEntry.split(' ').slice(2).map(Number);
    if (logEntries.length != 0) {
        //获取当日第一条log
        const [firstEntry] = logEntries;
        const [firstReceived, firstSent] = firstEntry.split(' ').slice(2).map(Number);

        //计算日累计
        todayReceived = lastReceived - firstReceived,
            todaySent = lastSent - firstSent

        /*
        console.log("日统计：");
        console.log("从",firstReceived, firstSent);
        console.log("到",lastReceived, lastSent);
        console.log("=",lastReceived - firstReceived, lastSent - firstSent);*/
    }

    const nowDate = new Date()
    //获取最近的一个28号的日期
    const targetDay = new Date(nowDate.getFullYear(), nowDate.getMonth(), 28);
    if (targetDay > nowDate) targetDay.setMonth(targetDay.getMonth() - 1);

    //获取最近一个28日的全天的log
    logEntries = fs.readFileSync(logFilePath(), 'utf-8').split('\n').filter(line => line.startsWith(targetDay.toISOString().substring(0, 10)));

    //
    if (logEntries.length != 0) {
        //获取第一条log
        const [firstEntry] = logEntries;
        const [firstReceived, firstSent] = firstEntry.split(' ').slice(2).map(Number);

        //计算日累计
        monthlyReceived = lastReceived - firstReceived,
            monthlySent = lastSent - firstSent

        /*
        console.log("月统计：");
        console.log("从",firstReceived, firstSent);
        console.log("到",lastReceived, lastSent);
        console.log("=",lastReceived - firstReceived, lastSent - firstSent);*/
    }
    return {
        todayReceived: todayReceived,
        todaySent: todaySent,
        monthlyReceived: monthlyReceived,
        monthlySent: monthlySent
    }
}


/**
 * 发送推送消息到ntfy服务
 * @param {string} topic - 消息的主题（必填）
 * @param {string} message - 要发送的消息内容（必填）
 * @param {string} title - 消息的大标题(默认不使用大标题)
 * @param {int} [priority=3] - 消息的优先级（默认是3），可以是1-5的整数，分别是最小、小、默认、大、最大
 * @param {array} [tags] - 消息的标签,字符串数组（默认无）
 * @param {array} [attach] - 附件、图片URL（默认无）
 * @param {array} [click] - 消息被点击时跳转的url（默认无）
 * @param {string} [serverUrl='https://ntfy.sh'] - ntfy服务的URL,默认为官方服务器
 */
async function sendNtfyMessage(topic, message, title = null, priority = 3, tags = null, attach = null, click = null, serverUrl = 'https://ntfy.sh') {
    try {
        if (topic == null || message == null || priority > 5 || priority < 1) {
            console.error("topic、message不能为空，priority的值只能取1、2、3、4、5!");
        }

        // 构建请求的headers
        const headers = new Headers({
            'Content-Type': 'application/json',
        });

        // 创建消息Object
        const payload = { topic, message, priority };
        if (title) payload.title = title;
        if (tags) payload.tags = tags;
        if (attach) payload.attach = attach;
        if (click) payload.click = click;

        // 构建请求的body
        const body = JSON.stringify(payload);
        console.log('拟发出的消息body:', body);

        // 发送POST请求到ntfy服务
        const response = await fetch(serverUrl, { method: 'POST', headers: headers, body: body });

        // 检查响应状态
        if (!response.ok) {
            throw new Error(`HTTP 错误! 状态: ${response.status}`);
        }

        // 获取响应数据
        const data = await response.json();
        console.log('消息发送成功:\n', data);
    } catch (error) {
        console.error('消息发送失败:\n', error);
    }
}

/**
 * 发送推送消息到ntfy服务
 * @param {string} topic - 消息的主题（必填）
 * @param {string} message - 要发送的消息内容（必填）
 * @param {string} title - 消息的大标题(默认不使用大标题)
 * @param {int} [priority=3] - 消息的优先级（默认是3），可以是1-5的整数，分别是最小、小、默认、大、最大
 * @param {array} [tags] - 消息的标签,字符串数组（默认无）
 * @param {array} [attach] - 附件、图片URL（默认无）
 * @param {array} [click] - 消息被点击时跳转的url（默认无）
 * @param {string} [serverUrl='https://ntfy.sh'] - ntfy服务的URL,默认为官方服务器
 */
async function sendNtfyMessage(topic, message, title = null, priority = 3, tags = null, attach = null, click = null, serverUrl = 'https://ntfy.sh') {
    try {
        if (topic == null || message == null || priority > 5 || priority < 1) {
            console.error("topic、message不能为空，priority的值只能取1、2、3、4、5!");
        }

        // 构建请求的headers
        const headers = new Headers({
            'Content-Type': 'application/json',
        });

        // 创建消息Object
        const payload = { topic, message, priority };
        if (title) payload.title = title;
        if (tags) payload.tags = tags;
        if (attach) payload.attach = attach;
        if (click) payload.click = click;

        // 构建请求的body
        const body = JSON.stringify(payload);
        //console.log('拟发出的消息body:', body);

        // 发送POST请求到ntfy服务
        const response = await fetch(serverUrl, { method: 'POST', headers: headers, body: body });

        // 检查响应状态
        if (!response.ok) {
            throw new Error(`HTTP 错误! 状态: ${response.status}`);
        }

        // 获取响应数据
        const data = await response.json();
        //console.log('消息发送成功:\n', data);
    } catch (error) {
        console.error('消息发送失败:\n', error);
    }
}

/**
 * 字节数转MB
 * @param {*} bytes 
 * @returns 
 */
function bytesToMB(bytes) {
    return Math.round(bytes / (1024 * 1024) );
}

function main() {
    const trafficData = readNetworkStats();
    logTraffic(trafficData);

    const traffic = calculateDailyTraffic();

    //生成报告
    let report = '';
    report += `🌐${new Date().getDate()}日累计流量:\n 上 ${bytesToMB(traffic.todaySent)} Mb, 下 ${bytesToMB(traffic.todayReceived)} Mb, 共计${bytesToMB(traffic.todaySent + traffic.todayReceived)}Mb\n`;
    report += `🌐自28日起累计流量:\n 上 ${bytesToMB(traffic.monthlySent)} Mb, 下 ${bytesToMB(traffic.monthlyReceived)} Mb, 共计${bytesToMB(traffic.monthlySent + traffic.monthlyReceived)}Mb`;
    console.log(report);
    //日流量超过1000MB发送消息提示
    if(bytesToMB(traffic.todaySent + traffic.todayReceived)>1000)
    {
        sendNtfyMessage('hotine',report,'BWG流量统计',3,['loudspeaker','BWG'])
    }
}


main();

schedule.scheduleJob("10 * * * *", main
)



```
