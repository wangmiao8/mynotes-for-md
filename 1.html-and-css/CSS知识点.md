## 选择器优先级





## position





## 盒模型

所有的 HTML 元素都能看做成一个盒子，盒子由四个部分组成：

1. content，盒子的内容
2. padding，内容与盒子边框的间距
3. boder，盒子的边框
4. margin，盒子与盒子之间的间距

由于浏览器历史原因，盒模型的计算规则存在差异，盒模型有分为 IE 盒模型和标准盒模型，在 IE6 以前使用的是 IE 盒模型，而我们现在默认使用的是标准盒模型，当然我们也可以手动修改盒模型。



**标准盒模型和 IE 盒模型：**

标准盒模型和 IE 模型的区别在于盒模型的宽高的计算方式

IE 盒模型：width = content + padding + border

标准盒模型：width = content



**修改盒模型的宽高计算方式**：

我们可以在 CSS 中使用 `box-sizing` 属性使用手动修改盒子宽高的计算方式

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

1. `ele.style.width/height `：只能获取到行内样式设置的宽高，不能获取到 link 或者 style 内设置的宽高

2. `ele.currentStyle.width/height`：能获取到渲染后的宽高，但只有 IE 上支持

3. `window.getComputeSytle(ele).width/height`：能获取到渲染后的宽高，大多数浏览器支持，IE9 以上支持
4. `ele.getBoundingClientRect().width/height`：效果和兼容性和第三种一样，同时该 API 还支持获取元素相对视窗的上下距离



## BFC

BFC 是 Web 页面CSS 渲染区域，是**块盒子**的布局过程发生区域，也是浮动元素交互区域。

简单来说就是：**这个区域内使脱离标准文档流的元素能正确布局和交互**。

BFC 的作用：**建立一个隔离区域，断绝区域内外的元素的作用**。

BFC 解决的问题：**浮动的影响（高度坍塌、元素重叠）、外边距重合**

创建 BFC 的方法：

- **html**
- **float** 不为 none
- **position** 值为 absolute 或 fixed
- **display** 值为 inline-block、flex、table-cell
- **overflow** 不为 visiable



## 清除浮动

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

**2. vertical-align**

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