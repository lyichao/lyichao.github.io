---
title: ReactNative开发-链接原生库
description: <center>在ReactNative上没有相对应的组件或第三方库提供时，可以用此方法自己来实现一个</center>
categories:
 - ReactNative
tags: 进阶
updated: 2017-11-14 00:00:00
---

## 前言

> 有时候可能由于业务的需求需要实现某些功能，但因为React Native上可能还没有相应的组件或第三方库提供使用，这时候可以通过访问原生平台上的API接口，去实现一些React Native上还没有的功能。

## 链接原生模块步骤

### 打开项目文件夹`/android/app/java/`目录下的包新增两个类。

#### （1）一是`MyNativeModule`类（名字自定义）并继承`ReactContextBaseJavaModule`，用来存放需要在React Native中调用的方法。如下图所示：
#### （2）一是`MyReactPackage`类（名字自定义）并实现接口`ReactPackage`包管理器，并把在第一步中创建的类加到原生模块`NativeModule`列表里。如下图所示：
![](https://upload-images.jianshu.io/upload_images/8154981-42830f02e8def8e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### （3）把在第二步中创建的包管理器添加到`ReactPackage`列表里（指在`getPackage`方法里面）

#### （4）在React Native中调用原生模块。

`MyNativeModule.java`代码如下：

```java
package com.connectnative;

import android.widget.Toast;
import android.content.Context;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;

/**
 * Created by can on 2017/11/14.
 */

public class MyNativeModule extends ReactContextBaseJavaModule {


    private Context mContext;

    public MyNativeModule(ReactApplicationContext reactContext) {
        super(reactContext);

        mContext = reactContext;
    }

    @Override
    public String getName() {

        //返回的这个名字是必须的，在rn代码中需要这个名字来调用该类的方法。
        return "MyNativeModule";
    }


    //函数不能有返回值，因为被调用的原生代码是异步的，原生代码执行结束之后只能通过回调函数或者发送信息给rn那边。


    @ReactMethod    //注解@ReactMethod是RN中要被调用的方法
    public void rnCallNative(String msg){

        Toast.makeText(mContext,msg,Toast.LENGTH_SHORT).show();

    }



}
```

MyReactPackage.java代码如下：

```java
package com.connectnative;

import com.facebook.react.ReactPackage;
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.uimanager.ViewManager;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;


public class MyReactPackage implements ReactPackage {

    @Override
    public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {

        List<NativeModule> modules=new ArrayList<>();
        //将我们创建的类添加进原生模块列表中
        modules.add(new MyNativeModule(reactContext));
        return modules;
    }

    @Override
    public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {

        //返回值需要修改
        return Collections.emptyList();
    }
}
```

`MainApplication.java`代码如下：

```java
package com.connectnative;

import android.app.Application;

import com.facebook.react.ReactApplication;
import com.facebook.react.ReactNativeHost;
import com.facebook.react.ReactPackage;
import com.facebook.react.shell.MainReactPackage;
import com.facebook.soloader.SoLoader;

import java.util.Arrays;
import java.util.List;

public class MainApplication extends Application implements ReactApplication {

    private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
        @Override
        public boolean getUseDeveloperSupport() {
            return BuildConfig.DEBUG;
        }

        @Override
        protected List<ReactPackage> getPackages() {
            return Arrays.<ReactPackage>asList(
                    new MainReactPackage(),
                    //将我们创建的包管理器给添加进来
                    new MyReactPackage()
            );
        }

        @Override
        protected String getJSMainModuleName() {
            return "index";
        }
    };

    @Override
    public ReactNativeHost getReactNativeHost() {
        return mReactNativeHost;
    }

    @Override
    public void onCreate() {
        super.onCreate();
        SoLoader.init(this, /* native exopackage */ false);
    }
}
```

MainActivity.java代码如下：

```java
package com.connectnative;

import com.facebook.react.ReactActivity;

public class MainActivity extends ReactActivity {

    /**
     * Returns the name of the main component registered from JavaScript.
     * This is used to schedule rendering of the component.
     */
    @Override
    protected String getMainComponentName() {
        return "ConnectNative";
    }
}
```

接着需要在RN中`import`原生模块

![img](https://upload-images.jianshu.io/upload_images/8154981-cf047a64eb5ac83b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后在相应的位置调用即可：

```jsx
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, {Component} from 'react';
import {
    Platform,
    StyleSheet,
    Text,
    View,
    NativeModules,
} from 'react-native';

const instructions = Platform.select({
    ios: 'Press Cmd+R to reload,\n' +
    'Cmd+D or shake for dev menu',
    android: 'Double tap R on your keyboard to reload,\n' +
    'Shake or press menu button for dev menu',
});

export default class App extends Component<{}> {
    render() {
        return (
            <View style={styles.container}>
                <Text style={styles.welcome}
                      onPress={this.connectNative.bind(this)}>
                    Welcome to React Native!
                </Text>
                <Text style={styles.instructions}>
                    To get started, edit App.js
                </Text>
                <Text style={styles.instructions}>
                    {instructions}
                </Text>
            </View>
        );
    }
    connectNative(){

        NativeModules.MyNativeModule.rnCallNative('调用原生方法的Demo!!!');
    }
}



const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
    },
    welcome: {
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
    },
    instructions: {
        textAlign: 'center',
        color: '#333333',
        marginBottom: 5,
    },
});
```

## 总结&理清流程

> - (1)在配置文件`AndroidManifest.xml`中，`android:name=".MainApplication”`，则`MainApplication.java`会执行。
> - (2)在`MainApplication.java`中，有我们创建的包管理器对象。程序加入`MyReactPackage.java`中。
> - (3)在`MyReactPackage.java`中，将我们自己创建的模块加入了原生模块列表中，程序进入`MyNativeModule.java`中。
> - (4)在`MyNativeModule.java`中，有我们需要被复用的原生方法`rnCallNative( )`。