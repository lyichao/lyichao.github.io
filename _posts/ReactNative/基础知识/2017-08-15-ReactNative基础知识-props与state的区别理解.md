---
title: ReactNative基础知识-props与state的区别理解
description: <center>emmmm....算是ReactNative必须掌握的东西</center>
categories:
 - ReactNative
tags: 
updated: 2019-01-25 17:30:08
---

## props与state的区别理解

>通俗理解：一个只可读(props)，一个可读可写(state)

先来看props

```javascript
function Hello(props){
    return <div>hello world  +  {props.name}</div>
}

React.render(
    <Hello name="zhangshan"/>,
    document.getElementById('app')
)

```

https://www.jianshu.com/p/1b32ceb8295c

