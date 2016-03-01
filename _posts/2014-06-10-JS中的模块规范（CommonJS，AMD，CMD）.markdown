---
layout: post
title:  "JS中的模块规范(CommonJS，AMD，CMD)"
date:   2014-06-10
author: cyan
category: front
tags: javascript
---
如果你听过js模块化这个东西，那么你就应该听过或CommonJS或AMD甚至是CMD这些规范咯，我也听过，但之前也真的是听听而已。
 
   现在就看看吧，这些规范到底是啥东西，干嘛的。
 
 
 
一、CommonJS
 
 CommonJS就是为JS的表现来制定规范，因为js没有模块的功能所以CommonJS应运而生，它希望js可以在任何地方运行，不只是浏览器中。
 
 CommonJS能有一定的影响力，我觉得绝对离不开Node的人气，不过喔，Node，CommonJS，浏览器甚至是W3C之间有什么关系呢，我找到了个贴切的图：
 
 
 
  |---------------浏览器----- ------------------|        |--------------------------CommonJS----------------------------------|
 
  |  BOM  |       | DOM |        | ECMAScript |         | FS |           | TCP |         | Stream |        | Buffer |          |........|
 
  |-------W3C-----------|       |---------------------------------------Node--------------------------------------------------|
 
 
 
CommonJS定义的模块分为:{模块引用(require)} {模块定义(exports)} {模块标识(module)}
 
require()用来引入外部模块；exports对象用于导出当前模块的方法或变量，唯一的导出口；module对象就代表模块本身。
 
 
 
比如说我们就可以这样用了：
 
复制代码
1 //sum.js
2 exports.sum = function(){...做加操作..};
3 
4 //calculate.js
5 var math = require('sum');
6 exports.add = function(n){
7     return math.sum(val,n);
8 };
复制代码
 
 
虽说Node遵循CommonJS的规范，但是相比也是做了一些取舍，填了一些新东西的。
 
不过，说了CommonJS也说了Node，那么我觉得也得先了解下NPM了。NPM作为Node的包管理器，不是为了帮助Node解决依赖包的安装问题嘛，那它肯定也要遵循CommonJS规范啦，它遵循包规范（还是理论）的。
 
CommonJS WIKI讲了它的历史，还介绍了modules和packages等。
 
 
 
二、AMD
 
CommonJS是主要为了JS在后端的表现制定的，他是不适合前端的，为什么这么说呢？
 
这需要分析一下浏览器端的js和服务器端js都主要做了哪些事，有什么不同了：
 
 
 
---------------------------------------服务器端JS   | 浏览器端JS-------------------------------------------

 				相同的代码需要多次执行  |    代码需要从一个服务器端分发到多个客户端执行
 
 				CPU和内存资源是瓶颈   |    带宽是瓶颈
 
 				加载时从磁盘中加载   |    加载时需要通过网络加载
 
---------------------------------------------------------------------------------------------------------------
 
 
 
于是乎，AMD(异步模块定义)出现了，它就主要为前端JS的表现制定规范。
 
AMD就只有一个接口：define(id?,dependencies?,factory);
 
它要在声明模块的时候制定所有的依赖(dep)，并且还要当做形参传到factory中，像这样：

 
```js 
define(['dep1','dep2'],function(dep1,dep2){...});
``` 
 
要是没什么依赖，就定义简单的模块，下面这样就可以啦：
 
 
```js
define(function(){
    var exports = {};
    exports.method = function(){...};
    return exports; });
```
 
咦，这里有define，把东西包装起来啦，那Node实现中怎么没看到有define关键字呢，它也要把东西包装起来呀，其实吧，只是Node隐式包装了而已.....
 
RequireJS就是实现了AMD规范的呢。
 
这有AMD的WIKI中文版，讲了很多蛮详细的东西，用到的时候可以查看：AMD的WIKI中文版
 
 
 
三、CMD
 
大名远扬的玉伯写了seajs，就是遵循他提出的CMD规范，与AMD蛮相近的，不过用起来感觉更加方便些，最重要的是中文版。
 
```js
define(function(require,exports,module){...});
```
 
用过seajs吧，这个不陌生吧，对吧.

四、AMD与CMD的区别

CMD相当于按需加载，定义一个模块的时候不需要立即制定依赖模块，在需要的时候require就可以了，比较方便；而AMD则相反，定义模块的时候需要制定依赖模块，并以形参的方式引入factory中

```js
//AMD方式定义模块
define(['dep1','dep2'],function(dep1,dep2){
     //内部只能使用制定的模块
      return function(){};
});
//CMD
define(function(require,exports,module){
   //此处如果需要某XX模块，可以引入
   var xx=require('XX');
});
```

而SEAJS也有use功能也是需要先引入所有依赖的模块，如

```js
//SEAJS.Use方式
seajs.use(['dep1','dep2'],function(dep1,dep2){
     //这里实现事务
});
```

五、插件支持

但全球有两种比较流行的 JavaScript 模块化体系，一个是 Node 实现的 CommonJS，另外一个是 AMD。很多类库都同时支持 AMD 和 CommonJS，但是不支持 CMD。或许国内有很多 CMD 模块，但并没有在世界上流行起来。

现在比较火的 React 及周边类库，就是直接使用 CommonJS 的模块体系，使用 npm 管理模块，使用 Browserify 打包输出模块。

不久的将来 ES6 中新的模块化标准，可能就都得遵循新的标准了，什么AMD、CMD可能到时也不会怎么用了。

但是目前来说，前端开发没有用模块化编程就真的out的了，而目前的模块化编程，本人还是建议用SEAJS，虽然很多插件需要追加或修改一小块代码才能支持。但改过一次就能反复使用，也不会影响其它标准的支持。总体还算是比较方便实用的