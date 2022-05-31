---
title: Pikachu--暴力破解
tags:
  - pikachu
categories:
  - 学习
  - web安全
  - Pikachu
abbrlink: 95e7ce5d
date: 2022-04-05 19:40:20
---

## Burte Force（暴力破解）概述

“暴力破解”是一攻击具手段，在web攻击中，一般会使用这种手段对应用系统的认证信息进行获取。 其过程就是使用大量的认证信息在认证接口进行尝试登录，直到得到正确的结果。 为了提高效率，暴力破解一般会使用带有字典的工具来进行自动化操作。
理论上来说，大多数系统都是可以被暴力破解的，只要攻击者有足够强大的计算能力和时间，所以断定一个系统是否存在暴力破解漏洞，其条件也不是绝对的。 我们说一个web应用系统存在暴力破解漏洞，一般是指该web应用系统没有采用或者采用了比较弱的认证安全策略，导致其被暴力破解的“可能性”变的比较高。 这里的认证安全策略, 包括：
1.是否要求用户设置复杂的密码；
2.是否每次认证都使用安全的验证码（想想你买火车票时输的验证码～）或者手机otp；
3.是否对尝试登录的行为进行判断和限制（如：连续5次错误登录，进行账号锁定或IP地址锁定等）；
4.是否采用了双因素认证；
...等等。



## 基于表单的暴力破解

1. 设置代理（这里以谷歌浏览器为例）

* 点击右上角的三个点，选择设置、高级、系统、打开您计算机的代理设置

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130956010.png)



![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130956440.png)





![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130956733.png)



* 在Internet选项里点击 连接、局域网设置、

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130956489.png)

* 设置为手动代理，填上127.0.0.1(这是本地IP)，端口8080(对应burpsuite的端口),点击确认

  ![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130956513.png)

2. 打开burpsuite开启拦截，让后在页面填入账号：admin ，密码：随便，然后点击login

	![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130956322.png)

3. burpsuite已抓到post包，右键选择发送给intruder

   ![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130957512.png)

4. 然后在测试区中点击位置，在右边点击clear清除，然后选中password中的密码（add选中），即是我们要pj的账号密码)，攻击类型选择最后一个

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130957762.png)

[![qO7CKP.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130957888.png)](https://imgtu.com/i/qO7CKP)

5. 添加密码字典

[![qO7E5Q.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130957508.png)](https://imgtu.com/i/qO7E5Q)

6. 开始破解

   [![qO7ZCj.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130957226.png)](https://imgtu.com/i/qO7ZCj)

7. 完成

	[![qO7QbT.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130957598.png)](https://imgtu.com/i/qO7QbT)



## 验证码绕过（on server）

1. 和之前一样，设置代理，burpsuite拦截请求

   [![qO73aF.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130957953.png)](https://imgtu.com/i/qO73aF)

2. 发给Repeater(重发器)

	[![qO7854.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130957264.png)](https://imgtu.com/i/qO7854)

3. 来到重发器，点击发送

   [![qO7YG9.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130958165.png)](https://imgtu.com/i/qO7YG9)

4. 把响应这边的拖动条拖到最后，可以看到用户名或密码错误，再发送一次同意是账号密码错误，接着我们把验证码修改了发送就可以看到验证码错误了（这这里有点问题显示□）

   [![qO7Nx1.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130958346.png)](https://imgtu.com/i/qO7Nx1)

   ```这说明是验证码无条件不刷新，无条件不刷新是指在某一时间段内，无论登录失败多少次，只要不刷新页面，就可以无限次的使用同一个验证码来对一个或多个用户帐号进行暴力猜解.```

5. 接着我们回到代理这里，然后把内容发送测试器，像之前一样，在测试器里面把全部清楚了，再把密码处添加上，攻击类型第四个，把账号密码的文本加载进去，最后攻击。

[![qO7wqK.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130958298.png)](https://imgtu.com/i/qO7wqK)

[![qO7yPH.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130958682.png)](https://imgtu.com/i/qO7yPH)

[![qO7cRA.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130958911.png)](https://imgtu.com/i/qO7cRA)



6. 长度唯一确定密码

[![qO7gxI.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130958645.png)](https://imgtu.com/i/qO7gxI)

## 验证码绕过（on dilent）

基本过程同上（抓包>pj>登录）





## token防爆破

```token由返回包确定，显然，每次我们登陆，token的值肯定会变化，token值错误无法验证账号密码的正确性，需要我们每一次发送请求包都要拿到上一个返回包的token值放在token包里面```

1. 输入账号密码，开始抓包

[![qO7fqf.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130958539.png)](https://imgtu.com/i/qO7fqf)

[![qO74Z8.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130958791.png)](https://imgtu.com/i/qO74Z8)



2. 像之前一样，在测试器里面把全部清楚了，再把密码和token处添加上

   [![qO77Gj.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130959725.png)](https://imgtu.com/i/qO77Gj)

3. 线程数设为1（只有获取上一个请求返回的taken值才能，做下一次请求，无法并发)）

   [![qO7LMq.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130959447.png)](https://imgtu.com/i/qO7LMq)

4. 在选项下面的Grep Extract点添加，获得回复，选中token的值复制然后确认

[![qO7XLV.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130959025.png)](https://imgtu.com/i/qO7XLV)

		最下面选中总是

[![qO7zoF.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130959755.png)](https://imgtu.com/i/qO7zoF)

5. 在有效载荷中选择递归搜索，然后把值放在第一个请求的初始有效负载

[![qOHwSs.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130959715.png)](https://imgtu.com/i/qOHwSs)

6. 加载字典后攻击

[![qOH2Y4.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130959202.png)](https://imgtu.com/i/qOH2Y4)

[![qOHRfJ.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130959902.png)](https://imgtu.com/i/qOHRfJ)

7. 唯一的长度,成功

[![qOHfp9.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130959475.png)](https://imgtu.com/i/qOHfp9)
