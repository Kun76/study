## **jQuery** **属性操作**

```
prop操作固有属性，attr操作自定义属性
```

### **设置或获取元素固有属性值** **prop()**

```
所谓元素固有属性就是元素本身自带的属性，比如 <a>元素里面的 href ，比如 <input>元素里面的 type。
```

 **获取语法：**

> prop(''属性'')
>
> $('div').prop('id')

**设置属性语法**

> prop(''属性'', ''属性值'')
>
> $('div').prop('id','d2')

```
change事件，改变事件,发生变化后触发,表单中checked属性
: checked 伪类选择器,获取被选中的元素
```

### **设置或获取元素自定义属性值** **attr()**

```
用户自己给元素添加的属性，我们称为自定义属性。比如给 div 添加 index=“1”。 
```

**获取属性语法**

attr(''属性'')      // 类似原生 getAttribute()

**设置属性语法**

attr(''属性'', ''属性值'')   //类似原生 setAttribute()

### **数据缓存** **data()**【了解】

当做变量存储

```
data() 方法可以在指定的元素上存取数据，并不会修改 DOM 元素结构，所以元素上无法查看。一旦页面刷新，之前存放的数据都将被移除。
```

 **附加数据语法**

data(''name'',''value'')   // 向被选元素附加数据   

**获取数据语法**

date(''name'')             //   向被选元素获取数据   

**例如：**不会再html页面中显示

```
$('span').data('spanindex',3);

console.log($('span').data('spanindex'));
```

```
:checked 伪类选择器      :checked 查找被选中的表单元素。
```

## **jQuery** **内容文本值**

innerHTML，innerText，value

**普通元素内容** **html()**（相当于原生inner HTML)

​	获取：html()   // 获取元素的内容

​	设置：html(''内容'')   // 设置元素的内容

**普通元素文本内容** **text() **  (相当与原生innerText)

​	获取：text()   // 获取元素的文本内容

​	设置：text(''文本内容'')   // 设置元素的文本内容

**表单的值** **val()**（ 相当于原生value)

​	获取：val()   // 获取表单的值

​	设置：val(''内容'')  // 设置表单的值



toFiexd() 保留有效数字

p.toFiexd(2) 

parents,获取上级元素	parents("元素名")

## **jQuery** **元素操作**

> 主要是遍历、创建、添加、删除元素操作。

**遍历元素**

jQuery 隐式迭代是对同一类元素做了同样的操作。如果想要给同一类元素做不同操作，就需要用到遍历。

> 语法1：$("div").each(function(index, domEle) { xxx; }）

```
\1. each() 方法遍历匹配的每一个元素。主要用DOM处理。 each 每一个

\2. 里面的回调函数有2个参数：  index 是每个元素的索引号;  demEle 是每个DOM元素对象，不是jquery对象

\3. 所以要想使用jquery方法，需要给这个dom元素转换为jquery对象  $(domEle)
```

语法2：$.each(object，function(index, element){ xxx;}）

```
\1. $.each()方法可用于遍历任何对象。主要用于数据处理，比如数组，对象

\2. 里面的函数有2个参数：  index 是每个元素的索引号;  element  遍历内容
```

**创建元素**

语法：$(''<li>内容</li>'');    

**添加元素**

**内部添加**：产生的父子级关系

element.append(''内容'')    把内容放入匹配元素内部最后面，类似原生 appendChild。

也可以写成把内容添加到某个元素中      :       内容.appendTo.(element);

element.prepend(''内容'') 把内容放入匹配元素内部最前面。

**外部添加**: 产生兄弟关系

element.after(''内容'') // 把内容放入目标元素后面

element.before(''内容'')    //  把内容放入目标元素前面 

**删除元素**

element.remove()   //  删除匹配的元素（本身）

element.empty()    //  删除匹配的元素集合中所有的子节点

element.html('''')   //  清空匹配的元素内容

## **jQuery** **尺寸、位置操作**

### **jQuery 尺寸**

> width()、height()【只算width和height】
>
> innerWidth()、innerHeight()【包含padding+width】
>
> outerWidth()、outerHeight()【包含padding、border、width】
>
> outerWidth(true)、outerHeight(true)【包含padding、border、margin、width】

参数为空，则是获取相应值，返回的是数字型。
如果参数为数字，则是修改相应值。
参数可以不必写单位。

### **jQuery 位置**

> 位置主要有三个： offset()、position()、scrollTop()/scrollLeft();

**offset()设置或获取元素偏移**

```
1.offset() 方法设置或返回被选元素相对于**文档**的偏移坐标，跟父级没有关系。

2.该方法有2个属性 left、top 。offset().top  用于获取距离文档顶部的距离，offset().left 用于获取距离文档左侧的距离。

3.可以设置元素的偏移：offset({ top: 10, left: 30 });
```

**position()** 获取元素偏移

①position() 方法用于返回被选元素相对于**带有定位的父级**偏移坐标，如果父级都没有定位，则以文档为准。

②该方法有2个属性 left、top。

position().top 用于获取距离定位父级顶部的距离

position().left 用于获取距离定位父级左侧的距离。

注意：该方法只能获取。

**scrollTop()、scrollLeft()设置**或获取元素被卷去的头部和左侧

①scrollTop() 方法设置或返回被选元素被卷去的头部。

②不跟参数是获取，参数为不带单位的数字则是设置被卷去的头部。

scroll事件：滚动事件，（谁有滚动条加给谁）

###课程回顾：

​	**属性：prop，attr**

​	**文本内容值：html，text，val**

​	parents：上级元素

​	元素操作：

​			**遍历元素：**

​					$(元素).each(function (i, elm) {});

​					$.each(对象,function (i,elm)  {});

​			创建元素：$（'<li>哇哈哈</li>'）;

​			添加元素：**append，appendTo**，prepend，prependTo，after，before

​			删除元素：remove，empty，html('')

​		元素尺寸：width，height，innerWidth，outerWidth，outerWidth（true）

​		偏移位置：**offset**，position

​		卷起距离：**scrollTop**，scrollLeft 

​		**scroll事件：滚动事件**















