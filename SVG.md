# SVG

SVG Scalable Vector Graphics 可缩放矢量图

## SVG 文件的基本属性

- SVG 全局有效的规则是"后来居上"
- HTML5 可以直接嵌入 SVG 代码
- 可以通过 iframe/img/object/embed 等元素引用 SVG 文件

```html
<iframe src="image.svg"></iframe>
```

## 语法

- svg 中绝对数值的单位默认为 px

- <svg>标签
  如果 svg 不是根元素,svg 元素可以用于在当前文档内嵌套一个独立的 svg 片段,这个独立片段拥有独立的视口和坐标.即坐标系原点在 svg 盒子的左上角

  - **widt** 和 **height** 属性,指定了 SVG 盒子在 HTML 元素中占据的宽度和高度.默认 300px\*150px
  - 如果只想展示 SVG 图像的一部分,需要指定**viewBox**属性
    - viewBox="左上角 x 坐标 左上角 y 坐标 视口高度 视口宽度"
    - 注意:视口大小必须匹配 SVG 的大小,否则视口会放大或缩小区域内的 SVG 图像以适配 SVG
    - 如果不指定 width 和 height 属性,只指定 viewBox 属性,则相当于只给定了 SVG 图像的长宽比;此时 SVG 图像的大小将等于 HTML 元素的大小

- <g>标签 可以将一系列的形状和样式组成一个**集合**,可以为集合内的元素写共同的样式,或是将整个集合引用使用

### 基本形状

- <rect>矩形标签

  - x 属性 矩形左上角的 x 坐标
    y 属性 矩形左上角的 y 坐标
    width 属性 矩形的宽度
    height 属性 矩形的高度
    rx 属性 矩形圆角的 x 方位的半径
    ry 属性 矩形圆角的 y 方位的半径

- <circle>圆形标签

  - r 属性 圆的半径
    cx 属性 圆心的 x 坐标
    cy 属性 圆心的 y 坐标

- <ellipse>椭圆标签

  - rx 椭圆的 x 半径
    ry 椭圆的 y 半径
    cx 椭圆中心的 x 位置
    cy 椭圆中心的 y 位置

- <line>直线标签

  - x1 起点的 x 位置
    y1 起点的 y 位置
    x2 终点的 x 位置
    y2 终点的 y 位置

- <polyline>折线标签
  折现的所有点位置都放在一个**points**属性中

  - points
    点集数列。每个数字用空白、逗号、终止命令符或者换行符分隔开。每个点必须包含 2 个数字，一个是 x 坐标，一个是 y 坐标。所以点列表 (0,0), (1,1) 和 (2,2) 可以写成这样：“0 0, 1 1, 2 2”。

- <polygon>多边形标签
  和折线很像,他们都由很多点构成,只是最后一个点会和第一个点闭合起来.所以同样使用**points**属性来设置

### <path>路径标签

path 元素的形状是通过属性**d**定义的,其值是一个"命令+参数"的序列

- 直线命令

  - M move to 移动画笔到指定坐标
  - L line to 划线到指定坐标
  - H horizontal line to 绘制水平线到指定坐标(只有 x 方位一个值)
  - V vertical line to 绘制垂直线到指定坐标(只有 y 方位一个值)
  - Z close path 闭合路径(放到路径的最后)

- 曲线命令

  - C curve to 三次贝塞尔曲线
    三次贝塞尔曲线需要定义一个终点和两个控制点,所以需要三组坐标参数
    C x1 y1, x2 y2, x y
    这里的最后一个坐标 (x,y) 表示的是曲线的终点，另外两个坐标是控制点，(x1,y1) 是起点的控制点，(x2,y2) 是终点的控制点。
    - 当一个点起点的斜率和前一个点终点的斜率相同,可以使用**S**命令来省略起点控制点坐标
      S x2 y2, x y
      如果 S 命令跟在一个 C 或 S 命令后面，则它的第一个控制点会被假设成前一个命令曲线的第二个控制点的中心对称点。如果 S 命令单独使用，前面没有 C 或 S 命令，那当前点将作为第一个控制点。
  - Q quadratic Bézier curve 二次贝塞尔曲线
    起点和终点斜率相同的话,只需要两组参数,一个控制点
    Q x1 y1, x y
    - 就像三次贝塞尔曲线有一个 S 命令，二次贝塞尔曲线有一个差不多的**T**命令，只需要写终点坐标,就可以用相同斜率延长二次贝塞尔曲线

- 弧形命令
  - A elliptical Arc
    A rx ry x-axis-rotation large-arc-flag sweep-flag x y
    - 弧形命令 A 的前两个参数分别是 x 轴半径和 y 轴半径
    - x-axis-rotation 是椭圆相对于坐标系的旋转角度
    - large-arc-flag（角度大小）和 sweep-flag（弧线方向），large-arc-flag 决定弧线是大于还是小于 180 度，0 表示小角度弧，1 表示大角度弧。sweep-flag 表示弧线的方向，0 表示从起点到终点沿逆时针画弧，1 表示从起点到终点沿顺时针画弧。

### 填充和边框

- fill 设置对象内部的颜色
  fill-opacity 控制填充色透明度

- stroke 设置绘制对象的线条的颜色

  - stroke-opacity 控制描边的透明度

  - stroke-linecap 控制描边的方式(butt/square/round)
    ![描边方式](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Fills_and_Strokes/svg_stroke_linecap_example.png)

  - stroke-width 控制描边宽度

  - stroke-linejoin 控制两条描边线段之间，用什么方式连接(miter/round/bevel)
    ![连接方式](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Fills_and_Strokes/svg_stroke_linejoin_example.png)

    - 每条折线都是由两个线段连接起来的，连接处的样式也由 stroke-linejoin 属性控制

  - stroke-dasharray 设置描边为虚线
    - stroke-dasharray 属性的参数，是一组用逗号分割的数字组成的数列。注意，和 path 不一样，这里的数字必须用逗号分割（空格会被忽略）。每一组数字，第一个用来表示填色区域的长度，第二个用来表示非填色区域的长度。
    - 如果提供了奇数个值，则这个值的数列重复一次，从而变成偶数个值。例如:5,3,2 等同于 5,3,2,5,3,2。

- 除了定义对象的属性外，你也可以通过 CSS 来样式化填充和描边。语法和在 HTML 里使用 CSS 一样，只不过你要把 background-color、border 改成 fill 和 stroke
  - SVG 元素也可以使用 style 插入行内样式
  - 通常 svg 的 style 会放在**defs**元素中
    <defs>里面可以定义一些不会在 SVG 图形中出现,但是可以被其他元素使用的元素
    当然,defs 中也可以放入图形

### 渐变

渐变元素通常会创建在 defs 元素内部,设置 id 值,通过 fill/stroke="url(#id)"来使用

#### 线性渐变

- <linearGradient>标签用于创建线性渐变节点
  其中需要设置两个坐标点来定义渐变的走向,坐标点 x1 y1 x2 y2 需要分别设置(默认为水平方向)(具体规则没搞明白)
  - <stop>节点 写在<linearGradient></linearGradient>中间
    - 通过 offset(偏移)和 stop-color(颜色中值)来说明在渐变的特定位置上应该是什么颜色
    - offset 应该始终从 0/0%开始,到 1/100%结束
    - 也可以通过 stop-opacity 来设置某个位置的半透明度
  ```xml
  <linearGradient id="Gradient2" x1="0" y1="1" x2="0"  y2="0">
    <stop offset="0%" stop-color="red"/>
    <stop offset="100%" stop-color="blue"/>
  </linearGradient>
  ```
  上述代码设置了从蓝到红的垂直渐变

#### 径向渐变(从某个中心发散的渐变)

- <radialGradient>标签用于创建径向渐变
  其中需要设置两个坐标点和一个半径来定义渐变的走向

  - 第一个点定义了渐变结束所围绕的圆环，它需要一个中心点，由 cx 和 cy 属性及半径 r 来定义,这个点规定了渐变的圆形范围
  - 第二个点被称为焦点，由 fx 和 fy 属性定义。焦点则描述了渐变的中心(默认范围圆心)
  - 注意:如果焦点在渐变范围外,渐变将不能正确呈现
  - <radialGradient>标签还可以使用**spreadMethod**属性设置渐变范围外是否重复渐变及重复规律
    - pad 默认值外围无渐变
    - reflect 会让渐变一直持续下去，不过它的效果是与渐变本身是相反的
    - repeat 也会让渐变继续，但是它不会像 reflect 那样反向渐变，而是跳回到最初的颜色然后继续渐变
  - <radialGradient>依然使用<stop>节点来设置每个渐变阶段的颜色

  ```xml
  <radialGradient id="RadialGradient2" cx="0.25" cy="0.25" r="0.25">
    <stop offset="0%" stop-color="red"/>
    <stop offset="100%" stop-color="blue"/>
  </radialGradient>
  ```

- 两种渐变都有一个叫做 gradientUnits（渐变单元）的属性，它描述了用来描述渐变的大小和方向的单元系统。该属性有两个值：**userSpaceOnUse 、objectBoundingBox。默认值为 objectBoundingBox，我们目前看到的效果都是在这种系统下的，它大体上定义了对象的渐变大小范围，所以你只要指定从 0 到 1 的坐标值，渐变就会自动的缩放到对象相同大小。**userSpaceOnUse 使用绝对单元，所以你必须知道对象的位置，并将渐变放在同样地位置上。

### 图案 patterns

用于将一组 SVG 平铺在其他 SVG 内部,类似于 CSS 的 background-img
图案元素通常也放在 defs 内部

- 在 <pattern></pattern> 标签内部可以包含其他基本形状
  - 其内可以设置 width 和 height 属性，规定每个元素重复图案占盒子的百分之多少
  - 如果你想要在绘制时偏移矩形的开始点，也可以使用 x 和 y 属性
  - 没搞明白

### 文本 text

- <text>元素内部可以放任何文字
  - 其中可以设置 xy 属性来决定文本在视口中显示的位置
  - 设置 text-anchor 属性,决定文本流的方向 (start、middle、end 或 inherit)
  - 也可以使用 fill 和 stroke 属性

#### 基础变形

- svg 元素同样可以使用 transform 进行 2D 变换,且语法与 CSS 的一致

### 剪切和遮罩

#### Clipping

用来移除在别处定义的元素的部分内容。在这里，任何半透明效果都是不行的。它只能要么显示要么不显示。

- <clipPath>标签用于创建一个剪切区域
  - <clipPath></clipPath>中可以放置任何的 SVG 形状,用于规定剪切范围
  - clipPath 元素通常放在一个 defs 元素内
  - 使用 clip-path="url()" 属性将剪切区域引入任何一个 SVG 元素
  - 注意 clipPath 定义的范围是保留的区域,其他地方将不会显示

#### Masking

允许使用透明度和灰度值遮罩计算得的软边缘

- <mask>标签用于创建一个遮罩区域
  - <mask></mask>中可以放置任何的 SVG 形状,用于规定遮罩范围
  - 使用 mask="url()"属性将遮罩引入任何一个 SVG 元素
  - 注意引入了 mask 的元素将作为遮罩渲染

## 动画

- 动画标签写在需要动画效果的元素标签内
- 同一个元素标签可以有多个动画效果
- 如果多个元素需要组合在一起渲染动画效果
  - 将元素嵌套景<g></g>标签中
  - 在最后一个元素后,<g/>标签前写入动画

#### 动画标签

- <animate> 普通动画
  用于为 SVG 元素的属性设置动画,如位置,大小等
- <animateTransform> 变换动画
  用于为 SVG 元素的 transform 属性设置动画
- <animateMotion> 运动动画
  SVG 元素可以沿设置好的路径运动,同时可以使元素旋转以匹配路径的斜率

#### 动画属性

- transform-origin 设置动画的变化原点
  注意:该属性直接设置在发生动画效果的 SVG 元素上

##### 状态属性

- attributeName 设置动画针对的 SVG 元素属性,如 width 等
  注意:该属性设置在 <animateTransform> 上时,属性值必须为 transform
- type <animateTransform> 专有属性,设置变换类型(translate,rotate,scale,skew)
- path <animateMotion> 专有属性,设置运动路径(属性值和 path 元素的 d 属性一样)
- origin <animateMotion> 专有属性,设置动画的运动原点
- rotate <animateMotion> 专有属性,设置元素在运动中是否需要旋转以匹配运动路径的斜率
  - 默认值为 0 不需要旋转
  - auto 自动旋转匹配 auto-reverse 以斜率相反的值旋转
  - 数字值 设置固定的旋转角度
- additive 设置动画每次循环的状态都在上一次循环的状态上叠加
- accumulate 没搞明白!!!!!!!!!!!!!!!!!!!!

##### 动画时间属性

- begin
  设置动画开始时间,可以作为 delay 使用
  在复合动画中,可以设置在其他动画完成后执行
  如:begin="动画 ID.end"表示在 ID 指向的动画完成后执行
  begin="0;动画 ID.end"表示第一次直接执行,之后的每一次在 ID 指向的动画完成后执行
- end 动画在多久之后结束,可以使用封号分隔的数值列
- dur 动画持续时间
- repeatCount 动画重复次数
- repeatDur 动画总重复时间
- fill="freeze" 动画完成后保持状态不变

##### 动画取值属性

- calcMode 动画的执行形式(像 timing-function)
  - discrete 离散地 会忽略动画过程
  - linear 线性动画
  - paced 匀速动画
  - spline 配合 keySplines 属性来定义各个动画过渡效, 自定义动画
- values 设置动画的关键帧状态,状态间以封号分隔
- keyTimes 取 0-1 之间的值,配合 value 使用,设置每个关键帧的触发时间点
- keyPoints 取 0-1 之间的值,配合 keyTimes 使用,设置动画在某个时间点应该执行到动画整体状态的哪个关键点
  - 通常配合 animateMotion,设置动画在某段运动状态中的速度
    注意:keyTimes 的值必须和 values 的值一一对应
- from 和 to 设置动画起始状态和结束状态
