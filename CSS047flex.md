# 特殊性质

- overflow 行为的冒泡
  在特定情况下，给 body 或 html 元素设置 overflow 属性，相当于给视口设置
  所以此时给 body 设置 overflow hidden 相当于高视口不出现滚动条，即使有内容在视口以外
- 背景颜色的冒泡
  给 html 元素设置 bgc 或 ov 就相当于给视口
  而在 html 元素未设定这两个属性时，给 body 设置就相当于给 html 元素设置

# Flex 布局

“大号的行内布局”
flex 布局是内部布局

- 普通块元素里的 flex 元素的外在表现就像一个块一样

- justify-content: space-between|space-around|start|end|center;
  设定主轴方向上额外空间的分配,当额外空间没被元素扩张占满时才有效,在在 grow 之后才生效

- align-content: space-between|space-around|stretch|start|end|center;
  设定交叉轴方向上额外空间如何分配给 flex"行"

- align-items: stretch|start|end|center;
  设定每个 flex 子元素在"行"中的垂直摆放

- align-self: stretch|start|end|center
  设定给 flex 子元素,单独调整这个元素在 flex"行"中的垂直摆放,取值跟 align-items 一样

- flex-basis: ;
  当主轴水平时,它代表宽度,如果此时也设定了元素的宽度,那只要 flex-basis 不是 auto 就是 flex-basis 生效,否则就是高度生效
  当主轴垂直时,它代表高度,如果此时也设定了元素的高度,那只要 flex-basis 不是 auto 就是 flex-basis 生效,否则就是高度生效

- flex-flow: ;一次性设定 flex-direction 和 flex-wrap;
  - flex: ;一次性设定 flex-grow, flex-shrink, flex-basis;
  - flex: auto|1|500px|none;
  - gap: ; 设定 flex 子元素之间的间隙
- row-gap: 行水平间隙;
  column-gap: 行垂直间隙;

- order: number; 设定 flex 子元素的布局顺序

- 注意

  - flex 子元素可以直接使用 z-index,在负 margin 让它们重叠的时候

  - flex 子元素不能浮动,因为浮动需要发生在普通块/行内上下文里
  - flex 父元素可以认为触发了 bfc,会水平方向避开浮动元素

  - 主轴垂直时,想要让元素折行,需要给 flex 父元素定高

  - flex 子元素 auto 的 margin 会被计算为相同的,即便是垂直方向上,但是左和上还是不能计算为负值

  - flex 父元素中的匿名文本,可以想象成被标签包起来,成为一整个 flex 子元素

  - 额外空间的分配:
    首先分给 flex-grow 的元素,然后分给相应方向上 auto 的 margin,再然后才是留白

  - 扩张与收缩是发生在折行之后
    所以无论如何设定 grow 与 shrink,都不会改变元素在不同行之间的分配
    折行之后不会发生收缩,因为空间不够就折行了,除非一行的唯一一个元素比包含块宽

- [flex 布局查询](https://www.runoob.com/w3cnote/flex-grammar.html)
