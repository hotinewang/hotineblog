---
title: NTFY消息推送
date: 2024-12-27T07:51:00.000Z
categories:
  - 其他
tags:
  - 消息推送
  - 脚本自动化
  - ntfy
draft: false
featuredImage: /blog/电脑折腾/附件/其他/NTFY消息推送.jpg
---
研究了好几天，作为消息推送、订阅服务，首先查到的就是基于MQTT协议的相关服务，包括出名的EMQX、Mosquitto等，但是研究了半天，这些东西主要用于物联网，安卓缺少易用的客户端APP，最终发现了基于HTTP的消息推送服务NTFY。
官网:https://ntfy.sh/

NTFY不需要复杂的配置，甚至不需要注册或者部署服务，仅仅通过访问一个url就能推送消息（GET方式，当然通过POST方式也可以）。
就像这样:
http://ntfy.sh/newtopic/publish?message=这是一条新消息
或者复杂一点:
http://ntfy.sh/newtopic/publish?message=这是一条新消息&priority=5&title=大标题&tags=skull
就能成功在名为newtopic的话题下，发送一条内容为“这是一条新消息”的信息，并且指定了优先级为5(非常紧急)，标题为“大标题”，还有一个名为skull的标签。
![NTFY消息推送.jpg](/blog/电脑折腾/附件/其他/NTFY消息推送.jpg)

客户端也非常贴心的给出了网页版（[[https://ntfy.sh/app]]）、渐进式网页应用（PWA）和APP版（就叫ntfy），整体都非常的简洁易用，只有订阅话题、接收消息两个功能,网页和PWA版还有发布话题的功能。

服务端可以直接使用官网提供的公共服务器，也可以通过docker创建自托管服务器。
（其实公共服务器就够用了）
自建的话命令行如下:
```bash
docker run \
  --name ntfy
  -v /path/to/ntfy/config:/etc/ntfy \
  -e TZ=Asia/Shanghai \
  -p 1800:80 \
  -it \
  binwiederhier/ntfy \
  serve
```
之后访问：主机地址+端口号进行后台管理，例如localhost:1800
## 发布消息

怎样发布一条消息呢，可以通过webhook(GET)方式发送消息，就像上文那样，通过访问url即可。
也可通过POST方式进行发布，例如这个简单的javascript函数:

```JavaScript
/**
 * 发送推送消息到ntfy服务
 * @param {string} topic - 消息的主题
 * @param {string} message - 要发送的消息内容
 * @param {string} title - 消息的大标题(默认不使用大标题)
 * @param {int} [priority=3] - 消息的优先级，可以是1-5的整数，分别是最小、小、默认、大、最大
 * @param {array} [tags] - 消息的标签,字符串数组。
 * @param {array} [attach] - 附件、图片URL。
 * @param {array} [click] - 消息被点击时跳转的url。
 * @param {string} [serverUrl='https://ntfy.sh'] - ntfy服务的URL,默认为官方服务器
 */

async function sendNtfyMessage(topic, message, title = null,  priority = 3, tags = null, attach = null , click = null , serverUrl = 'https://ntfy.sh') {
  try {
    if(topic==null || message==null || priority >5 ||priority <1){
      console.error("topic、message不能为空，priority的值只能取1、2、3、4、5!");
    }

    // 构建请求的headers
    const headers = new Headers({
      'Content-Type': 'application/json',
    });

    // 创建消息Object
    const payload={topic,message,priority};
    if(title)  payload.title = title;
    if(tags) payload.tags = tags;
    if(attach) payload.attach = attach;
    if(click) payload.click = click;
  
    // 构建请求的body
    const body = JSON.stringify(payload);
    console.log('拟发出的消息body:', body);
  
    // 发送POST请求到ntfy服务
    const response = await fetch(serverUrl, { method: 'POST', headers: headers, body: body });

    // 检查响应状态
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    // 获取响应数据
    const data = await response.json();
    console.log('Message sent successfully:', data);
  } catch (error) {
    console.error('Failed to send message:', error);
  }
}


// 使用示例

sendNtfyMessage('newtopic','这是一条测试用的MSG', '测试信息',4,['loudspeaker','skull','hugging_face']);

```

其他方式发送消息以及参数，详见官方文档[[https://ntfy.sh/doc]]。

**注意一个坑** ：
使用普通的发送方式时，需要把消息推送至`https://ntfy.sh/topic_name`(topic_name是话题名称)，而使用JSON数据格式的方式发送时，需要推送的网址应该是`https://ntfy.sh`（不能带话题名），在JSON数据中单独指定话题名`topic:topic_name` 。

## 接收消息

下载客户端，或使用网页客户端，选择“Subscribe to topic”（订阅话题），输入话题名称，就可以了。
