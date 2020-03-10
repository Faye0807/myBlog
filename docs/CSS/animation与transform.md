---
title: animation与transform、transition
date: 2018-10-11 10:14:08
tags: CSS
description: animation与transform、transition
categories: CSS
keywords: animation与transform、transition
---
# transform 变换
## 倾斜：skew(x-angle,y-angle),skewX(angle),skewY(angle)
- skewX(angle)：元素高不变，x方向即宽被拉长
- skewY(angle)：元素宽不变，y方向即高被拉长
- skew(x-angle,y-angle)是前两个的结合，
- skew(x deg,-x deg)会产生一个逆时针旋转x度且被放大的效果
- 角度值可以是负值，skewX(10deg)与skewX(-10deg)倾斜角度关于y轴对称；
- skewX(90deg)、skewX(-90deg)、skewX(270deg) 元素消失
- skewX(91deg) == skewX(-89deg)
## 示例
原图

![原图](https://static.oschina.net/uploads/space/2018/0304/201402_dIRA_2994006.png)

transform:skewX(30deg)

![skewX(30deg)](https://static.oschina.net/uploads/space/2018/0304/185713_dZOS_2994006.png)

transform:skewY(30deg)

![skewY(30deg)](https://static.oschina.net/uploads/space/2018/0304/185928_Yinh_2994006.png)

transform:skew(30deg,-30deg)

![skew(30deg,-30deg)](https://static.oschina.net/uploads/space/2018/0304/190910_UAS2_2994006.png)

ransform:skew(-30deg,30deg)

![skew(-30deg,30deg)](https://static.oschina.net/uploads/space/2018/0304/190946_zIRD_2994006.png)

## 旋转：rotate(angle),rotateX(angle),rotateY(angle),rotateZ(angle)（与rotate(angle)效果相同）
绕对应轴旋转
rotate() == rotateZ() 在平面内顺时针旋转
## 缩放：scale(x,y),scaleX(x),scaleY(y),scaleZ(z)
沿对应轴方向缩放（根据变换原点不同，方向不同；比如origin是left的时候 x轴方向的缩放是向右缩放；若默认在元素中心，则x轴的缩放是从中心向两边同时缩放）
z方向的缩放2d看不出效果
接受值：数值 不接受百分比
## 移动：translate(x,y), translateX(x), translateY(y), translateZ(z), translate3d(x,y,z)
沿对应轴方向位移
z方向2d变换看不出效果
接受值： 30px或者百分比（百分比是以自身宽高为基数）
## transform-origin：更改一个元素变形的原点
transform-origin: left;
transform-origin: 20px;
transform-origin: 40px  left;(水平，垂直)

# transition 过渡
> [参考](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions)

```css
transition: <property属性名> <duration过渡时长> <timing-function过渡时间线> <delay>;
```
- transition-property: width, left;
- transition-timing-function
  - ease 中间快两端慢
  - linear 匀速
  - step-end 直接跳到最后一帧
  - step(4,end) 分四步完成过渡
- transition-duration: 3s
- transition-delay: 3s
可以合并写多个属性

```css
transition:width 2s, height 2s;
```

# animation 动画
> [参考](http://www.runoob.com/cssref/css3-pr-animation.html)

```css
animation: name duration timing-function delay iteration-count direction fill-mode play-state;
```

- animation-name	指定要绑定到选择器的关键帧的名称
  - 默认值：none
- animation-duration	动画指定需要多少秒或毫秒完成
  - 3s
  - 默认 0
- animation-timing-function	设置动画将如何完成一个周期
  - linear 匀速
  - ease 默认 中间快两边慢
  - ease-in 低速开始
  - ease-out 低速结束
  - ease-in-out 低俗开始，低速结束（与ease区别大概是中间速度没有ease高）
  - cubic-bezier(n,n,n,n) n是0-1的值
- animation-delay	设置动画在启动前的延迟间隔。
  - 3s
  - 默认 0
- animation-iteration-count	定义动画的播放次数。 
  - 默认 1
  - n 整数表示播放n次
  - infinite 无限循环播放
- animation-direction	指定是否应该轮流反向播放动画。
  - normal 默认值
  - reverse 方向播放
  - alternate 在奇数次 正常播放，在偶数次 反向播放
  - alternate-reverse 在偶数次正常播放，在奇数次 反向播放
  - initial 设置该属性为他的默认值
  - inherit
- animation-fill-mode	规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。
  - none 默认 不会应用任何样式（其实就是在动画结束后，画面回到动画的第一帧
  - forwards 动画结束后，停留在最后一帧
  - backwards 动画结束后，停留到动画开始的第一帧
  - both forwards和backwards两个方向上扩展动画属性
- animation-play-state	指定动画是否正在运行或已暂停。
  - running 默认 运行
  - paused 暂停 当属性值为paused时，动画停留在被暂停的那一帧
- initial	设置属性为其默认值。 阅读关于 initial的介绍。
- inherit	从父元素继承属性。 阅读关于 initinherital的介绍。