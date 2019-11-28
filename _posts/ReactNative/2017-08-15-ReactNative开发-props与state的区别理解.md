---
title: ReactNative开发-props与state的区别与理解
description: <center>emmmm....算是ReactNative必须掌握的东西</center>
categories:
 - ReactNative
tags: 基础知识
updated: 2019-02-18 00:00:00
---

## props与state

>### 通俗理解：一个只可读(props)，一个可读可写(state)

### 什么是props？

> <mark>props是组件自身的属性</mark>，一般用于嵌套的内外层组件中，负责传递信息（通常由父层组件向子层组件传递）`props`对象中的属性与组件的属性一一对应，不要直接去修改`props`中的属性的值。

#### 父类向子类参数的传递

```jsx
//子类
class Child extends Component{  
    constructor(props){
    	super(props)
    }
    render(){  
        return(  
            <Text>{this.props.name}</Text>
            <Text>{this.props.tel}</Text>  
        );  
    }  
}  
```

> 这里有两个注意点：
>
> - 在获取`props`的时候要注意下：在`function`和`class`里的写法是有所区别的
>
>   - 在`function`下获取`props`写作`{props.属性名}`
>   - 在`class`下获取`props`写作`{this.props.属性名}`
>   - 具体原因可以参考下JS中this的相关文章
>
> - 在`class`中获取`props`，`constructor`构造函数是必须要添加上的，并且要添加`super`关键字
>
>   ```jsx
>   constructor(props){
>    super(props)
>   }
>   ```

> 这里的`this.props.tel`和`this.props.name`是子类的属性。然后我们创建一个父类包含这个子类，然后利用这个父类向子类传递数据

```jsx
//父类
class Father extends Component{  
    constructor(props){
    	super(props)
    }
    render(){  
        return(  
            <View>  
                <Child 
                    name={小明}
                    tel={18912345678}
                    />  
            </View>  
        );  
    }  
}  
```

> 可以看到，父类中的`name`和`tel`是子类的属性，然后我们用父类的属性传递给子类，那么父类的属性就是`Child`。也就是说我们设置父类的属性时，它的子类`props`属性也会被赋值。
>
> ![image.png](http://lc-lf8Y5Iic.cn-n1.lcfile.com/9ee88e5ea5a65f0f0803/props%E4%B8%8Estate%E7%9A%84%E5%8C%BA%E5%88%AB%E4%B8%8E%E7%90%86%E8%A7%A3.png)

### 什么是state？

> <mark>state是组件持有的状态</mark>。如果想要修改组件持有的状态或者叫属性，那么就应该用`state`来更改。
>
> 一般情况下，我们需要在`constructor`构造方法中进行初始化`state`，然后在想要修改更新的时候调用`setState`方法。
>
> ```jsx
> //初始化state
> constructor(props) {  
>   super(props);  
>   this.state = { 
>    showText: true 
> };  
> } 
> 
> //使用setState修改showText的属性
> this.setState(()=>{
>  showText: false 
> })
> ```
>
> 但使用`setState`时值得注意的是：它存在一些的问题经常会导致初学者出错，核心原因就是因为这个 API 是异步的。
>
> 首先 `setState` 的调用并不会马上引起 `state` 的改变，并且如果你一次调用了多个 `setState` ，那么结果可能并不如你期待的一样。
>
> ```jsx
> handle() {
>   // 初始化 `count` 为 0
>   console.log(this.state.count) // -> 0
>   this.setState({ count: this.state.count + 1 })
>   this.setState({ count: this.state.count + 1 })
>   this.setState({ count: this.state.count + 1 })
>   console.log(this.state.count) // -> 0
> }
> ```
>
> 第一，两次的打印都为 0，因为 `setState` 是个异步 API，只有同步代码运行完毕才会执行。
>
> 第二，虽然调用了三次 `setState` ，但是 `count` 的值还是为 1。因为多次调用会合并为一次，只有当更新结束后 `state` 才会改变。
>
> 当然也可以通过以下方式来实现调用三次 `setState` 使得 `count` 为 3
>
> ```jsx
> handle() {
>   this.setState((prevState) => ({ count: prevState.count + 1 }))
>   this.setState((prevState) => ({ count: prevState.count + 1 }))
>   this.setState((prevState) => ({ count: prevState.count + 1 }))
> }
> ```
>
> 如果你想在每次调用 `setState` 后获得正确的 `state` ，可以通过如下代码实现
>
> ```jsx
> handle() {
>     this.setState((prevState) => ({ count: prevState.count + 1 }), () => {
>         console.log(this.state)
>     })
> }
> ```

