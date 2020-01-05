# DOM

## 1. 获取DOM节点

document.getElementById(id)

document.getElementsByClassName(class)

docment.getElementsByTagName(tagname)

document.body

document.querySelector(css选择器)

document.querySelectorAll(css选择器)

伪数组：下标获取元素、查length、可遍历、不能使用数组的其他方法

## 2. 事件

捕获事件：从根节点向目标节点传播

* 元素.addEventListener('事件类型'，函数，true)

冒泡事件：从目标节点向根节点传播    

* 元素.on+事件类型 = 函数
* 元素.addEventListener('事件类型'，函数，false)
* 利用：事件委托   e.target    (将事件委托给父级)
* 避开：阻止冒泡   e.stopPropagation()

事件对象：

* 阻止默认行为：e.preventDefault()
* 获取鼠标坐标： 
  * 可视区域坐标：e.clientX  e.clientY
  * 文档内：e.pageX    e.pageY  

e.target 和 this 在事件处理程序中有什么区别 ？

  e.target ： 触发的具体元素

  this： 绑定事件的元素

事件类型：

* 鼠标事件：click  mousedown  mouseup  mousemove  mouseover  mouseout  focus  blur   

* 键盘事件：keydown keyup  

  contextmenu

事件移除： 元素.on+事件类型 = null     元素.removeEventListenenr('事件类型'，函数名)

## 3.修改元素的内容

普通元素： 元素.innerHTML = '内容'；（识别标签）                                      元素.innerText = '内容'

表单元素： 元素.value = '内容'

## 4. 修改元素的样式

#### 1. style 

* 元素.style.样式属性名 = ‘样式属性值’
* 元素.style = '样式属性名：样式属性值'

#### 2. class

* 元素.className = '类名1  类名2'；
* 元素.classList.add('类名')     元素.classList.remove('类名')     元素.classList.toggle('类名')

## 5.自定义属性

* 设置 获取
  * ‘<p data-自定义属性名 = ‘值’></p>’     元素.dataset.自定义属性名
  * ‘<p 自定义属性名 = ‘值’></p>’ 
  * 万能（内置属性+自定义属性）自定义属性设置、获取、删除方法
    * 元素.setAttribute('属性名',‘属性’)
    * 元素.getAttribute('属性名')
    * 元素.removeAttribute(‘属性名’)

## 6.操作节点

* 获取元素的父元素  ： 元素.parentNode
* 获取子元素： 元素.children   
* 获取兄弟节点： 元素.nextElementSibling       元素.previousElementSibling

* 创建新元素： document.creatElement('标签名')
* 将节点插入到页面上   
  * 父元素.appendChild(元素对象)
  * 父元素.insertBefore(新元素对象,旧元素对象)

* 删除某个节点：  父元素.removeChild(子元素)

# BOM

#### 1. window对象的事件

* load  :  当页面静态资源加载完毕触发该事件
* resize：当窗口大小发生改变的时候触发该事件

#### 2. 定时器

* 延迟定时器：setTimeout(function(){},1000)   : 执行一次
* 周期定时器：setInterval(function(){},1000)   ： 多次
* 清除： clearTimeout(定时器名称)     clearInterval(定时器名称)

#### 3. loaction对象

* location.href = '地址'

#### 4. localStorage  

* localStorage.setItem('键名'，键值)
* localStorage.getItem('键名')
* localStorage.removeItem('键名')
* localStorage.clear()
* 周期：一直存在，只要不手动删除  
* 存储容量： 20M

#### 5. JSON

* 把复杂数据类型转换成具有原数据格式的字符串
* JSON.stringfy(复杂数据类型)
* JSON.parse(字符串)