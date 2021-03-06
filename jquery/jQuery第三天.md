课程回顾：

​	属性：prop，attr

​	parents：获取上级元素，toFixed

​	文本内容值：html，text，val

​	元素操作：

​		遍历元素：

```
$(元素).each(function () {})
$.each(对象,function () {})
```

​		创建元素：$('元素')

​		添加元素：append，appendTo，prepend，prependTo，after，before

​		删除元素：remove，empty，html

​	尺寸：width，innerWidth，outerWidth，outerWidth（true）

​	位置：offset，pisotion

​	卷起：scrollTop，scrollLeft

​	滚动事件：scroll

​	改变事件：change

## **jQuery** **事件**

目标：

```
能够说出4种常见的注册事件 

能够说出 on 绑定事件的优势

能够说出 jQuery 事件委派的优点以及方式

能够说出绑定事件与解绑事件
```

### **jQuery**事件注册

> 语法：element.事件(function(){})

```
$(“div”).click(function(){  事件处理程序 }) 
```

> 其他事件和原生基本一致。
>
> 比如mouseover、mouseout、blur、focus、change、keydown、keyup、resize、scroll 等

### **事件处理** **on()** **绑定事件**

> on() 方法在匹配元素上绑定一个或多个事件的事件处理函数

> 语法：element.on(events,[selector],fn)

```
\1. events:一个或多个用空格分隔的事件类型，如"click"或"keydown" 。

\2. selector: 元素的子元素选择器 。

\3. fn:回调函数 即绑定在元素身上的侦听函数。 
```

**on() 方法优势1：**

1、可以绑定多个事件，多个处理事件处理程序。 

```
 $(“div”).on({

  mouseover: function(){},
  mouseout: function(){},
  click: function(){} 

});       
```

**on() 方法优势2：**

> 可以事件委派操作。事件委派的定义就是，把原来加给子元素身上的事件绑定在父元素身上，就是把事件委派给父元素。

```
$('ul').on('click', 'li', function() {

​    alert('hello world!');

}); 

```

> 在此之前有bind(), live()，delegate()等方法来处理事件绑定或者事件委派，最新版本的请用on替代他们。  

**on() 方法优势3：**

> 动态创建的元素，click()没有办法绑定事件，on() 可以给动态生成的元素绑定事件

```
 $(“div").on("click",”p”, function(){

​     alert("俺可以给动态生成的元素绑定事件")

 });
```

### **事件处理** **off()** **解绑事件**

> off() 方法可以移除通过 on() 方法添加的事件处理程序。

```
$("p").off() // 解绑p元素所有事件处理程序

$("p").off( "click")  // 解绑p元素上面的点击事件 后面的 foo 是侦听函数名

$("ul").off("click", "li");   // 解绑事件委托
```

> 如果有的事件只想触发一次， 可以使用 one()来绑定事件。

### **自动触发事件trigger()** 

> 有些事件希望自动触发, 比如轮播图自动播放功能跟点击右侧按钮一致。可以利用定时器自动触发右侧按钮点击事件，不必鼠标点击触发

> element.click()  // 第一种简写形式

> element.trigger("type")//第二种自动触发模式

```
$("p").on("click", function () {

  alert("hi~");

}); 

$("p").trigger("click"); // 此时自动触发点击事件，不需要鼠标点击

```

> element.triggerHandler(type)  // 第三种自动触发模式

> triggerHandler模式不会触发元素的默认行为，这是和前面两种的区别。

## **jQuery**事件对象

> 事件被触发，就会有事件对象的产生。
> 【event==》事件对象】

```
element.on(events,[selector],function(event){})

```

```
阻止默认行为：event.preventDefault()   或者 return  false 

阻止冒泡： event.stopPropagation() 

```

上午回顾：

​	jQuery事件注册：

​			$(元素).click(function () {});

​			$(元素).on('事件类型'，'后代元素'，function () {});

​	解绑：$(元素).off('click','后代元素');

​	一次性：$(元素).one('事件类型',function () {});

​	自动触发事件：

​			$(元素).click();

​			$(元素).trigger('事件类型');

​			$(元素).triggerHandler(''事件类型);

​	事件对象：event：

​			阻止默认行为：event.preventDefault()

​			阻止冒泡：event.stopPropagation() 

## **jQuery** **其他方法**

> jQuery 插件

### **jQuery**插件

```
jQuery 功能比较有限，想要更复杂的特效效果，可以借助于 jQuery 插件完成。 

注意: 这些插件也是依赖于jQuery来完成的，所以必须要先引入jQuery文件，因此也称为 jQuery 插件。

**jQuery** **插件常用的网站：**

1.  jQuery 插件库  http://www.jq22.com/     

2.  jQuery 之家   http://www.htmleaf.com/  

**jQuery** **插件使用步骤：**

1.  引入相关文件。（jQuery 文件 和 插件文件）    

2.  复制相关html、css、js (调用插件)。

```

### 图片懒加载或者（BOOTSTRAP插件）

（图片使用延迟加载在可提高网页下载速度。它也能帮助减轻服务器负载）

当我们页面滑动到可视区域，再显示图片。

我们使用jquery 插件库  EasyLazyload。 注意，此时的js引入文件和js调用必须写到 DOM元素（图片）最后面

注意：

​	1、要引入JQuery

​	2、插件JS【js引入文件和js调用必须写到 DOM元素（图片）最后面】

​	3、将图片 src 替换为 data-lazy-src

​	4、调用lazyLoadInit(）

### BOOTSTRAP插件

​	1、引入CSS、引入JQ、引入JS

​	2、.container

​	3、复制粘贴

JQ==>J高级