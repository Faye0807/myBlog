---
title: css居中
date: 2018-08-20 15:06:52
tags: CSS
description: 使用css居中的几种方法
keywords: css 居中
categories: CSS
---

> css居中的方法有多种，需要居中的情形也有多种，下面就来总结一下

### `inline-block` 的居中
- 多个`inline-block` 的水平居中`text-align: center`
- 多个`inline-block` 的垂直居中`vertical-align: middle`

```html
<ul class="list-outer">
  <li class="list-item">1</li>
  <li class="list-item">2</li>
  <li class="list-item">3</li>
</ul>
```
``` LESS
.list-outer{
  margin: 0;
  padding: 0;
  color: #fff;
  text-align: center;
  background: #999;
  .list-item{
    display: inline-block;
    width: 100px;
    background: #1246a4;
    vertical-align: middle;
    &:first-child{
      height: 30px;
    }
    &:nth-child(2){
      height: 50px;
    }
    &:last-child{
      height: 100px;
    }
  }
}
```
![效果](/teresa/images/inline-block.jpg)

### 块元素的居中
> 无论父元素、子元素的宽度是否都已知，都可以通过`margin: auto；`来水平居中
> 注意margin、padding的百分比是基于其包含块宽度的百分比

#### 父元素、子元素定宽定高
- 通过设置margin值，计算出垂直、水平居中的margin值达到居中的效果

```html
  <div class="block-outer">
    <div class="block-inner">inner div</div>
  </div>
```
 
```css
.block-outer{
  width: 300px;
  height: 300px;
  margin: 50px auto;
  background: #999;
  overflow: hidden;
}
.block-inner{
  width: 80%;
  height: 150px;
  background: #1246a4;
  /* margin: 75px 30px; */
  /* margin padding 的百分比值是取自父元素大小的百分比 */
  margin: 75px 10%;
}
```
![效果](/teresa/images/block_1.jpg)

#### 父元素大小未知、子元素大小确定
> 知识点： absolute定位的top、right等值，百分比时，取自其相对元素的宽度、高度的百分比；

- 父元素相对定位；子元素绝对定位 + margin负值

```css
/* 父元素大小未知 */
.block-outer{
  position: relative;
  width: 300px;
  height: 300px;
  margin: 50px auto;
  background: #999;
}
.block-inner{
  position: absolute;
  top: 50%;
  left: 50%;
  width: 80%;
  height: 150px;
  margin: -75px -40%;
  background: #1246a4;
}
```
#### 父元素、子元素大小未知
- 父元素相对定位；子元素绝对定位(子元素left、right、bottom、top均为0) + `margin: auto`

``` css
.block-outer{
  position: relative;
  width: 300px;
  height: 300px;
  margin: 50px auto;
  background: #999;
}
.block-inner{
  position: absolute;
  top: 0;
  left: 0;
  width: 80%;
  height: 150px;
  margin: auto;
  background: #1246a4;
}
```
- 父元素相对定位；子元素绝对定位 + 位移

> 知识点： 定位百分比的值取自“其父元素”，而位移变换的百分比，取自作用当前元素的百分比

``` css
.block-outer{
  position: relative;
  width: 300px;
  height: 300px;
  margin: 50px auto;
  background: #999;
}
.block-inner{
  position: absolute;
  top: 50%;
  left: 50%;
  width: 80%;
  height: 150px;
  transform: translate(-50%, -50%);
  background: #1246a4;
}
```
- 父元素flex

```css
.block-outer{
  display: flex;
  width: 300px;
  height: 300px;
  margin: 50px auto;
  background: #999;
  align-items: center;
  justify-content: center;
}
.block-inner{
  width: 80%;
  height: 150px;
  background: #1246a4;
}
```

#### 其它居中方法
- 父元素`table-cell`+`vertical-align`;

```css
.block-outer{
  display: table-cell;
  width: 300px;
  height: 300px;
  background: #999;
  vertical-align: middle;
}
.block-inner{
  width: 80%;
  height: 150px;
  background: #1246a4;
  /* 水平居中 */
  amrgin: 0 auto; 
}
```

- 添加一个伪元素，作为对照物+`vertical-align`来垂直居中;

```css
.block-outer{
  width: 300px;
  height: 300px;
  margin: 50px auto;
  background: #999;
  /* 水平居中 */
  text-align: center;
}
.block-outer::after{
  display: inline-block;
  content: '';
  height: 100%;
  vertical-align: middle;
}
.block-inner{
  display: inline-block;
  width: 80%;
  height: 150px;
  background: #1246a4;
  vertical-align: middle;
}
```