---
layout: post
title:  "高性能网站建设指南读书笔记之CSS Sprites"
date:   2013-11-04
author: cyan
category: front
tags: css 性能优化
---

大名鼎鼎的CSS Sprites。今天来总结一下：

CSS Sprites简介

通常被意译为“CSS图像拼合”或“CSS贴图定位”。CSS Sprites并不是一门新技术，目前它已经在网页开发中发展得较为成熟，一些较为成熟的网站的网页中到处都可发现css sprites 的影子。但CSS Sprites并不是什么金科玉律，但在很多情况下，它有着一定的优势，最重要的是它可以减轻服务器的负载，提高网页加载速度。随着Web设计向着精致、 巧妙的方向发展，设计师们开始考虑使用非Javascript的方式制作鼠标滑过、悬停菜单的效果，这时CSS Sprite应运而生。

**说白了，CSS Sprites其实就是把网页中一些背景图片整合到一张图片文件中，再利用CSS的“background-image”，“background- repeat”，“background-position”的组合进行背景定位，background-position可以用数字能精确的定位出背景图片的位置。**

当页面加载时，不是加载每个单独图片，而是一次加载整个组合图片。这是一个了不起的改进，它大大减少了HTTP请求的次数，减轻服务器压力，同时缩短了悬停加载图片所需要的时间延迟，使效果更流畅，不会停顿。

___
CSS Sprites优点

CSS Sprites为什么突然跑火，跟能够提升网站性能有关。显而易见，这是它的巨大优点之一。

利用CSS Sprites能很好地减少了网页的http请求，从而大大的提高了页面的性能，这是CSS Sprites最大的优点，也是其被广泛传播和应用的主要原因；
个人认为CSS Sprites能减少图片的字节，我曾经比较过多次3张图片合并成1张图片的字节总是小于这3张图片的字节总和。
CSS Sprites缺点

诚然CSS Sprites是如此的强大，但是也存在一些不可忽视的缺点。

在图片合并的时候，你要把多张图片有序的合理的合并成一张图片，还要留好只够的空间，防止板块内不会出现不必要的背景，否则可能会出现出现干扰图片的情况；这些还好，做痛苦的是在宽屏，高分辨率的屏幕下的自适应页面，你的图片如果不够宽，很容易出现背景断裂；
CSS Sprites在开发的时候比较麻烦，你要通过photoshop或其他工具测量计算每一个背景单元的精确位置，这是针线活，没什么难度，但是很繁琐；不过网上已经有高手开发出“CSS Sprites 样式生成工具”，大家可以尝试一下。
CSS Sprites在维护的时候比较麻烦，sprites是一般双刃剑，如果页面背景有少许改动，一般就要改这张合并的图片，无需改的地方最好不要动，这样避 免改动更多的css，如果在原来的地方放不下，有只能（最好）往下加图片，这样图片的字节就增加了，因为每次的图片改动都得往这个图片删除或添加内容，显 得稍微繁琐，而且重新算图片的位置（尤其是这种上千px的图）也是一件颇为不爽的事情。当然，在性能的口号下，这些都是可以克服的。
由于图片的位置需要固定为某个绝对数值，这就失去了诸如center之类的灵活性。
___

CSS Sprites总结

性能压倒一切。CSS Sprites非常值得学习和应用，特别是页面有一堆ico（图标）。总之很多时候大家要权衡一下利弊，在决定是不是应用CSS Sprites。为保持兼容性和维护性，sprites图片中的各个部分保持一定的距离是一种不错的做法。


___
书中示例：

```html

   <style> #navbar span { width:31px; height:31px; display:inline; float:left; background-image:url(/images/spritebg.gif?t=1442038329); } .home { background-position:0 0; margin-right:4px; margin-left: 4px;} .gifts { background-position:-32px 0; margin-right:4px;} .cart { background-position:-64px 0; margin-right:4px;} .settings { background-position:-96px 0; margin-right:4px;} .help { background-position:-128px 0; margin-right:0px;} 
   </style> 

   <div id="navbar" style="background-color: #F4F5EB; border: 2px ridge #333; width: 180px; height: 32px; padding: 4px 0 4px 0;"> 
      <a href="javascript:alert('Home')" title="Home"><span class="home"></span></a>
      <a href="javascript:alert('Gifts')" title="Gifts"><span class="gifts"></span></a> 
      <a href="javascript:alert('Cart')" title="Cart"><span class="cart"></span></a> 
      <a href="javascript:alert('Settings')" title="Settings"><span class="settings"></span></a>      <a href="javascript:alert('Help')" title="Help"><span class="help"></span></a> 
    </div>
```
