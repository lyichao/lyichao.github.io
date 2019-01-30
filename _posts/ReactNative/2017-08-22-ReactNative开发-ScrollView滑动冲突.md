---
title: ReactNative开发-ScrollView滑动冲突
description: <center>看来滑动冲突这问题无论在什么平台上都是会有啊！</center>
categories:
 - ReactNative
tags: 基础知识
updated: 2019-01-25 17:30:08
---

## ScrollView滑动冲突

### 前言

> 在ReactNative中，`ScrollView` 必须有一个确定的高度才能正常工作,对于` ScrollView `来说，它就是将一些不确定高度的子组件装进确定高度的容器。
>
> `ScrollView `内部的其它响应者没办法阻止 `ScrollView `本身成为响应者（也就是说，`ScrollView` 响应的优先级比内部组件高，且内部组件没办法改变优先级

### 解决办法

> - 1.给` ScrollView` 中加 上`flex:1`。
> - 2.直接给该` ScrollView` 设置高度（不建议），因为它会根据内部组件自动延伸自己的尺寸到合适的大小。



