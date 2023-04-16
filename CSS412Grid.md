# grid 网格布局

- 设置 display:grid;的元素就称为 grid 容器
- grid 容器的子元素自动就成为了 grid 元素
- grid 布局是二维布局(其他布局都是一维)
- grid 上下文的元素不能浮动, 浮动元素只能存在于块级上下文中
- grid 子元素也可以直接使用 z-index
- grid 子元素也可以使用 order 来调整自己的布局顺序

## grid 布局由网格行、网格列、网格线、网格单元、网格区域组成

- 网格线存在但不可见,纵向和横向的网格线都有各自的编号
  - 从 grid 容器的左边/上边线开始为 1,一直到右边/下边线结束
  - 反方向从-1 开始负值编号
  - 网格线编号永远比网格单元多 1
- 网格单元就是由各网格线包围的区域
- 网格区域是由 N 个网格单元组成的矩形区域
  - grid 子元素的包含块不是父元素自身而其所在的网格区域

## 设置每行每列的大小

- grid-template-column 设置每列的大小
- grid-template-row 设置每行的大小
- 有几行或几列就要写几个数值(其中可以使用 calc() 计算函数)
- 单位可以是数值、百分比(以容器大小为基准)、fr(自适应)
  fr = fraction 一部分;一份 会根据行或列的剩余空间的分配
- 简写 grid-template: 每行的高度 / 每列的宽度;

## 设置跨行跨列(生成网格区域)

- 写在子元素中,指定某个元素行或列的起始和结束位置,数字代表网格线的编号
- grid-column-start: 2; 设置子元素列的开始
  grid-column-end: 4;设置子元素列的结束,span n 表示跨越多少列
  grid-column: 2 / -2; 简写

- grid-row-start: 2; 设置子元素行的开始
  grid-row-end: span 3;设置子元素行的结束

  - span n 表示从起始位置开始跨越几行/几列
    grid-row: 2 / span 3; 简写

- 简写 grid-area: row-start / column-start / row-end /column-start

## repeat()函数 用于填充单位相同的多行或多列

- repeat(重复次数,需要重复的数值)
  Eg:grid-template-columns: 1fr repeat(11, 10px 1fr);
  表示重复 10px 宽度和 1fr 宽度的两个元素十一次

- 可以配合 auto-fit 或 auto-fill 自适应填充的数量
  grid-template-columns: repeat(auto-fit, 50px);
  每列 50px,自动适配列的数量

- 还可以配合 minmax()函数设置重复的数值区间
  grid-template-columns: repeat(auto-fit, minmax(50px, 1fr) );每列至少 50px,均分剩下的空间

- 当 auto-fit 或 auto-fill 与 minmax 共用时的区别
  当容器宽度大于子元素的最小宽度总和时才有显示区别

  - repeat(auto-fill, minmax(50px, 1fr) );
    每列至少 50px
    即如果容器宽 800,这几乎等价于:
    1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr
  - repeat(auto-fit, minmax(50px, 1fr) );
    即如果容器宽 800,这几乎等价于(当后三列没有元素时):
    1fr 1fr 1fr 1fr 1fr 0fr 0fr 0fr
    每列至少 50px,但是没有元素的列宽度将会被折叠为 0,这些宽度给到其他列.
  - auto-fill 和 auto-fit 一开始做的事情是一样的就是尽可能的分配轨道数量，区别在于后面空轨道是否会折叠为 0。auto-fill 不折叠空轨道，auto-fit 折叠空轨道。

## 设定自动布局算法方式 grid-auto-flow

- grid-auto-flow: column/row dense;
  设定元素的流向是“从左往右再从上到下”还是“从上到下再
  从左往右”
- 增加 dense 关键字的效果是该指定自动布局算法使用一种"稠密"堆积算法，如果后面出现了稍小的元素，则会试图去填充网格中前面留下的空白。这样做会填上稍大元素留下的空白，但同时也可能导致原来的元素次序被打乱。
  - row dense 按行来填充网格中前面留下的空白
  - column dense 按列来填充网格中前面留下的空白

## 指定行或列的默认尺寸

只影响未设置大小的行或列

- grid-auto-rows: 100px; 没有设置高度的的行的默认高度为 100px
  当 grid-auto-flow 为 row 时生效
- grid-auto-columns: 80px; 没有设置宽度的的列的默认宽度为 100px
  当 grid-auto-flow 为 column 时生效
- 除数值外,还可以设置 max-content /min-content 来设置元素默认尺寸
  根据其中的最大/最小的网格元素设置尺寸

## 命名网格布局

在父元素中设置 grid-template-areas 矩阵,可以直接为子元素规定其 cell 的大小及位置

- grid-template-areas 以给每个网格命名的方式设置网格布局
- grid-area 以设置对应网格命名的网格区域

```css
.grid-container {
  grid-template-areas:
    'header header header header header header'
    'menu   main   main   main   right  right'
    'menu   footer footer footer footer footer';
} /* 设置一个三行六列的网格 */

.item1 {
  grid-area: header;
} /* 该子元素会以整个第一行为网格区域 */
.item2 {
  grid-area: menu;
} /* 该子元素会以父元素命名矩阵中menu的大小和位置来作为其cell */
.item3 {
  grid-area: main;
}
.item4 {
  grid-area: right;
}
.item5 {
  grid-area: footer;
}
```

## 其他布局属性

- align-items: ;
  控制每个 item 在 cell 中的垂直位置
- justify-items: ;
  控制每个 item 在 cell 中的水平位置
  简单属性：place-items:;

- justify-content:
  统一控制子元素的 justify-self
- align-content:
  控制垂直方向额外空间如何分配的
  简单属性：place-content:;

- align-self: ;
  单独设定 item 在 cell 中的垂直摆放
- justify-self: end;
  单独设定 item 在 cell 中的水平摆放
  简单属性：place-self:;

- grid 居中布局
  父元素设置 display:grid 子元素设置 margin:auto
