# 01-rem布局-媒体查询-01-语法

* 媒体查询：感受屏幕变化，

* 语法：当我们设置好某个设置，接下来等待屏幕区进入我的范围，进入就生效；

  ```css
  @media mediatype and|not|only (media feature) {
      CSS-Code;
  }
  ```

  

```css
    /* 作用：感受 屏幕 变化 */
    /* @media 定义媒体查询的关键字  */
     mediatype： screen 屏幕 print 打印机 all 所有设备
     and: 且  not 非  noly 只有
    /*  () 语法：不能省略*/
    /* media feature：条件 宽度 */
    /* 当 感受屏幕 当屏幕宽度是500px */
    /*下面设置的 CSS-Code; 会生效  */
    /* @media screen and (width:500px) {
      body {
        background-color: red;
      }
    } */
    /* min-width:最小值；从500px开始  >=500px  */
    /* @media screen and (min-width:500px) {
      body {
        background-color: red;
      }
    } */
    /* max-width：最大致，终点是500px w <=500px*/
    
    @media screen and (max-width:500px) {
      body {
        background-color: red;
      }
    }
```

* 特点：
  * 本身的语法格式；（width:??px）没有分号；
  * min-/max 包含等于=；



# 02-rem布局-媒体查询-02-档位划分

* 档位：划分的不同范围

```css
  /* - 档位1：w<540px w<=539px; */
    /*     
    @media screen and (max-width:539px) {
      body {
        background-color: green;
      }
    } */
    /* - 档位2 : 540px<=w and w< 640px; */
    /* @media screen and (min-width:540px) and (max-width:639px) {
      body {
        background-color: blue;
      }
    } */
    /* - 档位3 : 640px<=w */
    /* @media screen and (min-width:640px) {
      body {
        background-color: pink;
      }
    } */
    /* -------------------------------------------优化写法 */
    
    @media screen and (min-width:0px) {
      body {
        background-color: green;
      }
    }
    
    @media screen and (min-width:540px) {
      body {
        background-color: blue;
      }
    }
    
    @media screen and (min-width:640px) {
      body {
        background-color: pink;
      }
    }
```

* 特点：为什么要从min-wdith，代码抒写方便，从小到大；

* 了解：资源 引入

```html
<!-- 320px~640px -->
<link rel="stylesheet" href="style320.css" media="screen and (min-width: 320px)">
<!-- n>=640px -->
<link rel="stylesheet" href="style640.css" media="screen and (min-width: 640px)">
```





# 03-rem布局-rem单位

- rem：等比目的：第二个知识，根基；
- 1rem：HTML font-size设置大小??px；
- 1rem =  ?? px;  
- 只要用到rem单位，不是直接计算大小，10rem = 10*??px 计算出具体的多少的px的值；
- 只要根基1rem背后代表的具体的值会变的话，页面上所有用到rem的单位的属性都会跟着变，等比变化；



# 04-rem布局-rem&媒体查询

* 语法：

```css
/* 媒体查询：感受屏幕的变化，进入我的设置范围，下面的设置就会生效 */
    /* 下面CSS设置就会生效：设置1rem背后代表值； */
    /* 页面中所有的元素都使用px 转化为 rem单位，等比效果 */
    
    @media screen and (min-width:0px) {
      /* 设置1rem=20px */
      html {
        font-size: 20px;
      }
    }
    
    @media screen and (min-width:540px) {
      /* 设置1rem=30px */
      html {
        font-size: 30px;
      }
    }
    
    @media screen and (min-width:640px) {
      /* 设置1rem=40px */
      html {
        font-size: 40px;
      }
    }
    
    div {
      width: 10rem;
      height: 10rem;
      background-color: #ccc;
    }
```

* 等比原理：
  * 媒体查询：感受屏幕变化；
  * 变化下设置什么：根基rem ，设置不同档位下1rem 背后代表的 px值；
  * 实现等比变化效果；
* **需要真实面对三个问题：**
  * 1.各个档位下的字体大小（根基 1rem背后代表的值）如何确定？
  * 2.拿到一个设计稿应该还需要注意什么问题？
  * 3.具体如何开发？如何写代码



# 05-rem布局-问题解决-01-如何确定rem

```css
/* 约定1： 把UI面对设计尺寸，从小到大排列，把设计宽度当做每个档位的起始点，开始点，最小值 */
/* 约定2:  1rem 怎么确定的？用当前每个档位的起始点的值 / 公共的数 */
/* 公共数：10 */

/* 320 */
    
    @media screen and (min-width: 320px) {
      html {
        font-size: 32.0px;
      }
    }
    /* 360 */
    
    @media screen and (min-width: 360px) {
      html {
        font-size: 36.0px;
      }
    }
    /* 375 iphone 678 */
    
    @media screen and (min-width: 375px) {
      html {
        font-size: 37.5px;
      }
    }
    /* 384 */
    
    @media screen and (min-width: 384px) {
      html {
        font-size: 38.4px;
      }
    }
    /* 400 */
    
    @media screen and (min-width: 400px) {
      html {
        font-size: 40.0px;
      }
    }
    /* 414 */
    
    @media screen and (min-width: 414px) {
      html {
        font-size: 41.4px;
      }
    }
    /* 424 */
    
    @media screen and (min-width: 424px) {
      html {
        font-size: 42.4px;
      }
    }
    /* 480 */
    
    @media screen and (min-width: 480px) {
      html {
        font-size: 48.0px;
      }
    }
    /* 540 */
    
    @media screen and (min-width: 540px) {
      html {
        font-size: 54.0px;
      }
    }
    /* 720 */
    
    @media screen and (min-width: 720px) {
      html {
        font-size: 72.0px;
      }
    }
    /* 750 */
    
    @media screen and (min-width: 750px) {
      html {
        font-size: 75.0px;
      }
    }
```

# 05-rem布局-问题解决-02-UI给的是什么

* 设计稿：上面标注的是PX单位；
* 目标：等比变化；
* UI：只给一份设计稿，一般UI给咱们（前端工程师）的是等比变化最大的设计稿；



# 05-rem布局-问题解决-03-01-开发该怎么做

* 档位和根基设置按照约定设置好了；第一个问题解决 √ 
* UI给的图的已经拿到了，上面标注的是px单位；
* 目标：把px单位换算 成rem单位；
* 如何做：选择当前设计稿在档位下的rem背后代表的值进行计算；
  * 如果UI给540px 设计稿？如何选择1rem 背后代表的值？
    * 540px设计稿必定要在众多等比变化的效果中显示的一个；
    * 必定在某个媒体查询的档位下进行显示；当前这个档位下的rem 背后代表的值就是我们需要的值；
    * 1rem = 54px;
    * 如果给你的540px宽度设计稿，540px = 540 / 54 = 10rem；
    * 页面中其他元素：44 px  =  44 / 54  = 0.8148148148148148 ‬rem;
* 大问题：计算问题！恶心！

# 05-rem布局-问题解决-03-02-计算引出的LESS

* less：Css预处理语言；一直用less;

```css
// 1.less:CSS预处理语言，不能直接用，转后的CSS文件
// 2.可以在less 正常写CSS 代码；
// 3.如果要修改CSS里面的代码，less这里，点保存；（自动保存）

// --------------------------------------1.正常写
body {
  width: 100%;
  min-width: 320px;
}

div {
  background-color: #ccc;
}


// --------------------------------------2.变量
@bg:#ccc;
header {
  background-color: @bg;
}

.banner {
  color: @bg;
}

span {
  border:10px solid @bg;
}
// ---------------------------------------3.嵌套 HTML结构
header {
  height: 44px;
  border-bottom: 1px solid #ccc;
  display: flex;
  .left {
    width: 40px;
    background-color: pink;
  }
  .mid {
    flex:1
  }
  .right {
    width: 40px;
    background-color: #222;
  }
  div {
    background-color: #ccc;
    // &：代表当前这个CSS的自己 div
    &:hover {
      background-color: #333;
    }
    &::before {
      content:"";
    }
  }
  // div:hover {
  //   background-color: #222;
  // }
}



// ---------------------------------------4.计算
div {
  width: 60px+20px;
  height: 60px/20px;
  height: 60px*20px;
  height: (60-38)/2*20px;
  // 单位如何使？单位不参与运算，选择一个单位；
  // 两个不同的单位，选前面那个单位
  height: 44px/54rem;
  height: 44/54rem;
}

```





# 06-rem布局-方案1-步骤

- 1.准备档位和档位下的rem；按照约定，各个档位下rem准备好了；
  - 公共的CSS文件；common.css
  - 引入index.html页面；
- 2.拿到UI设计稿，**原稿实现**；页面上所有的元素，在设计稿上进行测量，代码实现；（流式、flex）只要是UI给图上有标注，就是写出来；先全部实现出来，一会统一替换；
  - 在哪里写？.less文件；(里面的所有的元素都是px);
  - 需要把生成的css文件进行引入index.html页面；
- 3.找到当前设计稿 对应某个档位下的 1rem 背后代表的值；1rem=??px;
- 4.统一替换：自己下less文件里的px单位：44/75rem;



# 07-rem布局-方案1-构建文件目录及初始化

```html
  <!-- 初始化base -->
  <link rel="stylesheet" href="./css/normalize.css">

  <!-- commom.css 公共的啥？各个档位下rem  -->
  <link rel="stylesheet" href="./css/commom.css">

  <!-- 生成的CSS文件，看less文件 -->
  <link rel="stylesheet" href="./css/index.css">
```

* 步骤：
  1. 准备档位和档位下的rem；公共的CSS文件；引入index.html页面；
  2. 新建index.less文件，在里面写我们的代码，把生成的index.css文件引入；
  3. 引入normalize.css，注意三个的先后关系；

* index.less：初始化(原稿实现)

```css
body {
  width: 750px;
  margin: 0 auto;
}
```





# 08-rem布局-方案1-01-顶部栏

* 步骤：
  * 1.形成约定的档位划分的文件commom.css 文件，要引入在index.css在前面；
  * 2.拿到UI设计稿，**原稿实现**；为了方便统一替换；

![1571890653025](D:/就业班学习资料/就业班第4天/001-资料/imgs/1571890653025.png)

* 顶部栏：
  * 圣杯布局第三个方案
  * 固定定位：

```less
// -------------------------搜索区
header {
  width: 750px;
  height: 88px;
  display: flex;
  background-color: #FFC001;
  position: fixed;
  .left {
    width: 40px;
    height: 70px;
    margin: (88-70)/2px 15px;
    // background-color: #ccc;
    background: url('../imgs/classify.png') no-repeat;
    background-size: 100%;
  }
  .mid {
    flex: 1;
    background-color: #fff;
    margin: 10px 0;
    border-radius: (88-10*2)/2px;
    line-height: 88-10*2px;
    padding-left: 50px;
  }
  .right {
    width: 88px;
    height: 88px;
    line-height: 88px;
    text-align: center;
    color: #fff;
    
  }
}

```



# 09-rem布局-方案1-rem替换及小结

* 方案1：媒体查询+rem+less

```less
// 第三步：找到当前设计稿对应档位下的1rem 背后代表的值；1rem=??px;
// 看设计稿宽度 750px ,去哪找common.css
// 1rem = 75px;

// 第四步：只替换index.less 文件里的PX---->rem;

```

* 小问题解决：0-320档位没有设置：

```css
/* 0-320这个档位没有设置，1rem背后代表的值 HTML font-size 16px */
@media screen and (min-width: 0px) {
  html {
    font-size: 32.0px;
  }
}

```

* 注意：less保存完，没有要效果，less某些地方写错了，一般是{}缺少，删除后重新生成文件；
* 问题：
  * 虽然说按照UI的面对尺寸进行划分档位，但是对于我们前端工程师 划分的不够细致；
  * 档位划分不够细致，看起来变化不是连续的；
  * **方案不推荐使用；**

<img src="D:/就业班学习资料/就业班第4天/001-资料/images/010.png">





# 10-rem布局-方案2-rem&flexible.js-步骤

* flexible.js：
  * 帮助我们实现，连续变化，帮我们实现了430个档位；
  * 0-正无穷大 每个档位都给我了；（需要的是：320-750）
* 步骤：
  * 1.flexible.js去实现连续变化，也是上面的430个档位；引入页面内；不需要common.css
  * 2.拿到UI设计稿，**原稿实现**；页面上所有的元素，在设计稿上进行测量，代码实现；（流式、flex）只要是UI给图上有标注，就是写出来；先全部实现出来，一会统一替换；
    - 在哪里写？less文件
    - 需要把生成的css文件进行引入index.html；
  * 3.查询：设计稿宽度 /10 ：1rem=??px；
  * 4.统一替换：44/75rem;



# 11-rem布局-方案2-介绍及操作-rem替换小结

```less
// 第二步：原稿实现，UI设计稿上面标注为多大，我们就CSS设置为多少，没有标注，自己测量
// 第三步：找当前档位的1rem 背后代表 值 750px： 1rem = 75px
// 第四步：统一的替换


// 问题1：js文件 从0-∞  每个档位都给我了。我需要320~750之间；
// <320    >750 范围不要：CSS媒体查询控制
@media screen and (max-width:319px) {
  html {
    font-size: 32px!important;
  }
}

@media screen and (min-width:750px) {
  html {
    font-size: 75px!important;
  }
}
// 问题1.1：为啥我们的设置不生效？注意权重问题；JS控制样式在行内；CSS设置的是在文件；
// 问题2：为啥body内的字体不跟着变化？body的行内样式有JS的设置，权重比CSS文件高，需要提权；

body {
  width: 750/75rem;
  margin: 0 auto;
  font-size: 24/75rem!important;
}

```

* 方案：连续变化，这个工作上要做等比变化推荐方案；

<img src="D:/就业班学习资料/就业班第4天/001-资料/images/011.png">





# 12-总结

* 基础班：传统布局PC端页面；
  * 定位、浮动、清除浮动、margin、padding；
    * 浮动：清除浮动；严谨：width:100%;overflow:hidden;
    * 定位：
      * 脱标，盒子默认的100%就会失效；【！】
      * 不设置top/left，在原位置上脱标；【！】
    * 盒子模型：CSS3盒子模型--->圣杯布局三个方案‘；
    * margin：
      * 塌陷；
      * 如果盒子不是通过自己的width/height设置宽高，但是显示有宽高，使用margin会向内挤；【！】
  * PC端，UI给就是宽高，把宽高设置为固定PX，
* 移动端：
  * 高度：思路上，好多盒子高度不是写死，基本上靠内部的元素盒子撑起来；
  * 宽度：
    * 流式布局：传统布局；把宽度设置为百分数，浮动（清除浮动）
    * flex：新的布局方案【更推荐flex】
      * 思想上，不要再想这些元素是块级元素等；
      * 想这些是行排列或者列排列；
      * flex:1  20%  主轴剩余空间的划分；
    * rem：等比变化；rem+flexible.js+less：连续变化特点；【推荐等比变化】
      * 实现布局？推荐大家flex布局；
      * rem+flexible.js+less：步骤：
* 到了公司：
  * 移动端，flex布局；
  * PC项目：基础班，传统布局，或者不考虑兼容性flex布局；



* 以后写所有的CSS文件，都用less;
* rem作业：
  * 携程  less  
  * 京东

































