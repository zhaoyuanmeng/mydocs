# 跨端调试
whistle + Vconsole.js

## whistle包 这是一个代理的东西


## 常用方案

- 移动端安装vconsole.js

```javascript

npm install vconsole

import VConsole from 'vconsole';

const isDebug = true;

// 本地开发调试注入vConsole
if (isDebug) {
  new VConsole();
}

```
---

[参考资料](https://juejin.cn/post/7077385311642189832)
[参考资料](https://juejin.cn/post/7095537562378305566#heading-0)