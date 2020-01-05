# ES6

[ECMAScript](https://baike.baidu.com/item/ECMAScript/1889420) 6，简称ES6 。是 Javascript 语言下一代标准。随着js语言应用场景的不断扩展，开发者需要编写的代码越来越复杂。在这个背景下，ES6 的目标是`使JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言 `。ES 是一种语言标准，6表示版本号。

ES2015：称为es6。

## let和const

### let

作用: 定义变量。(var也是用来定义变量的)

特点: 

* 拥有块级作用域,外边无法使用里面的变量
* 不能重复声明同一个变量
* 全局变量不会附加到window对象中
* 没有变量的提升(预解析),必须先声明在使用

格式： `let 变量名 = 变量值`；

### const

作用: 定义常量 (定义一个不允许修改的数据,只读的数据)

特点: 

* 一旦定义就不能修改,定义必须设置初始值

* 拥有块级作用域,外边无法使用里面的变量
* 全局变量不会附加到window对象中
* 没有变量的提升(预解析),必须先声明在使用

格式： `const 常量名 = 常量值;`

### const本质

const实际保证的并不是变量的值不得改动,而是变量指向的那个**内存单元中保存的数据不能改动**

* 对于简单的基本类型数据(number,string,boolean),值就保存在变量指向的那个内存单元(栈区),因此等同于常量值
* 对于复杂类型数据(对象和数组),变量指向那个内存单元(栈);但是保存的只是一个指向实际数据的指针(地址);const只能保证这个地址是固定的,而地址中的数据结构是否不可变就无法控制了

**深冻结** :

​        如果你真的希望**定义一个不能修改的对象**（属性不能添加，修改，删除），你可以使用**Object.freeze()**冻结。下面是一段参考代码：它一个可以把一个对象全部冻结的函数（深冻结）:

```js
// 创建函数,用于深冻结
function deepFreeze(obj) {
 // 首先通过 Object.freeze()对对象属性冻结
  Object.freeze(obj);
  for (var key in obj) {
       // 判断类型是否为复杂类型,是就冻结
  	if (typeof obj[key] === 'object' && obj[key] !== null) {
         // 递归调用
       deepFreeze(obj[key])
         }
    }
}
// 声明要冻结的对象
const obj = {
  name: 'jack',
  age: 18,
  hobbies: {
    eat: '吃',
    drink: '喝',
    run: '跑步',
    playBall: ['篮球', '弹球', '悠悠球']
  }
};
// 调用函数冻结对象obj
deepFreeze(obj);
obj.name = 'rose';
obj.hobbies.eat = '各种吃';
obj.hobbies.playBall[2] = '各种弹';
console.log(obj); // obj没有变化，说明冻结完成
```

### 小结

| 关键字 | 变量提升 | 块级作用域 | 是否必设初始值 | 是否可更改 | 是否是顶级对象属性 |
| :----: | :------: | :--------: | :------------: | :--------: | :----------------: |
|  let   |    ×     |     √      |       -        |    Yes     |         No         |
| const  |    ×     |     √      |      Yes       |     No     |         No         |
|  var   |    √     |     ×      |       -        |    Yes     |        Yes         |

##解构赋值

定义: ES6 允许按照一定**模式**，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

### 数组解构赋值

格式：

```javascript
let [变量1=默认值1, 变量2=默认值2, 变量n=默认值n] = [数组元素1, 数组元素2, 数组元素n];
```

> 注：默认值是可选的，可以设置，也可以不设置；

规则: 

* 赋值符号左右两边都是数组,把右边的数组按下标从小到大取出来,放到左边的数组中
* 如果左边的变量没有分配到某一个值,有默认值就是默认值;没有就是undefined

```js
// 数据都是一一对应,如果对了就是只声明没有赋值;  
		var arr = [11, 22, 33, 44, 55];
        var [a, b, c, d, e, f] = arr;
        console.log(a, b, c, d, e, f);

// 一一对应,少了声明的数据,获取的也是依次的数据
        var arr = [11, 22, 33, 44, 55];
        var [a, b] = arr;
        console.log(a, b);//11,22

        // 如果想获取后面的,就用逗号分隔
        var arr = [11, 22, 33, 44, 55];
        var [a, , , , b] = arr;
        console.log(a, b);//11,55

 // 默认值
     // 设置默认值后,可以取到默认值;如果可以取到值,就取值,没有值,就去默认值
        var arr = [11, 22, 33];
        var [a, b = 10, c, d = 0, e = a, f = 10, g] = arr;
        console.log(a, b, c, d, e, f, g);
									//11,22,33,0,11,10,undefined

// 剩余值
    // 在结构左侧的最后一个名称前设置...,表示获取剩余值;
   // 没有获取的数据将保存在最后一个数组中;如果剩余值不在最后使用就会报错
        var arr = [11, 22, 33, 44, 55];
        var [a, ...b] = arr;
        console.log(a, b);//11,[22, 33, 44, 55]
```

###对象解构赋值

作用 ：快速从对象中获取值保存到变量中。它的本质是给变量赋值。

格式 : 

```js
let {"属性名1"：变量名1=默认值1, "属性名2"：变量名2=默认值2,... } = {"属性名1"：属性值1,"属性名2"：属性值2,...}
```

解析规则：

- 默认值是可选的。
- 右边的"属性名"与左边的“属性名” 一致，则把右边的属性值赋值给左边的变量名。
- 如果右边的匹配不成立，看看是否有使用默认值，有默认值就使用默认值，没有就是undefined。

```js
// 个数不对应可以不用逗号分隔,对象是只是获取类名相同的属性值,名称相同即可,顺序无所谓
let obj = {
  name: '张三',
  age: '1000',
  school: 'dark',
  gf: {
    hby: 'sing',
    qwe: "123",
     }
        };
// 如果右边的属性名跟左边变量名相同,则把右边的属性值赋值给左边的变量名
let { name, age, hhh } = obj
console.log(name, age, hhh);

// 别名
let { name: mingzi, age: nl, hhh } = obj
console.log(mingzi, nl, hhh);

 //默认值
let { name, age, hhh = '黑马' } = obj
console.log(name, age, hhh);
  
 // 别名和默认值 ,先写别名在写默认值
let { name: ming = '熊大', age = 111, school: hhh = '学校' } = obj;
console.log(ming, age, hhh);

//   剩余值
let { name, ...a } = obj;
console.log(name, a);

// 获取对象中对象的属性 
let { age, gf: { hby }, gf } = obj;
console.log(age, hby);
console.log(gf);
```

##定义对象的简洁方式

```js
let name = 'zhangsan', age = 20, gender = '女';
let obj = {
    name: name, // 原来的写法
    age, // 对象属性和变量名相同，可以省略后面的 “:age”，下面的gender同理
    gender,
    fn1 : function(){  // 常规写法
        console.log(123);
    },
    fn2 () { // function可以省略 
        console.log(456);
    }
};
console.log(obj.age); // 20
obj.fn2(); // 456
```

## 函数的拓展

### 参数默认值

当定义一个函数时,我们可以给形参设置默认值 : 当用户不传入实参时;我们就有个一个默认值可以使用

传统方式

```JS
// ES5 中给参数设置默认值的变通做法，以ajax函数部分封装为示例：
function ajax (type, url, isAsync) {
	// 基本写法：  
  if(isAsync === undefined){
      isAsync = true;
  }
  // 简化写法：
	// isAsync = isAsync || true;
  
  console.log(type, url, isAsync);
	//.. 其他代码略
}
// 下面这两句是等价的
open("get","common/get"); // 参数3使用默认值true
open("get","common/get",true);

open("get","common/get",false);
```

ES6的实现

格式:

```js
function 函数名 (参数名1=默认值1，参数名2=默认值2，参数名3=默认值3){
    // 函数体
}
```

示例: 

```js
function open(type, url, isAsync = true) {
    console.log(type, url, isAsync);
}
// 下面这两句是等价的
open("get","common/get")；// // 参数3使用默认值true
open("get","common/get",true);

open("get","common/get",false); 
```

### rest参数

rest （其它的，剩余的）参数 用于获取函数多余参数，把它们放在一个数组中。

rest参数不能设置默认值，且必须设置在参数列表最后位置

在定义函数时，在最后一个参数前面加上`...`， 则这个参数就是剩余参数；

```js
let fn = function(参数1，参数2，...rest参数){}

let fn = (参数1，参数2，...rest参数) => { }; // 箭头函数具体使用，见下一小节。
```

rest 参数，它可以替代 arguments 的使用

与arguments相比，它是一个真正的数组，可以使用全部的数组的方法。

问题：编写一个函数，求所有参数之和；

```js
//arguments方法
function getSum (){
    //  在这里写你的代码
    var sum = 0 ; 
    for(var i = 0; i < arguments.length; i++){
        console.info( arguemnts[i])
        sum += arguments[i];
    }
}
//如果以箭头函数 的方式去定义这个函数，则内部不可以使用arguments这个对象了。此时，我们就可以使用剩余值

//rest方法
const  getSum = (...values) => {
    var sum = 0 ; 
    for(var i = 0; i < values.length; i++){
        console.info( values[i])
        sum += values[i];
    }
}
// 调用
console.log(fn(6, 1, 100, 9, 10));
```

## 箭头函数

> 箭头函数本质上是 匿名函数的简化写法。

### 格式

```javascript
let 函数名 = (形参1，...形参n) => {
    // 函数体
};
```

格式小结：

1. 去除function关键字
2. 在形参和函数体之间设置 `=>`

### 简化写法

- 当形参有且只有一个，可以省略小括号

  ```js
  let f = (x) => {console.log(x)}
  // 可以简化成：
  let f = x => {console.log(x)}
  ```

- 当函数体只有一条语句，可以省略大括号; 

  ```js
  let f = x => {console.log(x);}
  // 可以简化成：
  let f = x => console.log(x);
  ```

- 当函数体只有一条语句，并且就是return语句，则可以省略return和大括号。

  - 如果省略了大括号，则必须省略return，否则报错

  ```js
  let f = x => {return x*2; }
  // 可以简化成：
  let f = x => x*2;
  ```

- ` 如果返回值是一个对象，要加()

  ```js
  let f = x => { return {a:1};  }
  // 可以简化成：
  let f = x => {a:1}; // 如果不加，{}会被认为是函数的代码块，不会被解析器当成对象处理
  let f = x => ({a:1});
  ```



### 箭头函数与普通函数的区别

- 内部没有this
  - 内部的`this`对象，指向定义时所在的对象，而不是使用时所在的对象。
- 内部没有arguments（了解）
  - 因为es6中提供了函数的rest剩余值功能，可以替代arguments
- 不能作为构造器（了解）

## 数组的拓展

### 扩展运算符

功能：它的作用是把数组中的元素一项项地展开：把一个整体的数组拆开成单个的元素。

格式 : `...数组`

在解构操作和函数形参列表中...设置的都是取剩余值,在其他位置对数组使用是扩展运算符

1.数组拷贝

```js
var arr = [1,2,3]
var arr1 = [...arr1]
```

2.数组合并

```js
var arr0 = ['a', 'b'];
var arr1 = [1, 2, 3];
var arr2 = [...arr1];
var arr3 = [...arr0, ...arr1];
```

### Array.form()

功能：把其它伪数组的对象转成数组。

格式： `数组 = Array.from(伪数组对象)`

它的实参有三种情况：

1. 自定义的，类似数组格式的对象。

```js
let fakeArr = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
};
```

就是为了演示，并无实际应用价值。

2. arguments对象
3. DOM 操作返回的 NodeList 集合

   ###find和findIndex

作用：从数组中找出我们符合条件的第一个元素（或者是下标）。

格式

find和findIndex的格式是一致的。

```javascript
let result = [].find(function(item,index,self){ 
    //...
    // 如果满足查找的条件
    return true;
}); 返回值为找到的第一个元素,并且终止后面的循环
```

- 回调函数有三个参数，分别表示：数组元素的值、索引及整个数组
- 如果某次循环返回的是true，find和findIndex方法的返回值就是满足这个条件的第一个元素或索引 

区别 : find查找的元素,findIndex查找是下标

**findIndex** 找到数组中**第一个满足条件**的成员并**返回该成员的索引**，如果找不到返回 **-1**。

### includes()

功能：判断数组是否包含某个值，返回 true / false

格式：`数组.includes(参数1，参数2)`

- 参数1，必须，表示查找的内容
- 参数2，可选，表示开始查找的位置，0表示从第一个元素开始找。默认值是0。

```js
let arr = [1, 4, 3, 9];
console.log(arr.includes(4)); // true
console.log(arr.includes(4, 2)); // false， 从2的位置开始查，所以没有找到4
console.log(arr.includes(5)); // false
```

##set对象

Set是集合的意思。是ES6 中新增的内置对象，它类似于数组，但是`成员的值都是唯一的，即没有重复的值`。使用它可以方便地实现用它就可以实现数组去重的操作。

### 数组去重

```js
let arr = [1,1,2,3,3];
console.log([...new Set(arr)])
```

### set的成员用法

- `size`：属性，获取 `set` 中成员的个数，相当于数组中的 `length`
- `add(value)`：添加某个值，返回 Set 结构本身。
- `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `clear()`：清除所有成员，没有返回值。
- ` forEach`:遍历

## String的扩展

- 模板字符串
- includes 查找字符串内有没有某个元素
  - 格式：`str.includes(searchString, [position])`		
  - 功能：返回布尔值，表示是否找到了参数字符串
    - position: 从当前字符串的哪个索引位置开始搜寻子字符串，默认值为0。
- startsWith 查找字符串是不是某个开头
  - 格式：`str.startsWidth(searchString, [position])`         
  - 功能：返回布尔值，表示参数字符串是否在原字符串的头部或指定位置
    - position: 在 `str` 中搜索 `searchString` 的开始位置，默认值为 0，也就是真正的字符串开头处。
- endsWith 查找字符串是不是某个结尾
  - 格式：`str.endsWith(searchString, [len])`            
  - 功能：返回布尔值，表示参数字符串是否在原字符串的尾部或指定位置.
    - len:可选。作为 `str` 的长度。默认值为 `str.length`。
- repeat  重复字符串,返回新的字符串
  - `repeat`方法返回一个新字符串，表示将原字符串重复`n`次。
  - 语法:`str.repeat(n)`

