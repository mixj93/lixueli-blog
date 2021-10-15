---
title: Web机器人入门
desc: 
date: 2021-10-15 19:35:55
tags:
  - web
  - 风控
---

本文介绍了Web机器人（Web bot）是什么？是做什么的？有哪些类型？

<!--more-->

## 什么是Web机器人？

### 定义

能够在网络运行自动化任务的程序。

### 作用

提高吞吐量，降低成本。

### 使用场景

* 自动化测试
* 网络欺诈
* 证书填充（credential stuffing）
* 爬虫

## Web机器人分类

可以通过使用的技术，将Web机器人分为三类：

* 使用HTTP框架
* 使用真正的浏览器
* 使用无头浏览器

### 简单的HTTP请求库

如使用Python的`urlllib`或是Node.js的`http`模块。

优点：占用CPU和内存少。

缺点：无法加载动态内容，或单页面应用（SPA），不能执行JavaScript代码，通常需要借助别的库来解析HTML，如：[Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/)、[Cheerio](https://cheerio.js.org/)。

### 使用了Selenium/puppeter的真正的浏览器

Selenium 和 Puppeteer 是浏览器自动化框架。它们能够运行自动化程序，例如加载页面、移动鼠标或输入文本等。

Puppeteer主要通过Node.js来控制Chrome和Headless Chrome。

Selenium可以控制常见的浏览器，如Firefox、 Chrome和Safari，并且能够使用不同的编程语言(Node.js、Python和Java)。

优点：能够执行JavaScript，并且使用真正的浏览器。

缺点：占用较多的CPU和内存，不能直接在服务器上使用，因为通常需要显示器。

### 无头（Headless）浏览器

无头浏览器是没有图形界面的浏览器。最早流行的无头浏览器之一是PhantomJS（已[不再维护](https://github.com/ariya/phantomjs/issues/15344)，最后一个稳定版本是2016年发布的）。

2017年谷歌发布了Headless Chrome，几乎支持所有普通Chrome的功能。

优点：能执行JavaScript，并且占用较少的CPU和内存资源。

## 识别Web机器人

### 行为检测（Behavioral detection）

该方法通过检测用户的行为，如鼠标移动、浏览速度等来检测是不是真人。

#### 服务端特征

* Session期间所浏览的页面数
* 总请求数
* 浏览页面的顺序，机器人往往有某种模式，而人类会更加混乱。
* 浏览两个连续页面的平均时间。
* 页面加载的资源类型，机器人可能会因为节省带宽而屏蔽CSS、图片、广告等。

#### 客户端特征

* 鼠标移动轨迹
* 鼠标点击
* 滚动
* 按键，连续按键的时间间隔

### 基于指纹识别的检测（Fingerprinting-based detection）

指纹识别通过设备或浏览器相关的信息进行检测。既可以进行身份验证，也可以检测机器人。

该方法的思路：收集设备、操作系统、浏览器信息；然后对手机代码进行混淆和加密，否则会被机器人开发人员发现检测原理。

#### 浏览器指纹识别（Browser fingerprinting）

* PhantomJS
  * `window.callPhantom`
  * `window._phantom`
  * `window.phantom`
* Nightmare
  * `window.__nightmare`
* webdriver
  * `navigator.webdriver`
* Selenium
  * `document.__selenium_unwrapped`
  * `document.__webdriver_evaluate`
  * `document.__driver_evaluate`

**浏览器一致性（Browser consistency）**

用来验证UA是否造假

```js
eval.toString().length
// Chrome返回33
// Firefox、Safari返回37
// IE返回39
```

**操作系统一致性（OS consistency）**

UA和navigator.platform的是否值一致

* Linux -> Linux i686, Linux x86\_64
* Windows -> Win32, Win64
* iOS -> iPhone, iPad
* Android -> Linux armv71, Linux i686
* macOS -> MacIntel
* FreeBSD -> FreeBSD amd64, FreeBSD i386

**不一致行为（Inconsistent feature behavior）**

通过权限检测Headless Chrome

```js
navigator.permissions.query({name:'notifications'}).then(function(permissionStatus) {
  if(Notification.permission === 'denied' && permissionStatus.state === 'prompt') {
    console.log('This is Chrome headless')
  } else {
    console.log('This is not Chrome headless')
  }
});
```

**Red pills**

Red pills allow programs to detect if their execution environment is a CPU emulator or a virtual machine.

### CAPTCHAs

Completely Automated Public Turing test to tell Computers and Humans Apart

CAPTCHAs 利用图灵测试，比如图像和音频识别来区分机器人和人类。

## 参考资料

* [Getting Started with Headless Chrome](https://developers.google.com/web/updates/2017/04/headless-chrome)
* [Bot detection 101: Categories of web bots](https://antoinevastel.com/crawler/2019/12/29/families-web-bots.html)
* [Bot detection 101: How to detect web bots?](https://antoinevastel.com/javascript/2020/02/09/detecting-web-bots.html)
* [Tick Tock: Building Browser Red Pills from Timing Side Channels](https://www.usenix.org/system/files/conference/woot14/woot14-ho.pdf)
* [CAPTCHA Wikipedia](https://en.wikipedia.org/wiki/CAPTCHA)