---
layout: posts
title: React 学习笔记
date: 2019-05-08 16:32:15
tags: 
- react
- javascript
---

# react 学习笔记
[TOC]

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

```js
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
    
	4. ...操作符用于函数传参数，表示将数组各个元素传递给函数参数
	
	```js
	[1, 2, 3].push(...[4, 5, 6])
	```

### 二. 箭头函数的使用

#### 1. 定义

```js
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

### 三. 其他补充

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

创建一个 React 的单页应用

```bash
npx create-react-app my-app
cd my-app
npm start
```

### 一、JSX语法

JSX，是一个 JavaScript 的语法扩展

#### 1、使用

```js
	const element = <h1>测试</h1>
```

#### 2、使用表达式和变量

```js
	const username = "my-test"
	const element = (
		<h1 className={'test'} style={{textAlign: 'center'}} title="123">
			{username}
			{username === 'my-test' ? '1' : '2'}
			<input disabled />
		</h1>
	)
```

- 属性 使用`{}`包起来的表示变量或者表达式,如果使用`""`包起来表示简单的字符串
- 属性定义了但没有给值 默认为 `true`


### 二、定义组件

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

```js
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

##### react 允许外层不适用外层标签、即允许两个同级标签，不过需要使用Fragment 标签标记

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

##### 出于安全考虑的原因（XSS 攻击），在 React.js 当中所有的表达式插入的内容都会被自动转义,如果需要使用html 标记必须使用dangerouslySetInnerHTML

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
#### 4、state和生命周期

`state` 相当于 `vue` 的 `data`

修改`state` 必须使用 `this.setState()` 函数

[详细的生命周期](https://react.docschina.org/docs/react-component.html)

- constructor() 初始化
- render() 渲染
- componentDidMount() 挂载之后

#### 5、条件渲染&循环

1. 条件渲染
	- 可以使用 `&&` 操作符 
	- 可以使用 三元运算符 `?:`

	```js
	const MyComponent = props => {

		return <div>
			{this.props.title && <h1>{this.props.title}<h1>}
			
			{this.props.content ? <div>{this.props.conent}</div> : ''}
			
			{0 && <div>123</div>}
		</div>
	}
	```
	
	注意： 使用 `&&` 操作符的时候，前面表达式最好是一个布尔值 
	
2. 列表渲染
	- 需要给同级元素一个唯一的`key`
	- 如果是数组变量可以直接渲染 
	```js
	const MyComponent = props => {
		const items = [1, 2, 3]
		return <div>
			{items}
			
			{items.map(v => v)}
			
			<ul>
				{item.map(v => <li key={v}>{v}</li>)}
			</ul>
		</div>
	}
	```

### 三、组件传参

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

### 四、默认传参defaultProps

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

### 五、PropTypes 和组件参数验证

1. 需要引入React 提供的第三方库 prop-types

```shell
npm install --save prop-types
```

2. [使用说明](https://react.docschina.org/docs/typechecking-with-proptypes.html)

```js
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

### 六、组件的事件处理

- React 事件的命名采用小驼峰式（camelCase），而不是纯小写。
- 使用 JSX 语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。

#### 1. 绑定事件时候 this 指向问题

##### [文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

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

```js

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

### 七、... 操作符在 JSX 语法中常用方式

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

### 八、高阶组件

1. 定义高阶组件
	就是像定义普通函数一样，不过第一个参数是一个组件，而且必须要返回一个组件
	
	```js
	const hocComponent = WrappedComponent => {

	  class HocComponent extends Component {
		render() {
		  return <WrappedComponent style={{padding: '5px', borderRadius: '3px'}} {...this.props}/>
		}
	  }

	  // 这个是约定： 需要定义一个displayName属性，方便调试
	  HocComponent.displayName = `HocComponent(${getDisplayName(WrappedComponent)})`
	  
	  return HocComponent
	}
	
	// 使用
	const Button = props => {
	  return <button {...props}>{props.children}</button>
	}

	const HocButton = hocComponent(Button)
	
	```

2. 高阶组件静态属性的导出(务必复制静态方法)
	- 默认不会导出静态属性，需要手动去导出
	- 使用[`hoist-non-react-statics`](https://github.com/mridgway/hoist-non-react-statics) 
	
3. 高阶组件ref问题
	- refs 提供了一种方式，允许我们访问 DOM 节点或在 render 方法中创建的 React 元素。
	- 默认 ref 指向高阶组件自己，如果需要转发到被高阶组件包裹的组件中，要用到refs转发

### 九、Context

Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法

四个API:
	- React.createContext
	- Context.Provider
	- Class.contextType
	- Context.Consumer
	
## 我们项目中常用组件

[基于 Ant Design Pro](https://pro.ant.design/docs/getting-started-cn)
[Ant Design of React UI库](https://ant.design/docs/react/introduce-cn)

### 一、表格组件`CTable`

```js
	<CTable columns={[....]} api={queryApi} btnClick={this.handleClick} />
```

常用的属性列表

|属性名|类型|说明|
|:-----|:------|:-----|
|`columns`|`array`|表格字段信息|
|`api`|`function`|表格数据来源API|
|`btnClick`|`(key, rows) => {}`|表格操作项点击事件处理|
|`index`|`string` or `function`|表格每一行唯一的key|
|`filters`|`object`|查询的默认条件|

其他说明

`columns`中，元素配置 `_render` 表示生成操作项，配置格式 `{key: 'update', title: '修改'}` 
或者 `[{key: 'update', title: '修改'}]`, 点击事件交给表格的 `btnClick` 函数处理  

```jsx
<CTable 
	columns={[
		{title: 'ID', dataIndex: 'id'},
		{
			title: '操作', 
			_render: [
				{key: 'update', title: '修改'},
				[
					{key: 'delete', title: '删除'},
					{key: 'offline', title: '启用'},
				],
			]
		}
	]} 
	api={queryApi} 
	btnClick={this.handleClick} 
/>
```

需要后端服务器返回数据格式

```json
{
	code: 10000,
	data: {
		items: [...],
		page: {
			total: 10,
			current: 1,
			pageSize: 10,
		}
	},
	message: "success",
}
```

如果不需要分页， `page` 返回 `false`

### 二、表单组件`CForm`

1. `CForm` 表单组件

```jsx
	<CForm rules={{name: [{required: true, message: '不能为空'}]}} onSubmit={(values) => {
		console.info(values)
	}}>
		<CInput name="name" label="名称"/>
	</CForm>
```

常用的属性列表

|属性名|类型|说明|
|:-----|:------|:-----|
|`rules`|`object`|表单字段验证信息|
|`onSubmit`|`(values) => {}`|表单提交事件处理|
|`initialValue`|`object`|表单字段初始值数据|

2. `CInput` Input 输入框

```jsx
	<CInput name="name" label="名称" />
```
常用的属性列表

|属性名|类型|说明|
|:-----|:------|:-----|
|`name`|`string`|表单字段名称(`html` `input` 元素的`name`属性,可以是 ：`name="user[name]"` |
|`label`|`string`| label 名称 |
|`initialValue`|`mixed`|表单字段的初始值|
|`show`|`(values) => boolean`|显示处理函数，`values`表示所有表单的值，需要返回`ture`表示渲染|

	
## JS异步

### 一、异步示例

```js
for (var i = 0; i < 10; i ++) {
	setTimeout(function () {
	  console.info(i)
	}, 1000)
}
```

### 二、使用`Promise`对象

>所谓 Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。

Promise对象特点：
1. 对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。
2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。

#### 1. 定义`Promise`对象

`Promise` 函数有两个参数 `resolve` 和 `reject`，这两个参数都是函数
	- resolve 表示成功返回
	- reject 表示错误返回
	
```js
const handle = () => {
	return new Promise((resolve, reject) => {
      setTimeout(() => {
        alert('加载完成了')
        resolve('执行完成')
      }, 2000)
	  
	  // 如果失败 reject()
    })
}
```

#### 2. 使用`Promise`对象

- `then` 表示 `resolve` 后的回调函数;参数是 `resolve` 返回
- `catch` 表示 `reject` 后的回调函数;参数是 `reject` 返回 
- `finally` 表示不管是`resolve` 还是 `reject` 都会执行的回调

```js
handle().then(value => {
	console.info(value)
}).catch(error => {
	console.info(error)
}).finally(() => {
	console.info('我都呗执行了')
})
```

### 三、使用`co`包

为什么使用`co` 
- 解决异步回调的嵌套
- 让异步回调，同步的形式执行

例如： 发送一个异步请求，但这个请求依赖于另一个异步请求

```js

// jquery.ajax 写法
$.ajax({
	url: 'a.php',
	type: 'get',
	dataType: 'json',
	success: function (data) {
		$.ajax({
			url: 'b.php',
			type: 'get',
			dataType: 'json',
		}).done(function (data) {
			console.info(123)
		})
	}
})

// 使用 Promise 同样存在这样的问题
const handle = (a) => {
	return new Promise(resolve => {
		console.info(a)
		setTimeout(() => {
			resolve(a)
		}, 1000)
	})
}

handle(1).then(value => {
	console.info(value)
	return handle(2)
}).then(val => {
	console.info(val)
})

// 使用 co 包
co(function *() {
	const a = yield handle(1)
	const b = yield handle(a)
	console.info(a, b)
})
```

### 四、使用`async`函数

`async` 函数是 ES2017 标准引入的，是生成器函数的语法糖 

使用 `async` 就是将 生成器函数的 `*` 改为 `async`, 将 `yield` 改为 `await`

```js

const handle = (x) => {
	return new Promise(resolve => {
		setTimeout(() => {
			console.info(x)
			resolve(x + 1)
		}, 1000)
	})
} 

(async () => {
	const a = await handle(1)
	const b = await handle(a)
	console.info(a, b)
})()

```

`async` 返回的还是一个 `Promise` 所以可以继续调用 `then`,`catch`,`finally` 

```js
(async () => {
	const a = await handle(1)
	const b = await handle(a)
	console.info(a, b)
})()
.then(v => console.info(v))
.catch(error => console.info(error))
```




