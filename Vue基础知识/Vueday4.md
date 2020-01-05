# vue第4天

今天目标：

- 能够运行脚手架项目
- 知道什么是 和 熟练应用**es6模块化**技术
- 知道什么是单文件组件【重要】
  - 熟悉scoped属性作用
  - 掌握父子组件嵌套应用
  - 掌握父、子组件传值技术
- 能够实现简单的spa案例效果



# VueCLI



## 结构文件说明

`目标`：

​	了解项目的各个文件作用

```js
|-- node_modules								// 项目需要的依赖包
|-- public										 // 静态资源存储目录
|   |-- index.html							// 项目主容器文件********
|   |-- favicon.ico							// 项目默认索引图片
|-- src
|   |-- assets									// 放置一些静态资源文件，例如图片、图标、字体 
|   |-- components							// 公共组件目录
|   |-- App.vue									// 顶层根基路由组件
|   |-- main.js									// 项目主入口文件(包括Vue实例也在这)******
|-- .browserslistrc               // 哪些浏览器及版本可以运行该项目的说明
|-- .editorconfig								// 代码规范配置文件
|-- .eslintrc.js								// eslint代码规范检查配置文件
|-- .gitignore									// git上传需要忽略的文件格式
|-- babel.config.js							// babel配置文件
|-- package.json								// 项目基本信息配置文件
|-- package-lock.json						// 依赖包版本锁定文件
|-- README.md								    // 项目普通说明文件
|-- vue.config.js								// webpack 配置文件(与webpack.config.js作用一致)
```

`注意`：

1. public/index.html文件是div容器所在文件

2. src/main.js是Vue实例所在文件




## 项目运行

`目标`：

​	能够把vuecli创建的项目运行起来



`步骤`：

1. 修改src/main.js文件为如下内容

   ```js
   // import Vue from 'vue'
   import Vue from 'vue/dist/vue.common.js'
   // import App from './App.vue'
   
   Vue.config.productionTip = false
   
   new Vue({
     data:{
       msg:'第一次运行VueCli项目'
     }
     // render: h => h(App)
   }).$mount('#app')
   
   ```

   > 注释App相关行内容
   >
   > 引入vue.common.js模块文件

2. 修改public/index.html文件，使用Vue实例的data数据

   ```html
       <div id="app">
         {{msg}}
       </div>
   
   ```

   

3. 修改vue.config.js文件内容如下

   ```js
   module.exports = {
     lintOnSave: false,
     devServer:{
       open:true, // 项目自动启动浏览器查看效果
       port:19922 // 项目会创建http服务，这是服务端口号码，介于1~65535
     }
   }
   
   ```

> open:true  项目运行的时候，会**自动**打开浏览器并呈现效果

4. 在终端中执行命令(需要在项目的根目录下运行如下指令)

   ```
    npm  run  serve
   ```

   > 上述命令需要在项目根目录下运行(01-pro目录下)
   >
   > serve:指令标志是package.json文件配置好了

> 现在项目可以运行了，并且有实时加载的效果，业务文件随时改变，浏览器随时查看到对应效果



`注意`：

​	npm run serve  指令需要在项目**根目录**下运行

`效果`：

![1577930196919](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1577930196919.png)



# ES6模块化

## 介绍

`目标`：

​	知道什么是模块化



`什么是`：

项目中一些**程序代码**经常被其他业务场景使用，为了避免重复开发，就把这些代码设置为**共享模式**，共享模式就是**模块化**。像jquery、axios、vue等等都是模块化的体现，需要的时候直接拿过来用即可 

![1566000527767](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1566000527767.png)





`模块化技术有哪些`：

CommonJS(nodejs)、ES6模块化、AMD、CMD等



`CommonJS`：

CommonJS模块化 是**2009**年发布的，是民间出品的，相对不正规，可以在nodejs中应用

```js
// 导出
module.exports = 对象
// 导入
var obj = require(模块文件)
```



`ES6模块化`：

ES6模块化 是**2015**年**官方正式出品**的，已经被纳入到JavaScript标准里边，也是js未来的标准，由于各种原因 nodejs和浏览器中现在还不能直接使用ES6模块化，要相信未来可以



`AMD模块化`：

 Asynchronous Module Definition  异步模块定义 

**2009**年诞生，可以实现浏览器中应用模块化技术



`CMD模块化`：

Common Module Definition 通用模块定义

**2011**年诞生，阿里公司出品，可以实现浏览器中应用模块化技术



各个模块化应用的场合：

1. commonjs模块化可以应用在nodejs中，浏览器中不可以
2. es6模块化暂时还不能在nodejs 和 浏览器 中使用，但是要相信，未来可以
3. 浏览器中可以运行的模块化名称为 AMD 和 CMD



[AMD、CMD、Commonjs、ES6模块化介绍](https://segmentfault.com/a/1190000015302578)



`注意`：

​	重点掌握两种  ES6 和 CommonJS



## 默认导出和导入

`目标`：

​	掌握默认方式信息的导出、导入操作



`什么是默认导出和导入`：

数据提供者称为 导出

数据使用者称为 导入

在一个js文件中，通过一个**对象**把全部的数据导出出去，就是**默认导出**

对默认导出的成员进行接收就是默认导入



`模块`：

一个js文件就是一个模块，前提是该文件有做**导出**动作



`导出语法`：

```js
// 导出：
export default  对象
```

`导入语法`：

```js
// 导入：
import 名称  from  模块文件名字
```



`注意`：

1. 一个模块中 默认导出 只能进行**一次**
2. es6模块化现在只可以在**VueCli项目**中使用
3. 默认必须  导出一个对象



`示意案例`：

01-默认导出.js

```js
var a = 100
var b = 200
var c = 300
function getInfo(){
  return 'hello'
}

// 当前文件对外部导出内容，以便被使用
// 01. 默认导出：通过一个大对象把所有的信息都导出出去
// CommonJS: module.exports = {}
// ES6: export default 对象
export default {
  name:'kitty',
  age:4,
  walk:function(){
    console.log('走直线')
  }
}


```



main.js导入

```js
// 01.默认导入
// import 名称 from '模块js文件路径名'
import cat from './modules/01-默认导出.js'
console.log(cat)

```



## 按需导出和导入

`目标`：

​	掌握按需方式信息导出、导入技术



`什么是按需`：

一个js模块中，定义了N多的成员信息，根据需要，用哪个就导出哪个，这就是按需



哪些成员可以做按需导出导入处理？

答： **常量**、**对象** 、**函数** 三种信息可以做模块化应用 (var、let等变量用于模块化没有意义)



`导出语法`：

```js
export const  a = 10		// 常量
export function ab(){}  // 函数
export const  b = 20
export const cat = {name:'kitty',age:5} // 对象
...

```

> 注意：一般按需只做常量、函数导出，var/let变量不导出，本身没有意义



`导入语法`：

```js
import {xx,yy,zz} from 模块文件
import {xx as kk,yy as mm,zz as qq} from 模块文件     // 根据需要可以设置别名

```



`注意`：

1. xx,yy,zz代表被导入的成员名称，与导出的要求**一致**

2. 成员不用全部都导入，根据需要，导入 **1个或多个或全部** 都可以
3. 如果导入进来的成员名称 与 当前环境名称 有冲突，可以使用 **as** 设置别名



`案例`：

对按需导出、导入进行简单使用

导出：

```js
// 02. 按需导出，根据需要导出相关的成员
// 语法： export 常量声明/函数声明/对象声明
//     (module.exports.xx = yy)
var a = 100
var b = 200
let c = 300
function getInfo(){
  return 'hello'
}

export const city = 'beijing'
const people = 2000

export function getApple(){
  return '国光苹果'  
}
function getPear(){
  return '雪花梨'
}

export const tiger = {color:'yellow and black'}
const dog = {hobby:'看家'}



```

导入：

```js
// 02. 按需导入
// import {名称，名称。。} from '模块文件'
import {city as ct,getApple,tiger} from './modules/02-按需导出.js'
console.log(ct)
console.log(getApple)
console.log(tiger)

```





## 默认和按需同时导出和导入

`目标`：

​	掌握 默认和按需 同时导出、导入技术



有时，在一个模块中 默认 和 按需 导出会同时存在



`导出语法`：

```js
export const  a = 10    // 按需导出
export function ab(){}  // 按需导出
export default  对象/{}  // 默认导出
export const  b = 20
export function abc(){}

```

> 1. 一个模块只能  默认导出**一次**，按需导入可以设置多次
> 2. 默认导出  的语句没有摆放位置要求



`导入语法`：

```js
// 1) 分别导入
import 名称  from  模块
import  {xx,yy}  from  模块

// 2) 一并导入
import 名称,{xx,yy} from 模块

```

> 一并导入必须是 默认在"前"，按需在"后"



`案例`：

对默认和按需同时导出和导入做简单应用

导出：

```js
// 03. 默认和按需同时导出
// 语法： export 常量声明/函数声明/对象声明
//     (module.exports.xx = yy)
var a = 100
var b = 200
let c = 300
function getInfo(){
  return 'hello'
}
// 默认
export default {
  banji:'92期前端'
}

// 按需
const city = 'beijing'
export const people = 2000

function getApple(){
  return '国光苹果'  
}
export function getPear(){
  return '雪花梨'
}

const tiger = {color:'yellow and black'}
export const dog = {hobby:'看家'}




```

导入：

```js
// 03. 默认和按需同时导入
// A. 分别导入(具体参考 01 和 02)
//    import 名称 from 模块
//    import {名称,名称……} from 模块
// B. 一并导入
//    import 名称,{名称，名称……} from 模块
import obj,{people as pp,getPear} from './modules/03-默认和按需同时导出.js'
console.log(obj)
console.log(pp)
console.log(getPear)

```





## 没有导出应用

`目标`：

​	知道如何处理非模块文件



项目开发时有的文件没有做导出动作(例如 css、less)，这样文件可以称为为 非模块文件，那么可以通过如下方式做导入

```
import  文件路径名

```



使用示例：

导出：

```js
// 4. 没有导出
for(var i=0; i<5; i++){
  console.log(i)
}



```

导入：

```js
// 4. 导入空的模块
// import  xx模块名字
import './modules/04-没有导出.js'

```



## 小结

1. 导出，就是共享内容**提供方**
2. 导入，就是共享内容**使用者**
3. **默认导出**就是通过一个对象把全部的内容都提供出来 export default 对象
4. **按需导出**就是需要哪个信息就导哪个出来，不需要的不操作
5. 按需导出导入的相关元素：常量、函数、对象
6. 一个模块只能做**一次默认**导出，但是可以做**多次按需**导出
7. 默认导出必须导出 对象



# 单文件组件 (1000)

## 什么是

`目标`：

​	知道什么是单文件组件

`什么是`：

​	把一个组件的全部内容汇合到**一个文件**中，文件名字是以`.vue`结尾的就称作**vue单文件组件**



`普通组件的缺点`：

普通vue组件  其代码 和 其它JS代码逻辑掺杂在一块儿，不易维护，优势没有发挥到极致！

单文件组件才是重点、主角。



## 最简单使用

`目标`：

​	创建一个最简单的单文件组件并应用



`步骤`：

1. 通过vuecli创建一个空的项目并运行

2. 创建单文件组件   src/components/01-第一个单文件组件.vue文件，内容如下：

   ```html
   <template>
     <div id="page">
       <!--结构：当前组件拥有的html标签集合体现-->
       <!--要求内部有唯一根元素-->
       <ul>
         <li>1</li>
         <li>2</li>
         <li>3</li>
       </ul>
     </div>
   </template>
   
   <script>
   // 行为
   </script>
   
   <style>
   /*给结构设置css样式*/
   </style>
   
   
   ```

3. src/main.js文件 **导入**、**注册** 组件，内容如下：

```js
// 导入单文件组件(ES6的默认导入)
import ComPage from './components/01-第一个单文件组件.vue'

new Vue({
  // 注册单文件组件
  components:{
    // 名称:组件模块
    'com-page':ComPage
  },

```

> 注意：
>
> 1. vue要引入 vue.common.js的文件包

4. public/index.html文件 使用 单文件组件

```html
    <div id="app">
      {{msg}}
      <hr />
      <!-- 应用单文件组件 -->
      <com-one></com-one>
    </div>

```



`注意`：

1. 单文件组件的模板内容必须通过template绘制,内部要求有**唯一根元素**
2. 单文件组件文件本身比较特殊，必须借助**vuecli脚手架项目**才可以运行



效果：

![1577935517435](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1577935517435.png)

## 组织结构

`目标`：

​	清楚单文件组件的组成结构



单文件组件的构成：

```html
<template>
  相关html标签
</template>

<script>
  export default {
    data,
    methods,
    computed,
    filters,
    created,
    ……
  }
</script>

<style>
  css样式内容
</style>

```



一个单文件组件涉及有如下3部分：

1. template标签，内部要求有**唯一根元素**(推荐div)，template是html5标签，只运行，浏览器源代码不显示
2. script标签：该标签内部可以执行普通js代码，但是最主要的是内部可以通过**export  default {}** 导出一个对象，该对象的成员完全可以参考 **Vue实例**，类似 data、methods、created、filters、components等等都可以应用，各个成员都是为template模板服务的
3. style标签:设置css样式，作用给template内部的html标签使用



`注意`：

​	script和style如果不需要，可以不设置，template必须要写



组件script设置成员应用：

模板代码：

`components/02-组件成员.vue`

```vue
<template>
  <div id="page2">
    <!--结构：当前组件拥有的html标签集合体现-->
    <!--要求内部有唯一根元素-->
    <ul>
      <li @click="up">{{prev}}</li>
      <li>1</li>
      <li>2</li>
      <li>3</li>
      <li>{{next}}</li>
    </ul>
  </div>
</template>

<script>
// 单文件组件  就是 特殊的Vue实例，可以参考配置相关成员
// 行为：具体是给template提供服务的，有固定的ES6模块化导出动作
export default {
  // 成员(具体可以参考Vue实例)
  data:function(){
    return {
      prev:'上页',
      next:'下页',
    }
  },
  methods:{
    up(){
      console.log('进入上一页')
    }
  },
  created(){
    console.log('单文件组件的created自动执行了')
  }
  // filters:,
  // directives:,
  // ……
}
</script>

<style>
/*给结构设置css样式*/
#page2{border:2px solid orange;}
</style>


```

main.js引入和注册

```js
import ComTwo from './components/02-成员结构.vue'

Vue.config.productionTip = false

new Vue({
  components:{
    // 名称:{配置对象成员} // 普通组件注册
    // 名称:组件模块名称 // 单文件组件注册
    'com-one':ComOne, // 单文件组件注册
    'com-two':ComTwo,
  },

```

index.html应用

```html
      <hr>
      <com-two></com-two>

```

效果：

![1577936173914](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1577936173914.png)





## 私有注册应用

`目标`：

​	制作一个table表格的组件， template、script、style 都使用，Vue实例要求应用data、methods、created等成员



`私有方式注册语法`：

```js
new Vue({
  components:{
		组件名称: 组件模块,
		组件名称: 组件模块,
		组件名称: 组件模块,
	}
})


```



`步骤`：

1. 创建组件文件 src/components/03-table.vue

   ```html
   <template>
     <div>
       <table>
         <tr>
           <td>序号</td>
           <td>名称</td>
           <td>操作</td>
         </tr>
         <tr>
           <td>{{goods.id}}</td>
           <td>{{goods.name}}</td>
           <td><button @click="del()">删除</button></td>
         </tr>
       </table>
     </div>
   </template>
   
   <script>
   // es6模块化默认导出
   // export default 对象 ，
   //  该对象的成员可以参考Vue实例，因此data/methods/filters/created/computed等
   //  并且是为 上述template做服务的
   export default {
     data(){
       return {
         goods:{id:203,name:'奔驰汽车'}
       }
     },
     methods:{
       del(){
         console.log('该商品已经被删除了')
       }
     },
     created(){
       console.log('table表格的数据已经准备好了')
     }
   }
   </script>
   
   <style>
   /*css样式，具体也是为template做服务的*/
   td{color:orange;}
   </style>
   
   
   ```

   

2. 在main.js中引入组件  并  通过 私有方式注册 好

   ```js
   import ComTable from './components/03-Table'
   
   Vue.config.productionTip = false
   
   new Vue({
     // 把导入进来的组件给注册好
     // components:{组件名称:{配置对象成员}} // 普通组件
     // components:{
     //   组件名称:组件模块,
     //   组件名称:组件模块,
     // } // 单文件组件
     // 组件名称推荐是“xx-yy”格式
     components:{
       'com-one':ComOne,
       'com-table':ComTable,
     },
   
   
   
   ```

   

3. 在public/index.html中应用组件

   ```html
         <com-table></com-table>
   
   
   ```

`注意`：

​	vuecli脚手架已经把一些通用文件的后缀设置好了，像.js、.vue等可以不设置



小结：

1. 单文件组件，每个组件都有3个部分（template/script/style）
2. template 是固定标签，内部要求有**唯一根元素**
3. script  导出一个对象     对象成员可以参考Vue实例
4. style 设置样式
5. 组件需要被main.js文件 引入、注册，之后index.html应用





## 全局注册介绍

`目标`：

​	了解全局方式单文件组件的注册和应用



`注册`：

main.js

```js
import 组件模块  from  'xxx.vue'       // .vue后缀可以不设置，系统已经准备好了

// 全局方式声明组件
Vue.component(组件名称, 组件模块)
Vue.component(组件名称, 组件模块)

var vm = new Vue({……})

```

`注意`：

​	组件注册的代码需要在new Vue()上边

​	

## 一并应用两个

`目标`：

​	一并创建、使用两个组件



在实际项目中一并使用多个组件的情形也是存在的，并且很多，例如 要求同时使用 分页组件 和 table表格组件



`步骤`：

1. 创建组件

   src/components/04-First.vue

   ```html
   <template>
       <div>
         <p>我是大哥</p>
       </div>
   </template>
   
   <script>
   export default {
     name: ''
   }
   </script>
   
   <style lang="less">
   p{color:red;}
   </style>
   
   
   ```

   src/components/04-Second.vue

   ```html
   <template>
       <div>
         <p>我是小弟</p>
       </div>
   </template>
   
   <script>
   export default {
     name: ''
   }
   </script>
   
   <style lang="less">
   p{color:blue;}
   </style>
   
   
   ```

   > 注意：它们有使用相同名称的css选择器进行样式控制

2. 在main.js中**引入、注册**两个组件

   ```js
   import ComFirst from './components/04-First.vue'
   import ComSecond from './components/04-Second.vue'
   
   Vue.config.productionTip = false
   
   // 全局方式注册
   // Vue.component(名称,组件模块名称)
   Vue.component('com-two2',ComTwo)
   
   new Vue({
     // 私有方式注册组件
     components:{
       // 名称:{配置对象成员} // 普通组件注册
       // 名称:组件模块名称 // 单文件组件注册
       'com-one':ComOne, // 单文件组件注册
       'com-two':ComTwo,
       'com-first':ComFirst,
       'com-second':ComSecond,
     },
   
   ```

   

3. 在public/index.html中使用两个组件

   ```html
         <hr>
         <com-first></com-first>
         <com-second></com-second>
   
   ```

![1577938318332](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1577938318332.png)

`注意`：

​	效果是不对的，First应该显示红色，Second应该显示蓝色，但是现在都显示蓝色了	



## scoped属性

`目标`：

​	知道scoped属性的作用和原理



默认情况下，vue单文件组件的style样式是[全局的]()，

如果在一个应用中使用了**多个**单文件组件，它们使用<span style="background-color:yellow;">相同选择器</span>为相同的元素设置了style样式，那么只有一个会起作用 (后者会覆盖前者)



`scoped作用`：

​	使得style的样式针对自己组件生效

`原理`：

​	给每个style标签都设置一个`scoped`属性，这样组件的各个html标签解析出来后都会带有一个与其他单组件标签不同的 <font color=red>data-v-xxx</font> 的唯一属性名称，style样式设定也会自动与这个的data-v-xxx联系起来，这样就使得style样式只针对自己的组件起作用了

![1577939227257](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1577939227257.png)



`语法`：

```html
<style  scoped>
  xxxxx
</style>


```



`注意`：

尽管有scoped，但是不同的组件间还是尽量**不要使用相同名称**的选择器



上午总结：

- 能够运行脚手架项目

  - index.html
  - main.js  
  - vue.config.js : open/port

- 知道什么是 和 熟练应用**es6模块化**技术

  - es6(VueCli)、commonjs(nodejs)、CMD、AMD
  - 代码共享机制
  - 导出、导入
  - 默认  export default 对象   、 import  名称 from 模块
  - 按需  export 常量/函数/对象    import {名称 as 别名，名称……} from 模块
  - 默认和按需同时存在 导入：import 名称， {名称，名称……} from 模块
  - 没有  import xxx
  - 

- 知道什么是单文件组件【重要】

  结构：template(唯一根元素)、script(export default {参考Vue实例})、style(scoped)

  (template是html5发布的html标签，只运行，浏览器不显示，雷锋标签)

  熟悉scoped属性作用

  组件使用步骤：创建、导入/注册、应用



## 父子嵌套组件应用

多个组件一并应用，根据业务需要，它们可以是平行**兄弟**关系， 也可以是嵌套的**父子**关系，

下边就讲解下组件**嵌套**的应用

![1560827125478](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1560827125478.png)



`目标`：

​	应用两个组件，它们是父子嵌套关系的

`步骤`：

1. 创建两个组件 05-Fat.vue  和  05-Son.vue  内容简单填充

   05-Son.vue内容：

   ```html
   <template>
       <div id="son">
         <h2>我是小子郭小达</h2>
       </div>
   </template>
   
   <script>
   export default {
     name: ''
   }
   </script>
   
   <style lang="less" scoped>
   #son{
     width: 390px;
     height: 70px;
     border:2px solid blueviolet;
   }
   </style>
   
   
   ```

   

05-Fat.vue内容，对子组件Son做引入、注册、使用：

```html
<template>
    <div id="fat">
      <h2>我是老子郭达</h2>
      <!-- 使用组件 -->
      <com-son></com-son>
    </div>
</template>

<script>
// 把小子组件给导入进来
import ComSon from './05-Son.vue'

export default {
  // 组件注册(私有方式)
  components:{
    'com-son':ComSon
  },
  name: ''
}
</script>

<style lang="less" scoped>
#fat{
  width: 400px;
  height: 200px;
  border:2px solid green;
}
</style>


```

   

2. 在src/main.js中 引入、注册  05-Fat.vue

   ```js
   import ComFat from './components/05-Fat.vue' // 导入老子
   
   Vue.config.productionTip = false
   
   // 全局方式注册
   // Vue.component(名称,组件模块名称)
   Vue.component('com-two2',ComTwo)
   
   new Vue({
     // 私有方式注册组件
     components:{
       // 名称:{配置对象成员} // 普通组件注册
       // 名称:组件模块名称 // 单文件组件注册
       'com-one':ComOne, // 单文件组件注册
       'com-two':ComTwo,
       'com-first':ComFirst,
       'com-second':ComSecond,
       'com-fat':ComFat,
     },
   
   ```

   

3. 在public/index.html 应用 041-Fat.vue

   ```html
         <!-- 显示老子组件 -->
         <com-fat></com-fat>
   
   ```



关系：index.html容器------------->父组件---------------->子组件



`嵌套本质`：

​	一个单文件组件可以  **引入**、**注册**、**使用** 另外一个单文件组件



`效果`：

![1577948156773](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1577948156773.png)





`扩展`：

一个 单文件组件内部  导入、注册、使用 其他组件，可以通过  “成员简易赋值+大驼峰名称“ 方式注册

使用的时候有两种方式：中横线 或 大驼峰，具体如下：

```vue
<!--使用两种形式：-->
<xxx-yyy></xxx-yyy>   或  <XxxYyy></XxxYyy>  // 推荐前者

<script>
import XxxYyy from 'xxxyyy.vue'
  export default {
    components:{
  		XxxYyy   // 全写XxxYyy  :   XxxYyy
		}
  }
</script>


```



## vscode安装扩展

![1560827238530](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1560827238530.png)

给vscode安装如下扩展：

1. Vetur：使得vue文件内容有**代码高亮**，同时也可以做代码**格式化**操作

2. Vue 2 Snippets：设置代码片段，vscode默认不能给vue文件做代码片段设置，需要安装此扩展

   扩展安装完毕，需要给vscode编辑器执行如下步骤：

   设置 ------> user snippets ----->  vue.json,设置如下内容

   ```js
   {	
   	"Create vue template": {
   		"prefix": "vuec",
   		"body": [
   		 "<template>",
   		 "  <div>$1</div>",
   		 "</template>",
   		 "",
   		 "<script>",
   		 "  export default {",
   		 "}",
   		 "</script>",
   		 "",
   		 "<style lang=\"less\" scoped>",
   		 "</style>",
   		 "",
   		],
   		"description": "Create vue template"
   	}
   }
   
   ```

   > 之后，在vue文件中就可以通过 “vuec” 代码片段创建组件结构内容了



## 父给子传值(1500)

父组件可以  引入、使用 子组件，从业务上看，该父组件有可能对子组件有**个性化**需求，为了体现组件的**灵活多变**，可以通过**传值**实现

例如

​	父组件多次使用按钮组件，每次要求按钮的文字显示不同颜色，就可以通过传值实现



`语法`：

父组件要在子组件标签上通过**属性值**方式传值

```html
<子组件标签 name=value name=value name=value></子组件标签>

```

子组件接收并应用值，具体通过props接收

```html
<!--在模板中应用传递来的数据-->
<input :style="{color:xx}">

<script>
  export default {
    // 通过props接收父传递过来的数据,注意各个名称需要使用单引号圈选
    props:['xx','xx','xx']
  }
</script>

```



`目标`：

利用父给子传值实现应用**按钮组件**效果，多次使用，每次**按钮颜色**都不一样



步骤：

1. 制作按钮子组件 05-Button.vue，设置一个简单按钮

   ```html
   <template>
       <div id="btn">
         <button :style="{color:yan}">我是按钮</button>
       </div>
   </template>
   
   <script>
   export default {
     name: '',
     // 接收父给当前子组件传递的数据
     // props:[名称，名称，名称……]
     // 注意：名称就是值的属性名
     props:['yan']
   }
   </script>
   
   
   ```

   

2. 在05-Fat.vue  中  对 05-Button.vue 做 引入、注册、使用(3次)，并同时传值

   ```html
         <!-- 使用按钮组件 -->
         <!-- 当前Fat父组件要个性化定制Button子组件样式 -->
         <!-- 具体通过父给子传值方式实现 -->
         <!-- 传值：属性值 -->
         <com-button yan="red"></com-button>
         <com-button yan="blue"></com-button>
         <com-button yan="orange"></com-button>
       </div>
   </template>
   
   <script>
   // 把小子组件给导入进来
   import ComSon from './05-Son.vue'
   import ComButton from './05-Button.vue'
   
   export default {
     // 组件注册(私有方式)
     components:{
       // 组件名称:组件模块
       // 'com-son':ComSon
       // 对象成员简易赋值
       ComSon, // 全写：ComSon:ComSon
       ComButton
     },
   
   ```



`效果`：

![1577950388270](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1577950388270.png)

`注意`：

在子组件内部**必须**通过**props**接收传递过来的信息



## 扩展

### this是谁

`目标`：

​	知道this在组件内部指向谁



`this指向`：

1. **Vue实例内部**指向 new  Vue()

2. **组件内部**指向**组件实例(构造器VueComponent)**，同时该VueComponent有**原型继承Vue**，

   A. 组件内部this可以访问自己的  的data、methods、computed等成员

   B. Vue如果有成员，那么组件的this也可以去访问

   > 构造器：实例化对象的构造函数，就是构造器



### render方法扩展使用

`目标`：

​	了解render方法的二种使用形式



具体如下：

```js
new Vue({
  render:h=>h('p','ok')  // 要生成 <p>ok</p> 并覆盖渲染 div容器
  render:h=>h(组件模块) // 要使用一个组件模块 对 div容器 进行覆盖渲染
})

```



`说明`：

​	真实项目中一般喜欢创建一个名称为App.vue的组件，并用它去渲染覆盖 div容器

​	所有业务逻辑实现都从App.vue开始，div容器 就是一个空壳摆设



通过render对App.vue组件覆盖渲染应用,所有业务通过App.vue展开：

1. 在main.js文件中，要使用默认的vue    **import  Vue  from 'vue'**
2. 所有业务组件都通过App.vue文件做应用( 组件的引入、注册、使用)



# SPA

## 介绍和优缺点

`目标`：

​	了解什么是SPA及优缺点



`什么是`：

概念定义：SPA英文全称是Single Page Application, 中文翻译是 “单页面应用程序项目”

通俗的理解是：一个网站只有一个Web页面；网站的所有功能都在这个唯一的页面上进行展示与使用



`好处`：

1. 实现了前后端**分离模式**(目前最好的开发模式)开发，各司其职；提高了开发效率；
2. 用户体验好，页面部分内容发生变化只需要更新局部即可(非刷新整个页面)；

`缺点`：

1. 对SEO(搜索引擎)不是很友好，网站从开始到结束始终访问一个程序文件，造成搜索引擎不给检索， 但是有解决方案，再者后台系统应用本身对seo不做要求

   (ssr处理seo阅读了解： https://www.jianshu.com/p/fcb98533bc18?tdsourcetag=s_pctim_aiomsg )

2. 每次应用运行时，需要一次性把全部的html、js、css等内容加载进来，因此会造成页面一开始请求速度较慢的问题(首页较慢，后续页面正常)



`spa应用场合`：

后台管理系统 和 移动端项目，它们共同特点是供访问的页面总数量比较少，一般小于500个





## 组件切换案例

`目标`：

​	模拟vue-cms项目，

​	在页面上绘制3个按钮(首页、电影、音乐) 和 对应的组件页面，各个按钮被单击后要显示对应的组件



`完整url`：

 URL为统一资源定位符 

一个完整的URL可能是下面这个样子的
http://www.xxxyyzzz.cn:80/stu/index.html?name=xxx&age=25#stdx

 **①[传输协议]** ：http、https、ftp等

 **②[域名]** Domain Name
一级域名(顶级域名) [www.qq.com](http://www.qq.com/)
二级域名 [sports.qq.com](http://sports.qq.com/)
三级域名 [kbs.sport.qq.com](http://kbs.sport.qq.com/) 

 **③[端口号]** 

 HTTP => 默认端口号80
HTTPS => 默认端口号443
FTP => 默认端口号21 

 **④[请求路径名称]** 斜杠之后-问号之前 path 

 例如 /stu/index.html 

 **⑤[问号传参]** ?xxx=xxx&…

 客户端把信息传递给服务器的一种方式 

 **⑥[哈希值]** #xxx 

 哈希值一般都跟客户端服务器交互没啥关系，主要用于页面中的**锚点定位**和**Hash组件切换** 



`步骤`：

1. 利用vuecli创建空项目，配置vue.config.js文件为如下内容

   ```js
   module.exports = {
     lintOnSave: false,
     devServer: {
       open: true,
       port: 19922 // 区间 1~65535 之间
     }
   }
   
   ```

   > open：自动开启浏览器查看效果
   >
   > port：服务端口号码

2. 在src/components目录中创建   Home.vue Movie.vue Music.vue  的3个业务组件，内容分别为如下：

   ```html
   <template>
       <div>
         首页展示
       </div>
   </template>
   
   ```

   ```html
   <template>
       <div>
         电影展示
       </div>
   </template>
   
   ```

   ```html
   <template>
       <div>
         音乐展示
       </div>
   </template>
   
   ```

3. 在 src/App.vue 中设置如下内容

   ```html
   <template>
       <div>
         <h2>SPA按钮组件切换应用</h2>
         <p>
           <!-- 给超链接设置#锚点信息，使得与组件联系并显示
                #锚点作用：
                1. 定位页面局部内容显示  #apple  <div id="apple"></div>
                2. 组件切换的标志
                url统一资源定位器：
                http://www.jd.com:8080/shop/car/txt.html?name=htc&price=1200#pear
                http:请求协议(https  ftp)
                :8080: 服务端口号码，80是默认的，可以不设置
                www.jd.com: 域名地址
                /shop/car/txt.html: 请求的文件路径名信息
                ?name=htc&price=1200: 请求参数
                #pear: #锚点信息
   
                #锚点两种方式设置：
                1. #xxx
                2. #/xxx   推荐使用
            -->
           <a href="#/hm">首页</a>
           <a href="#/mv">电影</a>
           <a href="#/ms">音乐</a>
         </p>
         <p id="cont">
           <!-- 显示业务组件， 放置一个"代表",分别代表各个业务组件-->
           <!-- <component is="组件名称"></component> -->
           <component :is=" flag "></component>
         </p>
       </div>
   </template>
   
   <script>
   // 导入各个业务组件
   import Home from './components/Home.vue'
   import Movie from './components/Movie.vue'
   import Music from './components/Music.vue'
   
   
   export default {
     // 把onhashchange相关代码挪到组件内部，使得可以操控data成员
     created(){
       // 浏览器中有一个名称为“onhashchange的事件”，可以感知#锚点发生变化
       // 事件函数需要是箭头形式，使得内部this指向外部，是组件实例对象
       window.onhashchange = ()=>{
         // 获得变化后的#锚点(hash)信息
         var hh = window.location.hash
         // console.log(hh)  // #/ms
   
         if(hh==='#/hm'){
           this.flag = 'home'
         } else if(hh === '#/mv'){
           this.flag = 'movie'
         } else {
           this.flag = 'music'
         }
       }
     },
     data(){
       return {
         // 设置显示组件标志
         flag:'home'
       }
     },
     // 注册组件
     components:{
       'home':Home,
       'movie':Movie,
       'music':Music,
     }
   }
   </script>
   
   <style lang="less" scoped>
   a{margin-right:10px;}
   #cont{ width: 400px; height: 300px;border:2px solid orange; }
   </style>
   
   
   ```

4. main.js文件代码，对App.vue做导入render使用

   ```js
   import Vue from 'vue' // 删减版Vue，内部没有template，进而导致普通的渲染不成功
   
   // 导入App.vue组件
   import App from './App.vue'
   
   Vue.config.productionTip = false
   
   new Vue({
     // 通过一个组件模块去覆盖div容器
     render:h=>h(App)
   }).$mount('#app')
   
   
   ```



`说明`：

 1. 在App.vue中设置component标签，代表各个业务组件要在此显示，component可以理解为是“占位符”

    ```html
    <component :is=" 组件名字  "></component>   // 设置显示各个子组件
    
    ```

> is属性要接收一个具体组件名称，控制显示组件
>
> is属性本身需要冒号属性绑定方式设置

2. window.onhashchange事件，可以感知#锚点发生变化
3. window.location.hash 获得浏览器当前变化后的锚点数据



`注意`：

1. 在main.js文件中直接引入使用vue   **import Vue from 'vue'**
2. App.vue中设置window.onhashchange=**箭头函数**，箭头函数使得内部this与外部保持一致，都是组件实例

效果：

![1577956421814](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1577956421814.png)



`小结`：

1. 业务组件有3个，App.vue是根基组件，它们形成父子关系
2. 通过**#锚点**设置标志信息 (按钮和组件 的对应关系)
3. 通过component的is属性设定哪个组件显示
4. 通过 window.onhashchange 感知 锚点发生变化
5. 通过 window.location.hash 获得变化后的锚点数据



下午总结：

- - 

  - - 掌握父子组件嵌套应用

      一个组件 可以 导入、注册、使用 另一个组件，它们就是父子关系

    - 掌握父、子组件传值技术【重要】

      父：属性值

      子：props接收、应用数据

  - 能够实现简单的spa案例效果

    \#锚点

    component占位符标签  is

    window.onhashchange

    window.location.hash



作业：

1. 【父给子传值】给button按钮子组件不仅传递文字**颜色**信息，还要传递不同的**文字大小**、**宽**、**高**、**背景**色等信息(传递多个值)

   ![1576764165807](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1576764165807.png)

2. 通过spa完成**vue-cms**案例效果，4个按钮，4个组件页面

   公共的头部 和 脚步按钮区域，中间是要显示的各个子级组件

![1572841598765](D:/就业班学习资料/Vue基础/Vue day4/笔记/img(online)/1572841598765.png)

