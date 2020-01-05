# Vue简单使用

```php+HTML
//1.创建div容器
<div id="app"> {{ city }}----{{weather}} </div>
//2.引入vue.js文件
<script src="./vue.js"></script>
//3.实例化Vue对象
<script>
var vm = new Vue({
  el: '#app',
  data:{
  	city:'北京',
  	weather:'snow'
	}
})
</script>
```

**注意 :**

1. Vue需要有目标操作容器,可以是div,p,span等标签,所有被处理Vue处理的内容都放到该容器中
2. {{}} 是Vue内容(Vue语法)，浏览器上看不到，最终要被解析掉
3. data内部可以声明一个或多个数据供使用
4. el:'#app' 是通过id="app"联系容器，也可以通过其他“选择器”联系

# Vue-MVVM设计模式

`目标`：

​	了解MVVM各部分含义 和 对应代码

mvvm设计模式可以解读为如下：

m: model  数据部分  data

v：view  视图部分  div容器

vm： view & model 视图和数据 的 结合体  

![1565680427600](D:/就业班学习资料/Vue基础/Vue day1/笔记/img(online)/1565680427600.png)

# vue指令

## 插值表达式

`语法`：

```html
<标签> {{ 表达式 }} </标签>
```

> 表达式：变量、常量、算术运算符、比较运算符、逻辑运算符、三元运算符等等

使用示例：

```html
<div id="app">
  <p>{{ msg }}</p><!--变量-->
  <p>{{ score }}</p> <!--变量-->
  <p>{{ 500 }}</p> <!--常量-->
  <p>{{ score+10 }}</p> <!--算术运算-->
  <p>{{ score>10 }}</p> <!--比较运算-->
  <p>{{ score>80 && age>18 }}</p> <!--逻辑运算-->
  <p>{{ age>18 ? '成年' :'少年' }}</p>  <!--三元运算-->
</div>
```

 JS表达式 

```js
//=右边的是表达式,左边不是
var a = 10;
var b = 20>10;
var c = 12+12;
//在javascript中,有实在信息返回的语句就是表达式
console.log(10);
console.log(20/10)
//没有返回实在信息,就是语句(undefined)
console.log(alert(123));
console.log(var b = a/c)
```

如果{{}}使用中有冲突，想更换为其他标记，可以这样：

```js
delimiters:['${'，'}$']  // 标记符号变为${  }$了 
```

`使用要点`：

1. 在插值表达式中 只能设置**简单**的javascript表达式，不能设置**复杂**表达式(例如for循环)
2. 在data值大小不改变的前提下，可以进行一般的 **算术**运算、**比较**运算、**逻辑**运算、**三元**操作符 等运算使用，也可以通过**常量**进行数据体现
3. 插值表达式只能用在html标签的**内容区域**；不能用在其他地方
4. {{}}花括号与变量之间为了美观可以有适当的**空格**，数量不限制，例如{{      msg}}、{{msg    }}、{{   msg   }}}等都可以，为了美观，表达式左右**各一个**空格即可

## v-text  

作用 : v-text与{{}}的作用是一样的，都是操控 标签的**内容区域**信息

语法 : ` <标签 v-text="表达式"> </标签>`

注意 : 

- 使用v-text如果在标签内添加内容会覆盖,使用双花括号不会覆盖;
- v-text 是通过标签的**属性**形式呈现
- v-text 可以进行 变量、常量、算术符号、比较符号、逻辑符号、三元运算符号等运算

**闪烁** : 网速非常慢时，{{ }}花括号  等原生内容 在 Vue编译期间 在浏览器**短时**显示的现象就是闪烁

而 v-text没有(属性 本身就是不会显示出来的)

解决闪烁：

1. 使用v-text
2. vue.js在div容器上边引入

## v-html

使用方式和v-text一样,v-html可以解析html标签

1. v-html尽量避免使用(除非完全掌控)，否则会带来危险(XSS攻击 跨站脚本攻击)
2. 标签的默认内容会被v-html覆盖

如果 服务器返回的数据中，包含有<font color=red>HTML标签</font>(例如富文本编辑器内容)，就可以使用 v-html 来渲染(v-text和 {{}}都不行)

`语法`：`<标签 v-html="表达式"> </标签>`

`v-html、v-text、{{ }}的异同`：

1. v-html对 **html标签** 和 **普通文本** 内容都可以设置显示

2. v-text、{{ }}  只针对  **字符串** 起作用，如果数据中有html标签，那么标签的箭头符号要做<font color=red>字符实体</font>转换，进而使得浏览器上直接"显示箭头"等标签内容，等同于不解析html标签内容

   > <  变为  \&lt;    // less than
   >
   > \> 变为  \&gt;     // great than

   字符实体详细说明： https://www.w3school.com.cn/html/html_entities.asp 

3. v-

4. 它们都可以进行 **变量**、**常量**、**算术运算**、**比较运算**、**逻辑运算**、**三元运算**等技术应用



## v-bind 

{{ }}、v-text、v-html可以对标签的**内容区域**进行操作，操作标签的**属性**需要通过 v-bind: 指令

属性绑定 

1.可以设置标签内的固有属性" v-bind : "可以简写成" : "

```php+HTML
<img v-bind:src="mysrc" alt="" :width="wh" :height="ht" />
<script>
    var vm = new Vue({
      el:'#app',
      data:{
        mysrc:'./laofu.jpg',
        wh:280,
        ht:190
      }
    })
  </script>
```

2.class属性绑定

- 对象方式

```html
<p :class="{apple:true, orange:false}">这是class属性绑定</p>
	<!--true: 设置   false:不设置-->
```

- 数组方式

```html
<p class="['apple','orange']">这是class属性绑定</p>
```

3.style属性绑定

- 对象方式

```php+HTML
<p :style="{'color':'blue','fontSize':'20px','background-color':'hotpink'}"> 这是style属性绑定</p>
//如果属性里有"-",可以使用驼峰,还可以用引号直接引起来
```

- 数组方式

```html
<p class="[{'color':'blue'},{'fontSize':'20px'},{'background-color':'hotpink'}]"></p>
<--根据需要，数组的每个元素都是一个对象，每个对象包含一对或多对css样式-->
```

## v-on(@): 

"@"是"v-on : "的简写

**1.事件绑定**

```php+HTML
<button v-on:click="say"></button>
<button @clicjk="say"></button>  <!-- @符号 简便用法，推荐使用-->
<div @click="事件处理驱动"></tag>

<script>
var vm new Vue({
  el:xx
  data:xx,
  // 给当前vue实例 声明方法，以供事件调用,methods设置事件驱动
  methods:{
    名称：function(){}
    //es6简写方法
    say(){  }
  }
})
</script>
```

methods设置事件驱动

事件绑定传递参数

传统的必须设置上小括号;

**2.传递参数**

vue“单击”事件参数的3种类型：

1. 有声明()，也有传递实参，形参就代表被传递的实参
2. 有声明(),但是没有传递实参，形参就是**undefined**
3. 没有声明()，第一个形参就是**事件对象**

没有()括号情形，由于**事件类型**不一样，参数的意思也会有不同，请灵活使用

**3.访问data数据**

事件执行的时候要调用data数据,通过'this'关键字

**4.this的指向**

构造器, var vm = new Vue ,Vue就是vm的构造器,vm就是Vue的实例化对象

this 和vm的指引完全一致,同一个实例化对象不同的名字this===vm

在Vue实例内部包括不限于methods方法中，**this关键字** 是Vue实例化对象，具体与 **new Vue()** 是一样的

并且其可以对 **data** 和 **methods**成员进行直接访问

## v-model

语法 : `<标签 v-model="data成员"></标签>`

注意：

1. v-model是vue中**唯一**的双向数据绑定指令

2. v-model只针对**value属性**可以绑定，因此经常用在form表单标签中，例如input(输入框、单选按钮、复选框)/select(下拉列表)/textarea(文本域)，相反div、p标签不能用

3. v-model只能绑定**data成员**，不能设置其他表达式，否则错误

   > v-model="score+100"  错误
   >
   > v-model="120"  错误
   >
   > v-model="score" 正确的

4. v-model绑定的成员需提前在data中声明好

5. 页面修改数据，Vue实例会感知到，Vue实例修改数据，页面也会感知到

`响应式`：

vue实例的data数据如果发生变化，那么页面上(或Vue实例内部其他场合)用到的地方会重新编译执行，这样就把更新后的内容显示出来了，这个过程就是“响应式”

> 如果Vue实例内部对变化的数据有使用，也会同步更新编译执行

### 简易原理

```html
  <div id="app">
    <h2>v-model简易原理</h2>
    <p>{{city}}</p>
    <p><input type="text" :value="city"></p>
    <hr />
    <!-- oninput:是一个键盘事件，可以感知输入框输入信息状态 -->
    <!-- 事件@xxx="方法名称/语句" -->
    <!-- $event:在vue的事件被绑定元素的行内部，代表事件对象 -->
    <p><input type="text" :value="city" @input="city = $event.target.value"></p>
    <p><input type="text" :value="city" @input="feel"></p>
  </div>

  <script src="./vue.js"></script>
  <script>
    var vm = new Vue({
      el:'#app',
      data:{
        city:'北京'
      },
      // 给Vue实例 声明方法，该方法可以给事件使用
      methods:{
        feel(evt){
          // console.log(evt)  // InputEvent输入键盘事件对象
          // evt.target: 触发当前事件的元素节点dom对象(类似document.getElementById()的返回结果)
          // console.dir(evt.target)
          // evt.target.value // 获得输入框当前输入的信息
          // console.log(evt.target.value)
          // 把输入框已经输入的信息赋予给city
          this.city = evt.target.value
        }
      }
    })
  </script>
```

##v-for

### 遍历数组

`语法`：

```html
<标签 v-for="成员值 in 数组"></标签>
<标签 v-for="(成员值,下标) in 数组"></标签>
```

`示例`：

```html
<div id="app">
<ul><li v-for="item in color">{{item}}</li></ul>
<ul>
 <li v-for="(item,k) in color">{{k}}-----{{item}</li>
</ul></div>

<script src="./vue.js"></script>
<script>
  var vm = new Vue({
    el:'#app',
    data:{color:['red','yellow','pink']}
  })
</script>
```

使用v-for指令的html标签,由于遍历机制,本身标签会被**创建多份**出来

###v-for :key

在vue中v-for做遍历应用时，都需要与:key一并进行使用

在2.2.0+版本里边，v-for使用的同时必须使用:key，以便vue能<font color=red>准确跟踪</font>每个节点，从而重用和重新排序现有元素，你需要为每个数据项提供一个唯一的、代表当前项目的属性值赋予给key



`语法`：

```html
<标签 v-for="" :key="可以代表每个项目的唯一的值"></标签>
```

`注意`：

1. :key不设置，也是存在的，默认绑定每个项目下标序号信息
2. key是一个普通属性，前边设置**:冒号**，代表属性绑定