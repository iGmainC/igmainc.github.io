---
title: PowerShell中的wget
tags: PowerShell Windows
---

事情的起因是要在 Win10子系统 上安装 code-server，要下载 rpm 安装包，子系统没办法直接用 Win 上的代理，所以我就准备在能用代理的 PowerShell 中下载好后 `cp` 到 子系统，随便打了个 `wget` 发现真能用，但是下载完成没有发现下载的文件。

查找后发现需要加上 `-OutFile` 参数，参数后边跟文件名：`wget https://xxx/ -OutFile file.txt`

> 写入的文件必须提前建好，不然会报错
> 
> 创建文件用 `New-Item -ItemType file filename.txt`