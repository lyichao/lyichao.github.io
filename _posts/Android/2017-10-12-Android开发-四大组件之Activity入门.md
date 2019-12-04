---
title: Android开发-四大组件之Activity入门
description: <center>Android四大神兽之一啊~快来围观吃瓜</center>
categories:
 - Android
tags: 基础知识
updated: 2017-10-12 16:43:11
---

# 目录

![img](http://lc-lf8Y5Iic.cn-n1.lcfile.com/cbebd2ce56ba9dace043/%E5%9B%9B%E5%A4%A7%E7%BB%84%E4%BB%B6%E4%B9%8BActivity%E5%85%A5%E9%97%A81.png)

------

# 1. 生命周期流程 & 方法详解

### 1.1 具体请看下图

![img](<http://lc-lf8y5iic.cn-n1.lcfile.com/6163a28807d8f44b5265/%E5%9B%9B%E5%A4%A7%E7%BB%84%E4%BB%B6%E4%B9%8BActivity%E5%85%A5%E9%97%A82.png>)

### 1.2 注意点

#### a. 生命周期方法 = 成对出现（配对）

-  `onCreate()` & `onDestory()` 
-  `onStart()` & `onStop()` 
-  `onResume()`  & `onPause()` 

#### b. onStart() & onStop()、onResume()  & onPause() 除了回调时刻，在实际使用中无任何区别

-  `onStart()` & `onStop()` ：从 `Activity` 是否完全可见的角度 进行回调
-  `onResume()` & `onPause()`： 从 `Activity` 是否位于前台（UI最顶层）的角度进行回调；
- 除了上述的区别，在实际使用中没有任何区别

#### c. 当前Activity为A，此时用户打开ActivityB后，那么A的onPause（）和B的onResume()哪个方法先执行？

答：先 A的`onPause（）` ，再B的`onResume()`

-  `Activity`的启动过程：由`ActivityManagerService`（AMS）对栈内的`Activity`状态进行同步管理 & 规定：**新Activity启动前，栈顶的Activity必须先onPause（），才能启动新的Activity（执行onResume()）** 

> 注：为了让新的`Activity`尽快切换到前台，在 `onPause()`尽量不要做耗时 / 重量级操作

------

# 2. 常见场景的生命周期调用方式

![img](<http://lc-lf8y5iic.cn-n1.lcfile.com/ce0be4f936f6665f296a/%E5%9B%9B%E5%A4%A7%E7%BB%84%E4%BB%B6%E4%B9%8BActivity%E5%85%A5%E9%97%A83.png>)

------

# 3. 与Fragment生命周期对比

-  `Fragment`、`Activity`的生命周期非常相似

- 具体对比如下图：

  ![img](<http://lc-lf8y5iic.cn-n1.lcfile.com/718f25b827da30be3ff2/%E5%9B%9B%E5%A4%A7%E7%BB%84%E4%BB%B6%E4%B9%8BActivity%E5%85%A5%E9%97%A84.png>)

