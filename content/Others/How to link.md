---
title: How to link
date: 2024-9-15 18:26
updated: 2024-9-15 18:37
categories: 学习笔记
cover: 
tags:
- Hexo
---

#### Hexo博客新手任务学习分支线：

### 主线任务1：

根据博客搭建教程完成博客的本地搭建，配合github完成线上部署。

教程链接：https://akilar.top/posts/6ef63e2d/

任务难度：⭐⭐⭐

### 支线任务1：

挑选一款合适的主题，推荐使用Butterfly主题，跟随主题文档完成基本配置。

教程链接：https://butterfly.js.org/

任务难度：⭐⭐⭐⭐⭐


### 主线任务2：

在根目录[Blogroot]下打开终端,使用 hexo new post <标题> 新建一篇文章。
可以在[Blogroot]/source/_post/目录下找到新建的文章。
可以通过修改[Blogroot]/scaffolds/post.md的模板内容来改变默认生成的文章格式。

教程链接：https://hexo.io/zh-cn/docs/writing

任务难度：⭐

### 支线任务2：

学习markdown基本语法，使用markdown语法编写博客。

教程链接：https://guides.github.com/features/mastering-markdown/

任务难度：⭐⭐

### 支线任务3：

熟练掌握markdown基本语法后，可以使用外挂标签丰富博客文章的格式。

教程链接：https://akilar.top/posts/615e2dec/

任务难度：⭐⭐

### 主线任务3：

学习github action，将博客源码托管至github，并通过github action完成线上部署。

教程链接：https://akilar.top/posts/f752c86d/

任务难度：⭐⭐⭐⭐

### 支线任务4：

使用Vercel部署博客网页，进一步提高访问速度

教程链接：https://akilar.top/posts/812734f8/

任务难度：⭐

### 博客优化副本任务：

按照糖果屋优化日记的教程索引，完成博客基本优化

教程链接：https://akilar.top/posts/7c16c4bb/

任务难度：⭐⭐⭐⭐⭐

### 博客美化副本任务：

按照糖果屋美化日记的教程索引，挑选喜爱的组件对博客进行自定义美化。

教程链接：https://akilar.top/posts/f99b208/

任务难度：难度不详，遇强则强

Q:

比如我建了一个叫“网址导航”的页面，我想在里面显示我加的网址（然后希望显示是和link的一样，因为我link的样式换了），然后link的页面叫“链接”，和“网址导航”是不同的页面

A:

难倒是不难。你找到flink.pug文件，复制一个，重命名一下，比如改成dlink.pug，然后在里面找到data.link这个关键词，改成data.dlink，然后找到page.pug，模仿里面的代码结构把dlink.pug加进去。然后hexo new page dlink，和友链差不多，写type：“dlink”，到data/目录下新建dlink.yml，友链咋写格式，这个就咋写。




