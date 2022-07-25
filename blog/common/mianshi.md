# 面试题



## 浏览器渲染原理

首先我们打开一个网页的时候,浏览器会去请求对应的html文件,然后通过将字节数据转化为字符串然后再通过编译原理的词法分析转化为
Token(标记),最后这些标记会转化为Node,然后这些Node会通过之间的联系转为DOM树.<font color="red">这个过程就是HTML文件转化为DOM树</font>

---

再解析HTML的时候,也会遇到CSS和JS文件,这时候浏览器也会去下载并且解析这些文件

---

从CSS到CSSDOM的过程与上面的过程也类似.

---

当我们生成DOM树和CSSDOM树以后,就需要把这两颗树组合为渲染树.当浏览器生成渲染树以后，就会根据渲染树来进行布局（也可以叫做回流），然后调用 GPU 绘制，合成图层，显示在屏幕上。

![图片](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a71c46b79d364655ba35b9b87f6d133b~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)


## 引出的问题

-  ### 为什么操作DOM会慢

因为 DOM 是属于渲染引擎中的东西，而 JS 又是 JS 引擎中的东西。当我们通过 JS 操作 DOM 的时候，其实这个操作涉及到了两个线程之间的通信，那么势必会带来一些性能上的损耗。操作 DOM 次数一多，也就等同于一直在进行线程之间的通信，并且操作 DOM 可能还会带来重绘回流的情况，所以也就导致了性能上的问题。

--- 


- ### 如何解决这些问题(插入几万个 DOM，如何实现页面不卡顿？)

采用虚拟列表的知识,这种技术的原理就是只渲染可视区域内的内容，非可见区域的那就完全不渲染了，当用户在滚动的时候就实时去替换渲染的内容。



-  ### 什么情况阻塞渲染

首先渲染的前提是生成渲染树,所以HTML和CSS肯定会阻塞渲染,所以如果想要渲染的快,就需要一开始降低渲染的文件大小,并且扁平层级,优化选择器.

<font color="red">当浏览器解析到script标签的时候,会暂停构建DOM,然后去下载JS文件,下载完去执行(如果标签里面没有defer属性的话),因此都建议script标签放
在底部.(当 script 标签加上 defer 属性以后，表示该 JS 文件会并行下载，但是会放到 HTML 解析完成后顺序执行)</font>


- ### 重绘和回流

* 重绘是当节点需要更改外观而不会影响布局的，比如改变 color 就叫称为重绘

* 流是布局或者几何属性需要改变就称为回流。

* <font color="red">回流必定会发生重绘，重绘不一定会引发回流。回流所需的成本比重绘高的多，改变父节点里的子节点很可能会导致父节点的一系列回流。</font>


- ### 综合题(输入 URL 到页面渲染的整个流程)

  - 首先DNS域名查询

  - 接下来是 TCP 握手，建立数据连接,这时假设服务端会响应一个 HTML 文件。

  - 浏览器会判断状态码是什么，如果是 200 那就继续解析，如果 400 或 500 的话就会报错，如果 300 的话会进行重定向，这里会有个重定向计数器，避免过多次的重定向，超过次数也会报错。

  - 浏览器开始解析文件,文件解码成功后会正式开始渲染流程，先会根据 HTML 构建 DOM 树，有 CSS 的话会去构建 CSSOM 树。如果遇到 script 标签的话，会判断是否存在 async 或者 defer ，前者会并行进行下载并执行 JS，后者会先下载文件，然后等待 HTML 解析完成后顺序执行。

  - CSSOM 树和 DOM 树构建完成后会开始生成 Render 树，这一步就是确定页面元素的布局、样式等等诸多方面的东西
    在生成 Render 树的过程中，浏览器就开始调用 GPU 绘制，合成图层，将内容显示在屏幕上了。



--- 


## 事件机制

### 事件触发三阶段

* window往事件触发处传播,遇到注册的捕获事件会触发

* 传播到事件触发处时触发注册的事件

* 从事件触发处往window传播,遇到注册的冒泡事件会触发


### 注册事件

如果我们只希望事件只触发在目标上，这时候可以使用 stopPropagation 来阻止事件的进一步传播。通常我们认为 stopPropagation 是用来阻止事件冒泡的，其实该函数也可以阻止捕获事件。stopImmediatePropagation 同样也能实现阻止事件，但是还能阻止该事件目标执行别的注册事件。

```html
node.addEventListener(
  'click',
  event => {
    event.stopImmediatePropagation()
    console.log('冒泡')
  },
  false
)
// 点击 node 只会执行上面的函数，该函数不会执行
node.addEventListener(
  'click',
  event => {
    console.log('捕获 ')
  },
  true
)

```


### 事件代理

如果一个节点中的子节点是动态生成的，那么子节点需要注册事件的话应该注册在父节点上

```html
<ul id="ul">
	<li>1</li>
    <li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
</ul>
<script>
	let ul = document.querySelector('#ul')
	ul.addEventListener('click', (event) => {
		console.log(event.target);
	})
</script>

```
---

### 跨域

- 定义:因为浏览器出于安全考虑，有同源策略。也就是说，如果协议、域名或者端口有一个不同就是跨域，Ajax 请求会失败.
主要是用来防止 CSRF 攻击的。简单点说，CSRF 攻击是利用用户的登录态发起恶意请求。

- 思考:请求跨域了，那么请求到底发出去没有？

 请求必然是发出去了，但是浏览器拦截了响应。你可能会疑问明明通过表单的方式可以发起跨域请求，为什么 Ajax 就不会。因为归根结底，跨域是为了阻止用户读取到另一个域名下的内容，Ajax 可以获取响应，浏览器认为这不安全，所以拦截了响应。但是表单并不会获取新的内容，所以可以发起跨域请求。同时也说明了跨域并不能完全阻止 CSRF，因为请求毕竟是发出去了。


- 解决方式

  - JSONP

  JSONP 的原理很简单，就是利用 script 标签 img标签也可以,没有跨域限制的漏洞。通过script标签指向一个需要访问的地址并提供一个回调函数来接收数据当需要通讯时。

  ```javascript
    function jsonp(url, jsonpCallback, success) {
      let script = document.createElement('script')
      script.src = url
      script.async = true
      script.type = 'text/javascript'
      window[jsonpCallback] = function(data) {
        success && success(data)
      }
      document.body.appendChild(script)
    }
    jsonp('http://xxx', 'callback', function(value) {
      console.log(value)
    })
  ```

  - CORS

  CORS 需要浏览器和后端同时支持。IE 8 和 9 需要通过 XDomainRequest 来实现。浏览器会自动进行 CORS 通信，实现 CORS 通信的关键是后端。只要后端实现了 CORS，就实现了跨域。服务端设置 Access-Control-Allow-Origin 就可以开启 CORS。 该属性表示哪些域名可以访问资源，如果设置通配符则表示所有网站都可以访问资源。
