---
title: ReactNative开发-Redux学习记录
description: <center>学得怀疑人生系列之记录😳</center>
categories:
 - ReactNative
tags: 框架 进阶
updated: 2019-02-20 00:00:00
---

## 前言

> `Redux`这东西……真是令人又爱又恨。前前后后看了不下10篇文章+官方文档来理解这东西。差点就从入门到放弃。不过框架这东西，百变不离其中，懂得**分工合作**的思想后，什么`MVC`，`MVP`，`MVVC`都不在话下。借助这篇记录，来巩固下自己学习和使用`Redux`的过程。
>
> **本文章配套Demo**：[点我下载](<https://codeload.github.com/lyichao/ReduxDemo/zip/master>)

## 什么是Redux?

> - **官方说法**：`Redux`是一个状态容器管理工具。将所有状态进行统一管理，适用于多交互，多数据的场景。
>
> - **我的理解**：我们可以把采用了`Redux`后的应用看作是一个`component`，整个应用只有一个`state`,所有页面都可以拿到这个`state`中的内容。当这个`state`中的内容发生改变的时候，与之关联的页面也会跟着刷新状态。

## 基本概念

> 首先，在未使用Redux的项目中，组件（父组件、子组件、孙子组件）间都各自含有的`state`和`props`属性。而且`props`是从父级组件上**分发下来**的属性，只能是从上往下走；而`state`是组件**内部**自行管理的状态属性。因此整个应用并没有数据向上回溯的能力。要么从上面单向得到并向下级分发，要么自行内部消化刷新页面状态。正如下图所示：![image.png](https://upload-images.jianshu.io/upload_images/8154981-fdf3c02ac9c6db0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> 因此，一但应用场景的页面较多，交互复杂的时候，每次都去通过`state`重新刷新页面引起页面变化，就会出现卡卡卡卡卡的情况。
>
> 那么，采用了`Redux`后又会怎样呢？先来看一下App结构图：
>
> ![image.png](https://upload-images.jianshu.io/upload_images/8154981-f6db3205e483de42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> 最后，从上图可以看到采用Redux后的App结构比没采用Redux时在最外包裹了一层`Provider`。接下来我们就从这个`Prodiver`开始，逐个解析`Store`、`Action`和`Reducer`。
>
> - **Provider**:**整个App的容器，位于App的最外层。能够使得原有组件变得接受Redux的`store`作为props。可以理解为所有的页面都是它的子组件，这样就能够像父组件向子组件传递props。**
>
>   例如想通过改变孙子组件3的同时刷新孙子组件2的页面状态，这时后就可以先把孙子组件3要改变的状态通知告诉`Provider`，然后再由`Provider`刷新孙子组件2的页面状态。这样就能使得整个App内的组件都拥有数据往上回溯的能力。
>
> - **Store**：**存放和管理App内所有组件State的地方。**
>
>   整个App只允许有一个`Store`对象，改变了`Store`中的state就可以实现我们改变UI的操作。
>
> -  **Action**:**用户触发或程序触发的一个普通对象。**
>
>   State 的变化，会导致View的变化。但是，用户接触不到State，只能接触到View。所以，State的变化必须是View导致的。Action就是View 发出的通知，表示State应该要发生变化了。例如要实现登录操作，就要发起登录action，当登录成功的就返回View显示登录成功的状态和信息，登录失败就返回View显示登录失败的状态和信息。
>
> - **Reducer**:**根据`action`操作来做出不同的数据响应，返回一个新的`state`。**

## Redux状态管理流程

> ![image.png](https://upload-images.jianshu.io/upload_images/8154981-67c5bdc3f12e01f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> <mark>梳理一下流程就是：用户触发action→部署触发的action后通知reducer→reducer→新store→反馈到UI上更新页面</mark>

## 实例：App登录操作

> 说了一大堆原理和废话后，还是得看实例。不然脑袋记不住。

### 实例需求

> 我们要做一个App登录操作，能让用户输入账号密码登录。登录成功就弹出提醒登录成功，跳转到主页并显示出当前登录用户的信息。登录失败就弹出登录失败提醒。如果用户不登录也可以选择跳过登录，直接跳转到主页并弹出欢迎游客信息。

### 需求分析

> 通过需求，我们提炼出其中重要的点：
>
> - **登录成功状态**
> - **登录失败状态**
> - **跳过登录**

### 具体操作

#### 步骤一

> 创建一个名为ReduxDemo的空项目。
>
> 打开终端，输入
>
> ```
> react-native init ReduxDemo --version 0.48.1
> ```

#### 步骤二

> 继续在终端输入一下命令，安装redux、react-navigation（用作跳转页面）和redux-thunk（稍后讲解）
>
> ```
> 
> ```