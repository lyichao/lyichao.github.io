---
title: ReactNative开发-Redux学习记录
description: <center>学得怀疑人生系列之记录😳</center>
categories:
 - ReactNative
tags: 框架 进阶
updated: 2019-02-20 00:00:00
---

## 前言

> Redux这东西……真是令人又爱又恨。前前后后看了不下10篇文章+官方文档来理解这东西。差点就从入门到放弃。不过框架这东西，百变不离其中，懂得**分工合作**的思想后，什么MVC，MVP，MVVC都不在话下。借助这篇记录，来巩固下自己学习和使用Redux的过程。
>
> 本文章配套Demo：[点我下载](<https://codeload.github.com/lyichao/ReduxDemo/zip/master>)

## 什么是Redux?

> 简单地说：Redux是一个状态容器管理工具。将所有状态进行统一管理，适用于多交互，多数据的场景。反之，如果应用场景不是很复杂，像页面比较少，UI交互简单这些情况就没必要使用Redux了，用了反而增加复杂性。

## Redux状态管理流程

> ![image.png](https://upload-images.jianshu.io/upload_images/8154981-6bc3c8cf8909d6f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> - action是用户触发或程序触发的一个普通对象。
>
> - reducer是根据action操作来做出不同的数据响应，返回一个新的state。
> - store的最终值就是由reducer的值来确定的。
>
> 梳理一下流程就是：`action`->`reducer`->`新store`->`反馈到UI上更新页面`
>
> 



