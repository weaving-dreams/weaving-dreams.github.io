---
title: 卡里安装pycharm后创建快捷方式
tags:
  - kali
categories:
  - 教程
  - Linux
abbrlink: 1f1dc0da
date: 2022-03-09 19:31:26
---



1. kali的快捷方式都放在/usr/share/applications里，首先在该目录下创建一个Pycharm.desktop

2. 终端输输入：

   ```
   dusudo vim /usr/share/applications/Pycharm.desktop
   ```

3. 粘贴模板zhi：

```
[Desktop Entry]
Type=Application
Name=Pycharm
GenericName=Pycharm3
Comment=Pycharm3:The Python IDE
Exec=sh /opt/pycharm/bin/pycharm.sh
Icon=/opt/pycharm/bin/pycharm.png
Terminal=pycharm
Categories=Pycharm;
```

注：Exec 是执行文件位置；

​		Icon是快捷图标位置



附:

```
sudo apt-get autoremove                            删除系统不再使用的孤立软件
sudo apt-get autoclean                              清理旧版本的软件缓存
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P       清除残余的配置文件
```
