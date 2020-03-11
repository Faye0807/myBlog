---
title: IOS的坑
date: 2018-10-26 18:36:39
tags: BUG
description: 记录iOS开发问题及解决方法
keywords: BUG
categories: BUG
---
# 1、overflow: hidden;失效 ，背景仍可滑动问题
- 改变控制背景 滑动方法；不使用 `overflow: hidden`; 改用 `position:fixed`;
- 缺点：用于页面顶部还可以，当你有弹框时， `body` 会自动滑到顶部（此处还没找到破解方法，各位大神有方法，望赐教）

# 2、input
- 上面总多出边框 `outline:none` 也不管用(类似还有 `button` 的倒圆角问题)
  - `-webkit-appearance:none`;
  - [关于-webkit-appearance 属性](https://www.cnblogs.com/shawn-en/p/4352929.html)
- [光标问题](https://littlefaye.gitee.io//teresa/2018/10/26/HTML元素简记/)
- [autofocus](https://segmentfault.com/q/1010000014364210/a-1020000014450087) 如果没有通过某种用户交互，iOS不会（触发`focus`事件）

# 3、scroll滑动卡顿不流畅
- `-webkit-overflow-scrolling : touch`;

# 4、safari 橡皮筋效果
- `touch`事件添加 `e.preventDefault()`