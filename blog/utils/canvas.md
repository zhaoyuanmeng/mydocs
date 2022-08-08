## canvas技术


- 基础



# 注意点
- 使用 js 获取 canvas 的宽高，此时返回的是 canvas 的默认值（300 150)
- 如果使用 css 修改 canvas 的宽高（比如本例变成 400px * 400px），那宽度就由 300px 拉伸到 400px，高度由 150px 拉伸到 400px



# 绘制基本图形



# 渲染图片

- js里加载图片

- DOM里的图片拿到 canvas 里渲染。


## JS版

步骤
    - 创建Image对象
    - 引入图片
    - 等待图片加载完
    - 通过drawImage()方法渲染图片

```html
<canvas id="c" width="500" height="500" style="border: 1px solid #ccc;"></canvas>

<script>
  const cnv = document.getElementById('c')
  const cxt = cnv.getContext('2d')

  // 1 创建 Image 对象
  const image = new Image()

  // 2 引入图片
  image.src = './images/dog.jpg'

  // 3 等待图片加载完成
  image.onload = () => {
    // 4 使用 drawImage() 方法渲染图片
    // 参数的意义  1：要渲染的图片对象  2：图片左上角的横坐标位置 3：图片左上角的纵坐标位置 4:图片的width 5:图片的height
    cxt.drawImage(image, 30, 30)
  }
</script>

```

---


## DOM版

```html
<style>
  #dogImg {
    display: none;
  }
</style>

<img src="./images/dog.jpg" id="dogImg"/>
<canvas id="c" width="500" height="500" style="border: 1px solid #ccc;"></canvas>

<script>
  const cnv = document.getElementById('c')
  const cxt = cnv.getContext('2d')

  const image = document.getElementById('dogImg')

  cxt.drawImage(image, 70, 70)
</script>




```
---





- 事件系统

- 业务(看上一个)

[代码](git@github.com:zhaoyuanmeng/ztcanvas.git)
[参考资料-事件系统](https://juejin.cn/post/6996811170459942925)