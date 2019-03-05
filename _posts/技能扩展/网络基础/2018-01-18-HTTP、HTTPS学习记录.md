---
title: HTTP、HTTPS学习记录
description: <center>这种记录千千万，我就问你看不看！</center>
categories:
 - 技能扩展
tags: 网络基础
updated: 2018-01-18 00:00:00
---

## 前言

> 作为一个合格的开发人员，`HTTP`、`HTTPS`这种网络知识都应该掌握。不管做的事前台还是后台。本着这样的理念，故写一篇文章来做记录。

## 什么是HTTP协议？

![image.png](https://upload-images.jianshu.io/upload_images/8154981-273a80790d6d9587.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 工作方式

> 请求/响应工作方式，如下图所示：

![image.png](https://upload-images.jianshu.io/upload_images/8154981-5d4e0fa71f5f2233.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 报文结构

> `HTTP`在应用层交互数据的方式叫做报文，而报文分为请求报文和响应报文。

#### 请求报文 

> 请求报文由请求行、请求头和请求体组成。具体报文结构如下所示：

![image.png](https://upload-images.jianshu.io/upload_images/8154981-f7d4bb51b5908a35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

请求报文例子：

```http
POST /user HTTP/1.1      //请求行
Host: www.user.com
Content-Type: application/x-www-form-urlencoded
Connection: Keep-Alive
User-agent: Mozilla/5.0.      //以上是请求头部
（此处必须有一空行）  //空行分割请求头部和请求数据 
name=world   //请求数据
```

> 接下来我们详细说明下请求报文的每个部分的作用及使用方式

- **请求行（request ling）**

  - 作用：声明请求方法、主机域名、资源路径和协议版本；

  - 组成介绍：

    ![image.png](https://upload-images.jianshu.io/upload_images/8154981-8a285dbb4a1f98e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    <mark>此处特意说明GET、POST方法的区别：</mark>

    ![image.png](https://upload-images.jianshu.io/upload_images/8154981-9478233c74902828.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **请求头（header）**

  - 作用：声明客户端、服务器/报文的部分信息；
  - 使用方式：采用`header`（字段名）：`value`(值)的方式；
  - 请求/响应报文通用`header`

  ![image.png](https://upload-images.jianshu.io/upload_images/8154981-8cd5a33713e1b751.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  - 下面是请求头常用的`header`

  ![image.png](https://upload-images.jianshu.io/upload_images/8154981-91d4f0c982169116.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **请求数据**

  - 作用：存放需要发送的数据信息；

  - 使用方式：

    ![image.png](https://upload-images.jianshu.io/upload_images/8154981-d3a615ed9a49312b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 响应报文

> `HTTP`的响应报文包括了状态行、响应头和响应体。具体响应报文结构如下图所示：

![image.png](https://upload-images.jianshu.io/upload_images/8154981-5860f4718b20ce6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

响应报文例子：

```http
HTTP/1.1 304 Not Modified
Date：Mon, 15 Jan 2018 15:39:29
//空行                                     
//空响应体
```

> 接下来我们吧请求报文和响应报文放一起看下：

![image.png](https://upload-images.jianshu.io/upload_images/8154981-980e427008afc600.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 如图所示，两者在头部和数据部分都类似，其中区别最大的就是请求行和状态行。
>
> 接下来我们详细说明下响应报文的每个部分的作用及使用方式：

- **状态行**

  - 作用：声明协议版本、状态码和状态码的描述信息；

  - 具体介绍如下图所示：

    ![image.png](https://upload-images.jianshu.io/upload_images/8154981-f30283511536760e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **响应头**

  - 作用：申明客户端报文和服务器报文的部分信息；

  - 使用方式：采用`header`（字段名）：`value`(值)的方式；

  - 请求/响应报文通用`header`：这个部分不再贴图，详情可以看请求头部分；

  - 下面是响应头常用的`header`：

    ![image.png](https://upload-images.jianshu.io/upload_images/8154981-91d4f0c982169116.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **响应数据**

  - 作用：存放需要返回客户端的数据信息；
  - 使用方式：和请求数据是一致的。请参考请求数据部分。

  