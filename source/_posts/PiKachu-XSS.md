---
title: PiKachu--XSS
tags:
  - pikachu
categories:
  - 学习
  - web安全
  - Pikachu
abbrlink: 96f58941
date: 2022-04-07 15:14:07
---

## XSS（跨站脚本）概述

Cross-Site Scripting 简称为“CSS”，为避免与前端叠成样式表的缩写"CSS"冲突，故又称XSS。一般XSS可以分为如下几种常见类型：
    1.反射性XSS;
    2.存储型XSS;
    3.DOM型XSS;

XSS漏洞一直被评估为web漏洞中危害较大的漏洞，在OWASP TOP10的排名中一直属于前三的江湖地位。
XSS是一种发生在前端浏览器端的漏洞，所以其危害的对象也是前端用户。
形成XSS漏洞的主要原因是程序对输入和输出没有做合适的处理，导致“精心构造”的字符输出在前端时被浏览器当作有效代码解析执行从而产生危害。
因此在XSS漏洞的防范上，一般会采用“对输入进行过滤”和“输出进行转义”的方式进行处理:
  输入过滤：对输入进行过滤，不允许可能导致XSS攻击的字符输入;
  输出转义：根据输出点的位置对输出到前端的内容进行适当转义;



## 一、反射型XSS

> 通过构造url地址，地址中暗藏xss平台地址，当有受害者点击此连接，会访问两个网址，一个是受害者原本想访问的网站，另一个是xss平台，受害者原本想访问的网站的cookie会被记录在XSS平台



### 反射型XSS (Get)

> **有关 GET 请求的其他一些注释：**（注释来自：[ 菜鸟教程 ](https://www.runoob.com/tags/html-httpmethods.html)）
>
> - GET 请求可被缓存
> - GET 请求保留在浏览器历史记录中
> - GET 请求可被收藏为书签
> - GET 请求不应在处理敏感数据时使用
> - GET 请求有长度限制
> - GET 请求只应当用于取回数据





1. 输入``` <script>alert('xss')</script>```发现输入长度有限制，

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130937338.png)

2. 解决方法，修改input标签里的maxlength值

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130938499.png)

3. 修改后可输入完整长度

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130938479.png)

4. 点击submit执行，成功

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130938660.png)



### 反射型XSS (Post)



>**有关 POST 请求的其他一些注释：**(注释来自：[ 菜鸟教程](https://www.runoob.com/tags/html-httpmethods.html))
>
>- POST 请求不会被缓存
>- POST 请求不会保留在浏览器历史记录中
>- POST 不能被收藏为书签
>- POST 请求对数据长度没有要求



登录

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130938235.png)

输入```<script>alert('xss')</script>```

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130939496.png)

点击submit执行，成功

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130939187.png)



<details>
  <summary>附：GET方法与POST方法的区别</summary>
    <p><li>在浏览器进行回退操作时，GET 请求是无害的，而 POST 请求则会重新请求一次</li></p>
    <p><li>GET 请求参数是连接在 URL 后面的,而POST请求参数是存放在消息主体（Requestbody）内</li></p>
    <p><li>GET 请求因为浏览器对 url 长度有限制（不同浏览器长度限制不一样）对传参数量有限制，而 post 请求因为参数存放 Requestbody 内所以参数数量没有限制</li> </p>
    <p><li>因为 GET 请求参数暴露在URL上,所以安全方面 POST 比 GET 更加安全</li></p>
    <p><li>GET 请求浏览器会主动缓存（Cache），POST 并不会，除非主动设置</li></p>
    <p><li>GET 请求参数会保存在浏览器历史记录内，POST 请求并不会</li></p>
    <p><li>GET 请求只能进行 URL 编码，而 POST 请求可以支持多种编码方式</li></p>
    <p><li>GET 请求产生1个 Tcp 数据包，POST 请求产生2个 Tcp 数据包</li></p>
    <p><li>浏览器在发送 GET 请求时会将请求头（Header）和数据（Data）一起发送给服务器，服务器返回200状态码，而在发送 POST 请求时，会先将 Header 发送给服务器，服务器返回100，之后再将 Data 发送给服务器，服务器返回200</li></p>
 <p>
注：GET方法与POST方法的区别来自：<a href="https://www.w3cschool.cn/article/31105654.html">W3School</a>
</p>
</details>








## 二、存储型XSS

> 一般会存入网站数据库，比如留言板，网站后台日志啥的，等受害者或管理员查看点击链接时，发生访问xss平台并被记录cookie的情形



输入```<script>alert('xss')</script>```

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130939293.png)

点击submit执行，成功

[![qzQhWt.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130939342.png)](https://imgtu.com/i/qzQhWt)

```留言板将输入的内容写入到网页中, 并且存储到网站数据库, 假如遭受到恶意代码攻击, 那么受到的攻击将是持久化的```

## 三、DOM型XSS

### DOM型XSS

> DOM型XSS的核心是运用DOM函数去执行访问xss平台的目的，在本靶场中，我们通过DOM函数改变HTML的结构，使浏览器加载页面的同时访问了xss平台，记录了使用者的cookie。



随便输入一个数字，点击click时出现链接，点击链接实现跳转，在网页源代码可以看到点击click生成了<a>标签。

[![qzQTOS.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130939974.png)](https://imgtu.com/i/qzQTOS)

方法一: 利用JavaScript伪协议

输入```javascript:alert("xss")```

[![qzQXYn.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130939967.png)](https://imgtu.com/i/qzQXYn)



[![qzQxS0.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130940199.png)](https://imgtu.com/i/qzQxS0)

方法二：代码闭合

输入```'onclick="alert('xss')">```   // a标签内部加属性

[![qzlSyT.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130940154.png)](https://imgtu.com/i/qzlSyT)
[![qzQzlV.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130940784.png)](https://imgtu.com/i/qzQzlV)

输入``` '> <img src="" onerror=alert('xss')> ```// 闭合出a标签, 将img标签嵌入上一级div执行

[![qzlZSx.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130940220.png)](https://imgtu.com/i/qzlZSx)
[![qzlel6.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130940638.png)](https://imgtu.com/i/qzlel6)

### DOM型XSS_x

输入```  ' onfocus=alert('xss')>```

[![qzlM0e.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130940838.png)](https://imgtu.com/i/qzlM0e)
[![qzlKmD.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130940549.png)](https://imgtu.com/i/qzlKmD)



## 四、XSS盲打

> XSS盲打简单来说，盲打就是在一切可能的地方尽可能多的提交xss语句，然后看哪一条会被执行，就能获取管理员的cooike，就是不知道后台不知道有没有xss存在的情况下，不顾一切的输入xss代码在留言啊，反馈啊之类的地方，尽可能多的尝试xss的语句与语句的存在方式，就叫盲打

注：XSS盲打不是一种XSS漏洞的类型，它其实是一种XSS一种的攻击场景。



输入```<script>alert('xss')</script>```

[![qzl1kd.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130940400.png)](https://imgtu.com/i/qzl1kd)

管理员进入后台访问并受到攻击

[![qzlJpt.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130940778.png)](https://imgtu.com/i/qzlJpt)

## 五、XSS之过滤

> 很多网站为了避免XSS的攻击，对用户的输入都采取了过滤，最常见的就是对<>转换成&lt;以及&gt;，经过转换以后<>虽然可在正确显示在页面上，但是已经不能构成代码语句了。这个貌似很彻底，因为一旦<>被转换掉，什么```<script src=1.js></script>```就会转换成```“&lt;script src=1.js&gt;&lt;/script&gt;”```，不能执行，因此，很多人认为只要用户的输入没有构成<>，就不能闭合前后的标签，其语句当然也不会有害。



输入```<ScRiPt>alert('xss')</ScRipt>``` // 大小写混合绕过

[![qzld0g.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130940472.png)](https://imgtu.com/i/qzld0g)
[![qzlanS.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130941394.png)](https://imgtu.com/i/qzlanS)

输入```<img src="" onerror=alert('xss')>  ```// img标签

[![qzl610.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130941569.png)](https://imgtu.com/i/qzl610)
[![qzlypq.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130941144.png)](https://imgtu.com/i/qzlypq)



## 六、XSS之htmlspecialchars

> htmlspecialchars — 将特殊字符转换为 HTML 实体

预定义的字符是：

> - & （和号） 成为 &amp;
>
> - " （双引号） 成为 &quot;
>
> - ' （单引号） 成为 &#039;
>
> - < （小于） 成为 &lt;
>
> - \> （大于） 成为 &gt;

可用引号类型

>  - ENT_COMPAT：默认，仅编码双引号
>  - ENT_QUOTES：编码双引号和单引号
>  - ENT_NOQUOTES：不编码任何引号




输入```hack' onfocus='alert("xss") ``` *// 单引号闭合+事件标签*

[![qzl4AJ.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130941405.png)
[![qzlf74.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130942641.png)](https://imgtu.com/i/qzlf74)

输入```javascript:alert("xss")  ```*// JavaScript伪协议*

[![qz3QJA.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130942343.png)](https://imgtu.com/i/qz3QJA)
[![qz3Mid.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130942399.png)](https://imgtu.com/i/qz3Mid)



## 七、XSS之href输出

>**herf输出做防御的两个逻辑**
>
>- 输入的时候只允许 http 或 https 开头的协议，才允许输出
>- 再进行htmlspecialchars处理，把特殊字符给处理掉



输入```javascript:alert(/xss/)```

[![qz14r8.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130942084.png)](https://imgtu.com/i/qz14r8)
[![qz1hKf.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130942567.png)](https://imgtu.com/i/qz1hKf)

## 八、XSS之js输出

>js输出漏洞：
>输出点是在javaScript，通过用户的输入动态的生成JavaScript代码
>javaScript里面是不会对tag和字符实体进行解释的,所以正常的输入得不到正常的输出，这不是我们需要的，所以，如果需要进行处理，就进行JavaScript转义处理，也就是用\ 对特殊字符进行处理。



输入```</script><script>alert('xss~')</script>```

[![qz1qGn.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130942647.png)](https://imgtu.com/i/qz1qGn)
[![qz1bPs.png](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130942338.png)](https://imgtu.com/i/qz1bPs)

这个漏洞的输出点是在JS中，通过用户的输入动态生成了JS代码

JS有个特点，它不会对实体编码进行解释，如果想要用htmlspecialchars对我们的输入做实体编码处理的话, 在JS中不会把它解释会去，虽然这样解决了XSS问题，但不能构成合法的JS

所以在JS的输出点应该对应该使用 \ 对特殊字符进行转义。
