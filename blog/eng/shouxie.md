# 👺 手写系列
- [虚拟列表](#👻-虚拟列表)
---
- ## <span id ="a">👻 手写promise</span>
```javascript

// 这个就是回调函数的完美应用
class MyPromise{
  // 私有属性

  // 终值
  PromiseResult
  // 状态
  PromiseState

  // 保存成功的回调函数
  sucessFns
  // 保存失败的回调函数
  errFns

  constructor(exec){
    // 这里exec是一个函数 这个函数要两个参数 这两个参数也是函数

    // 绑定好this
    this.initBind()
    // 初始化值
    this.initVal()
    exec(this.resolve,this.reject)
  }
  // 处理this问题的
  initBind(){
    this.resolve =  this.resolve.bind(this)
    this.reject =  this.reject.bind(this)
  }
  // 初始化的
  initVal(){
    this.PromiseResult = null
    this.PromiseState = 'pending'
  }
  // 核心函数
  resolve(val){
    // 说明是等待状态
    if(this.PromiseState !== 'pending'){
      return
    }
    this.PromiseState = "fulfilled";
    this.PromiseResult = val;
    // 说明当时存贮了异步的操作
    if (this.sucessFns.length > 0) {
      // 执行相当于then里面的成功函数
      this.sucessFns.map((fn) => fn(this.PromiseResult));
    }

  }
  reject(reason){
    // 说明是等待状态
     if(this.PromiseState !== 'pending'){
      return
    }
    this.PromiseState = "rejected";
    this.PromiseResult = reason;
    if (this.errFns.length > 0) {
      // 执行相当于then里面的成功函数
      this.errFns.map((fn) => fn(this.PromiseResult));
    }

  }
  // 这里也接受两个参数 都是函数参数
  then(onFulFilled,onRejected = () => { }){
    if(typeof onFulFilled != 'function')return
    if(typeof onRejected != 'function')return

    // 如果有异步的情况
    if(this.PromiseState == 'pending'){
       this.sucessFns.push(onFulFilled);
       this.errFns.push(onRejected);
       return;
    }
     // 成功
    if (this.PromiseState == "fulfilled") {
      onFulFilled(this.PromiseResult);
      return;
    }
    // 失败
    if (this.PromiseState == "rejected") {
      onRejected(this.PromiseResult);
      return;
    }

  }
  catch(){}
}

// 测试 
let p1 = new MyPromise((resolve, reject) => {
  // 这里面的参数实际上是一个函数 真正定义这个函数的逻辑是放在了promise那个类里面
  // reject("失败了！！！");
  // resolve("成功了！！！！");
  setTimeout(() => {
    resolve("成功了！！！！");
  }, 1000);
});
p1.then(
  (res) => {
    console.log("res", res);
  },
  (err) => {
    console.log("erro", err);
  }
);

```

---
- ## 👻 new实现的原理

```javascript
function Person(name){
  this.name =name
  this.sayName =function(){
    console.log('1111')
  }
}

// myNew的实现
function myNew(fn,...args) {
  // obj.__proto__ = fn.prototype
  let obj = Object.create(fn.prototype)

  let res = fn.call(obj,arg)
  // 返回值 一定是一个对象
  return typeof res == 'object' ? res : obj
}
let ceshi = myNew(Person,'liming')

```



---
- ## 👻 源码里面缓存的实现原理

```javascript

// 主要是利用了event loop
// 这里也可以用闭包写 就是做个全局变量
let cbs = [];
let wating = false;
function nextTickPlus(cb) {
  cbs.push(cb);
  if (!wating) {
    // 异步更新 不是掉一次更新一次
    setTimeout(flushCallBacks, 0);
    wating = true;
  }
}
function flushCallBacks() {
  // 这个cbs会实时的改变 如果有改变的情况下
  cbs.forEach((cb) => cb());
  wating = false;
}

```


---
- ## 👻 bind call apply

```javascript

// 实现apply

function apply(context, args){
  //这里默认不传就是给window,也可以用es6给参数设置默认参数
  context = context || window
  args = args ? args : []
  //给context新增一个独一无二的属性以免覆盖原有属性
  const key = Symbol()
  context[key] = this
  //通过隐式绑定的方式调用函数
  context[key](...args)
   //删除添加的属性
  delete context[key]
  //返回函数调用的返回值
  return result
}


```

- ## 👻 虚拟列表

```javascript
<template>
  <h1>测试虚拟列表{{}}</h1>
  <div
    ref="demo"
    class="scroll-box demo"
    :style="`height: ${showNumber * itemHeight}px;`"
  >
    <!-- 这个是用来有滚动的效果的 -->
    <div
      class="scroll-blank"
      :style="`height: ${data.length * itemHeight}px;`"
    ></div>
    <div class="scroll-data" :style="`top: ${positionTop}px;`">
      <div v-for="(item, index) in activeList" :key="item" class="scroll-item">
        {{ item }}
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, computed, onMounted, onUnmounted, ref } from 'vue';
import { useDebounceFn, useThrottleFn } from '@vueuse/core';

export default defineComponent({
  name: 'VueUse',
  setup() {
    //  scrollTop在有滚动条的情况下，为元素可视区域距离元素顶部的像素，也就是已经滚动了多少距离

    // 设计思路
    // 监听滚轮事件/触摸事件，记录列表的总偏移量。
    // 根据总偏移量计算列表的可视元素起始索引。
    // 从起始索引渲染元素至视口底部。
    // 当总偏移量更新时，重新渲染可视元素列表。（可以用computed计算）
    // 为可视元素列表前后加入缓冲元素。
    // 在滚动量比较小时，直接修改可视元素列表的偏移量。
    // 在滚动量比较大时（比如拖动滚动条），会重新渲染整个列表。
    // 事件节流。

    const createData = (length: number) => {
      return Object.keys(new Array(length).fill(''));
    };

    const demo: any = ref(null); // 外框盒子
    const showNumber = 20; // 当前视窗展示条数
    const itemHeight = 20; // 每一条内容的高度
    const data = createData(1000); // 实际数据
    let startNum = ref(0); // 当前视窗范围内第一个元素下标
    let positionTop = ref(0); // 当前视窗范围内第一个元素偏移量

    // 计算当前视窗内实际要渲染的内容
    const activeList = computed(() => {
      const start = startNum.value;
      return data.slice(start, start + showNumber);
    });

    // 滚动的时候计算当前视窗范围内第一个元素下标
    // 加个优化 节流
    const scrollEvent = (event: any) => {
      const { scrollTop } = event.target;
      startNum.value = parseInt(scrollTop / itemHeight + '');
      positionTop.value = scrollTop;
      console.log('positionTop', positionTop.value);
      console.log('startNum', startNum.value);
    };
    // 防抖的结果
    const debouncedFn = useDebounceFn(scrollEvent, 100);

    // 节流的结果
    const throttleFn = useThrottleFn(scrollEvent, 100);
    onMounted(() => {
      demo.value?.addEventListener('scroll', throttleFn);
    });
    onUnmounted(() => {
      if (!demo.value) return;
      demo.value?.removeEventListener('scroll', throttleFn);
      demo.value = null;
    });

    return {
      showNumber,
      itemHeight,
      demo,
      positionTop,
      data,
      activeList,
    };
  },
});
</script>

<style>
.scroll-box {
  position: relative;
  overflow: auto;
  width: 400px;
  border: 1px solid rgb(0, 0, 0);
}
.scroll-data {
  position: absolute;
  background-color: aqua;
  width: 100%;
}
.scroll-item {
  height: 20px;
}
.scroll-item:hover {
  background: rgb(104, 111, 211);
  color: #fff;
}
</style>

```
[参考资料][https://juejin.cn/post/6844904183582162957#heading-3]

