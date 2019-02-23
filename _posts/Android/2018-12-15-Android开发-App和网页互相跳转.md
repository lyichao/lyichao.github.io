---
title: Android开发-App和网页互相跳转
description: <center>很常见，记录一下吧</center>
categories:
 - Android
tags: 基础知识 
updated: 2018-12-15 00:00:00
---

## 前言

> 这种功能在很多大型App上都有出现，像京东、QQ、淘宝都很容易发现。外部浏览器调起App，App跳转外部浏览器网页。不得不说，这样的操作就有很强的产品塑造性了。

## 具体实现

- App跳转外部浏览器网页

  这个实现很简单，两三行代码的事情，不复杂！

  > 1.获取需要打开的uri  
  >
  > 2.通过隐性intent唤起浏览器

  ```java
  Uri uri = Uri.parse("https://www.baidu.com");
  Intent intent = new Intent(Intent.ACTION_VIEW, uri);
  startActivity(intent);
  ```

- 外部浏览器调起App

  这个实现要先来说说URI原理性知识。Android上的URI主要有三部分组成，`scheme`, `authority`和`path`。其中`authority`又分为`host`和`port`，具体格式如下所示：

  ```java
  scheme://host:port/path
  ```

  ![image.png](https://upload-images.jianshu.io/upload_images/8154981-75a9bdd4b0c95570.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  举个具体的例子：

  假设这是我们用来调起App的网页html代码

  ```html
  <html>
  <meta charset="UTF-8">
     <body>
       <h1>Test Scheme</h1> <!--自动加载隐藏页面跳转-->
        <!--手动点击跳转-->
        <a href="union://myunion:7380/ucar/user">点击打开APP</a>
     </body>
  </html>
  ```

  然后在App入口Activity（我这里是MainActivity）的注册清单`AndroidManifest.xml`中配置`intent-filter`，指定App要接收到的`host`和`scheme`，具体代码如下：

  ```xml
          <activity
              android:name="com.uu.genauction.view.activity.MainActivity"
              android:screenOrientation="sensorPortrait">
              <intent-filter>
                  <action android:name="android.intent.action.MAIN" />
                  <category android:name="android.intent.category.LAUNCHER" />
              </intent-filter>
              <intent-filter>
                  <!-- 协议部分配置 ,要在web配置相同的-->
                  <data
                      android:host="myunion"
                      android:scheme="union"/>
                  <category android:name="android.intent.category.DEFAULT"/>
                  <category android:name="android.intent.category.BROWSABLE"/>
                  <action android:name="android.intent.action.VIEW"/>
              </intent-filter>
          </activity>
  ```

  最后，当我们点击html的时候，实际上就是打开URI`union://myunion:7380/ucar/user`。所以当与App上事先配置的信息一致时，就能够调起我们的App。

  另外，如果想把网页的信息带过去App的话，可以在URI中的path中添加参数，添加方式是`?key=value`。例如：

  ```java
  union://myunion:7380/ucar/user?name=helloworld
  ```

  然后App端接收传过来的数据

  ```java
    protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          mTextView= (TextView) findViewById(R.id.tv_main_show);
          Intent intent = getIntent();
          Uri uri=intent.getData();
          if(uri!=null){
              String name=uri.getQueryParameter("name");
              String scheme= uri.getScheme();
              String host=uri.getHost();
              String port=uri.getPort()+"";
              String path=uri.getPath();
              String query=uri.getQuery();
              mTextView.setText("获得的数据name="+name+"/r"+"scheme"+scheme+"/r"+"host" +
                      "host"+host+"/r"+"port"+port+"/r"+"path"+path+"/r"+"query"+query);
          }
    }
  ```

