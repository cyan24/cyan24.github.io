---
layout: post
title:  "用chrome实时调试less编译的文件"
date:   2016-02-23
author: cyan
category: front
tags: less
---

首先看下chrome调试时，如何实时保存到本地文件。使用chrom调试工具中的 source 进入项目目录结构，右键选择本地文件目录add floder to workspace ,添加本地文件夹，之后选择本地文件右键map to network resource。
这样就能映射本地文件直接指向服务文件，也就是在chrome中直接调试样式后，ctrl+S 后直接同步保存到了本地文件。
接下来我们要使用chorm实时调试less文件，首先要安装一个监听文件 wr,类似grunt的watch,选择wr是因为他的速度相对其他监听软件快！先执行：

```html

npm install -g wr 

```


全局安装wr,之后开始我们可以实时监听到lessc 编译某一个less的命令：

比如：在less文件目录执行：

```html

wr ‘less duang.less duang.css’ duang.less

```

可以监听到当我们改变duang.less时，然后保存，可以看到duang.css同时做了自动编译后的改变。


<img src="{{ '/img/post/1602231.jpg' | prepend: site.baseurl }}" alt="">


那怎么实时去改变chrom里的css从而改变到less呢～？
现在我们要做的就是怎么把css和less做一个映射，我们用：

```html

lessc —source-map duang.less duang.css
```

去做一个资源映射。

<img src="{{ '/img/post/1602232.jpg' | prepend: site.baseurl }}" alt="">

同级目录马上生成一个map文件：

<img src="{{ '/img/post/1602233.jpg' | prepend: site.baseurl }}" alt="">

在css文件底部，可以看到一个映射关系的说明：


<img src="{{ '/img/post/1602234.jpg' | prepend: site.baseurl }}" alt="">

开启浏览器映射功能：

<img src="{{ '/img/post/1602235.jpg' | prepend: site.baseurl }}" alt="">
<img src="{{ '/img/post/1602236.jpg' | prepend: site.baseurl }}" alt="">


然后执行命令：

```html

wr 'lessc --source-map duang.less duang.css' duang.less

```

<img src="{{ '/img/post/1602237.jpg' | prepend: site.baseurl }}" alt="">

此时修改chrome中的less文件，保存刷新浏览器，同步可以修改本地的less 和css了。

<img src="{{ '/img/post/1602238.jpg' | prepend: site.baseurl }}" alt="">

标题修改为红色：

<img src="{{ '/img/post/1602239.jpg' | prepend: site.baseurl }}" alt="">

本地css和less都已自动修改：

<img src="{{ '/img/post/16022310.jpg' | prepend: site.baseurl }}" alt="">

<img src="{{ '/img/post/16022311.jpg' | prepend: site.baseurl }}" alt="">

以上说的这种方式在chrome调试工具的elements - Styles 下暂时没有办法做实时调试，着也是比较遗憾的一点。

