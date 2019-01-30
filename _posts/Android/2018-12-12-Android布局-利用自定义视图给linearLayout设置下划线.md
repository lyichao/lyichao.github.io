---
title: Android布局-利用自定义视图给linearLayout设置下划线
description: <center>实现起来挺简单的～！</center>
categories:
 - Android
tags: 布局
updated: 2018-12-12 15:24:12
---

## 概述

> 问题：想在`linearLayout`布局下给`tab`设置下划线，效果如下：

![](https://upload-images.jianshu.io/upload_images/8154981-33b5ecfc0166a36f.png)

## 分析&总结

> 解决思路：可以在`drawable`里新建`xml`，选择 `layer-list`，代码如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android" >
    <!-- 连框颜色值 -->
    <item>
        <shape>
            <solid android:color="#000000" />
        </shape>
    </item>
    <!-- 主体背景颜色值 -->
   <item android:bottom="1dp"> <!--设置只有底部有边框-->
        <shape>
            <solid android:color="#ffffff" />
        </shape>
    </item>
</layer-list>
```