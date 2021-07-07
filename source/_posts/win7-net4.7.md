---
layout: win7
title:  Win7 安装.NET 4.8解决办法
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

<!--more-->
## 解决办法

1.右击我的电脑，并点击‘属性’选项。

2.在Windows版本中，我们可以看到我们的系统是哪一版本的。

3.下载对应版本的两个补丁更新包
&ensp;Windows 7 SP1 x86 补丁更新包 [下载地址](http://go.microsoft.com/fwlink/?LinkId=848159)
&ensp;Windows 7 SP1 x64 补丁更新包 [下载地址](http://go.microsoft.com/fwlink/?LinkId=848158)
&ensp;Windows 7 SP1 x86 补丁更新包 2 [下载地址](http://download.windowsupdate.com/c/msdownload/update/software/secu/2019/09/windows6.1-kb4474419-v3-x86_0f687d50402790f340087c576886501b3223bec6.msu)
&ensp;Windows 7 SP1 x64 补丁更新包 2 [下载地址](http://download.windowsupdate.com/c/msdownload/update/software/secu/2019/09/windows6.1-kb4474419-v3-x64_b5614c6cea5cb4e198717789633dca16308ef79c.msu)

4.重启计算机

5.安装 .NET4.8
&ensp;中文简体在线下载包 [下载地址](https://dotnet.microsoft.com/download/dotnet-framework/thank-you/net48-developer-pack-chs)
&ensp;离线安装包 [下载地址](https://dotnet.microsoft.com/download/dotnet-framework/thank-you/net48-offline-installer)