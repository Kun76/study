# DAY1

##客户端和服务端(服务器)

* 客户端: 使用服务的计算机,一台计算机希望成为客户端,必须安装浏览器
* 服务端: 提供服务的计算机,如果想成为服务端,必须安装特定的服务端软

**URL的基本概念**

* URL概念：统一资源定位符 （UniformResourceLocator）
  * 俗称网址，用来标识某个资源在网络中的唯一位置。
* 组成部分 : 通信协议,域名,资源的具体位置

## 请求和响应的流程

任意一次的网络访问都是下面的三个步骤

步骤

- 1 **请求：客户端请求服务器**
- 2 处理：服务器的内部处理（找找资源等..）
- **3 响应：服务器响应客户端**

network用来检测浏览器发出的每次**请求**以及**响应**内容

- request 请求
- response 响应

##资源的请求方式get post

* 语义的区别
  * get 用来进行**获取资源**的操作
    - 如获取图片,文件,地址栏输入地址回车,location.href，a标签跳转等
  * post 用来进行**发送资源**的请求操作
    - 表单提交form标签(get,post)

##AJAX简介

> Ajax（**Async**hronous Javascript And XML（异步 JavaScript 和 XML））

- 作用：用来发送请求的一种方式
- 实现方式简介：浏览器提供了一个XMLHttpRequest的构造函数，创建的对象用来进行ajax操作
- 特点：无需刷新页面，也可以进行请求响应处理（局部刷新）

##同步异步的概念

### JS的语言特性 

JS的语言特性 : 是弱类型语言,动态语言,脚本语言,基于对象的语言(面向对象语言),基于原型的语言,事件驱动语言,是**单线程语言**,

* 单线程（只有一个人干活）
  * 因为js中具有DOM操作，例如修改元素颜色，单个线程操作不会与其他线程产生干扰
  * es5后js也出现了多线程的概念，但是有限制，多个线程只能进行辅助操作，主线程进行控制
* 多线程（有好多人干活）

### 同步和异步

- 同步任务：
  - **执行顺序：代码按顺序从上往下一个一个执行。**
  - 同步任务有哪些：除了异步任务都是同步任务。

- 异步任务：
  - 为什么要有异步任务: 因为某些任务较为耗时，或执行时间不确定，为了避免卡住（阻塞）后续代码，设置为异步任务。
  - **常见异步任务有哪些：**
    - 定时器
      - 示例：无论定时器的时间为多少，都会比同步任务执行晚。
    - Ajax
  - **执行顺序：异步任务的执行一定晚于同步任务**

## jQuery使用ajax请求的方法

### $.get()的使用

```js
$.get(地址, function(res) {
  // 响应接收完毕后，执行回调函数
  // res代表响应的数据，如果res为JSON格式，jQuery会自动转换
});
$.get(地址, 请求参数组成的对象, 回调函数);
```

请求参数：请求中发送给服务端的数据

### $.post()的使用

```JS
$.post(地址, function(res) {
  // 响应接收完毕后，执行回调函数
  // res代表响应的数据，如果res为JSON格式，jQuery会自动转换
});
$.post(地址, 请求参数组成的对象, 回调函数);
```

###$.ajax()的使用

* 参数:
  * type表示请求方式.默认不写为'GET',可设置为'GET/POST'
  * url请求地址, 只有url属性是**必须设置**的
  * data表示请求参数,对象结构
  * success表示响应成功后的处理函数 

```txt
$.get() /$.post()与 $.ajax()的关系

$.ajax是jQuery中设置的一个用来进行ajax请求发送的方法$.get() $.post()只是调用了$.ajax()实现的功能
```

juqery中还有一个方法 async默认的设置值为true，这种情况为异步方式,设置为false时为同步

##接口的概念

* 接口 : 接口指的是**能够提供某种能力**的事物
* 应用程序编程接口（API）
  - 概念：提供应用程序编程能力的事物
  - 以前学习过的API
    - WebAPI
      - 浏览器提供的，与web开发相关的一些API，本质上就是一堆属性方法
    - 内置对象API
      - js解析器提供的，用于js基础语法操作的一些API，本质上就是一堆属性方法
    - （回顾）js的组成部分和浏览器的主要构成
      - 浏览器的两个主要构成部分
        - 内核(渲染引擎): html和css渲染，WebAPI
        - js解析器：执行ECMAScript
      - 规范相关内容：
        - w3c规范：html css webAPI
        - ECMA规范：语法(含内置对象)
* 数据接口：
  - 概念：数据接口是**能够提供数据的**一种事物。表现形式就是一个URL
    - **简单来说，能够提供数据的一个URL地址，就称为是数据接口**
  - 小说明：当我们进行前端和后端(服务器)交互时，所说的'**接口**'，指的就是数据接口。
  - 使用方式：
    - **严格遵守接口的文档进行操作即可。**

# DAY2

##表单简介

- 表单的组成部分

```html
- form标签
- 表单域: input标签  textarea标签  select标签
- 提交按钮  <input type="submit">  
		<button type="submit">提交</button>
//如果希望button不进行提交操作，只是普通按钮，可以设置type为button
```

- form的属性
  - action 地址,target是否弹窗,method获取方式(get/post),enctype编码格式

- 表单操作的特点：
  - 提交数据时会出现跳转的情况
- 常用的使用方式：
  - **使用表单进行数据采集(用输入框等元素获取输入内容)，使用ajax进行请求发送即可(提交)**

## 表单和ajax结合使用

### 阻止表单的默认提交行为

submit注册事件,提交事件

在submit事件中，**设置e.preventDefault()方法即可(推荐)**， return false也可以,该方式比较暴力,不仅可以阻止默认行为,还可以阻止冒泡

### jQuery的serialize方法

> 可以快速获取表单中的所有数据(**只能获取纯文本数据**)
>

- 使用方式： $(form的选择器).serialize() 
- 结果形式：
  - 名=值&名=值&名........
- 注意：这种内容格式可以通过`$.get $.post $.ajax`进行直接发送
  - 注意表单元素如果没有name，数据是无法正确提交的

## 模板引擎

* 为什么使用模板引擎 :
  * 传统方式的字符串拼接比较麻烦,html和js书写在一起不便于维护
* 作用 :
  * 可以将生成结构的字符串拼接操作进行简化
* 安装: 官网 [<http://aui.github.io/art-template/zh-cn/docs/installation.html>]()
* 使用步骤:
  - 引入template-web.js
  - 准备数据(通常为请求得到的数据)
  - 准备模板
    - 使用script标签,设置type为text/template.设置id
    - 内部书写需要的结构内容
    - 结合模板语法进行操作
  - 调用template方法处理
    - template("id名",数据)
    - 返回值为将数据和模板结合后得到的字符串

### 语法

{{数据}}基本值的访问方式

- {{}} 是模板的基本标记，标记中会进行模板语法的执行，标记外原样输出
- {{$data}} 代表template()的第二个传入的参数
- {{`$data.属性名`}}用于访问$data的属性
  - **简写形式 {{`$data的属性名`}}  也表示访问$data的属性**
- {{@数据}}   如果数据中含有html标签结构，加@可以生成，如果不加@就不会生成,默认将标签打印在页面中
- 模板中的if使用

```js
{{if age===21}
    <p>此人21岁</p>
    {{else if age===20}}
    <p>此人20岁</p>
    {{else}}
    <p>此人不知道多少岁</p>
    {{/if}}
```

* {{each}} 模板数据遍历
  * 可以统一遍历数组和对象两种形式
  * 在each中`$index和$value`是模板默认提供的名称， `$index代表索引`/属性名  `$value`代表元素值/属性值
  * 可以通过自定义名称替换$index与$value

```php+HTML
	{{each aiHao}}
      <div>这是div</div>  {{$index}}   {{$value}}
    {{/each}}

    {{each aiHao v i}}
      <div>这是div</div> {{v}} {{i}} 
    {{/each}}


    {{each obj}}
    <p>这是p标签</p>  {{$index}}   {{$value}}
    {{/each}}

    {{each obj value key}}
    <p>这是p标签</p>  {{key}}   {{value}}
    {{/each}}
```

###模板的过滤功能

作用：是用来对模板中某些数据进行处理的一个方法

设置方式:

* 在调用模板功能前设置

```js
template.defaults.imports.过滤器名称 = function (模板中书写的数据) {
  // 进行各种的内容处理
  return '处理结果';
};
```

- 过滤器的返回值，会作为模板中最终展示的数据
- 模板中的写法

```
{{time}}  这是普通的输出方式
{{time|过滤器名称}} 这是通过过滤器处理后的输出方式
```

##正则表达式的提取方法

* exec()
  * 作用 : 正则提取方法,可以筛选字符串
  * 参数 : 需要筛选的字符串
  * 返回值 : 需要在字符串中提取的内容
  * 语法 : 正则.exec(需要提取字符串)
  * **每次提取一个新的内容,再次调用会提取之后的新内容,没有可提取的内容时返回nul**l
* replace()
  * 作用 : 替换字符串里的内容
  * 参数 : 两个参数,
    * 参数一可以是需要替换的字符串内容也可以是正则表达式
    * 参数二是需要替换为的新字符串
  * 语法 : 需要替换字符串.replace(参数1,参数2)
  * 返回值 : 新的字符串
* match()
  * 作用 : 可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配
  * 参数 : 可以是正则也可以是字符串
  * 语法 : 字符串.match(参数)
  * 返回值 : 存放匹配结果的数组,该数组的内容依赖于正则是否具有全局标志 g
  * 与exec的区别
    * match可以一次性提取所有的数据,但是无法分组,当正则表达式不加g时,只能提取第一个数据
    * exec每次只能提取一个,可以利用循环分组

正则.test()是检验正则是否匹配或者满足某些条件;

"."在正则中代表特殊字符,表示全部包含,可以用转译字符`\`,  `\.`表示只取一个值是点

# DAY3

##原生AJAX的使用

###get请求的基本使用

**步骤 :**

```JS
//1 创建xhr对象
var xhr = new XMLHttpRequest();
//2 调用open,设置请求方式和地址
xhr.open('GET','http://www.liulongbin.top:3006/api/getbooks');//默认异步,可以通过open来设置同步
//3 调用send,发送请求,这一步是异步操作
xhr.send()
//4 设置事件，监听请求的各种状态
    //   - readyState是xhr的属性，用来表示请求发送的状态
    //     - 取值为4代表下载完毕
    //     - 必须确保readyState为4才能使用响应的数据
    //     - 进一步检测：
    //       - xhr.status 代表请求是否成功
    //          - 200代表请求是成功的
xhr.onreadystatechange = function (){
   //4.1检测xhr.readyState取值和xhr.status取值,
    if(xhr.readyState === 4 && xhr.status === 200) {
        // 4.2 接收响应的数据即可
        //    - 原生接收的响应内容是JSON格式，需要自己进行转换
        //      - jQuery会自动转换
        console.log(xhr.responseText);
    }
}
```

**参数:**

* 位置: 在xhr.open()的参数2,url后面书写参数内容
* 名称: 参数的形式称为查询字符串
* 格式:  ?名=值&名=值...
  * 其中 名=值&名=值...称为url编码格式,英文为urlencoded
* jQuery的get请求和原生get请求的**本质是一样**的





**url编码是url对地址的一种处理方式(将中文部分进行编码转换**)

- 浏览器会进行自动的url处理，无需我们自己操作
- 两个用来处理的函数
  - encodeURI() 编码
  - decodeURI() 解码

###post请求的发送方式

步骤：

1. 创建xhr对象
2. 调用xhr.open()
3. 设置Content-Type（内容格式、内容类型）
   - xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
4. 调用xhr.send(数据)
   - 数据为url编码格式
5. 设置readystatechange事件获取响应内容

## get与post发送方式的区别

- **请求参数的书写位置不同：**
  - get请求方式：在xhr.open()的url后面使用?连接
  - post请求：在xhr.send()中书写
- **post请求需要设置Content-Type**

## ajax level2新功能: 超时时间设置

- 设置方式：
  - 属性：  xhr.timeout = 超时时间; //  毫秒单位
  - 事件：  xhr.ontimeout = function() { 请求超时后，触发的事件 }

## FormData

* 作用 : 
  * 可以快速处理表单的数据
  * 可以进行文件上传
* 注意点 : 
  * **发送FormData需要使用POST请求**
  * **不需要单独设置Content-Type**

使用方式 : 

* 首先创建FormData对象,  var fd = new FormData();
* 单独发送数据时,通过fd.append('name','aa')的方式添加数据,然后将fd放入在send()参数中即可
* 处理表单数据时,将表单以DOM对象的形式传入FormData对象中当做参数,然后将fd放入在send()参数中即可,也可以通过fd.append分方式传参

文件上传 : 可以通过FormData上传文件

- **1 准备结构(文件域)**

```html
<input type="file">
```

- **2 检测用户是否选择了文件**
  - 对DOM对象形式的input[type="file"]的**files**属性进行检测
    - 如果内部存储了任意的一个值，就说明选取了文件，否则就是没选
- 3 使用FormData保存文件信息
  - 使用fd.append()添加即可
- 4 通过ajax发送
- 5 响应处理，展示服务端获取到的图片

### jQuery使用FormData上传文件的注意点

- 必须设置两个属性
  - contentType: false 不指定内容类型
  - processData: false 不进行数据处理

### 上传进度条功能

- xhr.upload.onprogress 上传中实时触发事件
  - 事件对象e的属性
    - e.lengthComputable 文件长度使用可用
    - e.loaded 以上传大小
    - e.total 总大小
- xhr.upload.onload 上传完毕时触发事件



# DAY4

##jQuery的全局ajax处理器

作用 : 用来对页面中的ajax操作进行统一设置,简化操作,常用来加载资源的loading功能

* ajaxStart()任意请求开始时触发内部回调

```js
$(document).ajaxStart(function () {
  // 代码...
});
```

* ajaxStop() 任意请求结束后触发内部回调

```js
$(document).ajaxStop(function () {
  // 代码...
});
```

## axios使用

* axios.get()的使用

```js
axios.get('地址', {params: {数据。。。}}).then(成功时的处理函数)
```

* axios.post()的使用

```js
axios.post('地址', {数据。。。}).then(成功时的处理函数)
```

* axios()的使用
  * 发送给和post的区别:
    * 设置请求参数的属性名不同
      * get的请求参数的属性名为params
      * post的请求参数的属性名为data

```
axios({
	method : 请求方法
	url : 地址
	params/data { 请求参数}
}).then(成功时的处理函数);
```

## 跨域

### 同源(同源地址)

* 同源指的是两个地址的 **协议 域名 端口** 完全一样,那么这两个地址称为同源地址

###浏览器同源策略的机制

* 发送跨域请求时,请求发送是成功的,响应也是成功的,只不过浏览器将响应的信息拦截了.我们无法操作,
* 影响 : 无法对非同源的地址发送ajax请求(不能跨域)

### 跨域方式

* JSONP
  * 兼容性好,仅支持GET
* CORS-nodejs
  * 兼容性不太好，但是支持GET、POST，并且是标准中提供的方式。

##JSONP的实现原理

- 实现原理：
  - 同源策略不限制script标签对非同源地址进行请求
  - 可以借助script标签进行JSONP实现
- 实现方式:

```js
/*使用JSONP进行请求处理
   步骤:
   1 准备一个处理函数
   2 通过script标签的src属性，进行接口的请求
     - 因为同源策略不会限制script标签的功能
   3 请求时传入请求参数
     - callback 用来传递本地处理函数的函数名
     - 其他参数随便写
   4 当响应结束后，步骤1设置的处理函数会自动调用
      - 可以在处理函数中对响应数据进行后续处理*/
  <script>
    function fun(res) {
      console.log(res);
    }
  </script>
  <script src="http://ajax.frontend.itheima.net:3006/api/jsonp?callback=fun&name=jack"></script>
```

小说明：

- JSONP和JSON没有本质关联
- **JSONP与ajax没有任何关联**
  - JSONP是使用script标签进行请求发送
  - ajax是通过浏览器提供的XMLHttpRequest对象发送

## jQuery发送JSONP请求的方式

- jQuery使用的是$.ajax()方法进行JSONP操作
  - **设置dataType属性为'jsonp'**
- 小说明：
  - 可以从浏览器的network看到请求的类型为 script 
  - jQuery会将使用过的用于发送jsonp请求的script标签移除，打开页面也看不到

```js
	$.ajax({
      type: 'GET',
      url: 'http://ajax.frontend.itheima.net:3006/api/jsonp',
      data: {
        name: 'jack',
        age: 18
      },
      /* 
      // 下面的两个属性作为了解即可，通常不会使用
      jsonp: 'xyz', // 将默认的请求参数名callback设置为其他名称
      jsonpCallback: 'abc', // 将处理函数名称设置为指定名称
      */
      dataType: 'jsonp', // 设置这句话后，jQuery就会使用JSONP的方式进行请求发送
      success: function (res) {
        console.log(res);
      }
    });
```

## 节流和防抖

### 节流

**所谓节流，就是指连续触发事件但是在 n 秒中只执行一次函数**

### 防抖

> 进行重复操作时，设置时间间隔，以最后一次满足间隔的操作为准进行执行。
>
> 简单来说：**就是以满足间隔的最后一次为准**
>
> **所谓防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间**

* 节流和防抖的区别 :
  * 防抖是当在指定间隔时间内再次触发,新触发后的操作会重新计时
  * 节流是指在指定时间内再次触发会被忽略

## HTTP协议

基本概念 :

* HTTP协议寄**超文本传输协议** (HyperText Transfer Protocol) ,它规定了**客户端与服务器之间进行网页内容传输时，所必须遵守的传输格式**。

### HTTP的请求报文和响应报文

内容传输格式分为**请求报文**和**响应报文**两种

- 请求报文（客户端给服务端发送请求时发送的那些内容）
  - **请求报文由4部分组成**
    - **请求行**
      - 3部分组成：  请求方式  请求地址  协议和版本
    - **请求头**：**用来保存客户端的相关信息**(大部分是浏览器自动设置的)
      - Host：主机地址
      - User-Agent：用户代理字符串，客户端的一些浏览器和系统信息
      - Content-Type：发送的请求体的内容类型(内容格式)
    - 空行
    - **请求体**：**就是我们发送的请求参数**
      - **get请求的参数在url中发送，请求体为空**
      - **post请求的参数在请求体中**，所以需要设置Content-Type
  - 响应报文由4部分组成
    - **状态行**
      - 由3部分组成： 协议和版本  状态码  状态文本
    - **响应头**：用来**保存服务端的相关信息**(有的是服务端自动设置的，有的是后端自己设置的)
      - Content-Type: 响应体的内容类型
    - 空行
    - **响应体**
      - **服务端发送给客户端的响应内容**（无论什么请求方式，响应的数据都在响应体中保存）
        - 例如我们在jQuery中接收的res就是响应体

### 请求方式

GET获取数据,POST用来发送数据,PUT用来修改数据,DELETE用来删除数据

### 状态码

> 通过一些数字表示本次请求的状态：除了状态码还会配有状态文本

- **2xx 成功**，常见的是200
- **3xx 重定向**(我们请求a.html，服务端认为a.html有问题，没法给你看，将你的请求转给了b.html)
  - 常见：304 读取缓存的内容了
- **4xx 客户端错误**，常见的是404
- **5xx 服务器错误**，通常为服务端有问题时出现

