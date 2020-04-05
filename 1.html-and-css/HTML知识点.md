## 1. HTML 语义化

从代码层面上，代码是可读的且富有语义的。

对于开发者而言，在没有 CSS 和 JS 也能快速的了解页面的结构和内容的类型；

对于机器爬虫而言，则更好的进行爬取，提高搜索排名。

HTML5 常见的语义化标签：

header、nav、aside、article、seation、footer

## 2. 元信息类标签

元信息指的是描述自身的信息

### head 标签

head 标签本身不带任何信息，用于存放其他语言化标签的容器。比如 title、style、script等

### title 标签

存放整个网页的标题，所以标题最好能尽量的体现网页的内容，可能会被用在浏览器收藏夹、微信推送卡片、微博等场景。

### meta 标签

meta 标签是一组键值对，它是一种通用的元信息表示标签。常见的键值对是 name 和 content。

**meta 的其他属性：**

1. charset 属性

用于描述 HTML 文档自身的编码形式

```
<meta charset="UTF-8">
```

一般情况下，服务器会在 http 的头部指定正确的编码方式，但特殊情况下如使用 file 协议打开一个 HTML 文件，则没有 http 头，这个时候指定 charset 属性是非常重要的。

2. http-equiv 属性

能改变服务器和用户引擎的行为。

```
<!-- 每隔 30 秒刷新一次文档 -->
<meta http-equiv="refresh" content="30">
<!-- 默认使用最新浏览器 -->
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
<!-- 不被网页（加速）转码 -->
<meta http-equiv="Cache-Control" content="no-siteapp">
```

"content-security-policy" 内容安全策略属性，用于指定允许的服务器源和脚本的端点。

3. name 为 viewport 的属性

常见于移动端开发，用于控制视口的大小和比例。

```
<meta name="viewport" content="width=device-width, initial-scale=1">
```

上面的 meta 指定了视口的宽度为设备的宽度，初始的缩放比例为 100%。

## 3. DTD 和 SGML

SGML 是很古老的标记语言，在很久以前被 IBM 的所使用。DTD（文档类型定义） 用于定义文档的类型。

HTML 作为 SGML 的子集，遵循 SGML 的基本语法，所以能看到在 HTML5 前都使用 DTD 来定义 HTML 文档的类型。HTML5 之后抛弃了 SGML 兼容，使用了简单的 <!DOCTYPE html>

## 4. 严格模式和混杂模式

在 HTML 文档最前面使用 DOCTYPE 来引用 DTD，决定浏览器使用什么版本解析文档。

严格模式：正常的编写 DOCTYPE 就能启用，根据 W3C 标准解析代码。（又称标准模式）  
混杂模式：缺少 DOCTYPE 的时候会启用，浏览器用自己的方法解析文档。（又称兼容模式）

两种模式的差异之一，盒模型的宽高计算规则。

严格模式下，元素的 width 和 height 等于设置的宽高；而混杂模式下，元素的 width 和 height，除 content 本身还包含了 padding 和 border。