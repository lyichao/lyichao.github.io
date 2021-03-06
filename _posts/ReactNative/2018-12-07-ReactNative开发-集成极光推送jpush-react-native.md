---
title: ReactNative开发-集成极光推送jpush-react-native
description: <center>记录Android平台集成</center>
categories:
 - ReactNative
tags: 进阶
updated: 2018-12-07 00:00:00
---

## 前言

> 极光推送官方有提供支持React Native版本的插件（ios和android），可以快速集成推送功能。
> 目前只集成Android部分，ios因为需要Apple开发者账号（99美金一年），暂时没办法记录。

## 安装

### 打开终端，定位到React Native项目根目录下，执行：

```bash
$ npm install jpush-react-native --save
# jpush-react-native 1.4.2 版本以后需要同时安装 jcore-react-native
$ npm install jcore-react-native --save
```

## 配置

```bash
$ react-native link jpush-react-native
$ react-native link jcore-react-native
```

### 这里注意下！！！

link失败也没事，可以手动进行配置。继续进行下面操作：

## Android平台集成

> ### 1.在android studio打开React Native项目下的android文件夹，然后跟着以下路径打开`build.gradle`：
>
> > 路径：`android/app/build.gradle`

### 2.修改`build.gradle`文件：

```java
android {
    ...
    defaultConfig {
        applicationId "yourApplicationId" // 此处改成你在极光官网上申请应用时填写的包名
        ...
        manifestPlaceholders = [
                JPUSH_APPKEY: "yourAppKey", //在此替换你在极光官网上申请应用得到的APPKey
                APP_CHANNEL: "developer-default"    //应用渠道号, 默认即可
        ]
    }
}
...
dependencies {
    compile fileTree(dir: "libs", include: ["*.jar"])
    compile project(':jpush-react-native')  // 添加 jpush 依赖
    compile project(':jcore-react-native')  // 添加 jcore 依赖
    compile "com.facebook.react:react-native:+"  // From node_modules
}
```

![极光上申请得到的APPKey](<http://lc-lf8y5iic.cn-n1.lcfile.com/20a60381e1ac93687a44/%E9%9B%86%E6%88%90%E6%9E%81%E5%85%89%E6%8E%A8%E9%80%81jpush-react-native1.png>)

<center>极光上申请得到的APPKey</center>

![极光上申请得到的applicationId](<http://lc-lf8y5iic.cn-n1.lcfile.com/b5cc0d490528364b721c/%E9%9B%86%E6%88%90%E6%9E%81%E5%85%89%E6%8E%A8%E9%80%81jpush-react-native2.png>)

<center>极光上申请得到的applicationId</center>

![极光上申请得到的applicationId](<http://lc-lf8y5iic.cn-n1.lcfile.com/9b9f5a8c27caf64cf888/%E9%9B%86%E6%88%90%E6%9E%81%E5%85%89%E6%8E%A8%E9%80%81jpush-react-native3.png>)

<center>极光上申请得到的applicationId</center>

> ### 3.打开`settings.gradle`
>
> >路径：`android/settings.gradle`

### 4.添加以下代码：

```java
include ':jcore-react-native'
project(':jcore-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/jcore-react-native/android')

include ':jpush-react-native'
project(':jpush-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/jpush-react-native/android')
```

![settings.gradle完整代码](<http://lc-lf8y5iic.cn-n1.lcfile.com/1f80717c16bc66d444aa/%E9%9B%86%E6%88%90%E6%9E%81%E5%85%89%E6%8E%A8%E9%80%81jpush-react-native4.png>)

<center>settings.gradle完整代码</center>

### 5.打开`AndroidManifest.xml`，添加：

```xml
<meta-data android:name="JPUSH_CHANNEL" android:value="${APP_CHANNEL}"/>
<meta-data android:name="JPUSH_APPKEY" android:value="${JPUSH_APPKEY}"/>
```

![AndroidManifest.xml完整代码](<http://lc-lf8y5iic.cn-n1.lcfile.com/95590bdc069c19c904a9/%E9%9B%86%E6%88%90%E6%9E%81%E5%85%89%E6%8E%A8%E9%80%81jpush-react-native5.png>)

<center>AndroidManifest.xml完整代码</center>

### 6.点击页面右上角`Sync Now`按钮，等待Android Studio编译成功过后，左侧的导航栏会多出两个包：

![编译成功后引入的包](<http://lc-lf8y5iic.cn-n1.lcfile.com/3106cd17d1c55127a8f5/%E9%9B%86%E6%88%90%E6%9E%81%E5%85%89%E6%8E%A8%E9%80%81jpush-react-native6.png>)

<center>编译成功后引入的包</center>

如果`Sync`失败，可以参考以下几点：

> - 网络环境是否稳定
> - 项目目录下的`build:gradle`文件里的`gradle版本`是否过低（个人使用3.1.4）

![完整build:gradle代码](<http://lc-lf8y5iic.cn-n1.lcfile.com/6228eda4da1d1bcf9ddb/%E9%9B%86%E6%88%90%E6%9E%81%E5%85%89%E6%8E%A8%E9%80%81jpush-react-native7.png>)

<center>完整build:gradle代码</center>

> ### 7.打开`MainApplication.java`
>
> >路径：`app/src/java/…/MainApplication.java`

引入`JPushPackage`包：

```java
import cn.jpush.reactnativejpush.JPushPackage;
```

添加两个常量：

```java
  // 设置为 true 将不弹出 toast
  private boolean SHUTDOWN_TOAST = false;
  // 设置为 true 将不打印 log
  private boolean SHUTDOWN_LOG = false;
```

然后在`getPackages()`方法里加入:

```java
new JPushPackage(SHUTDOWN_TOAST, SHUTDOWN_LOG)
```

![MainApplication.java完整代码](<http://lc-lf8y5iic.cn-n1.lcfile.com/e54d7f2859a6bfe6b928/%E9%9B%86%E6%88%90%E6%9E%81%E5%85%89%E6%8E%A8%E9%80%81jpush-react-native8.png>)

<center>MainApplication.java完整代码</center>

> ### 8.打开`MainActivity.java`
>
> > 路径：`app/src/java/…/MainActivity.java`

引入`JPushInterface`：

```java
import cn.jpush.android.api.JPushInterface;
```

添加以下4个方法：

```java
    @Override
    public void onCreate(@Nullable Bundle savedInstanceState, @Nullable PersistableBundle persistentState) {
        super.onCreate(savedInstanceState, persistentState);
        JPushInterface.init(this);

    }

    @Override
    protected void onPause() {
        super.onPause();
        JPushInterface.onPause(this);
    }

    @Override
    protected void onResume() {
        super.onResume();
        JPushInterface.onResume(this);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
    }
```

![MainActivity.java完整代码](<http://lc-lf8y5iic.cn-n1.lcfile.com/71b1086553b84543de86/%E9%9B%86%E6%88%90%E6%9E%81%E5%85%89%E6%8E%A8%E9%80%81jpush-react-native9.png>)

<center>MainActivity.java完整代码</center>


## 使用

现在就可以像使用第三方库一样，在需要用到的页面里添加以下代码：

```jsx
import JPushModule from 'jpush-react-native';

...

componentDidMount() {
    // 新版本必需写回调函数
    // JPushModule.notifyJSDidLoad();
    JPushModule.notifyJSDidLoad((resultCode) => {
          if (resultCode === 0) {}
    });

    // 接收自定义消息
    JPushModule.addReceiveCustomMsgListener((message) => {
      this.setState({pushMsg: message});
    });
    // 接收推送通知
    JPushModule.addReceiveNotificationListener((message) => {
      console.log("receive notification: " + message);
    });
    // 打开通知
    JPushModule.addReceiveOpenNotificationListener((map) => {
      console.log("Opening notification!");
      console.log("map.extra: " + map.extras);
      // 可执行跳转操作，也可跳转原生页面
      // this.props.navigation.navigate("SecondActivity");
    });
  }

  componentWillUnmount() {
    JPushModule.removeReceiveCustomMsgListener();
    JPushModule.removeReceiveNotificationListener();
  }
```

配置好了后可以到极光上测试：

![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/c5674da6b81d9951c1ba/%E9%9B%86%E6%88%90%E6%9E%81%E5%85%89%E6%8E%A8%E9%80%81jpush-react-native10.png>)

然后如果配置正确，打开手机上的app，就可以收到刚才的推送啦！

![接收推送](<http://lc-lf8y5iic.cn-n1.lcfile.com/1e0e98fa878cabb90a33/%E9%9B%86%E6%88%90%E6%9E%81%E5%85%89%E6%8E%A8%E9%80%81jpush-react-native11.gif>)

