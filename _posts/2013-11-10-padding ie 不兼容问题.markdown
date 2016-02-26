---
layout: post
title:  "padding ie 不兼容问题"
date:   2013-11-10
author: cyan
category: front
tags: IE 兼容
---

在使用padding时，在ie 兼容时通常我们遇到3种情况：

* IE6 正常 IE7/FF不正常

这种情况下我们这样处理：

```html
   padding: 7px !important;（针对FF/IE7）
   padding: 6px;（针对IE6）
```

* IE6 IE7正常 FF不正常

这种情况我们要这么处理，因为!important IE7也是能识别的！

```html

  padding: 7px;（针对FF）
 *padding: 6px;（针对IE6/IE7）
```
 * E6 IE7 FF都不一样
```

这种情况我们这样来处理：

```html
   padding: 7px;（针对FF）
  *padding: 6px;（针对IE7）
  _padding: 5px; （针对IE6）
```