# 基础项目的搭建

## 考虑点是库函数还是工具轮子还是组件


## 打包工具的选择
- Esbuild
- Rollup
等等待补充理论知识

## 包管理工具的流程
- 软连接就是相当于快捷方式也叫符号链接
[https://www.cnblogs.com/Naylor/p/7597869.html]

- 硬链接就相当于copy了几份文件而且给文件换了个名字
[https://www.cnblogs.com/Naylor/p/7597869.html]
- 依赖提升的存在[]

- Pnpm
- Yarn
- Npm

[package.json字段详解][https://www.jianshu.com/p/1368b6bdf132]
- npm yarn install 原理以及存在的问题[https://juejin.cn/post/6932046455733485575#heading-7]

- npm run 理论
- node require 机制[https://segmentfault.com/a/1190000016899617]
- 📚 node 的包加载机制(很重要)
- package.json 参数意义(类似pnpm的yaml文件意义)
- 以及怎么软连接找到exe文件去执行命令

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