---
title: Activity启动模式
description: <center>这个也比较知识重要，记录！</center>
categories:
 - Android
tags: 基础知识
updated: 2019-10-13 17:02:13
---

# 目录



![img](http://lc-lf8Y5Iic.cn-n1.lcfile.com/26281a1cc4ffab5f5bca/Activity%E5%90%AF%E5%8A%A8%E6%A8%A1%E5%BC%8F1.png)



------

# 1. 定义

即Activity启动的方式

------

# 2. 启动模式的类别

Android启动提供了四种启动方式：

- 标准模式（`Standard`）
- 栈顶复用模式（`SingleTop`）
- 栈内复用模式（`SingleTask`）
- 单例模式（`SingleInstance`）

------

# 3. 知识储备

-  `Activity`的管理方式 = **任务栈** 
- 任务栈 采用的结构 = “后进先出” 的栈结构
- 每按一次Back键，就有一个`Activity`出栈



![img](http://lc-lf8Y5Iic.cn-n1.lcfile.com/590ca740f660323fa8c6/Activity%E5%90%AF%E5%8A%A8%E6%A8%A1%E5%BC%8F2.png)



------

# 4. 具体介绍

- 如下图



![img](http://lc-lf8Y5Iic.cn-n1.lcfile.com/6dd5b98e5f931f134ab9/Activity%E5%90%AF%E5%8A%A8%E6%A8%A1%E5%BC%8F3.png)



- 通俗讲解

  

  ![img](http://lc-lf8Y5Iic.cn-n1.lcfile.com/49a90d13ca61d7b1b69e/Activity%E5%90%AF%E5%8A%A8%E6%A8%A1%E5%BC%8F4.jpg)

  

------

# 5. 四种启动模式的区别



![img](<http://lc-lf8y5iic.cn-n1.lcfile.com/12a90283206a36231bc5/Activity%E5%90%AF%E5%8A%A8%E6%A8%A1%E5%BC%8F5.png>)



------

# 6. 启动模式的设置

启动模式有2种设置方式：在`AndroidMainifest`设置、通过`Intent`设置标志位

### 6.1 在AndroidMainifest设置

在`AndroidMainifest`的`Activity`配置进行设置

```cpp
<activity

android:launchMode="启动模式"
//属性
//standard：标准模式
//singleTop：栈顶复用模式
//singleTask：栈内复用模式
//singleInstance：单例模式
//如不设置，Activity的启动模式默认为**标准模式（standard）**
</activity>
```

### 6.2 通过`Intent`设置标志位

```java
Intent inten = new Intent (ActivityA.this,ActivityB.class);
intent,addFlags(Intent,FLAG_ACTIVITY_NEW_TASK);
startActivity(intent);
```

标记位属性

| 标记位属性                         | 含义                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| FLAG_ACTIVITY_SINGLE_TOP           | 指定启动模式为栈顶复用模式（`SingleTop`）                    |
| FLAG_ACTIVITY_NEW_TASK             | 指定启动模式为栈内复用模式（`SingleTask`）                   |
| FLAG_ACTIVITY_CLEAR_TOP            | 所有位于其上层的Activity都要移除，`SingleTask`模式默认具有此标记效果 |
| FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS | 具有该标记的Activity不会出现在历史Activity的列表中，即无法通过历史列表回到该Activity上 |

### 6.3 二者设置的区别

- 优先级不同
   `Intent`设置方式的优先级 > `Manifest`设置方式，即 以前者为准
- 限定范围不同
   `Manifest`设置方式无法设定 `FLAG_ACTIVITY_CLEAR_TOP`；`Intent`设置方式 无法设置单例模式（`SingleInstance`）

