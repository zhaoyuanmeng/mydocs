# 前端监控埋点

## 目的

- 开发出来的应用的使用情况：有没有用户使用，有多少用户使用；
- 用户在使用过程中遇到了什么样的问题；
- 作为开发者和运营者应该如何追踪定位到这些问题并及时解决；
- 同时从中吸取经验避免再犯；
- 埋点数据反哺业务：运营和产品负责人能从中得到哪些东西，从而优化产品质量。

![图片](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/18/17225207a5f67b64~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

## 理论支撑

前端监控分为性能监控和错误监控。其中监控又分为两个环节：数据采集和数据上报。

### 性能数据采集

- window.performance API
- 里面参数的具体意义

```javascript
timing: {
    // 同一个浏览器上一个页面卸载(unload)结束时的时间戳。如果没有上一个页面，这个值会和fetchStart相同。
	navigationStart: 1543806782096,

	// 上一个页面unload事件抛出时的时间戳。如果没有上一个页面，这个值会返回0。
	unloadEventStart: 1543806782523,

	// 和 unloadEventStart 相对应，unload事件处理完成时的时间戳。如果没有上一个页面,这个值会返回0。
	unloadEventEnd: 1543806782523,

	// 第一个HTTP重定向开始时的时间戳。如果没有重定向，或者重定向中的一个不同源，这个值会返回0。
	redirectStart: 0,

	// 最后一个HTTP重定向完成时（也就是说是HTTP响应的最后一个比特直接被收到的时间）的时间戳。
	// 如果没有重定向，或者重定向中的一个不同源，这个值会返回0.
	redirectEnd: 0,

	// 浏览器准备好使用HTTP请求来获取(fetch)文档的时间戳。这个时间点会在检查任何应用缓存之前。
	fetchStart: 1543806782096,

	// DNS 域名查询开始的UNIX时间戳。
        //如果使用了持续连接(persistent connection)，或者这个信息存储到了缓存或者本地资源上，这个值将和fetchStart一致。
	domainLookupStart: 1543806782096,

	// DNS 域名查询完成的时间.
	//如果使用了本地缓存（即无 DNS 查询）或持久连接，则与 fetchStart 值相等
	domainLookupEnd: 1543806782096,

	// HTTP（TCP） 域名查询结束的时间戳。
        //如果使用了持续连接(persistent connection)，或者这个信息存储到了缓存或者本地资源上，这个值将和 fetchStart一致。
	connectStart: 1543806782099,

	// HTTP（TCP） 返回浏览器与服务器之间的连接建立时的时间戳。
        // 如果建立的是持久连接，则返回值等同于fetchStart属性的值。连接建立指的是所有握手和认证过程全部结束。
	connectEnd: 1543806782227,

	// HTTPS 返回浏览器与服务器开始安全链接的握手时的时间戳。如果当前网页不要求安全连接，则返回0。
	secureConnectionStart: 1543806782162,

	// 返回浏览器向服务器发出HTTP请求时（或开始读取本地缓存时）的时间戳。
	requestStart: 1543806782241,

	// 返回浏览器从服务器收到（或从本地缓存读取）第一个字节时的时间戳。
        //如果传输层在开始请求之后失败并且连接被重开，该属性将会被数制成新的请求的相对应的发起时间。
	responseStart: 1543806782516,

	// 返回浏览器从服务器收到（或从本地缓存读取，或从本地资源读取）最后一个字节时
        //（如果在此之前HTTP连接已经关闭，则返回关闭时）的时间戳。
	responseEnd: 1543806782537,

	// 当前网页DOM结构开始解析时（即Document.readyState属性变为“loading”、相应的 readystatechange事件触发时）的时间戳。
	domLoading: 1543806782573,

	// 当前网页DOM结构结束解析、开始加载内嵌资源时（即Document.readyState属性变为“interactive”、相应的readystatechange事件触发时）的时间戳。
	domInteractive: 1543806783203,

	// 当解析器发送DOMContentLoaded 事件，即所有需要被执行的脚本已经被解析时的时间戳。
	domContentLoadedEventStart: 1543806783203,

	// 当所有需要立即执行的脚本已经被执行（不论执行顺序）时的时间戳。
	domContentLoadedEventEnd: 1543806783216,

	// 当前文档解析完成，即Document.readyState 变为 'complete'且相对应的readystatechange 被触发时的时间戳
	domComplete: 1543806783796,

	// load事件被发送时的时间戳。如果这个事件还未被发送，它的值将会是0。
	loadEventStart: 1543806783796,

	// 当load事件结束，即加载事件完成时的时间戳。如果这个事件还未被发送，或者尚未完成，它的值将会是0.
	loadEventEnd: 1543806783802
}

```

- 对我们有用的参数

```javascript
// 重定向耗时
redirect: timing.redirectEnd - timing.redirectStart,
// DOM 渲染耗时
dom: timing.domComplete - timing.domLoading,
// 页面加载耗时
load: timing.loadEventEnd - timing.navigationStart,
// 页面卸载耗时
unload: timing.unloadEventEnd - timing.unloadEventStart,
// 请求耗时
request: timing.responseEnd - timing.requestStart,
// 获取性能信息时当前时间
time: new Date().getTime(),

```

- 白屏时间的获取

```javascript
// 将以下脚本放在 </head> 前面就能获取白屏时间。
whiteScreen = new Date() - performance.timing.navigationStart;
// 通过 domLoading 和 navigationStart 也可以
whiteScreen =
  performance.timing.domLoading - performance.timing.navigationStart;
```

---

### 错误数据采集

- 资源加载错误

通过 addEventListener('error', callback, true) 在捕获阶段捕捉资源加载失败错误。

- js 执行错误

通过 window.onerror 捕捉 js 错误。

- promise 错误

通过 addEventListener('unhandledrejection', callback)捕捉 promise 错误，但是没有发生错误的行数，列数等信息，只能手动抛出相关错误信息。

- 封装好的操作

```javascript
// 捕获资源加载失败错误 js css img...
addEventListener(
  "error",
  (e) => {
    const target = e.target;
    if (target != window) {
      monitor.errors.push({
        type: target.localName,
        url: target.src || target.href,
        msg: (target.src || target.href) + " is load error",
        // 错误发生的时间
        time: new Date().getTime(),
      });
    }
  },
  true
);

// 监听 js 错误
window.onerror = function (msg, url, row, col, error) {
  monitor.errors.push({
    type: "javascript",
    row: row,
    col: col,
    msg: error && error.stack ? error.stack : msg,
    url: url,
    // 错误发生的时间
    time: new Date().getTime(),
  });
};

// 监听 promise 错误 缺点是获取不到行数数据
addEventListener("unhandledrejection", (e) => {
  monitor.errors.push({
    type: "promise",
    msg: (e.reason && e.reason.msg) || e.reason || "",
    // 错误发生的时间
    time: new Date().getTime(),
  });
});
```

### 用户数据采集

- Navigator

使用 window.navigator 可以收集到用户的设备信息，操作系统，浏览器信息...

- UV（Unique visitor）

指通过互联网浏览这个网页的访客，00:00-24:00 内相同的设备访问只被计算一次。一天内同个访客多次访问仅计算一个 UV。
在用户访问网站时，可以生成一个随机字符串+时间日期，保存在本地。在网页发生请求时（如果超过当天 24 小时，则重新生成），把这些参数传到后端，后端利用这些信息生成 UV 统计报告。

- PV（Page View）

页面浏览量或点击量，用户每 1 次对网站中的每个网页访问均被记录 1 个 PV。用户对同一页面的多次访问，访问量累计，用以衡量网站用户访问的网页数量。

- 页面停留时间

  - SPA 单页面可以利用 router 来获取用户停留时间，拿 Vue 举例，通过 router.beforeEach、destroyed 这两个钩子函数来获取用户停留该路由组件的时间。

  - 传统网站,用户在进入 A 页面时，通过后台请求把用户进入页面的时间捎上。过了 10 分钟，用户进入 B 页面，这时后台可以通过接口捎带的参数可以判断出用户在 A 页面停留了 10 分钟。

- 页面跳转来源

通过 document.referrer 属性，可以知道用户是从哪个网站跳转而来。

### 性能数据上报

使用 sendBeacon() 方法会使用户代理（浏览器）在有机会时异步地向服务器发送数据，同时不会延迟页面的卸载或影响下一导航的载入性能。这就解决了提交分析数据时的所有的问题：数据可靠，传输异步并且不会影响下一页面的加载。

## 实践部分

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <script>
      function monitorInit() {
        const monitor = {
          // 数据上传地址
          url: "",
          // 性能信息
          performance: {},
          // 资源信息
          resources: {},
          // 错误信息
          errors: [],
          // 用户信息
          user: {
            // 屏幕宽度
            screen: screen.width,
            // 屏幕高度
            height: screen.height,
            // 浏览器平台
            platform: navigator.platform,
            // 浏览器的用户代理信息
            userAgent: navigator.userAgent,
            // 浏览器用户界面的语言
            language: navigator.language,
          },
          // 手动添加错误
          addError(error) {
            const obj = {};
            const { type, msg, url, row, col } = error;
            if (type) obj.type = type;
            if (msg) obj.msg = msg;
            if (url) obj.url = url;
            if (row) obj.row = row;
            if (col) obj.col = col;
            obj.time = new Date().getTime();
            monitor.errors.push(obj);
          },
          // 重置 monitor 对象
          reset() {
            window.performance && window.performance.clearResourceTimings();
            monitor.performance = getPerformance();
            monitor.resources = getResources();
            monitor.errors = [];
          },
          // 清空 error 信息
          clearError() {
            monitor.errors = [];
          },
          // 上传监控数据
          upload() {
            // 自定义上传
            // axios.post({
            //     url: monitor.url,
            //     data: {
            //         performance,
            //         resources,
            //         errors,
            //         user,
            //     }
            // })
          },
          // 设置数据上传地址
          setURL(url) {
            monitor.url = url;
          },
        };

        // 获取性能信息
        const getPerformance = () => {
          if (!window.performance) return;
          const timing = window.performance.timing;
          const performance = {
            // 重定向耗时
            redirect: timing.redirectEnd - timing.redirectStart,
            // 白屏时间
            whiteScreen: whiteScreen,
            // DOM 渲染耗时
            dom: timing.domComplete - timing.domLoading,
            // 页面加载耗时
            load: timing.loadEventEnd - timing.navigationStart,
            // 页面卸载耗时
            unload: timing.unloadEventEnd - timing.unloadEventStart,
            // 请求耗时
            request: timing.responseEnd - timing.requestStart,
            // 获取性能信息时当前时间
            time: new Date().getTime(),
          };

          return performance;
        };

        // 获取资源信息
        const getResources = () => {
          if (!window.performance) return;
          const data = window.performance.getEntriesByType("resource");
          const resource = {
            xmlhttprequest: [],
            css: [],
            other: [],
            script: [],
            img: [],
            link: [],
            fetch: [],
            // 获取资源信息时当前时间
            time: new Date().getTime(),
          };

          data.forEach((item) => {
            const arry = resource[item.initiatorType];
            arry &&
              arry.push({
                // 资源的名称
                name: item.name,
                // 资源加载耗时
                duration: item.duration.toFixed(2),
                // 资源大小
                size: item.transferSize,
                // 资源所用协议
                protocol: item.nextHopProtocol,
              });
          });

          return resource;
        };

        window.onload = () => {
          // 在浏览器空闲时间获取性能及资源信息 https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestIdleCallback
          if (window.requestIdleCallback) {
            window.requestIdleCallback(() => {
              monitor.performance = getPerformance();
              monitor.resources = getResources();
              console.log("页面性能信息");
              console.log(monitor.performance);
              console.log("页面资源信息");
              console.log(monitor.resources);
            });
          } else {
            setTimeout(() => {
              monitor.performance = getPerformance();
              monitor.resources = getResources();
              console.log("页面性能信息");
              console.log(monitor.performance);
              console.log("页面资源信息");
              console.log(monitor.resources);
            }, 0);
          }
        };

        // 捕获资源加载失败错误 js css img...
        addEventListener(
          "error",
          (e) => {
            const target = e.target;
            if (target != window) {
              monitor.errors.push({
                type: target.localName,
                url: target.src || target.href,
                msg: (target.src || target.href) + " is load error",
                // 错误发生的时间
                time: new Date().getTime(),
              });

              console.log("所有的错误信息");
              console.log(monitor.errors);
            }
          },
          true
        );

        // 监听 js 错误
        window.onerror = function (msg, url, row, col, error) {
          monitor.errors.push({
            type: "javascript", // 错误类型
            row: row, // 发生错误时的代码行数
            col: col, // 发生错误时的代码列数
            msg: error && error.stack ? error.stack : msg, // 错误信息
            url: url, // 错误文件
            time: new Date().getTime(), // 错误发生的时间
          });

          console.log("所有的错误信息");
          console.log(monitor.errors);
        };

        // 监听 promise 错误 缺点是获取不到行数数据
        addEventListener("unhandledrejection", (e) => {
          monitor.errors.push({
            type: "promise",
            msg: (e.reason && e.reason.msg) || e.reason || "",
            // 错误发生的时间
            time: new Date().getTime(),
          });

          console.log("所有的错误信息");
          console.log(monitor.errors);
        });

        return monitor;
      }

      const monitor = monitorInit();
    </script>
    <link rel="stylesheet" href="test.css" />
    <title>Document</title>
  </head>
  <body>
    <button class="btn1">错误测试按钮1</button>
    <button class="btn2">错误测试按钮2</button>
    <button class="btn3">错误测试按钮3</button>
    <img
      src="https://avatars3.githubusercontent.com/u/22117876?s=460&v=4"
      alt=""
    />
    <img src="test.png" alt="" />
    <script src="192.168.10.15/test.js"></script>
    <script>
      document.querySelector(".btn1").onclick = () => {
        setTimeout(() => {
          console.log(button);
        }, 0);
      };

      document.querySelector(".btn2").onclick = () => {
        new Promise((resolve, reject) => {
          reject({
            msg: "test.js promise is error",
          });
        });
      };

      document.querySelector(".btn3").onclick = () => {
        throw "这是一个手动扔出的错误";
      };
    </script>
  </body>
</html>
```

[✨ 参考资料 1](https://juejin.cn/post/6844904161562066958#comment)

[✨ 参考资料 2](https://juejin.cn/post/6938075086737899534#heading-10)

[✨ 参考资料 3](https://juejin.cn/post/6899430989404045320#heading-20)

[🎃 完整代码](https://github.com/zhaoyuanmeng/monitor)
