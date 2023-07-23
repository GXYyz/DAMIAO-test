# canvas

- <canvas>是一个可以使用脚本 (通常为 JavaScript) 来绘制图形的 HTML 元素

## canvas 的基本用法

- Canvas 的默认大小为 300 像素 ×150 像素,其中的默认单位是 px,初始 canvas 是全透明的

- 可以设置其 width 和 height 属性来自定义 Canvas 的尺寸
  canvas 和其他普通图像一样可以设置 margin padding 和 border 等,但是这些样式不会影响其中的实际图像(使用 style 设置 canvas 会导致不可预测的 bug,应避免)

- 还可以操作其 DOM,使用**HTMLCanvasElement**提供的*height*和*width*接口来设置宽高

- <canvas> 元素有一个叫做 **getContext()** 的方法，用来获得渲染上下文和它的绘画功能,对于 2D 图像而言使用*CanvasRenderingContext2D*

  ```JavaScript
  // 获取HTMLCanvasElement
  const canvas = document.querySelector('canvas')
  // 获取画笔
  const ctx = canvas.getContext('2d');
  ```

## canvas 的基本绘图

### 绘制矩形

- canvas 提供了三种方法绘制矩形
  fillRect(x, y, width, height) 绘制一个填充的矩形
  strokeRect(x, y, width, height) 绘制一个矩形的边框
  clearRect(x, y, width, height) 清除指定矩形区域，让其完全透明
  其参数含义和 svg 的矩形绘制相同

### 绘制路径

本质上，路径是由很多子路径构成，这些子路径都是在一个列表中，所有的子路径（线、弧形、等等）构成图形

- beginPath() 新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。

  - 每当需要绘制不连续的图形时,就需要在起点之前调用 beginPath(),在断点之后调用 closePath()
    _当调用 beginPath()之后,或者 canvas 刚建立的时候,第一条路径构造命令无论实际上是什么都被视为是 moveTo()。出于这个原因，你几乎总是要在设置路径之后专门指定你的起始位置_

- moveTo(x,y) 将笔触移动到指定的坐标 x 以及 y 上(除了初始化路径起点,还可以使用该方法绘制一些不连续的路径)

- closePath() 闭合路径之后图形绘制命令又重新指向到上下文中。
  _闭合路径不是必须的,起点和终点一致的时候,或者调用 fill()方法的时候路径都会自动闭合_

- stroke() 通过线条来绘制图形轮廓。

- fill() 通过填充路径的内容区域生成实心的图形。

#### 绘制线路径

- lineTo(x,y) 绘制一条从当前位置到指定 x 以及 y 位置的直线

#### 绘制弧形路径

- arc(x, y, radius, startAngle, endAngle, anticlockwise)
  - x,y 为绘制圆弧所在圆上的圆心坐标。
  - radius 为半径。
  - startAngle 以及 endAngle 参数用弧度定义了开始以及结束的弧度。这些都是以 x 轴为基准。
  - 参数 anticlockwise 为一个布尔值。为 true 时，是逆时针方向，否则顺时针方向。
    _由于坐标系原点在左上角,x 轴向右,y 轴向下,所有 canvas 画布中 0 度的方位与 x 轴重合,默认顺时针增加_
    _arc() 函数中表示角的单位是弧度，不是角度。角度与弧度的 js 表达式：弧度=(角度\*Math.PI/180)。_

#### 绘制贝塞尔曲线路径

- quadraticCurveTo(cp1x, cp1y, x, y) 绘制二次贝塞尔曲线
  cp1x,cp1y 为一个控制点，x,y 为结束点

- bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y) 绘制三次贝塞尔曲线
  cp1x,cp1y 为控制点一，cp2x,cp2y 为控制点二，x,y 为结束点

  ![示意图](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes/canvas_curves.png)
  红色为控制点,蓝色为开始点和结束点

```JavaScript
// 对话框气泡
function draw() {
var canvas = document.getElementById('canvas');
if (canvas.getContext) {
  var ctx = canvas.getContext('2d');

  // 二次贝塞尔曲线
  ctx.beginPath();
  ctx.moveTo(75, 25);
  ctx.quadraticCurveTo(25, 25, 25, 62.5);
  ctx.quadraticCurveTo(25, 100, 50, 100);
  ctx.quadraticCurveTo(50, 120, 30, 125);
  ctx.quadraticCurveTo(60, 120, 65, 100);
  ctx.quadraticCurveTo(125, 100, 125, 62.5);
  ctx.quadraticCurveTo(125, 25, 75, 25);
  ctx.stroke();
 }
}
// 心
function draw() {
var canvas = document.getElementById('canvas');
if (canvas.getContext){
  var ctx = canvas.getContext('2d');

   //三次贝塞尔曲线
  ctx.beginPath();
  ctx.moveTo(75, 40);
  ctx.bezierCurveTo(75, 37, 70, 25, 50, 25);
  ctx.bezierCurveTo(20, 25, 20, 62.5, 20, 62.5);
  ctx.bezierCurveTo(20, 80, 40, 102, 75, 120);
  ctx.bezierCurveTo(110, 102, 130, 80, 130, 62.5);
  ctx.bezierCurveTo(130, 62.5, 130, 25, 100, 25);
  ctx.bezierCurveTo(85, 25, 75, 37, 75, 40);
  ctx.fill();
}
}
```

#### 绘制矩形路径

- rect(x, y, width, height) 绘制一个左上角坐标为（x,y），宽高为 width 以及 height 的矩形
  _当该方法执行的时候，moveTo() 方法自动设置坐标参数（0,0）。也就是说，当前笔触自动重置回默认坐标。_

#### Path2D 对象 (用于储存路径?)

- Path2D()会返回一个新初始化的 Path2D 对象（可能将某一个路径作为变量——创建一个它的副本，或者将一个包含 SVG path 数据的字符串作为变量）

```JavaScript
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext){
    var ctx = canvas.getContext('2d');

    // 实例化一个Path2D对象,并在里面创建一个矩形路径
    var rectangle = new Path2D();
    rectangle.rect(10, 10, 50, 50);
    // 实例化一个Path2D对象,并在里面创建一个圆形路径
    var circle = new Path2D();
    circle.moveTo(125, 35);
    circle.arc(100, 35, 25, 0, 2 * Math.PI);

    // 将对象绘制在画布上
    ctx.stroke(rectangle);
    ctx.fill(circle);
  }
}
```

- Path2D 中还可以使用 svg 中的路径语法

```JavaScript
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

let p = new Path2D("M10 10 h 80 v 80 h -80 Z");
ctx.fill(p);
```

## 使用样式和颜色

### 颜色

默认颜色均为黑色

- fillStyle = color 设置图形的填充颜色。
- strokeStyle = color 设置图形轮廓的颜色。
  color 可以是表示 CSS 颜色值的字符串，渐变对象或者图案对象

_一旦设置了 strokeStyle 或者 fillStyle 的值，那么这个新值就会成为新绘制的图形的默认值。如果你要给每个图形上不同的颜色，你需要重新设置 fillStyle 或 strokeStyle 的值_

### 透明度

通过设置**globalAlpha**属性或者使用一个 rgba() 颜色作为轮廓或填充的样式

- globalAlpha = transparencyValue 这个属性会影响到 canvas 里所有图形的透明度 取值 0-1

### 线型 Line styles

通过一些列属性来设置线的样式

- lineWidth = value 设置线条宽度 数值 必须为正数
  线宽是指给定路径的中心到两边的粗细。换句话说就是在路径的两边各绘制线宽的一半。因为画布的坐标并不和像素直接对应，当需要获得精确的水平或垂直线的时候要特别注意。

- lineCap = type 设置线条末端样式

  - butt 默认值 round 半圆末端 square 比默认值长一点点

- lineJoin = type 设定线条与线条间接合处的样式
  - miter 默认值 round 圆角
  - bevel 线段会在连接处外侧延伸直至交于一点，延伸效果受到 miterLimit 属性的制约
- miterLimit = value 限制当两条线相交时交接处最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度
- getLineDash() 返回一个包含当前虚线样式，长度为非负偶数的数组
- setLineDash(segments) 设置当前虚线样式
  - 参数为一个线段和间隙交替的数组
- lineDashOffset = value 设置虚线样式的起始偏移量

### 渐变

- 调用如下两个方法新建渐变对象,并且赋给图形的 fillStyle 或 strokeStyle 属性

  - 线性渐变路径 createLinearGradient(x1, y1, x2, y2)
    createLinearGradient 方法接受 4 个参数，表示渐变的起点 (x1,y1) 与终点 (x2,y2)。

  - 径向渐变路径 createRadialGradient(x1, y1, r1, x2, y2, r2)
    createRadialGradient 方法接受 6 个参数，前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆。

- 为渐变对象设置渐变节点 gradient.addColorStop(position, color)
  position 指渐变中颜色所在的相对位置,取值 0-1
  color 指定该位置上的颜色

- 注意:渐变中的位置参数仍然是以 canvas 画布坐标系为基准的

```JavaScript
const lg = context.createLinearGradient(100,100,300,100)
lg.addColorStop(0,'yellow')
lg.addColorStop(0.5,'orange')
lg.addColorStop(1,'red')
context.strokeStyle = lg
```

### 阴影

- 设置阴影的延伸距离 shadowOffsetX = float shadowOffsetY = float
  x 和 y 上的阴影延伸距离需要分别设置,他们是不受变换矩形所影响的

- 设置阴影的模糊程度 shadowBlur = float

- 设置阴影颜色 shadowColor = color

## 绘制文本

canvas 提供了两种方法来渲染文本

### 语法

- fillText(text, x, y, maxWidth)
  在指定的(x,y)位置填充指定的文本,绘制的最大宽度是可选项

- strokeText(text, x, y, maxWidth)
  在指定的(x,y)位置以绘制文本轮廓线的形式绘制文本,maxWidth 可选

- 注意
  - 绘制文本方法中的坐标位置与一般情况不同,(x,y)指定了文本的左下角
  - maxWidth 会导致文本在水平方向上被压缩

### 修改文本的样式

- font 修改文本大小字体等基础样式,和 CSS 的 font 属性语法相同

- direction 文本方向 value=ltr,rtl
  _与 CSS 不同的是,该属性不会改变文字的方向,文字还是会按照文本绘制方法中写入的方向显示,只是改变了文字的起点是文本的开头还是末尾_

- textAlgin 文本对齐选项 value=start,end,left,right,center

- textBaseLine 基线对齐选项 value=top,hanging,middle,alphabetic,ideographic,bottom

- 注意:**修改文本样式必须在文本绘制方法之前**

## 使用图像

- 引入图像到 canvas

  - 获得一个指向 HTMLImageElement 的对象或者另一个 canvas 元素的引用作为源，也可以通过提供一个 URL 的方式来使用图片
  - 使用 drawImage()函数将图片绘制到画布上

  ```javascript
  // 构造image对象
  let img = new Image()
  img.src = '相对文件地址/URL'
  // 等待img加载完成再将其绘制到画布上
  img.onload = function () {
    context.drawImage(img, x, y)
  }
  ```

  - 另
    - 除了 image()函数构造的对象外,还可以使用任何<img>元素,以 DOM 的形式引入
    - 也可以使用另一个<canvas>元素作为图片源
    - 还有其他方式详见 MDN

### 绘制图片

- drawImage(image,x,y)
  其中 image 是 image 或者 canvas 对象,x 和 y 是其在目标 canvas 里的起始坐标

- drawImage(image, x, y, width, height) 添加了缩放的参数

  - 使用 width 和 height 来控制图像画入 canvas 时的水平和垂直缩放比例
  - 可以使用 img.width 和 img.height 来计算原始图片的纵横比

- drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight) 添加了剪切的参数
  - sx 和 sy 表示以图片左上角为原点,剪切部分的左上角坐标
  - sWidth 和 sHeight 表示剪切的大小
  - 后面四个参数就是图像绘入 canvas 中的坐标和缩放比例

## canvas 滤镜

- 类似于 CSS 的 filter 属性,并接收相同的函数
  context.filter = '参数'

  - url() 函数接受一个描述 SVG 过滤器的 XML 文件的位置，并且可以包含一个针对特殊过滤元素的锚点
  - blur(length) 模糊效果
  - brightness(%) 调节百分比亮度
  - contrast(%) 调节百分比图像对比度
  - drop-shadow(<offset-x>, <offset-y>, <blur-radius>, <spread-radius>(阴影扩张), <color>) 阴影
  - grayscale(%) 调节百分比灰阶 0 没有变化 100 全灰
  - hue-rotate(deg) 对图像进行色彩旋转处理(在色彩空间中旋转) 0 度没有变化
  - invert(%) 图像反色处理 100 时呈现负片效果
  - opacity(%) 透明度
  - saturate(%) 饱和度
  - sepia(%) 褐色处理(怀旧风格) 0 无变化 100 全褐

## 变形 Transformations

### 状态的保存和恢复

- save() 保存调用该方法之前的 canvas 的所有状态
  - 多个 save()方法被调用时,save()方法只会保存到前一个 save()之间的状态
- restore() 恢复 canvas 的状态
  这两个方法都没有参数,canvas 的状态就是当前画面应用的所有样式和变形的一个快照,**保存的状态不包括绘制路径**

- canvas 状态存储在栈中,每当 save()方法被调用后,当前的状态就被推送到栈中保存
- 可以调用多次 save 方法,每一次调用 restore 方法,上一个保存的状态就从栈中弹出,所有设定都恢复,多个 save()被 store 时符合先进后出的原则
- 建议,每次在进行变形之前都使用 save()方法保存状态

### 移动

- translate(x,y)方法 用于移动绘制的原点到 x 和 y 表示的坐标,不是仅移动了绘画的图像,而是直接移动了坐标原点.
  需要注意的是 _translate()方法只会影响到移动之后的路径绘制的坐标原点,相同的,如果后面还有其他 translate(),也不会影响到前面的路径绘制_

### 旋转

- rotate() 用于以原点为中心旋转绘制的图像
  其中的参数单位依然是弧度
  旋转的心点始终是绘制的原点,需要使用 translate()方法配合以改变

### 缩放

- scale(x,y) 可以缩放画布的水平和垂直的单位
  x 和 y 是图像在水平和垂直方向上缩放的系数
  - 默认情况下 canvas 的 1 个单位就是 1px,缩放系数影响的是 canvas 的单位系数,如设置缩放系数为 0.5,那么一个单位就变成 0.5 个像素
    _在视觉上来看,scale()方法像是缩放了 canvas 的坐标系比例,绘制的图像会在缩放时有距离上的移动,而不是简单的放大_

### 变形

该方法允许对变形矩阵直接修改

- transform(a, b, c, d, e, f)
  各参数的代表如下
-
- a (m11) 水平方向的缩放
- b(m12) 竖直方向的倾斜偏移
- c(m21) 水平方向的倾斜偏移
- d(m22) 竖直方向的缩放
- e(dx) 水平方向的移动
- f(dy) 竖直方向的移动

### 注意

需要注意的是 _canvas 的变形方法只会影响到移动之后的路径绘制,直到出现下一个相同的变形方法之前,中间的路径绘制都会受到影响.同样的,后出现的变形方法也只会影响其之后的路径绘制_

## 组合

### 混合模式

canvas 绘图的默认模式为按照代码顺序层层叠加,后绘制的图像会覆盖之前的图像,但仅仅这样的模式是不够的.对于合成的图像来说,可以利用 globalCompositeOperation 属性来设置混合模式,和其他属性一样,该属性只会影响在其之后绘制的图像.

- globalCompositeOperation = type
  这个属性设定了在画新图形时采用的遮盖策略，其值是一个标识 12 种遮盖方式的字符串

  - **source-over**(默认值)在现有画布上下文之上绘制新图像
  - source-in 新图形只在和旧图形重叠的地方绘制,并且会将没有重叠的部分变为透明(至少看起来是这样)
  - source-out 新图形只在不与旧图像重叠的地方绘制,并且会将原有的图形分变为透明
  - **source-atop** 新图形只在与旧图形重叠的地方绘制,原有的图形不会有变化
  - **destination-over** 在现有的图形后面绘制新的图形
  - destination-in 与 source-in 相反,只保留旧图形中与新图形重叠的部分,其他都是透明
  - destination-out 与 source-out 相反,只保留旧图形中与新图形不重叠的部分,其他都是透明
  - destination-atop 旧图形只保留与新图形重叠的部分,不会影响新图形
  - lighter 重叠图形的颜色是通过颜色值相加来确定的
  - copy 只显示新图形
  - xor 重叠的地方为透明的
  - multiply 将顶层像素与底层相应像素相乘(像是把重叠部分变黑)
  - screen 像素被倒转，相乘，再倒转，结果是一幅更明亮的图片(类似于 lighter)
  - overlay multiply 和 screen 的结合，原本暗的地方更暗，原本亮的地方更亮
  - darken 保留两个图层中最暗的像素 (类似于 multiply)
  - lighten 保留两个图层中最亮的像素 (类似于 lighter)
    _以下部分没有验证_
  - color-dodge 将底层除以顶层的反置。
  - color-burn 将反置的底层除以顶层，然后将结果反过来。
  - hard-light 屏幕相乘（A combination of multiply and screen）类似于叠加，但上下图层互换了。
  - soft-light 用顶层减去底层或者相反来得到一个正值。
  - difference 一个柔和版本的强光（hard-light）。纯黑或纯白不会导致纯黑或纯白。
  - exclusion 和 difference 相似，但对比度较低。
  - hue 保留了底层的亮度（luma）和色度（chroma），同时采用了顶层的色调（hue）。
  - saturation 保留底层的亮度（luma）和色调（hue），同时采用顶层的色度（chroma）。
  - color 保留了底层的亮度（luma），同时采用了顶层的色调 (hue) 和色度 (chroma)。
  - luminosity 保持底层的色调（hue）和色度（chroma），同时采用顶层的亮度（luma）。

  ### 裁剪路径

  - clip()将当前正在构建的路径转换为当前的裁剪路径

  ```javascript
  ctx.beginPath()
  ctx.arc(0, 0, 60, 0, Math.PI * 2, true)
  ctx.clip()
  // 这个圆弧路径将会变为裁剪路径
  // 路径填充外将会被裁剪
  ```

  - **裁剪路径也属于 canvas 状态中的一部分,可以被 save()方法保存起来**

## 基本动画

### 动画的基本步骤

canvas 动画每一帧的步骤都如下

- 清空 canvas 画布 使用 clearRect()方法
- 保存 canvas 状态
  _如果需要改变一些会改变 canvas 状态的设置(变形,样式之类),那必须在画每一帧之前都保存一下 canvas 画布的初始状态,否则 canvas 状态的改变会叠加,而产生不可预知的状态_
- 绘制动画图形图形
- 恢复 canvas 状态

**事实上,canvas 绘制的每一帧动画都是在初始状态的 canvas 画布上绘制的,只是每次绘制的内容有一点点不同,叠加起来就成了动画**

### 操控动画

- 当我们能够绘制每一帧动画的画面之后,自然就需要不停地绘制,来实现动画的效果,因此可以通过 setInterval 和 setTimeout 方法或者来控制定时执行重绘
  如果你并不需要与用户互动，你可以使用 setInterval() 方法，它就可以定期执行指定代码。如果我们需要做一个游戏，我们可以使用键盘或者鼠标事件配合上 setTimeout() 方法来实现。通过设置事件监听，我们可以捕捉用户的交互，并执行相应的动作。

- 但是,使用定时器的方式绘制的帧数是固定的,会使得计算机无法根据算力来配置重绘次数

  - **requestAnimationFrame() 方法**
    这个方法提供了更加平缓并更加有效率的方式来执行动画，当系统准备好了重绘条件的时候，才调用绘制动画帧。一般每秒钟回调函数执行 60 次，也有可能会被降低

  - 所以,通常会使用 requestAnimationFrame()方法递归调用的方式来绘制动画
    eg.太阳系运动

  - requestAnimationFrame()方法每次运行都会返回一个 ID,该 ID 值为本动画已经运行的次数

  - 停止动画 **cancelAnimationFrame(requestID) 方法**(mozCancelAnimationFrame(requestID) // firefox)
    - 其中 requestID 为先前调用 requestAnimationFrame()方法时返回的 ID

```JavaScript
 function draw() {
      context.clearRect(0,0,canvas.width,canvas.height)
      context.save()
      // 太阳
      context.beginPath()
      context.arc(400, 400, 150, 0, 2 * Math.PI)
      context.fillStyle = 'orange'
      context.shadowColor = 'orange'
      context.shadowBlur = 100
      context.fill()
      context.closePath()
      // 地球
      context.beginPath()
      context.translate(400,400)
      context.rotate(earthContent += .5 * Math.PI / 180)
      context.arc(-200,-200,20,0,2 * Math.PI)
      context.fillStyle = 'skyblue'
      context.fill()
      context.closePath()
      // 月亮
      context.beginPath()
      context.translate(-200,-200)
      context.rotate(moonContent +=1 * Math.PI / 180)
      context.arc(-23,-23,5,0,2 * Math.PI)
      context.shadowColor = '#ddd'
      context.shadowBlur = 3
      context.fillStyle = '#ddd'
      context.fill()

      context.restore()

      requestAnimationFrame(draw)
    }
    let earthContent = 1
    let moonContent = 0
    requestAnimationFrame(draw)
```

## 像素操作

### ImageData 对象

ImageData 接口描述 <canvas> 元素的一个隐含像素数据的区域

- 对象中包含几个只读属性
  width 和 height 对象区域宽高单位像素
  data Uint8ClampedArray 类型的一维数组，包含着 RGBA 格式的整型数据，范围在 0 至 255 之间(包括 255)

  - data 属性返回一个 Uint8ClampedArray，它可以被使用作为查看初始像素数据。每个像素用 4 个 1bytes 值 (按照红，绿，蓝和透明值的顺序; 这就是"RGBA"格式) 来代表。每个颜色值部份用 0 至 255 来代表。每个部份被分配到一个在数组内连续的索引，左上角像素的红色部份在数组的索引 0 位置。像素从左到右被处理，然后往下，遍历整个数组。
    Uint8ClampedArray 包含 height × width × 4 字节数据，索引值从 0 到 (height× width × 4)-1
    ```javascript
    // 根据行、列读取某像素点的 R/G/B/A 值的公式：
    imageData.data[50 * (imageData.width * 4) + 200 * 4 + 0 / 1 / 2 / 3]
    // 读取图片中位于第 50 行，第 200 列的像素的蓝色部份
    blueComponent = imageData.data[50 * (imageData.width * 4) + 200 * 4 + 2]
    ```

#### 创建 ImageData 对象

- 通过 context.createImageData(width,height) 创建一个新的、空白的、指定大小的 ImageData 对象。所有的像素在新对象中都是透明黑(rgba(0,0,0,0))的
- 也可以通过传入另一个 imageData 对象(createImageData(anotherImageData)),来创建一个相同大小的全透明黑的 imageData 对象(不是复制了另一个对象的数据)

#### 获取 ImageData 对象

- 通过 context.getImageData(x,y,width,height)获取现有画布中指定区域的 imageData 对象(其参数含义和 fillRect()方法一致)
- putImageData()

```javascript
// 颜色选择器
// 在这个例子里面，我们会使用 getImageData() 去展示鼠标光标下的颜色。为此，我们要当前鼠标的位置，记为 layerX 和 layerY，然后我们去查询 getImageData() 给我们提供的在那个位置的像数数组里面的像素数据。最后我们使用数组数据去设置背景颜色和 <div> 的文字去展示颜色值。

var img = new Image()
img.crossOrigin = 'anonymous'
img.src = './assets/rhino.jpg'
var canvas = document.getElementById('canvas')
var ctx = canvas.getContext('2d')
img.onload = function () {
  ctx.drawImage(img, 0, 0)
  img.style.display = 'none'
}
var hoveredColor = document.getElementById('hovered-color')
var selectedColor = document.getElementById('selected-color')

function pick(event, destination) {
  var x = event.layerX
  var y = event.layerY
  var pixel = ctx.getImageData(x, y, 1, 1)
  var data = pixel.data

  const rgba = `rgba(${data[0]}, ${data[1]}, ${data[2]}, ${data[3] / 255})`
  destination.style.background = rgba
  destination.textContent = rgba

  return rgba
}

canvas.addEventListener('mousemove', function (event) {
  pick(event, hoveredColor)
})
canvas.addEventListener('click', function (event) {
  pick(event, selectedColor)
})
```

#### 在场景中写入像素数据

- 使用 ctx.putImageData(imagedata, dx, dy, dirtyX, dirtyY, dirtyWidth, dirtyHeight)将数据从已有的 image 对象绘制到图像

  - 参数 imageData 包含像素值的数组对象
    dx 源图像数据在目标画布中的位置偏移量（x 轴方向的偏移量）。
    dy 源图像数据在目标画布中的位置偏移量（y 轴方向的偏移量）。
    dirtyX (可选)在源图像数据中，矩形区域左上角的位置。默认是整个图像数据的左上角（x 坐标）。
    dirtyY (可选)在源图像数据中，矩形区域左上角的位置。默认是整个图像数据的左上角（y 坐标）。
    dirtyWidth (可选)在源图像数据中，矩形区域的宽度。默认是图像数据的宽度。
    dirtyHeight (可选)在源图像数据中，矩形区域的高度。默认是图像数据的高度。

    ```javascript
    // 图片灰度和反相颜色
    // 在这个例子里，我们遍历所有像素以改变他们的数值。然后我们将被修改的像素数组通过putImageData() 放回到画布中去。invert 函数仅仅是去减掉颜色的最大色值 255。grayscale 函数仅仅是用红绿和蓝的平均值。你也可以用加权平均，例如 x = 0.299r + 0.587g + 0.114b 这个公式
    var img = new Image()
    img.crossOrigin = 'anonymous'
    img.src = './assets/rhino.jpg'

    var canvas = document.getElementById('canvas')
    var ctx = canvas.getContext('2d')

    img.onload = function () {
      ctx.drawImage(img, 0, 0)
    }

    var original = function () {
      ctx.drawImage(img, 0, 0)
    }

    var invert = function () {
      ctx.drawImage(img, 0, 0)
      const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height)
      const data = imageData.data
      for (var i = 0; i < data.length; i += 4) {
        data[i] = 255 - data[i] // red
        data[i + 1] = 255 - data[i + 1] // green
        data[i + 2] = 255 - data[i + 2] // blue
      }
      ctx.putImageData(imageData, 0, 0)
    }

    var grayscale = function () {
      ctx.drawImage(img, 0, 0)
      const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height)
      const data = imageData.data
      for (var i = 0; i < data.length; i += 4) {
        var avg = (data[i] + data[i + 1] + data[i + 2]) / 3
        data[i] = avg // red
        data[i + 1] = avg // green
        data[i + 2] = avg // blue
      }
      ctx.putImageData(imageData, 0, 0)
    }

    const inputs = document.querySelectorAll('[name=color]')
    for (const input of inputs) {
      input.addEventListener('change', function (evt) {
        switch (evt.target.value) {
          case 'inverted':
            return invert()
          case 'grayscale':
            return grayscale()
          default:
            return original()
        }
      })
    }

    // 缩放和反锯齿
    // 在drawImage() 方法，第二个画布和imageSmoothingEnabled 属性的帮助下，我们可以放大显示我们的图片及看到详情内容。

    // 我们得到鼠标的位置并裁剪出距左和上 5 像素，距右和下 5 像素的图片。然后我们将这幅图复制到另一个画布然后将图片调整到我们想要的大小。在缩放画布里，我们将 10×10 像素的对原画布的裁剪调整为 200×200。

    zoomctx.drawImage(canvas, Math.abs(x - 5), Math.abs(y - 5), 10, 10, 0, 0, 200, 200)

    // 因为反锯齿默认是启用的，我们可能想要关闭它以看到清楚的像素。你可以通过切换勾选框来看到 imageSmoothingEnabled 属性的效果（不同浏览器需要不同前缀）

    var img = new Image()
    img.src = 'rhino.jpg'
    img.onload = function () {
      draw(this)
    }

    function draw(img) {
      var canvas = document.getElementById('canvas')
      var ctx = canvas.getContext('2d')
      ctx.drawImage(img, 0, 0)
      img.style.display = 'none'
      var zoomctx = document.getElementById('zoom').getContext('2d')

      var smoothbtn = document.getElementById('smoothbtn')
      var toggleSmoothing = function (event) {
        zoomctx.imageSmoothingEnabled = this.checked
        zoomctx.mozImageSmoothingEnabled = this.checked
        zoomctx.webkitImageSmoothingEnabled = this.checked
        zoomctx.msImageSmoothingEnabled = this.checked
      }
      smoothbtn.addEventListener('change', toggleSmoothing)

      var zoom = function (event) {
        var x = event.layerX
        var y = event.layerY
        zoomctx.drawImage(canvas, Math.abs(x - 5), Math.abs(y - 5), 10, 10, 0, 0, 200, 200)
      }

      canvas.addEventListener('mousemove', zoom)
    }
    ```

    #### 保存图片

    canvas 提供一个 toDataURL()方法,用于保存图片,其返回一个包含被类型参数规定的图像表现格式的数据链接。返回的图片分辨率是 96 dpi

    - canvas.toDataURL('image/png') 默认设定。创建一个 PNG 图片
    - canvas.toDataURL('image/jpeg', quality) 创建一个 JPG 图片。你可以有选择地提供从 0 到 1 的品质量，1 表示最好品质，0 基本不被辨析但有比较小的文件大小
      当你从画布中生成了一个数据链接，例如，你可以将它用于任何<image>元素，或者将它放在一个有 download 属性的超链接里用于保存到本地

    ```javascript
    // <a href="" download>下载图像</a>
    // a标签的download属性可以可以使用或不使用属性值,其属性值用于决定下载的文件名,如果没有属性值浏览器会从多个来源决定文件名和扩展名
    a.href = canvas.toDataURL('image/png')
    ```
