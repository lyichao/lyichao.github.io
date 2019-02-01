---
title: ReactNative开发-红屏、报错记录
description: <center>记录在使用ReactNative日常开发中出现的红屏、报错</center>
categories:
 - ReactNative
tags: BUG/问题
updated: 2019-01-31 00:00:00
---

> ## BUG描述
>
> > error: bundling failed: "Unable to resolve module react-native/Libraries/EventEmitter/EventEmitter
>
> ![](https://upload-images.jianshu.io/upload_images/8154981-af021544c224ad8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> ### 解决办法
>
> >1.先把node_modules目录下的react-native-root-siblings删除
> >
> >2.再把根目录下package.json里的react-native-root-toast删除
> >
> >3.然后重新安装npm install react-native-root-toast --save



------



> ## BUG描述
>
> >A problem occurred configuring project ':app'.
> >
> >Could not resolve all dependencies for configuration ':app:_debugApk'.
> >
> >Configuration with name 'default' not found.
>
> ![](https://upload-images.jianshu.io/upload_images/8154981-4d2bf9abea5f6ffb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> ### 解决办法
>
> > 待补充



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
> ![](https://upload-images.jianshu.io/upload_images/8154981-f9ab45f239ca58d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> ### 解决办法
>
> >网络问题，删除node-modules目录，重新执行npm install



------



> ## BUG描述
>
> >Execution failed for task ':app:mergeDebugResources'.
> >
> >Error: Cannot create directory C:\Users\user\WebstormProjects\union\union\android\app\build\intermediates\incremental\mergeDebugResources\merged.dir\values
>
> ### 解决办法
>
> > 删除项目根目录下\android\app的build文件夹即可



------



> ## BUG描述
>
> >What went wrong:
> >
> >Execution failed for task ':app:installRelease'.
> >
> >com.android.builder.testing.api.DeviceException: com.android.ddmlib.InstallException: Failed to finalize session : INSTALL_FAILED_UPDATE_INCOMPATIBLE: Package com.union signatures do not match the previously installed version; ignoring!
>
> ![](https://upload-images.jianshu.io/upload_images/8154981-ab13d74752a3f371.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> ### 解决办法
>
> >这是由于正在安装的APP与已安装再手机上的APP冲突引起的。直接删除手机上的APP即可。



------



> ## BUG描述
>
> >\*What went wrong:
> >
> >A problem occurred configuring project ':app'.
> >
> >SDK location not found. Define location with sdk.dir in the local.properties file or with an ANDROID_HOME environment variable.
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
> >A problem occurred configuring root project 'one'.
> >
> >Could not resolve all dependencies for configuration ':classpath'.
> >
> >Could not download builder.jar (com.android.tools.build:builder:2.2.3)
> >
> >Could not get resource 'https://jcenter.bintray.com/com/android/tools/build/builder/2.2.3/builder-2.2.3.jar'.
> >
> >Could not GET 'https://jcenter.bintray.com/com/android/tools/build/builder/2.2.3/builder-2.2.3.jar'.
> >
> >Read timed out
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
> ![](https://upload-images.jianshu.io/upload_images/8154981-58ca6b024ed5ffc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
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