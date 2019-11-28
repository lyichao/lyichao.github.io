---
title: ReactNative开发-解决IOS键盘遮挡问题
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

  ![QQ20190213-174228-HD.gif](<http://lc-lf8y5iic.cn-n1.lcfile.com/8b85aeba5d1dc212553c/%E8%A7%A3%E5%86%B3IOS%E9%94%AE%E7%9B%98%E9%81%AE%E6%8C%A1%E9%97%AE%E9%A2%981.gif>)

- Android

![QQ20190213-175722-HD.gif](<http://lc-lf8y5iic.cn-n1.lcfile.com/523487069596d310f273/%E8%A7%A3%E5%86%B3IOS%E9%94%AE%E7%9B%98%E9%81%AE%E6%8C%A1%E9%97%AE%E9%A2%982.gif>)

## IOS实现自动管理键盘弹/收问题

> 解决办法很简单，直接将此文件夹拖到项目的`IOS目录`下即可实现IOS自动管理键盘遮挡文本框的问题。
>
> - **[下载文件](<https://codeload.github.com/lyichao/IQKeyboardManager/zip/master>)**，解压，然后解压后得到的`IQKeyboardManager`文件夹
> - ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/7fbfe21b963642a66aa0/%E8%A7%A3%E5%86%B3IOS%E9%94%AE%E7%9B%98%E9%81%AE%E6%8C%A1%E9%97%AE%E9%A2%984.png>)
> - ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/991eac8f4d76bd058699/%E8%A7%A3%E5%86%B3IOS%E9%94%AE%E7%9B%98%E9%81%AE%E6%8C%A1%E9%97%AE%E9%A2%985.png>)
> - 添加后会在左侧的导航栏看到已添加的`IQKeyboardManager`文件夹。![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/be96ec1c27c47d735c6a/%E8%A7%A3%E5%86%B3IOS%E9%94%AE%E7%9B%98%E9%81%AE%E6%8C%A1%E9%97%AE%E9%A2%986.png>)
> - 最后重新运行项目即可！

## 处理后的效果

![QQ20190213-173849-HD.gif](<http://lc-lf8y5iic.cn-n1.lcfile.com/0f75fc3d1137d9d10343/%E8%A7%A3%E5%86%B3IOS%E9%94%AE%E7%9B%98%E9%81%AE%E6%8C%A1%E9%97%AE%E9%A2%983.gif>)