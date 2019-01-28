---
title: ReactNative基础知识-打包APk
description: <center>看了就会的傻瓜式操作</center>
categories:
 - ReactNative
tags: 
updated: 2019-01-25 17:30:08
---
## 打包APk

### windows平台

#### 生成一个签名密钥

> 用`keytool`命令生成一个私有密钥。在 Windows 上`keytool`命令放在 JDK 的 bin 目录中（比如`C:\Program Files\Java\jdkx.x.x_x\bin`），你可能需要在命令行中先进入那个目录才能执行此命令。

> 这条命令会要求你输入密钥库（keystore）和对应密钥的密码，然后设置一些发行相关的信息。最后它会生成一个叫做`my-release-key.keystore`的密钥库文件。
>
> 在运行上面这条语句之后，密钥库里应该已经生成了一个单独的密钥，有效期为 10000 天。--alias 参数后面的别名是你将来为应用签名时所需要用到的，所以记得记录这个别名。
>
> **注意：请记得妥善地保管好你的密钥库文件，一般不要上传到版本库或者其它的地方。**

