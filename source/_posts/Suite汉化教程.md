---
layout: burp
title: Suite汉化教程
date: 2022-05-28 10:53:00
tags:
  - 汉化
categories:
  - 教程
  - 工具汉化
---

# 



## 准备工作

下载所需安装包

Burp Suite官网：https://portswigger.net/burp

java环境（推荐下载11.0.1）：https://repo.huaweicloud.com/java/jdk/

汉化补丁（百度网盘含Java环境）：https://pan.baidu.com/s/1m_7QAVbN9QCvQV9xMh3avg?pwd=2yfo 	

提取码：2yfo

![0.5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105115.png)



## 步骤

1. .双击下载的文件，选择安装路径（例如我的安装位置是：D:\jdk）

![1.1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105117.png)

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105119.png)



2. 配置环境变量（设置--系统--关于--高级系统设置--环境变量）

在系统环境变量中添加新的环境变量	`JAVA_HOME`		`D:\jdk`

![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105120.png)

在系统环境变量中`Path`中添加:`%JAVA_HOME%\bin`

![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105122.png)



3. 验证是否安装成功

   ```
   jave -version
   ```

   ![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105123.png)

   

4. burp安装

双击下载的exe可执行文件，选择安装路径。（D:\BurpSuiteCommunity）

![5.5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105125.png)



![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105126.png)

5. 汉化

把下载的汉化工具移动到burp安装的路径下。

![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105128.png)



双击汉化工具，勾选`cn`后点击`run`即可

![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105129.png)



成功（虽然不是所有汉化，也够用了）

![8](Burp%20Suite%E6%B1%89%E5%8C%96%E6%95%99%E7%A8%8B/8.png)

6. 添加快捷方式（每次进目录点汉化工具太麻烦了）

右键汉化工具-发送到-桌面桌面快捷方式

![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105131.png)



发送到桌面的快捷方式看起来很不合适，可以对该快捷方式右键属性更改图标

![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105132.png)



![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105134.png)

搞定

![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/burp/20220528105136.png)
