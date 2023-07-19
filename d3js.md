# D3.js 基础使用

D3.js 用了一种与 jQuery 一样的 链式语法，这样通过 **.** 就把多个操作链接起来，执行逻辑上更加清晰，链式语法的关键就是每个操作函数都有返回值，这个返回值可以执行当前操作的对象，也可以是其他对象.需要注意的是.append()/.select()等方法返回的是新创建的元素对象,而不是调用这个方法的元素对象

_ps:本文中'selection'代表链式调用中前一个函数的返回值_
_d3js 中大部分的数据处理回调函数都可以接收两个参数数据项和数据索引,类似于数组方法回调函数的参数_

## 元素操控函数

- d3.create('tagName') 创建 svg 元素并返回

- selection.attr('属性',属性值)设置 svg 元素属性

- selection.style('属性',属性值) 设置元素属性

- selection.append('tagName') 创建元素并添加到调用者中

- selection.select('选择器') 选中父元素中第一个符合条件的元素

- selection.selectAll(选择器) 选中父元素中符合条件的所有元素(选择器除了字符串也可以是元素类数组对象,如 nodeList) 如果不存在则返回空集合(空集合可以和 data()join()配合,增删指定的元素)

  - _如果不存在指定的元素,相当于为指定的元素创建了占位符,可以对占位符绑定数据,可以根据占位符真实创建或删除元素_

- selection.data(数据数组) 将指定的数据数组与选定的元素们绑定

  - 返回值将是数组中每一项和元素的绑定,后续所有函数类似于写在了遍历中,挨个操作绑定了数据的元素
  - 后续调用的函数中的参数项可以使用(d => d)这样的自定义函数,来指定参数为返回值,d 代表'data(数据数组)'中数据数组的顺序项

- selection.join() 根据需要追加、删除或排序指定元素,以匹配 data()中绑的数据数组

  - join()函数中包含了 enter()追加,update()更新,exit()删除三个函数

  ```javascript
  .join('circle')
  // 相当于
  .join(
    enter => enter.append("circle"),
    update => update,
    exit => exit.remove()
  )
  ```

  - 也就是说,在增删改的过程中可以通过指定的函数来更灵活地配置

- selection.remove() 移除 selection 表示的这一项元素

- selection.transition() 为绘制的元素绑定缓动效果,当元素在 join()函数中发生增删改时,将以缓动方式呈现
  **要注意的是,设置缓动效果及其参数的函数必须在设置元素属性的函数之前调用**
- selection.duration(time) 设置缓动时间
- selection.delay(time) 设置延迟时间

```javascript
// Exp
d3.create('svg')
  .attr('width', width + margin * 2)
  .attr('height', height + margin * 2)
  .append('g')
  .selectAll('circle')
  .data(fakeOption)
  .join('circle')
  .attr('fill', (d) => `hsl(${Math.sqrt(d[0] ** 2 + d[1] ** 2)} 70% 80%)`)
  .attr('r', 5)
  .attr('cx', (d) => x(d[0]))
  .attr('cy', (d) => y(d[1]))
```

## 功能性函数

### 比例尺函数

比例尺函数可以创建线性比例尺,时间比例尺,log 比例尺等等,用来将数据按照比例转化为适
合绘图界面或视口的参数

- 创建线性比例尺 d3.scaleLinear()

  - 可以直接传入两个由两个或更多值数值组成的范围数组,前一个表示输入范围,后一个表示输出范围,这个范围不一定是从小到大的,输入范围和输出范围的大小关系也是不强制的
  - selection.domain() 传入输入范围数组,设置比例尺的输入范围
  - selection.range() 传入输出范围数组,设置比例尺的输出范围
  - 比例尺函数.invert()
    通常情况下,比例尺函数将由输入范围的值返回输出范围的值,但使用 invert(值),比例尺函数将由输出范围的值返回输入范围的的值,也就是*x == 比例尺函数(比例尺函数.invert(x))*
  - selection.clamp(boolean)
    通常情况下,比例尺函数的输出和输出范围不会被限制在设置的输入输出范围中,但当比例尺函数设置了 clamp(true)后,输出的值将不会超出设置的输入输出范围

  ```javascript
  const x = d3.scaleLinear([10, 130], [0, 960]) // clamping disabled by default
  x(-10) // -160, outside range
  x.invert(-160) // -10, outside domain
  x.clamp(true) // enable clamping
  x(-10) // 0, clamped to range
  x.invert(-160) // 10, clamped to domain
  ```

  - selection.unknown(输出值) 设置了这个 unknown()值的比例尺函数,在遇到不合法的输入时,将输出 unknown 的值
  - 比例尺函数.sticks(count) 返回比例尺输入范围中具有代表性的数个值,count 默认值为 10,返回值是一个数组
    - 该函数通常用于绘制图标中数轴时提供数轴各点的值
  - 比例尺函数.tickFormat(count,'specifier') 统一 stick()函数返回的代表性值的格式,第一个参数为返回的个数,第二个参数为加上百分号等个性化设置
    ```javascript
    const x = d3.scaleLinear([-1, 1], [0, 960])
    const T = x.ticks(5) // [-1, -0.5, 0, 0.5, 1]
    const f = x.tickFormat(5, '+%')
    T.map(f) // ["−100%", "−50%", "+0%", "+50%", "+100%"]
    ```
    - 该函数可直接用 d3 调用 d3.tickFormat(start, stop, count, specifier),返回一个函数,用于将 start 和 stop 范围内的数值'格式化'
      该函数还需要再研究!!!!!!
  - selection.nice() 当比例尺函数的输入范围是一个较长的浮点数时,该函数用于将笔记函数的输入值修整为一个包含输入范围的更接近整数的数值,当输入范围不有多个参数时没改函数只影响第一个和最后一个值

  ```javascript
  const x = d3.scaleLinear([0.241079, 0.969679], [0, 960]).nice()
  x.domain() // [0.2, 1]
  ```

  这个函数的参数没搞明白

  - 比例尺函数.copy() 复制并返回一个参数相同的比例尺函数

- 其他比例尺函数见[d3js 文档](https://d3js.org/d3-scale)

### 颜色函数

- d3-scale-chromatic 这个模块提供了了一系列配色方案,通过该模块创建的函数,可以通过输入值返回配色方案中的渐变或发散的十六进制色值
  ```javascript
  const color = d3.scaleOrdinal(d3.schemeAccent)
  color(1) // '#7fc97f'
  ```
  该模块中不同配色方案使用不同函数,数量很多,[详见](https://d3js.org/d3-scale-chromatic)

### 曲线函数

- 返回的函数用于传递到其他函数的 curve()中,决定其各点之间以何种方式连接
- 每种绘制方式调用不同的函数,[详见](https://d3js.org/d3-shape/curve)
- 需要额外传递参数的绘制函数如下
  - d3.curveBundle.beta() 该连接方式的曲线与点之间并非完全拟合的,拟合程度有传递的参数决定,参数在 0-1 之间,从完全不拟合到最大程度拟合(但不是完全拟合),默认值为 0.85
  - d3.curveCardinal.tension() 该连接方式将各点直接连接,连接线的'张力'(根据点之间变化的趋势决定)由传入的参数决定,参数在 0-1 之间,从最大张力到没有张力,参数为 0 时绘制出的将是折线

### 饼图数据生成函数

- d3.pie(data) 根据传入的数组,依照数值大小,返回用于绘制数据个扇形组成的圆的数据数组对象,其中包含
  data:原始数据
  value: 依据值(函数依据该值来分配圆弧大小)
  index: 索引
  startAngle: 起始弧度
  endAngle: 结束弧度
  padAngle: 间距弧度
  ```javascript
  const data = [1, 1, 2, 3, 5, 8, 13, 21]
  const pie = d3.pie()
  const arcs = pie(data)
  arcs = [
    { data: 1, value: 1, index: 6, startAngle: 6.050474740247008, endAngle: 6.166830023713296, padAngle: 0 },
    { data: 1, value: 1, index: 7, startAngle: 6.166830023713296, endAngle: 6.283185307179584, padAngle: 0 },
    { data: 2, value: 2, index: 5, startAngle: 5.817764173314431, endAngle: 6.050474740247008, padAngle: 0 },
    { data: 3, value: 3, index: 4, startAngle: 5.468698322915565, endAngle: 5.817764173314431, padAngle: 0 },
    { data: 5, value: 5, index: 3, startAngle: 4.886921905584122, endAngle: 5.468698322915565, padAngle: 0 },
    { data: 8, value: 8, index: 2, startAngle: 3.956079637853813, endAngle: 4.886921905584122, padAngle: 0 },
    { data: 13, value: 13, index: 1, startAngle: 2.443460952792061, endAngle: 3.956079637853813, padAngle: 0 },
    { data: 21, value: 21, index: 0, startAngle: 0.0, endAngle: 2.443460952792061, padAngle: 0 }
  ]
  ```
  - selection.value(fun) 传入函数,决定如何从原始数据中获取依据值
  - selection.sort(fun) 类似于数组的 sort()方法,根据传入的函数返回值将输出结果排序
  - selection.startAngle()设置饼图起始角,起始角是饼图整体的起始角,也是第一个弧的起始角(默认为 0)
  - selection.endAngle() 设置饼图的结束角,和起始角类似.也就是说饼图可以不是一个完整的圆
  - selection.padAngle() 设置间隔角度

### 堆叠数据生成函数

- d3.stack()
  堆叠数据函数将长度转换为连续的位置间隔

  - 因为需要绘制堆叠图的数据通常由同一个 key 值在不同情况下的值来组成,所以需要下面特殊的处理函数
  - selection.keys(fun) 依照函数,处理并返回数据中的 key 值(key 值组成的数组),方便后续根据 key 值整理数据
  - selection.value(fun) 依照函数,处理处组成堆叠数据的值

    - 该函数的回调函数较为特殊,接收两个参数(data,key),第一个参数为数据对象,第二个参数为 keys()函数处理出的 key 值数组(默认函数为 (data,key) => data\[key])

    ```javascript
    const data = [
      { date: new Date('2015-01-01'), fruit: 'apples', sales: 3840 },
      { date: new Date('2015-01-01'), fruit: 'bananas', sales: 1920 },
      { date: new Date('2015-01-01'), fruit: 'cherries', sales: 960 },
      { date: new Date('2015-01-01'), fruit: 'durians', sales: 400 },

      { date: new Date('2015-02-01'), fruit: 'apples', sales: 1600 },
      { date: new Date('2015-02-01'), fruit: 'bananas', sales: 1440 },
      { date: new Date('2015-02-01'), fruit: 'cherries', sales: 960 },
      { date: new Date('2015-02-01'), fruit: 'durians', sales: 400 },

      { date: new Date('2015-03-01'), fruit: 'apples', sales: 640 },
      { date: new Date('2015-03-01'), fruit: 'bananas', sales: 960 },
      { date: new Date('2015-03-01'), fruit: 'cherries', sales: 640 },
      { date: new Date('2015-03-01'), fruit: 'durians', sales: 400 },

      { date: new Date('2015-04-01'), fruit: 'apples', sales: 320 },
      { date: new Date('2015-04-01'), fruit: 'bananas', sales: 480 },
      { date: new Date('2015-04-01'), fruit: 'cherries', sales: 640 },
      { date: new Date('2015-04-01'), fruit: 'durians', sales: 400 }
    ]
    const series = d3
      .stack()
      .keys(d3.union(data.map((d) => d.fruit))) // apples, bananas, cherries, …
      .value(([, group], key) => group.get(key).sales)(
      // 此处的[, group]类似于解构赋值(但是不一样),第一个参数是map的key,第二个参数是该可以对应的值
      d3.index(
        data,
        (d) => d.date,
        (d) => d.fruit
      )
    )
    ;[
      // apples      // bananas    // cherries   // durians
      [
        [0, 3840],
        [0, 1600],
        [0, 640],
        [0, 320]
      ],
      [
        [3840, 5760],
        [1600, 3040],
        [640, 1600],
        [320, 800]
      ],
      [
        [5760, 6720],
        [3040, 4000],
        [1600, 2240],
        [800, 1440]
      ],
      [
        [6720, 7120],
        [4000, 4400],
        [2240, 2640],
        [1440, 1840]
      ]
    ]
    ```

    - 其中的 d3.index()函数,可以将数据按照回调函数分组并创建 map,并可以接收多个回调参数,在一次分组中再次分组,所以在例子中,stack 函数接收到的是个 map
    - 其中的 d3.union()函数是数组去重函数

  - selection.order() 设置堆叠数据的顺序,里面填写 d3 自带的[堆叠顺序函数](https://d3js.org/d3-shape/stack#stack-orders)(可选设置)
  - selection.offset() 设置堆叠数据的偏移量,里面填写 d3 自带的[堆叠偏移量函数](https://d3js.org/d3-shape/stack#stack-offsets)(可选设置)

## 图形绘制函数

### 数轴绘制函数

- d3.axios(比例尺函数) 返回一个函数,调用 selection.call()函数,引入到 svg 容器或 g 容器中,可直接绘制出一个依照比例尺代表性值生成刻度的数轴

  - 如果需要动画效果,也可以在 call 函数之前调用缓动函数

  ```javascript
  svg.append('g').transition().duration(750).call(d3.axisBottom(x))
  ```

  - 该函数绘制出的为刻度在数轴下方的横轴

- 单独绘制各方向的数轴

  - d3.axiosTop() 横轴,刻度在数轴之上
  - d3.axiosBottom() 横轴,刻度在数轴之下
  - d3.axiosRight() 纵轴,刻度在数轴右侧
  - d3.axiosLeft() 纵轴,刻度在数轴左侧

- selection.scale(比例尺函数) 单独设置比例尺函数
- selection.ticks(count) 设置比例尺函数的代表性值个数,以生成 count 个刻度组成的数轴
- selection.tickValues(可迭代对象) 指定可迭代对象来生成数轴的刻度,而不是自动生成
  ```javascript
  const axis = d3.axisBottom(x).tickValues([1, 2, 3, 5, 8, 13, 21])
  // 数组的刻度将由[1, 2, 3, 5, 8, 13, 21]组成
  ```
  - selection.tickSize(num) 设置数轴刻度线的长度(默认为 10)
  - selection.tickSizeInner()/tickSizeOuter() 单独设置数轴内部刻度和外部刻度的长度
    - 外部刻度指数轴 0 和最大值的刻度,中间的都是外部刻度
  - selection.tickPadding()设置数轴值和刻度之间的间距(默认为 0)
  - selection.offset(0) 设置数轴的偏移量,偏移量指数轴朝设置方向的移动距离,_需要注意的是,偏移量设置只针对轴,刻度线和值不会移动_(默认为 0)

### 线条生成器函数 -- 折线图

根据数据中的数个点,绘制线条

- d3.line() 返回一个函数,函数接收可迭代对象并返会线条路径

  - selection.x(fun) 传递一个函数,决定如何获得数据中 x 方向上的值
  - selection.y(fun) 传递一个函数,决定如何获得数据中 x 方向上的值
  - selection.defined(fun) 传递一个返回布尔值的函数,返回值为 false 时,绘制函数将跳过该项数据所对应点的绘制
    **这个完全是我猜的,不知道对不对**
  - selection.curve(curve) 设置以何种方式连接各个点,其中的参数是 d3[曲线函数](https://d3js.org/d3-shape/curve)的返回值
  - selection.digits(num) 决定其返回的路径字符串浮点数精确到小数点后几位(默认三位)
  - selection.context() 传入 canvas 画笔

    - 传入了 canvas 画笔的线生成器函数,需要在 canvas 绘制上下文中调用,并传入单一线数据,该函数会自动调用 canvas 绘制函数绘制线

    ```javascript
    let drawLine = d3
      .line()
      .x((d) => x(d.x))
      .y((d) => y(d.y))
      .context(context)

    context.beginPath()
    context.translate(100, 100)
    drawLine(data)
    context.lineWidth = 2
    context.strokeStyle = 'black'
    context.stroke()
    context.closePath()
    ```

### 弧形生成器函数 -- 饼图

- d3.arc() 返回一个函数,用于绘制弧形或扇形

  - 该函数有四个主要参数
    innerRadius 内径
    outerRadius 外径
    startAngle 起始角度
    endAngle 结束角度
    - 这些参数可以以对象的形式传入弧形生成器函数,或是以链式调用的方式传入,并可传入函数,决定如何获得绑定数据
    ```javascript
    const arc = d3.arc()
    arc({
      innerRadius: 0,
      outerRadius: 100,
      startAngle: 0,
      endAngle: Math.PI / 2
    })
    // 等于
    d3.arc()
      .innerRadius(0)
      .outerRadius(100)
      .startAngle(0)
      .endAngle(Math.PI / 2)
    ```
  - selection.centroid(data) 返回数据对应的弧形或扇形的中心点坐标,用于方便居中的标签效果
    - 返回的中心点坐标是由 xy 值组成的数组
  - selection.cornerRadius(num) 设置扇形四个角的圆角效果,类似于 border-radius
  - selection.padAngle() 设置每个扇形之间的间距角
    - 推荐的最小内半径是 outerRadius \* padAngle / sin(θ)，其中 θ 是填充前最小弧的角跨度
    - 通常这个值很小,在 0.0x 左右
  - selection.padRadius() 同样也是设置扇形之间的间距,从视觉效果来看,该函数设置的间距不会导致扇形相对位置发生变化
  - selection.context() 传入 canvas 画笔

    - 传入了 canvas 画笔的线生成器函数,需要在 canvas 绘制上下文中调用,并传入单扇形数据,该函数会自动调用 canvas 绘制函数绘制扇形

    ```javascript
    const arc = d3.arc()

    context.beginPath()
    context.translate(100, 100)
    arc({
      innerRadius: 0,
      outerRadius: 100,
      startAngle: 0,
      endAngle: Math.PI / 2
    })
    context.strokeStyle = 'black'
    context.stroke()
    context.closePath()
    ```

    - 当然,也可以设置好参数及获取函数,直接传入参数对象

### 区域生成器函数 -- 柱状图/面积图

- d3.area() 返回一个函数,用于绘制面积图
  面积图分为两个方向绘制,传递不同的参数
  - 沿着 x 轴绘制,y 方向两条线作为顶边和底边
    - selection.x(fun) 传递函数决定如何获取数据中的 x 值
    - selection.y0(fun) 传递函数决定如何获取数据中底边的 y 值
    - selection.y1(fun) 传递函数决定如何获取数据中顶边的 y 值
    - 其中获取到的 x 值将在两个边中共用
  - 沿着 y 轴绘制,x 方向两条线作为顶边和底边
    - selection.y(fun) 传递函数决定如何获取数据中的 y 值
    - selection.x0(fun) 传递函数决定如何获取数据中底边的 x 值
    - selection.x1(fun) 传递函数决定如何获取数据中顶边的 x 值
    - 其中获取到的 y 值将在两个边中共用
  - selection.curve(curve) 设置以何种方式连接各个点,其中的参数是 d3[曲线函数](https://d3js.org/d3-shape/curve)的返回值
  - selection.context(context) 传递 canvas 画笔,绘制方式与其他函数类似
  - selection.digits(num) 决定其返回的路径字符串浮点数精确到小数点后几位(默认三位)

### 连接线函数

- 以三阶贝塞尔曲线连接两个点

  - d3.linkVertical() 返回一个函数生成一个更靠近 y 轴的贝塞尔曲线连接两个点
  - d3.linkHorizontal() 返回一个函数生成一个更靠近 x 轴的贝塞尔曲线连接两个点

    - 无论以哪个方式创建连接线,单独使用时都需要传入起点和终点两个参数

  - selection.source(fun) 传入函数,决定如何获取起点参数(默认 return d.source)
  - selection.target(fun) 传入函数,决定如何获取终点参数(默认 return d.target)

  - selection.x(fun) 传入函数,决定如何获取 x 值
  - selection.y(fun) 传入函数,决定如何获取 y 值
    _当配合树状图生成器的 links()函数使用时,需要设置 xy 函数,将其传入起点坐标和终点坐标的特性转变为传入单一点并自动绘制_

  ```javascript
  const linkLine = d3
    .linkHorizontal()
    .x((d) => d.y)
    .y((d) => d.x)
  svg
    .append('g')
    .selectAll('path')
    .data(树状图生成器.links())
    .join('path')
    .attr('d', (d) => linkLine(d))
    .attr('fill', 'none')
    .attr('stroke', 'black')
  ```

  - selection.context(context) 传递 canvas 画笔,绘制方式与其他函数类似
  - selection.digits(num) 决定其返回的路径字符串浮点数精确到小数点后几位(默认三位)

### 符号绘制函数

- d3.symbol() d3 内置了一些列的符号数据,可以直接传入该函数,调用该函数会调用 canvas 画笔绘制或直接返回符号 svg 路径字符串,内置符号[详见](https://d3js.org/d3-shape/symbol#symbolsFill),如果不传递数据,函数将绘制一个圆

  ```javascript
  svg.append('path').attr('d', d3.symbol(d3.symbolCross)) // 绘制一个十字
  ```

  - selection.type() 内部传递符号数据,和直接在 symbol()函数中传递符号数据一样
  - selection.size() 设置符号大小
  - selection.context(context) 传递 canvas 画笔,绘制方式与其他函数类似
  - selection.digits(num) 决定其返回的路径字符串浮点数精确到小数点后几位(默认三位)

### Voronoi 图绘制函数

#### 德劳内图

- d3.Delaunay.from() 内部传递四个参数(array,fx,fy,that) array 是 xy 值组成的数组,fx 和 fy 是决定如何获取 xy 值的函数(当 xy 的形式为\[x,y]时,不需要传递 fx 和 fy) 返回由 xy 构成的点组成的德劳内三角剖分(Voronoi 图的基础)

  下方所有的 delaunay 均为已经建立了德劳内三角剖分的对象

  - delaunay.find(x,y,i) 返回与 xy 点最近的德劳内点的索引,i 为起始点(默认为 0)
  - delaunay.neighbors(i) 返回索引点 i 附近所有点的索引

  - **delaunay.render(context)** 将德劳内三角的所有边绘制在 canvas 上下文,如果没有传递 canvas 画笔,则返回 svg 路径字符串

  - **delaunay.renderHull(context)** 将德劳内最外层点连接起来,形成一个包裹了所有德劳内点的封闭图形,如果没有传递 canvas 画笔,则返回 svg 路径字符串

    - delaunay.hullPolygon() 返回构成封闭图形的所有点的 xy 值组成的数组

  - **delaunay.renderTriangle(i,context)** 将指定索引的德劳内三角图形返回,可以对其进行填充等操作,_注意,填充等操作需要后续代码指定,这个函数返回的只是索引指定的图形_,如果没有传递 canvas 画笔,则返回 svg 路径字符串

    - delaunay.trianglePolygons() 按顺序返回所有三角形的每个点的 xy 值组成的数组(前提是已经调用过 renderTriangle()函数)
    - delaunay.trianglePolygon(i) 返回索引指向的三角形点的 xy 值组成的数组

  - **delaunay.renderPoints(context, radius)** 将所有德劳内点绘制在 canvas 上下文,radius 表示点的半径,如果没有传递 canvas 画笔,则返回 svg 路径字符串(_注意,所有点返回一个路径字符串_)

  - delaunay.update() 更新德劳内三角剖分

### voronoi 图

voronoi 图的函数由德劳内图函数构建

- delaunay.voronoi([xmin, ymin, xmax, ymax]) 返回 voronoi 图函数,参数为图的绘制范围
  下方所有的 voronoi 均为已经构建了 voronoi 图的对象

  - voronoi.delaunay 指向构建了 voronoi 对象的 delaunay 对象
  - voronoi.circumcenters 返回 voronoi 图每个图形原型的坐标 Float64Array 数组
  - voronoi.vectors 也返回一个 Float64Array 数组,但是我没搞清楚是什么的坐标

  - voronoi.contains(i, x, y) 如果包含 xy 点的多边形索引为 i,则返回 true,否则返回 false

  - voronoi.neighbors(i) 返回索引 i 附近几个点的索引

  - **voronoi.render(context)** 将 voronoi 图每个细胞网格绘制在 canvas 上下文中,如果没有传递 canvas 画笔,则返回 svg 路径字符串

  - **voronoi.renderBounds(context)** 在 voronoi 图外,按照此前设定的图绘制范围绘制一个方框,如果没有传递 canvas 画笔,则返回 svg 路径字符串

  - **voronoi.renderCell(i, context)** 将指定索引的 voronoi 细胞图返回,可以对其进行填充等操作,_注意,填充等操作需要后续代码指定,这个函数返回的只是索引指定的图形_,如果没有传递 canvas 画笔,则返回 svg 路径字符串

  - voronoi.cellPolygons() 返回每个多边形的坐标组成的数组

  - voronoi.cellPolygon(i) 返回指定索引多边形点坐标组成的数组

  - voronoi.update() 更新 voronoi 图

### 地图绘制函数

#### 地图投影方式函数

地图投影是利用一定数学法则把地球表面的经、纬线转换到平面上的理论和方法。
由于地球是一个赤道略宽两极略扁的不规则的梨形球体，故其表面是一个不可展平的曲面，所以运用任何数学方法进行这种转换都会产生误差和变形，为按照不同的需求缩小误差，就产生了各种投影方式。
对于 D3js 的地图投影方式函数来说,就是把地图坐标数据转换为了屏幕坐标数据.

- D3js 中一共提供了十二种地图投影方式
  1. d3.geoAlbers() 返回阿伯斯投影函数 阿伯斯投影不支持旋转和设定中心
  2. d3.geoAzimuthalEqualArea() 返回等面积方位投影函数 等面积方位投影也适合等值线图，这个投影的极坐标方向被用来作为联合国图标。
  3. d3.geoAzimuthalEquidistant() 返回等距方位投影函数 等距方位投影保存着投影中心，从投影上的不论什么点到投影中心的弧线距离都是成比例的。因此。圆形的投影环绕着投影中心被投影在用圆圈住的一个笛卡尔平面上。这能够用于将距离參考点的数据可视化显示。比方通勤距离。
  4. d3.geoConicConformal() 返回圆锥共形投影函数 将地球投影在一个圆锥上
  5. d3.geoConicEqualArea() 返回圆锥等面积投影函数 作为一种等面积投影，被推荐作为等值线图。由于它保留了相对区域的地理特征
  6. d3.geoConicEquidistant() 返回圆锥等距投影函数
  7. d3.geoEquirectangular() 返回相等矩形投影函数 相等矩形投影，或者叫普拉特方形投影（貌似这是法国的叫法），是最简单可能的地理投影：标记功能（正比例函数。都是网上释义，我也搞不清楚这里的意思）。它既不等面积也不保持形状，只是有时候用于栅格数据。
  8. d3.geoGnomonic() 返回球心投影函数 球心投影是一种方位投影。它连续不断地投影一个巨大的包围圈(从地球仪中心看地图)
  9. d3.geoMercator() 返回墨卡托投影函数
     球形的墨卡托投影在映射平铺的数据时是最经常使用的，比如用墨卡托投影显示栅格，[例如](http://bl.ocks.org/mbostock/4150951) 它是正形的，然而，它将地图上的很多地方进行了严重变形，因此。不建议使用等值线图
  10. d3.geoOrthographic() 返回正射投影函数 正射投影也是一种方位投影。它适合显示一个半球：角度无穷大
  11. d3.geoStereographic() 返回极射赤平投影函数 极射赤平投影是还有一个角度的方位投影。它的视角相当于站在地球的表面向里面看（跟球心投影相反），它因此被经经常使用于天体图。
  12. d3.geoTransverseMercator() 返回横向墨卡托投影函数 就是一个横向的墨卡托投影。

#### 地图绘制函数

- d3.geoPath(projection, context) 返回地图绘制函数,projection 为地图投影方式函数(),不传入 canvas 画笔将用于 svg 图形的绘制
