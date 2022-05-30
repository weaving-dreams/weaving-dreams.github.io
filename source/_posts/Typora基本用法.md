---
title: Typora基本用法
tags:
  - Hexo
categories:
  - 教程
  - 博客
abbrlink: 1bd41834
date: 2022-03-09 09:48:53
---



[toc]



## Typore简介

Typora是一款轻便简洁的Markdown编辑器，支持即时渲染技术，这也是与其他Markdown编辑器最显著的区别。即时渲染使得你写Markdown就想是写Word文档一样流畅自如，不像其他编辑器的有编辑栏和显示栏。

## Markdown介绍

Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档。
Markdown 语言在 2004 由约翰·格鲁伯（英语：John Gruber）创建。
Markdown 编写的文档可以导出 HTML 、Word、图像、PDF、Epub 等多种格式的文档。
Markdown 编写的文档后缀为 .md, .markdown。

## 常用快捷键

加粗： Ctrl + B			字体倾斜：Ctrl+I			      下划线 ：Ctrl+U

插入链接：Ctrl + K 		   	创建表格：Ctrl+T

行内代码：Ctrl + Shift + K	  插入图片：Ctrl + Shift + I	   	     插入图片：Ctrl+Shift+M

选中某句话：Ctrl+L 		  选中某个单词： Ctrl+D	        搜索：Ctrl+F 

搜索并替换：Ctrl+H		选中相同格式的文字：Ctrl+E        删除线：Alt+Shift+5 

撤销： Ctrl + Z			返回Typora顶部：Ctrl+Home     返回Typora底部：Ctrl+End    

公式块：Ctrl+Shift+I  		     引用：Ctrl+Shift+Q          

标题： Ctrl + 1（一级标题）；Ctrl + 2（二级标题） -- 以此类推

## 引用文字

Markdown 使用电子邮件样式>字符进行块引用。它们表示为：

```
> 这是一个有两段的块引用。这是第一段。
>
> 这是第二段。Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.



> 这是另一个只有一个段落的块引用。有三个空行分隔两个块引用。
```

在typora中，只需输入’>’后跟引用内容即可生成块引用。Typora将为您插入正确的“>”或换行符。通过添加额外级别的“>”允许在块引用内嵌入另一个块引用。

## 元素列表

### 无序列表

使用 *   +   -  都可以创建一个无序列表

```
* AAA
+ BBB
- CCC
```

### 有序列表

使用 1. 2. 3. 创建有序列表

```
1. AAA
2. BBB
3. CCC
```

### 任务列表

任务列表是标记为[ ]或[x]（未完成或完成）的项目的列表。例如：

```
- [ ] 这是一个任务列表项
- [ ] 需要在前面使用列表的语法
- [ ] normal **formatting**, @mentions, #1234 refs
- [ ] 未完成
- [x] 完成
```

您可以通过单击项目前面的复选框来更改完成/未完成状态。

## 代码块

在Typora中插入程序代码的方式有两种：使用反引号 `（~ 键）、使用缩进（Tab）。

- 插入行内代码，即插入一个单词或者一句代码的情况，使用 `code` 这样的形式插入。

- 插入多行代码输入3个反引号（`） + 回车，并在后面选择一个语言名称即可实现语法高亮。

```python
  print("hell oword")
```

## 数学公式块

您可以使用 MathJax 渲染 LaTeX 数学表达式。

输入 $$, 然后按“return”键将触发一个接受Tex / LaTex源代码的输入区域。以下是一个例子： 

![](https://s3.bmp.ovh/imgs/2022/03/33ed0e61febfaa79.png)

在 markdown 源文件中，数学公式块是由’$$’标记包装的 LaTeX 表达式：

````
$$
\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix} 
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\
\end{vmatrix}
$$
````

LeTeX表达式参考连接：[超详细 LaTex数学公式](https://blog.csdn.net/ViatorSun/article/details/82826664#:~:text=%E7%9A%84LaTex%E8%A1%A8%E8%BE%BE%E5%BC%8F%E4%B8%BA%20%24x_i%5E2%24%20%E3%80%82.%20LaTex%E8%A1%A8%E8%BE%BE%E5%BC%8F%E4%B8%AD%E7%9A%84%E4%B8%8A%E4%B8%8B%E6%A0%87%E5%8F%AF%E4%BB%A5%E5%8F%A0%E5%8A%A0%E7%9A%84%EF%BC%8C%E5%B0%B1%E6%AF%94%E5%A6%82.%20x%20%20y%20,x_%20%7B2i%7D%5E%20%7B2%2Bb%7D.%20x2i2%2Bb.%20%E2%80%8B.%20%E7%9A%84LaTex%E8%A1%A8%E8%BE%BE%E5%BC%8F%E4%B8%BA%24x_%20%7B2i%7D%5E%20%7B2%2Bb%7D%24%E3%80%82.)   [LaTeX：公式常用字符和表达式](https://blog.csdn.net/weixin_39679367/article/details/84729452)

## 表格

输入 `| First Header | Second Header |` 并按下 `return` 键将创建一个包含两列的表。

创建表后，焦点在该表上将弹出一个表格工具栏，您可以在其中调整表格，对齐或删除表格。您还可以使用上下文菜单来复制和添加/删除列/行。

可以跳过以下描述，因为表格的 markdown 源代码是由typora自动生成的。

在 markdown 源代码中，它们看起来像这样：

```
| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |
```

您还可以在表格中包括内联 Markdown 语法，例如链接，粗体，斜体或删除线。

- 不管是哪种方式，第一行为表头，第二行为分割表头和主体部分，第三行开始每一行为一个表格行
- 列与列之间用管道符号`|` 隔开
- 还可设置对齐方式(表头与内容之间)，如果不使用对齐标记，内容默认左对齐，表头居中对齐
  - 左对齐 ：|
  - 右对齐 |：
  - 中对齐 ：|：
- 为了美观，可以使用空格对齐不同行的单元格，并在左右两侧都使用 | 来标记单元格边界
- 为了使 Markdown 更清晰，| 和 - 两侧需要至少有一个空格（最左侧和最右侧的 | 外就不需要了）

## 脚注

```
您可以像这样创建脚注[^footnote].

[^footnote]: Here is the *text* of the **footnote**.
```

将产生：

您可以像这样创建脚注[1](https://support.typoraio.cn/zh/Markdown-Reference/#fn:footnote).

鼠标移动到‘footnote’上标中查看脚注的内容。



## 水平线

输入 `***` 或 `---` 在空行上按 `return` 键将绘制一条水平线。



## 目录 (TOC)

输入 `[toc]` 然后按 `Return` 键将创建一个“目录”部分，自动从文档内容中提取所有标题，其内容会自动更新。

## 上下标

### 上标

可以使用`<sup>文本</sup>`实现上标。

(需在设置中打开该功能)

X<sup>2</sup>


### 下标

可以使用 `<sub>文本</sub>`实现下标。

(需在设置中打开该功能)

H<sub>2</sub>O

## 文本居中

使用 `<center>这是要居中的内容</center>`可以使文本居中

<center>这是要居中的文本内容</center>



## Span 元素

在您输入后Span元素会被立即解析并呈现。在这些span元素上移动光标会将这些元素扩展为markdown源代码。以下将解释这些span元素的语法。

### 链接

Markdown 支持两种类型的链接：内联和引用。

在这两种样式中，链接文本都写在[方括号]内。

要创建内联链接，请在链接文本的结束方括号后立即使用一组常规括号。在常规括号内，输入URL地址，以及可选的用引号括起来的链接标题。例如：

```
This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.
```

将产生：

This is [an example](http://example.com/"Title") inline link. (`<p>This is <a href="http://example.com/" title="Title">`)

[This link](http://example.net/) has no title attribute. (`<p><a href="http://example.net/">This link</a> has no`)

#### 内部链接

**您可以将常规括号内的 href 设置为文档内的某一个标题**，这将创建一个书签，允许您在单击后跳转到该部分。例如：

Command(在Windows上：Ctrl) + 单击 [此链接](https://support.typoraio.cn/zh/Markdown-Reference/#块元素) 将跳转到标题 `块元素`处。 要查看如何编写，请移动光标或按住 `⌘` 键单击以将元素展开为 Markdown 源代码。

#### 参考链接

参考样式链接使用第二组方括号，在其中放置您选择的标签以标识链接：

```
This is [an example][id] reference-style link.

然后，在文档中的任何位置，您可以单独定义链接标签，如下所示：

[id]: http://example.com/  "Optional Title Here"
```

在typora中，它们将呈现为：

This is [an example](http://example.com/) reference-style link.

隐式链接名称快捷方式允许您省略链接的名称，在这种情况下，链接文本本身将用作名称。只需使用一组空的方括号，例如，将“Google”一词链接到google.com网站，您只需写下：

```
[Google][]
然后定义链接：

[Google]: http://google.com/
```

在typora中单击链接将其展开以进行编辑，command + 单击将在 Web 浏览器中打开超链接。

### URL网址

Typora允许您将 URL 作为链接插入，用 `<`括号括起来`>`。

`<i@typora.io>` 成为 [i@typora.io](mailto:i@typora.io).

Typora也将自动链接标准URL。例如： www.google.com.

### 图片

图像与链接类似， 但在链接语法之前需要添加额外的 `!` 字符。 图像语法如下所示：

```
![替代文字](/path/to/img.jpg)

![替代文字](/path/to/img.jpg "可选标题")
```

您可以使用拖放操作从图像文件或浏览器来插入图像。并通过单击图像修改 markdown 源代码。如果图像在拖放时与当前编辑文档位于同一目录或子目录中，则将使用相对路径。

### 流程图

教程来自知乎：https://zhuanlan.zhihu.com/p/40810821

Typora可以直接在markdown中画流程图，而且语法简洁易懂

- sequence

语言选择sequence即可生成流程图

```
李雷 -> 韩梅梅: Hello 梅梅, How are you?
Note right of 韩梅梅: 韩梅梅心想
韩梅梅 --> 李雷: I'm fine, thanks, and you?
```

<svg width="460.78125" height="247.416" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" style="overflow: hidden; position: relative; left: -0.0336056px; top: -0.972681px; max-width: 100%; height: auto;" version="1.1">
 <desc style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);">Created with Raphaël 2.2.0</desc>
 <defs style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);">
  <path style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" id="raphael-marker-block" d="m5,0l-5,2.5l5,2.5l0,-5z" stroke-linecap="round"/>
  <marker style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" refY="2.5" refX="2.5" orient="auto" markerWidth="5" markerHeight="5" id="raphael-marker-endblock55-objehh4q">
   <use id="svg_1" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="currentColor" transform="rotate(180 2.5,2.5) scale(1) " xlink:href="#raphael-marker-block"/>
  </marker>
  <marker style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" refY="2.5" refX="2.5" orient="auto" markerWidth="5" markerHeight="5" id="raphael-marker-endblock55-objo2myt">
   <use id="svg_2" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="currentColor" transform="rotate(180 2.5,2.5) scale(1) " xlink:href="#raphael-marker-block"/>
  </marker>
 </defs>
 <g>
  <title>background</title>
  <rect fill="none" id="canvas_background" height="619" width="1292" y="-1" x="-1"/>
 </g>
 <g>
  <title>Layer 1</title>
  <rect id="svg_3" stroke-width="2" stroke="currentColor" fill="none" height="35.484375" width="52.28125" y="20" x="10"/>
  <rect id="svg_4" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="none" height="15.484375" width="32.28125" y="30" x="20"/>
  <text id="svg_5" fill="currentColor" font-size="16px" font-family="Andale Mono, monospace" text-anchor="middle" y="37.742188" x="36.140625">
   <tspan id="svg_6" dy="5.71875">李雷</tspan>
  </text>
  <rect id="svg_7" stroke-width="2" stroke="currentColor" fill="none" height="35.484375" width="52.28125" y="191.9375" x="10"/>
  <rect id="svg_8" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="none" height="15.484375" width="32.28125" y="201.9375" x="20"/>
  <text id="svg_9" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: middle; font-family: "Andale Mono", monospace; font-size: 16px;" fill="currentColor" font-size="16px" font-family="Andale Mono, monospace" text-anchor="middle" y="209.679688" x="36.140625">
   <tspan id="svg_10" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="5.71875">李雷</tspan>
  </text>
  <path id="svg_11" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" stroke-width="2" d="m36.14063,55.48438l0,136.45312" stroke="currentColor" fill="var(--bg-color)"/>
  <rect id="svg_12" stroke-width="2" stroke="currentColor" fill="none" height="35.484375" width="68.40625" y="20" x="231.6875"/>
  <rect id="svg_13" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="none" height="15.484375" width="48.40625" y="30" x="241.6875"/>
  <text id="svg_14" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: middle; font-family: "Andale Mono", monospace; font-size: 16px;" fill="currentColor" font-size="16px" font-family="Andale Mono, monospace" text-anchor="middle" y="37.742188" x="265.890625">
   <tspan id="svg_15" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="5.71875">韩梅梅</tspan>
  </text>
  <rect id="svg_16" stroke-width="2" stroke="currentColor" fill="none" height="35.484375" width="68.40625" y="191.9375" x="231.6875"/>
  <rect id="svg_17" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="none" height="15.484375" width="48.40625" y="201.9375" x="241.6875"/>
  <text id="svg_18" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: middle; font-family: "Andale Mono", monospace; font-size: 16px;" fill="currentColor" font-size="16px" font-family="Andale Mono, monospace" text-anchor="middle" y="209.679688" x="265.890625">
   <tspan id="svg_19" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="5.71875">韩梅梅</tspan>
  </text>
  <path id="svg_20" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" stroke-width="2" d="m265.89063,55.48438l0,136.45312" stroke="currentColor" fill="var(--bg-color)"/>
  <rect id="svg_21" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="none" height="15.484375" width="193.625" y="72.734375" x="54.203125"/>
  <text id="svg_22" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: middle; font-family: "Andale Mono", monospace; font-size: 16px;" fill="currentColor" font-size="16px" font-family="Andale Mono, monospace" text-anchor="middle" y="80.484375" x="151.015625">
   <tspan id="svg_23" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="5.710938">Hello 梅梅, How are you?</tspan>
  </text>
  <path id="svg_24" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" marker-end="url(#raphael-marker-endblock55-objehh4q)" stroke-width="2" d="m36.14063,90.96875c0,0 191.19018,0 224.75115,0" stroke="currentColor" fill="var(--bg-color)"/>
  <rect id="svg_25" stroke-width="2" stroke="currentColor" fill="none" height="25.484375" width="90.6875" y="110.96875" x="285.890625"/>
  <rect id="svg_26" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="none" height="15.484375" width="80.6875" y="115.96875" x="290.890625"/>
  <text id="svg_27" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: middle; font-family: "Andale Mono", monospace; font-size: 16px;" fill="currentColor" font-size="16px" font-family="Andale Mono, monospace" text-anchor="middle" y="123.710938" x="331.234375">
   <tspan id="svg_28" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="5.71875">韩梅梅心想</tspan>
  </text>
  <rect id="svg_29" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="none" height="15.484375" width="209.75" y="153.703125" x="46.140625"/>
  <text id="svg_30" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: middle; font-family: "Andale Mono", monospace; font-size: 16px;" fill="currentColor" font-size="16px" font-family="Andale Mono, monospace" text-anchor="middle" y="161.453125" x="151.015625">
   <tspan id="svg_31" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="5.710938">I'm fine, thanks, and you?</tspan>
  </text>
  <path id="svg_32" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" stroke-dasharray="6,2" marker-end="url(#raphael-marker-endblock55-objo2myt)" stroke-width="2" d="m265.89063,171.9375c0,0 -191.19019,0 -224.75116,0" stroke="currentColor" fill="var(--bg-color)"/>
 </g>
</svg>


- flowchart



对于Flowchart,语法选择flow

```
st=>start: 闹钟响起
op=>operation: 与床板分离
cond=>condition: 分离成功?
e=>end: 快乐的一天

st->op->cond
cond(yes)->e
cond(no)->op
```


<svg width="175.8203125" height="408.509765625" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRatio="xMidYMid meet" style="overflow: hidden; position: relative; left: -0.0378136px; top: -0.976878px;" version="1.1">
 <desc style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);">Created with Raphaël 2.2.0</desc>
 <defs style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);">
  <path style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" id="raphael-marker-block" d="m5,0l-5,2.5l5,2.5l0,-5z" stroke-linecap="round"/>
  <marker style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" refY="1.5" refX="1.5" orient="auto" markerWidth="3" markerHeight="3" id="raphael-marker-endblock33-objrngqv">
   <use id="svg_1" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="currentColor" stroke-width="1.6667" transform="rotate(180 1.5,1.5) scale(0.6000000238418579) " xlink:href="#raphael-marker-block"/>
  </marker>
  <marker style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" refY="1.5" refX="1.5" orient="auto" markerWidth="3" markerHeight="3" id="raphael-marker-endblock33-objfsd5j">
   <use id="svg_2" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="currentColor" stroke-width="1.6667" transform="rotate(180 1.5,1.5) scale(0.6000000238418579) " xlink:href="#raphael-marker-block"/>
  </marker>
  <marker style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" refY="1.5" refX="1.5" orient="auto" markerWidth="3" markerHeight="3" id="raphael-marker-endblock33-obj05jvn">
   <use id="svg_3" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="currentColor" stroke-width="1.6667" transform="rotate(180 1.5,1.5) scale(0.6000000238418579) " xlink:href="#raphael-marker-block"/>
  </marker>
  <marker style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" refY="1.5" refX="1.5" orient="auto" markerWidth="3" markerHeight="3" id="raphael-marker-endblock33-objyoquh">
   <use id="svg_4" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" fill="currentColor" stroke-width="1.6667" transform="rotate(180 1.5,1.5) scale(0.6000000238418579) " xlink:href="#raphael-marker-block"/>
  </marker>
 </defs>
 <g>
  <title>background</title>
  <rect fill="none" id="canvas_background" height="618" width="517" y="-1" x="-1"/>
 </g>
 <g>
  <title>Layer 1</title>
  <rect transform="matrix(1,0,0,1,38.668,23.377) " id="st" class="flowchart" stroke-width="3" stroke="currentColor" fill="none" ry="20" rx="20" height="36.15625" width="76.484375" y="0" x="0"/>
  <text transform="matrix(1,0,0,1,38.668,23.377) " class="flowchartt" id="stt" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: start; font-family: Arial; font-size: 14px;" fill="currentColor" font-size="14px" font-family=""Arial"" text-anchor="start" y="18.078125" x="10">
   <tspan id="svg_5" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="4.703125">闹钟响起</tspan>
  </text>
  <rect transform="matrix(1,0,0,1,31.6133,132.9102) " id="op" class="flowchart" stroke-width="3" stroke="currentColor" fill="none" height="36.15625" width="90.59375" y="0" x="0"/>
  <text transform="matrix(1,0,0,1,31.6133,132.9102) " class="flowchartt" id="opt" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: start; font-family: Arial; font-size: 14px;" fill="currentColor" font-size="14px" font-family=""Arial"" text-anchor="start" y="18.078125" x="10">
   <tspan id="svg_6" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="4.703125">与床板分离</tspan>
  </text>
  <path class="flowchart" id="cond" stroke-width="3" d="m41.45508,242.79394l-35.45508,17.72754l70.91016,35.45508l70.91015,-35.45508l-70.91015,-35.45508l-70.91016,35.45508" stroke="currentColor" fill="none"/>
  <text transform="matrix(1,0,0,1,6,225.0664) " class="flowchartt" id="condt" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: start; font-family: Arial; font-size: 14px;" fill="currentColor" font-size="14px" font-family=""Arial"" text-anchor="start" y="35.455078" x="40.455078">
   <tspan id="svg_7" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="4.705078">分离成功?</tspan>
  </text>
  <rect transform="matrix(1,0,0,1,31.6133,369.3535) " id="e" class="flowchart" stroke-width="3" stroke="currentColor" fill="none" ry="20" rx="20" height="36.15625" width="90.59375" y="0" x="0"/>
  <text transform="matrix(1,0,0,1,31.6133,369.3535) " class="flowchartt" id="et" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: start; font-family: Arial; font-size: 14px;" fill="currentColor" font-size="14px" font-family=""Arial"" text-anchor="start" y="18.078125" x="10">
   <tspan id="svg_8" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="4.703125">快乐的一天</tspan>
  </text>
  <path id="svg_9" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" marker-end="url(#raphael-marker-endblock33-objrngqv)" stroke-width="3" d="m76.91016,59.5332c0,0 0,52.90527 0,68.86985" stroke="currentColor" fill="none"/>
  <path id="svg_10" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" marker-end="url(#raphael-marker-endblock33-objfsd5j)" stroke-width="3" d="m76.91016,169.06641c0,0 0,38.20077 0,51.50016" stroke="currentColor" fill="none"/>
  <path id="svg_11" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" marker-end="url(#raphael-marker-endblock33-obj05jvn)" stroke-width="3" d="m76.91016,295.97656c0,0 0,52.90527 0,68.86985" stroke="currentColor" fill="none"/>
  <text id="svg_12" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: start; font-family: Arial; font-size: 14px;" fill="currentColor" font-size="14px" font-family=""Arial"" text-anchor="start" y="305.976563" x="81.910156">
   <tspan id="svg_13" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="4.703125">yes</tspan>
  </text>
  <path id="svg_14" marker-end="url(#raphael-marker-endblock33-objyoquh)" stroke-width="3" d="m147.82031,260.52148c0,0 25,-10 25,-10c0,0 0,-152.61132 0,-152.61132c0,0 -95.91015,0 -95.91015,0c0,0 0,21.04307 0,30.49601" stroke="currentColor" fill="none"/>
  <text id="svg_15" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-anchor: start; font-family: Arial; font-size: 14px;" fill="currentColor" font-size="14px" font-family=""Arial"" text-anchor="start" y="250.521484" x="152.820313">
   <tspan id="svg_16" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);" dy="4.708984">no</tspan>
  </text>
 </g>
</svg>




- gantt

除了Sequence和Flowchart两类图之外，Mermaid还提供一种叫做gantt的图。插入下面的代码，然后语法选mermaid就会自动渲染成gantt图了。

```
gantt
        dateFormat  YYYY-MM-DD
        title 快乐的生活
        section 吃一把鸡就学习
        学习            :done,    des1, 2014-01-06,2014-01-09
        疯狂学习               :active,  des2, 2014-01-09, 3d
        继续疯狂学习               :         des3, after des2, 5d
        吃鸡！               :         des4, after des3, 4d
        section 具体内容
        学习Python :crit, done, 2014-01-06,72h
        学习C++          :crit, done, after des1, 2d
        学习Lisp             :crit, active, 3d
        学习图形学        :crit, 4d
        跳伞           :2d
        打枪                      :2d

```

<svg id="mermaidChart0" width="100%" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 624 340" height="340" style="max-width: 624px;"><style>#mermaidChart0{font-family:sans-serif;font-size:16px;fill:#333;}#mermaidChart0 .error-icon{fill:#552222;}#mermaidChart0 .error-text{fill:#552222;stroke:#552222;}#mermaidChart0 .edge-thickness-normal{stroke-width:2px;}#mermaidChart0 .edge-thickness-thick{stroke-width:3.5px;}#mermaidChart0 .edge-pattern-solid{stroke-dasharray:0;}#mermaidChart0 .edge-pattern-dashed{stroke-dasharray:3;}#mermaidChart0 .edge-pattern-dotted{stroke-dasharray:2;}#mermaidChart0 .marker{fill:#333333;}#mermaidChart0 .marker.cross{stroke:#333333;}#mermaidChart0 svg{font-family:sans-serif;font-size:16px;}#mermaidChart0 .mermaid-main-font{font-family:"trebuchet ms",verdana,arial;font-family:var(--mermaid-font-family);}#mermaidChart0 .section{stroke:none;opacity:0.2;}#mermaidChart0 .section0{fill:rgba(102,102,255,0.49);}#mermaidChart0 .section2{fill:#fff400;}#mermaidChart0 .section1,#mermaidChart0 .section3{fill:white;opacity:0.2;}#mermaidChart0 .sectionTitle0{fill:#333;}#mermaidChart0 .sectionTitle1{fill:#333;}#mermaidChart0 .sectionTitle2{fill:#333;}#mermaidChart0 .sectionTitle3{fill:#333;}#mermaidChart0 .sectionTitle{text-anchor:start;font-size:11px;text-height:14px;font-family:'trebuchet ms',verdana,arial;font-family:var(--mermaid-font-family);}#mermaidChart0 .grid .tick{stroke:lightgrey;opacity:0.8;shape-rendering:crispEdges;}#mermaidChart0 .grid .tick text{font-family:sans-serif;fill:#333;}#mermaidChart0 .grid path{stroke-width:0;}#mermaidChart0 .today{fill:none;stroke:red;stroke-width:2px;}#mermaidChart0 .task{stroke-width:2;}#mermaidChart0 .taskText{text-anchor:middle;font-family:'trebuchet ms',verdana,arial;font-family:var(--mermaid-font-family);}#mermaidChart0 .taskText:not([font-size]){font-size:11px;}#mermaidChart0 .taskTextOutsideRight{fill:black;text-anchor:start;font-size:11px;font-family:'trebuchet ms',verdana,arial;font-family:var(--mermaid-font-family);}#mermaidChart0 .taskTextOutsideLeft{fill:black;text-anchor:end;font-size:11px;}#mermaidChart0 .task.clickable{cursor:pointer;}#mermaidChart0 .taskText.clickable{cursor:pointer;fill:#003163 !important;font-weight:bold;}#mermaidChart0 .taskTextOutsideLeft.clickable{cursor:pointer;fill:#003163 !important;font-weight:bold;}#mermaidChart0 .taskTextOutsideRight.clickable{cursor:pointer;fill:#003163 !important;font-weight:bold;}#mermaidChart0 .taskText0,#mermaidChart0 .taskText1,#mermaidChart0 .taskText2,#mermaidChart0 .taskText3{fill:white;}#mermaidChart0 .task0,#mermaidChart0 .task1,#mermaidChart0 .task2,#mermaidChart0 .task3{fill:#8a90dd;stroke:#534fbc;}#mermaidChart0 .taskTextOutside0,#mermaidChart0 .taskTextOutside2{fill:black;}#mermaidChart0 .taskTextOutside1,#mermaidChart0 .taskTextOutside3{fill:black;}#mermaidChart0 .active0,#mermaidChart0 .active1,#mermaidChart0 .active2,#mermaidChart0 .active3{fill:#bfc7ff;stroke:#534fbc;}#mermaidChart0 .activeText0,#mermaidChart0 .activeText1,#mermaidChart0 .activeText2,#mermaidChart0 .activeText3{fill:black !important;}#mermaidChart0 .done0,#mermaidChart0 .done1,#mermaidChart0 .done2,#mermaidChart0 .done3{stroke:grey;fill:lightgrey;stroke-width:2;}#mermaidChart0 .doneText0,#mermaidChart0 .doneText1,#mermaidChart0 .doneText2,#mermaidChart0 .doneText3{fill:black !important;}#mermaidChart0 .crit0,#mermaidChart0 .crit1,#mermaidChart0 .crit2,#mermaidChart0 .crit3{stroke:#ff8888;fill:red;stroke-width:2;}#mermaidChart0 .activeCrit0,#mermaidChart0 .activeCrit1,#mermaidChart0 .activeCrit2,#mermaidChart0 .activeCrit3{stroke:#ff8888;fill:#bfc7ff;stroke-width:2;}#mermaidChart0 .doneCrit0,#mermaidChart0 .doneCrit1,#mermaidChart0 .doneCrit2,#mermaidChart0 .doneCrit3{stroke:#ff8888;fill:lightgrey;stroke-width:2;cursor:pointer;shape-rendering:crispEdges;}#mermaidChart0 .milestone{-webkit-transform:rotate(45deg) scale(0.8,0.8);-ms-transform:rotate(45deg) scale(0.8,0.8);transform:rotate(45deg) scale(0.8,0.8);}#mermaidChart0 .milestoneText{font-style:italic;}#mermaidChart0 .doneCritText0,#mermaidChart0 .doneCritText1,#mermaidChart0 .doneCritText2,#mermaidChart0 .doneCritText3{fill:black !important;}#mermaidChart0 .activeCritText0,#mermaidChart0 .activeCritText1,#mermaidChart0 .activeCritText2,#mermaidChart0 .activeCritText3{fill:black !important;}#mermaidChart0 .titleText{text-anchor:middle;font-size:18px;fill:var(--text-color);font-family:'trebuchet ms',verdana,arial;font-family:var(--mermaid-font-family);}#mermaidChart0:root{--mermaid-font-family:sans-serif;}#mermaidChart0:root{--mermaid-alt-font-family:sans-serif;}#mermaidChart0 gantt{fill:apa;}</style><g></g><g class="grid" transform="translate(75, 290)" fill="none" font-size="10" font-family="sans-serif" text-anchor="middle"><path class="domain" stroke="currentColor" d="M0.5,-255V0.5H529.5V-255"></path><g class="tick" opacity="1" transform="translate(33.5,0)"><line stroke="currentColor" y2="-255"></line><text fill="#000" y="3" dy="1em" stroke="none" font-size="10" style="text-anchor: middle;">2014-01-07</text></g><g class="tick" opacity="1" transform="translate(99.5,0)"><line stroke="currentColor" y2="-255"></line><text fill="#000" y="3" dy="1em" stroke="none" font-size="10" style="text-anchor: middle;">2014-01-09</text></g><g class="tick" opacity="1" transform="translate(165.5,0)"><line stroke="currentColor" y2="-255"></line><text fill="#000" y="3" dy="1em" stroke="none" font-size="10" style="text-anchor: middle;">2014-01-11</text></g><g class="tick" opacity="1" transform="translate(231.5,0)"><line stroke="currentColor" y2="-255"></line><text fill="#000" y="3" dy="1em" stroke="none" font-size="10" style="text-anchor: middle;">2014-01-13</text></g><g class="tick" opacity="1" transform="translate(298.5,0)"><line stroke="currentColor" y2="-255"></line><text fill="#000" y="3" dy="1em" stroke="none" font-size="10" style="text-anchor: middle;">2014-01-15</text></g><g class="tick" opacity="1" transform="translate(364.5,0)"><line stroke="currentColor" y2="-255"></line><text fill="#000" y="3" dy="1em" stroke="none" font-size="10" style="text-anchor: middle;">2014-01-17</text></g><g class="tick" opacity="1" transform="translate(430.5,0)"><line stroke="currentColor" y2="-255"></line><text fill="#000" y="3" dy="1em" stroke="none" font-size="10" style="text-anchor: middle;">2014-01-19</text></g><g class="tick" opacity="1" transform="translate(496.5,0)"><line stroke="currentColor" y2="-255"></line><text fill="#000" y="3" dy="1em" stroke="none" font-size="10" style="text-anchor: middle;">2014-01-21</text></g></g><g><rect x="0" y="48" width="614" height="24" class="section section0"></rect><rect x="0" y="144" width="614" height="24" class="section section1"></rect><rect x="0" y="72" width="614" height="24" class="section section0"></rect><rect x="0" y="168" width="614" height="24" class="section section1"></rect><rect x="0" y="192" width="614" height="24" class="section section1"></rect><rect x="0" y="96" width="614" height="24" class="section section0"></rect><rect x="0" y="216" width="614" height="24" class="section section1"></rect><rect x="0" y="120" width="614" height="24" class="section section0"></rect><rect x="0" y="240" width="614" height="24" class="section section1"></rect><rect x="0" y="264" width="614" height="24" class="section section1"></rect></g><g><rect id="des1" rx="3" ry="3" x="75" y="50" width="99" height="20" transform-origin="124.5px 60px" class="task done0 "></rect><rect id="task1" rx="3" ry="3" x="75" y="146" width="99" height="20" transform-origin="124.5px 84px" class="task doneCrit1 "></rect><rect id="des2" rx="3" ry="3" x="174" y="74" width="99" height="20" transform-origin="223.5px 108px" class="task active0 "></rect><rect id="task2" rx="3" ry="3" x="174" y="170" width="66" height="20" transform-origin="207px 132px" class="task doneCrit1 "></rect><rect id="task3" rx="3" ry="3" x="240" y="194" width="100" height="20" transform-origin="290px 156px" class="task activeCrit1 "></rect><rect id="des3" rx="3" ry="3" x="273" y="98" width="166" height="20" transform-origin="356px 180px" class="task task0 "></rect><rect id="task4" rx="3" ry="3" x="340" y="218" width="132" height="20" transform-origin="406px 204px" class="task crit1 "></rect><rect id="des4" rx="3" ry="3" x="439" y="122" width="132" height="20" transform-origin="505px 228px" class="task task0 "></rect><rect id="task5" rx="3" ry="3" x="472" y="242" width="66" height="20" transform-origin="505px 252px" class="task task1 "></rect><rect id="task6" rx="3" ry="3" x="538" y="266" width="66" height="20" transform-origin="571px 276px" class="task task1 "></rect><text id="des1-text" font-size="11" x="124.5" y="63.5" text-height="20" class=" taskText taskText0  doneText0 width-21.515625">学习            </text><text id="task1-text" font-size="11" x="124.5" y="159.5" text-height="20" class=" taskText taskText1  doneCritText1 width-57.8125">学习Python </text><text id="des2-text" font-size="11" x="223.5" y="87.5" text-height="20" class=" taskText taskText0 activeText0 width-43.015625">疯狂学习               </text><text id="task2-text" font-size="11" x="207" y="183.5" text-height="20" class=" taskText taskText1  doneCritText1 width-45.03125">学习C++          </text><text id="task3-text" font-size="11" x="290" y="207.5" text-height="20" class=" taskText taskText1 activeCritText1 critText1 width-41">学习Lisp             </text><text id="des3-text" font-size="11" x="356" y="111.5" text-height="20" class=" taskText taskText0  width-64.53125">继续疯狂学习               </text><text id="task4-text" font-size="11" x="406" y="231.5" text-height="20" class=" taskText taskText1  critText1 width-53.78125">学习图形学        </text><text id="des4-text" font-size="11" x="505" y="135.5" text-height="20" class=" taskText taskText0  width-32.265625">吃鸡！               </text><text id="task5-text" font-size="11" x="505" y="255.5" text-height="20" class=" taskText taskText1  width-21.515625">跳伞           </text><text id="task6-text" font-size="11" x="571" y="279.5" text-height="20" class=" taskText taskText1  width-21.515625">打枪                      </text></g><g><text dy="0em" x="10" y="98" class="sectionTitle sectionTitle0"><tspan alignment-baseline="central" x="10">吃一把鸡就学习</tspan></text><text dy="0em" x="10" y="218" class="sectionTitle sectionTitle1"><tspan alignment-baseline="central" x="10">具体内容</tspan></text></g><g class="today"><line x1="100579" x2="100579" y1="25" y2="315" class="today"></line></g><text x="312" y="25" class="titleText">快乐的生活</text></svg>






## HTML

您可以使用HTML来设置纯 Markdown 不支持的内容，例如， `<span style="color:red">this text is red</span>` 用于添加红色文本。
