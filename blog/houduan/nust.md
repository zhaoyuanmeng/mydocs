# SSR

## 简介

> SSR 就是 服务器渲染，什么是 服务器渲染？由 服务器 组装好 DOM 元素，生成 HTML 字符串给到浏览器，也就是在浏览器里面是可以看到整个页面的 DOM 源码的

## 做 SSR 的方法

- 前端编写 原生静态页面，给到服务器，通过服务器框架进行组合数据，例如 php 的 Web 开发。

- Vue SSR

- Nuxt.js 使用 Webpack 和 Node.js 进行封装的基于 Vue 的 SSR 框架，不需要自己搭建一套 SSR 程序，而是通过其约定好的文件结构和 API 就可以实现一个首屏渲染的 Web 应用.

## 如何解决的跨域

> 其实就是利用服务端去加载数据并且渲染到页面中然后再把整体 html 输入给浏览器,因为服务端没有跨域
>
> 主要通过的 asyncData 的方法写的,固定写法

[官网](https://v3.nuxtjs.org/)

[参考资料](https://cloud.tencent.com/developer/news/221008)
