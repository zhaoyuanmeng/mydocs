# canvas技术

## canvas含义
- Canvas 允许开发者通过 JS在这个标签上绘制各种图案。
- Canvas 拥有多种绘制路径、矩形、圆形、字符以及图片的方法。
- Canvas 在某些情况下可以 “代替” 图片。
- Canvas 可用于动画、游戏、数据可视化、图片编辑器、实时视频处理等领域。
- canvas 和 svg的区别

|  Canvas   | SVG  |
|  ----  | ----  |
| 用JS动态生成元素  | 用XML描述元素 |
| 位图  | 矢量图 |
| 不支持事件(所有的事件都会再一个整体的canvasDOM上)  | 支持事件 |
| 数据发生变化需要重绘  | 不需要重绘 |

结论是:如果展示的数据量大,一个数据就是一个元素

---







- 事件系统

- 业务(看上一个)

[代码](git@github.com:zhaoyuanmeng/ztcanvas.git)
[参考资料-事件系统](https://juejin.cn/post/6996811170459942925)