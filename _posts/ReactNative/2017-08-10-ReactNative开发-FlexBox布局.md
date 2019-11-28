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

![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/413851bd37f44005729a/FlexBox%E5%B8%83%E5%B1%801.png>)

> 这里的父盒子和子盒子是相对来说的，任何一个盒子都可能成为父/子盒子，这要看此时该盒子是属于外层空间还是里层空间

### Flex容器的属性

> `flexDirection`、`justifyContent`、`alignItems`、`flexWrap`  
>
> 掌握这4个属性，基本上就能实现布局千变万化，接下来讲解每一个的用法

#### 1.flexDirection

> 作用：决定盒子的主轴方向。意思就是你想布局是往哪个方向走？是横还是竖？  
>
> 属性值：`row`、`row-reverse`、`column`、`column-reverse`

![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/0b6769d19ea31f371ca2/FlexBox%E5%B8%83%E5%B1%802.png>)

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
> 其中：`column`是作为默认的主轴属性。意思就是，如果不特意设置`flexDirection`,那么，该盒子的主轴默认方向就是`flex-direction:column`

#### 2.justifyContent

> 作用：决定盒子在主轴（水平）的对齐方向  
>
> 属性值：`flex-start`、`flex-end`、`center`、`space-between`、`space-around`

![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/c09e8d4d493d8ffe5f54/FlexBox%E5%B8%83%E5%B1%807.png>)

> 如图解所示：
>
> `flex-start`:左对齐
>
> `flex-end`：右对齐
>
> `center`:居中
>
> `space-between`:两端对齐，盒子之间的间隔都相等
>
> `space-around`:每个盒子两侧的间隔相等。盒子之间的间隔比盒子与边框的间隔大一倍
>
> 其中：`flex-start`是作为默认的主轴对齐方向。意思就是，如果不特意设置`flex-start`,那么，盒子的主轴对齐方向就是`flex-start`

#### 3.alignItems

> 作用：决定盒子在交叉轴（垂直）的对齐方向  
>
> 属性值：`flex-start`、`flex-end`、`center`、`baseline`、`stretch`

![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/03a2e0595b3e3c99298f/FlexBox%E5%B8%83%E5%B1%804.png>)

> 如图解所示：
>
> `flex-start`：交叉轴的起点对齐  
>
> `flex-end`：交叉轴的终点对齐  
>
> `center`：交叉轴的中点对齐
>
> `stretch`：如果项目未设置高度或设为auto，将占满整个容器的高度
>
> `baseline`：项目的第一行文字的基线对齐

#### 4.flex-wrap

> 作用：决定盒子是否排列在一条轴线上。说白了，就是需不需要换行显示。  
>
> 属性值：nowrap、 wrap、wrap-reverse

![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/8d6b765035db03453d25/FlexBox%E5%B8%83%E5%B1%805.png>)

> 如图解所示：  
>
> `nowrap`：不换行，如果父盒子有长度限制，那么多出的部分将会看不见。
>
> `wrap`：换行，第一行在上面。如果父盒子有长度限制，那么剩余空间不够的情况下将会换行显示。
>
> `wrap-reverse`：换行，第一行在下面。如果父盒子有长度限制，那么剩余空间不够的情况下将会换行显示。

### 总结

> 4个属性，`flexDirection`控制主轴方向，`justifyContent`和`alignItems`负责控制水平和垂直对齐方向，`flexWrap`就控制是否换行。就这么简单！
>
> 在我个人实际开发中，可以利用好ReactNative的开发者模式中的Hot Reloading，开启这个可以实时看到UI布局的变化。再配合borderWidth这一属性，为每个UI元素添加边框，这样就能很灵活的调动UI布局。下面是我自己写的布局例子：
>
> ```jsx
> import React, { Component } from 'react';
> import {
> AppRegistry,
> StyleSheet,
> Text,
> View, Dimensions
> } from 'react-native';
> const screenWidth = Dimensions.get('window').width;  //屏幕的宽度
> const screenHeight = Dimensions.get('window').height;  //屏幕的高度
> 
> export default class Demo extends Component {
> render() {
>  return (
>      <View style={styles.firstContainer}>
>          <View style={styles.secondContainer}>
>              <View style={styles.textContainer}>
>                  <Text style={styles.welcome}>
>                      Welcome to React Native!
>                  </Text>
>              </View>
>              <View style={styles.textContainer}>
>                  <Text style={styles.instructions}>
>                      To get started, edit index.android.js
>                  </Text>
>              </View>
>              <View style={styles.textContainer}>
>                  <Text style={styles.instructions}>
>                      Double tap R on your keyboard to reload,{'\n'}
>                      Shake or press menu button for dev menu
>                  </Text>
>              </View>
>          </View>
>      </View>
>  );
> }
> }
> 
> const styles = StyleSheet.create({
> firstContainer: {
>    flexDirection: 'column',height:screenHeight,
>    width:screenWidth, justifyContent: 'center',
>    alignItems: 'center'
> },
> secondContainer: {
>    flexDirection:'column',height:0.3*screenHeight,
>    width:0.8*screenWidth,borderWidth:1,
>    justifyContent: 'center',alignItems: 'center'
> },
> textContainer: {
>    flexDirection:'row',borderWidth:1,
>    justifyContent: 'center',alignItems: 'center'
> },
> welcome:{
>    fontSize: 20,margin: 10
> }
>  instructions:{
>    marginBottom: 5
> }
> });
> 
> AppRegistry.registerComponent('Demo', () => Demo);
> ```
>

![1548828942423.gif](<http://lc-lf8y5iic.cn-n1.lcfile.com/2ab221025d0d0c44879b/FlexBox%E5%B8%83%E5%B1%806.gif>)

