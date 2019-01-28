高性能JavaScript
===

为什么优化是必要的
---

Why optimization is necessary?

- IE6的JavaScript引擎是采用“静态垃圾回收机制”，也就是该引擎只监视内存中固定数量的对象来确定何时进行垃圾回收。
- 解释性的代码都比编译性的代码慢。因为解释性的代码需要经历代码转化成计算机指令的过程，也就是解释器中的代码怎么写，就被怎么执行。而且编译器是基于词法分析去判断代码想实现什么，然后产生出能完成任务的运行代码最快的机器码来进行优化。

下一代JavaScript引擎
---

Next-Generation JavaScript Engines

- Chrome 的V8引擎，V8是一款为 JavaScript 打造的实时编译引擎（JIT），它把 JavaScript 代码转化为机器码来执行，所以给人的感觉是 JavaScript 执行速度特别快
- Safari 的Nitro引擎（SquirrelFish Extreme）[SquirrelFish 金鳞鱼]
- FireFox 的TraceMonkey引擎

1、加载和执行
---

Loading and Execution

- 减少 JavaScript 对性能的影响
  - `</body>`闭合标签之前，将所有的`<script>`标签放到页面底部。因为能保证在脚本执行前，页面已经完成渲染
  - 合并脚本。加载速度更快，响应速度也更快
  - 使用`<script>`脚本的defer属性
  - 使用动态创建的`<script>`元素来下载并执行代码
  - 使用 XHR 对象下载 JavaScript 代码并注入代码中

2、数据存取
---

Data Access

- JavaScript引擎存取的内部属性`[[scope]]`包含一个函数被创建时的作用域中对象的集合。这个内部属性也可以叫做作用域链，它可以决定哪些数据能被这个函数访问到

- 执行函数时，会创建一个执行环境（也可以叫执行上下文）的活动对象。【注意，函数每次执行时，对应的执行环境都是独一无二的，所以多次调用同一个函数就会导致创建多个执行环境。当函数执行完毕之后，执行环境就会被销毁】活动对象包含了所有`局部变量`、`命名参数`、`参数集合`以及`this`。==> 然后这个活动对象会被推入到作用域链的最前端，当执行环境被销毁，活动对象也随之被销毁

- 全局作用域总是存在于执行环境的作用域链的最末端，因此全局作用域的读写速度也是最慢的。因为在执行环境的作用域中，一个标识符所在的位置越深，它的读写速度就越慢。==> 综上所述，1、建议尽可能使用局部变量；2、如果某个跨作用域的值被引用超过一次以上，就把它存储到局部变量里面

- 闭包允许访问作用域以外的数据。「一句话的精髓」

- 在大部分浏览器中，通过点表示法（Object.name）操作和通过括号表示法（Object["name"]）操作并没有明显的区别。只有在Safari中，点表示法始终会更快一些而已。

- 通常来说，在一个函数中，如果要多次读取同一个对象属性，最佳的做法是将属性值保存到局部变量中。因为局部变量能用来替代属性，以避免多次查找带来的性能开销

3、DOM编程
---

```javascript
// 较慢
function collectionGlobal(){
  var coll = document.getElementsByTagName('div'),
    len = coll.length,
    name = '';
  for(var count = 0; count < len; count++){
    name = document.getElementsByTagName('div')[count].nodeName;
    name = document.getElementsByTagName('div')[count].nodeType;
    name =document.getElementsByTagName('div')[count].tagName;
  }
  return name;
}

// 较快
function collectionGlobal(){
  var coll = document.getElementsByTagName('div'),
    len = coll.length,
    name = '';
  for(var count = 0; count < len; count++){
    name = coll[count].nodeName;
    name = coll[count].nodeType;
    name = coll[count].tagName;
  }
  return name;
}

// 最快
function collectionGlobal(){
  var coll = document.getElementsByTagName('div'),
    len = coll.length,
    name = '',
    el = null;
  for(var count = 0; count < len; count++){
    el = coll[count];
    name = el.nodeName;
    name = el.nodeType;
    name = el.tagName;
  }
  return name;
}
```