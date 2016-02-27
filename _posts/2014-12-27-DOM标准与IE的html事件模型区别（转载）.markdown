---
layout: post
title:  "DOM标准与IE的html事件模型区别（转载）"
date:   2014-12-27
author: cyan
category: front
tags: javascript
---

**事件**

HTML元素事件是浏览器内在自动产生的,当有事件发生时html元素会向外界(这里主要指元素事件的订阅者)发出各种事件,如click,onmouseover,onmouseout等等。

**DOM事件流**

DOM(文档对象模型)结构是一个树型结构，当一个HTML元素产生一个事件时，该事件会在元素结点与根结点之间的路径传播，路径所经过的结点都会收到该事件，这个传播过程可称为DOM事件流。

**主流浏览器的事件模型**

早在2004前在HTML元素事件的订阅，发送，传播，处理模型上各浏览器实现并不一致，直到DOM Level3中规定后，多数主流浏览器才陆陆续续支持DOM标准的事件处理模型 — 捕获型与冒泡型。
目前除IE浏览器外，其它主流的Firefox, Opera, Safari都支持标准的DOM事件处理模型。IE仍然使用自己的模型，即冒泡型，它模型的一部份被DOM采用，这点对于开发者来说也是有好处的，只使用DOM标准，IE都共有的事件处理方式才能有效的跨浏览器。

**冒泡型事件(Bubbling)**

这是IE浏览器对事件模型的实现，也是最容易理解的，至少笔者觉得比较符合实际的。冒泡，顾名思义，事件像个水中的气泡一样一直往上冒，直到顶端。从DOM树型结构上理解，就是事件由叶子结点沿祖先结点一直向上传递直到根结点；从浏览器界面视图HTML元素排列层次上理解就是事件由具有从属关系的最确定的目标元素一直传递到最不确定的目标元素.

**捕获型事件(Capturing)**

Netscape Navigator的实现，它与冒泡型刚好相反，由DOM树最顶层元素一直到最精确的元素，这个事件模型对于开发者来说（至少是我..）有点费解，因为直观上的理解应该如同冒泡型，事件传递应该由最确定的元素，即事件产生元素开始。
但这个模型在某些情况下也是很有用的，接下来会讲解到。

<img src="{{ '/img/post/1412270.jpg' | prepend: site.baseurl }}" alt=""> 

**DOM标准事件模型**

因为两个不同的模型都有其优点和解释，DOM标准支持捕获型与冒泡型，可以说是它们两者的结合体。它可以在一个DOM元素上绑定多个事件处理器，并且在处理函数内部，this关键字仍然指向被绑定的DOM元素，另外处理函数参数列表的第一个位置传递事件event对象。

首先是捕获式传递事件，接着是冒泡式传递，所以，如果一个处理函数既注册了捕获型事件的监听，又注册冒泡型事件监听，那么在DOM事件模型中它就会被调用两次。


<img src="{{ '/img/post/1412271.jpg' | prepend: site.baseurl }}" alt=""> 

注册与移除事件监听器

注册事件监听器，或又称订阅事件，当元素事件发生时浏览器回调该监听函数执行事件处理。目前主流浏览器中有两种注册事件的方法，一种是IE浏览器的，另一种是DOM标准的。

1、直接JS或HTML挂载法

```html
<div onclick="alert(this.innerHTML);">


element.onclick = function(){alert(this.innerHTML);}
```
移除时将事件属性设为nul即可，这个也是最常用的方法了，优缺点也是显然的：

简单方便，在HTML中直接书写处理函数的代码块，在JS中给元素对应事件属性赋值即可
IE与DOM标准都支持的一种方法，它在IE与DOM标准中都是在事件冒泡过程中被调用的。
可以在处理函数块内直接用this引用注册事件的元素
要给元素注册多个监听器，就不能用这方法了

2、IE下注册多个事件监听器与移除监听器方法

IE浏览器中HTML元素有个attachEvent方法允许外界注册该元素多个事件监听器,例如

 
element.attachEvent('onclick', observer);
attachEvent接受两个参数。第一个参数是事件名称，第二个参数observer是回调处理函数。这里得说明一下，有个经常会出错的地方，IE下利用attachEvent注册的处理函数调用时this指向不再是先前注册事件的元素，这时的this为window对象了，笔者很奇怪IE为什么要这么做，完全看不出好处所在。
要移除先前注册的事件的监听器,调用element的detachEvent方法即可，参数相同。

 
element.detachEvent('onclick', observer);
3、 DOM标准下注册多个事件监听器与移除监听器方法

实现DOM标准的浏览器与IE浏览器中注册元素事件监听器方式有所不同，它通过元素的addEventListener方法注册，该方法既支持注册冒泡型事件处理，又支持捕获型事件处理。

 
element.addEventListener('click', observer, useCapture);
addEventListener方法接受三个参数。第一个参数是事件名称，值得注意的是，这里事件名称与IE的不同，事件名称是没’on’开头的;第二个参数observer是回调处理函数;第三个参数注明该处理回调函数是在事件传递过程中的捕获阶段被调用还是冒泡阶段被调用

移除已注册的事件监听器调用element的removeEventListener即可，参数不变.

 
element.removeEventListener('click', observer, useCapture);

跨浏览器的注册与移除元素事件监听器方案

弄清楚DOM标准与IE的注册元素事件监听器之间的异同后，就可以实现一个跨浏览器的注册与移除元素事件监听器方案:

```javascript
//注册
function addEventHandler(element, evtName, callback, useCapture) {
   //DOM标准
    if (element.addEventListener) {
         element.addEventListener(evtName, callback, useCapture);
    }else {
       //IE方式,忽略useCapture参数
       element.attachEvent('on' + evtName, callback);
    }
}
//移除
function removeEventHandler(element, evtName, callback, useCapture) {
   //DOM标准
    if (element.removeEventListener) {
          element.removeEventListener(evtName, callback, useCapture);
    }else {
       //IE方式,忽略useCapture参数
       element.dettachEvent('on' + evtName, callback);
    }
}
```
如何取消浏览器事件的传递与事件传递后浏览器的默认处理

先说明取消事件传递与浏览器事件传递后的默认处理是两个不同的概念，可能很多同学朋友分不清，或者根本不存在这两个概念。

取消事件传递是指，停止捕获型事件或冒泡型事件的进一步传递。例如上图中的冒泡型事件传递中，在body处理停止事件传递后，位于上层的document的事件监听器就不再收到通知，不再被处理。

事件传递后的默认处理是指，通常浏览器在事件传递并处理完后会执行与该事件关联的默认动作（如果存在这样的动作）。例如，如果表单中input type 属性是 “submit”，点击后在事件传播完浏览器就就自动提交表单。又例如，input 元素的 keydown 事件发生并处理后，浏览器默认会将用户键入的字符自动追加到 input 元素的值中。

要取消浏览器的件传递,IE与DOM标准又有所不同。

在IE下,通过设置event对象的cancelBubble为true即可。

```javascript
function someHandle() {
   window.event.cancelBubble = true;
}
```

DOM标准通过调用event对象的stopPropagation()方法即可。

 ``javascript
function someHandle(event) {
   event.stopPropagation();
}
```

因些，跨浏览器的停止事件传递的方法是:


```javascript
function someHandle(event) {
  event = event || window.event;
  if(event.stopPropagation)
     event.stopPropagation();
  else event.cancelBubble = true;
}
```

取消事件传递后的默认处理，IE与DOM标准又不所不同。

在IE下,通过设置event对象的returnValue为false即可。
 

```javascript
function someHandle() {
   window.event.returnValue = false;
}
```
DOM标准通过调用event对象的preventDefault()方法即可。

 
 ```javascript
function someHandle(event) {
   event.preventDefault();
}
```

因些，跨浏览器的取消事件传递后的默认处理方法是：

 
```javascript
function someHandle(event) {
  event = event || window.event;
  if(event.preventDefault)
     event.preventDefault();
  else event.returnValue = false;
}
```

捕获型事件模型与冒泡型事件模型的应用场合

标准事件模型为我们提供了两种方案，可能很多朋友分不清这两种不同模型有啥好处，为什么不只采取一种模型。
这里抛开IE浏览器讨论（IE只有一种，没法选择）什么情况下适合哪种事件模型。

1、捕获型应用场合

捕获型事件传递由最不精确的祖先元素一直到最精确的事件源元素，传递方式与操作系统中的全局快捷键与应用程序快捷键相似。当一个系统组合键发生时，如果注册了系统全局快捷键监听器，该事件就先被操作系统层捕获，全局监听器就先于应用程序快捷键监听器得到通知，也就是全局的先获得控制权，它有权阻止事件的进一步传递。所以捕获型事件模型适用于作全局范围内的监听，这里的全局是相对的全局，相对于某个顶层结点与该结点所有子孙结点形成的集合范围。

例如你想作全局的点击事件监听，相对于document结点与document下所有的子结点，在某个条件下要求所有的子结点点击无效，这种情况下冒泡模型就解决不了了，而捕获型却非常适合，可以在最顶层结点添加捕获型事件监听器，伪码如下:

```javascript
if(canEventPass == false) {
    //取消事件进一步向子结点传递和冒泡传递
    event.stopPropagation();
    //取消浏览器事件后的默认执行
    event.preventDefault();
}
```

这样一来，当canEventPass条件为假时，document下所有的子结点click注册事件都不会被浏览器处理。

2、 冒泡型的应用场合

可以说我们平时用的都是冒泡事件模型，因为IE只支持这模型。这里还是说说，在恰当利用该模型可以提高脚本性能。在元素一些频繁触发的事件中，如onmousemove, onmouseover,onmouseout,如果明确事件处理后没必要进一步传递，那么就可以大胆的取消它。此外，对于子结点事件监听器的处理会对父层监听器处理造成负面影响的，也应该在子结点监听器中禁止事件进一步向上传递以消除影响。

综合案例分析

最后结合下面HTML代码作分析:

```html
<body onclick="alert('current is body');">
  <div id="div0" onclick="alert('current is '+this.id)">
     <div id="div1" onclick="alert('current is '+this.id)">
        <div id="div2">
            <div id="event_source"
                 onclick="alert('current is '+this.id)"
                 style="height:200px;width:200px;background-color:red;">
            </div>
         </div>
   </div>
  </div>
</body>
```

HTML运行后点击红色区域,这是最里层的DIV,根据上面说明,无论是DOM标准还是IE,直接写在html里的监听处理函数是事件冒泡传递时调用的,由最里层一直往上传递,所以会先后出现
current is event_source
current is div2
current is div1
current is div0
current is body

添加以下片段:

```javascript
var div2 = document.getElementById('div2');
addEventHandler(div2,'click',function(event){
   event = event || window.event;
   if(event.stopPropagation)
     event.stopPropagation();
   else event.cancelBubble = true;
},false);
```

当点击红色区域后,根据上面说明,在泡冒泡处理期间,事件传递到div2后被停止传递了,所以div2上层的元素收不到通知,所以会先后出现:

current is event_source
current is div2

在支持DOM标准的浏览器中,添加以下代码:

```javascript
document.body.addEventListener('click',function(event){
     event.stopPropagation();
},true);
```

以上代码中的监听函数由于是捕获型传递时被调用的,所以点击红色区域后,虽然事件源是ID为event_source的元素,但捕获型选传递,从最顶层开始,body结点监听函数先被调用,并且取消了事件进一步向下传递,所以只会出现current is body。

转载于：http://www.cnblogs.com/ilexcai/archive/2011/09/05/2168094.html

