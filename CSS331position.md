# 定位布局：

## 定位使用 position 属性触发

- fixed 固定定位，脱离常规流，相对于窗口/视口定位
  单纯固定定位的元素超出视口浏览器不会出现滚动条
- absolute 绝对定位，脱离常规流，相对于最近的定了位的父元素的 padding-box 定位
  - 什么叫“定了位的”？即 position 属性不为 static 的祖先
  - 如果找不到定位祖先，则相对于第一屏(浏览器首次渲染出的页面)
- relative 相对于定位，不脱离常规流(常规流里的位置为其保留)，相对于自身原来的位置定位
- sticky 黏粘定位，综合了以上三种定位方式

  - 当其包含块还处于视口中,但被定位的元素将要离开视口时,定位生效,它原本所占的空间仍然保留
  - 当其包含块离开视口之后,元素定位也失效,被定位的元素回到原本的位置

- 何为“脱离常规流”
  - 即其父元素、后面的兄弟元素感知不到它，或者说当它不存在
  - 对其他元素的布局不产生影响

## 定位位置通过 top bottom left right 来指定

- 如果不通过以上属性指定元素的位置(即默认值 auto)，则元素的起始点在自己本来应该在的起始点
  如果 margin 设置为 auto 上下或左右 margin 则会被计算为相同的值
  但是如果相应方向的宽/高也设置 auto 则相应方向的 margin 会被计算为 0
  基本上是同块元素水平方向的
- 使用绝对定位实现垂直居中效果
  margin 0 top 0 bottom 0

## 元素的层叠：

- 当不设置 z-index 的时候
  - 类型相同的元素后盖前
  - 类型不同的元素，定位元素盖住常规流元素
- 有 z-index 时，z-index 越大，元素越往上
  - 目前来讲 z-index 只能用在定位元素上
  - 当父子元素都定位时，父元素无法通过 z-index 盖住子元素，此时总是子元素盖住父元素

# 伪元素

每个非自闭合元素都有一前一后两个伪元素，相当于该元素的第一个和最后一个子元素

- div::after 选中 div 的所有子元素前面的伪元素
- div::before 选中 div 的所有子元素前面的伪元素
- 通过伪元素选择器选中伪元素后必须要设置 content 属性伪元素才会出现。

## 注意

- 伪元素里面不能再有伪元素了
- 伪元素不能被 hover
  div::after:hover {无效}
  div:hover::after {有效,当 div 被 hover 时，其伪元素如何}
- 又由于伪元素不能有后代，所以伪元素如果出现在选择器里，一定是最后一项
- 伪元素可以实现的效果几乎都可以通过在 html 中增加相应的真实元素来实现
- 伪元素的内容无法被选中或复制
- 伪元素一般用于没有单独交互效果的统一装饰性元素

- 伪类与伪元素的区别：
  伪类是真实存在的元素隐含的信息
  伪元素是没在 html 代码中书写的元素凭空出现

## 更多伪类

- :enabled 被启用的可交互元素
- :disabled 被禁用的可交互元素
  :enabled 不等价于 :not(:disabled)

- :read-only 匹配内容只能读的元素，包括普通元素
- :read-write 匹配内容可以读写（修改）的元素

- :checked 匹配被选中的元素（主要是 radio 和 checkbox）
- :indeterminate 匹配处于中间状态的 checkbox 如全选框的中间状态\

- :valid 填写正确的文本框
- :invalid 填写正确的文本框
  比如在 type=email 的 input 中填写了电话号码的文本框

- :required 必填项
- :optional 选填项

- :any-link 匹配任意链接（即有 href 的 a 标签）

- :target 目标元素：即 id 的值为地址栏#后面的内容的元素，或是页面内跳转到的元素
- :target-with 匹配内部有目标的元素
- :focus-within 匹配光标所在元素的祖先们

- :is()匹配所有符合条件的元素 多个同时使用的话有交叉相乘的效果
  如:
  :is(div,section,.foo) :is(i,span,em) {}
  基本等价于：
  div i,
  div span,
  div em,
  section i,
  section span,
  section em {}

- :has()匹配处于某种状态的元素
  如
  div:has(>:hover) {选中有子元素被 hover 的 div}
  div:has(:hover) {选中有后代元素被 hover 的 div}
  div:has(+:hover) {选中后面一个元素被 hover 的 div}
  div:has(~:hover) {选中后续有兄弟元素被 hover 的 div }
