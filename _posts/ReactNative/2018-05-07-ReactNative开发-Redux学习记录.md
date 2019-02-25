---
title: ReactNative开发-Redux学习记录
description: <center>学得怀疑人生系列之记录😳</center>
categories:
 - ReactNative
tags: 框架 进阶
updated: 2019-02-20 00:00:00
---

## 前言

> `Redux`这东西……真是令人又爱又恨。前前后后看了不下10篇文章+官方文档来理解这东西。差点就从入门到放弃。不过框架这东西，百变不离其中，懂得**分工合作**的思想后，什么`MVC`，`MVP`，`MVVC`都不在话下。借助这篇记录，来巩固下自己学习和使用`Redux`的过程。
>
> **本文章配套Demo**：[点我下载](<https://codeload.github.com/lyichao/ReduxDemo/zip/master>)

## 什么是Redux?

> - **官方说法**：`Redux`是一个状态容器管理工具。将所有状态进行统一管理，适用于多交互，多数据的场景。
>
> - **我的理解**：我们可以把采用了`Redux`后的应用看作是一个`component`，整个应用只有一个`state`,所有页面都可以拿到这个`state`中的内容。当这个`state`中的内容发生改变的时候，与之关联的页面也会跟着刷新状态。

## 基本概念

> 首先，在未使用Redux的项目中，组件（父组件、子组件、孙子组件）间都各自含有的`state`和`props`属性。而且`props`是从父级组件上**分发下来**的属性，只能是从上往下走；而`state`是组件**内部**自行管理的状态属性。因此整个应用并没有数据向上回溯的能力。要么从上面单向得到并向下级分发，要么自行内部消化刷新页面状态。正如下图所示：![image.png](https://upload-images.jianshu.io/upload_images/8154981-fdf3c02ac9c6db0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> 因此，一但应用场景的页面较多，交互复杂的时候，每次都去通过`state`重新刷新页面引起页面变化，就会出现卡卡卡卡卡的情况。
>
> 那么，采用了`Redux`后又会怎样呢？先来看一下App结构图：
>
> ![image.png](https://upload-images.jianshu.io/upload_images/8154981-f6db3205e483de42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> 最后，从上图可以看到采用Redux后的App结构比没采用Redux时在最外包裹了一层`Provider`。接下来我们就从这个`Prodiver`开始，逐个解析`Store`、`Action`和`Reducer`。
>
> - **Provider**:**整个App的容器，位于App的最外层。能够使得原有组件变得接受Redux的`store`作为props。可以理解为所有的页面都是它的子组件，这样就能够像父组件向子组件传递props。**
>
>   例如想通过改变孙子组件3的同时刷新孙子组件2的页面状态，这时后就可以先把孙子组件3要改变的状态通知告诉`Provider`，然后再由`Provider`刷新孙子组件2的页面状态。这样就能使得整个App内的组件都拥有数据往上回溯的能力。
>
> - **Store**：**存放和管理App内所有组件State的地方。**
>
>   整个App只允许有一个`Store`对象，改变了`Store`中的state就可以实现我们改变UI的操作。
>
> -  **Action**:**用户触发或程序触发的一个普通对象。**
>
>   State 的变化，会导致View的变化。但是，用户接触不到State，只能接触到View。所以，State的变化必须是View导致的。Action就是View 发出的通知，表示State应该要发生变化了。例如要实现登录操作，就要发起登录action，当登录成功的就返回View显示登录成功的状态和信息，登录失败就返回View显示登录失败的状态和信息。
>
> - **Reducer**:**根据`action`操作来做出不同的数据响应，返回一个新的`state`。**

## Redux状态管理流程

> ![image.png](https://upload-images.jianshu.io/upload_images/8154981-67c5bdc3f12e01f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> <mark>梳理一下流程就是：用户触发action→部署触发的action后通知reducer→reducer→新store→反馈到UI上更新页面</mark>

## 实战案例：App登录操作

> 说了一大堆原理和废话后，还是得看实战案例。不然脑袋记不住。

### 实例需求

> 我们要做一个App登录操作，能让用户输入账号密码登录。登录成功就弹出提醒登录成功，跳转到主页并显示出当前登录用户的信息。登录失败就弹出登录失败提醒。如果用户不登录也可以选择跳过登录，直接跳转到主页并弹出欢迎游客信息。

### 需求分析

> 通过需求，我们提炼出其中重要的点：
>
> - **登录成功状态**
> - **登录失败状态**
> - **跳过登录**

### 具体操作

#### 步骤一：创建

> 创建一个名为ReduxDemo的空项目。
>
> 打开终端，输入
>
> ```
> react-native init ReduxDemo --version 0.48.1
> ```

#### 步骤二：安装

> 进入项目根目录，在终端输入命令安装redux、react-navigation（用作跳转页面）、redux-thunk（稍后讲解）和redux-logger（稍后讲解）
>
> ```
> npm install --save redux
> npm install --save react-redux
> npm install --save react-navigation
> npm install --save redux-thunk
> npm install --save redux-logger
> npm install --save react-native-root-toast
> ```

#### 步骤三：构建目录结构

> 在根目录下新建一个app文件夹，然后在这个文件夹下再分别创建`actions`、`contants`、`pages`、`reducers`、`store`文件夹。如下图所示：
>
> ![image.png](https://upload-images.jianshu.io/upload_images/8154981-28382591193c55a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 步骤四：统一入口，利用react-navigation实现页面跳转

> 修改`index.android.js`和`index.ios.js`文件，两者都改成如下内容：
>
> ```jsx
> import React, { Component } from 'react';
> import {
>     AppRegistry,
> } from 'react-native';
> import App from './app/app';
> AppRegistry.registerComponent('ReduxDemo', () => App);
> ```
>
> 在app文件下创建app.js文件，并添加一下内容：
>
> ``````jsx
> import React from 'react';
> import {
>     StackNavigator,
>     TabNavigator
> } from 'react-navigation';
> import Login from './pages/Login';
> import Home from './pages/Home';
> 
> const App = StackNavigator({
>     Login:{screen:Login},
>     Home:{screen:Home}
> },{
> });
> 
> export default App;
> ``````
>
> 接着在`pages`文件夹下添加登陆页和首页：
>
> ![image.png](https://upload-images.jianshu.io/upload_images/8154981-9f30297b3ab38a5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> 接着继续在Login文件夹下的index.js文件里添加如下内容：
>
> ```jsx
> 
> import React, { Component } from 'react';
> import {
>     AppRegistry,
>     StyleSheet,
>     Text,
>     View,
>     Dimensions,
>     TextInput,
>     TouchableOpacity
> } from 'react-native';
> 
> import {NavigationActions} from 'react-navigation'
> const screenWidth = Dimensions.get('window').width;  //屏幕的宽度
> const screenHeight = Dimensions.get('window').height;  //屏幕的高度
> 
> export default class Login extends Component {
> 
>     static navigationOptions = {
>         header: null
>     }
> 
>     constructor(props) {
>         super(props);
>         this.state = {
>             uid:'',
>             pwd:'',
>         };
>     }
> 
>     render() {
>         return (
>             <View style={styles.container}>
>                 <TextInput
>                     style = {styles.textInput}
>                     blurOnSubmit={true}
>                     returnKeyType="done"
>                     placeholder = '请输入账号'
>                     selectionColor = "#bac3d4"
>                     placeholderTextColor = '#bac3d4'
>                     underlineColorAndroid = "transparent"
>                     onChangeText={(text)=>{this.setState({uid:text})}}
>                     value={this.state.uid}/>
> 
>                 <TextInput
>                     style = {styles.textInput}
>                     blurOnSubmit={true}
>                     returnKeyType="done"
>                     placeholder = '请输入密码'
>                     selectionColor = "#bac3d4"
>                     placeholderTextColor = '#bac3d4'
>                     underlineColorAndroid = "transparent"
>                     onChangeText={(text)=>{this.setState({pwd:text})}}
>                     value={this.state.pwd}/>
> 
>                 <View style={styles.btn}>
>                     <TouchableOpacity onPress={this.login}
>                                       style={styles.radiusBtn}>
>                         <Text style={styles.text}>登 录</Text>
>                     </TouchableOpacity>
>                     <TouchableOpacity onPress={this.skip}
>                                       style={styles.radiusBtn}>
>                         <Text style={styles.text}>跳 过</Text>
>                     </TouchableOpacity>
>                 </View>
> 
>             </View>
>         );
>     }
> 
>     login = () => {
>         this.props.navigation.navigate('Home')
>     }
> 
>     skip = () => {
>         this.props.navigation.navigate('Home')
>     }
> }
> 
> const styles = StyleSheet.create({
>     container: {
>         flex: 1,
>         justifyContent: 'center',
>         alignItems: 'center',
>         backgroundColor: '#F5FCFF',
>     },
>     textInput:{
>         width:0.8*screenWidth,
>         height:0.08*screenHeight,
>         color:'#1b6cec',
>         fontSize:16,
>         borderWidth: 1,
>         borderRadius:8,
>         borderColor:'#1b6cec',
>         marginBottom: 15
>     },
>     btn:{
>         height:0.25*screenHeight,
>         justifyContent:'center',
>         alignItems:'center'
>     },
>     radiusBtn:{
>         backgroundColor:'#1b6cec',
>         width:0.8*screenWidth,
>         height:0.08*screenHeight,
>         borderRadius:25,
>         justifyContent:'center',
>         alignItems:'center',
>         marginBottom:8
>     },
>     text:{
>         color:'#fff',
>         fontSize:16
>     }
> });
> ```
>
> 接着继续在Home文件夹下的index.js文件里添加如下内容：
>
> ```jsx
> import React, { Component } from 'react';
> import {
>     AppRegistry,
>     StyleSheet,
>     Text,
>     View
> } from 'react-native';
> 
> export default class Login extends Component {
> 
>     static navigationOptions = {
>         header: null
>     }
> 
>     render() {
>         return (
>             <View style={styles.container}>
>                 <Text style={styles.welcome}>
>                     Welcome to Home!
>                 </Text>
>                 <Text style={styles.instructions}>
>                     To get started, edit index.android.js
>                 </Text>
>                 <Text style={styles.instructions}>
>                     Double tap R on your keyboard to reload,{'\n'}
>                     Shake or press menu button for dev menu
>                 </Text>
>             </View>
>         );
>     }
> }
> 
> const styles = StyleSheet.create({
>     container: {
>         flex: 1,
>         justifyContent: 'center',
>         alignItems: 'center',
>         backgroundColor: '#F5FCFF',
>     },
>     welcome: {
>         fontSize: 20,
>         textAlign: 'center',
>         margin: 10,
>     },
>     instructions: {
>         textAlign: 'center',
>         color: '#333333',
>         marginBottom: 5,
>     },
>     textInput:{
>         width:0.8*screenWidth,
>         height:0.08*screenHeight,
>         color:'#1b6cec',
>         fontSize:16,
>         borderWidth: 1,
>         borderRadius:8,
>         borderColor:'#1b6cec',
>         marginBottom: 15
>     }
> });
> ```
>
> 到这里为止，App已经可以进行页面跳转啦。

#### 步骤五：配置Redux

> **1.**正如前面所说的，我们需要为使用了Redux的App项目最外层包裹一层`Provider`。因此在app文件夹下添加一个`index.js`文件，并使用`Provider`包裹整个程序的入口，并将`store`传递进去。代码如下：
>
> ```jsx
> import { AppRegistry,View,Text ,} from 'react-native';
> import React, { Component } from 'react';
> import {Provider} from 'react-redux';
> import configureStore from './store/ConfigureStore';
> // 调用 store 文件中的rootReducer常量中保存的方法
> const store = configureStore();
> import app from './app';
> 
> // 项目中使用了react-navigation，推荐的做法是将初始文件写在一个文件中，
> // 所以app.js也可以看做是页面的初始化入口
> export default class Root extends Component {
>  render() {
>      return (
>          //包裹app
>          <Provider store={store}>
>              <app />
>          </Provider>
>      );
>  }
> };
> 
> AppRegistry.registerComponent('ReduxDemo', () => Root);
> ```
>
> **2.**然后为了能让程序运行时首先索引到app文件夹下的`index.js`文件，需要把`index.android.js`和`index.ios.js`文件修改成以下内容：
>
> ```jsx
> require('./app/index');
> ```
>
> **3.**紧接着为`store`配置参数设置。在`store`文件夹下创建`ConfigureStore`文件，并添加以下内容：
>
> ```jsx
> // redux库里面提供的方法，创建store和middleware中间件
> import {createStore , applyMiddleware} from 'redux';	
> import thunk from 'redux-thunk';
> import logger from 'redux-logger';
> import RootReducer from '../reducers/RootReducer';
> 
> let middlewares = [];
> 
> middlewares.push(thunk);
> if (__DEV__) {
>  middlewares.push(logger);
> }
> // 通过applyMiddleware将中间件添加
> const createStoreWithMiddleware = applyMiddleware(...middlewares)(createStore);
> // 导出configureStore，里面携带着reducer，中间件，初始值
> export default function configureStore(initialState){
>  return createStoreWithMiddleware(RootReducer,initialState);
> }
> ```
>
> 这里涉及到了新的知识：**中间件**，redux-thunk和redux-logger 。这里简单说下中间件的概念和代码中的两个是干什么用的：
>
> > 中间件概念：用于为使用了Redux的项目添加额外的功能，如日志打印（redux-logger），异步请求（redux-thunk）等。
>
> 因为使用的中间件都是现成的，我们直接引入就好，这里就不再展开讲解了。
>
> **4.**然后继续`reducers`文件夹下创建一个`RootReducer.js`文件，（由于Redux中只允许有一个`store`，当业务越来越庞大的时候，我们就需要拆分出N个`reducer`。这时候，就需要把这N个`reducer`组合起来，因此我们需要一个`根reducer`。），并添加以下代码：
>
> ```jsx
> import { combineReducers } from 'redux';
> import LoginReducer from './LoginReducer';
> 
> //取决于这里你加入了多少 reducer
> export default RootReducer = combineReducers({
>  LoginReducer:LoginReducer
> });
> ```
>
> **5.**在`RootReducer.js`中我们引入了一个`LoginReducer`(处理由用户或程序触发`LoginAction`后,分派过来的任务并根据对应的`types`，修改更新`store`中的某些值)，因此我们也需要在`reducers`文件夹下创建一个`LoginReducer.js`并添加以下代码：
>
> ```jsx
> // ActionTypes里面存放着App中可能触发的事件
> import * as types from './../contants/ActionTypes';
> // 初始化值
> const initialState = {
>     showMsg:'',
>     userName:'',
>     tel:''
> };
> // 导出LoginReducer。
> export default function LoginReducer(state = initialState, action){
>     // 通过switch来判断types的值，在action中实现功能。
>     switch (action.type) {
>         // 当type=LOGIN_SUCCESS时，会将action中的值，
>         // 赋值给showMsg、userName和tel。在Login文件夹下的index.js中就能拿到
>         // showMsg、userName和tel的值。
>         case types.LOGIN_SUCCESS:
>             return Object.assign({}, state, {
>                 showMsg:action.showMsg,
>                 userName:action.userName,
>                 tel:action.tel
>             });
>         case types.LOGIN_FAIL:
>             return Object.assign({}, state, {
>                 showMsg:action.showMsg,
>                 userName:action.userName,
>                 tel:action.tel
>             });
>         case types.SKIP_LOGIN:
>             return Object.assign({}, state, {
>                 showMsg:action.showMsg,
>                 userName:action.userName,
>                 tel:action.tel
>             });
>         default:
>             return state;
>     }
> }
> ```
>
> **6.**继续，我们需要在`contants`文件夹下创建`ActionTypes.js`，为我们的登录事件添加几种状态：
>
> ```jsx
> // 登录成功
> export const LOGIN_SUCCESS = 'LOGIN_SUCCESS';
> // 登录失败
> export const LOGIN_FAIL = 'LOGIN_FAIL';
> //跳过登录
> export const SKIP_LOGIN = 'SKIP_LOGIN';
> ```
>
> **7.**然后跟着需要创建`action事件`，简单说就是用户或程序触发了A事件，而这个A事件就需要去处理对应的操作。像用户触发登录操作，那么这个就是`LoginAction`，具体操作就是拿到用户输入的账号密码后发起请求去验证密码正确与否。文章中用的就是登录案例，所以就在`actions`文件夹下创建`LoginAction.js`,并添加以下代码：
>
> ```jsx
> // 获取actionType中的全部状态，需要哪个就用哪个
> import * as types from './../contants/ActionTypes';
> 
> export function LoginAction(uid,pwd,flag) {
>     return dispatch => {
>         if(uid == 'lyichao' && pwd == '123456' && flag == 0){
>             dispatch(loginSuccess({
>                 showMsg:'LoginSuccess',
>                 userName:'lyichao',
>                 tel:'13888888888'
>             }));
>         }else if(uid == '' && pwd == '' && flag == 1){
>             dispatch(skipLogin({
>                 showMsg:'SkipLogin',
>                 userName:'',
>                 tel:''
>             }));
>         }else {
>             dispatch(loginFail({
>                 showMsg:'LoginFail',
>                 userName:'',
>                 tel:''
>             }));
>         }
>     }
> };
> 
> function loginSuccess(data) {
>     return {
>         // type是必要参数，通过type值判断
>         type: types.LOGIN_SUCCESS,
>         ...data
>     };
> }
> function loginFail(data) {
>     return {
>         // type是必要参数，通过type值判断
>         type: types.LOGIN_FAIL,
>         ...data
>     };
> }
> function skipLogin(data) {
>     return {
>         type: types.SKIP_LOGIN,
>         ...data
>     };
> }
> ```
>
> 8.最后，我们还需要为页面和各个模块间进行关联。在`Login`文件夹下的`index.js`文件新增以下内容：
>
> ```jsx
> import React, { Component } from 'react';
> import {
>     AppRegistry,
>     StyleSheet,
>     Text,
>     View,
>     TextInput,
>     Dimensions,
>     TouchableOpacity
> } from 'react-native';
> import Toast from 'react-native-root-toast';
> import { connect } from 'react-redux';
> import { LoginAction } from '../../actions/LoginAction';
> import {NavigationActions} from 'react-navigation';
> const screenWidth = Dimensions.get('window').width;  //屏幕的宽度
> const screenHeight = Dimensions.get('window').height;  //屏幕的高度
> class Login extends Component {
> 
>     static navigationOptions = {
>         header: null
>     }
> 
>     constructor(props) {
>         super(props);
>         this.state = {
>             uid:'',
>             pwd:'',
>             skip:0, //0正常登录 1跳过登录
>         };
>     }
> 
>     componentWillReceiveProps(nextProps) {
>         console.log('componentWillReceiveProps');
>         console.log(nextProps);
>         // 每次值更新的时候，都会走这个方法，所以可以在这个方法里面添加判断，用来更新页面
>         const { userName,tel,showMsg } = nextProps.LoginReducer;
>         console.log("showMsg=",showMsg)
>         if(showMsg === 'LoginSuccess' ){
>             console.log(1);
>             Toast.show('登录成功', {duration: Toast.durations.SHORT});
>             this.props.navigation.navigate('Home',{userName:userName,tel:tel})
>         }else if(showMsg === 'LoginFail' ){
>             console.log(2);
>             Toast.show('登录失败', {duration: Toast.durations.SHORT});
>         }else{
>             console.log(3);
>             Toast.show('欢迎游客！', {duration: Toast.durations.SHORT});
>             this.props.navigation.navigate('Home',{userName:userName,tel:tel})
>         }
>     }
> 
>     render() {
>         return (
>             <View style={styles.container}>
>                 <TextInput
>                     style = {styles.textInput}
>                     blurOnSubmit={true}
>                     returnKeyType="done"
>                     placeholder = '请输入账号'
>                     selectionColor = "#bac3d4"
>                     placeholderTextColor = '#bac3d4'
>                     underlineColorAndroid = "transparent"
>                     onChangeText={(text)=>{this.setState({uid:text})}}
>                     value={this.state.uid}/>
> 
>                 <TextInput
>                     style = {styles.textInput}
>                     blurOnSubmit={true}
>                     returnKeyType="done"
>                     placeholder = '请输入密码'
>                     selectionColor = "#bac3d4"
>                     placeholderTextColor = '#bac3d4'
>                     underlineColorAndroid = "transparent"
>                     onChangeText={(text)=>{this.setState({pwd:text})}}
>                     value={this.state.pwd}/>
> 
>                 <View style={styles.btn}>
>                     <TouchableOpacity onPress={this.login}
>                                       style={styles.radiusBtn}>
>                         <Text style={styles.text}>登 录</Text>
>                     </TouchableOpacity>
>                     <TouchableOpacity onPress={this.skip}
>                                       style={styles.radiusBtn}>
>                         <Text style={styles.text}>跳 过</Text>
>                     </TouchableOpacity>
>                 </View>
> 
>             </View>
>         );
>     }
> 
>     login = () => {
>         console.log("login")
>         let {uid,pwd} = this.state;
>         this.props.LoginAction(uid,pwd,0);
>     }
> 
>     skip = () => {
>         console.log("skip")
>         this.props.LoginAction('','',1);
>     }
> 
> 
> }
> 
> const styles = StyleSheet.create({
>     container: {
>         flex: 1,
>         justifyContent: 'center',
>         alignItems: 'center',
>         backgroundColor: '#F5FCFF',
>     },
>     textInput:{
>         width:0.8*screenWidth,
>         height:0.08*screenHeight,
>         color:'#1b6cec',
>         fontSize:16,
>         borderWidth: 1,
>         borderRadius:8,
>         borderColor:'#1b6cec',
>         marginBottom: 15
>     },
>     btn:{
>         height:0.25*screenHeight,
>         justifyContent:'center',
>         alignItems:'center'
>     },
>     radiusBtn:{
>         backgroundColor:'#1b6cec',
>         width:0.8*screenWidth,
>         height:0.08*screenHeight,
>         borderRadius:25,
>         justifyContent:'center',
>         alignItems:'center',
>         marginBottom:8
>     },
>     text:{
>         color:'#fff',
>         fontSize:16
>     }
> });
> 
> export default connect((state) => {
>     const { LoginReducer } = state;
>     return {
>         LoginReducer,
>     };
> },{ LoginAction })(Login)
> ```
>
> `Home`文件夹下的`index.js`也要相应修改如下：
>
> ```jsx
> 
> import React, { Component } from 'react';
> import {
>     AppRegistry,
>     StyleSheet,
>     Text,
>     View
> } from 'react-native';
> 
> export default class Login extends Component {
> 
>     static navigationOptions = {
>         header: null
>     }
> 
>     constructor(props) {
>         super(props);
>         this.state = {
>             userName:this.props.navigation.state.params.userName,
>             tel:this.props.navigation.state.params.tel,
>         };
> 
>     }
> 
>     render() {
>         let {userName,tel} = this.state;
>         return (
>             <View style={styles.container}>
>                 <Text style={styles.welcome}>
>                     Welcome to Home!
>                 </Text>
>                 <Text style={styles.instructions}>
>                     {userName}
>                 </Text>
>                 <Text style={styles.instructions}>
>                     {tel}
>                 </Text>
>             </View>
>         );
>     }
> }
> 
> const styles = StyleSheet.create({
>     container: {
>         flex: 1,
>         justifyContent: 'center',
>         alignItems: 'center',
>         backgroundColor: '#F5FCFF',
>     },
>     welcome: {
>         fontSize: 20,
>         textAlign: 'center',
>         margin: 10,
>     },
>     instructions: {
>         textAlign: 'center',
>         color: '#333333',
>         marginBottom: 5,
>     },
> });
> ```
>
> 大功告成，现在已经集成好Redux了！来看看效果～

### Demo效果演示

![untitled11.gif](https://upload-images.jianshu.io/upload_images/8154981-99b85100a8f04614.gif?imageMogr2/auto-orient/strip)





