# 浅谈MarkDown—世界上最受欢迎的标记语言之一

## 1.  MarkDown简介

![image-20220803220539869](%E6%B5%85%E8%B0%88MarkDown%E8%AF%AD%E6%B3%95.assets/image-20220803220539869.png)

​		Markdown 是一种轻量级标记语言，创始人为约翰·格鲁伯（John Gruber）。 它允许人们使用易读易写的纯文本格式编写文档，然后转换成有效的 XHTML（或者HTML）文档。这种语言吸收了很多在电子邮件中已有的纯文本标记的特性。

​		John Gruber在2004年创造了Markdown语言，目的是希望使用易于阅读、易于撰写的纯文字格式，并选择性的转换成有效的XHTML（或是HTML）。 		

​		MarkDown文件的后缀名为 .md或 .markdown，由于 Markdown 的轻量化、易读易写特性，并且对于图片，图表、数学式都有支持，许多网站都广泛使用 Markdown 来撰写帮助文档或是用于论坛上发表消息。 如 GitHub、Reddit、Diaspora、Stack Exchange、OpenStreetMap 、SourceForge、简书等，甚至还能被使用来撰写电子书。

## 2.MarkDown的发展历程

### 标准化

​		随着时间的推移，出现了许多Markdown实现。与此同时，非正式规范中的一些模糊的错误引起了人们的注意。这些问题促使Markdown解析器的一些开发人员努力实现标准化。

​		2016年3月发布了RFC 7763和RFC 7764。RFC 7763 从原始变体引入了MIME类型 text/markdown。RFC 7764讨论并注册了MultiMarkdown、GitHub Flavored Markdown (GFM)、Pandoc、CommonMark及Markdown等变体。

### CommandMark

​		2014年9月，Gruber反对在这一工作中继续使用“Markdown”这个名字，其被更名为CommonMark。CommonMark发布了规范、参考实现和测试包的几个版本，并计划在2018年宣布最终的1.0规范和测试包。

### GFM

​		2017年，GitHub发布了基于CommonMark的GitHub Flavored Markdown（GFM）的正式规范。除了表格、删除线、自动链接和任务列表被GitHub规范作为扩展添加之外，GitHub还相应地更改了其站点上使用的解析器，这要求更改某些文档 - 例如，GFM要求创建标题的哈希符号由空格字符分隔。

### MarkDown Extra

​		Markdown Extra是一种轻量级标记语言，基于在PHP（最初）、Python和Ruby中实现的Markdown。它添加了普通Markdown语法不具备的功能。

它为Markdown添加了以下功能：

- HTML块内的markdown标记
- 具有id / class属性的元素
- 围栏代码块
- 表格
- 定义清单
- 脚注
- 缩写

## 3.优点

- 世界上最流行的博客平台WordPress和大型CMS如Joomla、Drupal都能很好的支持Markdown。完全采用Markdown编辑器的博客平台有Ghost和Typecho等。
- 用于编写说明文档，以“README.md”的文件名保存在软件的目录下面。
- Markdown可以快速转化为演讲PPT、Word产品文档甚至是用非常少量的代码完成最小可用原型。

## 4.MarkDown语法入门

Markdown是一种简单的格式化文本的方法，使用起来容易上手，如果是在支持markdown语法的编辑器里书写文档还可以用快捷键提高效率，这里只截取一部分语法：

![image-20220803221855725](%E6%B5%85%E8%B0%88MarkDown%E8%AF%AD%E6%B3%95.assets/image-20220803221855725.png)

![image-20220803221913211](%E6%B5%85%E8%B0%88MarkDown%E8%AF%AD%E6%B3%95.assets/image-20220803221913211.png)

想了解更多请访问MarkDown官网：https://markdown.com.cn/

除此之外，MarkDown也可以用来编辑LaTex公式，可以跨平台、跨环境显式：

![image-20220803222112629](%E6%B5%85%E8%B0%88MarkDown%E8%AF%AD%E6%B3%95.assets/image-20220803222112629.png)

## 5.编辑器

支持MarkDown语法的编辑器和笔记软件很多：Typora、为知笔记、印象笔记、语雀、有道云笔记等

其中较推荐的编辑器为Typora

优点如下：

- 实时预览格式

    Typora 是一款适配 Windows / macOS / Linux 平台的 Markdown 编辑器，编辑实时预览标记格式，所见即所得，轻巧而强大。

    ![image-20220803223228024](%E6%B5%85%E8%B0%88MarkDown%E8%AF%AD%E6%B3%95.assets/image-20220803223228024.png)

- 侧边栏大纲目录

    通过隐藏式的侧边栏可展示大纲目录，快速跳转章节。而文件树列表功能可切换到其它文章编写，非常方便。

    ![image-20220803223447632](%E6%B5%85%E8%B0%88MarkDown%E8%AF%AD%E6%B3%95.assets/image-20220803223447632.png)

- 支持多种代码语言的高亮显示

    ![image-20220803223722549](%E6%B5%85%E8%B0%88MarkDown%E8%AF%AD%E6%B3%95.assets/image-20220803223722549.png)

    除了上面那些，Typora中还有很多强大的功能等着你去发现。

    

    ​		刚开始接触MarkDown语法时，可能会觉得有些奇怪，甚至觉得”好难用“，但只要你用过一段时间，你会不得不感叹其在日常中的方便、简洁、强大。希望读完这篇文章后能激起你对MarkDown的兴趣，进而去了解它、学会它、掌握它，从而对生活和学习有所帮助。

    

​		

​		