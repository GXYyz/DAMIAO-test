transition 属性可以设定元素在样式发生变化时，不是突然变到目标状态，而是缓动到目标状态
transition-property 设定哪些属性要缓动
transition-duration 设定每个属性缓动的时间长度
transition-delay 设定每个属性在执行缓前等待的时间，为负值，缓动动画从已经执行了那么久的位置开始播放

transition-timing-function 设定缓动的距离时间函数图像，默认值为 ease，自带 ease ease-in ease-out ease-in-out,还可以通过 cubic-bezier(p1x,p1y,p2x,p2y)的方式指定一条贝塞尔曲线做为其运动曲线，还有 steps(n)进行步进式变化

简写形式为
transition: width 2s -1s linear, height 5s 2s;
