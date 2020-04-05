## 选择器优先级


## position


## 盒模型

标准、IE 盒模型

修改盒模型的类型



JavaScript 获取盒模型高度

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

(1) margin  

适用于块级元素

```css
.block {
  margin: 0 auto;
}
```

(2) text-align

适用于行内元素

```css
.inline {
  text-align: center;
}
```

(3) flex

如果不考虑兼容性，使用 flex

```css
.parentbox {
  display: flex;
  justify-content: center;
}
```

(4) position + transform

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

(5) position + margin-left

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

(1) line-height

适用于单行文本，并且知道元素的高度

```css
.block {
  height: 100px;
  line-height: 100px;
}
```

(2) vertical-align

适用单行的子元素（块级或者行内都适用）

```css
.parentbox {
  display: table-cell;
  vertical-align: middle;
}
```

(3) flex

如果不考虑兼容性，使用 flex

```css
.parentbox {
  display: flex;
  align-items: center;
}
```

(4) position + tranform

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

(5) position + margin

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