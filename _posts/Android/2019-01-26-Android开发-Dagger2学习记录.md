---
title: Android开发-Dagger2学习记录
description: <center>从入门到放弃</center>
categories:
 - Android
tags: 框架
updated: 2019-01-26 10:22:12
---

## 前言

>`Dagger2`是目前`Android`流行的框架搭配之一。所以，学起来！（P.s：看了好多好多的文章解读~）

## 原理解读

##### Dagger2是什么？

>**官方介绍：**
>
>>`A fast dependency injector for Java and Android.`
>>
>>**译：**Java和Android的快速依赖关系注入器。
>
>**个人理解：**`Dagger`就是用来创造出一个容器，所有需要被依赖的对象在`Dagger`的容器中实例化，并通过		
>
>​		   `Dagger`注入到合适的地方，实现解耦。

##### 什么是依赖注入？

>依赖注入，英文名`Dependency Injection`，简称DI。简单说就是目标类（目标类需要进行依赖初始化的类）中所依赖的其他的类的初始化过程，不是通过手动编码的方式创建，而是通过技术手段可以把其他的类的已经初始化好的实例自动注入到目标类中。（P.s：不明白没关系，看文章你就明白了！认真脸！）

## 优点

>- 增加开发效率，神曲重复的简单体力劳动
>- 更好的管理类实例
>- 解耦
>- 便于扩展和维护

## 缺点

>- 相对于其他框架，有一定学习成本，需要时间消化。我自己就看了好多好多文章。。。。

## 少废话！入正题！

>`Dagger2`提供的注解：`@Inject`、`@Module`、`@Component`、`@Provieds`
>
>#### `@Inject`：
>
>（1）注解在属性中表示该属性需要依赖注入，不能使用`private`修饰，示例代码表示需要注入属性
>
>​	`mBus`(Bus的车位)：
>
>```java
>@Inject
>Bus mBus;
>```
>
>（2）注解在方法中表示该方法需要依赖注入，不能是抽象方法，不能使用`private`修饰，示例代码表示	
>
>​	  需要注入方法`injectBus`:
>
>```java
>//@Inject
>Bus mBus;
>@Inject
>public void injectBus(Bus bus) {
>    mBus = bus;
>}
>```
>
>​	方法注入的参数同样由`Dagger2`容器提供，以上代码的目的与第一点介绍的属性注入一样，都是为
>
>了注入`mBus`，如果目的是注入属性的话，方法注入和属性注入基本没有区别，属性注入是`Dagger2`中使用最多的一个注入方式。
>   那么什么情况下应该使用方法注入？比如依赖需要this对象的时候，方法注入可以提供安全的this对象。
>   注意，`Dagger2`容器先调用属性注入，然后再方法注入，若把示例代码`mBus`的`@Inject`取消注释，此时`mBus`会注入2次，并且两次注入的Bus也不相同。
>
>（3）注解在构造方法中表示此类能为`Dagger2`提供依赖关系，`Dagger2`可以使用这个构造方法构建对象(Bus的来历)：
>
>```java
>@Inject
>public Bus() {
>}
>```
>
>​	如果有多个构造函数，只能注解一个，否则编译报错。
>
>#### `@Component`:
>
>一般用来注解接口，被注解的接口在编译时会生成相应的实例。实例名称一般以`Dagger`为前缀，作为所需注入依赖和提供依赖之间的桥梁，把提供的依赖(Bus)注入到所需注入的依赖中。
>
>#### `@Module`:
>
>#### `@Provieds`:
>
>

## 总结

>

## 参考链接

> [dagger2让你爱不释手-基础依赖注入框架篇](<https://www.jianshu.com/p/cd2c1c9f68d4>)
> [google四件套之Dagger2。从入门到爱不释手系列](https://juejin.im/post/5d6f3e47f265da03aa258c72)
> [Android Dagger2 从零单排系列](<https://www.jianshu.com/p/7ee1a1100fab>)
> [Dagger2使用](<https://www.jianshu.com/p/c985e3f262f2>)
> [Dagger2简单入门](https://www.jianshu.com/p/0a3103c4b055)
> [浅析Dagger2的使用](https://www.cnblogs.com/all88/p/5788556.html)
> [Android_Dagger2篇——从小白最易上手的角度 + 最新dagger.android](https://www.jianshu.com/p/22c397354997/)
> [Dagger2 最清晰的使用教程](https://www.jianshu.com/p/24af4c102f62)
> [dagger2到底有哪些好处](<https://blog.csdn.net/hehe307/article/details/80109248>)


