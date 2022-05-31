---
title: Deepin检查更新失败
tags:
  - deepin
categories:
  - 教程
  - Linux
abbrlink: 8b4f8dc5
date: 2022-03-28 18:55:15
---



我们更新deepin系统是总会出现检查更新失败,去终端输入命令发现是没有公钥无法验证下列签名

```
root@123-PC:~$ sudo apt-get update
获取:1 https://d.store.deepinos.org.cn  InRelease [1,524 B]
命中:3 https://community-packages.deepin.com/deepin apricot InRelease                                        
命中:4 https://community-packages.deepin.com/printer eagle InRelease                                
获取:5 https://packages.microsoft.com/repos/edge stable InRelease [7,343 B]                         
命中:2 https://home-store-img.uniontech.com/appstore deepin InRelease
错误:5 https://packages.microsoft.com/repos/edge stable InRelease
  由于没有公钥，无法验证下列签名： NO_PUBKEY EB3E94ADDE1229DD
正在读取软件包列表... 完成
W: GPG 错误：https://packages.microsoft.com/repos/edge stable InRelease: 由于没有公钥，无法验证下列签名： NO_PUBKEY EB3E94ADDE1229DD
E: 仓库 “https://packages.microsoft.com/repos/edge stable InRelease” 没有数字签名。
N: 无法安全地用该源进行更新，所以默认禁用该源。
N: 参见 apt-secure(8) 手册以了解仓库创建和用户配置方面的细节。

```

## 原因

第三方应用商店商店（星火应用商店）的更新源与系统更新源冲突导致。

### 解决办法

### 方法一：把星火应用商店的仓库禁用

> 注：此做法不影响星火应用商店安装应用

打开终端先执行

```cd /etc/apt/sources.list.d```

再执行

```sudo mv sparkstore.list sparkstore.list.bkup```



### 方法二：更新公钥

打开终端输入

```sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys c #此处的EB3E94ADDE1229DD是报错时提示的key```

完成后显示

```xiao@xiao-PC:~$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EB3E94ADBE1229CF
Executing: /tmp/apt-key-gpghome.CB8XSEEM1u/gpg.1.sh --keyserver keyserver.ubuntu.com --recv-keys EB3E94ADDE1229DD
gpg: 密钥 EB3E94ADDE1229DD：公钥 “Microsoft (Release signing) <gpgsecurity@microsoft.com>” 已导入
gpg: 处理的总数：1
gpg:               已导入：1
```

之后输入

```sudo apt-get update```

提示

```获取:1 https://d.store.deepinos.org.cn  InRelease [1,524 B]
命中:3 https://community-packages.deepin.com/deepin apricot InRelease                                        
获取:4 https://packages.microsoft.com/repos/edge stable InRelease [7,343 B]                                  
命中:5 https://community-packages.deepin.com/printer eagle InRelease                                         
命中:2 https://home-store-img.uniontech.com/appstore deepin InRelease
获取:6 https://packages.microsoft.com/repos/edge stable/main amd64 Packages [11.4 kB]
已下载 12.9 kB，耗时 1秒 (15.5 kB/s)
正在读取软件包列表... 完成
```



之后就可以去设置里更新系统了
