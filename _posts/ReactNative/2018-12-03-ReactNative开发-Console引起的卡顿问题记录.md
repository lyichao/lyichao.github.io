---
title: ReactNative开发-Console引起的卡顿问题记录
description: <center>万万没想到引起卡顿的真凶居然是你！</center>
categories:
 - ReactNative
tags: 进阶 性能优化
updated: 2018-12-03 00:00:00
---

## 前言

> 在ReactNative开发中，经常用到了console语句来调试各种各样的问题。所以会经常在各种代码中穿插consloe语句。原本以为这些consloe语句只会存在与开发环境中，正式环境中会自动屏蔽。但残酷的事实教训告诉我并不是这样😱😱😱

## 为什么会出现卡顿现象？

> 首先，要知道

## 起因

> 在一个采用了`Redux`的项目中，因为有添加中间层`redux-logger`，项目中也有大量使用`console.log`语句。结果造成了在正式环境中APP存在卡顿严重的问题。后通过`Android Studio`的内存分析工具发现其中的js线程`mqt_js`占了绝大部分的cpu。进一步查询这个`mqt_js`得知，此线程是用于执行`JavaScript`代码的