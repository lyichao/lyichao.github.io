---
title: Android开发-MVP框架学习记录
description: <center>记录MVP是什么？有什么优势和劣势？使用登录验证demo来记录MVP</center>
categories:
 - Android
tags: 框架 进阶
updated: 2019-01-21 17:30:08
---

## 前言

> `MVP`架构流行已久，但由于实在没时间（太懒），所以一直对`MVP`停留在听说一词上。
>
> 由于最近公司让我接收一个android项目，而该项目之前的android版本就是使用`MVP`架构，因此得以真正接触到了`MVP`架构。
>
> **本文章Demo**：[点击下载](https://codeload.github.com/lyichao/MVPDemo/zip/master)



## 首先，我们来理解下MVP是什么？

> `MVP`全称`Model-View-Presenter`，它把项目大概划分为3个模块，其实分别对应：
>
> M层：实体层，负责获取实体数据。
>
> V层：视图层，对应`XML`文件与`Activity`/`Fragment`。
>
> P层：逻辑控制层，同时持有`View`和`Model`对象。
>
> `MVP`的流程图大致如下所示：
>
> ![image-20190121174157334](https://upload-images.jianshu.io/upload_images/8154981-7550cb8dd1bcaaa9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 那么，MVP有什么优势？

> 1. 把业务逻辑抽离到`Presenter`层中，`View`层专注于UI的处理。
>
> 2. 分离视图逻辑与业务逻辑，达到解耦的目的。
>
> 3. `Presenter`被抽象成接口，可以根据`Presenter`的实现方式进行单元测试。
>
> 4. 提高代码的阅读性。
>
> 5. 可拓展性强。

## 同样，有优势就有缺点！

> 1. 项目结构会对后期的开发和维护有一定影响，具体视APP的体量而定。
>
> 2. 代码量会增多，如何避免编写过多功能相似的重复代码是使用`MVP`开发的一个重要处理问题。
>
> 3. 有一定的学习成本。

## 趁热打铁，撸起袖子就是干！

> 下面通过输入账号密码实现登录的例子，来看看`MVP`到底是如何分工（甩锅）合作的。



### 创建一个空项目，名为MVPDemo好了。然后在项目下的mvpdemo文件夹中新增`model`、`view`、`presenter`三个文件夹，分别对应实体层、视图层、逻辑控制层。然后在`view`文件夹中创建`activity`文件夹，并把`MainActivity`拖至其中。如下图所示：

![image.png](https://upload-images.jianshu.io/upload_images/8154981-cc6cc0ad1c6712d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 在`activity_main.xml`中添加三个组件，用于输入账号密码和登录

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:app="http://schemas.android.com/apk/res-auto"
      xmlns:tools="http://schemas.android.com/tools"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      tools:context=".view.activity.MainActivity">
  
      <EditText
          android:id="@+id/uid"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_alignParentTop="true"
          android:layout_centerHorizontal="true"
          android:layout_marginTop="157dp"
          android:ems="10"
          android:hint="账号"
          android:inputType="textPersonName"
           />
  
      <EditText
          android:id="@+id/pwd"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_alignParentBottom="true"
          android:layout_centerHorizontal="true"
          android:layout_marginBottom="219dp"
          android:ems="10"
          android:hint="密码"
          android:inputType="textPassword" />
  
      <Button
          android:id="@+id/login"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_alignParentBottom="true"
          android:layout_centerHorizontal="true"
          android:layout_marginBottom="128dp"
          android:text="登录" />
  ```

### 回到`MainActivity`中，绑定三个UI组件  

  ```java
  public class MainActivity extends AppCompatActivity {
  
      private EditText mUid,mPwd;
      private Button mLogin;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          initUI();       //初始化UI
          initListener(); //初始化监听事件
      }
  
      private void initUI() {
          mUid.findViewById(R.id.uid);
          mPwd.findViewById(R.id.pwd);
          mLogin.findViewById(R.id.login);
      }
  
      private void initListener() {
          mLogin.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
  
              }
          });
      }
  }
  ```

### 接下来就算是开始用到`MVP`了，先来分析下登录操作，因为输入账号密码后需要进行登录操作，而登录操作可以看作有那么几步：

  > 1.登录验证失败：***账号密码不能为空，账号密码不能错误***。如果存在这两种情况，则***弹出对应`Toast`***；
  >
  > 2登录验证成功：***弹出登录成功`Toast`***，并跳转到登录成功页面

  因此登录操作中既包括了***逻辑控制层操作***，也包括了***视图层操作***。

### 那么我们就需要创建对应的文件

  > 1.在`presenter`文件夹下创建接口类`LoginActivityPresenter`和`impl`文件夹;
  >
  > 2.在`LoginActivityPresenter`接口中新建一个登录方法；
  >
  > 3.接着在`impl`文件夹内创建`LoginPresenterImpl`类并实现`LoginActivityPresenter`接口的登录方法；

![image.png](https://upload-images.jianshu.io/upload_images/8154981-534f89bcb21313dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




  ```java
  // 2.
  package com.example.lee.mvpdemo.presenter;
  public interface LoginActivityPresenter {
      void login(String uid, String pwd);
  }
  ```

  



```java
// 3.
package com.example.lee.mvpdemo.presenter.impl;
import com.example.lee.mvpdemo.presenter.LoginActivityPresenter;
public class LoginPresenterImpl implements LoginActivityPresenter {
    @Override
    public void login(String uid, String pwd) {
    }
}

```

### 在`LoginPresenterImpl`中的完善`login`方法，其中，因为需要在验证操作后通知UI层弹出`Toast`操作，所以需要引入视图层的方法。所以在完善`login`方法前，先来添加视图层需要的方法：

> 1.在`view`文件夹下创建`iView`文件夹
>
> 2.在`iView`文件夹新建一个接口类`ILoginActivity`
>
> 3.在接口类`ILoginActivity`中添加两个方法，一个用于验证成功后返回，一个用于验证失败后返回

![image.png](https://upload-images.jianshu.io/upload_images/8154981-f31687d3f07b81c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




```java
// 3.
package com.example.lee.mvpdemo.view.iView;
public interface ILoginActivity {
    //用于验证成功后返回
    void loginSuccess(int resultCode, String msg);
    //用于验证成失败后返回
    void loginFailed(int resultCode, String msg);
}
```

### 万事俱备，只欠`login`。我们回到`LoginPresenterImpl`类，往里面引入刚刚添加的接口类`ILoginActivity`，并完善登录验证失败/成功时的代码



```java
package com.example.lee.mvpdemo.presenter.impl;
import android.text.TextUtils;
import com.example.lee.mvpdemo.presenter.LoginActivityPresenter;
import com.example.lee.mvpdemo.view.iView.ILoginActivity;
public class LoginPresenterImpl implements LoginActivityPresenter {
    
    private ILoginActivity mIView;
    public  LoginPresenterImpl(ILoginActivity iLoginActivity){
        mIView = iLoginActivity;
    }
    @Override
    public void login(String uid, String pwd) {
        System.out.println("触发：login"+" "+"uid:"+uid+" "+"pwd:"+pwd);
        if(TextUtils.isEmpty(uid)){
            //此时账号为空，告诉视图层需要展示调取loginFailed方法
            mIView.loginFailed(0,"账号不能为空");
        }else if(TextUtils.isEmpty(pwd)){
            //此时密码为空，告诉视图层需要展示调取loginFailed方法
            mIView.loginFailed(0,"密码不能为空");
        }else if(uid.equals("chaoiscool")  && pwd.equals("123456")){
            //此时验证成功，告诉视图层需要展示调取loginSuccess方法；
            //如果有保存用户信息的需要，也可以在此处告诉实体层，让它去进行保存用户信息，
            //操作和第6步似，例如：
            //mModel.saveUserBean(userBean)
            mIView.loginSuccess(1,"登录成功");
        }else {
            //此时账号或者密码错误，告诉视图层需要展示调取loginFailed方法
            mIView.loginFailed(1,"登录失败");
        }
    }
}
```

### 现在基本的设置已经完成，我们把注意力移到`MainActivity`身上，此时的`MainActivity`还是独自一人，还没和逻辑控制层、实体层和视图层建立联系。因此我们来给他们搭建一座桥梁：

>1.引入逻辑控制层`LoginPresenterImpl`

```java
private LoginPresenterImpl mLoginPresenter;
```

> 2.为`LoginPresenterImpl`新建实例对象

```java
private LoginActivityPresenter getPresenter(){
   if(null == mLoginPresenter){
       mLoginPresenter = new LoginPresenterImpl(this);
   }
   return mLoginPresenter;
}
```

> 3.触发登录事件时，调用`LoginPresenterImpl`（逻辑控制层）里的`login`方法

```java
private void initListener() {
    mLogin.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            System.out.println("触发：onClick");
            getPresenter().login(mUid.getText().toString(),mPwd.getText().toString());
        }
    });
}
```

> 4.实现接口类`ILoginActivity`（视图层）的两个方法,并在各自的中实现对应的逻辑

```java
public class MainActivity extends Activity implements ILoginActivity
```

```java
//登录成功回调
@Override
public void loginSuccess(int resultCode, final String msg) {
    System.out.println("触发：loginSuccess");
    System.out.println("触发：msg:"+msg);
    runOnUiThread(new Runnable() {
        @Override
        public void run() {
            if(!TextUtils.isEmpty(msg)){
                Toast.makeText(MainActivity.this,msg,Toast.LENGTH_SHORT).show();
            }
        }
    });
}

//登录失败回调
@Override
public void loginFailed(int resultCode, final String msg) {
    System.out.println("触发：loginFailed");
    System.out.println("触发：msg:"+msg);
    runOnUiThread(new Runnable() {
        @Override
        public void run() {
            if(!TextUtils.isEmpty(msg)){
                Toast.makeText(MainActivity.this,msg,Toast.LENGTH_SHORT).show();
            }
        }
    });
}
```

> 5.同样的，如果有保存用户数据的需要，也需要实现前面3、4的操作

## 总结

到这里为止，MVP的分工合作就已经配置完成。现在也应该对MVP的操作有一定程度的了解了。其

<mark>核心是逻辑控制层，所有的UI操作和数据操作，都是经过逻辑控制层来分派的。它的存在就像是你的BOSS，分派不同的任务给下属，专业的事情由专业的人来干！</mark>

最后放上效果图：

![img](https://upload-images.jianshu.io/upload_images/8154981-98469f34d412b4cf?imageMogr2/auto-orient/strip)