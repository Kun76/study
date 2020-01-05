##jQuery

###jQuery引入

* jQuery: 是一个JavaScript库,
* JavaScript库：即 library，是一个封装好的特定的集合（方法和函数）。包括jQuery,PrototypeYUI,Dojo,Ext JS,移动端的zepto;

**链式编程 : 多个方法后面加. 来实现连写,**

**隐式迭代 :遍历内部 DOM 元素（伪数组形式存储）的过程就叫做 隐式迭代**

给匹配到的所有元素进行循环遍历，执行相应的方法，而不用我们再进行循环，简化我们的操作，方便我们调用。

###入口函数

第一种:

```js
$(functiuon(){
//此处是页面DOM加载完成的入口
});
```

第二种:

```js
$(document).ready(function(){
//此处是页面DOM加载完成的入口
});
```

### jQuery对象和DOM对象

* JS的对象只能用JS的属性和方法

  * 用原生 JS 获取来的对象就是 DOM 对象【document.getElement等方法】

* JQ的对象只能用JQ的属性和方法

  *  jQuery 方法获取的元素就是 jQuery 对象【$('div')等】

* 互相转化,有一些属性和方法JQ没有封装,需要转换后才能使用

  ```js
  DOM 对象转换为 jQuery 对象： $(DOM对象)
  
  jQuery 对象转换为 DOM 对象（两种方式）
  $('div') [index]       index 是索引号 
  $('div') .get(index)    index 是索引号
  ```

  

###jQuery选择器

```JS
$(“选择器”)   // 里面选择器直接写 CSS 选择器即可，但是要加引号   
$('#id')==》指定id元素

$('*')==》所有元素

$('.class')==》指定class元素

$('div')==》根据标签获取元素

$('div,p,li')==》获取多个

$('li.class')==>交集获取

$('ul>li')==>子代

$('ul li')==>后代
```

### 筛选选择器

```js
$("li:first"): 第一个元素
$("li:last"): 最后一个元素
$("li:eq(index)")  索引为index的元素【查找指定索引的元素】
$('li:odd')   索引为奇数
$('li:even')  索引为偶数
```

###**jQuery** **筛选方法（重点）**

$('选择器').方法()

```js
$('li').parent()父级
$('ul').children('li');子集【如果不加参数，获取所有的，如果添加指定的元素，按照指定的找】
$('ul').find('li')后代
$('li').siblings('li')兄弟
$('li'),nextAll();后面的兄弟
$('li').prevAll();前面的兄弟
判断是否具有某个类名：$('div').hasClass('aaa')
$('div').eq(index);指定索引方法【eq推荐用方法】 !!!
```

index( ); 索引值

###**jQuery** **样式操作**

jQuery 可以使用 css 方法来修改简单元素样式； 也可以操作类，修改多个样式。

**操作css 方法**

```js
参数只写属性名，则是返回属性值【$(this).css(''color'');】

参数是属性名，属性值，逗号分隔，是设置一组样式，属性必须加引号，值如果是数字可以不用跟单位和引号【$(this).css(''color'', ''red'');】

参数可以是对象形式，方便设置多组样式。属性名和属性值用冒号隔开， 属性可以不用加引号，
【$(this).css({ "color":"white","font-size":"20px"});】
```

**设置类样式方法**

```js
添加类【$(“div”).addClass(''current'');】

移除类【$(“div”).removeClass(''current'');】

切换类【$(“div”).toggleClass(''current'');】
```

###jQuery效果

所有的参数都可以省略， 无动画直接显示。
（1）speed：速度,可以用毫秒设置。
（2）easing：默认是“swing”，匀速“linear”。
（3）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。

**显示隐藏效果**

```js
show([speed,[easing],[fn]]) 【显示】
hide([speed,[easing],[fn]]) 【隐藏】
toggle([speed,[easing],[fn]])【切换】
```

**滑动效果**

```js
slideDown([speed,[easing],[fn]])【显示】
slideUp([speed,[easing],[fn]])【隐藏】
slideToggle([speed,[easing],[fn]]) 【切换】
```

**淡入淡出效果**

```js
fadeIn([speed,[easing],[fn]]) 【淡入】
fadeOut([speed,[easing],[fn]]) 【淡出】
fadeToggle([speed,[easing],[fn]]) 【切换】
fadeTo([[speed],opacity,[easing],[fn]])【达到透明某个程度】

//opacity 透明度必须写，取值 0~1 之间。
```

### 自定义动画

```js
语法：animate(params,[speed],[easing],[fn])

参数：
（1）params: 想要更改的样式属性，以对象形式传递，必须写。 属性名可以不用带引号， 如果是复合属性则需要采取驼峰命名法 borderLeft。其余参数都可以省略。
（2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
（3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
（4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
```

###事件切换

**hover([over,]out)**

$('div').hover(function () {//鼠标进入的函数},function () {//鼠标离开的函数});

```js
（1）over:鼠标移到元素上要触发的函数（相当于mouseenter）
（2）out:鼠标移出元素要触发的函数（相当于mouseleave）
（3）如果只写一个函数，则鼠标经过和离开都会触发它
```

**动画队列及其停止排队方法**

```js
动画或者效果一旦触发就会执行，如果多次触发，就造成多个动画或者效果排队执行。停止排队:stop()
(1）stop() 方法用于停止动画或效果。
(2)  注意： stop() 写到动画或者效果的前面， 相当于停止结束上一次的动画
```

i