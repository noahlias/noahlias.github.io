---
title: "How to Write a VSCode Extension"
date: 2025-08-14T17:32:38+08:00
tags: ["Programming", "VSCode", "Extension"]
categories: ["Fun", "Life"]
draft: false
---

# How to Write a VSCode Extension

已经很久没有写博客了，今天来写一个关于如何开发 VSCode 插件的简单介绍。本来是想用英文来写的，但是为了简短描述`VSCode`的插件开发，还是用中文来写吧。

## 背景

最近工作内容有用到`nc`文件，具体是[GCODE](https://en.wikipedia.org/wiki/G-code)的一个文件格式，有一些在线的
[ncviewer](https://nc-viewer.ncnetic.com/)
可以看但是都不是足够好用，就调研了一下同类型的都在 Windows 上有，但是也没有更好的，于是想了想不如把其中一个用
[Threejs](https://threejs.org/) 的用 **webview**
包裹一下放到 VSCode 里面，这样就可以在 VSCode 里面直接查看了。项目已经开源在[GitHub](https://github.com/noahlias/nc_view_vscode)

## 脚手架

vscode 提供了一个脚手架工具，可以通过以下命令来创建一个新的插件项目：

```bash
npm install -g yo generator-code
yo code
```

发布脚手架`vsce`，可以通过以下命令来安装：

```bash
npm install -g vsce
```

发布插件可以通过以下命令：

```bash
vsce package
vsce publish
```

## 核心实现

大部分代码是 AI 辅助，逻辑是自己想的，就是要打开一个 nc 文件就可以 view

- 注册绑定一个 vscode 的 command 注意在 package 里面注册好对应的 context
- 创建一个 webview panel, 定义好和 webview 通信的消息格式
- 在 webview 中加载 Threejs 的代码，注意要把相关的资源文件放到合适的位置，并在 webview 中正确引用
- 在 webview 中处理用户交互，比如加载文件、渲染模型等
- 处理 webview 和 VSCode 之间的通信，比如发送文件路径、接收渲染结果等 (主要是高亮等)

```javascript
vscode.workspace.openTextDocument(uri).then(doc => {
        const fileContent = doc.getText();
        panel.webview.postMessage({
          type: 'loadGCode',
          ncText: fileContent
        });

        // 获取对应的编辑器
        let targetEditor = null;

        // 查找对应文档的编辑器
        function findTargetEditor() {
          return vscode.window.visibleTextEditors.find(editor =>
            editor.document.uri.toString() === uri.toString()
          );
        }
```

## 注意

- 发布插件需要在微软软件市场注册 publisher 并获取对应的 PAT(personal access
  token)
- 插件发布有个 changelog 需要配置等
- 插件的版本号需要遵循语义化版本控制（semver）规范
- 插件的图标和描述需要符合微软的规范，建议使用 SVG 格式

## 相关文档

- [VSCode 插件开发指南](https://code.visualstudio.com/api/get-started/your-first-extension)
- [VSCode Webview API](https://code.visualstudio.com/api/extension-guides/webview)
- [VsCode](https://code.visualstudio.com)
