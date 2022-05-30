---
title: kali-Linux安装搜狗输入法
tags:
  - kali
categories:
  - 教程
  - Linux
abbrlink: 89dea71d
date: 2022-03-11 14:07:55
---



 

1. 	更改镜像源为国内的镜像源（如果已切换则忽略此步骤）

2. 打开搜狗输入法官网下载Linux版安装包

​	官网[搜狗输入法 ](https://pinyin.sogou.com/)

3. 打开终端，进入root模式，输入``` dpkg -i  *.deb```完成安装



注：

如果安装过程中提示缺少相关依赖，则执行如下命令解决：

```sudo apt -f install```
