---
title: ReactNative开发-监听设备返回键
description: <center>监听设备返回键</center>
categories:
 - ReactNative
tags: 基础知识
updated: 2019-01-25 17:30:08
---

## 监听设备返回键

### 前言

> 有时候我们需要在触发带返回键设备的时候做相应的操作（发api请求，第二次按下退出应用等），因此就需要用到ReactNative提供的`backhandler`方法。此外还有个`backandroid`方法，但并不建议使用。

### 基本用法

#### 导入`BackHandler`模块

```jsx
import {BackHandler} from 'react-native';
```

#### 在`componentDidMount`生命周期内添加设备监听方法

```jsx
//监听到设备触发返回键时，调用backForAndroid方法
BackHandler.addEventListener('hardwareBackPress', this.backForAndroid);

//在backForAndroid方法作出需要的操作
backForAndroid = () => {
    // 发api请求/第二次按下退出应用
}
```

