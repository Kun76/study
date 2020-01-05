

# 生命周期

## 介绍

`目标`：

​	知道什么是生命周期



辅助参考：

<https://segmentfault.com/a/1190000011381906>

![](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/2-1111-1576572624500.png)

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

created: 一般用于页面"首屏"数据的获取操作(获取好的数据可以直接赋予给data使用，此方法已经把data做好了，其可以做到**第1 时间**就把数据赋予给data，供后续使用)



`注意`：

​	创建阶段各个方法不设置则以，设置后就会**自动**执行，并且会**顺序**只执行**一次**



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
        // 该阶段是一个【重要】阶段，此时data 和 methods已经准备好了，但是还没有去找div容器
        // 此阶段可以用于页面首屏数据获取操作，获取回来的数据存储给data的某个成员即可
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

生命周期参考： [https://vue.docschina.org/v2/guide/instance.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E7%A4%BA%E6%84%8F%E5%9B%BE](https://vue.docschina.org/v2/guide/instance.html#生命周期示意图) 



## VirtualDOM

`目标`：

​	了解VirtualDOM是什么



`什么是VirtualDOM`：

div容器 在 Vue实例中存在的状态，就是  VirtualDOM(虚拟dom内容)，具体是**内存信息**的体现

Vue实例运行期间，该VirtualDOM始终存在



`VirtualDOM作用`：

1. 编译解析div容器，并渲染给浏览器
2. 响应式体现



# Promise

## 介绍

`目标`：

​	知道什么是Promise



Promise浅谈：https://www.jb51.net/article/134310.htm



`什么是`：

 Promise，它是一个对象，是用来处理**异步**操作的，可以让我们写异步调用的代码更加优雅、美观、有利于阅读。



`相关异步操作`：

1. axios(ajax)

2. setTimeout

3. fs.readFile()

   等等



`Promise的三种状态`：

1. pending（进行中）

2. resolved（完成）

3. rejected（失败） 



异步：在同一个时间中，可以发送**多个**进程进行执行

同步：在同一个时间点，只有**一个**进程在执行



`作用`：

1. 解决了异步调用彼此嵌套的**回调地狱**问题
2. 解决了多个异步过程**顺序执行**问题



## 演示使用

`目标`：

​	熟悉Promise的使用方法



`语法`：

```js
// 1. 创建Promise对象
var p = new Promise(function(resole,reject){
  if(异步操作成功){
  	resole(res)
  }else{
    reject(cuo)
  }
})

// 2. 对Promise对象结果进行处理
p
  .then(
  	function(data){
    	// data 与 res一致，代表成功输出结果
  	}
	)
  .catch(
  	function(err){
      // err 与 cuo一致，代表失败输出结果
    }
	)
```

> resolve: 异步操作成功的回调函数
>
> reject：异步操作失败的回调函数



```js
// 对3个文件进行读取操作

const fs = require('fs')

// 1. 3个进程没有顺序
// "异步"方式读取文件
// 由于电脑资源分配不均匀，以下3个进程执行前后顺序没有保障
// fs.readFile('./files/01.txt','utf8',function(err,data){
//   if(err){return console.log('文件错误了：'+err)}
//   console.log(data)
// })
// fs.readFile('./files/02.txt','utf8',function(err,data){
//   if(err){return console.log('文件错误了：'+err)}
//   console.log(data)
// })
// fs.readFile('./files/03.txt','utf8',function(err,data){
//   if(err){return console.log('文件错误了：'+err)}
//   console.log(data)
// })

// 2. 保障各个异步过程顺序执行(嵌套调用)
// 以下代码不好，嵌套调用层次深，形成回调地狱效果了，代码不好维护
// fs.readFile('./files/01.txt','utf8',function(err,data){
//   if(err){return console.log('文件错误了：'+err)}
//   console.log(data)

//   fs.readFile('./files/02.txt','utf8',function(err,data){
//     if(err){return console.log('文件错误了：'+err)}
//     console.log(data)

//     fs.readFile('./files/03.txt','utf8',function(err,data){
//       if(err){return console.log('文件错误了：'+err)}
//       console.log(data)
//     })
//   })
// })

// 3. Promise介入(还没有顺序保障)
// function getContent(filename){
//   var p = new Promise(function(){
//     // 异步操作
//     fs.readFile(filename,'utf8',function(err,data){
//       if(err){return console.log('文件错误了：'+err)}

//       console.log(data)
//     })
//   })
// }
// getContent('./files/01.txt')
// getContent('./files/02.txt')
// getContent('./files/03.txt')

// 4. 继续丰富Promise
//    在Promise内部不要设置"业务逻辑"代码
//    同时内部需要把promise对象进行return返回
function getContent(filename){
  // resolve:代表异步成功执行的回调函数
  // reject:代表异步失败执行的回调函数
  return new Promise(function(resolve,reject){
    // 异步操作
    fs.readFile(filename,'utf8',function(err,data){
      if(err){return reject('文件错误了：'+err)}

      resolve(data)
    })
  })
}

// B. 多个异步操作顺序执行、没有回调地狱
//    promise对象调用then完毕还会返回一个promise对象
//    因此可以继续调用then方法，默认情况promise对象是空的
//    如果在then内部return返回一个实体的Promise对象，则该then就把其返回了(就不是空的Promise对象了)
getContent('./files/01.txt')
  .then(function(rst){
    console.log(rst) // 111
    // 返回一个实体Promise对象
    return getContent('./files/02.txt')
  })
  .then(function(rst){
    console.log(rst) // 222
    return getContent('./files/03.txt')
  })
  .then(function(rst){
    console.log(rst) // 333
  })
  .catch(function(cuo){
    // 统一接收以上各个异步进程的错误信息
    console.log(cuo)
  })


// A. then和catch
// var p1 = getContent('./files/012323.txt')
// // console.log(p1) // Promise { <pending> }

// // 【Promise对象可以调用then和catch方法】接收 resolve 和 reject，进而获得相关的信息
// p1
//   .then(function(rst){
//     console.log(rst) /// 111
//   })
//   .catch(function(cuo){
//     console.log(cuo)
//   })


```

`记住`：

​	以后遇到Promise对象，就调用then、catch方法即可




# axios

## 介绍

axios是对**ajax封装**的一个产品



## 获取

访问npm官网：<https://www.npmjs.com/>

搜索axios即可



axios获取3种方式：

1. 直接下载源文件  推荐
2. 通过script标签引入，需要上网环境
3. npm  install axios  



## 使用

axios可以发送各种请求

```js
axios.get('请求地址', {params:{name:value,name:value。。。。}})
axios.post('请求地址', {name:value,name:value。。。。})
axios({
  method:'get/post',
  url:请求地址,
  params:{},  // 传递get参数信息
  data:{} // 传递post参数信息
})
```

`注意`：

get()方法如果传递参数需要额外设置params，post()方法则不用



## 品牌管理-搭建服务器端项目

`目标`：

​	实现本地搭建服务器端项目



![](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/2-bbb.png)

项目运行中：

- 客户端负责数据的**展示**
- 服务器端负责数据的**提供**
- axios是<font color=red>搬运工</font>，负责在 客户端 和 服务器端 传输数据



为了使得学习效果更好，现在统一设置`后端项目`

`步骤`：

1. 把后端的全部代码文件解压到桌面指定目录

   ![1561016785365](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561016785365.png)

2. 启动mysql

   ![1554629441784](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/2-1554629441784.png)

3. 创建数据库相关

   新建数据库：

   ![1561016897179](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561016897179.png)

   给数据库导入数据：

   ![1561016973984](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561016973984.png)

   ![1561016949657](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561016949657.png)

   数据导入成功：

   ![1561017013901](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561017013901.png)

4. 给项目配置数据库信息

   在src/app.js文件中根据实际情况配置 数据库用户名、密码、数据库名称

   ![1561017218181](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561017218181.png)

5. 给项目安装全部依赖包

   在**server**目录下运行 npm install

   ```
   npm install
   ```

   ![1561017337257](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561017337257.png)

   

6. 运行项目

   在**server/src**目录下运行 node app.js

   ```
   node app.js
   
   ```

   ![1561017520017](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561017520017.png)



注意：

1. 数据库文件导入(二选一即可)
2. 没有密码  root:@127




## 品牌管理-获取列表数据

`目标`：

​	通过 axios+Promise 获取服务器端给准备的品牌列表数据



`步骤`：

1. 把brandsList假数据给注释掉

   ```js
   branList: [
     // { id: 10, name: "宝马", ctime: new Date() },
     // { id: 11, name: "奔驰", ctime: new Date() },
     // { id: 12, name: "奥迪", ctime: new Date() }
   ]
   
   ```

   

2. 引入axios.min.js文件(在Vue.js之后)

   ```html
   <script src="./axios.min.js"></script>
   
   ```

3. 创建methods方法  getBrandList,通过axios发请求，获得数据

   ```js
   // 获取品牌列表信息
   getBrandList(){
     // axios
     var p = axios({
       url:'http://127.0.0.1:3006/api/getprodlist',
       method:'get'
     })
     // console.log(p) // Promise {<pending>}
     // then(箭头函数)使得内部this与外部保持一致，都是Vue实例
     p
       .then(rst=>{
       // rst: 【data】、status、statusText、headers、request、config
       // console.log(rst) 
       // 把获得数据给与data使用
       this.brandList = rst.data.message
     })
       .catch(cuo=>{
       console.log('获得数据错误了'+cuo)
     })
   },
   
   ```

   > 注意：then和catch的函数参数需要为箭头函数，使得内部this与外部保持一致，都是Vue实例

4. 在created中调用  getBrandList()方法

   ```js
   // 生命周期方法
   created(){
     this.getBrandList()
   },
   
   ```




`效果`：

![1576727163425](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1576727163425.png)



`注意`：

1. 获取数据的getBrandList方法需要在created生命周期方法中被调用，好处是

   1. 页面加载完成就**自动**触发执行获取数据
   2. 获取回来的数据可以在 “**第一时间**” 给相关data赋值使用

2. axios调用任何方法返回结果都是Promise对象

3. axios调用各种方法返回结果一般有如下信息特点：

   config  【data】  headers request status  statusText

   除了data是与业务有关系的，其他的都是axios的辅助信息

4. 由于**响应式**的原因，只要brandList成员的数据有更新，页面就立即更新显示



## 品牌管理-删除

`目标`：

​	Promise+axios结合实现品牌删除功能



`步骤`：

1. 给删除事件传递被删除**品牌的id** 和 **品牌下标** 数据
2. 升级改造del方法，通过axios实现持久删除



`核心代码`：

```html
<td><button @click="del(item.id,k)">删除</button></td>

```

```js
// 删除品牌
// id:被删除品牌的id
// index:被删除品牌的下标信息
del(id,index){
  if(!window.confirm('确定要删除该元素么？')){return false}

  // axios实现持久删除
  var p = axios({
    url:'http://127.0.0.1:3006/api/delproduct/'+id,
    method:'get'
  })
  p
    .then(rst=>{
    // 删除成功
    alert(rst.data.message)
    // 实现页面删除刷新
    this.brandList.splice(index,1)
  })
    .catch(cuo=>{
    console.log(cuo)
  })
},

```



## 品牌管理-添加

`目标`：

​	Promise+axios实现品牌数据添加功能



axios请求语法：

```js
axios({
  url:地址,
  method:post,
  data:{post传递的数据}
})

```



`核心代码`：

```js
// 添加品牌
add(evt){
  // 判断如果没有输入品牌这停止添加
  if(this.newbrand.trim().length===0){return false}

  // axios实现持久(永久)添加
  var p = axios({
    url:'http://127.0.0.1:3006/api/addproduct',
    method:'post',
    data:{
      name:this.newbrand
    }
  })
  p
    .then(rst=>{
    // console.log(rst)
    alert(rst.data.message)
    // 刷新显示新品牌
    this.getBrandList()
  })
    .catch(cuo=>{
    console.log(cuo)
  })

  // 清除添加好的品牌
  this.newbrand = ''
}
}

```



`注意`：

1. 相关操作完成后，如果要表达信息，可以与后端协商，接收后端给返回的信息，提高前端开发速度

   例如：alert(result.data.message)

2. 添加品牌完成，可以调用getBrandList()把最新的数据重新获得出来以更新显示



## 配置

### 公共根地址

`目标`：

​	能够给axios配置公共请求地址



给axios把各个请求都需要使用的相同的根地址做统一配置，使得相同代码只维护一份，提升开发速度



`语法`：

```js
axios.defaults.baseURL = 公共根地址

```



上午总结：

- 理解生命周期运行机制
  - 运行阶段：beforeUpdate  updated
  - 销毁销毁  vm.$destroy()  beforeDestroy  destroyed
  - VirtualDOM，虚拟dom，div容器 Vue实例
- 能够应用**Promise**
  - new Promise(function(resole,reject){    resolve(成功数据)   reject(失败数据)  })
  - promise对象.then(rst=>{}).catch(cuo=>{})
  - 解决问题：1. 顺序   2.回调地狱
- 熟练应用**axios** 
  - 获得
  - 引入   axios
  - axios({url:,     method:get/post,    data:     params:})
  - 公共根地址  axios.defaults.baseURL = xxx



### vue成员

在Vue的大环境中不要直接使用axios，相反要把其配置成为Vue的成员，通过Vue对象调用



`目标`：

​	把axios配置称为Vue实例的一个成员



把axios配置为 Vue构造器**原型继承**的成员，这样 vm实例对象就可以调用了

语法:

```js
// 原型继承成员名称为 $http ，也可以配置为其他名称
Vue.prototype.$http = axios


// vue实例中使用axios
this.$http({url:xx, method:xx})


```

Vue中所有的内容都通过自身调用，开发、维护比较方便



## 拦截器-interceptors

### 介绍

`目标`：

​	知道什么是拦截器



![](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/2-4-444.png)

`什么是`：

​	拦截器是axios向服务器端发送**请求**和**响应**回来所经历的两道关口



axios本身有两种拦截器：[请求拦截]()、[响应拦截]()

- 请求拦截器：

​	axios每次<font color=red>开始</font>请求的时候先执行此处逻辑，在这个地方可以给axios做出发前的配置，也可以做出发前的检查工作，检查ok的情况下就开始向服务器端发请求

- 响应拦截器：

​	axios<font color=red>完成</font>与服务器端交互回到客户端后就执行此处逻辑，在这个地方可以做一些后续收尾事宜，例如判断axios请求是否成功，或相关数据过滤操作



拦截器关键字：interceptors



`请求拦截器代码`：

```js
// 请求拦截器
axios.interceptors.request.use(function (config) {
  // 放置业务逻辑代码
  return config;
}, function (error) {
  // axios发生错误的处理
  return Promise.reject(error);
});
 


```



`响应拦截器代码`：

```js
// 响应拦截器
axios.interceptors.response.use(function (response) {
  // 放置业务逻辑代码
  // response是服务器端返回来的数据信息，与Promise获得数据一致
  return response;
}, function (error) {
  // axios请求服务器端发生错误的处理
  return Promise.reject(error);
});

```

> 把以上拦截器代码设置好，那么axios"所有"的请求就都会执行了



### 加载等待案例效果

`目标`：

​	在品牌管理案例中，添加品牌、删除品牌的时候请给与一个等待图片显示效果

`步骤`：

1. 通过img标签显示加载图片，同时设置v-show="flag" 控制是否显示
2. 在data中声明flag:false 信息，控制图片显示
3. 在created生命周期函数中配置  请求、响应 拦截器



`代码`：

```html
<div class="showhead">
    <h2>品牌案例管理</h2>
    <img src="./loading.gif" alt="" v-show="flag" >  
</div>

```

Vue实例相关代码：

```js
      // 生命周期方法
      // created：自动发生调用
      created(){
        // 拦截器放到此处
        // 好处：自动执行、随时调用data
        // 请求拦截器
        // use(箭头函数)使得内部this是Vue实例
        axios.interceptors.request.use(config=> {
          // 请求的业务逻辑
          this.flag = true // 显示加载图片
          return config;
        }, error=> {
          // 一开始请求失败的业务逻辑
          return Promise.reject(error);
        });
        
        // 响应拦截器
        axios.interceptors.response.use(response=> {
          // 请求完毕的后续回调处理，成功情况
          this.flag = false // 隐藏加载图片
          return response;
        }, error=> {
          // 请求完毕的后续回调处理，失败情况
          return Promise.reject(error);
        });


        // 第1时间获得数据并给与data使用
        this.getBrandList()
      },

```

`效果`：

![1576740833606](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1576740833606.png)

`注意`：

1. 各个拦截器的第一个函数参数需要设置为 “**箭头函数**” ，使得内部this与外部保持一致，都是Vue实例
2. 各个拦截器需要设置在created中，这样会**自动**发生执行和配置、**时机**最早、可以**随时**操控data



### 拦截器细节说明

`目标`：

​	知道config参数 和 Promise.reject(error) 分别代表的意思

```js
axios.interceptors.request.use(function (config) {
  return config;
}, function (error) {
  return Promise.reject(error);
});

axios.interceptors.response.use(function (response) {
  return response;
}, function (error) {
  return Promise.reject(error);
});

```

1. config参数

   config是一个对象 与  axios.defaults 相当(不等于)

   config可以给axios配置例如baseURL的信息的

2. response参数

   服务器端给返回的具体数据信息，与业务axios接收的数据一致

3. Promise.reject()

   Promise.reject(data)  是 **语法糖**的用法，本质与下述一致，即返回一个Promise对象

   ```js
   Promise.reject(data)
   上下效果一致
   new Promise(function(resolve,reject){
     reject(data)
   })
   
   Promise.resolve(data)
   上下效果一致
   new Promise(function(resolve){
     resolve(data)
   })
   
   
   ```

   

# vue-组件component-重要

## 介绍

`目标`：

​	知道什么是组件



`什么是`：

​	组件是拥有一定功能许多html标签的集合体，是对html标签的封装

![](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/4-6-1.png)

`好处`：

答：模板中为了实现一个(例如)分页效果，需要绘制**20**个html标签，如果使用组件，就设置**一个标签**就可以了，明显提升开发效率



框架：

Vue：电脑端、移动端

react：电脑端、移动端

angular：电脑端、移动端

组件库针对框架、平台都有针对开发



很有名的第3方组件库(饿了么)：Vue电脑端组件库

<https://element.eleme.cn/#/zh-CN>



组件关键字：components、component



## 私有组件语法

`目标`：

​	知道私有组件的语法规则



`声明私有组件语法`：

```js
new Vue({
  components:{
    '组件的名称': { 配置对象成员 }, 
    '组件的名称': { 配置对象成员 }...
  },
})

```

例如：

```js
components:{
  'my-page':{
    template:`
      <ul>
        <li>上一页</li>
        <li>[1]</li>
        <li>2</li>
        <li>[3]</li>
        <li>下一页</li>
      </ul>
      `,
  }
}

```

`注意`：

1. template设置的各个html标签需要有<font color=red>唯一的根元素</font>节点，上例为ul

2. 组件名称建议是 xx-yy 的格式



`使用组件语法`：

```html
<组件名称></组件名称>

```

> 组件形式上 就是html标签



`案例应用代码`：

制作一个分页组件并使用

```html
  <div id="app">
    <h2>组件应用</h2>
    <!-- 组件的名字被当做html标签使用 -->
    <com-page></com-page>
  </div>

  <script src="./vue.js"></script>

  <script>
    var vm = new Vue({
      // 注册私有组件
      components:{
        // 组件名称:{配置对象成员}
        // 组件名称格式推荐为 xx-yy 样子的
        'com-page':{
          // template: 设定当前组件拥有的html标签
          template:`
            <ul>
              <li>1</li>
              <li>2</li>
              <li>3</li>
            </ul>
          `
        }
      },
      el:'#app',
      data:{
      },
      methods:{
      }
     });
  </script>

```

效果：

![1577693820960](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1577693820960.png)

## 相关成员

`目标`：

​	知道组件内部都有哪些成员



可以认为**组件**是特殊的**Vue实例**，拥有着与Vue实例大致相同的**成员**

例如  **data**、**methods**、**filters**、**directives**、**created**等等成员在组件内部都可以设置



`注意`：

​	组件data成员 与 Vue实例的 不一样，需要通过 function/return 设置，具体要返回一个{}对象



`案例应用`：

给分页组件设置 单击事件、data成员、created生命周期  方法并执行

```vue
  <div id="app">
    <h2>组件应用-其他成员</h2>
    <!-- 组件的名字被当做html标签使用 -->
    <com-page></com-page>
  </div>

  <script src="./vue.js"></script>

  <script>
    var vm = new Vue({
      // 注册私有组件
      components:{
        // 组件名称:{配置对象成员}
        // 组件可以理解为是特殊的Vue实例，因此像 data、methods、filters、directives、computed等
        // 成员在组件内部都可以使用

        // 组件名称格式推荐为 xx-yy 样子的
        'com-page':{
          // template: 设定当前组件拥有的html标签
          template:`
            <ul>
              <li>{{ prev }}</li>
              <li>1</li>
              <li>2</li>
              <li>3</li>
              <li @click="xia()">{{ next }}</li>
            </ul>
          `,
          created(){
            console.log('created已经执行了')
          },
          methods:{
            xia(){
              console.log('进入下一页')
            }
          },
          // 组件中data的样子需要是function/return形式(与Vue实例不同)
          data:function(){
            return {
              prev:'上一页',
              next:'下一页',
            }
          }
        }
      },
      el:'#app',
      data:{
      },
      methods:{
      }
     });
  </script>

```

`注意`：

​	组件的data需要是function/return形式返回，与Vue实例不同



## function声明data

`目标`：

​	明白组件中data为什么必须通过function/return声明



组件中声明的data成员，值需要是一个function，内部通过return返回{}对象供使用，这个特性必须遵守



`为什么组件的data必须是一个function`：

答：组件根据业务需要，可以被使用多次，function会使得每次组件使用的时候都**亲自执行**并给当前应用分配一个**独立的数据对象**，这样多个组件的data数据是独立的，互相没有关联、牵扯，互相不会覆盖

​      相反，如果直接通过{}对象 给data赋值，多次使用组件会造成大家的data都是**共享的**，就是一份数据，一个组件修改data后，其他组件都受到影响，这与业务逻辑是相违背的

![1576500300121](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1576500300121.png)

 

`组件 与 Vue实例 异同`：

1. 组件中的 data 必须是一个 **function** 并 return 一个 字面量对象
   (Vue 实例的 data 可以是 字面量对象，也可以是 function/return形式，前者推荐使用)
2. 组件中直接通过 template 属性来指定组件的html结构
   Vue 实例中，一般通过 el 属性来指定渲染的容器，当然也可以使用template
3. 组件和Vue实例拥有类似的成员，都有自己的生命周期函数，过滤器，methods、data等成员



## 全局组件语法

`目标`：

​	了解全局组件创建的语法



`全局组件语法`：

```
Vue.component(名称,{配置对象成员})

new Vue()

```



`注意`：

​	全局组件需要在new Vue之前设置



## 小结

1. 组件是拥有一定功能多个html标签的**集合体**
2. 组件根据定义类型分为 **全局**/**私有** 组件
3. 组件是特殊的Vue实例，与Vue实例拥有着基本相同的成员(methods/created/data/template等)
4. template都必须有一个**根元素节点**
5. data的值需要是function/return形式



# VueCLI

## 介绍

`目标`：

​	知道什么是VueCLI

`什么是`

答：是脚手架，其可以把许多项目**通用的依赖包**(vue、webpack、路由、vuex、less编译器等等) 和 **通用的配置**都给做好安装好，使得开发者全部的注意力都集中在业务层面，明显提升开发效率的，真实项目都要使用脚手架开发。

![timg.jpg](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/timg.jpg)

依赖包：axios、vue等等都是依赖包，一个依赖包中有许多 模块



## 安装

`目标`：

​	能够安装和使用vuecli



`安装vuecli`：

```bash
npm i -g @vue/cli   // 使用该方式安装

```

> 上述依赖包通过全局方式安装，完毕后在系统任何目录都会执行名称为"vue"的一个指令

> 依赖包安装完毕，会形成 在 C:\Users\ssh\AppData\Roaming\npm\node_modules\@vue\cli 目录

> vue --version  查看脚手架版本，现在默认是4.0.5版本(如果是2.9.6就是错误的，需要卸载重新安装)

> npm un -g @vue/cli  卸载





`vuecli创建项目`：

```bash
vue  create  项目名称(01-pro)

```



`注意`：

1. vuecli创建新项目时，项目名称必须是一个**新目录**，完毕后会自动生产之，并在其中生成项目需要的文件

2. 项目(01-pro)上级各个目录名字最好为**英文** 或 **数字** 或 **中横线** 不要使用 中文的或其他特殊符号定义上级目录名字

 



## 创建项目结构文件

`目标`：

​	利用vuecli创建自己的第一个项目



`步骤`：

​	运行以下命令来创建一个新项目，位置随意，但是各个上级目录最好都是英文的，my-project是项目名称，也是项目目录，不要提前创建

```
vue create my-project

```

![1561622952268](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561622952268.png)



![1565964186423](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1565964186423.png)





![1561623170607](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561623170607.png)



![1561623274446](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561623274446.png)



![1563505874818](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1563505874818.png)

> 上图的两个项目都不要选取



![1561623412365](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561623412365.png)



![1561623482895](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561623482895.png)



![1561623586580](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561623586580.png)



![1577266191026](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1577266191026.png)



(题外话：当再次创建项目时，就可以选取之前配置好的项目，**一次性**完成项目的创建，不用再详细选取配置)

![1561623755361](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561623755361.png)



上边的20190628具体配置在如下文件，可以直接删除，以便恢复出厂设置：

![1561692040822](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1561692040822.png)



创建好的项目效果

![1577620228220](D:/就业班学习资料/Vue基础/Vue day3/笔记/img(online)/1577620228220.png)



安装vuecli(  npm i -g @vue/cli  )有可能失败:

​	解决：复制粘贴老师安装好的文件到c盘指定目录(C:\Users\ssh\AppData\Roaming\npm\node_modules)

**vue的3个相关文件**放到C:\Users\ssh\AppData\Roaming\npm目录下

**@vue**目录放到 C:\Users\ssh\AppData\Roaming\npm\node_modules

安装项目(  vue create 01pro )有可能失败

​	解决：直接使用老师安装好的项目即可



下午总结：

- axios配置
  - Vue.prototype.$http = axios     this.\$http
- 掌握**axios拦截器**部署应用
  - 请求
  - 响应
  - 配置位置：created
  - 细节：config、response、Promise.reject(xxx)
- 知道什么是**组件**，知道如何 创建 和 使用“重要”
  - 普通组件
  - 全局component、私有components
  - 组件成员：参考Vue实例设置
  - data：function/return
  - template:唯一根元素
- 能够通过**VueCli**创建项目
  - 创建：npm i -g  @vue/cli
  - 安装项目： vue  create  xxx



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
|   |-- views									  // 业务组件目录
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
   // import Vue from 'vue' // Vue实例有render可以使用的
   import Vue from 'vue/dist/vue.common.js' // Vue实例 没有 render时候可以使用的
   // import App from './App.vue'
   
   Vue.config.productionTip = false
   
   new Vue({
     data:{
       msg:'项目已经运行了'
     }
     // render: h => h(App)
   }).$mount('#app')
   
   
   ```

   

2. 修改public/index.html文件，使用Vue实例的data数据

   ```html
       <div id="app">
         {{ msg }}
       </div>
   
   ```

   

3. 修改vue.config.js文件内容如下

   ```js
   module.exports = {
     lintOnSave: false,
     devServer:{
       open:true, // 项目运行自动开启浏览器
       port:16699 // 给项目运行创建web服务的端口号码(1~65535之间)
     }
   }
   
   ```

   > 当前项目运行的时候会创建一个http服务，上述16699就是服务器的端口号码，
   >
   > open:true  项目运行的时候，会**自动**打开浏览器并呈现效果

4. 在终端中执行命令

   ```
    npm  run  serve
   
   ```

   > 上述命令需要在项目根目录下运行(01-pro目录下)
   >
   > serve:指令标志是package.json文件配置好了

> 现在项目可以运行了，并且有实时加载的效果，业务文件随时改变，浏览器随时查看到对应效果



`注意`：

​	npm run serve  指令需要在项目**根目录**下运行



作业：

1. axios+Promise结合，完成品牌案例  **列表展示**、**添加**、**删除**功能

2. 实现 品牌案例的axios请求 **加载等待图片**效果，应用**拦截器**技术

3. 制作一个table表格**组件**，要求可以实现如下表格(4行3列)，其中数据部分来自data定义

   | 序号 | 名称     | 价格   |
   | ---- | -------- | ------ |
   | 1    | 华为手机 | 3449元 |
   | 2    | oppo手机 | 2999元 |
   | 3    | 苹果手机 | 4229元 |


4. 通过VueCli创建一个项目



5. 预习 单文件组件

