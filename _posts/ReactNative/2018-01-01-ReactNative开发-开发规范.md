---
title: ReactNative开发-编码规范
description: <center>日常开发，还是需要养成良好的编码规范的～</center>
categories:
 - ReactNative
tags: 编码规范
updated: 2019-01-25 17:30:08
---

# 一、javascript代码规范

1. ## 变量

   1.1 一直使用 `const` 来声明变量，如果不这样做就会产生全局变量。我们需要避免全局命名空间的污染。地球队长已经警告过我们了。（译注：全局，global 亦有全球的意思。地球队长的责任是保卫地球环境，所以他警告我们不要造成「全球」污染。）

   ```javascript
   // bad
   superPower = new SuperPower();
   
   // good
   const superPower = new SuperPower();
   ```

   1.2 使用 **const**、**let** 声明变量，而不使用 **var**。

   

2. ## 数组

   2.1 使用字面值创建数组。

   ```javascript
   // bad
   const items = new Array();
   
   // good
   const items = [];
   ```

   2.2 向数组添加元素时使用 Arrary#push 替代直接赋值。

   ```javascript
   const someStack = [];
   
   // bad
   someStack[someStack.length] = 'abracadabra';
   
   // good
   someStack.push('abracadabra');
   ```

   2.3 使用拓展运算符 `...` 复制数组。

   ```javascript
   // bad
   const len = items.length;
   const itemsCopy = [];
   let i;
   
   for (i = 0; i < len; i++) {
     itemsCopy[i] = items[i];
   }
   
   // good
   const itemsCopy = [...items];
   ```

   2.4 使用 Array#from 把一个类数组对象转换成数组。

   ```javascript
   const foo = document.querySelectorAll('.foo');
   const nodes = Array.from(foo);
   ```

3. ## 字符串

   3.1 字符串使用单引号 `''`
   3.2 字符串过长时，使用+连接字符串
   过度使用字串连接符号可能会对性能造成影响。

   ```javascript
   // bad
   const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
   
   // bad
   const errorMessage = 'This is a super long error that was thrown because \
   of Batman. When you stop to think about how Batman had anything to do \
   with this, you would get nowhere \
   fast.';
   
   // good
   const errorMessage = 'This is a super long error that was thrown because ' +
     'of Batman. When you stop to think about how Batman had anything to do ' +
     'with this, you would get nowhere fast.';
   ```

   3.3 程序化生成字符串时，使用模板字符串代替字符串连接。

   ```javascript
   // bad
   function sayHi(name) {
      return 'How are you, ' + name + '?';
   }
   
   // bad
   function sayHi(name) {
     return ['How are you, ', name, '?'].join();
   }
   
   // good
   function sayHi(name) {
     return `How are you, ${name}?`;
   }
   ```

4. ## 函数

   4.1 永远不要把参数命名为 `arguments`。这将取代原来函数作用域内的 `arguments` 对象。

   ```javascript
   // bad
   function nope(name, options, arguments) {
     // ...stuff...
   }
   
   // good
   function yup(name, options, args) {
     // ...stuff...
   }
   ```

   4.2 不要使用 `arguments`。可以选择 rest 语法 `...` 替代。

   为什么？使用 `...` 能明确你要传入的参数。另外 rest 参数是一个真正的数组，而 `arguments` 是一个类数组。

   ```javascript
   // bad
   function concatenateAll() {
   	const args = Array.prototype.slice.call(arguments);
   	return args.join('');
   }
   // good
   function concatenateAll(...args) {
   	return args.join('');
   }
   ```

   4.3 直接给函数的参数指定默认值，不要使用一个变化的函数参数。

   ```javascript
   // really bad
   function handleThings(opts) {
     // 不！我们不应该改变函数参数。
     // 更加糟糕: 如果参数 opts 是 false 的话，它就会被设定为一个对象。
     // 但这样的写法会造成一些 Bugs。
     //（译注：例如当 opts 被赋值为空字符串，opts 仍然会被下一行代码设定为一个空对象。）
     opts = opts || {};
     // ...
   }
   
   // still bad
   function handleThings(opts) {
     if (opts === void 0) {
       opts = {};
     }
     // ...
   }
   
   // good
   function handleThings(opts = {}) {
     // ...
   }
   ```

   4.4 除非参数过多，不然不要传入一个对象

   ```javascript
   // bad
   function handleThings(user) {
     // ...
     return user.username == ''
   }
   
   // good
   function handleThings(username) {
     // ...
     return username == ''
   }
   ```

5. ## 箭头函数

   5.1 当你必须使用函数表达式（或传递一个匿名函数）时，使用箭头函数符号。

   > 为什么?因为箭头函数创造了新的一个 `this` 执行环境，通常情况下都能满足你的需求，而且这样的写法更为简洁。
   >
   > 为什么不？如果你有一个相当复杂的函数，你或许可以把逻辑部分转移到一个函数声明上。

   ```javascript
   // bad
   [1, 2, 3].map(function (x) {
   	return x * x;
   });
   // good
   [1, 2, 3].map((x) => {
   	return x * x;
   });
   ```

   5.2 如果一个函数适合用一行写出并且只有一个参数，那就把花括号、圆括号和 `return` 都省略掉。如果不是，那就不要省略。

   为什么？语法糖。在链式调用中可读性很高。

   为什么不？当你打算回传一个对象的时候。

   ```javascript
   // good
   [1, 2, 3].map(x => x * x);
   // good
   [1, 2, 3].reduce((total, n) => {
   	return total + n;
   }, 0);
   ```

6. ## 属性

   6.1 使用 `.` 来访问对象的属性。

   ```javascript
   const luke = {
     jedi: true,
     age: 28,
   };
   
   // bad
   const isJedi = luke['jedi'];
   
   // good
   const isJedi = luke.jedi;
   ```

   6.2 当通过变量访问属性时使用中括号 `[]`。

   ```javascript
   const luke = {
     jedi: true,
     age: 28,
   };
   
   function getProp(prop) {
     return luke[prop];
   }
   
   const isJedi = getProp('jedi');
   ```

7. ## 解构

   7.1 使用解构存取和使用多属性对象。
   为什么？因为解构能减少临时引用属性。

   ```javascript
   // bad
   function getFullName(user) {
     const firstName = user.firstName;
     const lastName = user.lastName;
   
     return `${firstName} ${lastName}`;
   }
   
   // good
   function getFullName(obj) {
     const { firstName, lastName } = obj;
     return `${firstName} ${lastName}`;
   }
   
   // best
   function getFullName({ firstName, lastName }) {
     return `${firstName} ${lastName}`;
   }
   ```

   7.2 对数组使用解构赋值。

   ```javascript
   const arr = [1, 2, 3, 4];
   
   // bad
   const first = arr[0];
   const second = arr[1];
   
   // good
   const [first, second] = arr;
   ```

   7.3 需要回传多个值时，使用对象解构，而不是数组解构。

   > 为什么？增加属性或者改变排序不会改变调用时的位置。

   ```javascript
   // bad
   function processInput(input) {
       // then a miracle occurs
       return [left, right, top, bottom];
   }
   // 调用时需要考虑回调数据的顺序。
   const [left, __, top] = processInput(input);
   // good
   function processInput(input) {
   	// then a miracle occurs
   	return { left, right, top, bottom };
   }
   // 调用时只选择需要的数据
   const { left, right } = processInput(input);
   ```

8. ## 比较运算符 & 等号

   8.1 

   优先使用 

   ```javascript
   === 和 !==
   ```

    而不是 

   ```javascript
   == 和 !=
   ```

   8.2 条件表达式例如 if 语句通过抽象方法 `ToBoolean` 强制计算它们的表达式并且总是遵守下面的规则：

   - 对象 被计算为 `true`
   - `Undefined` 被计算为 `false`
   - `Null` 被计算为 `false`
   - `Boolean` 被计算为`true`或`false`
   - 数字 如果是 `+0`、`-0`、或 `NaN `被计算为 `false`, 否则为 `true`
   - 字符串 如果是空字符串 `''` 被计算为 `false`，否则为 `true`

   ```javascript
   if ([0]) {
     // true
     // An array is an object, objects evaluate to true
   }
   ```

9. ## 注释

   9.1 使用 `/** ... */` 作为多行注释。包含描述、指定所有参数和返回值的类型和值。

   ```javascript
   // bad
   // make() returns a new element
   // based on the passed in tag name
   //
   // @param {String} tag
   // @return {Element} element
   function make(tag) {
   
     // ...stuff...
   
     return element;
   }
   
   // good
   /**
    * make() returns a new element
    * based on the passed in tag name
    *
    * @param {String} tag
    * @return {Element} element
    */
   function make(tag) {
   
     // ...stuff...
   
     return element;
   }
   ```

   9.2 使用 `//` 作为单行注释。在评论对象上面另起一行使用单行注释。在注释前插入空行。

   9.3  `//` 符号后添加一个空格

   9.4 方法使用[jsdoc](http://www.css88.com/doc/jsdoc/index.html)的方式注释

   

10. ## 分号

    10.1 匿名方法后必须使用分号

    代码压缩后如果多个匿名方法压缩在一起会变成代码错误。

11. ## 命名规制

    11.1 避免单字母命名。命名应具备描述性。

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }
    
    // good
    function query() {
      // ..stuff..
    }
    ```

    11.2 使用驼峰式命名对象、函数和实例。

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}
    
    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

    11.3 使用帕斯卡式命名构造函数或类。

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }
    
    const bad = new user({
      name: 'nope',
    });
    
    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }
    
    const good = new User({
      name: 'yup',
    });
    ```

12. ## 原型链

    12.1 尽量避免覆盖原型链的方式修改javascript基础对象，如String、Date

13. ## 模块

    13.1 总是使用模组 (`import`/`export`) 而不是其他非标准模块系统。你可以编译为你喜欢的模块系统。

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;
    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;
    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    
    ```

# 二、React/JSX

1. ## Declaration 声明模块

   使用 `class extends React.Component` 而不是 `React.createClass` ,除非你有充足的理由来使用这些方法.

   ```javascript
   // bad
     const Listing = React.createClass({
       // ...
       render() {
         return <div>{this.state.hello}</div>;
       }
     });
   
     // good
     class Listing extends React.Component {
       // ...
       render() {
         return <div>{this.state.hello}</div>;
       }
     }
   ```

2. ## Alignment 代码对齐

   2.1 遵循以下的JSX语法缩进/格式. eslint: `react/jsx-closing-bracket-location`

   ```javascript
   // bad
   <Foo superLongParam="bar"
        anotherSuperLongParam="baz" />
   
   // good, 有多行属性的话, 新建一行关闭标签
   <Foo
     superLongParam="bar"
     anotherSuperLongParam="baz"
   />
   
   // 若能在一行中显示, 直接写成一行
   <Foo bar="bar" />
   
   // 子元素按照常规方式缩进
   <Foo
     superLongParam="bar"
     anotherSuperLongParam="baz"
   >
     <Quux />
   </Foo>
   ```

3. ## Quotes 单引号还是双引号

   3.1 对于JSX属性值总是使用双引号(`"`), 其他均使用单引号(`'`). 
   为什么? HTML属性也是用双引号, 因此JSX的属性也遵循此约定.


4. ## Spacing 空格

   4.1 总是在自动关闭的标签前加一个空格，正常情况下也不需要换行. eslint: [`no-multi-spaces`](http://eslint.org/docs/rules/no-multi-spaces), `react/jsx-space-before-closing`

   ```javascript
   // bad
   <Foo/>
   
   // very bad
   <Foo                 />
   
   // bad
   <Foo
    />
   
   // good
   <Foo />
   ```

   4.2 不要在JSX `{}` 引用括号里两边加空格. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

   ```javascript
   // bad
   <Foo bar={ baz } />
   
   // good
   <Foo bar={baz} />
   ```

5. ## Props 属性

   5.1 JSX属性名使用骆驼式风格`camelCase`.

   ```javascript
   // bad
   <Foo
     UserName="hello"
     phone_number={12345678}
   />
   
   // good
   <Foo
     userName="hello"
     phoneNumber={12345678}
   />
   ```

   5.2 对于所有非必须的属性，总是手动去定义`defaultProps`属性.

   为什么? propTypes 可以作为模块的文档说明, 并且声明 defaultProps 的话意味着阅读代码的人不需要去假设一些默认值。更重要的是, 显示的声明默认属性可以让你的模块跳过属性类型的检查.

   ```javascript
   // bad
   function SFC({ foo, bar, children }) {
     return <div>{foo}{bar}{children}</div>;
   }
   SFC.propTypes = {
     foo: PropTypes.number.isRequired,
     bar: PropTypes.string,
     children: PropTypes.node,
   };
   
   // good
   function SFC({ foo, bar }) {
     return <div>{foo}{bar}</div>;
   }
   SFC.propTypes = {
     foo: PropTypes.number.isRequired,
     bar: PropTypes.string,
   };
   SFC.defaultProps = {
     bar: '',
     children: null,
   };
   
   ```

6. ## Parentheses 括号

   6.1 将多行的JSX标签写在 `()`里. eslint: `react/wrap-multilines`

   ```javascript
   // bad
   render() {
     return <MyComponent className="long body" foo="bar">
              <MyChild />
            </MyComponent>;
   }
   
   // good
   render() {
     return (
       <MyComponent className="long body" foo="bar">
         <MyChild />
       </MyComponent>
     );
   }
   
   // good, 单行可以不需要
   render() {
     const body = <div>hello</div>;
     return <MyComponent>{body}</MyComponent>;
   }
   ```

7. ## Tags 标签

   7.1 对于没有子元素的标签来说总是自己关闭标签. eslint: `react/self-closing-comp`

   ```javascript
   // bad
   <Foo className="stuff"></Foo>
   
   // good
   <Foo className="stuff" />
   ```

   

   7.2 如果模块有多行的属性， 关闭标签时新建一行. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

   ```javascript
   // bad
   <Foo
     bar="bar"
     baz="baz" />
   
   // good
   <Foo
     bar="bar"
     baz="baz"
   />
   ```

8. ## Methods 函数

   8.1 当在 `render()` 里使用事件处理方法时，提前在构造函数里把 `this` 绑定上去. eslint: `react/jsx-no-bind`

   ```javascript
   // bad
     class extends React.Component {
       onClickDiv() {
         // do stuff
       }
   
       render() {
         return <div onClick={this.onClickDiv.bind(this)} />
       }
     }
   
     // good
     class extends React.Component {
       constructor(props) {
         super(props);
   
         this.onClickDiv = this.onClickDiv.bind(this);
       }
   
       onClickDiv() {
         // do stuff
       }
   
       render() {
         return <div onClick={this.onClickDiv} />
       }
     }
    
     // good
     class extends React.Component {
       constructor(props) {
         super(props);
       }
   
       onClickDiv = () => {
         // do stuff
       }
   
       render() {
         return <div onClick={this.onClickDiv} />
       }
     }
   ```

   8.2 在 `render` 方法中总是确保 `return` 返回值. eslint: [`react/require-render-return`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)

   ```javascript
   // bad
   render() {
     (<div />);
   }
   
   // good
   render() {
     return (<div />);
   }
   ```

9. ## Ordering React 模块生命周期

   主要规范组件内部方法的定义顺序。

   Es6 class 定义规范:

   ```react
   static 方法
   constructor
   getChildContext
   componentWillMount
   componentDidMount
   componentWillReceiveProps
   shouldComponentUpdate
   componentWillUpdate
   componentDidUpdate
   componentWillUnmount
   clickHandlers + eventHandlers 如 onClickSubmit() 或 onChangeDescription()
   getter methods for render 如 getSelectReason() 或 getFooterContent()
   render methods 如 renderNavigation() 或 renderProfilePicture()
   render
   ```

   以 Es6 Class 定义的组件为例

   ```javascript
   class Person extends React.Component {
       static defaultProps = {}
       static propTypes = {}
       // 构造函数
       constructor (props) {
           super(props);
           // 定义 state
           this.state = { smiling: false };
       }
       
       // 生命周期方法
       componentWillMount () {}
       componentDidMount () {}
       componentWillUnmount () {}
         
       
       // getters and setters
       get attr() {}
       
       // handlers
       handleClick = () => {}
       
       // render
       renderChild() {}
       render () {}
   }
   ```



