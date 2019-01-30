---
title: ReactNative开发-检测版本更新（非热更新）
description: <center>为app提供检测版本更新功能</center>
categories:
 - ReactNative
tags: 进阶
updated: 2017-12-06 00:00:00
---

## 前言

在APP开发中，检测版本更新功能是应该说是必不可少的。那么既然有这样的需求，就要有对应的方法来解决。在RN混合开发中，可以使用`react-native-app-upgrade`组件来进行，接下来就以这个`react-native-app-upgrade`来操作检测版本更新功能。

## 具体实现

### 步骤1

从github上下载`react-native-app-upgrade`组件，将解压后得到的`android_upgrade`文件夹放到你需要添加版本更新功能的项目目录下`android\app\src\main\java\com\包名`下，如下图所示：

![](https://upload-images.jianshu.io/upload_images/8154981-1a37a94f1c0d5a03.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接着再把解压得到的`ios_upgrade`文件夹放到需要添加版本更新功能的项目目录`\ios`下，如下图所示：

![](https://upload-images.jianshu.io/upload_images/8154981-432df7d0c7a76dab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接着修改`android_upgrade`文件夹下8个类文件的包名，如下图所示：

![](https://upload-images.jianshu.io/upload_images/8154981-889b8dfd9c069910.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果R文件报错也要修改成和包名一致，如下图所示：

![](https://upload-images.jianshu.io/upload_images/8154981-384edf64f9018f00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 步骤2

在`AndroidMainfest.xml`文件下添加权限和`service`组件

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```

```xml
<service
        android:name="com.包名.upgrade.DownloadService"
        android:exported="true"/>
```

![](https://upload-images.jianshu.io/upload_images/8154981-7ccf3d747db5c03c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>AndroidMainfest.xml完整代码</center>

接着在`android/app/src/main/res/values/strings.xml`文件下添加

```xml
<string name="android_auto_update_download_progress">正在下载:%1$d%%</string>
```



### 步骤3

到`MainApplication.java`文件下，先添加一下代码，导入`UpgradePackage.java`文件
`import 项目工程包名.UpgradePackage;`
然后在`getPackages（）`方法中添加`new UpUpgradePackage()`，如下图所示：

![å¾3-1](https://upload-images.jianshu.io/upload_images/8154981-650edff5a097f710.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 步骤4

到需要添加检测版本更新的代码页面下，先导入`NativeModules`模块，如下图所示：

![image.png](https://upload-images.jianshu.io/upload_images/8154981-15569820712b8a44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接着在构造方法里添加APP下载地址，如下图所示：

![å¾4-2](https://upload-images.jianshu.io/upload_images/8154981-c47e241b596c25ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接着在需要触发检测版本更新功能的地方添加
`NativeModules.upgrade.upgrade(this.state.apkUrl);`
例如，在我自己的项目下，我为检测版本更新功能所写的代码

```jsx
 <ItemActionViews
                 onPress={() => this.checkVersion()}   //触发checkVersion()版本检查方法
                 showTitle="当前版本" showText={this.state.localVersion}/>
//版本检查
    checkVersion() {
        let version = this.state.version;        //api中的版本号
        let localVersion = this.state.localVersion;    //本地版本号
        console.log('最新版本：' + version);
        console.log('当前版本：' + localVersion);
        if (Platform.OS === 'android') {
            if (version != localVersion) {
                Alert.alert('', '最新版本为' + version + '，是否下载', [{text: '取消', onPress: () => console.log('取消')}, {
                    text: '确定', onPress: () => {
                        NativeModules.upgrade.upgrade(this.state.apkUrl);
                    }
                },]);
            } else {
                Toast.show('当前为最新版本');
            }
        } else {
            NativeModules.upgrade.upgrade('1297109983', (msg) => {
                if ('YES' == msg) {
                    //跳转到APP Stroe
                    NativeModules.upgrade.openAPPStore('1297109983');
                } else {
                    Toast.show('当前为最新版本');
                }
            })
        }

    };
```

如果在配置中出现问题，请留言指出或参考组件的github地址，我会力所能及解答疑惑。

