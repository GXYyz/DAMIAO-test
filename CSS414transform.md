# 2D transform

- transform: function(value);

  - transform 总是在布局之后渲染
  - transform 里的多个函数会同步执行
  - 在不设置 transform-origin 的情况下,元素的数轴以左上角为原点
    X 轴向右,Y 轴向下
  - transform 中的百分比以元素自身可见区域大小为基准

- 当多个选择器中的 transform 指向同一个元素
  高优先级选择器的 transform 会完全覆盖比它优先级第的选择器的 transform
  属性值无法同时生效

- rotate() 旋转函数
  其中可以设置 圈数单位 turn 角度单位 deg 弧度单位 rad
  正值顺时针旋转,负值逆时针旋转
  旋转会导致元素的 xy 轴方位也随之发生变化
  在不设置 transform-origin 的情况下,旋转中心为元素左上角

- transform-origin 设置元素的动画中心点
  其中可以设置数值、百分比和 left right top bottom center 关键字

  - 设置两个值的时候 前一个为水平中点,后一个为垂直中点
    设置一个值的时候为水平中点

- translate() 平移函数
  可以拆分为 translateX 和 translateY 两个函数
  在同一个属性中 x 轴方向移动数值在前 y 轴方向移动数值在后

- scale() 缩放函数
  可以拆分为 scaleX 和 scaleY 两个函数 在 X 或 Y 轴方向上单独缩放
  在同一个属性中 XY 轴等比缩放

  - 当缩放数值大于 0 时,正常缩放
    但当缩放数值小于 0 时,会出现类似于翻转缩放的效果

- skew() 倾斜函数
  可以拆分为 skewX 和 skewY 两个函数,在 XY 轴方向上分别倾斜

* transform 属性动画变化时,如果属性值的变换不是一一对应的
  如:
  div {
  transition: 2s;
  transform: translate(0,0) rotate(110deg);
  }
  div:hover {
  transform: rotate(1510deg);
  }
  浏览器会只计算出动画的起点坐标和终点坐标,忽略中间的变化过程,以最简略的方式完成动画

  如果属性值是一一对应的,浏览器才会将中间变化过程完整渲染出来
