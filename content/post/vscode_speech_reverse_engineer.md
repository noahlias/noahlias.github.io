---
title: "Vscode_speech_reverse_engineering"
date: 2024-04-23T18:35:38+08:00
tags: ["Reverse Engineering"]
categories: ["Fun"]
draft: false
---

## Reverse Engineering of VSCode Speech

![](/assets/vscode_speech.png)

### Introduction

简单介绍一下起源哈,我最近在b站看到一个很有意思的[STT](https://en.wikipedia.org/wiki/Speech_recognition)工具
它可以将[视频](https://www.bilibili.com/video/BV19m411o7Dk/)中的语音转换成文字,然后在视频中显示出来,关键效果很好
我就想到了它的原理,应该是[Github Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat)的语音识别功能
之后我就找了它的插件[VsCode Speech](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-speech)

### Embeded Speech Recognition

这是一个本地的嵌入式语音识别用到**MircoSoft**的**Speech SDK**
- [node-speech](https://github.com/microsoft/node-speech)

```javascript
import * as speech from "@vscode/node-speech";

const modelName = "<name of the speech model>";
const modelPath = "<path to the speech model>";
const modelKey = "<key for the speech model>";

// Live transcription from microphone
let transcriber = speech.createTranscriber(
  { modelName, modelPath, modelKey },
  (err, res) => console.log(err, res)
);
// you can stop/start later
transcriber.stop();
transcriber.start();
// later when done...
transcriber.dispose();
```

### Extension Source

![](/assets/extension_directory.png)

基础目录是一个vscode 插件, 之后会包含其他语言的语言包
![](assets/vscode_speech_tree.png)
核心就是这个`dist\main.js` 里面包含了所有的逻辑代码
代码经过最小化和混淆, 但是还是可以通过一些手段还原出来,
大致就是用了一段加密获取model key, 然后调用了`node-speech`的接口,
可以看下这段代码
![](/assets/main.js.png)

## POC

### Model key Get

```typescript
//Generated by GPT-4-turbo
function decrypt(n, e, c, d) {
    // n, e, c, and d are buffers
    // create a sha256 hash
    let hash = crypto.createHash('sha256');
    // update the hash with the data in n
    hash.update(n);
    // digest the hash (i.e., finalize it and prevent further data from being added),
    // producing a resultant buffer
    let key = hash.digest();
    // create a decipher object, specifying aes256 with a gcm mode of operation,
    // use the key derived above and the provided IV
    let decipher = crypto.createDecipheriv('aes-256-gcm', key, c);
    // set the authentication tag to the provided one
    decipher.setAuthTag(e);
    // decrypt the provided encrypted data and finalize the decipher,
    // converting the result to a string
    // let decryptedText = Buffer.concat([decipher.update(d),decipher.final()]).toString();
	let decryptedBuffer = Buffer.concat([decipher.update(d), decipher.final()]);
	let decryptedText = decryptedBuffer.toString();
    return decryptedText;
}
// you would use the function like this:
let decrypted = decrypt(x,end, w, r);
export { decrypted};
```


### Speech Recognition

我还是一个`ts`新手, 所以我在AI辅助工具的帮助下，写了下面的识别代码

```typescript
import { decrypted} from "./modelkey"
import path from "path"
import { createSynthesizer, TranscriptionStatusCode, ITranscriptionCallback, createTranscriber  } from "@vscode/node-speech"
//uuid generate
import { v4 as uuidv4 } from 'uuid';

const modelName = "Microsoft Speech Recognizer zh-CN FP Model V3.1"
const modelPath =  path.join(__dirname, "assets", "stt")
const modelKey = decrypted;

export function wrapperTranscriberCallback(session_id: string, callback: ITranscriptionCallback): ITranscriptionCallback {
  return (error, result) => {
		let resData = result.data ? result.data : "";
		const t = session_id ? session_id : "";
    if (error) {
      console.error(`[speech-${t}] error: ${error.message}`);
      callback(error, result);
    } else {
      switch(result.status) {
      case TranscriptionStatusCode.STARTED:
        console.log(`[speech-${t}] started speech-to-text session${resData}`);
        break;
      case TranscriptionStatusCode.RECOGNIZING:
        console.debug(`[speech-${t}] recognizing: ${resData}`);
        break;
      case TranscriptionStatusCode.RECOGNIZED:
        console.log(`[speech-${t}] : ${resData}`);
        break;
      case TranscriptionStatusCode.NOT_RECOGNIZED:
        console.debug(`[speech-${t}] not recognized${resData}`);
        break;
      case TranscriptionStatusCode.INITIAL_SILENCE_TIMEOUT:
        console.debug(`[speech-${t}] initial silence timeout${resData}`);
        break;
      case TranscriptionStatusCode.END_SILENCE_TIMEOUT:
        console.debug(`[speech-${t}] end silence timeout${resData}`);
        break;
      case TranscriptionStatusCode.SPEECH_START_DETECTED:
        console.debug(`[speech-${t}] speech start detected${resData}`);
        break;
      case TranscriptionStatusCode.SPEECH_END_DETECTED:
        console.debug(`[speech-${t}] speech end detected${resData}`);
        break;
      case TranscriptionStatusCode.STOPPED:
        console.debug(`[speech-${t}] stopped speech-to-text session${resData}`);
        break;
      case TranscriptionStatusCode.DISPOSED:
        console.debug(`[speech-${t}] disposed speech-to-text session${resData}`);
        break;
      case TranscriptionStatusCode.ERROR:
				console.error(`[speech-${t}] error: ${resData}`);
				break;
    default:
      console.log('Unknown transcription status:', result.status);
  }
}}
}
 use settimeout to simulate every 10 seconds to start a new session
 setTimeout(() => {
 		const t = uuidv4();
 const wrappedCallback = wrapperTranscriberCallback(t, (err, res) => console.log(err, res));
 let transcriber = createTranscriber(
   { modelName, modelPath, modelKey },
   wrappedCallback
 );
 	transcriber.start();
 }, 300);

```
### Config

你需要将动态链接库放到对应的包目录下本地才会运行
![](/assets/lib.png)
当然你也可以根据这个库的环境来自己编译打包,参考这个[gyp文件](https://github.com/microsoft/node-speech/blob/main/binding.gyp)

### Usage

```bash
bun run index.ts
```

![](/assets/show.png)

## Summary

这次玩了一下vscode的语音识别插件, 通过逆向工程还原了一下它的代码,但其实它的[`RTF`](https://openvoice-tech.net/index.php/Real-time-factor)还是很好的,
缺点就是语音识别效果感觉不好, 没有达摩院的[FunASR](https://github.com/alibaba-damo-academy/FunASR)模型效果好,也主要是我测试的是中文,
达摩院的缺陷就是安装依赖很大至少3G,微软的这个很轻量,140M左右.
我看官方的SDK里面有提到`TTS`,未来也有可能有本地嵌入式的`TTS`模型,这个也是一个很好的方向,
不过这个STT暂时之支持C++/C#/Java的SDK,它是用[onnxruntime](https://github.com/microsoft/onnxruntime)来进行推理的

## Extra

我们来看这个中文的语音模型里面有个很有意思的东西

![](/assets/profanity.png)

其实这个就是它的过滤词词表,但是我不知道如何解密它
还有一个很有意思的东西,有个`keyword/heycode.table`文件, 这个是服务于嵌入式语音识别的keyword recognition

## Reference

- [speech-to-text](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-to-text)
- [node-speech](https://github.com/microsoft/node-speech)
- [embedded-speech-recognition](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/embedded-speech)
- [vscode-speech](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-speech)
