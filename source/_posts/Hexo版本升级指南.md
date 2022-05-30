---
title: Hexo版本升级指南
tags:
  - Hexo
categories:
  - 教程
  - 博客
abbrlink: 3ba3be9
date: 2022-05-25 22:14:35
---



//以下指令均在Hexo目录下操作，先定位到Hexo目录
//查看当前版本，判断是否需要升级

```
hexo version
```

//全局升级hexo-cli

```
npm i hexo-cli -g
```

//再次查看版本，看hexo-cli是否升级成功

```
hexo version
```

//安装npm-check，若已安装可以跳过

```
npm install -g npm-check
```

//检查系统插件是否需要升级

```
npm-check
```

//安装npm-upgrade，若已安装可以跳过

```
npm install -g npm-upgrade
```

//更新package.json

```
npm-upgrade
```

//更新全局插件

```
npm update -g
```

//更新系统插件

```
npm update --save
```

//再次查看版本，判断是否升级成功

```
hexo version
```



<br>

文章转载自：[Hexo版本升级指南](https://novnan.github.io/Hexo/update_hexo/)
