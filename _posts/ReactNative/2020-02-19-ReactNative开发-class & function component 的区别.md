---
title: ReactNative开发-class & function component 的区别
description:<center>知识get！</center>
categories:
 - ReactNative
tags: 基础知识
updated: 2020-2-19 00:00:00
---

<mark>注意：使用class关键字创建的组件，有自己的私有数据（this.state）和生命周期函数；</mark>

<mark>注意：使用function创建的组件，只有props,没有自己的私有数据和生命周期函数；</mark>

1.用构造函数创建出来的组件：叫做无状态组件【无状态组件用的不多】

2.用class关键字创建出来的组件叫做有状态组件【用的最多】

3.什么情况下使用有状态组件？什么情况下用无状态组件？

- 如果一个组件需要有自己的私有数据，则推荐使用 ：class创建的有状态组件；
- 如果一个组件不需要私有的数据，则推荐使用，无状态组件；
- React 官方说：无状态组件，由于没有自己的state和生命周期函数，所以运行效率会比又状态组件稍微高一些；

有状态组件和无状态组件的本质区别就是：有无state属性和有无生命周期函数

4。组件中的props和state/data之间的区别

- props中的数据都是外界传递过来的；
- state/data中的数据，都是组件私有的；（通过ajax获取回来的数据一般都为私有数据）
- props中的数据都是只读的，不能重新赋值；
- state/data中的数据，都是可读可写的；