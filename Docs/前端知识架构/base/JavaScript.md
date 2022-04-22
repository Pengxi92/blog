# 数据类型

* 基本类型
字符串（String）、数字(Number)、布尔(Boolean)、对空（Null）、未定义（Undefined）、Symbol。
占据固定大小，保存在栈内存中。

* 引用数据类型
对象(Object)、函数(Function)。其他的还有数组(Array)、Date(日期)、RegExp(正则)等。
保存在堆内存中，栈内存中保存堆内存数据的引用。

> [「前端进阶」JS中的栈内存堆内存](https://juejin.cn/post/6844903873992196110)

## 数组是如何存储的

* 同种类型数据的数组分配连续的内存空间
* 存在非同种类型数据的数组使用哈希映射分配内存空间

> [ziyi2/data-structure-algorithm-procedure](https://github.com/ziyi2/data-structure-algorithm-procedure/blob/master/src/Hash.js)<br>
[浅谈JS内存机制](https://mp.weixin.qq.com/s/dtEciFpNSrPGR63aScaoww)

## 数据类型的判断

* typeof 对null、数组、特殊对象（如Date）会判断为object
* Object.prototype.toString.call(xxx) 大杀器，基本能搞定所有数据（排除`toString`被重构的情况）
* instanceof 判断元素的原型链是否存在于某种元素，能区分Array、Object、Function
* Number.isNaN 特殊处理，因为`NaN == NaN`和`NaN === NaN`都会返回`false`，也可以用`x !== x`来判断是否为NaN
* Array.isArray

## 数据类型转换

> [JavaScript-强制类型转换](https://juejin.cn/post/6855920843260690440)

## 0.1 + 0.2 === 0.3

> [一次Javascript的计算浮点数精度问题记录](https://juejin.cn/post/6944243108410458149)

## 函数相关

* arguments
* new操作符
* ...

## 常用函数

* 数组
* 遍历
* 对象设置(Proxy的兼容方案，思考【在 JavaScript 中如何实现对象的私有属性？】)
* call、apply、bind
* ...

> [前端算法入门一：刷算法题常用的JS基础扫盲](https://juejin.cn/post/7087134135193436197)<br>
[解锁多种JavaScript数组去重姿势](https://juejin.cn/post/6844903608467587085)

## 性能差异

* 考察 for、for...of、for...in、forEach、while、do...while等

## 如何提升 JavaScript 变量的存储性能

* 访问字面量和局部变量的速度最快，访问数组元素和对象成员相对较慢
* 由于局部变量存在于作用域链的起始位置，因此访问局部变量比访问跨作用域变量更快，全局变量的访问速度最慢
* 属性和方法在原型链中的位置越深，则访问它的速度也越慢
* ...

## ES6新增属性

* let、const
* 箭头函数
* 展开操作符
* Promise
* await/async
* Proxy
* Class
* ...

# 代码执行

## use strict

使用“use strict”指令的目的是强制执行严格模式下的代码。 在严格模式下，咱们不能在不声明变量的情况下使用变量。 早期版本的js忽略了“use strict”。

## 函数执行过程(重点，可以考虑再拆分下)

> [JavaScript函数执行过程](https://juejin.cn/post/6847902222144159752)

## JS的继承

* 原型链继承
* 构造继承
* 实例继承 Object.craete
* 拷贝继承
* 组合继承
* 寄生组合式继承

> [js类和继承](https://github.com/ziyi2/js/blob/master/JS%E7%B1%BB%E5%92%8C%E7%BB%A7%E6%89%BF.md)<br>
[js寄生组合式继承](https://blog.csdn.net/qq_26222859/article/details/77508778)

## ES5 和 ES6 继承的区别

> [js类和继承](https://github.com/ziyi2/js/blob/master/JS%E7%B1%BB%E5%92%8C%E7%BB%A7%E6%89%BF.md)

## 垃圾回收机制

### 栈空间的回收机制

调用栈中有一个记录当前执行状态的指针（称为 ESP），只要函数调用结束，该栈内存就会自动被回收，不需要我们操心。刚刚我们也聊到闭包，如果出现闭包的情况，闭包的数据就会组成一个对象保存在堆空间里。

### 堆空间的回收机制

JS 把堆空间分成新生代和老生代两个区域，新生代中存放的是生存时间短的对象，通常只支持 1～8M 的容量；老生代中存放的生存时间长的对象，一些大的数据也会被直接分配到老生区中。而针对这两个区域，JS 存在两个垃圾回收器：主垃圾处理器和副垃圾处理器。

1. 副垃圾回收器

> 副垃圾回收器主要是采用 Scavenge 算法进行新生区的垃圾回收，它把新生区划分为两个区域：对象区域和空闲区域，新加入的对象都会存放到对象区域，当对象区域快被写满时，会对对象区域进行垃圾标记，把存活对象复制并有序排列至空闲区域，完成后让这两个区域角色互转，由此便能无限循环进行垃圾回收。同时存在对象晋升策略，也就是经过两次垃圾回收依然还存活的对象，会被移动到老生区中。

2. 主垃圾回收器

> 主要是采用标记-整理算法，其工作流程是从一组根元素开始，递归遍历这组根元素，在这个遍历过程中，能到达的元素称为活动对象，没有到达的元素就可以判断为垃圾数据。接着让所有存活的对象都向一端移动，然后直接清理掉端边界以外的内存。垃圾回收工作是需要占用主线程的，必须暂停JS脚本执行等待垃圾回收完成后恢复，这种行为称为全停顿。 由于老生代内存大，全停顿对性能的影响非常大，所以出现了增量标记的策略进行老生区的垃圾回收。

* 标记清除
在js中，最常用的垃圾回收机制是标记清除：当变量进入执行环境时，被标记为“进入环境”，当变量离开执行环境时，会被标记为“离开环境”。垃圾回收器会销毁那些带标记的值并回收它们所占用的内存空间。

* 谷歌浏览器-查找引用
浏览器不定时去查找当前内存的引用，如果没有被占用了，浏览器会回收它；如果被占用，就不能回收。

* IE浏览器-引用计数法
当前内存被占用一次，计数累加1次，移除占用就减1，减到0时，浏览器就回收它。

> [浅谈JS内存机制](https://mp.weixin.qq.com/s/dtEciFpNSrPGR63aScaoww)

## 内存泄漏

不再用到的内存，没有及时释放，就叫做内存泄漏(memory leak)。

常见的内存泄露主要有 4 种,全局变量、闭包、DOM 元素的引用、定时器。

> [内存泄漏的原因及解决办法_浅谈 JS 内存泄漏问题](https://blog.csdn.net/weixin_39849287/article/details/110963792)

## import 和 exports

# 进阶

## PWA 和 Service Worker

> [面试官：请你实现一个PWA 我：😭](https://juejin.cn/post/6844904052166230030)
