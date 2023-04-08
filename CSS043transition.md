# transition 动画

- 为元素设置过渡效果
- 注意:
  只能从起始状态过渡到结束状态
  和 transition 写在一起的其他属性表示过渡完成后的元素状态
  如:
  ```css
  div {
    background-color: pink;
    transition: background-color 1s;
  }
  ```
  表示 div 的背景颜色过渡到粉色需要 1 秒钟
  ```css
  input:checked {
    margin-left: 119px;
    margin-right: 7px;
    transition: margin-left 0.6s, margin-right 0.5s;
  }
  ```
  表示 input 被选中之后,margin-left 需要 0.6s 从原状态变为 margin-left: 119px;;margin-right 需要 0.5s 从原状态变为 margin-right: 7px;

## transition 的属性

- transition-property 设定哪些属性要缓动
- transition-duration 设定每个属性缓动的时间长度
- transition-delay 设定每个属性在执行缓前等待的时间，为负值，缓动动画从已经执行了那么久的位置开始播放

- transition-timing-function 设定缓动的距离时间函数图像，默认值为 ease，自带 ease ease-in ease-out ease-in-out,还可以通过 cubic-bezier(p1x,p1y,p2x,p2y)的方式指定一条贝塞尔曲线做为其运动曲线，还有 steps(n)进行步进式变化

- 简写形式为
  transition: 要变化的属性名 过渡时间 过渡延迟时间 过渡速度曲线
  如:transition: width 2s -1s linear, height 5s 2s;
