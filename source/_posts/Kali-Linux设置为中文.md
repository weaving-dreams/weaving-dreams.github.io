---
title: Kali-Linux设置为中文
tags:
  - kali
categories:
  - 教程
  - Linux
abbrlink: 5e07eda7
date: 2022-03-10 09:24:35
---



1. 打开终端输入sudo su进入管理员界面
2. 输入 dpkg-reconfigure locales  进入软件包设置

	![](https://s3.bmp.ovh/imgs/2022/03/1a0bcfc366efca72.png)



3. 用上下键将光标移至“zh_CN.UTF-8 UTF-8"项按空格键选择后回车

![](https://s3.bmp.ovh/imgs/2022/03/3b0c73b054b762f5.png)

4. 将光标移至“zh_CN.UTF-8 UTF-8”点击“ok”

![](https://s3.bmp.ovh/imgs/2022/03/8172a8edacb35ce2.png)



5. 等待系统设置；完成后输入 reboot 重启
