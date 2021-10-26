---
title: 检测Headless Chrome
desc: 如何检测Headless Chrome
date: 2021-10-21 17:26:25
tags:
  - web
  - 风控
---

什么是无头浏览器（Headless browser）？为什么要检测它？如何检测它？

<!--more-->

## 什么是无头浏览器（Headless browser）？

无头浏览器是一种无需图形界面就可以使用的浏览器。可以通过程序控制无头浏览器来实现任务的自动化，比如进行测试、截屏、保存PDF、爬虫等。

## 为什么要检测无头浏览器（Headless browser）？

无头浏览器会被用来批量自动化执行恶意程序，如数据爬虫、寻找网页漏洞、伪造访问数据等等。

PhantomJS是曾经非常流行的一个无头浏览器。它是基于Qt框架开发的，因此与大多数浏览器不同，我们可以通过一些浏览器指纹识别技术来检测。

从Chrome 59开始，Google发布了Headless Chrome。与PhantomJS不同，它基于普通的Chrome，大大增加了检测的难度。

## 无头浏览器（Headless browser）的检测

### 用户代理（User Agent）*[有效]*

```js
if (/HeadlessChrome/.test(window.navigator.userAgent)) {
    console.log("Chrome headless detected");
}
```

### 插件（Plugins）*[有效]*

navigator.plugins返回浏览器插件数组。通常情况下，普通的Chrome浏览器中会有一些默认插件，如Chrome PDF阅读器或者Chrome Google Native Client。但是在headless模式下，插件为空。

```js
if(navigator.plugins.length == 0) {
    console.log("It may be Chrome headless");
}
```

### 语言（Languages）*[无效]*

```js
if(navigator.languages == "") {
    console.log("Chrome headless detected");
}
```

### WebGL *[无效]*

```js
var canvas = document.createElement('canvas');
var gl = canvas.getContext('webgl');

var debugInfo = gl.getExtension('WEBGL_debug_renderer_info');
var vendor = gl.getParameter(debugInfo.UNMASKED_VENDOR_WEBGL);
var renderer = gl.getParameter(debugInfo.UNMASKED_RENDERER_WEBGL);

if(vendor == "Brian Paul" && renderer == "Mesa OffScreen") {
    console.log("Chrome headless detected");
}
```

### Modernizr检测hairline

`Modernizr`可以测试浏览器中的HTML和CSS特性。Headless Chrome不支持hairline，这可以检测hidpi/retina对hairline的支持。

```js
if(!Modernizr["hairline"]) {
    console.log("It may be Chrome headless");
}
```

### 图像 *[无效]*

如果是普通的 Chrome，图片的宽度和高度取决于浏览器的缩放，但与零不同。在无头的 Chrome 中，图片的宽度和高度都等于零。

```js
var body = document.getElementsByTagName("body")[0];
var image = document.createElement("img");
image.src = "http://iloveponeydotcom32188.jg";
image.setAttribute("id", "fakeimage");
body.appendChild(image);
image.onerror = function(){
    if(image.width == 0 && image.height == 0) {
        console.log("Chrome headless detected");
    }
}
```

### Webdriver *[有效]*

```js
if(navigator.webdriver) {
    console.log("Chrome headless detected");
}
```

### window.chrome *[无效]*

```js
if(isChrome && !window.chrome) {
    console.log("Chrome headless detected");
}
```

### 权限（Permissions）*[有效]*

Headless Chrome不能正确的处理权限，会导致状态不一致，其中`notifation.permission`和`navigator.permissions.query`的值是矛盾的。

```js
navigator.permissions.query({name:'notifications'}).then(function(permissionStatus) {
    if(Notification.permission === 'denied' && permissionStatus.state === 'prompt') {
        console.log('This is Chrome headless')    
    } else {
        console.log('This is not Chrome headless')
    }
});
```

## 参考资料

- https://github.com/antoinevastel/fpscanner
- https://arh.antoinevastel.com/bots/areyouheadless
- https://antoinevastel.com/bot%20detection/2017/08/05/detect-chrome-headless.html
- https://antoinevastel.com/bot%20detection/2018/01/17/detect-chrome-headless-v2.html
