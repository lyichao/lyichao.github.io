---
title: ReactNative基础知识-props与state
description: <center>emmmm....算是ReactNative必须掌握的东西</center>
categories:
 - ReactNative
tags: 
updated: 2019-01-25 17:30:08
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
> ![image.png](https://upload-images.jianshu.io/upload_images/8154981-309a46ed2a8975bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 什么是state？

> <mark>state是组件持有的状态</mark>。如果想要修改组件持有的状态或者叫属性，那么就应该用`state`来更改。
>
> 一般情况下，我们需要在`constructor`构造方法中进行初始化`state`，然后在你想要修改更新的时候调用`setState`方法。
>
> ```jsx
> //初始化state
> constructor(props) {  
>   super(props);  
>   this.state = { 
>       showText: true 
>   };  
> } 
> 
> //使用setState修改showText的属性
> this.setState(()=>{
>     showText: false 
> })
> ```

