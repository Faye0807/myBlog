---
title: CSS细节简记
date: 2018-10-15 16:43:23
tags: CSS
description: 记录各类属性等细节，以备随时查询
keywords: CSS
categories: CSS
---
# LESS 文档
> http://www.css88.com/doc/less/features/

```less
// Variables
@images: "../img";
@themes: "../../src/themes";
@mySelector: banner;
@property: color;
@fnord:  "I am fnord.";
@var:    "fnord";
//变量名
content: @@var; // I am fnord
// URL
body {
  color: #444;
  background: url("@{images}/white-sand.png");
}
// import
@import "@{themes}/tidal-wave.less";
// 选择器
.@{mySelector} {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}
// 属性
.widget {
  @{property}: #0ee;
  background-@{property}: #999;
}
```

- 变量可以用于变量名、选择器、URL、impor导入
- 块级作用域链，子作用域链没有回继续向负作用域查找
- 变量的声明可以放在最后（延迟加载原因）不会影响变量的使用

# overflow
控制的是 `border` 以内的内容；比如 `overflow: hidden;`  `border` 以外的内容才会被隐藏（即 `padding`,`content` 的部分不会被隐藏， `margin` 部分会被隐藏


# position

当 `position` 值为 `absolute` 时，如果不设置 `top left` 等值，绝对定位的元素 在 `content` 的左上角边界排放

```html
<div class="formargin">
  <span class="oh">一个额外的内容哦哦哦哦哦</span>
  <div class="content">
  这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章这一一个很长的文章
  </div>
</div>
```
```css
.formargin{
  position: relative;
  margin: 200px;
  background: palevioletred;
}
.formargin .oh{
  position: absolute;
  background: red;
}
```
- 没有设置top等值
![没有设置top等位置值](/teresa/images/position0.png)
- top=0;left=0;
![top=0;left=0](/teresa/images/position_topLeft.png)

# 如何把元素从页面消失

- removeChild 把元素从文档删除
- display: none
- 将元素移动至容器以外 容器：overflow: hidden;
- visibility: hidden; 只是不可见，仍然占据空间
- width,height等设置为0
- transform: scale(0) 缩小到消失