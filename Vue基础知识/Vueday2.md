debugger 打一个断点

eval( 字符串 ) 可以使得字符串当做'js表达式'运行起来

# vue指令

## -v-if&v-show

v-if 和v-show会根据接收true/false来显示或隐藏 

语法 :  

```html
<标签 v-if="true/false"></标签>
<标签 v-show="true/false"></标签>
<!--true:显示   false:隐藏-->
```

区别 :

- v-if：通过 **创建**、**销毁** 方式达到显示、隐藏效果的(销毁后有一个占位符)
  - <!---->"占位符，与html的注释信息**没有关系**
- v-show：其是通过css控制达成显示、隐藏效果的 

v-if 有更高的**切换消耗** 、v-show有更高的**渲染消耗**

如果需要**频繁**切换 则v-show 较合适，如果运行条件不大可能改变 则v-if 较合适

## -v-if&v-else

在Vue中，v-if 、v-else-if 和 v-else 三个指令结合可以实现多路<font color=red>分支</font>结构

```html
<标签 v-if="true/false"></标签>
<标签 v-else-if="true/false"></标签>
<标签 v-else-if="true/false"></标签>
<标签 v-else></标签>
以上4个标签只分支结构，最终只会走一个，第一个为true的那个标签会执行  或 执行v-else
```

- v-if可以**单独**使用，形成单路分支结构
- v-if  和 v-else 也可以合作使用，实现**双路**分支结构
- v-if  、v-else-if 和 v-else 也可以合作使用，实现**多路**分支结构
- v-if和v-else一并使用，页面只没有<!---->占位符了

# computed 计算属性

`computed计算属性`：

Vue本身支持模板中使用**复杂表达式**表现业务数据，但是这会使得模板内容过于杂乱，如果确有需求，可以通过computed计算属性实现，该computed可以对其他data做复杂合成处理的

`语法`：

```js
new Vue({
  el:xx,
  data:xx,
  computed:{
    // 属性名称:function(){
    属性名称(){
      // 业务表达式实现，可以通过this操作data成员
      return  返回结果
    }
  }
})
```

> 计算属性普通函数赋值或简易成员函数 赋值 都可以，不要使用箭头函数

`使用`：

形式上，如何应用data成员，就如何应用计算属性

```vue
{{ computed计算属性名称 }}     <!--模板中-->
或
this.XXX				// Vue实例内部
```

`特点`：

1. 计算属性关联的data如果发生变化，会重新编译执行 获得 并 使用 对应新结果，即**响应式**
2. 计算属性内部可以使用this关键字，与Vue对象等效
4. 每个计算属性都需要通过**return**关键字返回处理结果

`与methods方法的区别`：

computed计算属性本身有“**缓存**”，在关联的data没有变化的情况下，后续会使用缓存结果，节省资源

methods方法没有缓存，每次访问 方法体 都需要加载执行，耗费资源

# 过滤器

什么是过滤器 : 过滤器是Vue中实现数据格式**转换**的一种机制。本质就是函数

过滤器关键字：filter、filters

## 私有过滤器

语法 : 

```js
new Vue({
  filters:{
    // 如下方法格式是es6简易设置方式，完整写法： 过滤器名称:function(被处理数据){]}
    过滤器名称(被处理的数据){
      // 对数据进行加工处理
      return 结果
    },
    ...
  }
})
```

使用 :

```
{{ 时间信息成员 | 过滤器名称 }}
```

- 过滤器被设置到应用数据的尾部，通过 “|竖线” 连接

设置字符串以指定的位置输出,不够就在字符串前边补位,可以用于设置时间

```js
// 字符串.padStart(位数,补位信息)
'hello'.padStart(8,0)      // 000hello
```

`注意`：

过滤器只可以用在**两个**地方： <font color=red>插值表达式</font>和 <font color=red>:冒号 属性绑定表达式</font>。

> 1) 插值表达式：  {{  数据 |  过滤器 }}
>
> 2) v-bind属性绑定中使用： <标签  :属性=“数据 | 过滤器">
>
> v-if="city|xx"  错误

## 全局过滤器

`注意`：

​	全局的过滤器需要在new Vue()**之前**声明

`全局过滤器`：

​	在new Vue()前边，直接给Vue调用filter声明的过滤器称为“全局过滤器”

​	全局的意思是过滤器可以供**所有Vue实例**使用

```js
Vue.filter(名称, function(被处理的数据){})
var vm = new Vue()
var vm2 = new Vue()
```

`注意`：

1. 创建多个div容器、多个Vue实例的情形只是技术实现而已，真实项目很少有这样用的
2. 多个过滤器可以被同时使用， {{ 信息 | 过滤器 | 过滤器 | 过滤器 …… }}，形成一个信息被多次处理效果

# 按键修饰符

按键修饰符：使得键盘事件只针对某个(或某几个)按键生效

应用中有许多 键盘事件 (onkeyup、onkeydown、onkeypress、oninput等等)，每个事件在执行的时候可以通过**许多键子**达成，有时我们要求只有<span style="background-color:yellow;">按到某个键子</span>时，才激活该事件，例如只有触碰 **回车键**或**ESC键** 才有效果，那么可以通过 **按键修饰符** 实现

> oninput：触碰键盘给输入框做输入动作时会触发执行
>
> onkeyup：键盘抬起触发执行
>
> onkeypress：按下任何字母数字键时触发执行，系统按钮（例如，箭头键和功能键）无法得到识别
>
> onkeydown：按下任何键盘键（包括系统按钮，如箭头键和功能键）时触发执行

键码值：键盘每个按键都对应一个**数字码**，称为 键码值

全部按键键码值： https://blog.csdn.net/qq_39207948/article/details/79882229 

`语法`：

```html
<input  @keyup.键码值/别名="处理">
<!--要求只有触碰回车键 才执行该事件-->
<input  @keyup.13="处理">  <!--键码值-->
<input  @keyup.enter="处理">  <!--别名-->
<!--键码值：键盘的每个键子都有一个数字码，就是键码值，event.keyCode 就获取到了-->
```

vue考虑到**键码值**使用多有不便，已经给常用**键码值**(event.keyCode)设置**别名**了

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”按键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

也可以自定义其他的**按键别名**

```js
// 要求使用 `@keyup.f6`
Vue.config.keyCodes.f6 = 118
<input @keyup.f6="xxx" />  // 只有单击f6键才会触发xxx的回调
```

`注意`：

​	如果有的需求比较特殊，需要多个按键一并触发该事件，也可以

​	`@keyup.ctrl.enter="XXX"`  表示 ctrl和enter一并触碰，才触发事件执行

# 自定义指令

关键字：directive     directives

`声明语法`：

```js
// 1. 声明全局指令
Vue.directive(指令名称,{ 配置对象成员 })

// 2. 声明私有指令
new Vue({
  directives:{
    指令名称:{ 配置对象成员 }
  }
})
// 配置对象：
//inserted是固定用法
// inserted：时机的事情，代表是div容器被Vue实例编译完毕并且也渲染好了
inserted(m){
  m：代表使用该指令的html标签dom对象，可以通过m进行原始dom操作实现业务需求
}
```

`注意`：

私有指令directives关键字 与el、data等都是并列的

# 扩展

## template

`目标`：

​	了解template用法

在Vue实例内部可以声明template，其内容可以**覆盖掉**原生的div容器的

```html
<div id="app">{{ city }}</div>
<script src="./vue.js"></script>
<script>
  var vm = new Vue({
    template: "<span>上海</span>",
    el: "#app",
    data: {
      city: "北京"
    }
  })
</script>
```

> 上述代码执行，页面上会看到“span上海”内容，相反“div北京”已经被覆盖了

## $mount

`目标`：

​	了解$mount用法



如果Vue没有提供`el`成员帮助找到div容器，那么可以调用$mount()方法

因此Vue实例与容器联系有两种方式：

1) el:'#app'

2) $mount方式

```js
var vm = new Vue()
vm.$mount('#app')

或 

var vm = new Vue().$mount('#app')  // 连贯调用
```

## render成员

目标：

​	了解render成员使用



在Vue中如果定义了render成员，那么其提供的内容会渲染到页面中，并且会**覆盖**原容器，包括template

优先级关系：render >>>>> template >>>>>>默认容器

render最高

```html
  <div id="app">
    <p>{{ weather }}</p>
  </div>

  <script src="./vue.js"></script>

  <script>
    var vm = new Vue({
      el:'#app', // Vue实例 与 div容器 联系
      data:{
        weather:'cloud'
      },
      methods:{
      },
      template:'<span>sunshine</span>',
      // render：渲染，去覆盖原生div容器的
      // render:function(create){
      //   // return create(html标签名称, 标签内容区域信息)
      //   return create('h2', 'snow') // 创建 <h2>snow</h2> 元素标签了
      // }
      // render:function(h){
      //   // return create(html标签名称, 标签内容区域信息)
      //   return h('h2', 'snow') // 创建 <h2>snow</h2> 元素标签了
      // }
      // render: h=>{
      //   // return create(html标签名称, 标签内容区域信息)
      //   return h('h2', 'snow') // 创建 <h2>snow</h2> 元素标签了
      // }
      
      // 箭头函数体内部只有一个语句，并且有return返回，那么{} 和 return 都可以省略
      render: h=>h('h2', 'snow') // 创建 <h2>snow</h2> 元素标签了
    })

  </script>
```

> 上述代码会看到 snow 内容，相反 cloud 和 sunshine 都被覆盖了



## console使用

`目标`：

​	了解console的基本用法

```js
console.log()   调试工具普通数据输出
console.dir()   可以把dom对象的各个成员给打印出来

console.group()  对输出的信息做分组处理，更加清楚

console.log('%c%s',css样式设置, 被输出的信息)
c:css样式  与 第2个参数对应
s:string字符串 与  第3个参数对一个
console.log('%c%s','color:red', '你好')
```

`应用`：

```js
console.group('前端91') // 按照分组效果输出调试信息
console.log('王一')
console.log('王二')
console.log('王三')
console.group('Java102') // 按照分组效果输出调试信息
console.log('张一')
console.log('张二')
console.log('张三')
// console.log('%c%s','css样式','输出的信息')
console.log('%c%s','color:red;background-color:lightgreen;font-size:25px;border:2px solid orange;','张四')
```

![1576570057868](D:/就业班学习资料/Vue基础/Vue day2/笔记/img(online)/1576570057868.png)



# 生命周期

## 介绍

`目标`：

​	知道什么是生命周期



辅助参考：

<https://segmentfault.com/a/1190000011381906>

![](D:/就业班学习资料/Vue基础/Vue day2/笔记/img(online)/2-1111.png)

`什么是`：

​	生命周期是指vue实例(或者组件)从诞生到消亡所经历的各个阶段的总和



生命周期分为3个阶段，分别是[创建]()、[运行]()、[销毁]()

- 创建阶段：由空白期、data/methods初始化、模板挂载、模板渲染等组成
- 运行阶段：分为 更新前 和 更新后 两部分
- 销毁阶段：分为 销毁前 和 销毁后



`成员方法`：

各个阶段在Vue实例内部都有对应的成员方法，可以定义、执行、感知

​	创建：beforeCreate    **created**   beforeMount   mounted

​	运行：beforeUpdate  updated

​	销毁:  beforeDestroy    destroyed



`为什么学习`：

不同阶段完成不同的任务，开发者可以利用各个阶段的特点完成业务需要的相关功能



## 创建阶段分析

`目标`：

​	了解创建阶段各个方法特点，重点记住created



创建阶段一共有4个方法，它们与 el、data都是并列关系的

```js
new Vue({
  beforeCreate(){   },
  created(){  },
  
  beforeMount(){   },
	mounted(){  },
})
```

> beforeCreate：此时Vue对象刚创建好，没有任何成员，data、methods等都没有呢，只有this
>
> **created**：此时vue对象已经长大一点，内部已经完成了data、methods等成员的设置，也是data初始化的最好时机
>
> beforeMount：此时vue实例已经把div容器给获得到了，但是内部的vue指令等信息还没有被编译处理
>
> mounted：此时，vue获取到的div容器内部的原生指令已经被编译处理好了，并且也完成了容器的渲染工作，此时模板中已经看不到vue原始指令了



重点关注方法是 created ：

created: 一般用于页面"首屏"数据的获取操作(获取好的数据可以直接赋予给data使用，其可以做到**第1 时间**就把数据赋予给data，进而不影响后续使用)



`注意`：

​	创建阶段各个函数不设置则以，设置后就会**自动**执行，并且会**顺序**只执行**一次**



## 运行和销毁阶段分析

`目标`：

​	了解运行阶段和销毁阶段各个方法特点



`运行阶段`：

```js
new Vue({
	beforeUpdate(){ 可以感知到数据变化之前页面上关于该数据的样子 }
	updated(){ 可以感知到数据变化之后页面上该数据的样子 }
})
```

> 运行阶段方法**不会**自动执行，当data成员数据发生变化，就执行了，并且数据变化多次，方法也会**重复**执行多次



`销毁阶段`：

```js
new Vue({
	beforeDestroy(){ 实例销毁之前 }
	destroyed(){ 实例销毁之后 }
})
```

> 当vue实例被销毁后，就要执行以上两个方法，vm.$destroy()



`注意`：

1. **运行阶段**各个方法与创建阶段不同，本身**不会**自动执行，需要数据变化的条件触发才会执行
2. **销毁阶段**各个方法也不会自动执行，需要Vue实例对象调用$destroy()方法



完整应用示例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <!--创建一个div容器，vue对该容器进行控制，设置要显示的内容-->
  <div id="app">
    <h2>{{ msg }}</h2>
  </div>
  
  <script src="./vue.js"></script>
  <script>
    var vm = new Vue({
      // 1) 生命周期创建阶段(4个函数),会自动执行
      beforeCreate(){
        // Vue实例已经创建完毕，但是相关的成员都没有，el、methods、data等等都没有
        console.group('--------beforeCreate发生调用--------')
        console.log('%c%s','color:red','el现在的样子：'+this.$el)     // undefined
        console.log('%c%s','color:red','data现在的样子：'+this.$data) // undefined
        console.log('%c%s','color:red','getDate现在的样子：'+this.getDate)  // undefined
      },
      created(){
        // 该函数是非常【重要】的，此时data 和 methods已经准备好了，但是还没有去找div容器
        // 此阶段可以用于页面“首屏”数据获取操作，可以第一时间把数据给到data
        console.group('--------created发生调用--------')
        console.log('%c%s','color:red','el现在的样子：'+this.$el)      // undefined
        console.log('%c%s','color:red','data现在的样子：'+this.$data)  // 实体
        console.log('%c%s','color:red','getDate现在的样子：'+this.getDate)  // 实体
      },
      beforeMount(){
        // 此阶段完成了Vue实例对象 与 div容器联系的过程(本质是div容器已经被Vue实例获取到了)
        // 但是div容器的内容还是没有编译前的原生内容
        console.group('--------beforeMount发生调用--------')
        console.log('%c%s','color:red','el现在的样子：'+this.$el)      // 实体
        console.log(document.getElementsByTagName('h2')[0])  // 
      },
      mounted(){
        // 此阶段 Vue实例已经完成了div容器的内容的编译，并且编译好的内容也渲染给div容器了
        console.group('--------mounted发生调用--------')
        console.log('%c%s','color:red','el现在的样子：'+this.$el)      // 实体
        console.log(document.getElementsByTagName('h2')[0])  // 容器编译【后】实体内容
      },

      // 2) 生命周期运行阶段(2个函数),data数据变化后才会执行
      beforeUpdate() {
        console.group('---------beforeUpdate调用--------')
        console.log(
            '%c%s',
            'color:red',
            'h2数据更新【前】的效果：' + document.querySelector('h2').innerHTML
        )
      },
      updated() {
        console.group('---------updated调用--------')
        console.log(
          '%c%s',
          'color:red',
          'h2数据更新【后】的效果：' + document.querySelector('h2').innerHTML
        )
      },
      
      // 3) 生命周期销毁阶段(2个函数),只有vm调用$destroy()方法后才执行
      beforeDestroy() {
        console.group('---------beforeDestroy调用--------')
        console.log('%c%s', 'color:red', 'el现在的样子：' + this.$el)
      },
      destroyed() {
        console.group('---------destroyed调用--------')
        console.log('%c%s', 'color:red', 'el现在的样子：' + this.$el)
      },
      
      el: '#app',
      data: {
        msg: '生命周期学习篇'
      },
      methods: {
        getDate(){
          console.log('Sunday')
        }
      }
    })
  </script>
</body>
</html>
```



`注意`：

生命周期的各个方法与 el、data、methods 等成员都是并列的，它们有固定的执行顺序，与设置顺序没有关系



## 图示

[生命周期参考](https://vue.docschina.org/v2/guide/instance.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E7%A4%BA%E6%84%8F%E5%9B%BE)



## VirtualDOM

`目标`：

​	了解VirtualDOM是什么



`什么是VirtualDOM`：

div容器 在 Vue实例中存在的状态，就是  VirtualDOM(虚拟dom内容)，具体是内存信息的体现

在Vue实例运行期间，该VirtualDOM始终存在



`VirtualDOM作用`：

1. 编译解析div容器，并渲染给浏览器
2. 响应式体现





# 安装devtools工具

`目标`：

​	能够给chrome浏览器安装devtools调试工具



devtools是vue在chrome浏览器中的调试工具，方便Vue项目开发

安装devtools有两种方式：
1.通过翻墙软件在chrome浏览器的扩展程序中直接安装
2.在github上下载该工具并自行编译、安装配置

![1556079740076](D:/就业班学习资料/Vue基础/Vue day2/笔记/img(online)/1556079740076.png)



`注意`：

1. 该调试工具要求使用**开发型vue**，压缩生产型vue不能被识别

2. 只有vue开发的项目有调试效果



下午总结：

- 熟练使用**按键修饰符**
  - @keyup.enter/13="xxx"
  - @keyup.enter.ctrl="xxx"
- 了解**自定义指令**的创建和应用
  - v-dian
  - directives:{指令名称:{inserted:function(m){ m.focus }}}  私有
  - Vue.directive(名称, {inserted....})全局
- 掌握Vue生命周期用法
  - 3阶段：创建、运行、销毁
  - 创建：4个函数
    - beforeCreate
    - created：data和methods准备好了，用于首屏数据操作
    - beforeMount
    - mounted



作业：

1. 利用computed给 计算器 实现计算逻辑并输出结果
2. 完成品牌案例管理各个功能
   1. 完成品牌的  删除、筛选  功能
   2. 利用 **computed** 完善**筛选**品牌功能
   3. 利用 **私有过滤器** 完成时间格式化操作
   4. 利用 **按键修饰符** 完成单击**回车键**实现添加品牌功能、单击**esc键**完成清除品牌功能
   5. 利用 **自定义指令** 完成页面加载完成，使得添加品牌输入框**获得焦点**功能

