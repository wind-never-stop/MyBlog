---
title: Hexo Next主题首页配置为只显示部分摘要（不显示全文）
categories:  Hexo
tags:
- Hexo
---
### 前言
>使用Next主题时，首页会默认显示全文，这样就会很不方便，所以需要设置让首页只显示一部分摘要，而不是全文。

<!--more-->

### 1.配置主题
首先打开主题的配置文件`_config.yml`，该文件在博客文件夹的`themes\next\_config.yml`路径下，打开后找到`excerpt_description`这个配置并设置为`true`,如下代码：
```
# Automatically excerpt description in homepage as preamble text.
excerpt_description: true
```
然后还需要对对应对文章进行设置，有两种方法

### 方法一：写概述
在对应文章对 `front-matter`（文章文件最上方以 `---`分割对区域，是用来指定个别文件的配置变量区域）中添加`description`变量，其中`description`变量设置的内容就会被显示在首页上门，其余的文案一律不显示。配置如下：
```
---
title: Hexo添加分类及标签（在Next主题下）
categories:  Hexo
tags:
- Hexo
description: 自建的博客怎么能没有分类和标签呢，所以我就去查了一下怎么去配置分类和标签。
---
```
这一种方法是需要自己写概述，所以比较费事，于是就有了第二种方法。

### 方法二：文章截断显示
这种方法只需要在对应的文章里，想要展示的文章后添加以下标签就可以了
```
<!--more-->
```
然后首页就会显示在这个标签以上的所有内容，隐藏文章下面的所有内容。