---
title: ReactNative开发-解决键盘遮挡问题
description: <center>用广东话来形容这问题就是：湿湿碎啦！碎料</center>
categories:
 - ReactNative
tags: 基础知识
updated: 2018-02-07 00:00:00
---

## 前言 

> 虽然ReactNative为`IOS`/`Android`平台提供了丰富且强大的兼容性组件给开发者们调用，但这毕竟是两套不同的系统。所以难免会会存在些无法兼容的问题或者给自己平台的特性所覆盖。弹出软键盘就是其中一个例子，在`Android`上，系统已经为我们处理好了键盘的弹出和收起，而且不会有遮挡文本框的情况。但在`IOS`上就有问题了，当文本框的位置低于软键盘的高度时，文本框就会被覆盖，而不会像`Android`那样顶在软键盘的上方。

## 未处理前的效果

- IOS

  ![QQ20190213-174228-HD.gif](https://upload-images.jianshu.io/upload_images/8154981-c8e4de69259938f6.gif?imageMogr2/auto-orient/strip)

- Android

![QQ20190213-175722-HD.gif](https://upload-images.jianshu.io/upload_images/8154981-d88fb22347d399ca.gif?imageMogr2/auto-orient/strip)