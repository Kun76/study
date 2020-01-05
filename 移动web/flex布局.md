# 01-flex-介绍

* 特点：相比于传统布局，代码少，布局快，对移动端兼容性好；
* 使用：移动端首选flex ,PC 不考虑兼容情况 可以flex；
* 面试：flex布局又叫**伸缩布局 、弹性布局** 、**伸缩盒布局** 、**弹性盒布局 ；**
* 思想：**使用思想上和传统盒子完全不同，不要再想子元素是块级元素、行内元素等**，
* 语法：加在父级  容器上面

```css
display:flex;
```



# 02-flex-容器属性-flex-direction

* 语法：作用确认主轴的方向，
* 特点：子元素会在主轴上进行排布，按照主轴选择正方向进行排布；
* 思想：
  * 不要再想这些元素是块级、行内等（不要用传统的知识解释今天的学习）；
  * 只要父亲设置flex布局，亲生子元素设置宽高就生效；

```css
      /* 1.主轴的选择 */
      /* row:选择水平向右为主轴 默认值*/
      /* 特点：子元素会在主轴上，按照主轴正方向进行排布 */
       flex-direction: row; 
      /* 主轴：行，从右到左 */
      flex-direction: row-reverse;
      
      /* 主轴：列，从上到下  */
      flex-direction: column; 
      /* 主轴：列，从下到上  */
      flex-direction: column-reverse;
```





# 03-flex-容器属性-justify-content

* 语法：**控制子元素在主轴上对齐方式；**
* 特点：如果要使用该属性，先要把主轴的去向决定；

```css
      /* 1.主轴的选择 */
      /* row:主轴：行，选择水平向右 */
      /* 语法:justify-content 控制子元素在主轴上  排布方式 */
      /* flex-start：默认值，从主轴头部开始  */
      /* flex-end: 从尾部开始对齐 */
      /* justify-content: center; 居中对齐 */
      /*  space-around:剩余空间环绕在子元素周围*/
      /* justify-content: space-between; 剩余空间分布在项目之间*/
```





# 04-flex-容器属性-flex-wrap和flex-flow 

* flex-wrap：控制子元素是否换行，默认不换行，子元素在主轴上进行合理压缩；

```css
/* 默认不换行  nowrap*/
/* 特点：如果子项目的加起来的总宽超过父亲的宽度，子项目的宽度会被合理的压缩 */
/* wrap：换行*/
flex-wrap: wrap;
```

* flex-flow：复合属性，确认主轴方向，控制是否换行。一般不用；

```css
flex-flow: row(主轴的方向) wrap(换行);
```







# 05-flex-容器属性-align-items-01-语法

* 语法：控制子元素（项目）整体在侧轴上对齐方式；

```css
      /* 主轴的选择 row，侧轴默认：Y轴*/
      /* !!! 控制 子元素 单行 在侧轴上对齐方式 */
		引入CSS align-items:
      /* flex-start : 侧轴的头部开始对齐 */
      /* flex-end : 侧轴的尾部开始对齐 */
      /* center : 侧轴上居中 */
      /* stretch：在侧轴方向上进行拉伸 */
      /* 规则：如果在侧轴方向上进行拉伸，CSS设置子元素不能在侧轴方向有大小的设置 ; 如果子元素在侧轴方向有大小的设置，拉伸不生效 */

```

# 05-flex-容器属性-align-items-03-其他

* 非应用补充知识：
  * 属性align-items：控制的是单行；
  * 设置换行：变为多行；多行下，每一行为单行，控制的是每个的侧轴上的对齐方式；
  * 活动范围：侧轴对齐方式，就以每个单行的范围进行对齐；

```css
.p {
      width: 800px;
      height: 600px;
      border: 1px solid #000;
      /* 父级：容器 */
      display: flex;
      /* 默认主轴：row */
      /* 默认不换行：元素会被合理的压缩 */
      flex-wrap: wrap;
      /* 侧轴对齐默认值；flex-start 侧轴头部*/
      /* 在侧轴方向 进行拉伸 ，特点：不能设置控制侧轴反向的CSS属性 */
      align-items: stretch;
    }
    
    .son {
      width: 100px;
      /* height: 100px; */
      background-color: #ccc;
    }
```







# 06-flex-容器属性-align-content

* 语法：控制子元素（多行）在侧轴上对齐方

```css
	 align-content: 控制多行对齐方式，把多行看成一个整体
     /* 侧轴上 单行 默认值：flex-start，看成每个单行 */
      /* align-content: flex-end;侧轴尾部对齐 */
      /* align-content: center; 居中*/
      /* space-around：剩余空间，环绕 */
      /* 把剩余空间分到两个行之间 */
      /* align-content: space-between; */
      /* 在侧轴上进行拉伸： */
      /* align-content: stretch; */
```





# 07-flex-容器属性-案例

![](D:/就业班学习资料/就业班第3天/001-资料/imgs/004.png)

* 整体：行排布，默认row;
* 局部：
  * 列排布；
  * 侧轴上居中对齐；

* 特点：整体的flex布局只是对整体下的子元素有flex布局的影响，单独子元素形成新的flex布局，需要重新写display:flex;





# 08-flex-项目属性-flex-使用

* 作用：flex划分主轴上剩余空间给子元素,可以直接通过属性flex设置主轴方向的上的宽度;
* 份数：常规使用，用整数；把所有的子元素的份数加起来N份，剩余空间分成N份，再看每个子元素占有几份；
* %：常规使用，比较保证加起来是100%；

* 场景：左右固定，中间随意拉伸；
* 圣杯布局的第三个

```css
    .p {
      width: 100%;
      height: 100px;
      border: 1px solid #000;
      display: flex;
      /* row */
    }
    
    .left {
      width: 100px;
      height: 100px;
      background-color: #ccc;
    }
    
    .mid {
      /* 把主轴上剩余空间 全部占有 吃掉 */
      flex: 1;
      height: 80px;
      background-color: pink;
    }
    
    .right {
      width: 100px;
      height: 100px;
      background-color: #ccc;
    }
```



# 10-flex-项目属性-align-self

* 语法：控制单个项目（子元素）在侧轴上自己的对齐方式；

<img src="D:/就业班学习资料/就业班第3天/001-资料/images/003.png">

* 默认值为auto的特别之处：
  * 默认值auto，如果父级设置了align-items ，auto继承父级元素上设置侧轴的对齐方式：align-items 属性；
  * 如果父级没有设置align-items 属性，auto默认值会变为strecth；（注意侧轴方向上控制元素大小的CSS设置要注释掉）

* 子元素设置flex:1

  ​	  flex: 1;主轴:把剩余空间均分掉
  ​      align-self的默认值是auto;
  ​      那么auto变为 拉伸 stretch,不用设置宽高来达到占满父元素

* margin规则：

  ​	  如何盒子显示有宽高，但是不是通过width、height设置来的，而是通过其他，那么你设置margin，向内挤



# 11-flex-项目属性-order-了解

* 语法：控制子元素的排布顺序，值越小，越前。可以取负值；



# 13-flex-顶部搜索区-03-固定定位

```css
  /* 固定定位 脱标，宽度失效*/
  position: fixed;
  /* 手动加100% */
  width: 100%;
  min-width: 320px;
  max-width: 540px;
```





# flex-总结

* flex：

  * 代码速度快，移动端首先；
  * 对PC兼容不好；
  * 面试：lex布局又叫**伸缩布局 、弹性布局** 、**伸缩盒布局** 、**弹性盒布局 ；**

* 思想：

  * 决定使用flex布局：看待从行和列角度看待这些布局；
  * 一定不要在想这些元素是块级、行内等；

* 核心属性：

  * 确认主轴：flex-direction元素会按照主轴方向进行排布；
  * 项目：
    * 设置flex:1；在**主轴方向**划分剩余空间；
    * **隐藏：侧轴方向会拉伸；如果父级不设置align-items ，子项中 align-self:auto 默认值 auto 会沿着侧轴方向进行拉伸**
  * 侧轴对齐方式：align-items：center;

  

  


