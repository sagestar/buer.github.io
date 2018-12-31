---
layout: post
title: '白帽子讲Web安全05点击劫持(ClickJacking) & HTML5安全'
date: 2018-12-31
author: Buer
color: rgb(255,210,32)
cover: 'http://qiniu.bueryo.com/18-12-31/27151483.jpg'
tags: 白帽子 Web安全 笔记 点击劫持 ClickJacking HTML5安全
---

# 点击劫持(ClickJacking)
## 什么是点击劫持

点击劫持是一种视觉上的欺骗手段。攻击者使用一个透明的，不可见的iframe，覆盖在一个网页上，然后诱使用户在网页上进行操作，此时用户在不知情的情况下点击透明的iframe页面。通过调整iframe页面的位置，可以诱使用户恰好点击在iframe里的一些功能性按钮上。

## Flash点击劫持

Flash本身有很多交互的情景，然后隐藏一个看不见的iframe……

## 图片覆盖攻击

Cross Site Image Overlaying攻击，简称XSIO。通过调整图片的style可以是图片覆盖在指定的任意位置。  
XSIO不同于XSS，它利用的是style，或者能够控制CSS。

## 拖拽劫持与数据窃取

很多浏览器都开始支持拖拽，拖拽对象可以是链接、文字、还可以从一个窗口拖拽到另一个窗口，因此拖拽是不受同源策略限制的。  
"拖拽劫持"的思路是诱使用户从隐藏的不可见iframe中"拖拽"出攻击者希望得到的数据，然后放到攻击者能控制的另外一个页面中，从而窃取数据。  
在JavaScript或者Java API的支持下，这个攻击过程会变得非常隐蔽。因为它突破了传统ClickJacking一些先天的局限，所以这种新型的"拖拽劫持"能够造成更大的破坏。  

## ClickJacking 3.0： 触屏劫持

智能手机上的"触屏劫持"攻击被称为TapJacking。

依然是不可见的iframe。

可以依靠手机浏览器地址栏自动隐藏的机制，画一个假的地址栏，等等。

## 防御ClickJacking

### frame busting

frame busting，通过js代码，禁止iframe嵌套。  
缺点，可以通过嵌套多个iframe绕过。

### X-Frame-Options

frame busting存在被绕过的可能，一个比较好的解决方案是使用一个HTTP头——X-Frame-Options。  

它有三个可选的值：

- DENY

- SAMEORIGIN

- ALLOW-FROM origin

 

当值为DENY时，浏览器会拒绝当前页面加载任何frame页面；若值为SAMEORIGIN，则frame页面的地址只能为同源域名下的页面；若值为ALLOW-FROM，则可以定义允许frame加载的页面地址。

除了X-Frame-Options之外，Firefox的"Content Security Policy"以及Firefox的NoScript扩展也能够有效防御ClickJacking，这些方案为我们提供了更多的选择。


# HTML5安全
## HTML5新标签
### 新标签的XSS

HTML5定义了新的标签、新的事件，这就有可能带来新的XSS攻击。  
所以黑白名单需要时常更新。

### iframe的sandbox
iframe的sandbox属性，就是html5安全中很重要的组成部分部分。于此同时还带来了一个新的mime类型，text-html/sandboxed。

在html5页面中，可以使用iframe的sandbox属性，比如：&lt;iframesrc="http://alibaba.com" sandbox&gt;,sandbox后面如果不加任何值，就代表采用默认的安全策略，即：iframe的页面将会被当做一个独自的源，同时不能提交表单，以及执行javascript脚本，也不能让包含iframe的父页面导航到其他地方，所有的插件，如flash,applet等也全部不能起作用。简单说iframe就只剩下一个展示的功能，正如他的名字一样，所有的内容都被放入了一个单独的沙盒。

sandbox属性可以通过参数来支持更加精确的控制，有以下几个值可选择：

- allow-same-original：允许同源访问
- allow-top-navigation：允许访问顶层窗口
- allow-forms：允许提交表单；
- allow-script：允许执行脚本。

### Link Types：noreferrer
HTML5中为&lt;a&gt;标签和&lt;area&gt;标签定义了一个新的Link Types：norefeerrer。  
顾名思义，指定了noreferrer后，浏览器在请求该地址时，不用在发送Referer。

### Canvas的妙用
&lt;canvas&gt;这个 HTML 元素是为了客户端矢量图形而设计的。它自己没有行为，但却把一个绘图 API展现给客户端JavaScript 以使脚本能够把想绘制的东西都绘制到一块画布上。  
可以使用&lt;canvas&gt;在线破解验证码。  

## 其他安全问题
### Cross-Origin Resource Sharing

合法的跨域技术，通配符“*”的使用是极其危险的。

### postMessage——跨窗口传递消息

在HTML5中新增了postMessage方法，postMessage可以实现跨文档消息传输（Cross Document Messaging），Internet Explorer 8, Firefox 3,Opera 9, Chrome 3和 Safari 4都支持postMessage。

该方法可以通过绑定window的message事件来监听发送跨文档消息传输内容。

postMessage允许每一个window（包括当前窗口，当初窗口，inframe等）对象往其他窗口发送文本消息，从而实现跨窗口的消息传递。这个功能不受同源策略的限制。

使用postMessage时，有两个问题需要注意：

- 在必要是，可以接收窗口验证Domain，设置验证url，以防止来之非法页面的消息。这实际上是在代码中实现一次同源策略的验证过程。

- 根据“secure By Default”原则，在接收窗口不应该新人接收的消息，需要对消息进行安全检测。

### Web Storage

Web Storage实际上由两部分组成：sessionStorage与localStorage。 sessionStorage用于本地存储一个会话（session）中的数据，这些数据只有在同一个会话中的页面才能访问并且当会话结束后数据也随之销毁。因此sessionStorage不是一种持久化的本地存储，仅仅是会话级别的存储。 localStorage用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。

Web Stroage让Web开发更加灵活多变，它的强大功能也为XSS Payload大开方便之门，攻击者可以将恶意代码保存在Web Stroage中，实现跨页面攻击。
