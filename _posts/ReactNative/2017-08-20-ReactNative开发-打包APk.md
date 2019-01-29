---
title: ReactNative开发-打包apk
description: <center>傻瓜式操作打包apk</center>
categories:
 - ReactNative
tags: 基础知识
updated: 2019-01-25 17:30:08
---


### 前言

> 本教程记录是在ReactNative中文网提供的打包教程基础上扩展出来，因此文章是在原教程上进行填坑处理。因为本人在Mac上就踩了不少坑。

### 生成一个签名密钥

#### Windows平台

> 在 Windows 上`keytool`命令放在 JDK 的 bin 目录中（比如`C:\Program Files\Java\jdkx.x.x_x\bin`），使用管理员权限进入到该目录后执行命令：
>
> ```bash
> keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
> ```

#### Mac平台

> 同样，进入到的bin目录中（`/Library/Java/JavaVirtualMachines/jdkx.x.x_xxx.jdk/Contents/Home/bin`）,然后执行命令：
>
> ```bash
> sudo keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
> ```

#### 输入相关信息

> 这条命令会要求你输入密钥库（keystore）和对应密钥的密码，然后设置一些发行相关的信息。最后它会生成一个叫做`my-release-key.keystore`的密钥库文件。

> 在运行上面这条语句之后，密钥库里应该已经生成了一个单独的密钥，有效期为 10000 天。--alias 参数后面的别名是你将来为应用签名时所需要用到的，所以记得记录这个别名。

> **注意：请记得妥善地保管好你的密钥库文件，一般不要上传到版本库或者其它的地方。**

### 设置 gradle 变量

> 1.把`my-release-key.keystore`文件放到你工程中的`android/app`文件夹下。  
>
> 2.编辑`~/.gradle/gradle.properties`（全局配置，对所有项目有效）或是项目目录`/android/gradle.properties`（项目配置，只对所在项目有效）。如果没有`gradle.properties`文件你就自己创建一个，添加如下的代码（注意把其中的****替换为相应密码）

> **注意：~符号表示用户目录，比如 windows 上可能是C:\Users\用户名，而 mac 上可能是/Users/用户名。**

```bash
MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEASE_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=*****
MYAPP_RELEASE_KEY_PASSWORD=*****
```

### 把签名配置加入到项目的 gradle 配置中

> 编辑你项目目录下的`android/app/build.gradle`，添加如下的签名配置：

```java
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            if (project.hasProperty('MYAPP_RELEASE_STORE_FILE')) {
                storeFile file(MYAPP_RELEASE_STORE_FILE)
                storePassword MYAPP_RELEASE_STORE_PASSWORD
                keyAlias MYAPP_RELEASE_KEY_ALIAS
                keyPassword MYAPP_RELEASE_KEY_PASSWORD
            }
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...
```