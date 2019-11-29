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

## 基础篇

>`Dagger2`提供的注解：`@Inject`、`@Component ` 和 `@Module`、`@Provieds`
>
>**在描述第一组注解的作用前，我们来看一则例子：**
>
>>&ensp;&ensp;&ensp;&ensp;假设领导下午要出去视察民情，需要一辆公交车，司机就不用请了，领导做表率自己开，小秘把领导的指示告诉了车场调度员。
>>
>>&ensp;&ensp;&ensp;&ensp;找辆公交车要停放在停车场(`Bus要注入到ParkingActivity`)，停车场不管公交车按什么路线停车(`ParkingActivity不管Bus是如何注入的`)，车场调度员会负责好管理(`Dagger2容器会将Bus的注入到ParkingActivity`)。
>
>```java
>public class Bus {
>	@Inject
>	public Bus() {
>	}
>}
>
>public class ParkingActivity extends Activity {
>	@Inject
>	Bus mBus;
>	@Override
>	protected void onCreate(Bundle savedInstanceState) {
>		super.onCreate(savedInstanceState);
>		setContentView(R.layout.activity_dagger);
>		DaggerParkingComponent.create().inject(this);//DaggerParkingComponent类需要编译才会生成
>		((TextView) findViewById(R.id.text)).setText(mBus.toString());
>	}
>}
>
>@Component
>public interface ParkingComponent {
>	void inject(ParkingActivity activity);
>}
>```
>
>`DaggerParkingComponent`类是编译过后`Dagger2`自动生成的，是`ParkingComponent`的实现类，可以说`DaggerParkingComponent`就是实际的车场调度员，`ParkingComponent`是对车场调度员的约束，在`onCreate`方法完成了注入过程。
>
>##### `@Inject`的作用
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
>	mBus = bus;
>}
>```
>
>&ensp;&ensp;&ensp;&ensp;方法注入的参数同样由`Dagger2`容器提供，以上代码的目的与第一点介绍的属性注入一样，都是为了注入`mBus`，如果目的是注入属性的话，方法注入和属性注入基本没有区别，属性注入是`Dagger2`中使用最多的一个注入方式。
>
>&ensp;&ensp;&ensp;&ensp;那么什么情况下应该使用方法注入？比如依赖需要this对象的时候，方法注入可以提供安全的this对象。
>
>&ensp;&ensp;&ensp;&ensp;注意，`Dagger2`容器先调用属性注入，然后再方法注入，若把示例代码`mBus`的`@Inject`取消注释，此时`mBus`会注入2次，并且两次注入的Bus也不相同。
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
>##### `@Component`的作用
>
>一般用来注解接口，被注解的接口在编译时会生成相应的实例。实例名称一般以`Dagger`为前缀，作为所需注入依赖(`ParkingActivity的mBus属性`)和提供依赖(`Bus类构造方法`)之间的桥梁，把提供的依赖)注入到所需注入的依赖中(`ParkingActivity的mBus属性`)。
>
><mark>例子一总结：通俗说，就是Dagger2的容器，在例子中是车场调度员，把公交车和停车场联系在一起。车场调度员知道停车场要准备一辆公交车，停车场不需要知道车是哪里来的，也不需要知道怎么停，车场调度员找好车后，停在车位上就完事，等候领导用车就可以了。</mark>
>
><mark>两个@Inject注解形成了依赖关系，@Component作为连接这个关系的桥梁存在，寻找到依赖并且注入，并且注入与被注入之间互不干涉，经过编译@Component生成Dagger为前缀的实例，调用实例的方法。</mark>
>
>**第二组注解例子：**
>
>>&ensp;&ensp;&ensp;&ensp;领导想着不妥，自己没开过公交车，为保险起见还是赶紧吩咐小秘，去给配个公交车司机，小秘自然跟车场调度员说了。
>>
>>&ensp;&ensp;&ensp;&ensp;车场调度员一想，只是比之前多了一个步骤，给公交车配置一个司机(Bus类添加String类型Driver构造方法)。于是打电话给开公交车的隔壁老王，老王自然是应诺了下来，于是乎，对例子一的代码做一下修改。
>
>```java
>public class Bus {
>private String driver;
>@Inject
>public Bus(String driver) {
>   this.driver = driver;
>}
>}
>
>public class ParkingActivity extends Activity {
>@Inject
>Bus mBus;
>@Override
>protected void onCreate(Bundle savedInstanceState) {
>   super.onCreate(savedInstanceState);
>   setContentView(R.layout.activity_dagger);
>   DaggerParkingComponent.create().inject(this);//DaggerParkingComponent类需要编译才会生成
>   ((TextView) findViewById(R.id.text)).setText(mBus.toString());//重写Bus的toString()方法能看到打印出"隔壁老王"，注入成功
>}
>}
>
>@Component(modules = ParkingModule.class)
>public interface ParkingComponent {
>void inject(ParkingActivity activity);
>}
>
>@Module
>public class ParkingModule {
>public ParkingModule() {
>}
>@Provides
>public String provideDriver() {
>   return "隔壁老王";
>}
>}
>```
>
>
>
>##### `@Module`的作用
>
>该注解与`@Provide`s结合为`Dagger2`提供依赖关系，对上文`@Inject`第三点的补充，用于不能用`@Inject`提供依赖的地方，如第三方库提供的类，基本数据类型等不能修改源码的情况。
>
>##### `@Provieds`的作用
>
>`@Provides`仅能注解方法，且方法所在类要有`@Module`注解。注解后的方法表示`Dagger2`能用该方法实例对象提供依赖。按照惯例，`@Provides`方法的命名以`provide`为前缀，方便阅读管理。
>
><mark>例子二总结：首先@Component注解包含了一个ParkingModule类，表示Dagger2可以从ParkingModule类查找依赖，Dagger2会自动查找ParkingModule类有@Provides注释的方法实例依赖，最后完成注入</mark>
>  <mark>**注意1**，如果在ParkingModule里面同样提供Bus的依赖，Dagger2会优先在@Module注解的类上查找依赖，没有的情况才会去查询类的@Inject构造方法，如下面的代码，则Bus的司机就是小王而不是老王了。</mark>
>
>```java
>@Module
>public class ParkingModule {
>public ParkingModule() {
>}
>@Provides
>public String provideDriver() {
>   return "隔壁老王";
>}
>@Provides
>public Bus provideBus() {
>   return new Bus("楼上小王");
>}
>}
>```
>
>**注意2**，`@Module`类可以从构造方法传入依赖，`@Provides`方法也可以有依赖关系。
>  `@Provides`方法也有依赖关系的情况，`Dagger2`会继续查找可以提供依赖的方法，类似于一种递归的状态，一步一步返回实例。如下代码，`ParkingModule`构造方法传入`driver`为`provideDriver`方法提供依赖返回，`provideDriver`返回`driver`作为`provideBus`方法的依赖实例`Bus`。
>  `ParkingModule`加入有参构造方法后，调用方式也需要变成，现在司机就变成了楼下老李了。
>
>```java
>@Module
>public class ParkingModule {
>private String driver;
>public ParkingModule(String driver) {
>   this.driver = driver;
>}
>@Provides
>public String provideDriver() {
>   return driver;
>}
>@Provides
>public Bus provideBus(String driver) {
>   return new Bus(driver);
>}
>}
>//调用方式改变
>DaggerParkingComponent.builder().parkingModule(new ParkingModule("楼下老李")).build().inject(this);
>```
>
>##### 总结
>
>>最后，来总结下依赖注入的大致流程：
>>
>>1：查找`Module`中是否有该实例的`@Provides`方法。
>>
>>- 1.1：有，走第2点。
>>- 1.2：没有，查找该实例是否有@Inject构造方法。
>>
>>&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;1.2.1：有，走第3点。
>>
>>&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;1.2.2：没有，注入失败。
>>
>>2：`@Provides`方法是否有参数
>>
>>- 2.1：有，则回到第1点查找每个参数的依赖
>>- 2.2：没有，实例该类返回一次依赖
>>
>>3：`@Inject`构造方法是否有参数
>>
>>- 3.1：有，则回到第1点查找每个参数的依赖
>>- 3.2：没有，实例该类返回一次依赖
>>
>>4：以上流程递归返回注入目标的所有依赖，最后依赖注入。

## 总结

>
>

## 进阶篇

>

## 参考链接

> [dagger2让你爱不释手-基础依赖注入框架篇](<https://www.jianshu.com/p/cd2c1c9f68d4>)
>
> [google四件套之Dagger2。从入门到爱不释手系列](https://juejin.im/post/5d6f3e47f265da03aa258c72)
>
> [Android Dagger2 从零单排系列](<https://www.jianshu.com/p/7ee1a1100fab>)
>
> [Dagger2使用](<https://www.jianshu.com/p/c985e3f262f2>)
>
> [Dagger2简单入门](https://www.jianshu.com/p/0a3103c4b055)
>
> [浅析Dagger2的使用](https://www.cnblogs.com/all88/p/5788556.html)
>
> [Android_Dagger2篇——从小白最易上手的角度 + 最新dagger.android](https://www.jianshu.com/p/22c397354997/)
>
> [Dagger2 最清晰的使用教程](https://www.jianshu.com/p/24af4c102f62)
>
> [dagger2到底有哪些好处](<https://blog.csdn.net/hehe307/article/details/80109248>)


