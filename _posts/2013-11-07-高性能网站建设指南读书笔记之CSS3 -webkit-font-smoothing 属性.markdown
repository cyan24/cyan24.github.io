---
layout: post
title:  "高性能网站建设指南读书笔记之CSS3 -webkit-font-smoothing 属性"
date:   2013-11-07
author: cyan
category: front
tags: css 性能优化
---

CSS3里面加入了一个“-webkit-font-smoothing”属性。这个属性可以使页面上的字体抗锯齿,使用后字体看起来会更清晰舒服。加上之后就顿时感觉页面小清晰了。


在测试中将 -webkit-font-smoothing设置为none，非常模糊。


将-webkit-font-smoothing设置为antialiased，变得非常平滑，效果非常不错。


其默认可以支持6个值（如图），暂时我能看到效果的就是三个：
none | subpixel-antialiased | antialiased
其他的三个，我设置了，好像没什么变化。大家可以自己在控制台调试看看。

<img src="{{ '/img/post/1312193.jpg' | prepend: site.baseurl }}" alt=""> 