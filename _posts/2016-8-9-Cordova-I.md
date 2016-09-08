---
layout: post
title: 科尔多瓦(Cordova)系列1 -- Cordova是什么?
permalink: /Cordova-I/
---
# 鸣谢：
本系列文章是我在阅读 John M. Wargo 所著的 [<<Apache Cordova 3 Programming>>](http://www.cordovaprogramming.com/) 期间，作为读书笔记记录下来的。感谢作者！  
PS：有条件的同学请购买正版图书参阅。

# Cordova是什么?
Cordova是一个开源框架，它为开发 **跨移动平台** 的应用提供了一种解决方案。基于该框架，开发者可以使用 **网页开发技术(JavaScript+HTML...)** 来开发能够在不同移动平台上使用的 **原生应用** 。  
此处我粗体了三个关键词：跨移动平台、网页开发技术和原生应用。 反正我看到这三个词马上就有了下面这两个问题：  
1. 怎么实现的 **“跨移动平台”** 呢？  
2. 用 **“网页开发技术”** 怎么能开发 **“原生应用”** ？  

别着急，我们先来看看这个框架的由来，也就是它的历史；然后再介绍它的构成，上述问题就自然清楚了。  
  
## Cordova的历史  
在2008年的iPhone开发训练营(iPhoneDevCamp)上，Nitobi公司启动了一个叫PhoneGap的项目。该项目的目标是创造出一种简单的开发跨平台应用的方法。  

项目伊始，一队开发人员只用了一个周末的时间，就搭建出了项目构架: 一个【原生应用容器】用来呈现Web内容以及它的核心功能。 关于【原生应用容器】后文还会解释。 起初只支持iPhone，但没过多久就增加了对Android及黑莓的支持。  
  
在2009年的Web 2.0博览会(Web 2.0 Expo)上，PhoneGap赢得了【用户选择奖（the People's Choice）】。值得一提的是：与会的极客们是通过手机短信的方式来投票的。  
  
随着时间的推移，PhoneGap增加了对新增硬件平台的支持，并且在API功能跨平台一致方面做了些工作。这期间，包括IBM在内的一些企业开始赞助这个项目。  
  
时间到了2011年的下旬。Nitobi声明将PhoneGap捐赠给Apache基金会。此后不久，Adobe宣布收购Nitobi；同时PhoneGap正式作为Apache的孵化项目开源。最初的名字叫：Apache Callback， 不久改名：DeviceReady， 最终版本到1.4的时候，正式命名：Apache Cordova。 Cordova这个名字正式Nitobi办公室所在街道的名字。  
  
2016年8月同时发布了Cordova Android 5.2.2 和 Cordova iOS 4.2.1。 

## Cordova应用架构  
我们来看一下Cordova最新的应用架构图：  
  
![Cordova应用架构](../images/cordovaapparchitecture.png)  
  
上图中的Cordova Application就是一个由Cordova Framework 构建出来的 **“原生应用”** 。它由三大组件构成：Web App、WebView以及Cordova插件。  

我们来分别看看这些组件的作用：  
*Web App: 包含你编写的应用代码（HTML5)的部分。其实就是一个可以包含CSS、JavaScript、图片等其他资源的网页文件，缺省为: index.html。     
*WebView: 其实就是一个内嵌的浏览器组件，用来解析运行Web App。 在某些平台下，该部分也可能同时包含浏览器组件及其他需要的原生应用所需组件。  
*Plugins: Plugins组件是Cordova生态系统中不可或缺的一部分。它是Cordova与原生组件交互的接口。  
  
当Web App中的代码需要使用原生应用才能使用的功能（如：摄像头、GPS等）时，这些请求就可以通过WebView发送给相应的Plugin，并由Plugin调用原生代码完成。  
  
现在来再来回顾一下本文开始处的两个问题：  

每个Cordova应用本身确实是原生应用。而其业务代码部分都包含在Web App组件中。换句话说，Web App中除了纯Html及JavaScript代码外只包含所需调用的抽象Cordova API，所以是平台无关的。而平台相关的其他两个组件，会在build时确认了所需支持的平台后，选择相应代码打包。   
  
## Cordova支持的平台  
  
*[Android (Google)](http://developer.android.com/index.html)  
*[bada (Samsung)](http://developer.bada.com)  
*[BlackBerry 10 (BlackBerry)](https://developer.blackberry.com/)  
*[iOS (Apple)](https://developer.apple.com/devcenter/ios/index.action)  
*[Firefox OS](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox_OS)  
*[Tizen (originally Samsung, now the Linux Foundation)](https://developer.tizen.org)  
*[Windows Phone 7 and Windows Phone 8 (Microsoft)](http://developer.windowsphone.com/en-us)  
*[Windows 8 (Microsoft)](http://msdn.microsoft.com)  
  
最新的平台支持信息请访问 [这](https://cordova.apache.org/docs/en/latest/guide/support/index.html)。  

值得一提的是：Cordova项目启动后，各大平台的开发商都陆续加入了支持该项目的行列。其中甚至包括：微软和Intel。然而，苹果公司却不包括在其中。。。  
  

----------
  
笔者会努力让本文不成为该系列的最后一篇：）也会尽快更新。
