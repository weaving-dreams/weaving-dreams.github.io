---
title: hexo博客上使用waline评论系统
tags:
  - Hexo
categories:
  - 教程
  - 博客
abbrlink: 5b3b1c7f
date: 2022-03-29 21:13:41
---



## 前言

Waline 是一款从 [Valine](https://valine.js.org/) 衍生的带后端评论系统。部署简单使用方面，干净整洁，可匿名评论。



## 步骤

### 准备工作

注册腾讯云并打开[腾讯云开发 CloudBase](https://cloud.tencent.com/login?s_url=https%3A%2F%2Fconsole.cloud.tencent.com%2Ftcb%2Fenv%2Findex%3Frid%3D4)

[![q6zEvR.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130854271.png)](https://imgtu.com/i/q6zEvR)



打开[Waline ](https://waline.js.org/)

[![q6zmb6.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130854796.png)](https://imgtu.com/i/q6zmb6)



### 创建环境

点击新建创建环境，选择空模板，点击下一步。

[![q6zlPe.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130854835.png)](https://imgtu.com/i/q6zlPe)

选择地域、计费方式，填写环境名称，同意计费规则，点击下一步

[![q6z32d.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130854820.png)](https://imgtu.com/i/q6z32d)



点击立即开通

[![q6zYrt.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130854777.png)](https://imgtu.com/i/q6zYrt)



### 设置云函数

打开[Waline | Waline](https://waline.js.org/) 点击快速上手

[![q6zdIS.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130855610.png)](https://imgtu.com/i/q6zdIS)





选择 服务器端——CloudBase 云开发部署

[![q6z6rq.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130855856.png)](https://imgtu.com/i/q6z6rq)





点击部署到云开发

[![qcS1oT.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130855331.png)](https://imgtu.com/i/qcS1oT)

点击下一步应用配置，等待部署完成，部署过程一般3至5分钟

[![qcSJW4.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130855693.png)](https://imgtu.com/i/qcSJW4)



### 评论区添加

点击访问服务可看到HTTP访问服务的默认域名

[![qcSwex.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130855409.png)](https://imgtu.com/i/qcSwex)



将你看到的默认域名填入前端脚本的 `serverURL` 配置中，即可完成全部配置。



例：在hexo中t添加评论区

```
waline:
  enable: true
  serverURL: 后端部署后的链接
  # visitor: true
  comment: false
```

- `serverURL`: 后端部署后的链接（需自行部署）
- `comment`: 是否显示本文评论数量
- `emoji`: 自定义表情

其他配置详情可参考：[快速上手 | Waline](https://waline.js.org/guide/get-started.html)



### 评论管理 (管理端)

1. 部署完成后，请访问 `<serverURL>/ui/register` 进行注册。首个注册的人会被设定成管理员。

2. 管理员登陆后，即可看到评论管理界面。在这里可以修改、标记或删除评论。

3. 用户也可通过评论框注册账号，登陆后会跳转到自己的档案页。

   

### 评论通知（server酱提醒）

进入云开发 CloudBase选择我的应用，点击管理

[![qcScSH.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130855335.png)](https://imgtu.com/i/qcScSH)

点击编辑，配置环境变量

[![qcSh0P.jpg](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130855550.jpeg)](https://imgtu.com/i/qcSh0P)

新建环境变量

```
SC_KEY: Server 酱提供的 Token，必填。
AUTHOR_EMAIL: 博主邮箱，用来区分发布的评论是否是博主本身发布的。如果是博主发布的则不进行提醒通知。
SITE_NAME: 网站名称，用于在消息中显示。
SITE_URL: 网站地址，用于在消息中显示。
```



附：其他形式请参考官方服务手册  [评论通知 | Waline](https://waline.js.org/guide/server/notification.html#邮件通知)



## FAQ

问题：配置好后评论提示 Failed to fecth 

[![qcSIk8.jpg](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130904176.jpeg)](https://imgtu.com/i/qcSIk8)

解决方法：配置安全域名

在安全配置里添加你博客的网址

[![qcSTfg.jpg](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130904081.jpeg)](https://imgtu.com/i/qcSTfg)
