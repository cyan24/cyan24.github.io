---
layout: post
title:  "zepto lazyload图片"
date:   2015-04-24
author: cyan
category: front
tags: zepto
---

图片懒加载有很多js插件，非常著名的属jQuery的Lazy Load了。自己做手机项目上也需要图片懒加载，而且手机上的图片懒加载有两种：一种是普通img标签的，一种是div标签（或者其他标签）上加背景图片的。所以就练手写了个支持以上两种情况的Zepto小插件。

###功能：
支持img标签图片懒加载、div标签（或者其他标签）的背景图片懒加载；
支持预加载。默认是图片出现在屏幕时开始加载，可以设置threshold参数，如threshold:100，此时图片出现在屏幕之前100px时开始加载；
背景图片懒加载时，自动添加base64的1*1透明图片做默认背景图。也可以自定义默认背景图，参数为placeholder。
###使用方法：

引用1.1.3的Zepto（我只测过这个目前最新版），并引用picLazyLoad.js

```html

<script src="1.1.3/zepto.min.js"></script>
<script src="1.0/zepto.picLazyLoad.min.js"></script>

```
在要懒加载的标签上，加类名或者id名，方便调用。在标签上加data-original="original.jpg"，original.jpg为实际加载图片路径。如果是img标签，还需要加src="blank.png"。blank.png为默认背景图，建议用base64图。例如：

```html
<img class="test-lazyload" src="blank.png" data-original="original.jpg"><div class="test-lazyload" data-original="original.jpg"></div>
```

***调用:***


```javascript

$('.test-lazyload').picLazyLoad();
```

如需加参数：

```javascript

$('.test-lazyload').picLazyLoad({
    threshold: 100,
    placeholder: 'http://image.yihaodianimg.com/front-homepage/global/images/blank.gif'
});
```
<a href="http://ons.me/wp-content/uploads/2014/05/picLazyLoad/">→示例DEMO←</a>

* 已知问题：
iPhone 5S 7.1.1里，所有浏览器，网页滑到下面，刷新网页，不执行onscroll方法，获取不到scrolltop值。导致此时刷新网页，网页下方的图片没有默认加载。
* 待增加功能：
现在是直接显示图片show，以后增加fadeIn图片淡入；
现在是只能根据窗口懒加载图片，待增加根据容器、tab选项卡等。

* 小提示：
使用背景图片比使用img标签懒加载浏览器显示速度要快，不知道是不是后者多了一步替换src路径操作导致的；
如果要兼容更多的设备，使用背景图片可以显示更清晰，因为可以设置background-size，之前的文章有提到过。