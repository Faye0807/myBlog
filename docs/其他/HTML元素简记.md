---
title: HTML元素简记
date: 2018-10-26 11:25:40
tags: HTML
description: 记录各类属性等细节，以备随时查询
keywords: HTML
categories: HTML
---

# input
#### 1、`type = number`时候的坑

- `number` 可以输入小数点
- 此时 `maxlength` 属性失效(`maxlength` 属性与 `<input type="text"> 或 <input type="password">` 配合使用)

### 2、利用input实现 6个输入框
[实现demo](https://gitee.com/littleFaye/aboutFE)
### 3、input 光标
```css
input{
  opacity: 0;
}
```
以上代码，ios、安卓都会看不到 input，但是ios的光标还是隐藏不调的；且长按输入字体，安卓也会有光标

解决：`transform: scale(0)`; 光标消失