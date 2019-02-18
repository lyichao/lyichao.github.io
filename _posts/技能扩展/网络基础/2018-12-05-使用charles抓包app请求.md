---
title: 使用charles抓包app请求
description: <center>抓包真的十分有趣</center>
categories:
 - 计算机技能
tags: 网络基础
updated: 2018-12-05 00:00:00
---

### 什么是抓包？

> 简单的说，就是对目标网络数据进行截获、重发、编辑、转存等。抓包常用作检测网络安全和获取、分析数据等场景。对于没用加密措施的应用，就很容易可以获取到当中的数据。

### 抓包工具：charles

#### charles能干什么？

> 可以让开发者监视查看所有连接互联网的HTTP通信，包括请求，响应和HTTP头信息等等

#### 使用方法

1.将Charles设为成代理服务器。打开Charles，点击`Proxy`选项卡，勾选`Mac OS X Proxy`

2.把手机连接到电脑同一个网络下，然后打开手机当前连接wifi的设置，设置手动代理，其中主机名填写当前电脑上的ip地址，端口号填写8888。

3.首次链接charles会弹出是否允许链接的弹窗，点击允许就好。

4.接下来charles上就能看到一切流过app的网络数据库啦。

![image.png](https://upload-images.jianshu.io/upload_images/8154981-eb234add58408ff1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)