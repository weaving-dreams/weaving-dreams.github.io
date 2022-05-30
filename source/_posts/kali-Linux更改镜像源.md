---
title: kali-Linux更改镜像源
tags:
  - kali
categories:
  - 教程
  - Linux
abbrlink: 2753dfc5
date: 2022-03-11 14:07:39
---

1. 输入``` sudo su ```进入root权限

2. 输入```   vim /etc/apt/sources.list```命令进入源地址文件

![](https://s3.bmp.ovh/imgs/2022/03/57087644aef84736.png)

3. 按 i 进入编辑模式
4. 注释掉kali自带的镜像源；复制国内镜像源

```
#aliyun 阿里云

deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib

deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib

# ustc 中科大

deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib

deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib

# tsinghua 清华

deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free

deb-src http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free

#浙大源

deb http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free

deb-src http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
```

5. 按esc推出编辑模式，输入：wq 保存退出

![](https://s3.bmp.ovh/imgs/2022/03/3e7dbe62546c39a1.png)



6. 更新镜像源

```
apt-get clean       		#清除缓存索引
apt-get update				#更新索引文件
apt-get upgrade				#通过软件 安装/升级 软件更新系统
apt-get dist-upgrade		#根据依赖关系更新
```



7. reboot重启
