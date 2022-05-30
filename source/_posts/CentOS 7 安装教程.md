---
title: CentOS 7 安装教程
tags:
  - CentOS7
categories:
  - 教程
  - Linux
abbrlink: c734c1de
date: 2022-05-02 20:55:15
---





>  已省略部分步骤

<br>

## 步骤

### 先择语言

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Centos7%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/20220502104535.png)

### 调整时间，时区

![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Centos7%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/20220502104538.png)

### 选择预安装的软件

![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Centos7%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/20220502104540.png)

### 配置分区

如果嫌手动分区麻烦可选择自动分区，但这里我们手动配置。
选择我要配置分区，点击完成进入手动配置界面。
选择标准分区，点击 + 号添加分区，我们准备分3个区
/boot（1G）：系统启动引导配置文件存放的区域，不需要太大
swap（4G）：交换分区，在系统的物理内存不够用的时候，把硬盘中的一部分空间释放出来，以供当前运行的程序使用，一般为物理内存大小的1.5-2倍
/（95G）：根分区，将剩余全部大小作为根分区

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Centos7%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/20220502104542.png)

点击完成，接收更改

![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Centos7%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/20220502104544.png)

### 启用网络

![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Centos7%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/20220502104552.png)

### 创建普通用户和密码

![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Centos7%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/20220502104555.png)

### Cetos7安装完成

![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Centos7%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/20220502104558.png)



## FAQ

Centos7初次开机提示Initial setup of CentOS Linux 7

开机后提示以下信息

```
Initial setup of CentOS Linux 7 (core) 
\1) [x] Creat user 2) [!] License information 
(no user will be created) (license not accepted) 
Please make your choice from above [‘q’ to quit | ‘c’ to continue | ‘r’ to refresh]:
```

解决步骤如下：

1，输入【1】，按Enter键同意许可协议， 
2，输入【2】，按Enter键阅读许可协议， 
3，输入【q】，按Enter键退出， 
4，输入【yes】，按Enter键确定， 
5，重启之后即可进入图形登录界面
