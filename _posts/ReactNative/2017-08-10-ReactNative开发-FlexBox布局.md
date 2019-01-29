---
title: ReactNative开发-FlexBox布局
description: <center>ReactNative的布局神器，掌握这个，布局随你摆弄！</center>
categories:
 - ReactNative
tags: 布局 基础知识
updated: 2017-08-10 00:00:00
---

### 前言

> 看过很多关于FlexBox布局的文章，但绝大部份都讲的多而乱，我初学的时候也是看的一脸懵逼。所以打算自己总结一篇关于FlexBox布局的文章，注重实用，而不是一大堆原理和废话。

### 什么是FlexBox布局？

> 从字面上可以理解成：能够很容易变化以适应外界条件变化的通用矩形容器，也就是我们常听到的弹性布局。

### 作用

> 用来解决父盒子和子盒子之间的约束关系，如下图所示：

![image.png](https://upload-images.jianshu.io/upload_images/8154981-b3b99d307f54bcc5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Flex容器的属性

> `flexDirection`、`justifyContent`、`alignItems`、`flexWrap`

> 掌握这4个属性，基本上就能实现布局千变万化，接下来讲解每一个的用法

#### 1.flexDirection

> 作用：决定容器的主轴方向。意思就是你想布局是往哪个方向走？是横还是竖？

> 属性值：`row`、`row-reverse`、`column`、`column-reverse`

![image.png](https://upload-images.jianshu.io/upload_images/8154981-33b91d20977ad8a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 如图解所示：
>
> `row`：主轴是水平方向，从左往右排列
>
> `row-reverse`：主轴是水平方向，从右往左排列
>
> `column`：主轴是垂直方向，从上往下排列
>
> `column-reverse`：主轴是垂直方向，从下往上排列
>
> 其中：`column`是作为默认的主轴属性。意思就是，如果不特意设置`flexDirection`,那么，该容器的主轴默认方向就是`flex-direction:column`

#### 2.justifyContent

> 作用：决定item在主轴上的对齐方向

> 属性值：`flex-start`、`flex-end`、`center`、`space-between`、`space-around`

![image.png](https://upload-images.jianshu.io/upload_images/8154981-c90f4f93e246f9c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 如图解所示：
>
> `flex-start`:左对齐
>
> `flex-end`：右对齐
>
> ``