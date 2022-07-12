# vite插件

## 从0开发一个vite插件


## 前置知识(vite独有的钩子)
- enforce ：值可以是pre 或 post ， pre 会较于 post 先执行；
- apply ：值可以是 build 或 serve  亦可以是一个函数，指明它们仅在 build 或 serve 模式时调用；
- config(config, env) ：可以在 vite 被解析之前修改 vite 的相关配置。钩子接收原始用户配置 config 和一个描述配置环境的变量env；
- configResolved(resolvedConfig) ：在解析 vite 配置后调用。使用这个钩子读取和存储最终解析的配置。当插件需要根据运行的命令做一些不同的事情时，它很有用。
- configureServer(server) ：主要用来配置开发服务器，为 dev-server (connect 应用程序) 添加自定义的中间件；
- transformIndexHtml(html) ：转换 index.html 的专用钩子。钩子接收当前的 HTML 字符串和转换上下文；
- handleHotUpdate(ctx)：执行自定义HMR更新，可以通过ws往客户端发送自定义的事件；



📔参考资料: [https://juejin.cn/post/7103165205483356168](https://juejin.cn/post/7103165205483356168)

😀代码地址: [https://github.com/zhaoyuanmeng/my-vite-plugin](https://github.com/zhaoyuanmeng/my-vite-plugin)
