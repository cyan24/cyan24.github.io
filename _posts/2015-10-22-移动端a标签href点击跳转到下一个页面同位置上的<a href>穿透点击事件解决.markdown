---
layout: post
title:  "移动端a标签href点击跳转到下一个页面同位置上的< a href>穿透点击事件解决"
date:   2015-10-22
author: cyan
category: front
tags: html5
---

项目中遇到的一个问题。移动开发的h5页面上，点击前一个页面的a标签，进入下一个页面后，此时下一个页面的相同位置上也有一个a标签，此时会将后一个页面中的a标签点击触发。

***解决方案：***

将原来的< a>标签改为 div ，用移动端的 ontouchstart 事件代替，此时前一个页面点击事件不会穿透到下一个页面同位置的点击区域。

```javascript

$(function () {
         var div = document.getElementById("operate_tbkp");
         //touchstart类似mousedown
         //div.ontouchstart = function(e){
         div.ontouchstart = function(e){
         //事件的touches属性是一个数组，其中一个元素代表同一时刻的一个触控点，从而可以通过touches获取多点触控的每个触控点
         //由于我们只有一点触控，所以直接指向[0]
         //var touch = e.touches[0];
         //获取当前触控点的坐标，等同于MouseEvent事件的clientX/clientY
         //var x = touch.clientX;
         //var y = touch.clientY;
         window.location.href='../applicationformAppcs/formBilling';
         };
 })

 ```