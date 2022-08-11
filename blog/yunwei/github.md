# GitHub


## GitHub Action 


### WorkFlow基本概念

- Event(触发事件):触发workflow运行的事件

- Job(作业):一个工作流程包含一个或者多个Job,默认job是并行处理但是我们也可以通过设置让其顺序执行,每一个job都在指定的环境里面开启一个runner去执行,包含多个step

- step:job的组成部分,用于定义每一部的工作内容.每个step在运行器环境中以单独的进程运行,而且可以访问工作区和文件系统.

![图片](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/27deef91333747ab8f7e09bd2649b5bb~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)



步骤:

>第一步:在根目录里新建.github/workflows目录,然后通过编写yml格式文件定义Workflows(工作流程)去实现CI
>
>
>

