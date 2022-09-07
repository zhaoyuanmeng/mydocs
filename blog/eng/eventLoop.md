# 🤖 EventLoop 原理

## 🎵 async await 的表现

async 在 Event Loop 中，遇到 await 之前的代码，都属于同步代码，当遇到第一个 await 后，属于 aysnc 函数中的 await 后面的所有代码，都作为一个微任务，保存到任务队列中，当执行完成同步代码后，执行任务队列中的微任务。
当执行 await 后面的 微任务 代码块时，如果内部还有 await ，后面的代码再继续被添加到 微任务队列 中，由于 微任务 的特性，所有 微任务 都执行完成后，再去执行 宏任务，所以才会出现上面的情况。

```javascript
console.log(111);

setTimeout(() => {
  console.log(222);
}, 0);

(async () => {
  console.log(333);
  await console.log(444);
  console.log(555);
})().then(() => {
  console.log(666);
});

console.log(777);

// 执行结果
// 111
// 333
// 444
// 777
// 555
// 666
// 222
```

## 🎹 有嵌套的情况

别管啥 就遵循 主 微 宏 的原则 如果嵌套了也要遵循这个原则

```javascript
console.log("start");
setTimeout(() => {
  console.log("timer1");
  Promise.resolve().then(function () {
    console.log("promise1");
  });
}, 0);
setTimeout(() => {
  console.log("timer2");
  Promise.resolve().then(function () {
    console.log("promise2");
  });
}, 0);
Promise.resolve().then(function () {
  console.log("promise3");
});
console.log("end");
// start
// end
// promise3
// timer1
// promise1
// timer2
// promise2
```

## promise(重学)

## async await(重学)
