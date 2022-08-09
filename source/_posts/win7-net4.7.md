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
<font color=red size=5>非常重要（请一定仔细阅读，按照步骤一步一步来）</font>

1.右击我的电脑，并点击‘属性’选项。

2.在Windows版本中，我们可以看到我们的系统是哪一版本的。

3.打开系统更新下面链接是教程
&ensp;[教程](http://www.ldlv.cn/2022/08/09/open_windows_update/)

4.安装对应版本的两个补丁更新包（安装后需要重启电脑） 如果碰到证书问题请点继续访问
x64
&ensp;Windows 7 SP1 x64 补丁更新包 [下载地址](http://go.microsoft.com/fwlink/?LinkId=848158)
&ensp;Windows 7 SP1 x64 补丁更新包 2 [下载地址](http://download.windowsupdate.com/c/msdownload/update/software/secu/2019/09/windows6.1-kb4474419-v3-x64_b5614c6cea5cb4e198717789633dca16308ef79c.msu)
x86
&ensp;Windows 7 SP1 x86 补丁更新包 [下载地址](http://go.microsoft.com/fwlink/?LinkId=848159)
&ensp;Windows 7 SP1 x86 补丁更新包 2 [下载地址](http://download.windowsupdate.com/c/msdownload/update/software/secu/2019/09/windows6.1-kb4474419-v3-x86_0f687d50402790f340087c576886501b3223bec6.msu)

5.下载安装 .NET4.8 框架
&ensp;离线安装包 [下载地址](https://dotnet.microsoft.com/download/dotnet-framework/thank-you/net48-offline-installer)