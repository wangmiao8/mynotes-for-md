# this 指向问题

那么相信对于前端来说 this 肯定是必须要深入理解的知识点，这是也是我们平时写出 Bug 的重要原因之一。

为什么要深入理解 this ？

- 更好的理解和阅读源码，成为大佬的必经之路
- 工作中少写 Bug，写出代码复用性高和简洁的代码
- 面试高频题啊～

### this 到底是什么呢？

**this 是个指针，指向调用函数的对象。**下面我们来看一下正常也最常见的栗子🌰：

```javascript
const name = 'noko'

// 默认绑定
function sayName() {
  console.log(this.name)  
}

// 隐式绑定
const o = {
  name: 'miko',
  sayName: sayName
}

sayName()     // output: noko
o.sayName()   // output: miko
```

很好理解对不对，没人调用函数的时候指向的是 window 对象，o 调用了函数那么 this 就指向 o 对象；这里 this 绑定规则属于 **默认绑定和隐式绑定，**至于什么是默认绑定和隐式绑定？我们后面会说到。

### this 到底指向谁？

这个问题是新手在面对 this 的时候困惑最多的问题，同时也是容易出现 bug 的地方。<br />**<br />**那么我们参考最常听说的用一句话来总结：**<br />**
> **this 的指向，“谁调用它，this 就指向谁”**

这样不够专业，现在我们要用更专业一点的一句话来描述：

> **this 的指向，是根据函数调用时的执行上下文动态确定的**

（概念的解释）

### 解谜，this 的绑定规则

要想解谜 this 的指向，那么我们可以从 this 的绑定规则入手，this 的绑定规则有 4 个：

1. **默认绑定**
1. **隐式绑定**
1. **显性绑定**
1. **new 绑定**

我们只需要判断遇到 this 场景属于是什么绑定，以及它们间多种绑定的优先级，那么就能知道 this 的指向。

#### 1. 默认绑定

我们一开始举的栗子🌰就是默认绑定，知道你们懒得翻所以我给你们又贴出来了。

```javascript
const name = 'noko'

function sayName() {
  console.log(this.name)  
}

sayName()     // output: noko
```

在没有其他对象调用函数的时候，this 的指向会默认指向 window，而 name 是 window 下的属性，所以毫无疑问的输出 noko，但是值得注意的是严格模式下为 this 为 undefined，所以叫** 默认绑定**。也可以理解为无法应用其他三条规则的时候的默认规则。

以此看来，默认绑定的优先级是最低的。

#### 2. 隐式绑定

函数的调用是在某个对象上触发的，即调用位置上存在上下文对象，典型的形式为 obj.fun()，obj 就是上下文对象 。参考还是之前举过的栗子🌰：

```javascript
function sayName() {
  console.log(this.name)  
}

var o = {
  name: 'miko',
  sayName: sayName
}

o.sayName()    // output: miko
```

那么问题来了，根据这样 obj.fun() 来判断是 this 有问题的，再来一段代码：

```javascript
function sayName() {
  console.log(this.name)  
}

const o = {
  name: 'miko',
  sayName: sayName
}

const o2 = {
  name: '小明',
  myFriend: o
}

o.sayName()            // output: miko
o2.myFriend.sayName()  // output: miko 
```

o2.myFriend.sayName() 输出的还是 miko，跟小明没有半毛钱关系。小明 os：表面朋友，哼～

判断这种隐式绑定的 this 情况，如 o.sayName()，又或者是 ... .o3.o2.myFriend.sayName() 这样长长的调用，判断 fun() 中 this 的有个小技巧，**敲黑板划重点了：**

> **this 永远指向最后调用它的那个对象**


也就是说o.sayName()， ... .o3.o2.myFriend.sayName() 这些情况，this 的指向都取决于最后调用它的这个对象，在上面的栗子🌰中，指的就是 o 对象，myFriend 也是 o 对象的引用，所以最后都是 miko。

🌟**绑定丢失的陷阱**

上面的隐式绑定的规则可能会出现误导，我们来看看都发生了什么事情。

（1）赋值操作出现的绑定丢失：

```javascript
function sayName() {
  console.log(this.name)  
}

const name = 'noko'

const o = {
  name: 'miko',
  sayName: sayName
}

const o2 = {
  name: 'jeny',
}

o.sayName()  // output: miko

const newo = o.sayName
newo()      // output: noko
```

newo() 为什么会输出在 window 下的 noko？

这也是隐式绑定的一大坑，绑定规则是没错，但是绑定丢失了，怎么理解呢？再来回顾一下隐式绑定的概念

> 函数的调用是在某个对象上触发的，即调用位置上存在上下文对象

上面的例子把 o.sayName 的上下文对象是 o 这个对象，但当我们把 o.sayName 函数的引用传递给了 newo 这个变量，那么 newo 调用的时候所处的上下文环境就是 this 的指向，也就是 window。而这个问题经常会在我们写代码中不知不觉的写出来，很隐蔽的陷阱，并且它**通常会在赋值的时候出现**，所以存在 this 的函数赋值到其他地方需要小心又小心。

其实这个题目已经不是隐式绑定了，丢失了绑定，不属于上面所说的后面三种的 this 绑定规则，所以采用了默认绑定，绑定到了 window 上。我们只要看到 fun() 这种方式的调用，肯定不是隐式绑定。

（2）回调函数出现的绑定丢失：

```javascript
const name = 'miko'
const o = {
  name: 'noko',
  sayName: function() {
    setTimeout(function() {
      console.log(this.name)
    },100)
  }
}

o.sayName()  // output: miko
```

> MDN 文档：由 setTimeout() 调用的代码运行在与所在函数完全分离的执行环境上。这会导致，这些代码中包含的 this 关键字在非严格模式会指向 window (或全局)对象，严格模式下为 undefined，这和所期望的 this 的值是不一样的。

这里关于 setTimeout 的 this 的解释和解决方案 MDN 更加详细和权威：[window.setTimeout](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setTimeout)

#### 3. 显性绑定

显性绑定就容易理解多了，顾名思义，显性绑定就是看得见的绑定，一眼就能看出来 this 的指向。

显示绑定的三个方式：call、apply、bind。

call 和 apply 的区别只是传参方式的不同，一个例子搞定它：

```javascript
function showAge() {
  console.log(this.age)
}

const noko = {
  age: 18
}
const miko = {
  age: 20
}
const rous = {
  age: 22
}

const nokoAge = showAge.call(noko)
const mikoAge = showAge.call(miko)
const rousAge = showAge.call(rous)

nokoAge() // output: 18
mikoAge()  // output: 20
rousAge()  // output: 22
```

关于 call、apply、bind 的区别和模拟实现，本文不做太多解释（不解释不代表不重要），我们的目的是继续搞定 this 的问题。

#### 4. new 绑定

没错 new 也是能够改变 this 的，这个也很好判断和理解

```javascript
function SayName(name) {
  this.name = name
} 

const aName = new SayName('noko')

console.log(aName.name)  // output: noko
```
SayName 作为一个构造函数，将传入的 name 参数赋值给 new 操作后得到的对象上，这里的 this 指向的是new 操作后得到的对象。

#### 5. 绑定规则的优先级

如果一个 this 碰到了多个绑定规则，那么会优先采用什么规则呢？

> new 绑定 > 显式绑定 > 隐式绑定 > 默认绑定

###  总结

在上面 4 种绑定规则中，隐式绑定是最容易踩坑的，其他三种都能明确的看出 this 的指向，隐式绑定中 this 的指向不会明明白白告诉我们，看不见的就会容易出错。
