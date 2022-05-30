---
title: 如何彻底关闭Hyper-V
tags:
  - windows
categories:
  - 教程
  - Windows
abbrlink: 8dc48be7
date: 2022-03-16 10:02:42
---

许多第三方虚拟化应用程序无法与 Hyper-V同时工作。 受影响的应用程序包括 VMware Workstation 和 VirtualBox。 这些应用程序可能无法启动虚拟机，或者可能回退到较慢的模拟模式。

这些症状是在虚拟机监控Hyper-V运行时引入的。 某些安全解决方案还依赖于虚拟机监控程序，例如：

> - Device Guard
> - Credential Guard

鱼和熊掌不可兼得，所以有时候我们需要关闭他们

<!-- more -->

## 网络搜到常用方式

### 1. 在启动或关闭Windows功能中关闭

1） 进入控制面板，选择程序和功能

<img src="https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204131001053.png" alt="图一" style="zoom: 80%;" />

<center><p>图一</p></center>

2）打开启动或关闭windows功能

<img src="https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204131004260.png" alt="图二" style="zoom: 80%;" />

<center><p>图二</p></center>

3）hyper-v、Windows虚拟机监控平台、虚拟机平台取消勾选

<img src="https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204131004929.png" style="zoom: 80%;" />



<center><p>图三</p></center>

4）重新启动



### 2.禁用服务

1）右键此电脑，管理

<img src="https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204131003763.png" style="zoom: 80%;" />

<center><p>图四</p></center>

2）服务和应用程序--服务

<img src="https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204131003939.png" style="zoom: 80%;" />

<center><p>图五</p></center>

3）找到Hyper-V虚拟机管理，双击，启动类型改为手动，确定

<img src="https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204131002695.png" style="zoom: 80%;" />

<center><p>图六</p></center>

4）重启

### 3.cmd命令

以管理员身份运行命令提示符，执行命令  ```bcdedit /set hypervisorlaunchtype off```

<img src="https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204131002461.png"  />

<center><p>图七</p></center>



### 4. 依然是cmd命令

以管理员身份打开cmd，执行命令：

```bcdedit /copy {current} /d “Windows10 no Hyper-V```

会返回一个串字符串{xxx-xxx-xxx}，电脑不一样，返回发东西不一样，
然后再次执行命令：

```bcdedit /set {XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX} hypervisorlaunchtype OFF```

将刚才返回的字符串替换到上面的命令里执行。返回成功后，重启电脑即可。





> **结果发现统统不管用，最后终于在微软官方找到方法**



## 微软官方解决方法

网址：[管理Windows Defender Credential Guard （Windows） - Windows security | Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/security/identity-protection/credential-guard/credential-guard-manage)



方法：

1）管理员模式打开 CMD, 运行下面的命令

```

mountvol X: /s

copy %WINDIR%\System32\SecConfig.efi X:\EFI\Microsoft\Boot\SecConfig.efi /Y

bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader

bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"

bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}

bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO

bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=X:

mountvol X: /d
```

注：需逐行复制



2）重启电脑

3）接受禁用 Windows Defender Credential Guard 的提示。(根据提示按下WIN键或者F3)
