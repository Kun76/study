# vue第5天

今天目标：

- 安装、配置**路由**
  - 知道什么是**重定向**
  - 能够理解、使用**子路由**
  - 能够理解、使用**路由传参**
  - 能够理解、使用**编程式导航**
  - 能够理解、使用**守卫**
- 知道如何应用**WebStorage(localStorage/sessionStorage)**
- 实现管理员强制登录系统案例



# 路由【重要】

## 什么是

`目标`：

​	知道什么是路由



`什么是`：

答：路由是一个js功能模块，用于解决SPA项目组件切换显示问题的，本身对**组件切换**的各个底层技术有做**封装**，是更成熟组件切换解决方案，使用起来更高级、方便。

路由封装的元素有：

- \#锚点超链接
- component占位符标签
- window.onhashchange
- window.location.hash
- 等等



路由是开发SPA项目必备技能之一

![1576928824671](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/1576928824671.png)



## 安装

`目标`：

​	在项目中安装路由



安装路由两种方式：

1. vuecli创建项目的时候(勾选Router项目即可，这是会增加一个步骤，选择n即可)

2. 单独安装

   > 两种方式本质完全一样



单独安装：

```bash
npm i vue-router
```



`注意`：

​	以上安装依赖包的指令需要在**项目根目录**执行





`Tip提示`：

> 依赖包：通过npm i   装的东西是依赖包(功能包)，每个依赖包内部有**多个**功能模块
>
> 模块：一个js文件内部有做"模块化导出"动作，这个js文件就是一个(功能)模块 
>
> 例如：npm  install  依赖包/功能包



## 案例应用

`目标`：

​	应用 路由 升级SPA案例的组件切换效果



具体步骤如下：

### 具体配置

在`src/main.js`中给路由做如下配置：

1. import引入 路由
2. import引入 各个业务组件模块
3. Vue.use(路由模块) 注册路由组件
4. 创建路由对象，通过path、component设置#锚点 与 组件的联系
5. 在Vue实例内部 挂载 router路由对象



具体代码：

```js
import Vue from 'vue'
import App from './App.vue'

// 1. 引入路由模块
import VueRouter from 'vue-router'

// 2. 导入各个业务组件
import Home from './components/Home.vue'
import Movie from './components/Movie.vue'
import Music from './components/Music.vue'

// 3. 注册路由中的相关组件
//    路由中有两个组件( <router-link>  <router-view> )被进场使用
// A. 单独注册
// Vue.component('router-link',XXX)
// Vue.component('router-view',XXX)
// B. 一并注册，一次性把所有的组件都注册好，更高效
Vue.use(VueRouter) // 插件

// 4. 创建一个路由对象
const router = new VueRouter({
  // #锚点信息就是路由地址(路由地址不用设置#号，内部已经封装好了)
  // routes:固定名称，用于设置 路由地址(#/xxx) 与 组件 的对应关系
  routes:[
    // path和component都是固定名称
    // {path:路由地址, component:被显示的组件模块}
    {path:'/home', component:Home},
    {path:'/movie', component:Movie},
    {path:'/music', component:Music},
  ]
})


Vue.config.productionTip = false

new Vue({
  // 5. 挂载路由对象
  //    对created、window.onhashchange、window.location.hash、if else if等的封装
  router, // 全写：router:router
  render: h => h(App)
}).$mount('#app')

```



### 设置切换按钮和占位符

在App.vue中设置按钮 和 占位符

按钮：

```html
<router-link to="/home">首页</router-link>
<router-link to="/user">会员</router-link>
<router-link to="/movie">电影</router-link>
```

> 通过router-link设置  按钮和#锚点信息

占位符：

```html
<router-view></router-view>
```

> 通过 router-view 设置组件显示占位符



App.vue具体代码：

```html
<template>
    <div>
      <h2>App根组件-路由实现spa</h2>
      <p>
        <!-- 切换按钮设置-通过路由组件实现 -->
        <!-- <a href="#/home">首页</a> -->
        <!-- <router-link to="设置当前按钮连接到锚点信息">首页</router-link> -->
        <!-- router-link组件默认会生成超链接a标签 -->
        <router-link to="/home">首页</router-link>
        <router-link to="/movie">电影</router-link>
        <router-link to="/music">音乐</router-link>
      </p>
      <p id="cont">
        <!-- 设置业务组件显示-路由组件实现代表占位符 -->
        <router-view></router-view>
      </p>
    </div>
</template>

<script>
export default {
  name: ''
}
</script>

<style lang="less" scoped>
#cont{
  width: 400px;
  height: 200px;
  border:2px solid orange;
}
</style>

```



效果：

![1578017392619](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/1578017392619.png)



### 执行过程分析

路由执行过程：

- 用户点击 页面的 路由链接router-link，点击的一瞬间，就会修改 浏览器 地址栏 中的 #号 锚点地址信息，
- \#锚点变化了 会立即被  路由 监听到   (路由有封装onhashchange事件)
- 之后 路由 会获取变化后的#锚点信息  (路由有封装window.location.hash)
- 再之后 路由 根据#锚点信息找到对应 的组件 (在main.js中可知)
- 最后组件是通过路由占位符router-view显示的



### 重定向

`目标`：

​	知道什么是重定向 并 熟练使用



`什么是`：

用户第一次访问网站页面("/根目录"首页)时，地址栏里边没有“#锚点”的信息，
也就没有对应的组件用于显示，用户体验不好，现在可以通过重定向实现一种效果，即当浏览器没有任何 #锚点信息时，我们默认也给显示一个组件

重定向：使得一个路由地址A与另一个路由地址B联系起来，执行A的时候会跳转执行B

![1576930005019](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/1576930005019.png)

`语法`：

关键字：redirect

```js
var router = new VueRouter({
  routes:[
    // {path:'/', redirect:'跳转到的路由地址'}
    {path:'/', redirect:'/home'},
    {path:'/home', component:Home},
    ……
  ]
})
```

> 执行  /根目录路由地址  时，就跳转执行 /home路由地址 ，进而把对应的组件给显示出来了



`注意`：

1. 不仅 "/" 可以被重定向，其他普通路由地址互相也可以重定向
2. 重定向会使得路由再次发生调用请求



`案例`：

​	实现在没有任何#锚点信息，就显示首页组件效果

```js
const router = new VueRouter({
  // #锚点信息就是路由地址(路由地址不用设置#号，内部已经封装好了)
  // routes:固定名称，用于设置 路由地址(#/xxx) 与 组件 的对应关系
  routes:[
    // path和component都是固定名称
    // {path:路由地址, component:被显示的组件模块}
    // {path:'路由地址A', redirect:'路由地址B'}  // A 重定向执行 B
    {path:'/', redirect:'/home'},
    {path:'/home', component:Home},
    {path:'/movie', component:Movie},
    {path:'/music', component:Music},
  ]
})
```



### 按钮高亮设置

![](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/5-8-4.png)

标签按钮被单击访问，其对应组件就显示，为了使得用户体验比较好，要给当前的按钮设置与众不同的css样式，使得可以很清楚地知道是哪个按钮正在被访问，增强用户体验

`语法`：

在App.vue中设置如下css样式：

```html
<style lang="less" scoped>
/*给激活按钮设置高亮样式*/
.router-link-active{color:blue;}
</style>
```

> 通过观察，发现按钮被访问激活后，其对应的html标签就会存在  class="router-link-active" 的属性值



`提示`：

> ​	被激活的按钮的class属性值里边还有一个值为  router-link-exact-active  的信息，不要使用，其表达的意思是，只有路由**严格匹配**的时候才会出现这个信息，**模糊匹配**时候该信息就没有了，造成高亮失效，在后边学习子路由时会有体现



### 小结：

1. 安装路由npm i  vue-router
2. main.js  配置路由5个步骤(导入、引入业务组件、注册路由、创建路由对象、挂载)
3. 路由相关组件：
   - router-link  设置#锚点超链接按钮
   - router-view  设置组件显示占位符

4. 重定向，  通过**redirect**设置一个路由执行时可以跳转到另一个路由去执行

5. 切换按钮高亮显示， class="router-link-active" 被设置css样式即可



## 子路由

### 介绍

`目标`：

​	知道什么是子路由



一般项目开发中，App.vue是根基组件(第1级别的)，内部可以有具体业务组件Home.vue  Movie.vue  Music.vue，它们是第2级别，根据业务需要，业务组件内部还要做内容**分级**显示，例如音乐Music.vue下边要求显示 香港音乐/台湾音乐/大陆音乐等，它们是第3级别组件

第3级别组件做应用的时候需要设置路由，并且与第2级别组件路由有形成父子关系，故称为  子路由

![1576931183209](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/1576931183209.png)

### 应用

`案例`：

​	给Music组件创建子级组件，同时创建并应用子路由

​	创建 组件切换 显示项目，要求有3个级别组件

App.vue

​	Home.vue

​	Movie.vue

​	Music.vue

​		Hongkong.vue

​		Taiwan.vue

​		Dalu.vue

![1576931360446](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/1576931360446.png)



`步骤`：

1. 创建第3级别的业务组件，在components/yinyue/  目录下创建Dalu.vue、Hongkong.vue、Taiwan.vue3个组件，内容分别如下：

   ```html
   <template>
       <div>大陆系列音乐</div>
   </template>
   ```

   ```html
   <template>
       <div>香港系列音乐</div>
   </template>
   ```

   ```html
   <template>
       <div>台湾系列音乐</div>
   </template>
   ```

   

2. 在main.js文件中引入 第3级别业务组件，并给Music配置为子路由

   ```js
   // 导入第三层级业务组件
   import Hongkong from './components/yinyue/Hongkong.vue'
   import Dalu from './components/yinyue/Dalu.vue'
   import Taiwan from './components/yinyue/Taiwan.vue'
   
   // 3. 注册路由中的相关组件
   //    路由中有两个组件( <router-link>  <router-view> )被进场使用
   // A. 单独注册
   // Vue.component('router-link',XXX)
   // Vue.component('router-view',XXX)
   // B. 一并注册，一次性把所有的组件都注册好，更高效
   Vue.use(VueRouter) // 插件
   
   // 4. 创建一个路由对象
   const router = new VueRouter({
     // #锚点信息就是路由地址(路由地址不用设置#号，内部已经封装好了)
     // routes:固定名称，用于设置 路由地址(#/xxx) 与 组件 的对应关系
     routes:[
       // path和component都是固定名称
       // {path:路由地址, component:被显示的组件模块}
       // {path:'路由地址A', redirect:'路由地址B'}  // A 重定向执行 B
       {path:'/', redirect:'/home'},
       {path:'/home', component:Home},
       {path:'/movie', component:Movie},
       {
         // 给当前路由 配置子路由，关键字children
         path:'/music', 
         component:Music,
         children:[
           // 设置 路由地址 与 组件 对应关系
           {path:'/music/hongkong',component:Hongkong},
           {path:'/music/taiwan',component:Taiwan},
           {path:'/music/dalu',component:Dalu}
         ]
       },
   ```

   

3. 在Music.vue中设置 切换按钮  和  组件显示 占位符

   ```html
   <template>
     <div>
       <span>音乐列表展示</span>
       <p>
         <!-- 子路由按钮位置 -->
         <router-link to="/music/hongkong">香港</router-link>
         <router-link to="/music/taiwan">台湾</router-link>
         <router-link to="/music/dalu">大陆</router-link>
       </p>
       <p id="soncont">
         <!-- 子路由组件显示占位符 -->
         <router-view></router-view>
       </p>
     </div>
   </template>
   
   <style lang="less" scoped>
   #soncont{
     width: 280px;
     height: 100px;
     border:1px solid green;
   }
   a{margin-right:5px;}
   </style>
   
   ```



效果：

![1578020980542](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/1578020980542.png)

`注意`：

1. 要通过 children 关键字设置子路由
2. 第三级别组件对应的  切换按钮(router-link) 和 显示占位符(router-view)  需要在Music.vue组件中设置

3. 第二级别组件对应的 切换按钮(router-link)  和 显示占位符(router-view)  需要在App.vue组件中设置



### 重定向和按钮高亮

`目标`：

​	给第三级组件设置重定向 和 按钮激活高亮 效果



`重定向`：

在main.js中给子路由配置重定向：

```js
    {
      // 给当前路由 配置子路由，关键字children
      path:'/music', 
      redirect:'/music/dalu', // A. 子路由重定向配置 推荐
      component:Music,
      children:[
        // 设置 路由地址 与 组件 对应关系
        // {path:'/music', redirect:'/music/dalu'}, // B.子路由重定向配置
        {path:'/music/hongkong',component:Hongkong},
        {path:'/music/taiwan',component:Taiwan},
        {path:'/music/dalu',component:Dalu}
      ]
    },
```

> Music 和 children 内部都可以设置重定向，两种方式



`按钮高亮`：

在src/components/Music.vue中配置

```html
<style lang="less" scoped>
/*给激活按钮设置css高亮效果*/
.router-link-active{
  color:red;
}
……
</style>
```



`注意`：

1. 子路由设置**重定向**有两种方式：父路由中 或 子路由中
2. 父级路由的component:Music   成员不能去除



### 小结

1. main.js中import引入全部组件(包括子路由组件)，  给 Music二级组件通过**children**关键字路由设置子路由

2. 在Music.vue中创建 子路由组件按钮  、  显示占位符，router-link-active设置按钮高亮

    




## 路由传参

### 介绍

`目标`：

​	知道什么是路由传参

项目中有应用场景是这样的：

商品列表页面中，每个商品项目都有 详情页面，为了知道哪个商品被展示查看需要在路由地址中额外设置商品的数字id信息(如下图)，以便据此查询商品，这个商品id就是“**路由传参**”

![1572656638931](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/1572656638931.png)



### 应用

`目标`：

​	实现案例效果：

​	单击电影列表项目，进入详情页面，并对当前电影项目的id信息传递接收

​	

路由：

/movie -------------Movie

/detail --------------Detail



`声明路由参数`：

```js
{ path: '/dt/:xx/:yy/:zz', component: Detail }
```

> 通过   :xx   的方式声明参数，一个或多个都可以，彼此通过  /斜杠  分隔



`接收路由参数`：

```html
<标签>{{$route.params.xxx}}</标签>

<script>
new Vue({
  created(){
    this.$route.params.xxx
  }
	
})
</script>
```

> 模板 和 Vue实例 内部都可以接收



电影列表展示详情应用：

`步骤`：

1. 创建业务组件 src/components/Movie.vue，展示电影列表

   ```vue
   <template>
       <div>
         <h3>电影列表展示</h3>
         <ul>
           <!-- 给router-link设置属性，使得生成其他标签(默认生成a标签) 
             tag="标签名称" ，变为其他标签，不影响单击效果
             <li data-v-358f1330="" class=""> 第一滴血 </li>
           -->
           <router-link 
             tag="li"
             v-for="item in movieList" 
             :key="item.id"
             :to=" '/detail/'+item.id  "   >
             {{item.name}}
             </router-link>
         </ul>
       </div>
   </template>
   
   <script>
   export default {
     name: '',
     data(){
       return {
         movieList:[
           {id:101, name:'第一滴血'},
           {id:102, name:'我不是药神'},
           {id:103, name:'我不是码神'},
         ]
       }
     }
   }
   </script>
   ```

   

2. 创建业务组件 src/components/Detail.vue 展示电影详情，同时接收电影参数

   ```vue
   <template>
       <div>电影详情展示-----{{$route.params.mid}}</div>
   </template>
   
   <script>
   export default {
     // 组件实例内部获得"路由参数"，进而可以通过axios获得指定信息的详情内容
     name: '',
     created(){
       console.log(this) // VueComponent 组件对象
       console.log(this.$route.params.mid) // 获得名称为mid的路由参数
     }
   }
   </script>
   ```

   

3. 在main.js中 给 Movie.vue 和 Detail.vue 两个组件创建路由，Detail的路由要设置**参数**

   ```js
   // 2. 导入各个业务组件
   import Movie from './components/Movie.vue'
   import Detail from './components/Detail.vue'
   
   // 3. 注册路由中的相关组件
   //    路由中有两个组件( <router-link>  <router-view> )被进场使用
   // A. 单独注册
   // Vue.component('router-link',XXX)
   // Vue.component('router-view',XXX)
   // B. 一并注册，一次性把所有的组件都注册好，更高效
   Vue.use(VueRouter) // 插件
   
   // 4. 创建一个路由对象
   const router = new VueRouter({
     // #锚点信息就是路由地址(路由地址不用设置#号，内部已经封装好了)
     // routes:固定名称，用于设置 路由地址(#/xxx) 与 组件 的对应关系
     routes:[
       {path:'/', redirect:'/movie'},
       {path:'/movie', component:Movie}, // 电影列表路由
       // 给当前路由设置参数
       // path:'/xxx/:a/:b:c'
       // 通过:冒号方式 表达有参数，a/b/c 是参数的名称，可以自定义为其他
       {path:'/detail/:mid', component:Detail}, // 电影详情路由
     ]
   })
   ```

   

4. App.vue中只保留router-view 和 css 样式即可

   ```vue
   <template>
       <div>
         <h2>App根组件-路由传参</h2>
         <p id="cont">
           <!-- 设置业务组件显示-路由组件实现代表占位符 -->
           <router-view></router-view>
         </p>
       </div>
   </template>
   
   <script>
   export default {
     name: ''
   }
   </script>
   
   <style lang="less" scoped>
   #cont{
     width: 400px;
     height: 200px;
     border:2px solid orange;
   }
   a{margin-right:10px;}
   </style>
   
   ```



`注意`：

1. 路由有参数，那么应用中要**传递**该参数
2. router-link标签默认被编译为超链接a标签，可以设置**tag属性**，使其变为其他标签



### 小结

1. **设置**路由有参数     {path:  '/dt/:mid', component:XXX}
2. 应用路由需要**传递**具体参数   <router-link to="/detail/1003">
3. 组件**获取**到路由参数  
   1. this.$route.params.mid            组件实例内部
   2. $router.params.mid            template模板中

上午总结：

- 安装、配置**路由**

  封装

  安装  npm i vue-router

  spa升级：

  ​	main.js 路由5个步骤

  ​		导入路由模块

  ​		导入业务组件模块

  ​		注册路由组件( Vue.use(VueRouter)  一次性注册全部组件  插件 )

  ​		创建路由对象  new VueRouter({routes:[ {path:xx,component:组件模块},{} ]})

  ​	App.vue 应用两个组件 router-link to='路由地址'        router-view

- 知道什么是**重定向**

  redirect

- 能够理解、使用**子路由**

  关键字：children

  重定向：父路由、子路由

  按钮高亮：.router-link-active{}

- 能够理解、使用**路由传参**

  路由有参数： path="/detail/:mid"

  应用传递：to=" '/detail' + item.id "

  接收：this.$route.params.mid



## 编程式导航

### 介绍

`目标`：

​	知道什么是编程式导航



`导航`：

​	一个路由被执行一次，就是一次导航



`导航种类`：

1. **声明式导航**：router-link可以编译生成超链接按钮，单击按钮就切换路由并显示对应的组件，这个过程称为“声明式导航(静态)”
2. **编程式导航**：有时由于业务需要，一个路由被切换执行并不方便通过**声明式导航**实现，相反是要通过**程序代码**的方式给实现出来，就是“编程式导航(动态)”



`编程式导航的实现`：

```js
路由对象.push(路由地址)   // 根据路由地址执行到昂  最经常使用********
路由对象.back()  				// 后退************
路由对象.forward()  		// 前进
路由对象.go(数字整数)    // 根据“整型数字”参数做路由的 前进(大于0)、刷新(等于0)、后退(小于0) 操作
```

> 灵感来自BOM浏览器对象模式
>
> window.history.go(负数|0|正数)
>
> window.history.back()
>
> window.history.forward()



`路由对象`：

1. main.js中，就是**router**
2. 在组件实例中 就是 **this.$router**



### 简单使用

`目标`：

​	在电影详情页面通过“编程式导航”实现单击“**返回**”按钮回到列表页面效果

src/components/Detail.vue具体实现：

```html
<template>
    <div>
      <p><button @click="$router.back()">返回back</button></p>
      <p><button @click="$router.go(-1)">返回go</button></p>
      <p><button @click="$router.push('/movie')">返回push</button></p>
      <p>电影详情展示-----{{$route.params.mid}}</p>
    </div>
</template>
```



## 守卫

### 介绍

`目标`：

​	知道什么是守卫



`什么是`：

​	每个路由在执行的时候都会经历一些"**关口**"，关口可以做决定是否 继续前进 或 执行其他路由 或 停止当前路由执行 ，关口就是守卫，有着一夫当关万夫莫开的效果

![1555401458158](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/5-1555401458158.png)



`为什么使用`：

​	每个项目都要使用守卫，例如后台管理系统，很多组件页面要求只有处于登录状态的用户才可以访问，判断是否登录就是通过守卫做的。



### 简单使用

`目标`：

​	知道如何部署守卫



`语法`：

```js
router.beforeEach((to, from, next) => { /* 导航守卫 处理逻辑 */ })
```

> router是 new VueRouter() 得到的 路由对象

​	参数1

- to:是一个对象，保存着将要访问的路由相关参数

- from:是一个对象，保存着离开的那个路由的相关参数

- next:是一个回调函数，对后续的执行起着 拦截 或 放行 的作用

  如果没有问题请<font color=red>一定执行next()</font>方法，以进行后续操作，

  next()方法使用示例：

  ​	next('/login')   执行其他路由，例如登录

  ​	next(false)  停止当前路由执行

  ​	next()  执行后续动作



在main.js的**new VueRouter()代码之后**设置如下代码，查看执行效果

```js
// 6. 查看"全局前置守卫"执行效果
router.beforeEach((to,from,next)=>{
  // to:即将访问路由的对象,  to.path:被访问的路由地址信息
  // from:从哪来的路由对象 ， from.path: 从哪来的路由地址
  // next: 下一个，判官，可以决定是否  放行、停止执行、执行其他路由
  //       next()  放行
  //       next(false)  停止
  //       next(路由地址)  执行其他路由
  // 注意：如果没有特殊情况，请一定执行next()放行，否则页面没有效果了
  console.log(from)
  console.log(to)
  next()
})
```



`注意`：

​	守卫种类有[**很多**](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html)，我们要学习使用的是 “**全局前置守卫**” ，特点是所有的路由在执行之前都会经过该守卫



# WebStorage

## 介绍

`目标`：

​	知道什么是WebStorage 和 种类



`什么是`：

​	WebStorage是浏览器中存储信息的技术，是HTML5为克服cookie的缺陷而发布的



`cookie缺陷`：

1. 大小受限，单个项目有**4k**限制 abcd=4个字节   1024个字节=1k  1024k=1MB
2. 用户可以操作（禁用）cookie，使功能受限
3. 每次请求时，cookie都会存放在请求头中，请求被拦截，cookie数据会存在**安全**隐患



`WebStorage特点`：

1. 存储空间更大，单个项目 Chrome，Firefox和Opera 数据大小可以达到 **5MB**， IE是10M 
2. 只能存储**字符串**类型
3. 不能被爬虫抓取到，更安全 



WebStorage的使用具体分为两种：localStorage 和 sessionStorage

- localStorage（持久存储），关闭浏览器之后localStorage中的数据也不会消失。除非主动删除数据，否则数据永远存在
- sessionStorage（会话存储），临时存储，数据只在当前会话下是有效的，只要这个 浏览器/标签窗口 没有关闭，即使刷新页面或者进入同源另一个页面，数据依然存在，关闭了 浏览器/标签窗口 后数据就被被销毁



`应用场景`：

localStorage：用于长期登录（+判断用户是否已登录），适合长期保存在本地的数据。

sessionStorage：敏感账号一次性登录



## 相关方法

`目标`：

​	掌握WebStorage操作的相关方法



`相关方法`：

```javascript
setItem (key, value) 	 // 保存数据，以键值对的方式储存信息。
getItem (key) 			//  获取数据，将键值传入，即可获取到对应的value值。
removeItem (key) 		//  删除单个数据，根据键值移除对应的信息。
clear () 			    //  删除所有的数据
```

> 使用的时候需要设置前缀，例如
> window.localStorage.setItem()
>
> window.sessionStorage.setItem()



获取方式：

- localStorage：window.localStorage;
- sessionStorage：window.sessionStorage;



## 简单使用

```js
// 通过localStorage和sessionStorage实现数据存储给浏览器
window.localStorage.setItem('city','北京')
window.localStorage.setItem('weather','cloud')
window.sessionStorage.setItem('name','老虎')
window.sessionStorage.setItem('weight','200')
```





# 路由-案例-强制登录

`目标`：

​		利用 " **守卫** + **sessionStorage** +**路由**+**编程式导航**" 实现**非登录用户**访问后台首页就   **强制**去登录的效果



`如何保持用户登录状态`：

​	实际项目中：用户输入用户名、密码后，该账号信息需要给到服务器端做校验，校验成功  后台系统会给客户端浏览器传递一个**秘钥**信息  <font color=red>token</font>  [学员胸牌]，表明当前账号已经登录系统，请客户端浏览器把token保存起来，后期每次向服务器端发起请求的时候，都要把这个token带着，以便服务器端识别当前账号是通过验证的，因此可以通过浏览器中是否有token表明用户是否处于**登录**状态

![1572678736831](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/1572678736831.png)

`步骤`：

1. 在 src/components 目录下创建业务组件： Login.vue(登录)   Home.vue(后台首页)

   Login.vue   组件中实现管理员登录系统

   1. 创建表单域，双向数据绑定username/userpass
   2. 制作"登录"按钮,methods方法实现登录页面跳转逻辑
   3. 账号校验成功通过sessionStorage存储token(暂时杜撰一个即可)信息

   ```vue
   <template>
       <div>
         <h3>管理员登录系统</h3>
         <p>用户名：<input type="text" v-model="username" /></p>
         <p>密码：<input type="password" v-model="userpass" /></p>
         <p><button @click="denglu()">登录</button></p>
       </div>
   </template>
   
   <script>
   export default {
     name: '',
     data(){
       return {
         username:'',
         userpass:''
       }
     },
     methods:{
       // 登录系统
       denglu(){
         // 账号有效性校验(非空)
         if(!this.username.trim() || !this.userpass.trim()){return false}
         
         // 模拟ajax带着账号信息 访问服务器端校验，服务器端校验成功，返回token
         // 浏览器保存token(token一般是一个32位加密串)
         window.sessionStorage.setItem('token','LKUIY&*G*T^&TT&R%R^F^R&^G')
   
         // 跳转到后台首页(编程式导航)
         this.$router.push('/home')
       }
     }
   }
   </script>
   
   <style lang="less" scoped>
   </style>
   
   ```

   Home.vue  设置简单后台显示内容(实现退出系统功能)

   ```html
   <template>
       <div>
         <h3>系统后台首页展示</h3>
         <p><button @click="logout()">退出系统</button></p>
       </div>
   </template>
   
   <script>
   export default {
     name: '',
     methods:{
       // 退出系统
       logout(){
         // 确认动作
         if(!window.confirm('确认要退出系统么？')){
           // 单击取消按钮会执行此逻辑
           return false
         }
   
         // 销毁token秘钥数据
         window.sessionStorage.removeItem('token')
   
         // 跳转到登录页面去
         this.$router.push('/login')
       }
     }
   }
   </script>
   
   <style lang="less" scoped>
   </style>
   
   ```

   

2. 在main.js中 引入 相关组件 并  配置路由 ，同时设置路由守卫，使得非登录用户要访问后台页面，就强制登录去

   main.js

   ```js
   // 2. 导入各个业务组件
   import Home from './components/Home.vue'
   import Login from './components/Login.vue'
   
   // 3. 注册路由中的相关组件
   //    路由中有两个组件( <router-link>  <router-view> )被进场使用
   // A. 单独注册
   // Vue.component('router-link',XXX)
   // Vue.component('router-view',XXX)
   // B. 一并注册，一次性把所有的组件都注册好，更高效
   Vue.use(VueRouter) // 插件
   
   // 4. 创建一个路由对象
   const router = new VueRouter({
     // #锚点信息就是路由地址(路由地址不用设置#号，内部已经封装好了)
     // routes:固定名称，用于设置 路由地址(#/xxx) 与 组件 的对应关系
     routes:[
       {path:'/', redirect:'/home'},
       {path:'/home', component:Home}, 
       {path:'/login', component:Login},
     ]
   })
   
   // 6. 路由守卫简单使用【全局前置守卫，所有路由都生效】
   router.beforeEach(function(to,from,next){
     // "非登录"用户禁止访问后台页面，相反可以访问login登录页面
     let tk = window.sessionStorage.getItem('token')
     // "没有token" 并且 还要访问 "home页面[非login登录页面]" 就强制登录
     if(!tk && to.path!=='/login'){
       // return会停止后续代码执行
       return next('/login')
     }
   
     next() // 放行
   })
   ```

   

3. App.vue中只设置router-view占位符 和 css样式即可

   App.vue

   ```vue
   <template>
       <div>
         <h2>App根组件-管理员访问系统</h2>
         <p id="cont">
           <!-- 设置业务组件显示-路由组件实现代表占位符 -->
           <router-view></router-view>
         </p>
       </div>
   </template>
   
   <script>
   export default {
     name: ''
   }
   </script>
   
   <style lang="less" scoped>
   #cont{
     width: 400px;
     height: 200px;
     border:2px solid orange;
   }
   a{margin-right:10px;}
   /*给激活按钮设置高亮样式*/
   .router-link-active{color:blue;}
   </style>
   
   ```



![1578039677756](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/1578039677756.png)





下午总结：

- - 能够理解、使用**编程式导航**

    路由对象.**push(路由地址)**/go()/**back()**

    路由对象：main.js->router       普通组件->this.$router

  - 能够理解、使用**守卫**

    全局前置守卫

    路由对象.beforeEach(function(to, from ,next){})

- 知道如何应用**WebStorage(localStorage/sessionStorage)**

- 实现管理员强制登录系统案例

  - 路由+sessionStorage+守卫+编程式导航
  - 秘钥token

  






作业：

1. 利用路由  实现按钮切换显示组件(商城、购物车、登录)效果       默认访问商城

2. 子路由(商城：手机、电脑、服饰)      默认访问手机

3. 利用  表单域+sessionStorage 实现用户登录功能

     

![1576937016112](D:/就业班学习资料/Vue基础/Vue day5/笔记/img(online)/1576937016112.png)

4. 选做

   编程式导航 + 守卫 + sessionStorage 实现强制登录，

   即访问  购物车   路由时要求用户处于登录状态，否则强制登录

