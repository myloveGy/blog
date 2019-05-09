---
layout: posts
title: React 学习笔记
date: 2019-05-08 16:32:15
tags: React
---

## ES6 基础

### 一. 变量的解构赋值

#### 1. 数组的解构赋值

```js

// 简单
let [a, b, ...c] = [1, 2, 3, 4, 5]
// a = 1, b = 2, c = [3, 4, 5] (...操作符表示其余的元素都赋值给 c 组成一个数组)

// 同样适用于二维数组
let [a, [b, c]] = [1, [2, 3]]
// a = 1, b = 2, c = 3

// 忽略其中的值
let [, , a] = [1, 2, 3]
// a = 3

// 解构不成功值为 undefined
let [, , a] = [1, 2]
// a = undefined

// 解构的时候给默认值
let [a, b = true, c = true] = [1, false]
// a = 1 b = false c = true 只有右边的值为 === undefined 的时候，才会赋值默认值

```
> 数组的解构，前提是右边一定要是数组(可以是可遍历的结构)，不然会报错

#### 2. 对象的解构赋值

```js

// 简单解构 
let {a, b, ...c} = {a: 1, b: 2, c: 4, d: 5}
// a = 1 b = 2 c = {c: 4, d: 5} (...表示把右边剩余的字段全部赋值给 c 组成一个对象)

// 同样适用于多层对象
let {a, b: {c}} = {a: 123, b: {c: 456}}
// a = 123 c = 456 这里并没有解构出 b 左边的写法只是为了解构 a 和 c

// 解构的时候，重新命名变量名称 (左边的字段名称必须与右边字段对应，不然解构失败 值为undefined, 但可以在左边解构的时候重新命名变量)
let {a, b:name, c} = {a: 123, b: 456}
// a = 123 name = 456 c = undefined 解构的时候，将右边的b 字段重新命名为 name, c 字段在右边对象中不存在 为 undefined  

// 深层对象解构，如果第一层字段就不存在，那么会报错
let {a: {c}} = {}
// 报错 因为右边对象没有 a 字段信息

// 同样可以赋值默认值(当右边 === undefined 时才有效)
let {a = 1, b = 456, c = 1} = {b: true, c: null}
// a = 1 b = true c = null
```

* 解构对象的字段都是普通类型，是值拷贝 

```
let object1 = {a: 1, b: 2, c: 3}

let {...object2} = object1
// object2 = {a: 1, b: 2. c: 3}
// 不会影响 object 1
object2.a = 4

// 需要区别直接赋值的情况，直接赋值还是对象引用
let ojbect3 = object1
// object3 = {a: 1, b: 2, c:3}

// 会影响 object1 
object3.a = 2 

```

* 解构对象的字段还是一个对象的话，也是赋值的对象引用，不是值拷贝
    
```js

let object1 = {
    a: 123,
    b: {
        username: '123',
        age: 456
    }
}

let {a, b} = object1

// 这个修改会影响 object1 b 字段
b.username = '456'
// b = {username: '456', age: 456} object1 = {a: 123, b: {username: '456', age: 456}}

```

#### 3. 函数参数的解构

```js

/**
 * getUser 函数需要接收一个对象作为参数
 * 函数只需要这个对象的 username, age 字段，并用这个两个字段重新组合为一个对象返回
 */
function getUser({username, age}) {
    return {
        username: username,
        age: age,
    }
}

const a = getUser({username: '123', age: 456, sex: 1})
// a = {username: '123', age: 456}

const b = getUser({})
// b = {username: undefined, age: undefind}

// 使用箭头函数简写getUser函数
const f = ({username, age}) => ({username, age})

```

* 补充说明 ( ... 操作符)

    1. ...操作符一般用来解构对象和数组，解构的时候，只能放到最后一个参数使用
    
    ```js
    // 正确解构赋值
    let {a, ...b} = {a: 123, b: 2, c: 3}
    // a = 123 b = {b: 2, c: 3}
    
    let object1 = {a: 123, b: 2, c: 3}
    let {...a} = object1
    // a = {a: 123, b: 2, c: 3} 这里 a 和 直接赋值 ( let a = object1 ) 还是有区别的
    // 解构的话是 object1 值的拷贝; 直接赋值 是 object1 对象引用
    
    // 错误
    let {...a, b} = {a: 123, b: 456}
    // 程序报错
    
    ```
    
    2. ...操作符也能用于对象的合并和数组的合并
    
    ```js
    
    // 对象的合并
    let object1 = {username: 123, age: 456}
    let object2 = {name: '123', sex: 1}
    
    // 后者的同名字段会覆盖前者的同名字段 相当于php数组的 array_merge
    let object3 = {...object1, ...object2, username: 'jinxing'}
    // object2 = {username: 'jinxing', age: 456, name: '123', sex: 1}
    
    // 数组的合并
    let a = [1, 2, 3, 4, 5]
    let b = [1, 2, 3, 4, 5]
    
    // 相当于将 a, b 数组展开追加到新数组中 c = [], c.push(...a), c.push(...b), c.push(7, 8)
    let c = [...a, ...b, 7, 8]
    // c = [1, 2, 3, 4, 5, 1, 2, 3, 4, 5, 7, 8]
    ```
    
    3. ...操作符用于函数，表示接收不确定参数个数 (和 go 有点相似)
    
    ```js
    // 求和 params 是传入参数组成的数组
    function sum(...params) {
        let i = 0
        params.forEach(v => i += v)
        return i
    }
    
    const a = sum(1, 2, 3) 
    // a = 6
    
    ```
    

### 二. 箭头函数的使用

#### 1. 定义

```
// 不要参数
var x1 = () => {
    console.info(123)
}

// 相当于
function x1() {
   console.info(123) 
}


// 单个参数
var x2 = a => {
    console.info(a)
}

// 相当于
function x2(a) {
   console.info(a) 
}

// 多个参数
var x3 = (a, b, c) => {
    console.info(a, b, c)
}

// 相当于
function x3(a, b, c) {
   console.info(a, b, c) 
}


// 简单返回值可以一行写完
var x4 = (a, b) => a + b

// 返回对象的话
var x5 = (a, b) => ({a, b})

// 返回函数
var x6 = (a, b) => (c) => a + b + c

// 相当于
function x6(a, b) {
    return function(c) {
        return a + b + c
    }
}

// 调用
const a = x6(1, 2)(3)
// a = 6

```

#### 2. this 指向

> 箭头函数不会创建自己的this,它只会从自己的作用域链的上一层继承this

```js
// 不使用箭头函数
function Person() {
  // Person() 构造函数定义 `this`作为它自己的实例.
  this.age = 0;

  setInterval(function growUp() {
    // 在非严格模式, growUp()函数定义 `this` 作为全局对象, 
    // 与在 Person() 构造函数中定义的 `this` 并不相同.
    this.age++;
    console.info(this.age) // NaN, NaN ....
  }, 1000);
}

var p = new Person();

// 解决 this 指向问题
function Person() {
  var that = this;
  that.age = 0;

  setInterval(function growUp() {
    //  回调引用的是`that`变量, 其值是预期的对象. 
    that.age++;
    console.info(this.age) // 1, 2, 3 ...
  }, 1000);
}

// 使用箭头函数 

function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // this 正确地指向 p 实例
  }, 1000);
}

```

### 3. 其他补充

#### 1. 对象字段的简写

```js

let field1 = 1
let field2 = 2
let username = 'react'

// 变量名称为对象字段名称，值为对象字段的值
let object1 = {username, field1, field2, name: '123', age: 456}
// object1 = {username: 'react', field1: 1, field2: 2, name: '123', age: 456}

```

#### 2. 对象字段名为变量

```js

let username = 'username'
let age = 'test'
let object1 = {[username]: 123, [age]: '123'}

// object1 = {username: 123, test: '123'}

```

#### 3. export 和 export default 的区别

##### export 导出的函数或者类 名称是什么，外面引入的时候就要是什么, 但可以引入的时候修改名称; 而且import 引入的时候，需要加上 {}

```js

// a.js 文件 使用 export 导出的，函数名称是什么，外面引入的时候就要用什么
export const username = () => 'username'

// b.js 引入 a.js 
import {username} from './a.js' 

// c.js 引入 a.js 并且修改名称
import {username as user} from './a.js'

```

##### export default 导出的函数或者类，在其他文件引入，需要为其定义名称；import 不要使用 {}

* 一个 js 文件中 只能有一个 export default 导出

```js

// a.js 文件，使用 export default 导出 并且一个文件只能有一个 export default 导出
export default function() { return 'username' }

// b.js 文件引入 a.js 
import user from './a.js'

```


## React 基础

### 一、定义组件

#### 1. 使用类定义

```js
import React, {Component} from 'react'

export default class MyComponent extends Component {
    render() {
        return (
            <div> my component </div>
        )
    }
}

```

#### 2. 使用函数定义

```js
const MyComponent = props => {
    return (
        <div> my component {props.children} </div>
    )
}

```


#### 3. 注意事项

* 组件命名必须组照大驼峰命名法 
* 渲染class类名，需要使用className
* 渲染style时候，必须使用对象，并且字段需要使用小驼峰法 例如：font-size: "16px" 需要写为 fontSize: "16px"


```js
const MyComponent = props => {
    return (
        <div className="my-component" style={{
            fontSize: "16px", 
            textAlign: "center",
            color: "red"
        }}> my component {props.children} </div>
    )
}
```

* render 函数里面渲染的时候，最外层必须要一个父级标记，不允许同时出现两个同级外层标签

```
// 正确
const MyComponent = props => {
    return (
        <div> my component {props.children} </div>
    )
}


// 错误(最外层出现两个同级标签)
const MyComponent = props => {
    return (
        <div> my component {props.children} </div> 
        <div> 123 </div>
    )
}

```

#### react 允许外层不适用外层标签、即允许两个同级标签，不过需要使用Fragment 标签标记


```js

import React, {Fragment} from 'react'

const MyComponent = props => {
    return (
        <Fragment>
            <div> my component {props.children} </div> 
            <div> 123 </div>
        </Fragment>
    )
}

```

#### 出于安全考虑的原因（XSS 攻击），在 React.js 当中所有的表达式插入的内容都会被自动转义,如果需要使用html 标记必须使用dangerouslySetInnerHTML

```js
import React, {Component} from 'react'

export default class MyComponent extends Component {
    state = {
        html: "<h1> my component </h1>"
    }
    
    render() {
        return (
            <div dangerouslySetInnerHTML={{__html: this.state.html}}/>
        )
    }
}

```

### 二、组件传参

#### 父组件传参给子组件直接像html标签属性一样

```js
import React, {Component} from 'react'
import Children from './Children'

export default class MyComponent extends Component {
    state = {
        message: "my message"    
    }
    
    render () {
        return (
            <div>
                <Children name="my-child" message={this.state.message}>
                    这里面的信息为子组件 props.children
                </Children>
            </div>
        )
    }
}

```

#### 子组件接收父组件的参数，使用this.props 接收

```js
import React, {Component} from 'react'

export default class Children extends Component {
   render() {
       return (
            <div>
                <h3> {this.props.name} </h3>
                <p> {this.props.message} </p>
                <div>
                    {this.props.children}
                </div>
            </div>
       )
   } 
}

```

### 三、默认传参defaultProps

```js
import React, {Component} from 'react'

export default class Children extends Component {
    static defaultProps = {
        name: "liujinxing",
        message: "my-test"
    }
    
    render() {
       return (
            <div>
                <h3> {this.props.name} </h3>
                <p> {this.props.message} </p>
                <div>
                    {this.props.children}
                </div>
            </div>
       )
   } 
}

```

### 四、PropTypes 和组件参数验证

1. 需要引入React 提供的第三方库 prop-types

```
npm install --save prop-types
```

2. [使用说明](https://react.docschina.org/docs/typechecking-with-proptypes.html)

```jsx
import React, {Component} from 'react'
import PropTypes from 'prop-types'

class Item extends Component {
    static propTypes = {
        // 必须要传递title为字符串信息,并且不能为空
        title: PropTypes.string.isRequired,
        items: PropTypes.object.isRequired
    }
}
```

### 五、组件的事件处理

#### 1. 绑定事件时候 this 指向问题

```js

class Item extends Component {
    
    name = '按钮'
    
    handleClick() {
        // 这里会存在 this 指向问题  this = undefined
        alert(this.name)
    }
    
    render() {
        return <a onClick={this.handleClick} > 点击效果 </a>
    }
}

// 解决方法一 在初始化的时候，为函数绑定 this

class Item extends Component {

    constructor(props) {
        super(props)
        this.handleClick = this.handleClick.bind(this)
    }
    
    name = '按钮'
    
    handleClick() {
        alert(this.name)
    }
    
    render() {
        return <a onClick={this.handleClick} > 点击效果 </a>
    }
}

// 解决方法二 在指定属性的时候，为函数绑定 this

class Item extends Component {

    name = '按钮'
    
    handleClick() {
        alert(this.name)
    }
    
    render() {
        return <a onClick={this.handleClick.bind(this)} >点击效果</a>
    }
}

// 解决方法三 使用箭头函数(推荐使用箭头函数)

class Item extends Component {

    name = '按钮'
    
    handleClick = () => {
        alert(this.name)
    }
    
    render() {
        return <a onClick={this.handleClick} >点击效果</a>
    }
}

```

* 推荐使用箭头函数

#### 2. 绑定事件的时候，添加传递参数

```

class Item extends Component {

    name = '按钮'
    
    handleClick = (key) => {
        alert(this.name, key)
    }
    
    render() {
        return <a onClick={this.handleClick.bind(this, '123456')} >点击效果</a>
    }
}

```

* 使用函数的 bind 方法, 第一个参数，函数内部 this 指向谁, 后面参数为函数需要接收的参数

##### 如果原有事件就有传递参数，但是处理的时候还想添加参数的话，同样使函数的 bind 方法，自己添加的参数在最前面; 原有的参数，会在自定义参数后面传递

```js
// 子组件
export class Children extends Component {
    
    name = '按钮'
    
    handleClick = (key) => {
        const {onClick} = this.props
        alert(this.name, key)
        
        // 向上调用事件
        onClick && onClick(this.name, key)
    }
    
    render() {
        return <a onClick={this.handleClick.bind(this, '123456')} >点击效果</a>
    }
}


// 父组件
import {Children} from './Children.js'

class Parent extends Component {
    
    /**
     * 参数 name 是自己在Children 组件 onClick 绑定时候添加的参数
     * 参数 childrenName, key 是 Children 组件自己的点击事件传递的参数
     */
    handleClick = (name, childrenName, key) => {
        alert(name, childrenName, key)
        // name = 'parent' children = '按钮' key = '123456'
    }
    
    render() {
        return <Children onClick={this.handleClick.bind(this, 'parent')} />
    }
}

```

##### 使用箭头函数解决传递参数问题

* 通过箭头函数返回函数实现 注意箭头函数的写法 () => () => {}

```js
// 子组件
export class Children extends Component {
    
    name = '按钮'
    
    // 注意这个地方的写法
    handleClick = (key) => () => {
        const {onClick} = this.props
        alert(this.name, key)
        
        // 向上调用事件
        onClick && onClick(this.name, key)
    }
    
    render() {
        // 绑定的时候直接传递参数就好了
        return <a onClick={this.handleClick('123456')} >点击效果</a>
    }
}


// 父组件
import {Children} from './Children.js'

class Parent extends Component {
    
    /**
     * 第一个函数的参数 name 是自己在Children 组件 onClick 绑定时候添加的参数
     * 第二个函数的参数 childrenName, key 是 Children 组件自己的点击事件传递的参数
     */
    handleClick = (name) => (childrenName, key) => {
        alert(name, childrenName, key)
        // name = 'parent' children = '按钮' key = '123456'
    }
    
    render() {
        return <Children onClick={this.handleClick('parent')} />
    }
}
```

### 六、... 操作符在 JSX 语法中常用方式

#### 1. 接收剩余传递参数

```js

class Children extends Component {
    render() {
        // 接收剩余参数，传递给 button 组件
        const {title, name, ...otherProps} = this.props
        return (
            <div>
                <h2>{title}</h2>
                <Button {...otherProps}>{name}</Button>
            </div>
        )
    }
}

// 使用组件
<Children title="测试" name="点击" onClick={() => console.info(123)} type="button" />

// 其中 onClick, type 传递的参数被传递给了 Button 组件
```

#### 2. 属性展开

```js

const defaultProps = {
    type: 'button', 
    onClick: () => alert('被点击了')
    className: 'btn btn-success',
    style: {
        backgroundColor: 'red',
        width: '100px',
        height: '100px'
    }
}

// 将defaultProps 展开传递给组件
<button {...defaultProps}>按钮</button>

// 上面相当于
<button type={defaultProps.type} onClick={defaultProps.onClick} className={defaultProps.className} style={defaultProps.style}>按钮</button>

```


