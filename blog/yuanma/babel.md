# 🚀 Babel源码

## babel 就是一个js转译器用于 es next、typescript等代码的转换，同时还暴露出了 api 让开发者可以进行特定用途的转换。

## 整体流程
为了让计算机理解代码需要先对源码字符串进行 parse，生成 AST，把对代码的修改转为对 AST 的增删改，转换完 AST 之后再打印成目标代码字符串。