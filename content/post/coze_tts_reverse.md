---
title: "Coze_tts_reverse"
date: 2024-04-28T21:22:13+08:00
draft: false
tags: ["TTS","Reverse Engineer","Fun"]
categories: ["Fun"]
---

## Introduction

其实之前就看到coze有对应的voice,它自带的中文tts还挺有意思
我就想了个办法找了一下API

![](/assets/coze_voice.jpg)
请注意, 这里需要获取一个voice token
![](/assets/coze_voice_token.jpg)
直接找到这个接口复制这个token就可以了,
其次还需要找到这个appkey
你可以在对应的websocket请求里面找到这个值
![](tts_ws.jpg)

## POC

我在GITHUB找到了类似的的项目
[CapCut-TTS](https://github.com/kuwacom/CapCut-TTS/)

OK, 我们就从这个项目开始实践
下载安装依赖修改部分设置和环境变量

```bash
npm run test
```

之后开始测试,用curl对本地的express服务发起请求
![](/assets/curl_local_tts.jpg)
可以看到本地tts已经接受到对应的请求并返回了相应的数据

![](/assets/tts_local.jpg)

## Output

![](/assets/local_tts.mp3)

## TODO

- [ ] docker部署
- [ ] 添加更多的speaker
- [ ] 改成终端命令行形式
- [ ] 支持多种语音格式
