# canvas 技术

## canvas 含义

- Canvas 允许开发者通过 JS 在这个标签上绘制各种图案。
- Canvas 拥有多种绘制路径、矩形、圆形、字符以及图片的方法。
- Canvas 在某些情况下可以 “代替” 图片。
- Canvas 可用于动画、游戏、数据可视化、图片编辑器、实时视频处理等领域。
- canvas 和 svg 的区别

| Canvas                                              | SVG             |
| --------------------------------------------------- | --------------- |
| 用 JS 动态生成元素                                  | 用 XML 描述元素 |
| 位图                                                | 矢量图          |
| 不支持事件(所有的事件都会再一个整体的 canvasDOM 上) | 支持事件        |
| 数据发生变化需要重绘                                | 不需要重绘      |

结论是:如果展示的数据量大,一个数据就是一个元素

---

# 注意点

- 使用 js 获取 canvas 的宽高，此时返回的是 canvas 的默认值（300 150)
- 如果使用 css 修改 canvas 的宽高（比如本例变成 400px \* 400px），那宽度就由 300px 拉伸到 400px，高度由 150px 拉伸到 400px

# 绘制基本图形

# 渲染图片

- js 里加载图片

- DOM 里的图片拿到 canvas 里渲染。

## JS 版

步骤 - 创建 Image 对象 - 引入图片 - 等待图片加载完 - 通过 drawImage()方法渲染图片

```html
<canvas
  id="c"
  width="500"
  height="500"
  style="border: 1px solid #ccc;"
></canvas>

<script>
  const cnv = document.getElementById("c");
  const cxt = cnv.getContext("2d");

  // 1 创建 Image 对象
  const image = new Image();

  // 2 引入图片
  image.src = "./images/dog.jpg";

  // 3 等待图片加载完成
  image.onload = () => {
    // 4 使用 drawImage() 方法渲染图片
    // 参数的意义  1：要渲染的图片对象  2：图片左上角的横坐标位置 3：图片左上角的纵坐标位置 4:图片的width 5:图片的height
    cxt.drawImage(image, 30, 30);
  };
</script>
```

---

## DOM 版

```html
<style>
  #dogImg {
    display: none;
  }
</style>

<img src="./images/dog.jpg" id="dogImg" />
<canvas
  id="c"
  width="500"
  height="500"
  style="border: 1px solid #ccc;"
></canvas>

<script>
  const cnv = document.getElementById("c");
  const cxt = cnv.getContext("2d");
  // 通过dom的方式
  window.onload = () => {
    const image = document.getElementById("dogImg");
    cxt.drawImage(image, 70, 70);
  };
</script>
```

---

- 事件系统/渲染系统

理论代码看下面的 ycanvas

- hooks 函数

理论代码看下面的 ycanvasuse

[🔥 ycanvas](git@github.com:zhaoyuanmeng/ycanvas.git)

[🎡 ycanvasuse](git@github.com:zhaoyuanmeng/ycanvasuse.git)

[👑 参考资料-事件系统](https://juejin.cn/post/6996811170459942925)
