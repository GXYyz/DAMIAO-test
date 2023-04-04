# less/sass

- css 变量，又称 css 自定义属性
  即双中划线开头的属性
  目的是为了不跟 css 自带属性冲突/重叠/重复
  所有的 css 自定义属性都是自动继承的
  变量的声明就是通过双中划线开头的属性
  变量的使用/引用就是通过 var(--variable-name)
  var 函数可以有第二个参数，当这个变量不存在时，其将会使用第二个参数
  var 函数可以放进 calc 里参与 calc 支持的运算

- 与 sass less 变量的区别：
  sass/less 的变量会预处理阶段就运算完成，浏览器看到的时候已经没有变量了，已经是计算完成的内容了。所以不能混合单位使用，因为 sass/less 在处理时根本不知道有些单位的具体值是多少。
  css 的变量是浏览器直接支持的，是在运行过程中执行运算的，可以混合单位使用

# 颜色与背景

- color

  - 颜色 前景色（与之对应的则是背景色）
    - 一般画图工具中都有类似对应的图标
  - 默认为黑色
  - 会被子元素继承
    - 所以设定一个元素的颜色，其子元素都将是这个颜色
      - 这是很明显的(#333)
  - 会做为 border，text/box-shadow 的默认值

    - text-shadow: 2px 3px 3px;
    - box-shadow: 5px 10px 5px ;
    - border: 10px solid;

  - css3 的 currentColor
    - 用在其它属性上比如 bgc 上，或者 linear-gradient 等

- background
  - background-color
    - 背景色
      - 默认值为 transparent ，即透明
    - 不继承
      - 否则会有奇怪的效果，比如如果设置了 semi 透明颜色，而且又继承的话。。
  - background-image
    - url()
    - 默认从 padding box 开始渲染（画）的
    - 背景图片无法从网页上直接复制
  - background-size
    - https://developer.mozilla.org/en-US/docs/Web/CSS/background-size#Browser_compatibility
    - cover 图片由无穷大缩小到正好覆盖元素
    - contain 图片由无穷小放大到正好被元素包围
    * object-fit 用在图片/视频元素上的 css 属性
      - img
      - video 等
    - 如果 attachment 为 fixed，背景区为浏览器可视区（即视口），不包括滚动条。不能为负值。
  - background-repeat
    - background-repeat
      - repeat
      - repeat-x/y
      - no-repeat
  - background-origin css3
    - content-box
    - padding-box
    - border-box
    - 与 box-sizing 的关键字是对应的
  - backgorund-attachment
    - scroll 相对于元素自身不动
    - local 相对于元素的内容不动
      - 为此值时 bg-size 的百分比以元素内容的大小来计算
    - fixed 相对于视口不动
      - 为此值时 bg-size 的百分比以浏览器窗口的大小来计算
      - 可以用来做视差滚动
      - http://www.mi.com/xiaoyi/?cfrom=list
  - background-position
    - background-position-x/y
    - 雪碧图，css sprite
    - 0 0
    - center 200px
    - 100px
    - 0px 10px 相对于左上角
    - 50% 30% 相对于左上角
    - top left /// right bottom 让图片处于某个角落
    - top 20px right 50px 相对于右上角，往元素中心水平偏 50px，垂直偏移 20px
    - calc(100% - 50px) 从最右往多偏移 50px
  - background-clip
    - xx-box
    - 平铺以后再裁剪
  - -webkit-background-clip
    - text
  - css3 多背景
    - 分开写，合并写
  - background: <bg-img> <bg-repeat> <bg-origin> <bg-size> / <bt-pos> , <bg-img> <bg-repeat> <bg-origin> <bg-clip>, <bg-img> <bg-repeat> <bg-origin> <bg-clip> bg-color;
  - 应用
    - 伪元素里的图片，
    - css sprite
    - 动画，菜单，小米网首页 logo 动画
    - 视差滚动：小蚁摄像机页面效果，Nike 活动页面效果
    - 多背景做花纹
