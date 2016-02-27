---
layout: post
title:  "HTTP 头控制缓存以及header常用指令及配置方式"
date:   2014-01-07
author: cyan
category: front
tags: 性能优化
---

网页的缓存是由HTTP消息头中的“Cache-control”来控制的，常见的取值有private、no-cache、max-age、must-revalidate等，默认为private。其作用根据不同的重新浏览方式分为以下几种情况：

1. 打开新窗口
值为private、no-cache、must-revalidate，那么打开新窗口访问时都会重新访问服务器。
而如果指定了max-age值，那么在此值内的时间里就不会重新访问服务器，例如：
Cache-control: max-age=5(表示当访问此网页后的5秒内再次访问不会去服务器)

2. 在地址栏回车
值为private或must-revalidate则只有第一次访问时会访问服务器，以后就不再访问。
值为no-cache，那么每次都会访问。
值为max-age，则在过期之前不会重复访问。

3. 按后退按扭
值为private、must-revalidate、max-age，则不会重访问，
值为no-cache，则每次都重复访问

4. 按刷新按扭
　 无论为何值，都会重复访问

Cache-control值为“no-cache”时，访问此页面不会在Internet临时文章夹留下页面备份。

另外，通过指定“Expires”值也会影响到缓存。例如，指定Expires值为一个早已过去的时间，那么访问此网时若重复在地址栏按回车，那么每次都会重复访问： Expires: Fri, 31 Dec 1999 16:00:00 GMT

比如：禁止页面在IE中缓存

http响应消息头部设置：


CacheControl = no-cache
Pragma=no-cache
Expires = -1

Expires是个好东东，如果服务器上的网页经常变化，就把它设置为-1，表示立即过期。如果一个网页每天凌晨1点更新，可以把Expires设置为第二天的凌晨1点。

当HTTP1.1服务器指定CacheControl = no-cache时，浏览器就不会缓存该网页。

旧式 HTTP 1.0 服务器不能使用 Cache-Control 标题。
所以为了向后兼容 HTTP 1.0 服务器，IE使用Pragma:no-cache 标题对 HTTP 提供特殊支持。
如果客户端通过安全连接 (https://)/与服务器通讯，且服务器在响应中返回 Pragma:no-cache 标题，
则 Internet Explorer不会缓存此响应。注意：Pragma:no-cache 仅当在安全连接中使用时才防止缓存，如果在非安全页中使用，处理方式与 Expires:-1相同，该页将被缓存，但被标记为立即过期。

___
header常用指令

header分为三部分：

第一部分为HTTP协议的版本(HTTP-Version)；
第二部分为状态代码(Status)；
第三部分为原因短语(Reason-Phrase)。



// fix 404 pages: 用这个header指令来解决URL重写产生的404 header
header('HTTP/1.1 200 OK');

// set 404 header: 页面没找到
header('HTTP/1.1 404 Not Found');

//页面永久重定向，可以告诉搜索引擎更新它们的urls
// set Moved Permanently header (good for redrictions)
// use with location header
header('HTTP/1.1 301 Moved Permanently');

// 访问受限
header('HTTP/1.1 403 Forbidden');

// 服务器错误
header('HTTP/1.1 500 Internal Server Error');

// 重定向到一个新的位置
// redirect to a new location:
header('Location: http://www.sina.com.cn);

延迟一段时间后重定向
// redrict with delay:
header('Refresh: 10; url=http://www.sina.com.cn');
print 'You will be redirected in 10 seconds';

// 覆盖 X-Powered-By value
// override X-Powered-By: PHP:
header('X-Powered-By: PHP/4.4.0');
header('X-Powered-By: Brain/0.6b');

// 内容语言 (en = English)
// content language (en = English)
header('Content-language: en');

//最后修改时间(在缓存的时候可以用到)
// last modified (good for caching)
$time = time() - 60; // or filemtime($fn), etc
header('Last-Modified: '.gmdate('D, d M Y H:i:s', $time).' GMT');

// 告诉浏览器要获取的内容还没有更新
// header for telling the browser that the content
// did not get changed
header('HTTP/1.1 304 Not Modified');

// 设置内容的长度 (缓存的时候可以用到):
// set content length (good for caching):
header('Content-Length: 1234');

// 用来下载文件:
// Headers for an download:
header('Content-Type: application/octet-stream');
header('Content-Disposition: attachment; filename="example.zip"');
header('Content-Transfer-Encoding: binary');

// 禁止缓存当前文档:
// load the file to send:readfile('example.zip');
// Disable caching of the current document:
header('Cache-Control: no-cache, no-store, max-age=0, must-revalidate');
header('Expires: Mon, 26 Jul 1997 05:00:00 GMT');

// 设置内容类型:
// Date in the pastheader('Pragma: no-cache');
// set content type:
header('Content-Type: text/html; charset=iso-8859-1');
header('Content-Type: text/html; charset=utf-8');

// plain text file
header('Content-Type: text/plain');

// JPG picture
header('Content-Type: image/jpeg');

// ZIP file
header('Content-Type: application/zip');

// PDF file
header('Content-Type: application/pdf');

// Audio MPEG (MP3,...) file
header('Content-Type: audio/mpeg');

// Flash animation// show sign in box
header('Content-Type: application/x-shockwave-flash');

// 显示登录对话框，可以用来进行HTTP认证
header('HTTP/1.1 401 Unauthorized');
header('WWW-Authenticate: Basic realm="Top Secret"');
print 'Text that will be displayed if the user hits cancel or ';
print 'enters wrong login data';

___

当使用Expires / Cache-Control的时候，尽量给图片，样式表，脚本等设置一个足够大的缓存时间，如果在此期间，缓存文件有过修改，更新最简单的方式改名或者设置一个查询参数，比如开始图片名是logo.gif，如果做了一个新的图片，你想更新，可以把图片名字修改成logo_v2.gif，或者把图片的路径修改成logo.gif?foobar，这样，浏览器就会去请求新的图片了。不过，并不是所有的Web组件都适合有一个大的缓存时间，比如html，就不适合使用过大的缓存时间，否则你在缓存到期前，就没机会更新任何东西了。

___
配置方式：

1. 如果你使用tomcat作为web服务器和容器使用的话可以编写一个filter来实现。
2. 如果使用apache+tocmat的话可以使用apache的mod_expires来实现。

这里以情况1举例：

```java

public class ExpiresFilter implements Filter {
//log日志
private Log log = LogFactory.getLog(getClass());

//记录客户端需要缓存的静态文件类型及缓存时间 KEY=文件类型(String型)，VALUE=缓存时间(Long型)
private Map map = new HashMap();

@Override
public void init(FilterConfig config) throws ServletException {
Enumeration en = config.getInitParameterNames();
while(en.hasMoreElements()){
//取得静态文件类型
String paramName = en.nextElement().toString();
if(paramName == null || paramName.equals("")) continue;

//取得缓存时间 。单位：秒
String paramValue = config.getInitParameter(paramName);
try{
int time = Integer.valueOf(paramValue);
if(time > 0){
//存入MAP中
map.put(paramName, new Long(time));
log.info("Set " + paramName + "expires seconds: " + time);
}
} catch (Exception e){
log.warn("Exception in initilazing ExpiredFilter. Set " + paramName + " error", e);
}
}

}

@Override
public void destroy() {
}

@Override
public void doFilter(ServletRequest request, ServletResponse response,
FilterChain chain) throws IOException, ServletException {
//取得当前访问的URI
String uri = ((HttpServletRequest)request).getRequestURI();

int n = uri.lastIndexOf(".");

if(n != (-1)){
//取得访问资源的扩展名
String ext = uri.substring(n);

Long exp = (Long)map.get(ext);

if(exp != null){
HttpServletResponse resp = (HttpServletResponse) response;
//设置缓存
resp.setHeader("Cache-Control", "max-age=" + exp);
//设置过期时间
resp.setDateHeader("Expires", System.currentTimeMillis() + exp*1000);
}
}

chain.doFilter(request, response);
}
}
```

web.xml配置

```html
<filter>
<filter-name>expiresFilter</filter-name>
<filter-class>com.yinbo.common.web.filter.ExpiresFilter<filter-class>
<init-param>
<param-name>.css</para-name>
<!-- 单位时间：秒 -->
<param-value>3600</para-value>
</init-param>
<init-param>
<param-name>.gif</para-name>
<!-- 单位时间：秒 -->
<param-value>3600</para-value>
</init-param>
<!-- 该参数在开发阶段可以不加，以方便JS调试 -->
<init-param>
<param-name>.js</para-name>
<!-- 单位时间：秒 -->
<param-value>3600</para-value>
</init-param>
</filter>

<filter-mapping>
<filter-name>expiresFilter</filter-name>
<url-pattern>.css</url-pattern>
</filter-mapping>
<filter-mapping>
<filter-name>expiresFilter</filter-name>
<url-pattern>.gif</url-pattern>
</filter-mapping>
<filter-mapping>
<filter-name>expiresFilter</filter-name>
<url-pattern>.js</url-pattern>
</filter-mapping>
```