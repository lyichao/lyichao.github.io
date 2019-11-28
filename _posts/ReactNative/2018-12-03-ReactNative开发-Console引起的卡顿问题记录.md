---
title: ReactNative开发-Console引起的卡顿问题记录
description: <center>万万没想到引起卡顿的真凶居然是你！</center>
categories:
 - ReactNative
tags: 进阶 性能优化
updated: 2018-12-03 00:00:00
---

## 前言

> 在ReactNative开发中，经常用到了`console`语句来调试各种各样的问题。所以会经常在各种代码中穿插`console`语句。原本以为这些`console`语句只会存在与开发环境中，正式环境中会自动屏蔽。但残酷的事实教训告诉我并不是这样😱😱😱
>
> 

## 为什么会出现卡顿现象？

> ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/3601df74ab742f7ef5ff/Console%E5%BC%95%E8%B5%B7%E7%9A%84%E5%8D%A1%E9%A1%BF%E9%97%AE%E9%A2%98%E8%AE%B0%E5%BD%951.png>)
>
> 首先，要知道。动画或电影一类视频之所以看起来会是连贯的，是因为我们人的眼睛具有视觉残留的特性，当光对视网膜所产生的视觉在光停止作用后，仍保留一段时间的现象。其停留间隔大约的二十四分之一秒。因为当一张静态的图片稳定快速变化时，人眼看起来就是连贯的。当图片翻动变化的速度越快，人眼看起来就会越流畅，当中翻动的每一张图片我们称之为“帧”。
>
> 同理，在APP中造成卡顿的现象，就是因为渲染UI的时候不够快（超过16毫秒），而造成这种情况很有可能是因为在渲染UI的时候有大量的耗时操作。

## 发现问题

> 公司内部测试一个新项目的APP时反馈说APP非常卡顿，基本上是一点一个卡。通过`Android Studio`的内存分析工具发现其中的js线程`mqt_js`占了绝大部分的cpu。进一步查询这个`mqt_js`得知，这个是用于执行`JavaScript`代码的线程。因此可以得知引起卡顿的真凶是在`JavaScript`代码部分。在查阅了相关`ReactNative`性能问题后，发现了`console.log`语句会极大的拖累`JavaScript`线程。因为项目有用到了`Redux`，而且有添加中间层`redux-logger`，项目中也有大量使用`console.log`语句。于是最后我们把问题定位在了大量使用`console.log`语句上。

## 解决问题

> 开发模式下大量使用`console.log`语句是不可避免的，因为需要通过`console.log`语句来获取各种信息。但这在正式环境下就没必要了。因此，我们可以利用好ReactNative提供的`_DEV_`变量，用来指示当前环境时候属于开发环境，如果不属于，那我们就可以把所有`console`语句都替换掉。
>
> ```jsx
> if (!__DEV__) {
>   global.console = {
>     info: () => {},
>     log: () => {},
>     warn: () => {},
>     debug: () => {},
>     error: () => {}
>   };
> }
> ```