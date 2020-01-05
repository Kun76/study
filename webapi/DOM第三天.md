# 00-反馈

* 能不能讲一下以下这些词的定义和关系,有点晕... DOM / 节点/ 对象/ 标签/ 事件/ 事件对象/ 属性/ 方法/ 行为/
  * DOM  document object 对象
  * 节点：HTML结构标签，在JS这叫节点，叫DOM节点；本质是个对象；
  * 对象：属性和方法的集合体
  * 方法：函数，
    * DOM节点.onclick 设置方法，本质就是名字，方法名，函数名；在JS这叫事件,(与用户交互)
    * 什么时候调用刚才注册的方法？用户的行为；
    * 事件对象：形参，注册事件后面的函数内部的形参；本质对象；
      * 鼠标的坐标
      * 阻止冒泡、默认；
    * getAttribute("自定义属性的名字")
  * 标准属性：
    * style  value className classList
  * 行为：用户；
* 事件offset和style有什嘛区别
  * 属性：
  * DOM节点.offsetLeft; 值 数字，距离DOM节点.offsetParent 水平距离；
  * DOM节点.offsetParent：找有定位的父亲，上级父级没有定位，继续找，直到body;
  * DOM.style.left 只是获取（设置）定位数据，来自行内，返回是字符串；
* 最近单词量太多,记不住,记起来特别混乱：**Excel表；**JQ混淆；
  * 先步骤；
  * 记忆单词，方法（功能）；
* CSS问题：抽奖的 css样式只能用 div包裹图片 让div 旋转才可以吗？ 直接用 img 为啥不可以 用一定要用定位吗？ 用margin 让它居中为什么不可以 用margin 让它居中然后再旋转 要怎样才能达到和div 一样的旋转效果





# 01-DOM-节点-修改、创建、添加

* 创建：
  * innerHTML属性：可以识别HTML结构；
  * document.write();  可以识别HTML结构；
  * document.createElement('li') ：在JS里面创建出DOM节点，只是创建，不显示在页面；
* 添加：
  * 父节节点.appendChild(新DOM节点)：从后添加新的DOM节点；
  * 父节节点.insertBefore(新DOM节点，插入谁之前的DOM节点)：在某个元素之前，添加新的DOM节点
* 修改DOM内部结构或者文本：
  * innerHTML：结构；
  * innerText：文本内容；



# 02-案例-发布微博v1-新增

* 核心步骤：
  * 获取文本域内容（优化）.value
  * 创建新的DOM节点
  * 给新的DOM节点修改内容为文本域东西
  * 从前插入；

```js
  // 需求：点击发布，把发布的内容，发布在列表最前面；
  // 分析：

  //  1.获取DOM节点？
  var btn = document.querySelector(".weibo-btn");
  var textarea = document.querySelector('.weibo-text');
  var ul = document.querySelector('.weibo-list');

  // 2. 注册（设置）点击事件，不调用；用户才觉得什么时候调用；
  btn.onclick = function() {
    // 3.点击之后：
    // 3.1 新建li 
    var li = document.createElement("li");

    // 3.2 把文本框的内容 设置到li 内部（结构）;
    li.innerHTML = `<p>${textarea.value}</p><span>删除</span>`;

    // 3.3 添加到ul 最前面（DOM节点）;
    var first = document.querySelector("ul li:first-child");
    ul.insertBefore(li, first);

    // 3.4 textarea内容清空
    textarea.value = "";
  }

  // ---------------------------------------------
  // 模拟用户操作
  // 用户输入1：点击发布；btn.onclick();  ul li:first-child  ---->li发布上去；
  // 用户输入2：点击发布；btn.onclick();  ul li:first-child（上一次li发布上去）  ---->li发布上去；
```



# 03-DOM-节点-通过节点获取节点-发布微博v1优化

- 通过属性  获取DOM节点：

```js
// 
  var ul = document.querySelector("ul");

  // 属性：children 伪数组；
  // console.log(ul.children[0]);


  // 属性：获取父级DOM节点 找上级父级
  var li3 = document.querySelector("#li3");
  // console.log(li3.parentNode);
  // // 找定位父亲，没有--->body
  // console.log(li3.offsetParent);


  // 属性：获取兄弟元素,下一个兄弟
  console.log(li3.nextElementSibling);
  console.log(li3.previousElementSibling);
```





# 04-DOM-节点-删除

* 语法：方法；

```js
    // 谁调用：指定父级，删除某个子元素；
    // 参数：被指定的子元素；
    ul.removeChild(ul.children[0]);
```



# 05-案例-发布微博v2-删除1

```js
// 2. 注册（设置）点击事件，不调用；用户才觉得什么时候调用；
  btn.onclick = function() {
    // 3.点击之后：
    // 3.1 新建li 
    var li = document.createElement("li");

    // 3.2 把文本框的内容 设置到li 内部（结构）;
    li.innerHTML = `<p>${textarea.value}</p><span>删除</span>`;

    // 3.3 添加到ul 最前面（DOM节点）;
    // var first = document.querySelector("ul li:first-child");  
    // 属性获取第一个孩子
    var first = ul.children[0];
    ul.insertBefore(li, first);

    // 3.4 textarea内容清空
    textarea.value = "";


    var spans = document.querySelectorAll("span");
    // console.log(spans);

    //  2. 所有的删除 注册（设置）事件；
    for (var i = 0; i < spans.length; i++) {
      spans[i].onclick = function() {
        //  3. 点击之后，删除自己的父级li；
        var li = this.parentNode;
        // 删除
        ul.removeChild(li);
      }

    }
  }


  // 需求：点击 删除，删除本条li
  // 分析：
  //  1. 获取所有的删除按钮
  // 
  var spans = document.querySelectorAll("span");
  // console.log(spans);

  //  2. 所有的删除 注册（设置）事件；
  for (var i = 0; i < spans.length; i++) {
    spans[i].onclick = function() {
      //  3. 点击之后，删除自己的父级li；
      var li = this.parentNode;
      // 删除
      ul.removeChild(li);
    }

  }

  // --------------------------------------------
  // 用户点击发布：btn.onclick()  : 第一步：发布；第二部：把所有的span重新获取，重新注册事件
  // 用户点击发布：btn.onclick()  : 第一步：发布；第二部：把所有的span重新获取，重新注册事件
  // 性能问题：同样的事件，多次注册，浪费性能；
  // 解决：事件委托
  // 1 把事件注册给父级上。点击子元素，冒泡阶段，信号传递到父级的时候，触发父级注册的函数；
  // 2.点击对象 是 span;
```



# 06-案例-发布微博v2-删除2

* 事件委托:
  * 把事件注册给父级上。点击子元素，冒泡阶段，信号传递到父级的时候，触发父级注册的函数；
  * 判断 点击对象 是 span;

```js
  // 需求：点击 删除，删除本条li 事件委托
  // 场景：已经存在的DOM节点，还是新生成的，都注册同一个事件，用事件委托
  ul.onclick = function(e) {
    // 当前点击的是哪个？
    // DOM节点 点谁是谁的DOM节点；

    // nodeName：节点的大写名称；
    if (e.target.nodeName == "SPAN") {
      // 找到span 的父级；
      var li = e.target.parentNode;

      // 删除；
      ul.removeChild(li);
    }
  }
```







# 07-BOM-window及onload

* BOM学习的对象：window上面的属性和方法；
* onload：等待静态资源加载完成后执行你的逻辑；

```js
// BOM 把浏览器看成对象
  // 浏览器 window

  // 1.window绝大部分属性和方法，用的时候都不需要调用window对象；
  // 顶级对象：页面中所有的东西都是依赖于这个对象存在的；
  // console.log(document == window.document);


  // 2.声明的全局的变量和函数都是 window 下的属性和方法；
  // var a = "----------------";
  // console.log(window.a);

  // function fn() {
  //   alert(1)
  // }
  // window.fn();


  // 3.在js代码里面，不使用var声明的变量，都是隐式全局变量，这个方式是不推荐的，因为如果不使用var声明，是不会变量提升的；
  // 提升：在代码的var a 前面可以使用 a;
  // console.log(a);
  // var a = 1;
  // console.log(window.a);

  // 不会提升；
  // console.log(a); // 报错；
  // a = 1; // 给window设置属性a; 不会提升；
  // console.log(window.a);


  // onload:注册，事件，什么时候执行？页面静态资源加载完毕时才会执行；
  // 静态资源：css img 文件；页面中有图片，使用 window.onload 把所有的代码包起来；
  window.onload = function() {
    // 加载之后，JS代码了’
  }


  // 网络请求。。。
  // 网络请求。。。
  // 网络请求。。。
  // 网络请求。。。
  // 网络请求。。。
  // 网络请求。。。
  // 网络请求。。。
  // window.onload();
```



# 08-BOM-定时器-语法

```js
// ----------------------------------------------一次性定时器  
  // setTimeout  是window上的方法。
  // 参数：倒计时执行函数；
  //    第一个：执行函数；
  //    第二个：倒计时 默认单位是ms  1s = 1000ms；

  // 返回值：返回一个number值，用于清除定时器；
  // var timer = setTimeout(function() {
  //   console.log(1);
  // }, 3000);
  // // console.log(timer, '-------------------');

  // // 
  // var btn = document.querySelector("#stop");
  // btn.onclick = function() {

  //   // 参数：上面获取到的number类型的timer;
  //   clearTimeout(timer);
  // }




  // ---------------------------------------------------永久性定时器
  // Interval：间隔,等待间隔后的时间才执行；
  // 参数：
  //    第一个：执行函数；
  //    第二个：事件间隔 默认单位是ms  1s = 1000ms；

  // 返回：返回一个number值，用于清除定时器；
  var timer = setInterval(function() {
    // console.log(1);
    console.log(Math.random());

  }, 1000);

  var btn = document.querySelector("#stop");
  btn.onclick = function() {

    // 参数：上面获取到的number类型的timer;
    clearInterval(timer);
  }
```



# 09-BOM-定时器-案例-验证码倒计时

```js
// 2. 注册事件 点击之后：
  //  2.1 按钮变灰
  //  2.2 按钮文字立马显示 
  //  2.3 开启定时器；文字显示；等待间隔1s，执行函数；--
  //  2.4 判断定时器 是否到达0s；恢复原状；
  btn.onclick = function() {
    // 2.1 按钮变灰
    btn.disabled = true;
      
    // 2.2 按钮文字立马显示 
    var num = 5;
    btn.value = `获取验证码(${num}s)`;

    // 2.3 开启定时器，等待1s后， 
    var timer = setInterval(function() {
      num--;
      btn.value = `获取验证码(${num}s)`;

      // 2.4 判断定时器 是否到达0s；恢复原状；
      if (num == 0) {
        clearInterval(timer);

        btn.disabled = false;
        btn.value = `获取验证码`;
      }

    }, 1000);
  }
```



# 10-BOM-location

* 语法：负责管理浏览器地址栏相关的行为和信息的对象；

```js
// location:是 window上的属性
  // 值：是个对象（属性和方法）
  // console.log(location);

  // 对象（属性和方法） 属性href：获取和设置地址；
  // 获取
  // console.log(location.href);

  // 设置：页面地址，页面展现内容就不一样了
  setTimeout(function() {
    // JS通过属性控制页面转跳
    location.href = "http://www.baidu.com";
  }, 2000);

```



# 11-BOM-localStorage

* 作用：小U盘，浏览器自带的；20M；

```js
// 以前：页面新增完数据后，刷新就没有了；
  // 原因：新增的数据没有被保存下来；‘
  // 解决：把数据保存在 浏览器 里面；
  // 如何使用：
  // 语法：localStorage 属性名；
  // 值：对象（属性和方法的集合体）

  // 方法：存入
  // 参数1：存入本地哪个字段内。什么名字；键；位置；
  // 参数2：存入的数据
  // 真实存入（本地浏览器、小U盘）数据：
  // localStorage.setItem("a", "---------------------");
  // localStorage.setItem("b", "1234");


  // 方法：拿出真实存的数据；
  // 参数：键，位置；
  // 返回：读取的数据；如果读取一个不存在的数据，返回null;
  // var info = localStorage.getItem("c");
  // console.log(info);
  // 数据覆盖：
  // localStorage.setItem("b", "9999");

  // 方法：真实清除某条数据
  // 参数：键，位置；
  // localStorage.removeItem("b");

  // 方法：全部清除；
  // localStorage.clear();

```

* 获取不存在的键的名称，返回的是null；

* **把本地存储想象成一个本地U盘，真实的存入一些数据；**



# 12-BOM-JSON

* **记住：localStorage 存入复杂数据类型，要先转换为 JSON格式的字符串；**

* 描述JSON字符串是个字符串，有一定格式的字符串；
* JS BOM上的方法：

```js
 // 本地储存数据，
  // 特别： 如果存储的是对象之类的复杂类型，需要先把复杂类型转换为的字符串，再存进去；
  // 怎么把对象---->字符串？
  // var obj = {
  //   a: 1,
  //   b: 2
  // };

  // 手动变："{a: 1,b: 2}"
  // 
  var obj_1 = {
    name: 'zs',
    infos: ["1", "2", '3']
  };
  // var str = '{name: \'zs \',infos: ["1", "2", \'3 \']}';

  // 内置BOM JSON
  //
  // 作用:把对象类型转换为字符串； 字符串化
  // 参数：传入对象数据
  // 返回：字符串；JSON格式的字符串
  var str = JSON.stringify(obj_1);
  // console.log(str);

  // JSON格式的字符串：属性名，也带上双引号；
  // localStorage.setItem("obj_1", str);
  var str_1 = localStorage.getItem("obj_1");
  console.log(str_1); // 字符串：看起来里面有的地方是像对象，数组； JSON；

  // 作用：把JSON格式字符串转为对象；
  // 参数：JSON格式字符串
  // 返回：对象
  var obj_2 = JSON.parse(str_1);
  console.log(obj_2);

```



# 13-案例-发布微博v3-抽象数据-01

```
// 真实工作：用户真实输入一个信息，存在服务器（大U盘）；从服务器拿回来的数据，也是下面这样的格式；
  //  后台工程师：接口文档，ajax阶段学习新的方法，把后台给数据拿到；
  // 接口文档：说明，给每一份数据 key val 代表什么意思；


  // 现在：约定格式 ，数据存在本地（小U盘）

  // 约定：.
  // 一条数据：{}
  // var one = {
  //   info: "快来收了这九款用上就停不下来的应用吧！！"
  // };

  // 列表：多条数据  [一条数据，一条数据]
  var list = [{
    info: "1111111111111111111"
  }, {
    info: "22222222222222222222"
  }, {
    info: "33333333333333333333"
  }];


  // 真实拿到数组：
  var ul = document.querySelector("ul");

  for (var i = 0; i < list.length; i++) {
    // list[i] 每条数据 
    // {
    //   info: "1111111111111111111"
    // }

    var li = document.createElement("li");
    li.innerHTML = `<p>${list[i].info}</p><span>删除</span>`;
    ul.appendChild(li);

  }



  // 抽象阿里百秀的的一条新闻：
  // var one = {
  //   title: "生活馆 关于指甲的10个健康知识 你知道几个？",
  //   author: "alibaixiu",
  //   time: "2015-11-23",
  //   info: "指甲是经常容易被人们忽略的身体部位， 事实上从指甲的健康状况可以看出一个人的身体健康状况， 快来看看10个暗藏在指甲里知识吧！",
  //   read: 18,
  //   pinglun: 1,
  //   zan: 18,
  //   tags: ["健康", "感染", "指甲", "疾病", "皮肤", "营养", "趣味生活"]
  // }

```



# 14-案例-发布微博v3-新增-02

```js
  // 需求：点击发布，把发布的内容，发布在列表最前面；


  // 分析：

  //  1.获取DOM节点？
  var btn = document.querySelector(".weibo-btn");
  var textarea = document.querySelector('.weibo-text');
  var ul = document.querySelector('.weibo-list');


  // 2. 注册（设置）点击事件，不调用；用户才觉得什么时候调用；
  var list = [];
  btn.onclick = function() {


    // ----------------------------------------------DOM操作
    // 3.点击之后：
    // 3.1 新建li 
    var li = document.createElement("li");

    // 3.2 把文本框的内容 设置到li 内部（结构）;

    li.innerHTML = `<p>${textarea.value}</p><span>删除</span>`;

    // 3.3 添加到ul 最前面（DOM节点）;
    // var first = document.querySelector("ul li:first-child");  
    // 属性获取第一个孩子
    var first = ul.children[0];
    ul.insertBefore(li, first);


    // -----------------------------------------------数据储存在本地（小U盘）
    // 一条数据的格式：
    var one = {};
    // {
    //   info: "1111111111111111111"
    // }
    one.info = textarea.value;

    // 不是一条单独的数据进行本地的储存，把一条数据放在一个列表 [];
    // var list = []; 会出现问题1；
    // 把刚才数据放入数组中；哪个方法？
    // push: 从后添加   unshift:从前添加,和HTML结构对应；
    // 数组：全局变量
    list.unshift(one);

    // 存入本地；
    // 复杂数据类型先转换为JSON
    // 变量重新被赋值 不是数组；
    var str = JSON.stringify(list);
    localStorage.setItem("list", str);

   
    // 3.4 textarea内容清空
    textarea.value = "";
  }



  // -------------------------------------模拟用户
  // 问题1：两条数据，被覆盖了； 问题演示;
  // 点击发布：btn.onclick()  HTML出现；数据存入本地 ：var list = [];  [{info:"1" }]  --->存入
  // 点击发布：btn.onclick()  HTML出现；数据存入本地 ：var list = [];  [{info:"2" }]  --->存入

  // 正确演示：var list 全局变量 []
  // 点击发布：btn.onclick() HTML出现；数据存入本地 从前添加到全局的数组 [{info:"1" }]--->存入;
  // 点击发布：btn.onclick() HTML出现；数据存入本地 从前添加到全局的数组 [{info:"2" },{info:"1" }]--->存入;

```

* 步骤：
  * 点击之后：
    * DOM操作：HTML页面上显示新增的内容；
      * 1.获取文本域的内容’
      * 2.新建li DOM节点；
      * 3.组装li 里面结构 设置li.innerHTML
      * 4.添加到ul 里，从前添加；
    * 把刚才新增的内容 的数据 存入 本地（服务器）：
      * 1.新建 对象 {} 
      * 2.给对象设置 obj.info = 文本域的内容’;
      * 3.把这个对象（一条数据）添加到数组（全局数组）
      * 4.把数组转化为JSON字符串；
      * 5.存入本地；



# 15-案例-发布微博v3-列表显示-03

```js
// 需求：真实数据已经存入本地了；但是：再次刷新，页面没有；
  // 分析：
  //  1.把本地数据拿出来；JSON 字符串
  var str = localStorage.getItem("list");

  // 优化：本地没有数据，先存入空 []
  if (str == null) {
    localStorage.setItem("list", "[]");
    // "[]"
    str = localStorage.getItem("list");
  }

  //  2.把JSON 字符串 转化为数组；全局list 被重新赋值；
  list = JSON.parse(str);

  //  3.循环遍历，
  for (var i = 0; i < list.length; i++) {
    // list[i]  {info:"xxx"}
    var li = document.createElement("li");
    li.innerHTML = `<p>${list[i].info}</p><span>删除</span>`;


    // 从前添加？从后添加？看数组的顺序  3 2 1
    ul.appendChild(li);
  }

  // 问题：不小心把本地U盘删除了；

```



# 16-总结

* 创建：document.createElement(标签名)，在JS代码里创建，没有添加到页面中；
* 修改内容：
  * innerHTML:可识别HTML标签
  * innerText：不识别；
* 添加：
  * 父节节点.appendChild(新DOM节点)：从后添加新的DOM节点；
  * 父节节点.insertBefore(新DOM节点，插入谁之前的DOM节点)：在某个元素之前，添加新的DOM节点
* BOM：
  * window
  * onload：静态资源全部加载完成后；
  * 定时器：
    * setTimeout
    * setInterval
  * localStorage：模拟数据存起来，以后把数据给后台  ajax 
  * JSON :
    * JSON 字符串
    * 内置JSON方法：配合JSON 字符串
* 微博：最为重要的一个案例
  * 配合：
    * HTML页面写完了；
    * 试着把HTML 表现 抽象为一个数据  [{},{}]
    * JS 代码了；
    * 拿着 抽象出来的数据 找 后台工程师；以后就要按照格来，key 名字可以改，格式不能动；
* 作业：
  * 先敲步骤：把每个步骤之间逻辑关系敲出来；
  * 删除功能：思考删除怎么做？
    * ID : 身份证；
    * 新增数据：给这个身份证；
    * 列表展示：把这个ID 和 当前这个标签写在一起；
    * 点击删除：
      * DOM操作‘
      * 数据删除：ID