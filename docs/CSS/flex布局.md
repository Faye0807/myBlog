> 参考链接
> - [flex布局教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?^%$)
> - [深入理解css3中的flex-grow、flex-shrink、flex-basis](http://zhoon.github.io/css3/2014/08/23/flex.html)

## 布局方案
传统的布局解决方案： 基于基本的盒子模型，依赖 `display` 属性 + `position` 属性 + `float` 属性；
现在还有了 `flex` 布局（弹性布局）；

任何一个元素都可以指为 flex 布局

兼容性：
- Chrome 21+ 
- Opera 12.1+
- Firefox 22+
- Safari 6.1+（-webkit-）
- IE 10+

## 容器属性
- flex-direction（决定主轴的方向，即项目的排列方向）
  - row（默认，水平+自左向右）
  - row-reverse（水平+自右向左）
  - column（垂直+自上而下）
  - column-reverse（垂直+自下而上）
- flex-wrap（决定如果一条轴线排不下，怎么换行，只决定行的排列，与每条主线上项目的排列无关）
  - nowrap 默认，不换行
  - wrap 正常排序换行 多出的一行在下面
  - wrap-reverse 反序 多出的一行在上面（会影响交叉轴的方向）
- flex-flow（direction和wrap的简写）
  - direction || wrap(无所谓两个属性的顺序 及 是否必填)
- justify-content（决定项目在主轴上的对齐方式）
  - flex-start 默认 左对齐
  - flex-end 默认 右对齐
  - center 居中
  - space-between 两端对齐，项目之间的间隔相等
  - space-around 效果类似，每个项目的左右margin相等 所以项目间的间隔要比项目与父元素边框的距离大一倍
- align-items(觉得项目在交叉轴上如何对齐；比如水平排列时项目在垂直方向上的排序)
  - flex-start 交叉轴的起点对齐
  - flex-end 交叉轴的终点对齐
  - center 交叉轴中点对齐
  - baseline 项目第一行文字的底部基线对齐
  - stretch 默认 如果项目没有设置高度或者为auto，将占满整个容器的高度
- align-content（决定多条轴线（主轴）的对齐方式；如果只有一条轴线，该属性不起作用）
  - flex-start 交叉轴的起点对齐
  - flex-end 交叉轴的终点对齐
  - center 交叉轴中点对齐
  - space-between 交叉轴两端对齐，轴线间的距离均分
  - space-around 效果类似每个轴线的上线margin相等，所以轴线与父元素边框的距离会比轴线间的距离小一半
  - stretch 默认 如果项目没有设置高度或者为auto，将占满整个容器的高度

## 项目的属性

- order
  - 决定项目的排列顺序，数值越小排列越靠前，默认为0 可以负值
- flex-grow
  - 定义项目瓜分容器剩余空间的比例 默认为0即不瓜分剩余空间
- flex-shrink
  - 定义了项目的缩小比例（即空间不足的时候）默认为1（0表示不缩小） 负值无效
- flex-basis
  - 定义在分配多余空间前，项目占据的主轴空间，默认为auto即项目的本来大小（其实跟width、height效果差不多）
- flex 
  - flex-grow、flex-shrink、flex-basic的简写 默认 0 1 auto
  - auto（1 1 auto）
  - none（0 0 auto）
- align-self
  - 定义单个项目的对齐方式，覆盖align-items 属性