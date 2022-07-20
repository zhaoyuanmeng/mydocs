# 💀 模块相关知识

## 前置知识(进化过程)

- 全局function模式

将不同的功能封装成不同的全局函数.问题是容易污染全局命名空间,容易引起命名冲突或数据不安全,并且
模块成员之间看不出直接关系

```javascript

function test(params) {

}

function test2() {

}

```

- namespace模式

减少了全局变量,解决命名冲突,问题是数据不安全(外部可以直接修改模块内部的数据)

```javascript

let obj = {
  data:'adaa',
  test(params) {

  },

  test2() {
    
  }
}

// 外部能够直接访问这些东西
obj.data = 'aaa'

```

---


- IIFE模式(这个很重要源码打完包都是这个玩意)

数据是私有的,外部只能通过暴露的方法进行操作,将数据和行为方法封装到一个函数内部,通过给window添加属性来向外暴露接口

问题是如果当前这个模块依赖另外一个模块怎么办

```javascript

// 基础版 不涉及到依赖另外一个模块
(function(window) {
  let data = 'www.baidu.com'
  //操作数据的函数
  function foo() {
    //用于暴露有函数
    console.log(`foo() ${data}`)
  }
  function bar() {
    //用于暴露有函数
    console.log(`bar() ${data}`)
    otherFun() //内部调用
  }
  function otherFun() {
    //内部私有的函数
    console.log('otherFun()')
  }
  //暴露行为
  window.myModule = { foo, bar } //ES6写法
})(window)


// 依赖另外一个模块 就把他当成一个参数传递进去 当然这个依赖的模块需要在最前面否则会报找不到的错误
(function(window, $) {
  let data = 'www.baidu.com'
  //操作数据的函数
  function foo() {
    //用于暴露有函数
    console.log(`foo() ${data}`)
    $('body').css('background', 'red')
  }
  function bar() {
    //用于暴露有函数
    console.log(`bar() ${data}`)
    otherFun() //内部调用
  }
  function otherFun() {
    //内部私有的函数
    console.log('otherFun()')
  }
  //暴露行为
  window.myModule = { foo, bar }
})(window, jQuery)



```

---

## 模块化的好处

- 避免命名冲突(减少命名空间污染)

- 更好的分离,按需加载

- 更高的复用性

- 高可维护性


## 为什么要产生下面几个规范方式

### 问题:引入多个script后出现出现问题

- 请求过多,我们要依赖多个模块,那样就会发送多个请求,导致请求过多.

- 依赖模糊(这是关键),我们不知道他们的具体的依赖关系,导致需要有很大的人为负担还需要担心他的加载先后顺序.

- 维护成本高.也就是上面两个产生的一些东西导致的.


## CommonJs(这个很重要)

-  简介

> 我们现在所用到的node就是模块化的继承,里面的核心用到的就是CommonJs.
> 
> 每一个文件就是一个模块,有自己的作用域.在文件里面定义的变量/函数/类都是私有的,对其他的文件不可见.
>
> CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。

---


- 优点
  - 所有的代码都运行在模块作用域,不会污染全局作用域

  - <font face="逐浪立楷" color=red >模块可以多次加载,但是只会在第一次加载的时候运行一次,然后运行结果就被缓存了,以后再加载的时候就直接读取缓存</font>

  - 模块加载的顺序,按照在代码中出现的顺序.

- 原理部分
  - require基本功能
  > 读入并执行一个JavaScript文件，然后返回该模块的exports对象。如果没有发现指定模块，会报错。

  - 模块的加载机制
  > CommonJS模块的加载机制是，输入的是被输出的值的拷贝(浅拷贝)对于引用类型来说。也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。与ES6模块化有重大差异

    ```javascript
      // lib.js
      var counter = 3;
      function incCounter() {
        counter++;
      }
      module.exports = {
        counter: counter,
        incCounter: incCounter,
      };

      // main.js
      var counter = require('./lib').counter;
      var incCounter = require('./lib').incCounter;

      console.log(counter);  // 3
      incCounter();
      console.log(counter); // 3

      上面代码说明，counter输出以后，lib.js模块内部的变化就影响不到counter了。这是因为counter是一个原始类型的值，会被缓存。除非写成一个函数，才能得到内部变动后的值。也就是下面的写法

      // lib.js
      var counter = 3;
      function incCounter() {
        counter++;
      }
      module.exports = {
        get counter() {
          return counter
        },
        incCounter: incCounter,
      };
    ```
  ---

- 基本用法

```javascript




// example.js
var x = 5;
var addX = function (value) {
  return value + x;
};
module.exports.x = x;
module.exports.addX = addX;


// 引入模块 require('xxx')
var example = require('./example.js');//如果参数字符串以“./”开头，则表示加载的是一个位于相对路径
console.log(example.x); // 5
console.log(example.addX(1)); // 6


```

---


## ESMoudule

- 简介
> ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。
> 
> import read-only特性： import的属性是只读的，不能赋值，类似于const的特性
>
> export/import提升： import/export必须位于模块顶级，不能位于作用域内；其次对于模块内的import/export会提升到模块顶部，这是在编译阶段完成的
>
> 严格模式：ES6 的模块自动采用严格模式

- 优点(与commonjs不同点)

  - ES6 模块输出的是值的引用。

  >ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本静态分析的时候，遇到模块加载命令import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。换句话说，ES6 的import有点像 Unix 系统的“符号连接”，原始值变了，import加载的值也会跟着变。因此，ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。

  - CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

  >第二个差异是因为 CommonJS 加载的是一个对象（即module.exports属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

- 基本用法

```javascript
/** 定义模块 math.js **/
let basicNum = 0;
const add = function (a, b) {
    return a + b;
};
export { basicNum, add };
/** 引用模块 **/
import { basicNum, add } from './math';
function test(ele) {
    ele.textContent = add(99 + basicNum);
}
// export-default.js
export default function () {
  console.log('foo');
}

// import-default.js
import customName from './export-default';
customName(); // 'foo'


```
---

## UMD

- 简介

>AMD推崇依赖前置、提前执行，CMD推崇依赖就近、延迟执行
>
>UMD是AMD和CommonJS的一个糅合。AMD是浏览器优先，异步加载；CommonJS是服务器优先，同步加载。
>
>既然要通用，怎么办呢？那就先判断是否支持node.js的模块，存在就使用node.js；再判断是否支持AMD（define是否存在），存在则使用AMD的方式加载。这就是所谓的UMD。

- 优点

- 基本用法

```javascript
((root, factory) => {
  if (typeof define === 'function' && define.amd) {
    //AMD
    define(['jquery'], factory);
  } else if (typeof exports === 'object') {
    //CommonJS
    var $ = requie('jquery');
    module.exports = factory($);
  } else {
    //都不是，浏览器全局定义
    root.testModule = factory(root.jQuery);
  }
})(this, ($) => {
  //do something...  这里是真正的函数体
});

```


[参考资料](https://juejin.cn/post/6844903744518389768#heading-1)
[参考资料](https://zhuanlan.zhihu.com/p/467991875)
[参考资料](https://es6.ruanyifeng.com/#docs/module-loader#ES6-%E6%A8%A1%E5%9D%97%E4%B8%8E-CommonJS-%E6%A8%A1%E5%9D%97%E7%9A%84%E5%B7%AE%E5%BC%82)