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

- relative 相对自身定位

- absolute 相对非 static 的祖先元素定位

- sticky 相对最近的滚动父元素和最近的块级父元素定位

- fixed 相对视口（viewport）定位



**属性的特性：**

- absolute 和 fixed，设置后会脱离正常的文档流，并且创建 BFC

- 在 sticky 的父元素上设置 overflow 为 visible 以外的值会导致 sticky 无效



**fixed 和 sticky 的不同：**

- 设置 position 为 fixed 后会脱离正常文档流，而 sticky 不会
- sticky 相对滚动父元素定位，受父元素约束，而 fixed 已脱离文档流，相对视口定位



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

1. `ele.style.width/height `：只能获取到行内样式设置的宽高，不能获取到 link 或者 style 内设置的宽高。

2. `ele.currentStyle.width/height`：能获取到渲染后的宽高，但只有 IE 上支持。

3. `window.getComputeSytle(ele).width/height`：能获取到渲染后的宽高，大多数浏览器支持，IE9 以上支持。
4. `ele.getBoundingClientRect().width/height`：效果和兼容性和第三种一样，同时该 API 还支持获取元素相对视窗的上下距离。



## BFC

MDN 文档给出的定义：BFC 是 Web 页面 CSS 渲染区域，是块盒子的布局过程发生区域，也是浮动元素交互区域。

简单来说就是：**这个区域内使脱离标准文档流的元素能正确布局和交互**。



BFC 的作用：**建立一个隔离区域，断绝区域内外的元素的作用。**

BFC 解决的问题：**浮动的影响（高度坍塌、元素重叠）、外边距重合。**



**创建 BFC 的方法：**

- **html**
- **float** 不为 none
- **position** 值为 absolute 或 fixed
- **display** 值为 inline-block、flex、table-cell
- **overflow** 不为 visiable



**BFC 渲染规则：**



## 清除浮动

浮动（float）会导致元素脱离标准文档流，可能引起父元素高度塌陷，影响兄弟元素的排版。



![浮动效果](https://user-gold-cdn.xitu.io/2017/10/18/2af797ffc0918352ac8d381994ff1a27?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



清除浮动的大致分为以下三种方法：



第一种：在浮动元素后创建新的块级元素添加 `clear:both` 或者 `clear:left`

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



第二种：浮动元素的父元素上使用 overflow 触发 BFC

```css
.wrapperDiv {
  overflow: auto; /* 或者 hidden */
}
```



 第三种：浮动元素的父元素上使用伪元素

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

**1. margin**  

适用于块级元素

```css
.block {
  margin: 0 auto;
}
```

**2. text-align**

适用于行内元素

```css
.inline {
  text-align: center;
}
```

**3. flex**

如果不考虑兼容性，使用 flex

```css
.parentbox {
  display: flex;
  justify-content: center;
}
```

**4. position + transform**

块级元素和行内元素均适用

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

**5. position + margin-left**

与上一种方法原理一样，**但需要知道元素自身的宽度**

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

**1.  line-height**

适用于单行文本，并且知道元素的高度

```css
.block {
  height: 100px;
  line-height: 100px;
}
```

**2. table-cell + vertical-align**

适用单行的子元素（块级或者行内都适用）

```css
.parentbox {
  display: table-cell;
  vertical-align: middle;
}
```

**3. flex**

如果不考虑兼容性，使用 flex

```css
.parentbox {
  display: flex;
  align-items: center;
}
```

**4. position + tranform**

块级元素和行内元素均适用

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

**5. position + margin**

与上一种方法原理一样，**但需要知道元素自身的高度**

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



## 常见布局

### 双栏布局

### 三栏布局

float、position、flex、table、grid

## 响应式布局





## CSS 阻塞渲染





## CSS 模块化

BEM、CSS in JS、CSS Modules