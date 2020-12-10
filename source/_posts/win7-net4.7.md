---
layout: win7
title:  Win7 安装.NET 阻塞问题解决办法
date: 2020-12-10 17:14:18
tags:
- Win7
- .NET
cover: https://cdn.jsdelivr.net/gh/liangdongin/blog@latest/2020/12/10/win7-net4.7/net47.png
coverWidth: 517
coverHeight: 427
---

## 原因

系统缺少（D3dcompiler_47.dll）文件

## 解决办法

1.右击我的电脑，并点击‘属性’选项。

2.在Windows版本中，我们可以看到我们的系统是哪一版本的。

3.下载对应版本的补丁升级
&ensp;Windows 7 SP1 x86 补丁更新包 [下载地址](http://go.microsoft.com/fwlink/?LinkId=848159)
&ensp;Windows 7 SP1 x64 补丁更新包 [下载地址](http://go.microsoft.com/fwlink/?LinkId=848158)

4.重新安装 .NET4.7
&ensp;中文简体在线下载包 [下载地址](https://dotnet.microsoft.com/download/dotnet-framework/thank-you/net471-chs)
&ensp;离线安装包 [下载地址](https://dotnet.microsoft.com/download/dotnet-framework/thank-you/net471-offline-installer)