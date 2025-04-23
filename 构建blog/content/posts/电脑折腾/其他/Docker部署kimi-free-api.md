---
title: Docker部署kimi-free-api
date: 2025-04-23T03:28:58.000Z
categories:
  - 其他
tags:
  - 脚本自动化
  - 消息推送
  - AI
  - Docker
featuredImage: null
draft: false
---

### 简介
🚀 KIMI AI 长文本大模型逆向API【特长：长文本解读整理】，支持高速流式输出、智能体对话、联网搜索、探索版、K1思考模型、长文档解读、图像解析、多轮对话，零配置部署，多路token支持，自动清理会话痕迹，仅供测试，如需商用请前往官方开放平台。
[GitHub项目页面](https://github.com/LLM-Red-Team/kimi-free-api)

### Docker部署

```bash
docker run -it -d --init \
--name kimi \
-p 7086:8000 \
-e TZ=Asia/Shanghai \
--restart=unless-stopped \
vinlic/kimi-free-api:latest
```
之后可访问http://localhost:7086进行测试

### 获取KIMI refresh_token

从 [kimi.moonshot.cn](https://kimi.moonshot.cn/) 获取refresh_token

进入kimi随便发起一个对话，然后F12打开开发者工具，从`Application应用 > Local Storage本地存储空间`中找到`refresh_token`的值，这将作为Authorization的Bearer Token值：`Authorization: Bearer TOKEN`

如果你看到的`refresh_token`是一个数组，请使用`.`拼接起来再使用。

目前kimi限制普通账号每3小时内只能进行30轮长文本的问答（短文本不限），你可以通过提供多个账号的refresh_token并使用`,`拼接提供：

`Authorization: Bearer TOKEN1,TOKEN2,TOKEN3`

每次请求服务会从中挑选一个。

### 代码开发

代码用法示例，其他用法详见官方文档，很详细。

```js
const axios = require('axios');
// 替换为你的refresh_token，获取方法见GitHub页面说明
const REFRESH_TOKEN = "eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ1c2VyLWNlbnRlciIsImV4cCI6MTc1MDU2MTg4NywiaWF0IjoxNzQyNzg1ODg3LCJqdGkiOiJjdmdjcW5ydWEzZDVsb3RsM2U4ZyIsInR5cCI6InJlZnJlc2giLCJhcHBfaWQiOiJraW1pIiwic3ViIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAANzAiLCJzcGFjZV9pZCI6ImNtcWkxdDAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAl9pZCI6ImNtcWkxdDAzcjA3Y2pzbGs1djYwIiwicm9sZXMiOlsidmlkZW9fZ2VuX2FjY2VzcyJdfQ.Yw07vOZfAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA48w3llR4BAvtpGmqh-_WOJ6NPC70Gj76vc5A";
const API_URL = "https://kimi.xxxxxx.wang/v1/chat/completions"; // 如果是本地部署，使用localhost:8000；如果是远程部署，替换为实际的API地址
var CONVERSATION_ID = "none";//会话ID，开启新会话为none
  
/**
 *
 * @param {string} content 会话正文
 * @param {string} conversation_id 会话ID，可实现多轮上下文会话
 * @returns Object,id属性为会话id，msg属性为AI答复内容
 */
async function askAI(content, conversation_id = "none") {
  try {
    // 请求头
    const headers = {
      "Authorization": `Bearer ${REFRESH_TOKEN}`,
      "Content-Type": "application/json"
    };

    // 请求体
    const requestBody = {
      model: "kimi", // 使用默认模型
      conversation_id: conversation_id, // 如果是第一轮对话，不传conversation_id
      messages: [
        {
          role: "user",
          content: content
        }
      ],
      use_search: true, // 根据需求开启联网搜索
      stream: false // 不使用流式输出
    };

    // 发送POST请求
    const response = await axios.post(API_URL, requestBody, { headers });
    // 提取响应中的输出文本
    //console.log("API响应：");
    //console.log(response.data);
    //console.log(response.data.choices[0].message);
    //AI回答正文
    let outputText = response.data.choices[0].message.content;
    //移除引用
    if(outputText.split("\n搜索结果来自：\n")[0]){
      outputText=outputText.split("\n搜索结果来自：\n")[0];
    }

    return {
      id: response.data.id,
      msg: outputText
    };
  } catch (error) {
    console.error("POST到API时发生错误:", error.message);
    throw error; // 抛出错误以便调用者处理
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
 * 把一个md格式的字符串转换成emoji的字符串
 */
function md_to_txt(string) {
    // 删除引用（假设引用是用 `>` 开头的行）
    string = string.replace(/^>.*\n?/gm, '');
    // 将 ### 替换为 🔶
    string = string.replace(/^####\s*/gm, '🔸 ');
    string = string.replace(/^###\s*/gm, '🔶 ');
    // 删除 Markdown 中的标题格式（如 #、## 等，但保留换行）
    string = string.replace(/^#{1,2}\s*/gm, '');
    // 删除 Markdown 中的加粗、斜体、下划线、删除线等格式
    string = string.replace(/\*\*|\*|__|_|~~/g, '');
    // 删除 Markdown 中的链接格式 [text](url)
    string = string.replace(/\[.*?\]\(.*?\)/g, '');
    // 保留无序列表符号，但将其替换为普通文本
    string = string.replace(/^\s*[-*+]\s/gm, '- ');
    // 保留有序列表符号
    string = string.replace(/^\s*(\d+\.)\s/gm, '$1 ');
    // 删除引用角标
    string = string.replace(/\[\^[^\]]+\^]/g, '');
    // 删除多余的空格（但保留换行和缩进）
    //string = string.replace(/\s+/g, ' ').trim();

    return string;
}
  

// 调用
(async () => {
  try {
    const result = await askAI("分析一下当前国内上海金价格，较上日变化，近期走势(包含影响金价走势的原因)。预测一下后续黄金价格是涨还是跌。给出投资建议。");
    let msg = result.msg;
    CONVERSATION_ID = result.id;
    //console.log(`AI回答: ${msg}\nc_id:${CONVERSATION_ID}`);
    sendNtfyMessage("hotine",md_to_txt(msg),"金价分析",3,["💰"]);
  } catch (error) {
    console.error("Error in askAI:", error);
  }
})();
```
