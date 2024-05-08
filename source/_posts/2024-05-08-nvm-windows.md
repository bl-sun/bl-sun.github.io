---
title: NVM for Windows
date: 2024-05-08 14:52:15
tags:
    - nodejs
    - 编程
categories:
    - 工具
description:
cover: https://www.freecodecamp.org/news/content/images/size/w2000/2022/08/nvmWindows.png
---

# NVM for Windows安装与使用

该软件用于管理nodejs版本

- github: [https://github.com/coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows)

## 常用命令

```powershell
nvm list
```

```powershell
nvm install 18.16.0
```

```powershell
nvm use 18.16.0
```

## 安装

**Step 1 卸载nodejs**

> 参考：[Windows系统完全卸载删除 Node.js](https://blog.csdn.net/dgfdhgghd/article/details/123356831)

清除npm缓存

```powershell
npm cache clean --force
```

在控制面板中卸载nodejs

检查环境变量

**Step 2 下载程序并安装**

从[`nvm-windows/releases`页面](https://github.com/coreybutler/nvm-windows/releases)下载`nvm-setup.exe`并安装即可
