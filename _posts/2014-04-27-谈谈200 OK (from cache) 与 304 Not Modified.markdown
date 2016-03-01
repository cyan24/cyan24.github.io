---
layout: post
title:  "谈谈200 OK (from cache) 与 304 Not Modified"
date:   2014-04-27
author: cyan
category: front
tags: 性能优化
---

为什么有的缓存是 200 OK (from cache)，有的缓存是 304 Not Modified 呢？很简单，看运维是否移除了 Entity Tag。移除了，就总是 200 OK (from cache)。没有移除，就两者交替出现。

其实， 200 OK (from cache)  是浏览器没有跟服务器确认，直接用了浏览器缓存；而 304 Not Modified 是浏览器和服务器多确认了一次缓存有效性，再用的缓存。

它们都是在设置了缓存的情况下触发的。

那么，两者触发的时机有什么区别呢？200 OK (from cache) 是直接点击链接访问，输入网址按回车访问也能触发；而 304 Not Modified 是刷新页面时触发，或是设置了长缓存、但 Entity Tags 没有移除时触发。这是经过查阅资料得出的结论。博主实际测试了一下，结论与之相符：

<img src="{{ '/img/post/1404271.jpg' | prepend: site.baseurl }}" alt=""> 

直接访问有缓存的网站都触发 200 OK (from cache)

<img src="{{ '/img/post/1404272.jpg' | prepend: site.baseurl }}" alt="">

图2 – 刷新浏览器则会触发 304 Not Modifie

<img src="{{ '/img/post/1404273.jpg' | prepend: site.baseurl }}" alt=""> 

同一域名下，没有 Entity Tag 的资源直接访问，是 200 OK (from cache) 的结果

<img src="{{ '/img/post/1404274.jpg' | prepend: site.baseurl }}" alt=""> 


同一域名下，有 Entity Tag ，直接访问就会触发 304 Not Modified
现在一般都会设置长时间的缓存，正确设置方式参考这两篇笔记：


<a href="https://www.bokeyy.com/post/high-performance-web-sites-rule3.html">如何正确设置 Expires Header 及其原理</a>

<a href="https://www.bokeyy.com/post/high-performance-web-sites-rule-13.html">如何正确移除 Entity Tags 及其原理</a>

本文并不是说影响浏览器缓存只有 ETag 这一个因素的意思，请大家不要误解。只是就“为什么我加了缓存，有的却是 304 Not Modified， 而不是 200 OK(from cache)”这件事给出一个一针见血的原因和解答.