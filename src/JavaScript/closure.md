# 闭包

- 特性

  1. 函数嵌套函数
  2. 函数内部可以引用外部的参数和变量
  3. 参数和变量不会被垃圾回收机制回收

- 定义

  - 闭包指有权访问另一个函数作用域中的变量的函数【更通俗的说法，一个函数可以任意访问在其定义的作用域外的变量】
  - [常见的方式是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量]

- 好处

  1. 希望一个变量长期驻扎在内存中
  2. 避免全局变量的污染
  3. 私有成员的存在

- 例子：

```javascript
const f = (a) => (b) => {
  console.log(a, b);
};

// 等价于

function f(a) {
  return function (b) {
    console.log(a, b);
  };
}
```
