---
layout: post
title:  "关于css3 动画的一点笔记"
date:   2016-03-21
author: cyan
category: front
tags: 动画
---


代码中当要使用transform 和 opicity 比如同时0.3秒过渡时，下面这种写法不推荐，因为all会引起性能问题。

```css

div {
     transform :translate (0 ,-20px);
     transition: all 0.3s ease;
}
```


推荐使用下面这种写法：

```css

div {
     transform :translate (0 ,-20px);
     transition: opacity 0.3s ease,transform 0.3s ease;
}

```

另外，在兼容各种webkit 的写法时，可以使用autoprefixer 插件，在less编译成css时再编译一次，可以高效得把各种前缀都自动加了。