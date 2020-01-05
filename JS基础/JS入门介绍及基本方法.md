#1.js介绍及引入方式

* JavaSript是一种轻量级脚本语言,作用是与用户交流互动;交互;
* 引入方式:
  * 内嵌式: demo学习阶段;
  * 外链式: 工作使用;
  * 行内样式: 一般不用;
* 注释:
  * 多行shift+ctrl+/	可以换行,写很多注释,函数,功能介绍; /* */
  * 单行ctrl+/ 一般用作简单说明 //

# 2.js输入输出

* 输入信息:	
  * prompt(" ");引导信息,可用单引号,双引号的一种
* 输出信息:
  * alert("这个是弹窗"); 警告,不点击的话后面的代码不会执行;
* 调试用:
  * console 控制台;
  * log  日志;
  * console.log("这是控制台的输出") 一般用于调试用, 多个输出：语法 用 , 隔开，注意只是console.log()
* 在页面文档中输出:
  * document.write(); 特点: 可用识别html代码,独有的特点;  
    * document.write("<h1>这是哪里？92期前端</h1>");

# 3.变量 语法及储存

 * 语法: 定义变量,存储数据

 * 关键字var 定义一个变量名

 * 变量可以多次声明和赋值

    * var a = 1,b = 2;
    * var a = 1 ; var b = a;

   ```js
   //1.语法:定义变量 ，存数据；
   var = a;
   //定义一个变量a  =：赋值运算：规则：把右侧的值10，存入左侧的变量里；
     // 特点：给a变量存入10，a前面不需要再var;
   a = 10 ;
   
   
   //2.声明（定义）变量时候，同时进行赋值；
   var a = 10;
   var b = 20;
   console.log(a+b)
   
   var info = prompt("输入");
   //1.界面有弹窗
   //2.用户输入信息给info赋值
   console.log(info);
   //3.控制台输出用户信息
   
   
   
   //3.可以再次赋值
   var a = 10;
   console.log(a) //此时输出为10
   var a = 20;
   console.log(a) //此时输出为20
   
   
   
   //4.运算结果，赋值给自己；
    var a = 10;
   //声明一个变量a,并给a赋值10
    a = a + 1;
   //右侧运算后再次给a赋值 11
   console.log(a)
   
   
   //5.变量可以交换两个值
   // 需求：换两个变量的值；
     var a = 10;
     var b = 20;
     console.log(a, b);
   
   
     // 准备空变量；
     var c;
     // 把a背后值 10 赋值了一份给了 c  10;
     c = a;
     // 把b背后的值 20 赋值了左侧变量a:20;
     a = b;
     // 把 c背后的值10 赋值给了b;
     b = c;
   
     console.log(a, b);
   ```

   

   ###变量命名

* 范围：字母、数字、下划线、$符号；

  * 1.不能数字开头 ,如果用数字名字 会报错：Uncaught SyntaxError: Invalid or unexpected token 
  * 2.不能使用关键字和保留字；

* 市场上，一般用驼峰命名；驼峰命名习惯：方便我们开发人员观测我们自己写代码

* 根据公司的需要：变量用英文；



# 4.js五种简单类型

* 语法：源于生活；

  ```js
    1.number 类型
  //也叫数字类型,就是数字  
    var a = 10.1;
    var b = NaN;
    // NaN：不是某个数，泛指，不确定；
    console.log(a, b);
  
  
     2.String 类型：字符串
    // 规则：只要两边有单双引号的一种，这个就是String 类型，就是个字符串；
     var a = "10.1";
     console.log(a);
  
    // 输出一段文字
    	var info = "  我说："今天天气不错 "  ";
    // console.log(info);
    // 解决问题：引号成对出现，成对寻找；
    // 1.单双引号配合：
    // var info = '  我说："今天天气不错 "  ';
    // 2.如果一句话里，既有" ' 外面用哪个包都不合适；解决：
    // 转译字符\：特殊的字符；" ' \
    // 为什么？字符串里面出现 " ' ,可能会干扰到我JS字符串 外面 " '（语法）
    // 转一下：JS认为这些特殊的字符，是正常的字符；
    // 语法:  \"，特点必须写在字符串内；
    // var info = '我说：\"xxxxxxx\" ;  他说:\'xxxxxxxxx1\';     她说：\\  ';
    
    
  
  
    // 3.boolean 类型：(布尔类型);
    // 作用：描述一件事 对或者错？存在？确定？
     var a = true;
     var b = false;
  
  
    // 4.Null值; 了解
     var a = null;
    // console.log(null);
  
  
    // 5.Undefined类型；
    	 var a;
    // JS 会给默认值；undefined
    // console.log(a);
  ```

  

# 5.数据-检查类型

* 用于js查看数据类型,可以用变量也可以直接是数据;

* 语法: typeof(a);

  ```js
  // 两种用法：可以不用(),typeof后面加空格
    // var aType = typeof(a);
    var aType = typeof a;
    console.log(a, aType);
  ```

  

# 6.js转换-转数字

```js
//1.Number方法
	//string类型转number类型
	var a = "1000";
	a = Number(a);
	console.log (a) 
	//如果转换类型里有非数字,那么就会转成NaN undefined Null 也是一样
	//boolean转成number
	true:1;  false:0;
//2.parseInt方法 
	//只会保留整数 ,返回整数部分;转成功的话,只能传入有数字部分在前面的字符串, 如果传入其他类型则是NaN;
	var b = "10.1abc";
  	b = parseInt(b);
  	console.log(b); //b→10;
//3.parseFloat方法
	//返回值会保留小数部分;转成功的话,只能传入有数字部分在前面的字符串,传入其他类型也是NaN;
	var a = "10.1adc";
	a = parseFloat(a);
	console.log(a); //a→10.1;
```

# 7.js-转换-转字符串

```js
1.String()
  // 传入：数据或者变量；
  // 返回：转换后数据，字符串；


  // Number ----->String
  // var a = NaN;
  // 先把右侧进行加工，把右侧的结果赋值给左侧；
  // NaN----> "NaN";
  // a = String(a);
  // console.log(a, typeof a);



  // Boolean ----->String
  // var b = true;
  // b = String(b);
  // console.log(b, typeof b);


  // null undefined;
  // 声明一个变量后，不进行赋值，JS给c 赋值为默认 undefined
  // var c;
  // c = String(c);
  // console.log(c, typeof c);



  2.toString();
  // 格式：注意有个.
  // 注意：undefined和null不能使用这个方式变成字符串；
  // 用法：数据或者变量.toString()
  // 返回：字符串；


  // Number
  // var a = 10;
  // a = a.toString();
  // console.log(a);


  // Boolean
  // var a = true;
  // a = a.toString();
  // console.log(a);


  // Null
  // var a = null;
  // // 报错： Uncaught TypeError: Cannot read property 'toString' of null
  // a = a.toString();
  // console.log(a);
```

#8.js-转化-转Boolean

```js
// Boolean()
  // 参数：传入数据或者变量；
  // 返回：true /false;
  // true:  确定，对，成立；
  // false: 不确定，错误，不存在；


  String 
  字符串所有内容转换为true,空格是也为true 空标签为false;
  // var a = "abc";
  // var a = "0";
  // var a = " ";
  // var a = ""; // 特别：空字符串
  // a = Boolean(a);
  // console.log(a);




  Number
  NaN,0时为false 数字为true;
  // var a = 10;
  // var a = -10;
	
  // 特别：NaN 我也不知道指哪个数；
  // var a = NaN;
  // var a = 0;
  // a = Boolean(a);
  // console.log(a);


 Null undefined；
  // 转化后都是false;
```

* 转化为false的几种情况:

```js
var res1 = Boolean(0);

var res3 = Boolean(NaN);

var res2 = Boolean('');  // 空字符，里面啥没有，空格也没有；

var res6 = Boolean(false);


var res5 = Boolean(null);  
console.log(res5); //输出false

var res4 = Boolean(undefined); 
console.log(res4); //输出false
```

# 9-js-操作符-算术操作符-01

* 语法：常规需要Number类型
* 重点：**字符串遇见+；**
* 算术运算符：
  * **规则：字符串遇见+：左右两边数据类型转换为是string类型，形成字符串拼接；**
  * **++ 规则：**（面试）
    * 如果遇见++写在a的后面，先把自己的数推到位置上，在进行a++运算（运算后结果不推到位置上）
    * 如果遇见++写在a前面，++a先算，计算后的结果，再往位置上提供数据；

````js
  // + - * / %



  // 常规：Number类型 进行 算术运算；
  // var a = 10 % 3;
  // 场景：奇偶数判断；
  // var a = 50 % 2;
  // console.log(a);



  // ----------------------------------特别：字符串 遇见 + ;
  // 规则：字符串 遇见 +，使临近字符串的其他数据类型，转化为字符串，形成字符串拼接；
  // 隐式转化：看不见的转化过程；
  // var a = 10 + "abc" + 10;
  // // 10 + "abc" + 10;
  // // "10"+"abc"+"10";
  // console.log(a);



  // ----------------------------------特别：字符串 遇见 - * / %
  // 规则：字符串会隐式转化Number类型；
  // var a = "abc" - 1;
  // "abc" - 1;
  // NaN-1
  // console.log(a);
````

# 10-js-操作符-算术操作符-02

```js
// ++  --：
  // 作用：在自己的基础上进行自增1，或自减1；
  // 演示：
  // var a = 10;
  // 一元运算：
  // a++;
  // ++a;
  // console.log(a);


  // 参加其他运算的时候：
  // var a = 10;
  // var b = a++ + 1;
  // 先算右侧：
  // + 会使两个位置上提供一个数据参加运算；
  // 规则：如果遇见++写在a的后面，先把自己的数推到位置上，在进行a++运算（运算后结果不推到位置上）
  // 10(此时a变为11)   +  1;
  // console.log(b, a);



  // var a = 10;
  // var b = ++a + 1;
  // 先算右侧：
  // + 会使两个位置上提供一个数据参加运算；
  // 规则：如果遇见++写在a前面，++a先算，计算后的结果，再往位置上提供数据；
  // 11 + 1
  // console.log(b, a);



  var a = 10;
  // 先算右侧：每个位置上绝对有个数据参加运算；
  // 10(++写在a后面，先把自己数据推到位置，在进行自己计算a:11，计算后的结果和这个位置上10没有关系) + 12（++a：先运算，看此时a:11,计算完后 a:12,把计算结果推到位置上）
  var b = a++ + ++a;
  console.log(b);
```





# 11-js-操作符-比较运算符

* 常规操作：左右需要Number类型；

```js
// 比较运算符：> < >= <=


  // 常规：两边都是需要Number类型；
  // = ：优先级最低；
  // 返回：Boolean;
  // >= 满足一个就可以；
  // var a = 4 >= 4;
  // console.log(a);



  // -----------------------------------
  // 非常规操作 规则：非Number类型要和Number类型比较的话，非Number类型就要隐式转换为Number类型；（了解）
  // var a = "abc" > 10;
  // // NaN > 10;
  // console.log(a);


  // 重点：
  // == ===
  // 作用：判断两边的数据是否相等；


  // == 规则：
  // 1.如果两边的数据类型不同，非Number类型转Number类型，看值是否相同；
  // 2.如果两边的数据类型相同，看值是否相等；

  // var a = "abc" == "abc";
  // var a = true == 1;
  // 特别：
  // var a = NaN == NaN;
  // console.log(a);



  // === 规则：(严格，没有隐式转化)
  // 1.直接先看数据类型，如果数据类型不同，直接返回false;
  // 2.如果两边的数据类型相同，看值是否相等；
  // var a = true === 1;
  // console.log(a);

```







# 12-js-操作符-逻辑运算符

* 语法：

```js
  // && 
  // 且：全部满足，如果有一个条件不满足，直接返回不满足的结果;
  // 执行过程：&& 前后 每个位置上需要一个数据 （Boolean类型）只进行比较，不返回；
  // 返回： 最后成立的结果 或 不成立的结果；

  // var a = true && true;
  // console.log(a);


  // var a = 1 && 2;
  // 位置上需要一个Boolean类型（隐式转化）的数据只是进行比较，不进行返回；
  // true && true; 只是进行比较
  // 返回：最后成立的结果 或 不成立的结果；
  // console.log(a);


  // var a = 1 && 0 && 2 && 3;
  // 只是进行比较：true && false ，不进行返回；
  // console.log(a);



  // ----------------------------------------------------
  // || 或：只要满足一个条件就可以；
  // 返回：满足条件的结果；

  // var a = 1 || 0;
  // 只是进行比较：true || false;不进行返回；
  // console.log(a);



  // var a = "abc" || 1;
  // 只是进行比较：true || true;不进行返回；
  // 返回最后 是true 结果1；



  // --------------------------------------------------
  // !:取反；
  var a = true;
  // 进行取反的运算；
  var b = !a;
  console.log(a, b);
```



# 13-js-操作符-赋值运算符

* 语法：

```js
// = 的作用就是把右边的结果赋值(存储)给左边的变量之类的容器
var a = 10;

// 相当于是 a = a + 2; 就是一个简写的语法
// -=   *=   /=   %= 除了运算规则不同，作用是一样的，都是简写
a += 2; 
console.log(a); // 输出12
```





# 14-js-操作符-优先级

```js
// 1. 第一优先级：()
  // 2. 第二优先级：++ -- !
  // 3. 第三优先级： * /  %
  // 4. 第四优先级： + -
  // 5. 第五优先级： > >= < <=
  // 6. 第六优先级： == != === !==
  // 7. 第七优先级： &&
  // 8. 第八优先级： ||
  // 9. 第九优先级： = += -= *= /= %=  



  var a = 200 === (10 + 10) * 10 > 1;

  // 200 === 20 * 10 > 1
  // 200 === 200 > 1
  // 200 === true;
  // false

  console.log(a);
```



# 总结

* 输入：prompt("引导信息")，接受数据类型：string类型；

* 输出：【用熟了】

  * 调试
  * console.log("信息 1","信息 2") ；

* 变量：

  * 储存数据。
  * 数据类型：【string 、Number 、Boolean】、null、undefined;
  * 查看类型：typeof a; typeof(a);
  * 数据类型相互转换：记忆；

* 运算符：

  * 算术运算符：
    * **规则：字符串遇见+：左右两边数据类型转换为是string类型，形成字符串拼接；**
    * **++ 规则：**（面试）
      * 如果遇见++写在a的后面，先把自己的数推到位置上，在进行a++运算（运算后结果不推到位置上）
      * 如果遇见++写在a前面，++a先算，计算后的结果，再往位置上提供数据；
  * 比较运算符：
    * **==规则：**
      * 数量类型不同的时候，不同的类型要隐式转化为Number类型，再去比值；
      * 数量类型相同的时候，直接比值的是否相等；
    * **===规则：如果数据类型不同，直接返回false;**
  * 逻辑：
    * && ||：
      * 比较过程：只是进行比较，不进行返回；
      * 全部满足，如果有一个不满足的话，返回不满足位置上的数据；
  * !：Boolean值取反；
    * !a；（a此时为变量）

  

-







