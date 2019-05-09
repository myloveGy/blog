---
layout: posts
title: Vue 学习笔记
date: 2019-05-08 16:33:45
tags: 
- Vue
- javascript
---

## 一、vue 基础

[vue 官方文档](https://cn.vuejs.org/v2/guide/)

### 1、使用和模板语法

1、变量输出

```html
<div id="app">
	<div>{{ message }}</div>
</div>
```

```js
let app = new Vue({
	el: "#app",
	data() {
		return {
			message: "hello Vue!"
		}
	}
})
```

2、html输出

```html
<div id="app">
    <p> {{ message }} </p>
    <p v-html="html"></P>
</div>
```

``` js
let app = new Vue({
    el: "app",
    data() {
        return {
            "message": "我是普通信息",
            html: "<div style='color:red'>我是html信息</div>"
        }
    }
});

// app.message = app.html
```

区别在于：{{ message }} 这种如果message中包含html 内容，会被转义；要渲染 html 内容 需要使用 v-html 指令

### 2、常用指令

1. v-if 根据条件渲染

```html
<div id="app">
    <p v-if="hide"> 我渲染出来了 </p>
</div>
```

```js
let app = new Vue({
    el: "#app",
    data(){
        return {
            hide: false
        }
    }
})

// app.hide = true
```

2. v-show 根据条件显示&隐藏

```html
<div id="app">
    <p v-show="show"> 我显示出来了 </p>
</div>
```

```js
let app = new Vue({
    el: "#app",
    data(){
        return {
            show: false
        }
    }
})

// app.show = true
```

* v-if 和 v-show 的区别, v-if 是条件成立，才会渲染元素、但 v-show 是元素渲染出来，只是改变css display 属性  

3. v-for 循环渲染

```html
<div id="app">
    <ul>
        <li v-for="(value, index) in items" :key="index"> 
            我是第 {{ index }} 个 li : {{ item }}
        </li>
    </ul>
</div>
```

```js
let app = new Vue({
    el: "#app",
    data(){
        return {
            items: [1, 2, 3, 4, 5, 6]
        }
    }
})

// app.items.psuh(100)
```

4. v-model 表单绑定

```html
<div id="app">
    <label> 用户名 <label> <input v-model="username" />
    <p> 您输入的用户名为: {{ username }} </p>
</div>
```

```js
let app = new Vue({
    el: "#app",
    data(){
        return {
            username: ""
        }
    }
})
```

5. v-bind 绑定变量

```html
<div id="app">
    <p v-bind:href="url" v-bind:class="{'text-warning': warning}"> 连接地址 </a>
</div>
```

```js
let app = new Vue({
    el: "#app",
    data(){
        return {
            href: "/",
            warning: false
        }
    }
})
```

6. v-on   监听事件

```html
<div id="app">
    <button v-on:click="btnClick" > 按钮 </button>
    <button v-on:click="show = !show">
        {{ show ? '隐藏' : '显示' }}
    </button>
</div>
```

```js
let app = new Vue({
    el: "#app",
    methods: {
        btnClick() {
            alert('按钮点击了')
        }
    }
})
```

注意事项
* v-if 和 v-show 的区别 v-if 只有条件满足才会渲染，v-show 只是控制 css display 属性, html 始终是会渲染出来的; v-show 必须作用在真实的 html 上面, v-if 还可以使用在 ```<template>``` 上面， v-for 也可以使用  ```<template>``` 标签上面
* v-for 作用在 html 标签上, 需要定义唯一属性key
* v-on 可以简写为 @； v-bind 可以简写为 :
* v-on 绑定事件，还可以使用修饰符; 常用的 stop 阻止冒泡，prevent 阻止默认行为 [其他修饰符](https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6)

```html
<div id="app">
    <a @click.prevent.stop="show = !show" :href="url">
        {{ show ? '隐藏' : '显示' }}
    </button>
</div>
```

```js
let app = new Vue({
    el: "#app",
    data() {
        return {
            url: "/vue-demo/index.html"
        }
    }
})
```
* v-model 也有修饰符 
1. number (转为数字,可以转才会转)
2. lazy(change 事件触发变更数据)
3. trim (去除两边空格)

### 3、组件

1、定义

```html
<div id="app">
	<my-demo></my-demo>
	<test></test>
</div>
```

```js
    // 定义一个组件(全局)
	Vue.component("MyDemo", {
		template: "<p>我的第一个组件</p>"
	});
	
	// 定义组件(局部)，需要引入
	let test = {
		template: "<p>我定义的组件</p>"
	}

	// 使用组件
	let app = new Vue({
		el: "#app",
		methods: {
			btnClick() {
				alert('按钮被点击了')
			}
		}
	});
```

2、props 传值

```html
    <div id="app">
		<my-demo :info="info"></my-demo>	
	</div>
```

```js
    // 定义组件(全局)
	Vue.component("MyDemo", {
		props: {
			info: [String],
			message: {
				type: String,
				default: "我的测试"
			}
		},
		template: "<p>info: {{ info }} ; message: {{ message }} </p>"
	});

	// 使用组件
	let app = new Vue({
		el: "#app",
		data(){
			return {
				info: "123"
			}
		}
	});
```

3、向父组件通知事件

```html
    <div id="app">
		<my-demo :info="info" @p-click="demoClick"></my-demo>
	</div>
```

```js
    // 定义组件(全局)
	Vue.component("MyDemo", {
		props: {
			info: [String],
			message: {
				type: String,
				default: "我的测试"
			}
		},
		template: "<p @click='click'>info: {{ info }} ; message: {{ message }} </p>",
		methods: {
			click() {
				alert('我被点击了');

				// 向父组件提交事件
				this.$emit("p-click")
			}
		}
	});

	// 使用组件
	let app = new Vue({
		el: "#app",
		data(){
			return {
				info: "123"
			}
		},
		methods: {
			demoClick() {
				alert('接收到子组件的事件了');
				this.info = "收到你的事件了!"
			}
		}
	});
```

### 4、我们用到的其他插件

1. [Vue Router 路由处理](https://router.vuejs.org/zh/)
2. [Vuex 状态管理](https://vuex.vuejs.org/zh/guide/)
3. [Axios Http 请求](https://www.npmjs.com/package/axios)
4. [iView UI 组件库 ](https://www.iviewui.com/docs/guide/install)

## 二、目前前端项目

### 1、目录结构
```
public                           前端资源
src                              项目代码目录
    assets/                      前端资源(css)
    components/                  项目公共组件目录
        business/                目前业务公共组件
    mock/                        接口模拟数据
    model/                       数据model(本地化存储)
    pages/                       具体页面
    plugins/                     插件目录
    router/                      路由配置
    service/                     服务(定义接口api)
    store/                       状态管理
        modules/                 状态管理modules
    util/                        自定义工具库
vue.config.js                    配置代理(修改需要重新编辑)
```
### 2、项目规范

1. 页面组件文件命名为大驼峰，可以通过功能(或者路径)建立目录
2. 组件的样式必须有命名空间，防止组件样式相互污染
3. store中异步 action 使用co包裹 , 不处理异常
4. store中的 action 定义必须以 Action 后缀结束 ， 避免在map到组件中产生冲突
5. store中的 state 定义必须和业务相关 ， 避免使用 （list ／ data ／ item）这样无意义的数据  ， 以提高在组件内数据识别度／冲突
6. store中的 mutations 定义为加前缀 set / save / change 和具体的 state
5. vue组件中使用 vuex 提供的 map系列函数 ， 避免直接使用 this.$store对象
6. components 为通用组件库 ， 所有目录下面必须有 index.js ， 组件必须逐级导出的 @components 顶级
7. components/buissiz 为业务组件库 ， 可以根据需求随意更改 ， 其他组件库更改需谨慎 ，一般情况下不要更改 ，可以写业务员组件来增强通用组件的功能。如更改 , 必须考虑 兼容性 ／ 易用性 ／ 扩展性 ／ 独立性 等等
8. css使用less ，(!!!!)不要混用 ,  由于要和ivew保持一致 ， ivew 使用 less ， 而且项目有less通用变量 （accesset/common.less），有条件的尽量使用 less 变量

补充：

#### 动态路由和路由传递参数
 
定义

```js
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User, name: "user" }
  ]
})

```

上面路由定义 /user/123 和 /user/456 都能匹配到

路由参数获取

* 在组件里面可以通过 this.$route.params.id 获取的路由参数
* 视图里面可以通过 $route.params.id 获取路由参数

```vue
<template>
	<div> {{ $route.params.id }} </div>
</template>

<script>
	export default {
	 	name: "demo",
		created() {
			this.$route.params.id
		}
	}
</script>
```

在组件中手动跳转到动态路由

* 方式一： 自动拼接路由 this.$router.push("/user/" + user.id)
* 方式二： 通过路由名称 this.$router.push({name: "user", params: {id: user.id}})

通过 query 方法在路由中添加参数

组件中通过

```js
this.$router.push({path: "/user/1", query: {id: 1, "name": 123}})
// 生成的路由为 /user/1?id=1&name=123

// 获取请求参数
this.$route.query.id ; // 视图中 $route.query.id
```

#### 注意在组件中 this.$route 和 this.$router 是两个对象，this.$route 表示当前路径；this.$router 表示路由对象

### 5、页面组件开发
1. 页面尽量保持简单 ， 一个页面不要写过多的业务逻辑 ， 就好比PHP中的controller ， 负责调度就好

2. 约定： 如果目录中包含 index.vue 那么这个文件就是 page 的入口 ， 其他文件则为改页面的私有组件 ， 否则改页面没有私有组件

#### 1. vue 单文件组件

文件定义

```
<template>
	页面标签，只允许定义单个父级标签，不能定义多个同级父标签
</template>

<script>
	export default {
	 	name: "组件名称"
	}
</script>

<style scoped lang="less">
	css 样式，没有可以不用定义style 标签 lang 表示 css 使用的 less ，不需要的使用 less 可以去掉 lang 属性
</style>
```

在组件中使用 store 中的数据和方法

```
<template>
	<div @click="setMessage('12323')">
		{{ message }} {{ getMessage }}
	</div>
</template>

<script>
	import {mapState, mapGetters, mapMutations, mapActions} from 'vuex'
	export default {
	 	name: "test",
		methods: {
		// mapMutations, mapActions 第一个参数都是 module 名称， 第二个参数就是需要引入的方法(可以为数组形式，也可以是对象形式) 
			...mapMutations("app", ["setMessage"]), 
			...mapActions("app", ["getMessageAction"])
		},
		computed:{
			// mapState(), mapGetters() 第一个参数都是 module 名称， 第二个参数就是需要引入的数据(可以为数组形式，也可以是对象形式) 
			...mapState("app", ["message"]),
			...mapGetters("app", ["getMessage"])
		},
		created() {
			this.getMessageAction()
		}
	}
</script>
```

在组件中请求接口

* 接口定义都在service 下面定义

```
// 主商户列表
import {createApi} from "../util/request";
// 主商户列表
export const brandListApi = data => createApi('/bank/brand/list', data);
// 首页营收报表
export const homeIndexApi = data => createApi('/bank/home/index', data);

```

* 组件中调用接口

```js
<template>
	<div> {{ message }} </div>
</template>

<script>
	// 需要引入我们公共的基础包的 Process 方法 
	import {Process} from "@verypay/baseui"
	import {getMessageApi} from "./service"
	
	export default {
	 	name: "test",
		data() {
			return {
				message: ""
			}
		},
		created() {
			let me = this;
			Process(function* () {
			  // 获取到data 就是 接口返回数据中 data 字段中的数据
			  let data = yield getMessageApi();
			 // 做你想做的事
			 me.message = data;
			});
		}
	}
	
</script>
```


#### 2. vuex 状态管理(所有组件数据共享)

开始使用

```js
import vuex from 'vuex'
import Vue from "vue";
import app from './modules/app'

Vue.use(vuex);

export const store = new vuex.Store({
  state: {},
  mutations: {},
  getters: {},
  actions: {},
  modules: {
    app,
  },
});

window['store'] = store;

```

挂载到APP组件上

```js
import Vue from 'vue'
import App from './App.vue'
import {store} from "./store";
import {router} from "./router";

Vue.config.productionTip = false

new Vue({
  store,
  router,
  render: h => h(App)
}).$mount('#app')
```

模块文件定义

```js
import co from "co"

export default {
  namespaced: true,
  // 数据
  state: {
	message: "my name is demo"
  },
  
 // 基于state 的计算属性
 getters: {
 	messageInfo(state) {
		return state.message + " 我添加了新数据 "
	}
 },
  // 同步修改数据
  mutations: {
    setMessage(state, data) {
      state.message = data;
    }
  },
  
  // 异步获取数据
  actions: {
  	getMessageAction({commit}) {
		return co(function*() {
			let data = yield createApi(["user_id": 1])
			commit("setMessage", data)
		})
	}
  },
}
```

在非组件js 文件中使用 store 状态管理

```js
// 引入store 对象
import {store} from "../store"

// 获取 app 模块  state 数据
let message = store.state.app.message;

// 获取 app 模块中的计算属性
let messageInfo = store.getters["app/messageInfo"];

// 使用 app 模块的同步提交数据
store.commit("app/setMessage")

// 使用 app 模块的异步操作
store.dispatch("app/getMessageAction")
```
