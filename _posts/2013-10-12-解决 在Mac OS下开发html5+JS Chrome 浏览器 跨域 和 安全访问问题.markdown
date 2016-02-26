---
layout: post
title:  "解决在Mac OS下开发html5+JS Chrome跨域和安全访问问题"
date:   2013-10-12
author: cyan
category: front
tags: 浏览器 跨域 安全
---

从去年到今年一直从事 Hybrid开发模式，发现在mac os下开发html5 ＋js代码会遇到好多问题，各个浏览器兼容性 做的最好的就是chrome，chrome也是最好的调试工具。

而且我们在一个团队的开发中经常遇到 AJAX  请求跨域的问题，跨域是让人非常头疼的问题，或者遇到  Https  证书 没有授权的问题。这些问题在goolge 的 chrome 浏览器下都可以解决的， 方法是：

用命令行打开 Google Chrome 就好了

 Mac os 下面用

```javascript

/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --disable-web-security  
```
or

```javascript
open -a "Google Chrome" --args --disable-web-security  
```

然后就可以看到下面的效果了：
<img src="{{ '/img/131012.jpg' | prepend: site.baseurl }}" alt=""> 

Ubuntu Linux:

```javascript
chromium-browser --disable-web-security  
```

windows

```javascript

chrome.exe --disable-web-security  
```
用命令行打开 Apple Safafi 方法是：

Mac OS 下：

open -a '/Applications/Safari.app' --args --disable-web-security  

Windows：

C:\Program Files\Safari\Safari.exe --disable-web-security  
