---
title: Hexo添加分类及标签（在Next主题下）
categories:  Hexo
tags:
- Hexo
---
[toc]
## 前言
>自建的博客怎么能没有分类和标签呢，所以我就去查了一下怎么去配置分类和标签。

## 1.配置分类

#### 1.1在主题里配置好分类
首先我们得先在主题里把分类这个选项打开，例如在<font color=blue > Next </font>主题下找到hexo 博客项目文件夹下 **\themes\next\_config.yml** 这个路径得配置文件,然后打开这个文件并找到下面得代码
```
menu:
  home: / || fa fa-home
  # about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  # schedule: /schedule/ || fa fa-calendar
  # sitemap: /sitemap.xml || fa fa-sitemap
  # commonweal: /404/ || fa fa-heartbeat

```
然后放开 **<font color=blue > categories: /categories/ || fa fa-th </font>** 这行代码就已经配置好里分类。

#### 1.2 创建分类目录文件
>因为分类页是没有默认页面的所以需要我们手动创建分类页。

打开命令行，进入博客项目所在的文件夹下，执行以下命令
```
$ hexo new page categories
```
成功后会提示：
```
INFO  Created: ~/blog/source/categories/index.md
```
这样我们就创建好了分类页面了。但是这个时候主题还不会识别这个页面为分类页；所以我们需要编辑这个新建的页面，让主题识别这个页面，并自动为这个页面显示分类。

#### 1.3 编辑页面让主题识别页面为分类页面
上文说到需要编辑页面才能让主题识别这个页面为分类页面，我们只需要根据成功后到提示路径打开`index.md`这个页面文件，打开后默认内容是
```
---
title: 文章分类
date: 2021-01-25 22:37:25
---
```
我们需要添加上`type: "categories"`这段代码就能让主题识别该页面为分类页面了
```
---
title: 文章分类
date: 2021-01-25 22:37:25
type: "categories"
---
```
我们就完成了整个分类页面的配置了
#### 1.4 给文章设置分类属性
首先打开需要添加分类的文章，在文章里添加上以下文案就设置好分类了 
```
---
categories: 
- Android
---
```
如上`categories:Android`表示添加这边文章到 “**Android**” 这个分类下。
然后我们就可以在博客到分类里看到该分类了。
```
//设置二级分类
---
categories: 
- Android
- xxx
---
```
如上设置二级分类则该篇文章为 Android 分类下的 XXX 分类下。


## 2.配置标签
#### 2.1 在主题里配置好标签
首先我们得先在主题里把分类这个选项打开，例如在<font color=blue > Next </font>主题下找到hexo 博客项目文件夹下 **\themes\next\_config.yml** 这个路径得配置文件,然后打开这个文件并找到下面得代码
```
menu:
  home: / || fa fa-home
  # about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  # schedule: /schedule/ || fa fa-calendar
  # sitemap: /sitemap.xml || fa fa-sitemap
  # commonweal: /404/ || fa fa-heartbeat

```
然后放开 **<font color=blue > tags: /tags/ || fa fa-tags</font>** 这行代码就已经配置好里分类。

#### 2.2 创建标签目录文件
>和分类页一样，标签页也是没有默认页面的所以需要我们手动创建标签页。

打开命令行，进入博客项目所在的文件夹下，执行以下命令
```
$ hexo new page tags
```
成功后会提示：
```
INFO  Created: ~/blog/source/tags/index.md
```
这样我们就创建好了标签页面了。但是这个时候主题还不会识别这个页面为标签页；所以我们需要编辑这个新建的页面，让主题识别这个页面，并自动为这个页面显示标签。

#### 2.3 编辑页面让主题识别页面为标签页面
上文说到需要编辑页面才能让主题识别这个页面为标签页面，我们只需要根据成功后到提示路径打开`index.md`这个页面文件，打开后默认内容是
```
---
title: 标签
date: 2021-01-25 22:54:58
---
```
我们需要添加上`type: "tags"`这段代码就能让主题识别该页面为标签页面了
```
---
title: 标签
date: 2021-01-25 22:54:58
type: "tags"
---
```
我们就完成了整个标签页面的配置了

#### 2.4 给文章设置标签属性
首先打开需要添加标签的文章，在文章里添加上以下文案，就设置好标签里了 
```
//设置单标签
---
tags:
- Facebook配置
---

//设置多标签 并同时设置分类
---
categories: 
- Android
tags:
- Android
- RecyclerView
---
```
如上`tags:- Facebook配置`表示给这篇文章添加 “**Facebook配置**” 这个分标签。
然后我们就可以在博客到标签里看到该标签了。

## 3.结语
这样我们就完成了分类和标签的配置，可以看出分类和标签的配置流程基本一样。

关于Next的更多配置请查看官方文档 https://theme-next.iissnan.com/theme-settings.html