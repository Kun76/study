# 01-WebApi介绍-JS组成-DOM介绍

* js组成：
  * ES：前6天基础知识，js语法；
  * DOM：文档当做成为**对象模型**，树状结构当做对象模型
    * DOM节点（JS的叫法）：HTML里面标签（标签属性、标签内的文本）；
    * 学习文档document上各种各样的方法：
      * 看参数：
      * 看返回值：
  * BOM：浏览器当做一个**对象**；



# 02-DOM-获取ById-注册click-属性style

```js
  // ---------------------------------------------------------
  // 作用: 获取页面中  已经存在  的HTML标签,DOM节点,元素对象;
  // 参数:字符串 id 值；
  // 返回:DOM节点（元素对象）,如果页面中没有这个DOM节点，返回null(复杂数据类型，代表空);
  //     元素对象---->属性和  法的集合；
  // var btn = document.getElementById("btn");
  // console.log(btn);
  // 特别: 返回body DOM节点
  // var body = document.body;



  // --------------------------------------------------------
  // DOM节点，元素对象---->属性和方法的集合；
  // 作用: 用户和指定DOM节点交互;
  // 给DOM节点注册事件：方法名 默认的onclick,用户的点击行为;
  // 【!!!方法什么时候被调用？】用户真的去触发点击行为的时候,函数才会执行;(什么时候执行不确定,不是由代码控制) 用户点击一次：代码执行：btn.onclick();
  // 语法：
  // btn.onclick = function() {
  //   alert(1);
  // };
  // 代码调用；
  // btn.onclick();


  // ------------------------------------------------------
  // DOM节点的属性：style 
  // 标准属性：标签内内置好的属性：src(img)  id  class  style
  // btn.style背后：DOM设置的CSS组成属性的对象；

  // 
  // CSS属性：点的方式 obj.css属性 ----->行内的样式的值；
  // 返回：行内的样式的值；
  // ---------------------------
  // 获取：
  // var obj = btn.style;
  // console.log(obj.width);
  // console.log(obj.height);
  // 设置：
  // obj.width = "360px";
  // 作用：通过JS控制页面中已经存在的HTML节点 ----->样式；
  
  // 格式使用：
  // btn.style.width = "400px";


  // 需求：点击按钮，让页面背景色发生改变；
  // 分析：
  // 1.谁？按钮
  // 2.谁发生什么行为，注册事件？用户点击按钮
  // 3.行为之后，谁怎么了？页面背景色发生改变；

  var btn = document.getElementById("btn");
  var body = document.body;
  btn.onclick = function() {
    // 点击之后，调用函数，
    body.style.backgroundColor = "red";
  };
```



# 03-DOM-案例-两个按钮开关灯

* 1.获取DOM节点：
* 2.注册事件：
  * 注册给谁？
  * 注册什么事件类型；
* 3.事件发生之后发生什么？函数体内部的代码；

```js
// 需求：点击关灯，页面变黑；开灯，页面变白；

  // 分析：
  //  1. 获取谁？
  //  2. 谁注册什么事件？
  //  3. 点击后之后，谁这么了？
  var body = document.body;
  var close = document.getElementById("close");
  close.onclick = function() {
    // 必须设置字符串
    body.style.backgroundColor = "#000";
  };


  // 
  var open = document.getElementById("open");
  open.onclick = function() {
    body.style.backgroundColor = "#fff";
  };
```



# 04-DOM-案例-一个按钮开关灯

* 思想：通过DOM节点上value属性值，判断当前DOM节点不同的状态；不同的情况分支走不同的设置；

```js
 // DOM:onclick
  // DOM.style;
  // DOM节点：标准属性 value;有value属性的标签；
  // // 获取
  // console.log(btn.value);
  // // 设置
  // btn.value = "aaaaaaaaa";



  // 需求：一个按钮控制开关灯；
  // 分析：
  //   按钮是关灯：背景色白色，点击之后：
  //   按钮字是开灯，背景变为黑色； 点击之后：
  //   按钮字是关灯，背景变为白色，
  //  

  //  1.谁？
  var btn = document.getElementById("btn");
  var body = document.body;

  //  2.注册点击事件
  btn.onclick = function() {
    // 看下当前这个按钮是什么字？不同的字，实现不同的效果；
    // btn.value

    // 按钮字是 开灯,点击之后,变白
    if (btn.value == "开灯") {
      body.style.backgroundColor = "#fff";
      // 本身的按钮
      btn.value = "关灯";
    }
    // btn.value == "关灯"
    else {
      // 背景
      body.style.backgroundColor = "#000";
      // 
      btn.value = "开灯";
    }
  };
```



# 05-DOM-注册focus和blur-案例

```js
  // 需求：
  // 有光标的时候。下面的图片显示；
  // 没有光标的时候，图片隐藏；


  // 分析：
  //  1. 获取哪些DOM节点；
  var search = document.getElementById("search");
  var list = document.getElementById("list");

  // 2. 触发什么行为？
  // 3. 触发之后，谁这么了？
  search.onfocus = function() {
    list.style.display = "block";
  };

  search.onblur = function() {
    list.style.display = "none";
  };

  // DOM事件.onfocus
  // 作用：用户让input框聚焦的时候，执行函数里面的代码
  // 语法：
  // var ipt = document.getElementById("search");
  // ipt.onfocus = function() {
  //   // 何时调用函数？用户决定；
  //   // alert(1);
  //   console.log(1);
  // };
  // // !!!当用户 让input有光标的时候，执行 ipt.onfocus();

  // // 函数名：不是我们定，触发不是开发人员控制；
  // ipt.onblur = function() {
  //   // 何时调用函数？用户决定；
  //   // alert(1);
  //   console.log(2);
  // };
```



# 06-DOM-属性-类名-className

* className：属性，获取和设置DOM节点上的class

```js
  // 作用：直接控制已有的CSS类名（和style对比）
  // 属性：DOM.className
  // 获取值：HTML DOM节点 行内class属性的值；字符串
  var div = document.getElementById("box");
  // console.log(div.className);
  // 设置：原来的能看到，现在也能看到
  // div.className = div.className + " bg-red";


  // 需求：点击盒子，添加类名
  // 分析：
  //  1. 谁？
  var btn = document.getElementById("btn");
  // 2. 注册什么事件？
  // 3. 点击之后，谁怎么了？
  btn.onclick = function() {
    div.className += " bg-red";
  };


```



# 07-DOM-属性-类对象-classList-01-语法

```js
  //  问题：
  // 1. 一直点击按钮，bg-red 一直添加；
  // 2. 如果现有类名："abc aa bb cc dd"，通过className 删除一个 "bb";  非常不好做；


  // DOM.classList 属性名
  // 值：对象（和style记忆），上面有很多方法，直接方便的操作类名，解决2个问题；
  var box = document.getElementById("box");

  // .add()
  // 参数：新的类名（多个类名，逗号隔开）  解决问题1；
  // box.classList.add("bg-red");
  var btn = document.getElementById("btn");
  btn.onclick = function() {
    box.classList.add("bg-red");
  };

  // .remove()
  // 参数：指定的类名被删除（一个或多个）  解决问题2；
  // box.classList.remove("def");
  var btn2 = document.getElementById("btn2");
  btn2.onclick = function() {
    box.classList.remove("bg-red");
  };

  // .toggle()
  // 参数：切换类名。没有类名，加类名。有已经被添加的类名，加删除该类名
  var btn3 = document.getElementById("btn3");
  btn3.onclick = function() {
    box.classList.toggle("bg-red");
  };
```





# 07-DOM-属性-类样式对象-classList-02-搜索案例

```js
// 需求：有光标的时候，list显示；没有光标的时候，隐藏；CSS类名进行控制；

  // 分析：
  //  1. 谁？
  var search = document.getElementById("search");
  var list = document.getElementById("list");
  search.onfocus = function() {
    // list.style.display = "block";
    list.classList.add("show");
  };

  // 
  search.onblur = function() {
    // list.style.display = "none";
    list.classList.remove("show");
  };
```

* 以后操作类名就用 DOM.classList.方法();



# 08-DOM-获取ByTagName和ByClassName

* 小括号里面：放入字符串；

```js
  // document.getElementById
  // getElementsByTagName
  // 参数：TagName 标签名 字符串；
  // 返回：拥有标签名的伪数组，成员是DOM节点；  for循环；
  var ipts = document.getElementsByTagName("input");
  console.log(ipts);

  // getElementsByClassName
  // 参数：ClassName 类名 字符串；
  // 返回：拥有类名的伪数组，成员是DOM节点；  for循环；
  var btns = document.getElementsByClassName("btn");
  console.log(btns);


  // 注意：页面中元素只有一个，返回的是伪数组；

  // DOM节点 == DOM节点
  // 对象 == 对象   地址？
  // console.log(ipts[0] == btns[0]);
```







# 09-DOM-案例-点击按钮切换图-01-自定义属性

```js
  // 标准属性：HTML标签 规定 id class src alt title style
  // 获取标准属性  属性名：
  var img = document.getElementById("img");
  // console.log(img.id);
  // console.log(img.className);
  // console.log(img.src); 
  // console.log(img.alt);
  // console.log(img.title);
  // 

  // 需求：点击按钮，切换图片
  var btn_1 = document.getElementById("btn_1");
  btn_1.onclick = function() {
    img.src = "./images/01.jpg";
  }

  var btn_2 = document.getElementById("btn_2");
  btn_2.onclick = function() {
    img.src = "./images/02.jpg";
  }

  var btn_3 = document.getElementById("btn_3");
  btn_3.onclick = function() {
    img.src = "./images/03.jpg";
  }

  // 【!!！】btn_3.onclick 方法啥时候调用？用户交互决定；执行 btn_3.onclick();


  // 工作：我们一般习惯，把一一对应的关系数据写在标签内；
  // 如何写？
  // 自定义属性：开发人员（一一对应），往标签上面写任何我们想写的属性名；名字自己起，值自己设置；对标签来说，自定义没有任何影响；
  // 基础语法：<input type="button" value="图片1" id="btn_1" aaa="1">  aaa="1"

  // 形式1：
   // 约定：自定义属性名字： data-自己命名 = 值;  <input type="button" value="图片1" id="btn_1" data-abc="1">
  // 获取：DOM节点.dataset属性名，背后值是一个对象，对象上有刚才自定义的 属性abc 和 值"1"；
  console.log(btn_1.dataset);

  // 意义：一般要标签 btn和 某个数据(图片地址) 形参一一对应关系，放在自定义属性里；
```



# 09-DOM-案例-点击按钮切换图-02-具体操作

```js
  // 意义：一一对应；
  // 自定义属性：把有 一一对应 关系的数据，写入标签内； data-自己定的名字
  // 获取：Dom.dateset----->对象；


  // 步骤：
  // 1.新增 HTML结构 里 自定义属性名和值；
  // <input type="button" value="图片1" id="btn_1" data-dizhi="./images/01.jpg">


  // 2.JS代码
  // 2.1 获取哪个DOM节点？
  var btn_1 = document.getElementById("btn_1");
  var btn_2 = document.getElementById("btn_2");
  var btn_3 = document.getElementById("btn_3");
  var img = document.getElementById("img");
  // 2.2 给谁注册什么事件？
  btn_1.onclick = function() {
    // 2.3 点击之后，谁怎么了？图片地址被更换；
    // 
    // console.log(btn_1.dataset.dizhi);
    img.src = btn_1.dataset.dizhi;
  }
  btn_2.onclick = function() {
    // 2.3 点击之后，谁怎么了？图片地址被更换；
    // 
    // console.log(btn_2.dataset.dizhi);
    img.src = btn_2.dataset.dizhi;
  }
  btn_3.onclick = function() {
    // 2.3 点击之后，谁怎么了？图片地址被更换；
    // console.log(btn_3.dataset.dizhi);
    img.src = btn_3.dataset.dizhi;
  }
```

* **this规则:  关键字，在函数内部的时候，看this背后代表谁？谁在调用函数，this就指向谁；**

```js
btn_1.onclick = function() {
    // 2.3 点击之后，谁怎么了？图片地址被更换；
    // console.log(btn_1.dataset.dizhi);
    // img.src = btn_1.dataset.dizhi;
    
    img.src = this.dataset.dizhi;
    
    // this==btn_1 返回true
}
```

* 问题：一万个按钮，怎么办？
  * 一万个按钮全部拿到？
  * for



# 09-DOM-案例-点击按钮切换图-03-循环遍历

```js
 // 需求：有很多按钮，怎么办？
  // 分析：
  //  1. 全部拿到所有的按钮；
  var btns = document.getElementsByTagName("input");
  var img = document.getElementById("img");

  // 2.给每一个注册事件 点击
  for (var i = 0; i < btns.length; i++) {
    // 点击之后：啥时候点击？点击之前for 循环执行完成；i==btns.length
    btns[i].onclick = function() {
      // img.src = btns[i].dataset.dizhi;
      img.src = this.dataset.dizhi;
    }

  }

  // 报错：Cannot read property 'dataset' of undefined
  // 解析：undefined.dataset
  // 原因：模拟for
  // --------------------------------------------
  // var i = 0; i < 3;  i：3

  // 给对象 设置方法名 =  function() {}  设置
  // btns[0].onclick = function() {
  //   img.src = btns[i].dataset.dizhi;
  // }

  // btns[1].onclick = function() {
  //   img.src = btns[i].dataset.dizhi;
  // }

  // btns[2].onclick = function() {
  //   img.src = btns[i].dataset.dizhi;
  // }

  // 调用：执行函数，用户点击的时候调用；调用的时候，此时 i:3
  // 没有这个下标，btns[3] 返回undefined；
```





# 10-DOM-案例-点击按钮切换图v2-01

```js
    // 先把DOM全部获取到；伪数组；循环遍历
    // 伪数组：有下标，有长度，可for循环；
    // 页面元素只有一个，返回的是只含有一个元素的伪数组；
    var btns = document.getElementsByClassName("ipt");

    var img = document.getElementById("img");

    // for：重复的，一次又一次；
    // var btn;
    for (var i = 0; i < btns.length; i++) {
      // 循环的一次；给DOM节点注册事件
      // 元素对象：赋值的就是地址；
      // btn = btns[i];
      // // 注册事件
      // btn.onclick = function() {

      // }
      // 
      btns[i].onclick = function() {
        // 点击之后：图片要进行切换图；src标准属性
        // 自定义：当前循环的标签 DOM节点 上 自定义属性 data-src
        img.src = this.dataset.src;
      }

    }
```

# 10-DOM-案例-点击按钮切换图v2-02-补充

* 为什么在循环内部使用注册事件，事件执行内部写btns[i]会报错；
* 这里在循环内，注册事件，内部的执行体获取元素对象只能用this;

```js
// -------------------------------------模拟for循环
  // var i = 0; 执行一次

  // i：5 全局变量,每次循环执行完成后，i都要变一次；

  // i:0;
  // 进入第一次循环 0<5;
  btn_1.onclick = function() {
    img.src = btns[i].dataset.src;
  };
  // i++ 

  // i:1;
  // 进入第二次循环 1<5;
  // 后面的函数不会一下就执行；用户点击的时候才会执行；
  btn_2.onclick = function() {
    // 函数响应体：什么时候执行？
    // 用户点击之后才执行；
    // 用户啥时候点击，我不知道，
    // 但是我知道：用户没有点击的时候，循环已经完成；
    // 完成 后 此时 i 变为5 
    // 那么用户点击的时候，此时 i 的值已经变为5；
    // btns[5] 此时 是undefined 
    // 报错： Cannot read property 'dataset' of undefined

    img.src = btns[i].dataset.src;
  };
  // i++

  // i:2;
  // 进入第三次循环 2<5;
  btn_3.onclick = function() {
    img.src = btns[i].dataset.src;
  };
  // i++


  // i:3;
  // 进入第四次循环 3<5;
  btn_4.onclick = function() {};
  // i++


  // i:4;
  // 进入第五次循环 4<5;
  btn_5.onclick = function() {};
  // i++


  // 此时 i = 5
```





# 11-DOM-属性checked

* 属性：开关属性： checked/selected/disabled ，这种只有两种状态的属性；`<input type="checkbox" name="check" id="ck"  />`
* 获取：DOM.checked
* 设置：`ck.checked = true;` 两个状态的值，布尔类型；

```js
var ck = document.getElementById('ck');

ck.checked = true;
ck.checked = false;
```



# 12-DOM-案例-全选与取消

* 语法：

```js
// 步骤：
  // 1. 获取谁？
  var all = document.getElementById("checkAll");
  var cks = document.getElementsByClassName("ck"); // 伪数组 

  // 2. 给谁注册什么事件？all 点击事件
  all.onclick = function() {
    // 3. 点击之后，子项ck 跟着 all状态 动起来，

    // 3.1 all状态 先获取到； all.checked
    var flag = all.checked;
    // 3.2 把所有子ck拿到；cks
    // 3.3 把all 状态给每一个子ck上；
    for (var i = 0; i < cks.length; i++) {
      cks[i].checked = flag;
    }

  }
```



# 13-DOM-案例-反选

```js
  // 需求2：子选项 的变化 会使大选择框的状态的改变

  // 分析：
  //  1.子选ck，每一个都有点击事件
  //  2. 点击子项ck之后：
  //   2.1 三兄弟都是选择状态，all选择；
  //   2.2 三兄弟中至少有一个没有选择，all不选择；

  for (var i = 0; i < cks.length; i++) {
    cks[i].onclick = function() {
      // 三兄弟状态的判断：每一个兄弟状态看一次；
      for (var j = 0; j < cks.length; j++) {
        // 如果有一个兄弟，没有选中。没有必要继续循环 
        if (cks[j].checked == false) {
          break;
        }
      }

      // 如果j==cks.length ,上面的循环全部完成，没有一个进入终止，没有一个执行  cks[j].checked == false
      if (j == cks.length) {
        all.checked = true;
      }
      // j!=cks.length 上面的循环没有完成，意味循环被终止，有一个子ck 执行 cks[j].checked == false
      else {
        all.checked = false;
      }
    }
  }
```



# 14-小结

* 获取页面HTML节点：DOM节点（对象：属性和方法的集合体）

  * document.getElementById：一个,没有拿到就是null
  * getElementsByTagName：伪数组；for
  * getElementsByClassName

* 事件：对象上的一个方法

  * 名称不是我们定的；JS内置定；
  * 交互功能：【!!!】用户在触发事件，相当于在调用对象上方法；DOM.onclick();
  * 学什么：
    * onclick
    * onfocus
    * onblur

* 对象上属性：

  * 标准属性：
    * style：值是对象，CSS样式的对象，行内样式；
    * value：input 文本框类，
    * className：值是类名字符串，新增（和原来的字符串形成拼接）；删除（不好做）
    * classList：值是对象，很多方法：
      * add:新增类名（多个），解决多次重复问题；
      * remove：删除指定（多个），解决className不好删除类名
      * toggle：切换类名（一个）
  * 自定义属性：
    * 意义：把一一对应的数据 写在一起。写在HTML标签内部；
    * HTML写格式：`data-自定义属性名 = 属性值`
    * JS如果获取：`DOM.dataset.自定义属性名`;

* 案例：

  * 切换图片：

    * this：看函数属于谁，this就是那个谁；如果在函数内部，你要使用 注册给哪个DOM节点。用this;【重点】

  * 全选与反选：

    * 全选：
      * 1.获取全选all
      * 2.给all 注册点击事件；
      * 3.点击之后：
        * 获取自己 all 状态；
        * 给每一个子项都是和all 一样状态；

    * 反选：
      * 1.子项都要获取
      * 2.每个子项要注册点击事件
      * 3.点击之后：
        * 看子项的兄弟们的状态，如果有一个没有选中，all不能选；
        * 看子项的兄弟们的状态，如果三个兄弟全部选中，all跟着选中；

  * 先思维步骤：!!!
    * 填入代码；

  



