---
title: 浏览器工作原理
date: 2018-08-28 16:38:05
tags: 浏览器
description: 简记浏览器工作原理
categories: 移动端
keywords: touch 移动端
---

## 300ms延迟
点击移动端设备时，触发事件顺序：`touchstart` =》 `touchmove` =》 `touchend` =》 `click`
在 `touchend` 事件后有 300 ms  延时（因为要确认是否进行双击）后再触发 `click` 事件

Tips：
- 当触发了 `touchmove` 事件后，将不会触发 `click` 事件
- 触发 `click` 事件前提是 `touchstart` `touchend` 事件可以正常触发；
- 如果在 `touchstart` 事件处理程序里添加 `e.preventDefault()` 则浏览器误以为 `touchstart` 事件没有正常触发，由此会不触发 `click` 事件
- `touchend` 事件 在 pc 端有延迟（约700ms），即不能再 `touchmove` 事件结束后立即触发 `touchend` 事件 在移动端没有延迟
- `safari` 的橡皮筋效果 可以在 touchmove 事件处理程序后 添加 `e.preventDefault()` 阻止
