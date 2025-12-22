# 介绍

## 什么是Vue.js

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

# 第一个vue程序

## 安装Vue.js

1. 在html中引入cdn,或者下载到本地通过本地引入
2. 通过vue-cli创建vue项目(后面会写)

```
<!-- 开发环境-->
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
```

## hello,Vue!程序

1. 新建html
2. 引入vue.js
3. 创建一个vue对象并绑定html一个标签
4. 使用{{}}模板语法输出vue对象中的数据

```
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<body>
  <div id="app">
      {{ message }}
  </div>
</body>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
```

# vue基本语法

## 指令

带有前缀 `**v-**`，例如`**v-bind**`,`**v-on**`,`**v-model**`等以表示它们是 Vue 提供的特殊 attribute。他们所赋予的值不在是普通的字符串,而是绑定到vue对象的数据或者方法,javascript语句,它们会在渲染的 DOM 上应用特殊的响应式行为。

## 条件与循环

### v-if条件

使用v-if来控制切换一个元素是否显示,下面例子中p标签是否显示根据seen这个数据的变化而变化,现在他为true 为显示状态,打开浏览器控制台,通过app.seen=false 修改seen的值来达到使p标签隐藏的效果,当然vue中也有v-else-if 和v-else语法

```
<div id="app">
  <p v-if="seen">hello</p>
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    seen:true,
  }
})
</script>
```

### v-if与v-show

```
<!-- v-if 在条件符合后才开始渲染, 而show 开始时候就渲染,不论显示不显示-->
<div id="app10">
    <h1 v-if="msg==='a'">A</h1>
    <h1 v-else-if="msg==='b'">B</h1>
    <h1 v-else-if="msg==='c'">C</h1>
    <h1 v-else>Not A,B,C</h1>
    <h1 v-show="false">显示</h1>
</div>
```

### v-for循环

通过v-for循环渲染数组数据为列表,通过控制台修改app.list 渲染出来的数据列表会同步变化

```
<div id="app">
  <ol>
    <li v-for="i in list">
      {{ i.text }}
    </li>
  </ol>
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    list: [
      {id:1,text:'text1'},
      {id:2,text:'text2'},
    ]
  }
})
</script>
```

# Vue绑定事件

使用`**v-on:**`指令添加一个事件监听器，通过它调用在 Vue 实例中定义的方法：

给按钮绑定事件,点击按钮修改vue对象的数据,同时达到修改p标签中模板字符串的效果

```
<div id="app">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">反转消息</button>
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  },
  methods:{
      reverseMessage:function(){
        // split('')按括号中的值分割字符串
        // reverse() 反转字符串
        // join('')将字符使用括号中的值连接
        this.message=this.message.split('').reverse().join('')
      }
  }
})
</script>
```

# Vue双向绑定

### v-model

`**v-model**` 指令，它能轻松实现表单输入和应用状态之间的双向绑定。

```
<!-- 
    v-bind:value将其 value attribute 绑定到一个名叫 value 的 prop 上
    v-on:input在其 input 事件被触发时，将新的值赋予绑定的数据
-->
<input v-model="searchText">
<!-- 等同于-->
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```

在输入框中输入数据,vue对象中的message的数据同步修改,同时界面的p标签同步修改渲染,同理在控制台修改app.message,p标签和输入框数据也会改变

```
<div id="app">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: '',
  }
})
</script>
```

# Vue组件

### component自定义组件

自定义TODO标签,并且通过v-for将数据全部渲染出来,v-bind:todo 中todo为自定义props,通过他可以将值传递给内部的使用

```
 <!-- 自定义组件-->
 <div id="app">
     <ol>
         <todo-list
                 v-for='item in groceryList'
                 v-bind:todo="item"
                 v-bind:key="item.id">
         </todo-list>
     </ol>
 </div>
</body>
<script>
  // 自定义组件
    Vue.component('todo-list',{
        props:['todo'],
        template:'<li>{{todo.text}}</li>'
    })
    const app = new Vue({
        el: "#app",
        data: {
            groceryList: [
                {id: 0, text: '蔬菜'},
                {id: 1, text: '奶酪'},
                {id: 2, text: '随便其它什么人吃的东西'},
            ],
        },
    });


</script>
```

# Axios异步通信

## qt模块

使用axios进行post请求时请求的数据是json格式不是普通的表单格式,这时候需要使用qs对象进行格式转换,或者以拼接字符串的形式提交

```
npm install qs
```

使用axios需要注意不能在axios的回调函数中使用this关键字

```
<script>
import axios from 'axios'
import qs from 'qs'
  ```函数内部
        let v = this;
      let user = {
        uid: v.user,
        password: v.password,
        uname: v.uname
      };
      axios.post('http://localhost:8080/admin/addUser',qs.stringify(user)).then(function (resp) {
        v.msg = resp.data.uname;
      }).catch(function (error) {
        v.msg = "error" + error;
      });
  ```
</script>
```

  

# 计算属性

# 插槽slot

# 自定义事件内容分发

# 第一个vue-cli程序

使用命令创建一个简单的vue项目,

```
vue init  webpack ['项目名称']
根据终端提示 
| downloading template                                                                      - downloading temp? Vue build standalone
? Install vue-router? No
? Use ESLint to lint your code? No
? Set up unit tests No
? Setup e2e tests with Nightwatch? No
? Should we run `npm install` for you after the project has been created? (recommended) npm
```

切换到目录 cd test

运行项目 npm run dev

具体运行的脚本在package.json的script配置中

## 项目结构分析

  

# webpack

# vue-router路由

## npm安装vue-router

```
npm install vue-router@3
@3 用来指定版本,最新版路由只支持vue3
```

## 导入到项目

### 添加router路由文件夹并添加index.js

```
import VueRouter from 'vue-router'
import Main from "../components/Main";
import Music from "../components/Music";


// 配置路由
export default new VueRouter({
  routes:[
    {
      path:'/Main',
      name:'Main',
      component:Main,
    },
    {
      path:'/music',
      name:'Music',
      component:Music,
    }
  ]
})
```

### 在main.js中添加

```
import route from './router'
import Router from 'vue-router'
Vue.use(Router)
new Vue({
  el: '#app',
  router:route,
  render: h => h(App)
})
```

# vue+elementui

[element-ui官方文档](https://element.eleme.cn/#/zh-CN/component/installation)

## npm 安装element

```
npm i element-ui -S
```

## 导入到项目中

```
import Vue from 'vue'
import App from './App'
// 导入element-ui
import element from 'element-ui'
// 导入样式表
import 'element-ui/lib/theme-chalk/index.css';
// 加载element
Vue.use(element)
Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  render: h => h(App)
})
```

导入后即可使用el的组件

  

# 路由嵌套

# 参数传递及重定向

# 404和路由钩子

  

  

  

自定义组件

```
Vue.component('todo-list',{
props:['todo'],
template:'<li>{{todo.text}}</li>'
})
const app = new Vue({
el: "#app1",
data: {
groceryList: [
{id: 0, text: '蔬菜'},
{id: 1, text: '奶酪'},
{id: 2, text: '随便其它什么人吃的东西'},
],
},
    });
```

```
 <!-- 自定义组件-->
        <div id="app1">
            <ol>
                <todo-list
                           <!-- 循环 -->
                        v-for='item in groceryList'
                        <!-- 使用自定义props传递值-->
                        v-bind:todo="item"            
                        v-bind:key="item.id">
                </todo-list>
            </ol>
        </div>
```

![](学习笔记/Attachments/1657413474465-6fc6a627-e3c9-4407-8eef-eb02c64f145d.png "效果图")

vue按钮的点击事件

```
<div id="app">
    <h1>{{msg}}</h1>
    <button v-on:click="rand">点击我</button>
</div>
<script>
let app1=new Vue({
  el:'app',
  data:{
    msg:'',
  },
  methods:{
    rang:funcation(){
      this.msg=Math.random().toString();
    }
  }

})
</script>
```

v-bind 指令可以用于响应式地更新 HTML attribute

v-if 指令将根据表达式 seen 的值的真假来插入/移除 所在的标签 元素

v-on 指令，它用于监听 DOM 事件 v-on:click 中click是监听的事件名

以上 bind , if , on 皆为参数

<av-on:[eventName]="doSomething"> ... </a>

eventName 动态参数,动态传递参数

```
<div id="app">
<h2 v-bind:[val]="sky">测试动态模板</h2>
  <!-- 点击修改参数   -->
    <button v-on:click="modify">Button</button>
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            color:'red',
            sky:'background: #0c0c0c;',
            val:'style',
        },
        methods:{
            modify:function (){
                this.val='class';
                this.sky=this.color;
            }
        }
    });
</script>
```

### 缩写

指令带有前缀 v-，以表示它们是 Vue 提供的特殊 attribute

```
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

```
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

### 计算属性

```
<div id="app">
    <h1>开始的字符串{{msg}}</h1>
    <h1>函数修改过后的字符串{{rev()}}</h1>
    <h1>计算属性过后的字符串{{reversedMessage}}</h1>
</div>
```

```
<script>
    let vm = new Vue({
       el:'#app',
       data:{
           msg:'hello,vue!',
       },
       methods:{
           rev:function(){
               return this.msg.split('').reverse().join('');
           }
       },
        computed: {
            // 计算属性的 getter
            reversedMessage: function () {
                // `this` 指向 vm 实例
                return this.msg.split('').reverse().join('')
            }
        }
    });
</script>
```

![](学习笔记/Attachments/1657427233784-d17769f5-a324-42ca-8158-9cb7464a8e62.png "结果")

结论:

1. 两种方式的最终结果确实是完全相同的。
2. 不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 msg 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。

例子:

```
<div id="app">
    <h1>开始的字符串{{msg}}</h1>
    <!-- 显示两种时间-->
    <h1>计算属性的时间{{date2}}</h1>
    <h1>方法的时间{{date()}}</h1>
    <!-- 点击修改msg触发视图的更新-->
    <button v-on:click="reverse">点击</button>
</div>

</body>
<script>
    let vm = new Vue({
       el:'#app',
       data:{
           msg:'hello,vue!',
       },
       methods:{
           date:function () {
               return Date.now()
           },
           reverse:function(){
               this.msg=this.msg.split('').reverse().join('');
           },
       },
        computed: {
            date2:function () {
                return Date.now()
            }
        }
    });
</script>
```

结果: 每次点击方法的值在变化,计算属性的值不变

![](学习笔记/Attachments/1657438254167-bbfe69ed-224e-459c-a16c-c531c2952436.png "结果")

## vue中表单的使用

父子点击事件,使用@click.stop可以不触发父控件的点击事件

## 事件修饰符

例如只让嵌套内层的按钮事件运行,不涉及父控件

```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">
```

![](学习笔记/Attachments/1657676599041-5f369464-0b05-4b0b-a6e2-857b180b60ec.png)

# 跨域资源共享问题

我在前端的项目中,想通过axios获取后端的数据,这时候需要后端去设置响应头或者使用注解才能使前端访问返回回来的数据`**@CrossOrigin**`,注解同样是通过改变响应头的方式结局的