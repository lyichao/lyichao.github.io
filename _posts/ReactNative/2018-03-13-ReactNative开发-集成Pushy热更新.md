---
title: ReactNative开发-集成Pushy热更新
description: <center>记录集成Pushy</center>
categories:
 - ReactNative
tags: 进阶
updated: 2018-03-15 00:00:00
---

## 前言

> 因为在实际开发环境中，会有很多各种各样的原因导致各种各样的BUG存在，又或者想在某些节日推出特定的活动等这些情况存在。显然每次都叫用户重新下载更新APP是不太现实的。因此，这时候热更新就发挥作用了。这次，我们来看下目前比较主流的两套热更新方案`CodePush`和`Pushy`是怎么集成和有哪些注意问题。

## 什么是热更新？

> - **广义定义**：无需关闭服务器，不停机状态下修复漏洞，更新资源等，重点是更新逻辑代码。
>
> - **狭义定义**：无需将代码重新打包提交至`AppStore`，即可更新客户端执行代码，即不用下载app而自动更新程序。
>
> - **现状**：因为`AppStore`不允许通过热更新的方式修改原生代码，所以目前热更新只能实现JS部分的代码更新。如果修改设计原生代码部分，那么就需要重新发布APP安装包。
>
> - **热更新流程图**
>
>   ![image.png](https://upload-images.jianshu.io/upload_images/8154981-ca833169b5824a96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 配置Pushy

### 新建PushyDemo

> > 先新建一个名叫PushyDemo的例子，Reactnative版本就采用`0.48.1`好了（个人觉得比较稳定的一个版本）在命令行输入命令后并进入到项目根目录下：
>
> ```
> react-native init PushyDemo --version 0.48.1
> cd PushyDemo
> ```

### 安装Pushy

> > 在根目录下输入：  
> >
> > ```
> > npm i -g react-native-update-cli 
> > ```
>
> 
>
> > 接着再根据当前的ReactNative版本安装对应的Pushy版本：  
> >
> > | React Native版本 | react-native-update版本 |
> > | :--------------: | :---------------------: |
> > |    0.26及以下    |          1.0.x          |
> > |   0.27 - 0.28    |           2.x           |
> > |   0.29 - 0.33    |           3.x           |
> > |   0.34 - 0.45    |           4.x           |
> > |    0.46及以上    |           5.x           |
> >
> > ```
> > npm i react-native-update@5.x
> > ```
>
> 
>
> > 最后，link操作：
> >
> > ```   
> > react-native link react-native-update
> > ```

### 配置Bundle URl

#### Android平台

> > 在`根目录/android/app/src/main/java/com/PushyDemo`下的`MainApplication.java`文件增加以下代码：
> >
> > ```java
> > // ... 其它代码
> > 
> > // 请注意不要少了这句import
> > import cn.reactnative.modules.update.UpdateContext;
> > public class MainApplication extends Application implements ReactApplication {
> > 
> >   private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
> >     @Override
> >     protected String getJSBundleFile() {
> >         return UpdateContext.getBundleUrl(MainApplication.this);
> >     }
> >     // ... 其它代码
> >   }
> > }
> > ```

#### IOS平台（待更新）

### 登录&创建账号

> > **1.**   如果还没创建账号，要先到**[热更新平台](http://update.reactnative.cn)**创建账号，接着就回到项目根目录下执行以下代码，然后会要求你输入你的`email`（你注册的账号）和`password`密码。  
> >
> > ```
> > pushy login 
> > ```
>
> 
>
> > **2.**  登录成功后，命令行上会返回登录成功信息。接着会在项目根目录下生成一个`.update`文件。<mark>这里注意：因为这个.update记录的是token信息，记得把它添加到.gitignore文件中，可以在.gitignore末尾添加一行.update。这样就可以在提交到Github/GItLab中忽略这个文件。</mark>
>
> <br/>
>
> > **3.**  接下来就可以创建一个用于提供热更新的应用啦！执行以下命令（要区分平台创建喔！）：
> >
> > ```
> > pushy createApp --platform android
> > //或者
> > pushy createApp --platform ios
> > ```
>
> <br/>
>
> > **4.**  执行命令后会要求你输入`App Name:`这是要创建应用的名字，在本文这里就叫`PushyDemo`。
>
> <br/>
>
> > **5.**如果你在平台网页上已经创建了应用，也可以执行以下命令来选择应用：
> >
> > ```
> > pushy selectApp -- platform android
> > //或者
> > pushy selectApp -- platform android
> > ```
>
> <br/>
>
> > **6.**之后会提示要你输入`appId`,例如：
> >
> > ```
> > leedeMac-mini:Demo lee$ pushy selectApp --platform android
> > 17395) PushyDemo(android)
> > 
> > Total 1 android apps
> > Enter appId: 
> > ```
> >
> > 这里就直接输入17395，然后按回车好了。
>
> <br/>
>
> > **7.**在创建/选择应用后，会在根目录下看到一个`update.json`文件。这可以文件放心上传，它只是记录识别app的一些信息而已。

### 添加热更新功能

> > 检查应用更新，最好的时间当然就是打开app的那个时候了吧！所以，一般把检查更新放到登录页面或者登录后的首页。不过个人推荐把检查更新放到登录后的首页，这样做更符合产品设计逻辑。而触发检查的时间就放到生命周期的`componentDidMount`方法里。  
>
> <br/>
>
> > **1.**引入`appKey`，添加以下代码用于判断更新平台
> >
> > ```jsx
> > import {Platform} from 'react-native';
> > 
> > import _updateConfig from './update.json';
> > const {appKey} = _updateConfig[Platform.OS];
> > ```
>
> 
>
> > **2.**添加检查更新方法`checkUpdate`，用于检查当前版本是否需要更新：
> >
> > ```
> > checkUpdate(appKey)
> >     .then(info => {
> >     })
> > ```
> >
> > 其中，返回的`info`参数有三种情况：  
> >
> > - 第一种：`{expired:true}`原生代码部分有更新，需要重新下载整个APP；
> > - 第二种：`{upToDate:true}`应用已经是最新版本；
> > - 第三种：`{update:true}`有新版本可以更新。其中`info`中的`name`和`description`可以用于提醒用于，而`metaInfo`字段则可以根据你的需求自定义其它属性(如是否静默更新、 是否强制更新等等)。另外还有几个字段，包含了完整更新包或补丁包的下载地址， `react-native-update`会首先尝试耗费流量更少的更新方式。将`info`对象传递给`downloadUpdate`作为参数即可。
>
> <br/>
>
> > **3.**选择更新版本时机。Pushy为我们提供了`switchVersion`（立即更新版本）和`switchVersionLater`（下次启动再更新版本）两个方法，这里大家就根据业务需求选取一个就好了。当然，也可以![timg.gif](https://upload-images.jianshu.io/upload_images/8154981-4136264e53aa30df.gif?imageMogr2/auto-orient/strip)
>
> <br/>
>
> > **4.**必须知道的几个常量：`isFirstTime`、`markSuccess`和`isRolledBack`  。在每次更新完毕后的首次启动时，`isFirstTime`常量会为`true`。 你必须在应用退出前合适的任何时机，调用`markSuccess`。
>
> <br/>
>
> > **5.**下面我们引入官网提供的完整示例，来进一步了解Pushy：
> >
> > - 在官网提供的示例中，检查更新`checkUpdate`是手动点击第82行中的`TouchableOpacity`按钮触发的。
> > - 紧接着在第53行里，`checkUpdate`方法拿着已经判断好平台的`appKey`去发起请求信息：
> >   - 如果返回`info.expired`就弹出弹窗告诉用户APP版本需要重新下载；
> >   - 如果返回`info.upToDate`  就弹出弹窗告诉用户APP是最新版，不需要更新；
> >   - 如果 `info`返回的信息都不是以上两者中的某一个（其实我觉得这里应该直接拿`info.update`来做判断更形象具体），那么就提取其中的`info.name`和`info.description`字段弹出弹窗告诉用户APP有更新可用，让用户是否选择下载。
> > - 如果用户`info.update`情况下选择了去下载，那么就会带着返回的`info`信息调取第42行的`doUpdate`方法：
> >   - 如果能顺利下载完成，会继续弹出弹窗让用户选择是否更新立即更新（调取`switchVersion`方法）还是下次启动更新（调取`switchVersionLater`方法）
> >   - 如果用户选择了立即更新（调取`switchVersion`方法），则APP马上更新并再完成更新后重启APP。因为是重启APP，自然而然按照国际惯例都必须重走一遍生命周期，所以就会触发到第32行的`componentWillMount`方法。在该生命周期中，拿了`isFirstTime`和`isRolledBack`来做判断。因为是更新重启后的第一次启动，所以当前的`isFirstTime == true`，再次弹出弹窗提示用户做相应的操作。
> > - 到这里，官方示例就分析完毕了。顺着思路走挺简单吧！
>
> ```jsx
> import React, {
>   Component,
> } from 'react';
> 
> import {
>   AppRegistry,
>   StyleSheet,
>   Platform,
>   Text,
>   View,
>   Alert,
>   TouchableOpacity,
>   Linking,
> } from 'react-native';
> 
> import {
>   isFirstTime,
>   isRolledBack,
>   packageVersion,
>   currentVersion,
>   checkUpdate,
>   downloadUpdate,
>   switchVersion,
>   switchVersionLater,
>   markSuccess,
> } from 'react-native-update';
> 
> import _updateConfig from './update.json';
> const {appKey} = _updateConfig[Platform.OS];
> 
> class MyProject extends Component {
>   componentWillMount(){
>     if (isFirstTime) {
>       Alert.alert('提示', '这是当前版本第一次启动,是否要模拟启动失败?失败将回滚到上一版本', [
>         {text: '是', onPress: ()=>{throw new Error('模拟启动失败,请重启应用')}},
>         {text: '否', onPress: ()=>{markSuccess()}},
>       ]);
>     } else if (isRolledBack) {
>       Alert.alert('提示', '刚刚更新失败了,版本被回滚.');
>     }
>   }
>   doUpdate = info => {
>     downloadUpdate(info).then(hash => {
>       Alert.alert('提示', '下载完毕,是否重启应用?', [
>         {text: '是', onPress: ()=>{switchVersion(hash);}},
>         {text: '否',},
>         {text: '下次启动时', onPress: ()=>{switchVersionLater(hash);}},
>       ]);
>     }).catch(err => { 
>       Alert.alert('提示', '更新失败.');
>     });
>   };
>   checkUpdate = () => {
>     checkUpdate(appKey).then(info => {
>       if (info.expired) {
>         Alert.alert('提示', '您的应用版本已更新,请前往应用商店下载新的版本', [
>           {text: '确定', onPress: ()=>{info.downloadUrl && Linking.openURL(info.downloadUrl)}},
>         ]);
>       } else if (info.upToDate) {
>         Alert.alert('提示', '您的应用版本已是最新.');
>       } else {
>         Alert.alert('提示', '检查到新的版本'+info.name+',是否下载?\n'+ info.description, [
>           {text: '是', onPress: ()=>{this.doUpdate(info)}},
>           {text: '否',},
>         ]);
>       }
>     }).catch(err => { 
>       Alert.alert('提示', '更新失败.');
>     });
>   };
>   render() {
>     return (
>       <View style={styles.container}>
>         <Text style={styles.welcome}>
>           欢迎使用热更新服务
>         </Text>
>         <Text style={styles.instructions}>
>           这是版本一 {'\n'}
>           当前包版本号: {packageVersion}{'\n'}
>           当前版本Hash: {currentVersion||'(空)'}{'\n'}
>         </Text>
>         <TouchableOpacity onPress={this.checkUpdate}>
>           <Text style={styles.instructions}>
>             点击这里检查更新
>           </Text>
>         </TouchableOpacity>
>       </View>
>     );
>   }
> }
> 
> const styles = StyleSheet.create({
>   container: {
>     flex: 1,
>     justifyContent: 'center',
>     alignItems: 'center',
>     backgroundColor: '#F5FCFF',
>   },
>   welcome: {
>     fontSize: 20,
>     textAlign: 'center',
>     margin: 10,
>   },
>   instructions: {
>     textAlign: 'center',
>     color: '#333333',
>     marginBottom: 5,
>   },
> });
> 
> AppRegistry.registerComponent('MyProject', () => MyProject);
> ```

## 发布应用更新

> 配置好了，记下来的就是干活咯～

### Android平台

#### 1.到android目录下，运行打包命令打包APP：

```
./gradlew assembleRelease
```

#### 2.把打包好的APP发布到Pushy官网平台，执行发布应用命令：

```
pushy uploadApk android/app/build/outputs/apk/app-release.apk
```

<mark>注意：每次修改有涉及原生代码部分都要执行以上两个步骤，并且如果不是第一次发布的话，还要在Pushy官网平台上移除需要废弃的APK安装包</mark>

#### 3.发布新的热更新版本

> 这里就是每当要修改`JavaScript`部分并需要发布热更新的时候执行的步骤，比如我把官网示例中第78行改成“这是版本二！！！！！！”，然后保存修改后执行以下命令：
>
> ```
> pushy bundle --platform android
> ```
>
> 然后你会看到以下内容：
>
> ```
> Bundling with React Native version:  0.48.1
> <各种进度输出>
> Bundled saved to: build/output/android.1459850548545.ppk
> Would you like to publish it?(Y/N) 
> ```
>
> 如果想要立即发布，此时输入Y。然后控制台会继续输出显示以下内容并要求你输入相关信息：
>
> ```
> Uploading [========================================================] 100% 0.0s
> Enter version name: <输入版本名字，如1.0.0-rc>
> Enter description: <输入版本描述>
> Enter meta info: {"ok":1}
> Ok.
> Would you like to bind packages to this version?(Y/N)
> ```
>
> 此时继续输入Y，然后控制台会要求你输入`packageId`，我这里是`packageId`是26736
>
> ```
> 26736) 1.0.04(normal) - 55844 FkqViKag 1.0.04
> 
> Total 1 packages.
> Enter packageId: 
> ```
>
> 到此为止，发布已经完成，Pushy官网平台就会出刚刚发布的哟应用，APP上也可以收到更新消息啦！
>
> ![image.png](https://upload-images.jianshu.io/upload_images/8154981-b00d1311e286e703.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> <center>Pushy官网平台收到的更新包</center>
>
> ![ymmfd-561ao.gif](https://upload-images.jianshu.io/upload_images/8154981-ddcbb6ac70a9c99a.gif?imageMogr2/auto-orient/strip)

### IOS平台（待更新）