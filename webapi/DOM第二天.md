# DOM

## 1.DOM获取css选择器

* document.querySelector(.x  ); 

  输入:css语法,点(CSS)跟#(ID)格式不能省,返回第一个节点;没有返回null;

* document.querySelectorAll( );

  返回一个伪数组,可遍历

## 2.操作属性

元素对象.getAttribute(自定义属性名);

* 方法:侧重于自定义属性,标准属性也能获取,不限定于: data - 格式;
* 参数:传入查询的自定义属性名称
* 返回:查询的结果,自定义属性的值

设置: 元素对象.setAttribute(x,x);

参数:第一个自定义属性名,第二个自定义属性值

删除 : 删除自定义属性

元素对象.removeAttribute(自定义属性名)  参数:自定义属性名;

意义: 一一对应关系的数据,用方法,不限定HTML格式(data-);

## 3.注册事件

addEvebtListener

可以解决多次注册同`	一事件覆盖问题;

参数: 事件类型 - 字符串; 事件处理程序 (匿名函数)

DOM节点.addEventListener(类型(click,focus,blur),匿名函数);

## 4.DOM事件三个阶段

* 三个阶段: 捕获,到达目标,冒泡
  * 捕获:从根部节点一层一层往上找,直到找到我们刚才触发的那个节点,这过程叫捕获;
  * 到达目标: 找到我们刚才触发的那个节点;
  * 冒泡: 从找到的目标节点一层一层往根节点找;这个过程叫冒泡;
* **事件默认是在冒泡阶段执行**;
  * 在冒泡的阶段,发现父级节点也注册了和用户行为一样的事件,用户触发了子元素,父元素也会跟着触发
  * 冒泡阶段执行默认值false, 捕获阶段执行true;为什么冒泡阶段执行?用户体验好
* 阻止冒泡:  需要在函数上设置形参e;     e.stopPropagation(); 
  * stop: 停止,掐断
  * Propagation: 传播,冒泡这条线

## 5.事件对象

事件对象：把一次**事件**也看成对象 ,形参e ,一般习惯用e ,(event的缩写)

位置:  e参数,函数内部使用

事件源(DOM).addEventListener(事件类型,function(事件对象){});	

* 鼠标位置:
  * 参照当前可视窗口左上角为基准点:e.clientX,e.clientY
  * 参照body左上角为基准点:e.pageX,e.pageY
* e.target: 点击谁就是谁 -------DOM节点
* e.currentTarget: 事件注册给谁就是谁,e.currentTarget == this -------true;
* 阻止冒泡: e.stopPropagation( );掐断冒泡着条线
* 阻止默认行为: e.preventDefault( );右键弹窗,a转跳

* 阻止a标签转跳
  1 .把a标签的href属性设置为 javascript:void(0);
  2 .在a标签的点击事件里面，return false;
  3 .使用事件对象.preventDefault( );

## 6.事件委托

属性 DOM.innerHTML  获取: 内部HTML结构,字符串

属性  DOM.nodeName   返回大写的标签名;

​	不给子元素注册事件,给父级注册事件,事件默认在冒泡阶段执行,点击子元素,冒泡到父级上,父级的事件会被触发;

**什么时候用？当我们需要给动态创建（不是页面一开始写死的，是后期可能会变、被新增的DOM元素）的元素实现注册事件的效果的时候；**

```js
  // 鼠标在某个元素上 落下的时候，触发
  box.addEventListener("mousedown", function() {
    console.log(1);
  });

  // 鼠标在某个元素上 移动，触发
  box.addEventListener("mousemove", function() {
    console.log(2);
  });

  // 鼠标在某个元素上 弹起 触发
  box.addEventListener("mouseup", function() {
    console.log(3);
  });
```





## 7.元素对象在页面中的位置

* margin 会影响定位么？会影响定位，定位以margin左上角的点 定位 
* 属性:DOM.offsetTop;offsetLeft与有定位的父级的水平和垂直距离 offsetParent：返回有定位的父亲；

## 8.开关思想和解绑

* 开关思想:声明一个变量,通过代码控制变量不同情况,不同的值;
* 事件解绑 
  * btn.onclick = null;
  *  btn.removeEventListener('click',fn);

```js
var btn = document.getElementById('btn');
btn.onclick = function(){
  btn.onclick = null;
  console.log('谢谢惠顾');
}

var btn = document.getElementById('btn');
btn.addEventListener('click',function fn(){
  // 解绑 当前的函数
  btn.removeEventListener('click',fn);
  console.log('抽奖了');
})
```

