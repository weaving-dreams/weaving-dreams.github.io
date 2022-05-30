---
title: CentOS7加密grub
tags:
  - CentOS7
categories:
  - 教程
  - Linux
abbrlink: 71556fb
date: 2022-04-08 12:50:06
---





1. 下载grub2工具（如果已安装可忽略此步骤）

``` 
yum install -y grub2
```

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130933764.png)



2. 使用哈希密码，输入命令后输入当前密码，将当前密码生成哈希密码

```
grub2-mkpasswd-pbkdf2
```

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130933462.png)



3. 复制生成的哈希密码

   ![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130933933.png)

4. 在```/etc/grub.d/00_header``文件末尾，添加以下内容 (root 为单用户登录使用的用户名，第三行root后面为上一步加密后得到的密文，注意root和密文之间是空格隔开的不是换行符)


```vim /etc/grub.d/00_header```

```
cat <<EOF
set superusers='root'
password_pbkdf2 root +刚刚复制的哈希密码```
```

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130933725.png)

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130933527.png)



6. 输入```grub2-mkconfig -o /boot/grub2/grub.cfg```将grub的配置文件重新读取一遍

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130934270.png)

7. 重启验证

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130934502.png)

![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130934295.png)
