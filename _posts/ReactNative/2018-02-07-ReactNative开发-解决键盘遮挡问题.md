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

## IOS实现自动管理键盘弹/收问题

> 解决办法很简单，直接将此文件夹拖到项目的`IOS目录`下即可实现IOS自动管理键盘遮挡文本框的问题。
>
> - 下载文件，解压，然后解压后得到的`IQKeyboardManager`文件夹
> - ![image.png](https://upload-images.jianshu.io/upload_images/8154981-06f2d43fc342bf27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> - ![image.png](https://upload-images.jianshu.io/upload_images/8154981-79f0d51a53c06b51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> - 添加后会在左侧的导航栏看到已添加的`IQKeyboardManager`文件夹。![image.png](https://upload-images.jianshu.io/upload_images/8154981-02b418bdd700c62e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> - 最后重新运行项目即可！

## 处理后的效果

![QQ20190213-173849-HD.gif](https://upload-images.jianshu.io/upload_images/8154981-c936d2fe2e7dcb2a.gif?imageMogr2/auto-orient/strip)