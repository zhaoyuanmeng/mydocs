# 基础项目的搭建

## 考虑点是库函数还是工具轮子还是组件


## 打包工具的选择
- Esbuild
- Rollup

## 包管理工具的流程
- 软连接就是相当于快捷方式也叫符号链接
[https://www.cnblogs.com/Naylor/p/7597869.html]

- 硬链接就相当于copy了几份文件而且给文件换了个名字
[https://www.cnblogs.com/Naylor/p/7597869.html]
- 依赖提升的存在[]

- Pnpm
- Yarn
- Npm

## package.json字段详解

> ESMoudule 能够引用 commonjs的包 ,commonjs不能引用esmoudle

- type字段
>1 type字段的产生用于定义package.json文件和该文件所在目录根目录中.js文件和无拓展名文件的处理方式。值为’moduel’则当作es模块处理；值为’commonjs’则被当作commonJs模块处理
>
>2目前node默认的是如果pacakage.json没有定义type字段，则按照commonJs规范处理
>
>3node官方建议包的开发者明确指定package.json中type字段的值
>
>4无论package.json中的type字段为何值，.mjs的文件都按照es模块来处理，.cjs的文件都按照commonJs模块来处理

---

- main字段
> 默认入口文件 而且默认是commonjs规范
>

- moudle字段
> 如果采用import 导入包的时候 会加载这个入口文件 ESMoudule
>

[参考资料](https://www.jianshu.com/p/0ad1e1a8c518)

---


- npm yarn install 原理以及存在的问题[https://juejin.cn/post/6932046455733485575#heading-7]

> npm install 回去package.json下载依赖,并且根据每一个依赖里面的bin目录是否存在 然后建立文件链接并在主目录下生成.bin目录映射到那个目录下面

- npm run 理论
- node require 机制[https://segmentfault.com/a/1190000016899617]
- 📚 node 的包加载机制(很重要)
- package.json 参数意义(类似pnpm的yaml文件意义)
- 以及怎么软连接找到exe文件去执行命令

- npm | yarn | pnpm create xxx的机制作用
 - create-xxx包全局安装，或者如果软件包已存在，则将软件包更新为最新版本
 - 运行位于bin入门工具包领域的可执行文件package.json，将其转发<args>给它

- 理论知识
[参考资料][https://juejin.cn/post/6932046455733485575]
## monorepo搭建的流程

需要考虑的问题:项目间依赖分析、依赖安装、构建流程、测试流程、CI 及发布流程等诸多工程环节，同时还要考虑项目规模到达一定程度后的性能问题，比如项目构建/测试时间过长需要进行增量构建/测试、按需执行 CI等等，在实现全面工程化能力的同时，也需要兼顾到性能问题。

Pnpm版

[参考资料]

Yarn+Lerna版

[参考资料][https://juejin.cn/post/7073285203728269343]

## 自动化部署的流程
- 与GitHub的对接(action)

## 最后发包