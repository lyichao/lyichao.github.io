---
title: ReactNative开发-红屏、报错记录
description: <center>记录在使用ReactNative日常开发中出现的红屏、报错</center>
categories:
 - ReactNative
tags: BUG/问题
updated: 2020-06-09 00:00:00
---

> ## BUG描述
>
> > error: bundling failed: "Unable to resolve module react-native/Libraries/EventEmitter/EventEmitter
>
> ![](<http://lc-lf8y5iic.cn-n1.lcfile.com/56edace0e537d9d30462/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%951.png>)
>
> ### 解决办法
>
> >~~1.先把node_modules目录下的react-native-root-siblings删除~~
> >
> >~~2.再把根目录下package.json里的react-native-root-toast删除~~
> >
> >~~3.然后重新安装npm install react-native-root-toast —save~~
> >
> >1.进入/node_modules/react-native-root-siblings/lib/AppRegistryInjection.js
> >
> >2.找到`import EventEmitter from 'react-native/Libraries/vendor/emitter/EventEmitter’;`并修改为`import EventEmitter from 'react-native/Libraries/EventEmitter/EventEmitter';`



------



> ## BUG描述
>
> >A problem occurred configuring project ':app'.
> >
> >Could not resolve all dependencies for configuration ':app:_debugApk'.
> >
> >Configuration with name 'default' not found.
>
> ![](<http://lc-lf8y5iic.cn-n1.lcfile.com/52b5736d452bc85f2ef6/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%952.png>)
>
> ### 解决办法
>
> > 问题原因：都是墙惹的祸~~~
> >
> > 1.修改android根目录下的build.gradle。把`buildscript`和`allprojects`下的`google()`和`jcenter()`注释掉，添加阿里云镜像`maven{ url 'http://maven.aliyun.com/nexus/content/groups/public'}`
> >
> > P.s：如果某个组件也报这个错误，就到组件的android目录下同样处理



------



> ## BUG描述
>
> >Execution failed for task ':app:installDebug'.
> >
> >com.android.builder.testing.api.DeviceException: com.android.ddmlib.InstallException: Failed to finalize session : INSTALL_FAILED_UPDATE_INCOMPATIBLE: Package com.gs2000_rn signatures do not match the previously installed version; ignoring!
>
> ### 解决办法
>
> > 删除当前链接设备中已有的项目



------



> ## BUG描述
>
> >Execution failed for task ':app:prepareGs2000_rnReactNativeImagePickerUnspecifiedLibrary'.
> >
> >Could not expand ZIP 'C:\Users\user\WebstormProjects\gs2000e_rn\node_modules\react-native-image-picker\android\build\outputs\aar\react-native-image-picker-release.aar'.
>
> ![](<http://lc-lf8y5iic.cn-n1.lcfile.com/787dc62c36fb2552b1f7/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%953.png>)
>
> ### 解决办法
>
> >网络问题，删除node-modules目录，重新执行npm install



------



> ## BUG描述
>
> >```
> >Execution failed for task ':app:mergeDebugResources'.
> >Error: Cannot create directory C:\Users\user\WebstormProjects\union\union\android\app\build\intermediates\incremental\mergeDebugResources\merged.dir\values
> >```
>
> ### 解决办法
>
> > 删除项目根目录下\android\app的build文件夹即可



------



> ## BUG描述
>
> >```
> >What went wrong:
> >Execution failed for task ':app:installRelease'.
> >com.android.builder.testing.api.DeviceException: com.android.ddmlib.InstallException: Failed to finalize session : INSTALL_FAILED_UPDATE_INCOMPATIBLE: Package com.union signatures do not match the previously installed version; ignoring!
> >```
>
> ![](<http://lc-lf8y5iic.cn-n1.lcfile.com/47c65eb207c551b2e748/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%954.png>)
>
> ### 解决办法
>
> >这是由于正在安装的APP与已安装再手机上的APP冲突引起的。直接删除手机上的APP即可。



------



> ## BUG描述
>
> >```
> >*What went wrong:
> >A problem occurred configuring project ':app'.
> >SDK location not found. Define location with sdk.dir in the local.properties file or with an ANDROID_HOME environment variable.
> >```
>
> ### 解决办法
>
> >项目缺少android环境变量，从其他原生android项目根目录下复制一份local.properties文件到RN项目的android目录下即可！



------



> ## BUG描述
>
> >发布新的热更新版本时出现问题
> >
> >Cannot find module 'metro-bundler/src/babelRegisterOnly
>
> ### 解决办法
>
> > 根据问题查找得知是在高版本（0.52）以上在metro-bundler引用的路径错了。将node_modules\react-native-update\local-cli\lib\bundle.js的439行种的metro-bundler改成metro可成功运行！[参考文献



------



> ## BUG描述
>
> >SyntaxError:Strict mode does not allow function declarations in a lexically nested statement.
> >
> >react-native的源码问题，在use strict严苛模式下，不允许如上的方式声明函数，会编译不通过。
>
> ### 解决办法
>
> > 将react-native版本改成0.38.0或以上。



------



> ## BUG描述
>
> >```
> >A problem occurred configuring root project 'one'.
> >Could not resolve all dependencies for configuration ':classpath'.
> >Could not download builder.jar (com.android.tools.build:builder:2.2.3)
> >Could not get resource 'https://jcenter.bintray.com/com/android/tools/build/builder/2.2.3/builder-2.2.3.jar'.
> >Could not GET 'https://jcenter.bintray.com/com/android/tools/build/builder/2.2.3/builder-2.2.3.jar'.
> >Read timed out
> >```
>
> ### 解决办法
>
> >网络异常导致，保证网络的稳定下再次尝试基本可以解决



------



> ## BUG描述
>
> >SDK location not found. Define location with sdk.dir in the local.properties file or with an ANDROID_HOME environment variable.
>
> ### 解决办法
>
> >SDK路径无效！
> >
> >1.可以使用Android Studio打开该项目的android项目，Android Studio就会自动校正
> >
> >2.也可以打开local.properties文件，手动修改SDK路径



------



> ## BUG描述
>
> >运行项目出现unable to load script from assets 'index.android.bundle'，
> >
> >unable to load script from assets 'index.android.bundle'错误
> >
> >如图所示：
>
> ![](<http://lc-lf8y5iic.cn-n1.lcfile.com/206aa79f78892fae1cac/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%955.png>)
>
> ### 解决办法
>
> >1.进入项目的根目录
> >
> >2.在终端输入
> >
> >mkdir android/app/src/main/assets
> >
> >3.继续在终端输入
> >
> >React-native bundle --platform Android --dev false --entry-file index.android.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res



---



> ## BUG描述
>
> > ```bash
> > Error: Validation error
> >     at /Users/lee/RNWorkStation/Demo/node_modules/react-native-update/local-cli/lib/api.js:8:27
> >     at Generator.next (<anonymous>)
> >     at step (/Users/lee/RNWorkStation/Demo/node_modules/react-native-update/local-cli/lib/api.js:82:191)
> >     at /Users/lee/RNWorkStation/Demo/node_modules/react-native-update/local-cli/lib/api.js:82:361
> >     at <anonymous>
> >     at process._tickCallback (internal/process/next_tick.js:189:7)
> > ```
>
> ### 解决办法
>
> > 已经上传过同样版本号的ipa，重新上传需要修改版本号
> >
> > [问题参考]（https://github.com/reactnativecn/react-native-pushy/issues/143）
> >
> > - android平台：修改根目录`/android/app/build.gradle`下的`versionCode`和`versionName`.例如： 
> >
> >   ```java
> >   versionCode 2
> >   versionName "1.0.02"
> >   ```
> >
> > - ios平台：（待添加） 



---



> ## BUG描述
>
> > ```bash
> > * What went wrong:
> > Could not resolve all files for configuration ':react-native-code-push:_internal_aapt2_binary'.
> > > Could not resolve com.android.tools.build:aapt2:3.2.1-4818971.
> > Required by:
> >    project :react-native-code-push
> >    > Could not resolve com.android.tools.build:aapt2:3.2.1-4818971.
> >    > Could not get resource 'https://jcenter.bintray.com/com/android/tools/build/aapt2/3.2.1-4818971/aapt2-3.2.1-4818971.pom'.
> >       > Could not HEAD 'https://jcenter.bintray.com/com/android/tools/build/aapt2/3.2.1-4818971/aapt2-3.2.1-4818971.pom'.
> >          > Connect to 127.0.0.1:1087 [/127.0.0.1] failed: Connection refused (Connection refused)
> > 
> > ```
>
> ### 解决办法
>
> > ```
> > systemProp.https.proxyPort=``1080
> > systemProp.http.proxyHost=``127.0``.``0.1
> > systemProp.https.proxyHost=``127.0``.``0.1
> > systemProp.http.proxyPort=``1080
> > ```
> >
> > `gradle.properties `文件中使用了代理，删除`gradle.properties`中的代理以及计算机用户目录下的`.gradle`文件夹中的代理即可。



---



> ## BUG描述
>
> > ```bash
> > undefined is not a function (evaluating '_react.default.createContext(null)')
> > <unknown>
> >     Context.js:10:53
> > loadModuleImplementation
> >     require.js:174:12
> > guardedLoadModule
> >     require.js:126:36
> > _require
> >     require.js:110:20
> > <unknown>
> >     Provider.js:16:23
> > loadModuleImplementation
> >     require.js:174:12
> > guardedLoadModule
> >     require.js:126:36
> > _require
> >     require.js:110:20
> > <unknown>
> >     index.js:7:47
> > loadModuleImplementation
> >     require.js:174:12
> > guardedLoadModule
> >     require.js:126:36
> > _require
> >     require.js:110:20
> > <unknown>
> >     index.js:5
> > loadModuleImplementation
> >     require.js:174:12
> > guardedLoadModule
> >     require.js:126:36
> > _require
> >     require.js:110:20
> > <unknown>
> >     index.android.js:1:8
> > loadModuleImplementation
> >     require.js:174:12
> > guardedLoadModule
> >     require.js:119:45
> > _require
> >     require.js:110:20
> > global code
> > ```
>
> ### 解决办法
>
> > 这是由于react版本引起的，猜测可能是react版本与引入`react-redux`兼容性问题所导致。
> >
> > 引起BUG的环境：
> >
> > ```bash
> > "react": "16.0.0-alpha.12",
> > "react-native": "0.48.1",
> > "react-navigation": "1.0.0-beta.13",
> > "react-redux": "^6.0.0",
> > "redux": "^4.0.1",
> > "redux-logger": "^3.0.6",
> > "redux-thunk": "^2.3.0"
> > ```
> >
> > 消除BUG的环境：（降低了`react-redux`版本）
> >
> > ```bash
> > "react": "16.0.0-alpha.12",
> > "react-native": "0.48.1",
> > "react-navigation": "1.0.0-beta.13",
> > "react-redux": "5.0.5",
> > "redux": "^4.0.1",
> > "redux-logger": "^3.0.6",
> > "redux-thunk": "^2.3.0"
> > ```



---



> ## BUG描述
>
> 执行`react-native run-android`后提示：
>
> ```bash
> leedeMac-mini:RepairUnion lee$ react-native run-android
> Scanning 688 folders for symlinks in /Users/lee/RNWorkStation/RepairUnion/node_modules (8ms)
> Starting JS server...
> Building and installing the app on the device (cd android && ./gradlew installDebug)...
> Could not install the app on the device, read the error above for details.
> Make sure you have an Android emulator running or a device connected and have
> set up your Android development environment:
> https://facebook.github.io/react-native/docs/android-setup.html
> 
> ```
>
> ### 解决办法
>
> > 1.配置全局环境变量
> >
> > ```bash
> > $ vim ~/.bash_profile
> > $ export ANDROID_HOME="/Users/[your name]/Library/Android/sdk/android-sdk_r24.2" # 以实际位置为准
> > $ source ~/.bash_profile # 立即生效
> > ```
> >
> > 2.配置权限 
> >
> > 如果配置环境后不可以运行 可能是无权限运行,在项目下运行 就是为android文件夹下的gradlew命令添加权限, **在根目录下执行 chmod 755 android/gradlew,本次就是用这个方法解决 **
> >
> > 3.项目中`android`目录下新建文件`local.properties`,并添加如下内容：
> >
> > ```bash
> > sdk.dir=/Users/[your name]/Library/Android/sdk/android-sdk_r24.2
> > ```



---



> ## BUG描述
>
> ```bash
> Expected a component class, got [object Object].
> ```
>
> ### 解决办法 
>
> > 类名必须大写。检查引入的类是否有小写存在。
> >
> > ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/09ac6dd17a23518c1d03/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%956.png>)
> >
> > ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/c9a53a5263ae153e2456/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%957.png>)



---



> ##BUG描述
>
> ```
> unable to connect with remote debugger timeout while connecting to remote debugg
> ```
>
> ### 解决办法
>
> > 如图打勾即可！
> >
> > ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/7cc29b3674bab7f1d55d/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%958.png>)



---



>## BUG描述
>
>```objective-c
>node_modules/react-native/Libraries/WebSocket/libfishhook.a'
>```
>
>### 解决办法
>
>> - 1.在Xcode中找到RCTWebSocket.xcodeproj文件
>>
>>   ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/7dade65690135ab92be3/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%959.png>)
>>
>> - 2.找到Build Phases的Link Binary With Libraries, 点击减号去掉libfishhook.a引用
>>
>>   ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/06e04bbdd057f9f09ce6/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%9510.png>)
>>
>> - 3.再加回去: 点加号把libfishhook.a引用
>>
>>   ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/6adca72e706cd94de030/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%9511.png>)
>>
>> - 4.clean build 以及重新编译，OK解决！



---

>## BUG描述
>
>```objective-c
>":CFBundleIdentifier", Does Not Exist
>```
>
>### 解决办法
>
>>- 1.打开Project settings
>>
>>  ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/fb43412e829e03e33aed/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%9512.png>)
>>
>>- 2.点击Advanced按钮
>>
>>  ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/ba0ec8e367c5ac9d10d9/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%9513.png>)
>>
>>- 3.选择Custom，然后点击下拉选择Relative to Workspace
>>
>>- 4.修改Build/Products 为 build/Build/Products
>>
>>  ![image.png](<http://lc-lf8y5iic.cn-n1.lcfile.com/357b26b18d8a86033a03/%E7%BA%A2%E5%B1%8F%E3%80%81%E6%8A%A5%E9%94%99%E8%AE%B0%E5%BD%9514.png>)
>>
>>- 5.clean build 以及重新编译，OK解决！
>
>

---

>## BUG描述
>
>打包时出现
>
>```
>Unable to process incoming event 'ProgressComplete ' (ProgressCompleteEvent)
>```
>
>### 解决办法
>
>>到项目根目录/android下执行以下命令即可：
>>
>>```
>>gradlew.bat assembleRelease --console plain
>>```
>
>

---

>## BUG描述
>
>在执行pod install 命令到Installing glog (0.3.5)时报错，如下：
>
>```
>Installing glog (0.3.5)
>[!] /bin/bash -c
>set -e
>#!/bin/bash
># Copyright (c) Facebook, Inc. and its affiliates.
>#
># This source code is licensed under the MIT license found in the
># LICENSE file in the root directory of this source tree.
>
>set -e
>.........
>configure: error: C compiler cannot create executables
>See `config.log' for more details
>```
>
>### 解决办法
>
>>未选择指定的Xcode:
>>
>>```
>>sudo xcode-select --switch /Applications/Xcode.app
>>```
>
>
>
---

>## BUG描述
>
>Android打包App时出现错误，如下：
>
>```
>Expiring Daemon because JVM heap space is exhausted
>```
>
>### 解决办法
>
>>原因： jvm 默认可分配的堆内存不够
>>
>>在 **gradle.properties** 文件中添加
>>
>>```
>>org.gradle.jvmargs=-Xmx4096m -XX:MaxPermSize=512m
>>```
>
>
---

>## BUG描述
>
>Android打包App后运行App时出现错误，如下：
>
>```
>TypeError: console.assert is not a function.
>```
>
>### 解决办法
>
>>原因： 函数未定义。https://github.com/facebook/react-native/issues/26007
>>
>>添加如下代码即可
>>
>>```javascript
>>if (!__DEV__) {
>>    global.console = {
>>        info: () => {},
>>        log: () => {},
>>        assert: () => {},
>>        warn: () => {},
>>        debug: () => {},
>>        error: () => {},
>>        time: () => {},
>>        timeEnd: () => {},
>>    };
>>}  
>>```
>
>
---

>## BUG描述
>
>ios运行`pod install`时出现如下问题：
>
>```
>CDN: trunk URL couldn't be downloaded
>```
>
>### 解决办法
>
>>原因： 网络问题，到host文件下添加如下代码即可
>>
>>```
>>199.232.4.133 raw.githubusercontent.com
>>```
>
>



---



>## BUG描述
>
>运行android时出现如下问题：
>
>```
>Execution failed for task ':app:installDebug'.
>
>> com.android.builder.testing.api.DeviceException: com.android.ddmlib.InstallException: INSTALL_FAILED_UPDATE_INCOMPATIBLE: Package com.veminar signatures do not match previously installed version; ignoring!
>```
>
>### 解决办法
>
>>原因： packname包名冲突，打开android studio重新编译即可
>>
>
>



---



>## BUG描述
>
>运行IOS时出现如下问题：
>
>```
>Unknown argument type"attribute_inmethod-irctappstate
>getcurrentappstate: error: Extend
>Rctconvert to support this type.
>[Rctmodulemethod processmethodsignature]
>Rctmodulemethod, mm: 376
>-[Rctmodulemethod invokewithbridge: module: arguments: ]
>```
>
>### 解决办法
>
>>原因： 原因是`Xcode11(iOS13)`中对未使用的接口选择器的参数`unused`字符串属性进行了更改成了`__unused__`，导致`ReactNative`动态收集接口时不能把声明的接口进行导入，运行时无法查找到该接口导致的错误。
>>
>>解决办法：
>>
>>找到文件`RCTModuleMethod.mm`，路径：`node_modules/react-native/React/Base/RCTModuleMethod.mm`
>> (原文文件名称为`RCTModuleMethod.mm.mm`，但是我的项目中文件名为`RCTModuleMethod.m`)；
>> 在`RCTParseUnused`接口中新增新的解析字段，新增`RCTReadString(input, "__attribute__((__unused__))")`代码，如下：
>>
>>```cpp
>>static BOOL RCTParseUnused(const char **input)
>>{
>>  return RCTReadString(input, "__unused") ||
>>         RCTReadString(input, "__attribute__((__unused__))") || 
>>         RCTReadString(input, "__attribute__((unused))");
>>}
>>```
>>
>>
>
>



---



>## BUG描述
>
>运行Android时出现如下问题：
>
>```
>* What went wrong:
>Execution failed for task ':react-native-fetch-blob:processReleaseResources'.
>> com.android.ide.common.process.ProcessException: Failed to execute aapt
>
>```
>
>或
>
>```
>java.io.IOException: Cannot run program "/Users/vian_imac/Library/Android/sdk/build-tools/23.0.1/aapt": error=86, Bad CPU type in executable
>```
>
>
>
>### 解决办法
>
>>原因： 项目的gradle版本升级了导致第三方插件无法打包。
>>
>>解决办法：
>>
>>从node_modules文件夹中找到 react-native-fetch-blob/android/build.gradle ，并修改如下部分，：
>>
>>```cpp
>>compileSdkVersion 23
>>buildToolsVersion "23.0.3"
>>defaultConfig {
>>minSdkVersion 16
>>targetSdkVersion 23
>>versionCode 1
>>versionName "1.0"
>>}
>>
>>改为
>>
>>compileSdkVersion 26
>>buildToolsVersion "26.0.3"
>>defaultConfig {
>>minSdkVersion 16
>>targetSdkVersion 26
>>versionCode 1
>>versionName "1.0"
>>}
>>```
>>
>>
>
>

