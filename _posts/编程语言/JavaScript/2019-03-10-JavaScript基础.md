---
title: JavaScript基础
description: <center>经济基础决定上层建筑，基本知识必需掌握</center>
categories:
 - 编程语言
tags: 前端
updated: 2019-03-10 00:00:00
---

#### 1.`JavaScript`规定了几种语言类型

JS包括7种数据类型，其中有分为原始类型和引用类型。

##### 原始类型

- `Null` : 只包含一个值`null`
- `Undefined` : 只包含一个值`undrfined`
- `Number` : 整数和浮点数，还有一些特殊值（`-Infinity` 、`+Infinity` 和`NaN` ）
- `Boolean` : 包含两个值`true`和`false`
- `String` : 一串表示文本值的字符序列
- `Symbol` : 一种实例是唯一且不可改变的数据类型 

##### 引用类型

- `Object`、`Array`、`Function`

#### 2.`JavaScript`对象的底层数据结构是什么

- 原始类型直接存储在栈中，每种类型数据占用的内存空间大小时确定的，并由系统自动分配和自动释放；
- 引用类型会被存储在堆中，准确来说，引用类型的值实际存储在堆内存中，他在栈中只存储了一个固定长度的地址，这个地址指向堆内存的值。
- 数据在内存中的存储结构，也叫物理结构，分为两种：顺序存储结构和链式存储结构：
  - 顺序存储结构 ： 是把数据元素存放在地址连续的存储单元里，其数据见的逻辑关系和物理关系是一致的。数组就是其中的代表。
  - 链式存储结构：是把数据元素存放在任意的存储单元里，这些数据在内存中的位置有可能是连续的，也有可能不不连续的，链表就是其中的代表。
  - 形象地说，就是去银行取钱，顺序存储结构就是客户按照先来后到坐在标有顺序的座椅上，等候办理业务；链式存储结构就是客户拿着叫号机给的编号，然后随意地坐在没有顺序的座椅上，等候叫号办理业务。

#### 3.`null`和`undefined`的区别

- `null` : 表示被赋值过的对象，刻意把一个对象赋值为`null`，表示该值为空，不应该有值。转换数值时的值为`0`；
- `undefined` : 表示缺少值，在对象初始化的时候未进行赋值定义。转换数值时的值为`NaN`

#### 4.至少可以说出三种判断`JavaScript`数据类型的方式，以及他们的优缺点，如何准确的判断数组类型

| 类型         |                             优点                             |                             缺点                             |
| ------------ | :----------------------------------------------------------: | :----------------------------------------------------------: |
| `typeof`     |               能准确判断一个变量是否为原始类型               | 判断引用类型时略显乏力，除了函数判断为function外，其他都判断为object |
| `instanceof` |              能判断引用类型的具体是什么类型对象              |                                                              |
| `toString`   | 利用`Object`对象的继承性，`toString()`返回`[object type]`,其中`type`是对象的类型 | `如果此方法在自定义对象中未被覆盖`，`toString`才会达到预想的效果，事实上，大部分引用类型比如`Array、Date、RegExp`等都重写了`toString`方法。 |

#### 5.类型转换

因为JS是弱类型语言，所以类型转换会经常发生。类型转换分两种：隐式转换即程序自动进行的类型转换，强制转换即我们手动进行的类型转换。

##### 类型转换规则

| 转换前类型  |      转换前值       | 转换后（Boolean） | 转换后（Number） | 转换后（String）  |
| :---------- | :-----------------: | :---------------: | :--------------: | :---------------: |
| `Boolean`   |       `true`        |         -         |       `1`        |      `true`       |
| `Boolean`   |       `false`       |         -         |       `0`        |      `false`      |
| `Number`    |        `123`        |      `true`       |        -         |       `123`       |
| `Number`    |     `Infinity`      |      `true`       |        -         |    `Infinity`     |
| `Number`    |         `0`         |      `false`      |        -         |        `0`        |
| `Number`    |        `NaN`        |      `false`      |        -         |       `NaN`       |
| `String`    |         ‘ ’         |      `false`      |       `0`        |         -         |
| `String`    |        `123`        |      `true`       |      `123`       |         -         |
| `String`    |    `123lyichao`     |      `true`       |      `NaN`       |         -         |
| `String`    |      `lyichao`      |      `true`       |      `NaN`       |         -         |
| `Symbol`    |     `Symbol()`      |      `true`       |   `TypeError`    |    `TypeError`    |
| `Null`      |       `null`        |      `false`      |       `0`        |      `null`       |
| `Undefined` |     `undefined`     |      `false`      |      `NaN`       |    `undefined`    |
| `Function`  |   `function(){}`    |      `true`       |      `NaN`       |  `function(){}`   |
| `Object`    |        `{}`         |      `true`       |      `NaN`       | `[object Object]` |
| `Array`     |        `[]`         |      `true`       |       `0`        |        ``         |
| `Array`     |    `["lyichao"]`    |      `true`       |      `NaN`       |     `lyichao`     |
| `Array`     | `["123","lyichao"]` |      `true`       |      `NaN`       |  `123`,`lyichao`  |

##### if语句和逻辑语句

在`if`语句和逻辑语句中，如果只有单个变量，会先讲变量转换成`Boolean`，只有下面几种情况会转换成`false`，其他都会被转换成`true`

```javascript
null,undefined,'',NaN,0,false	
```

##### 各种数学运算符

我们在对各种非`Number`类型运用数学运算符（`- * /`）时，会先讲非`Number`类型转换为`Number`类型

```javascript
1 - true // 0
1 - null //  1
1 * undefined //  NaN
2 * ['5'] //  10
```

⚠️<mark>注意！加法是个例外，执行加法运算时：</mark>

- 1.当一侧为`String`类型时，会被识别为字符串拼接，并会优先将另一侧转换为字符串类型；
- 2.当一侧为`Number`类型，另一侧为原始类型，则将原始类型转换为`Number`类型；
- 3.当一侧为`Number`类型，另一侧为引用类型，则将引用类型和`Number`类型转换成字符串后拼接。

```javascript
123 + '123' // 123123   （规则1）
123 + null  // 123    （规则2）
123 + true // 124    （规则2）
123 + {}  // 123[object Object]    （规则3）
```

##### ==

使用`==`时，若两侧类型相同，则比较结果和`=== `相同，否则会发生隐式转换。使用`==`时会发生转换可以分为几种情况（只考虑两侧类型不同时）：

- NaN

  `NaN`和其他任何类型比较永远返回`false`(包括和他自己)。

  ```javascript
  NaN == NaN // false
  ```

- Boolean

  `Boolean`和其他任何类型比较，`Boolean`首先被转换为`Number`类型。

  ```javascript
  true == 1  // true 
  true == '2'  // false
  true == ['1']  // true
  true == ['2']  // false
  ```

  <mark>这里注意一个可能会弄混的点</mark>：`undefined、null`和`Boolean`比较，虽然`undefined、null`和`false`都很容易被想象成假值，但是他们比较结果是`false`，原因是`false`首先被转换成`0`：

  ```javascript
  undefined == false // false
  null == false // false
  ```

- String和Number

  `String`和`Number`比较，先将`String`转换为`Number`类型。

  ```javascript
  123 == '123' // true
  '' == 0 // true
  ```

- null和undefined

  `null == undefined`比较结果是`true`，除此之外，`null、undefined`和其他任何结果的比较值都为`false`。

  ```javascript
  null == undefined // true
  null == '' // false
  null == 0 // false
  null == false // false
  undefined == '' // false
  undefined == 0 // false
  undefined == false // false
  ```

- 原始类型和引用类型