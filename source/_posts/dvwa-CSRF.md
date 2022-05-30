---
title: dvwa--CSRF
tags:
  - DVWA
categories:
  - 学习
  - web安全
  - DVWA
abbrlink: 4161e8ef
date: 2022-04-14 16:44:50
---



## 简介

CSRF是一种攻击，它迫使最终用户在当前已通过身份验证的web应用程序上执行不想要的操作。借助一点社会工程的帮助(例如通过电子邮件/聊天发送链接)，攻击者可能会迫使web应用程序的用户执行攻击者所选择的操作。

一个成功的CSRF攻击可以在正常用户的情况下损害最终用户数据和操作。如果目标终端用户是管理员帐户，这可能会危及整个web应用程序。

这种攻击也可以称为“XSRF”，类似于“跨站点脚本编写(XSS)”，它们经常一起使用。



## CSRF与XSS区别

CSRF是借助用户的权限完成攻击，攻击者并没有拿到用户的权限。目标构造修改个人信息的链接，利用lucy在登录状态下点击此链接达到修改信息的目的。
XSS直接盗取了用户的权限，然后实施破坏。攻击者利用XSS盗取了目标的Cookie，登录lucy的后台，再修改相关信息。



## Low级

### 解析

```php
stripos( $_SERVER[ 'HTTP_REFERER' ] ,$_SERVER[ 'SERVER_NAME' ]) !== false )
```

stripos() ：数查找字符串在另一字符串中第一次出现的位置（不区分大小写）。

HTTP Referer：是header的一部分，当浏览器向web服务器发送请求的时候，一般会带上Referer，告诉服务器该网页是从哪个页面链接过来的，服务器因此可以获得一些信息用于处理。

SERVER_NAME：是要访问的主机名

过滤规则为http包头的Referer参数的值中是否包含主机名



### 过程



伪造链接

http://192.168.224.135/dvwa/vulnerabilities/csrf/?password_new=password&password_conf=password&Change=Change#



一旦用户点击链接，就会出现红字：提示密码改变了

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/CSRF--dvwa/20220414164025.png)

这样我们就成功地将密码更改为password了
当然，我们可以将链接修饰一下，毕竟这样一看就能看出来。
比如我们可以在网上找一个短链接生成器

![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/CSRF--dvwa/20220414164031.png)

得到这个链接：http://5l8.cn/ffHx0

伪装一下：[Change](http://5l8.cn/ffHx0)



也可以构造一个html界面

```html
<img src="http://192.168.224.135/dvwa/vulnerabilities/csrf/?password_new=password&password_conf=password&Change=Change#" border="0"style="display:none;"/>
<h1>404<h1>
<h2>file not found.<h2>
```



![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/CSRF--dvwa/20220414164034.png)_看似是一个失效界面，实际已经改掉了密码。_



## Medium级

### 解析

与low难度一样，没有token，这时候，我们可以看一下源代码比起low级多了

```php
if( stripos( $_SERVER[ 'HTTP_REFERER' ] ,$_SERVER[ 'SERVER_NAME' ]) !== false ) {
```



```stripos() ```函数查找字符串在另一字符串中第一次出现的位置（不区分大小写）。
```HTTP Referer```是header的一部分，当浏览器向web服务器发送请求的时候，一般会带上Referer，告诉服务器该网页是从哪个页面链接过来的，服务器因此可以获得一些信息用于处理。
```SERVER_NAME```是要访问的主机名
过滤规则为http包头的Referer参数的值中是否包含主机名





### 过程

抓包

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/CSRF--dvwa/20220414164039.png)

会发现有一栏Referer，就是该页面的url

```	Referer: http://192.168.224.135/dvwa/vulnerabilities/csrf/	```

开启抓包拦截、接着使用此前构造的链接尝试会发现报文里没有REferer


http://192.168.224.135/dvwa/vulnerabilities/csrf/?password_new=password&password_conf=password&Change=Change#


![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/CSRF--dvwa/20220414164042.png)

我们在报文里加上，然后放行

![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/CSRF--dvwa/20220414164044.png)

提示密码修改成功

![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/CSRF--dvwa/20220414164046.png)



## High级&Impossible级

略







注：教程来自[_[【DVWA全攻略】DVWA CSRF 实验-FancyPig's blog ](https://www.iculture.cc/cybersecurity/pig=243)_
