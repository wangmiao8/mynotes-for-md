## 选择器优先级

W3C 文档给出选择器权重的计算过程，将选择器的分为 A、B、C 三个等级。



A：表示选择器中 **ID 选择器**出现的次数

B：表示 **class 选择器**、**属性选择器**和**伪类**出现的次数

C：表示**标签选择器**和**伪元素**出现的次数



最终经过等级加权计算就能得出选择器的优先级。

列一个W3C给出的例子：

```css
*               /* a=0 b=0 c=0 */
LI              /* a=0 b=0 c=1 */
UL LI           /* a=0 b=0 c=2 */
UL OL+LI        /* a=0 b=0 c=3 */
H1 + *[REL=up]  /* a=0 b=1 c=1 */
UL OL LI.red    /* a=0 b=1 c=3 */
LI.red.level    /* a=0 b=2 c=1 */ 
#x34y           /* a=1 b=0 c=0 */
#s12:not(FOO)   /* a=1 b=0 c=1 */
.foo :is(.bar, #baz)
                /* a=1 b=1 c=0 */
```



总的来说样式的优先级是这样的：

**!importent > 行内样式 > 等级计算 > * 选择器**



## Position

position 的五个属性：static（静态定位）、relative（相对定位）、absolute（绝对定位）、fixed（固定定位）、sticky（粘性定位）



**定位原则：**

- relative 相对自身定位；

- absolute 相对非 static 的祖先元素定位；

- sticky 相对最近的滚动父元素和最近的块级父元素定位；

- fixed 相对视口（viewport）定位；



**属性的特性：**

- absolute 和 fixed，设置后会脱离正常的文档流，并且创建 BFC；

- 在 sticky 的父元素上设置 overflow 为 visible 以外的值会导致 sticky 无效；



**fixed 和 sticky 的不同：**

- 设置 position 为 fixed 后会脱离正常文档流，而 sticky 不会；
- sticky 相对滚动父元素定位，受父元素约束，而 fixed 已脱离文档流，相对视口定位；



## 盒模型

所有的 HTML 元素都能看做成一个盒子，盒子由四个部分组成：

1. content，盒子的内容
2. padding，内容与盒子边框的间距
3. boder，盒子的边框
4. margin，盒子与盒子之间的间距

由于浏览器历史原因，盒模型的计算规则存在差异，盒模型有分为 IE 盒模型和标准盒模型，在 IE6 以前使用的是 IE 盒模型，而我们现在默认使用的是标准盒模型，当然我们也可以手动修改盒模型。



**标准盒模型和 IE 盒模型：**

标准盒模型和 IE 模型的区别在于盒模型的宽高的计算方式。

IE 盒模型：width = content + padding + border

标准盒模型：width = content



**修改盒模型的宽高计算方式**：

我们可以在 CSS 中使用 `box-sizing` 属性使用手动修改盒子宽高的计算方式。

```css
.content-box {
  box-sizing: content-box; /* 默认标准盒模型 */
}

.border-box {
  box-sizing: border-box; /* 使用 IE 盒模型 */
}
```



**如何使用 JavaScript 获取盒模型宽高：**

获取如下元素的宽高：

```html
<div id="box"></div>
```

元素的样式：

```css
.box {
  width: 100px;
  height: 100px;
}
```

JavaScript 获取元素宽高方式和优缺点：

1. `ele.style.width/height`：只能获取到行内样式设置的宽高，不能获取到 link 或者 style 内设置的宽高。

2. `ele.currentStyle.width/height`：能获取到渲染后的宽高，但只有 IE 上支持。

3. `window.getComputeSytle(ele).width/height`：能获取到渲染后的宽高，大多数浏览器支持，IE9 以上支持。
4. `ele.getBoundingClientRect().width/height`：效果和兼容性和第三种一样，同时该 API 还支持获取元素相对视窗的上下距离。



## BFC

MDN 文档给出的定义：BFC 是 Web 页面 CSS 渲染区域，是块盒子的布局过程发生区域，也是浮动元素交互区域。

简单来说就是：**这个区域内使脱离标准文档流的元素能正确布局和交互**。



BFC 的作用：**建立一个隔离区域，断绝区域内外的元素的作用。**

BFC 解决的问题：**浮动的影响（高度坍塌、元素重叠）、外边距重合。**



**创建 BFC 的方法：**

- **html**；
- **float** 不为 none；
- **position** 值为 absolute 或 fixed；
- **display** 值为 inline-block、flex、table-cell、table-caption；
- **overflow** 不为 visiable；



**BFC 渲染规则：**

- BFC 容器内块级元素从上到下排列；

- 同一个 BFC 容器内相邻的元素间的 margin 会发生重叠；
- 每个元素的 margin box 的左边，与容器的 border box 的左边相接触；
- BFC 为页面上独立的容器，内部元素不会影响到外部元素，反之亦然；
- 计算 BFC 容器高度时，包含内部所有元素的高度，浮动元素也包括；
- 浮动元素不会叠加到 BFC 容器上；



## 清除浮动

浮动（float）会导致元素脱离标准文档流，可能引起父元素高度塌陷，影响兄弟元素的排版。

![浮动效果](https://user-gold-cdn.xitu.io/2017/10/18/2af797ffc0918352ac8d381994ff1a27?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



清除浮动的大致分为以下三种方法：



第一种：在浮动元素后创建新的块级元素添加 `clear:both` 或者 `clear:left`。

```html
<div class="wrapperDiv">
  <div class="textDiv">...</div>
  <div class="floatDiv">float left</div>
  <div class="blankDiv"></div>
</div>
<div class="bottomDiv">...</div>
```

```css
.blankDiv {
  clear: both;
}
```



第二种：浮动元素的父元素上使用 overflow 触发 BFC。

```css
.wrapperDiv {
  overflow: auto; /* 或者 hidden */
}
```



 第三种：浮动元素的父元素上使用伪元素。

```css
.clearfix:after {
  display: block;
  clear: both;
  content: "";
}
.clearfix {
  *zoom: 1; /* IE 下触发 haslayout，清除浮动和解决外边距重叠 */
}
```



## 水平居中

前三种方法就能满足需求了，后面的两种扩展思路，适合的场景在垂直居中。

**（1）margin**  

适用于块级元素。

```css
.block {
  margin: 0 auto;
}
```

**（2）text-align**

适用于行内元素。

```css
.inline {
  text-align: center;
}
```

**（3）flex**

如果不考虑兼容性，使用 flex。

```css
.parentbox {
  display: flex;
  justify-content: center;
}
```

**（4）position + transform**

块级元素和行内元素均适用。

```css
.parentbox {
  position: relative;
}
.childbox {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
```

**（5）position + margin-left**

与上一种方法原理一样，**但需要知道元素自身的宽度。**

```css
.parentbox {
  position: relative;
}
.childbox {
  position: absolute;
  left: 50%;
  margin-left: -50%的childbox宽度;
}
```



## 垂直居中

**（1）line-height**

适用于单行文本，并且知道元素的高度。

```css
.block {
  height: 100px;
  line-height: 100px;
}
```

**（2）table-cell + vertical-align**

适用单行的子元素（块级或者行内都适用）。

```css
.parentbox {
  display: table-cell;
  vertical-align: middle;
}
```

**（3）flex**

如果不考虑兼容性，使用 flex。

```css
.parentbox {
  display: flex;
  align-items: center;
}
```

**（4）position + tranform**

块级元素和行内元素均适用。

```css
.parentbox {
  position: relative;
}
.childbox {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
```

**（5）position + margin**

与上一种方法原理一样，**但需要知道元素自身的高度**。

```css
.parentbox {
  position: relative;
}
.childbox {
  position: absolute;
  top: 50%;
  margin-left: -50%的childbox高度度;
}
```



## 布局

传统的布局方式：table、float、position

CSS3 后新的布局方式：flex、grid



**记忆心得：**

需要组合才能三栏的布局的 float、position；

不管是双栏、三栏或者更多的固定宽度或不固定宽度万能布局的 table、flex、grid。



### 三栏布局

三栏布局值得注意的是，中间部分需要优先渲染，保证重要的部分优先出现，从而提高用户体验。也就是说在 HTML 文档结构，中间内容的标签是书写在左右两部分前的。



三栏布局我总结出来的有 8 种：calc()、float、position、圣杯布局、双飞翼布局、flex、table、grid。

float 和 position 最简单，但是实现的三栏布局会有些缺点。

float 实现，中间的主体需要放在左右两部分之后，中间部分晚于左右部分的渲染。

position  实现，中间的部分存在 min-width 或者子元素有固定宽度，则会发生重叠。



圣杯布局和双飞翼布局比较难理解，flex 、table-cell、grid 则是随 css3 出现的，老浏览器存在兼容性问题。



**（1）圣杯布局**

Matthew Levine 于2006年在「A LIST APART」上写的一篇文章，一个经典的三栏式布局，中间内容优先渲染。结构上两边固定宽度，中间自适应。

之所以叫做圣杯布局，是因为把两边固定宽度的看做杯壁，中间自适应的部分相当于液体，液体会随着杯子的大小而自动延伸，。



基本结构：

```html
<div id="header">#header</div>

<div id="container" class="clearfix">
  <div id="center" class="column">#center</div>
  <div id="left" class="column">#left</div>
  <div id="right" class="column">#right</div>
</div>

<div id="footer">#footer</div>
```



CSS 关键部分：

```css
#container {
  padding: 0 150px 0 100px; 
}

#container .column {
  float: left;
  position: relative;
}

#center {
  width: 100%;
}

#left {
  width: 100px;
  margin-left: -100%;
  right: 100px;
}

#right {
  width: 150px;
  margin-right: -150px;
}
```



书写步骤：

1. container 使用预留 padding 预留 left 和 right 的位置，并且需要形成 BFC（防止内部浮动导致高度塌陷）
2. container 下的三栏全部左浮动，center 宽度为 100%，left 和 right 固定宽度
3. 使用 `margin-left: -100%` 上移，`position: relative` 和 `right: 100px `调整 left 到 container 的预留位置
4. 使用 margin-right 调整 right 到预留位置



**（2）双飞翼布局**

双飞翼布局始于淘宝UED，与圣杯布局的效果一致。

**不同的是，解决“中间内容不被遮挡”的方式不一样**。双飞翼采用的新增一个元素包裹中间 center 部分，用 margin 来预留左右两边内容的位置，并且少些几个样式；在这点上圣杯布局用的是公共父元素 container 的 padding 来为左右部分预留位置。



基本结构：

```html
<div id="header">#header</div>

<div id="container" class="clearfix">
  <div class="center-box">
    <div id="center" class="column">#center</div>
  </div>
  <div id="left" class="column">#left</div>
  <div id="right" class="column">#right</div>
</div>

<div id="footer">#footer</div>
```



CSS 关键部分：

```css
.column {
  float: left;
}

#center-box {
  width: 100%;
}

#center {
  margin: 0 150px 0 100px;
}

#left {
  width: 100px;
  margin-left: -100%;
}

#right {
  width: 150px;
  margin-left: -150px;
}
```



**（3）Flex布局**

在不考虑老版本兼容性的情况下，flex 布局总是优先考虑的，既简单又方便。



基本结构：

```html
<div id="header">#header</div>

<div id="container">
  <div id="center">#center</div>
  <div id="left">#left</div>
  <div id="right">#right</div>
</div>

<div id="footer">#footer</div>
```



CSS 关键部分：

```css
#container {
    display: flex;
}

#center {
    flex: 1; /* 填满剩余空间 */
}

#left {
    width: 100px;
    order: -1; /* 排在 center 和 right 前 */
}

#right {
    width: 150px;
}
```



**（4）Table布局**

这里不是  `<table>` 标签，而是 `display: table` ，与 float 存在一样的问题，center 部分不能优先渲染。



基本结构：

```html
<div id="header">#header</div>

<div id="container">
  <div id="left">#left</div>
  <div id="center">#center</div>
  <div id="right">#right</div>
</div>

<div id="footer">#footer</div>
```



CSS 关键部分：

```css
#container {
    display: table;
    width: 100%;
}

#container>div {
    display: table-cell;
}

#left {
    width: 100px;
}

#right {
    width: 150px;
}
```





**（5）Grid布局**

grid 布局与 flex 布局有一定的相似性，区别在于 flex 布局控制轴线，看做一维布局；grid 布局 控制单元格，看做而为布局。grid 布局比 flex 布局要强大的多。

实现三栏布局问题也和 table、float 一样，结构不一样。

基本结构：

```html
<div id="header">#header</div>

<div id="container">
  <div id="center">#center</div>
  <div id="left">#left</div>
  <div id="right">#right</div>
</div>

<div id="footer">#footer</div>
```



CSS 关键部分：

```css
#container {
    display: grid;
    grid-template-columns: 100px 1fr 150px;
}
```