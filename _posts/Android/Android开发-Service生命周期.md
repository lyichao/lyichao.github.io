---
title: Android开发-Service生命周期
description: <center>这个也比较知识重要，继续记录！</center>
categories:
 - Android
tags: 基础知识
updated: 2017-10-16 22:04:54
---

# 目录



![img](http://lc-lf8Y5Iic.cn-n1.lcfile.com/76bc7ef407c3ac5cca71/Service%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F1.png)



------

# 1. 生命周期 常用方法

- 官方说明图



![img](http://lc-lf8Y5Iic.cn-n1.lcfile.com/4c9d60cc0676bce14d70/Service%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F2.png)



在`Service`的生命周期里，常用的有：

- 4个手动调用的方法

| 手动调用方法    | 作用     |
| --------------- | -------- |
| startService()  | 启动服务 |
| stopService()   | 关闭服务 |
| bindService()   | 绑定服务 |
| unbindService() | 解绑服务 |

- 5个自动调用的方法

| 内部自动调用的方法 | 作用     |
| ------------------ | -------- |
| onCreat()          | 创建服务 |
| onStartCommand()   | 开始服务 |
| onDestroy()        | 销毁服务 |
| onBind()           | 绑定服务 |
| onUnbind()         | 解绑服务 |

------

# 2. 生命周期方法具体介绍

主要介绍内部调用方法 & 外部调用方法的关系。



![img](http://lc-lf8Y5Iic.cn-n1.lcfile.com/123ade688e5033a8c1cb/Service%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F3.png)



------

# 3. 常见的生命周期使用



![img](http://lc-lf8Y5Iic.cn-n1.lcfile.com/ec0084190f98d9b0a846/Service%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F4.png)

