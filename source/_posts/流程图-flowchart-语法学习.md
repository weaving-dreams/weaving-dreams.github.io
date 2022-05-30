---
title: 流程图(flowchart)语法学习
tags:
  - 其他
categories:
  - 教程
  - 其他
abbrlink: 4b7a36ad
date: 2022-05-18 11:05:42
---







## 语法介绍

### **定义所有元素**

**start 开始节点**

定义语句： ```s=>start:开始```

意为定义一个名字叫“s”的“start”元素，该元素显示的内容为“开始”

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/VScode/20220518110844.png)

**operation 操作节点**

定义语句： ```o1=>operation:操作一```

意为定义一个名字叫“o1”的“operation”元素，该元素显示的内容为“操作一”

![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/VScode/20220518110845.png)



**condition 条件节点**

定义语句：```c1=>condition:条件判断一```

意为定义一个名字叫“c1”的“condition”元素，该元素显示的内容为“条件判断一?”

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/VScode/20220518110847.png)

**inputoutput 输入或输出节点**
定义语句：```io=>inputoutput: 输入/输出一```
意为定义一个名字叫“io”的“inputoutput”元素，该元素显示的内容为“输入/输出一”

![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/VScode/20220518110849.png)



**end 结束节点**
定义语句： `e=>end: 流程结束`
意为定义一个名字叫“e”的“end”元素，该元素显示的内容为“流程结束”

![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/VScode/20220518110851.png)

<br>

**subroutine   //子程序**





### 连线

用`->`将两个元素链接

连接元素时，在标签后面可以加上`(right)\(left)\(bottom)\(top)`来指定连线方向



### 超连接：

在定义元素时，用`:>`给某个元素添加超连接。下句让start元素连接至百度首页

```
s=>start: 系统初始化:>https://www.baidu.com/
```



### 注意

`元素类型`和`展示文字`中间有一个空格，这个空格是必须有的，否则会出错。

每条连接线方向默认为(bottom)。

判断语句默认：yes向下，no向右（默认），yes向右，no向下

<br>

Hexo原生是不支持Markdown的流程图语法的。
执行以下命令以支持流程图。

```
npm install --save hexo-filter-flowchart
```



## 实例演示

效果图1

```
s=>start: 开始
i1=>inputoutput: 输入/输出一
o1=>operation: 操作一
o2=>operation: 操作二
c1=>condition: 条件判断一
en=>end: 流程结束

s->i1->o1->c1(yes)->o2->en
c1(no)->o1
```




```flow
s=>start: 开始
i1=>inputoutput: 输入/输出一
o1=>operation: 操作一
o2=>operation: 操作二
c1=>condition: 条件判断一
en=>end: 流程结束

s->i1->o1->c1(yes)->o2->en
c1(no)->o1
```



效果图2

```
startID=>start: 开始框
inputoutputID=>inputoutput: 输入输出框
operationID=>operation: 操作框
conditionID=>condition: 条件框
subroutineID=>subroutine: 子流程
endID=>end: 结束框

startID->inputoutputID->operationID->conditionID
conditionID(no)->subroutineID
conditionID(yes)->endID
```



```flow
startID=>start: 开始框
inputoutputID=>inputoutput: 输入输出框
operationID=>operation: 操作框
conditionID=>condition: 条件框
subroutineID=>subroutine: 子流程
endID=>end: 结束框

startID->inputoutputID->operationID->conditionID
conditionID(no)->subroutineID
conditionID(yes)->endID
```





效果图3

```
s=>start: 开始
o1=>operation: 打开密码文件
c1=>condition: 文件处理是否结束
en=>end: 流程结束
o2=>operation: 获取用户名密码
o3=>operation: 传给testpass
c2=>condition: 判断testpass返回值是否破解成功
o4=>operation: 输出明文密码

s->o1->c1(no,bottom)->o2->o3->c2(yes)->o4
c1(yes,left)->en
c2(no)->c1
o4->c1
```



![](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/VScode/20220518111809.png)
