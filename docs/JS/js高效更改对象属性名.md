---
title: js高效更改对象属性名
date: 2018-08-23 14:39:52
tags: JavaScript
description: 使用js高效的更改对象中的属性名
categories: JavaScript
keywords: 使用js高效的更改对象中的属性名
---
项目中需要使用到嵌套对象，且每层对象的数据结构相同，如下：
```js
[
  {
    name: '北京',
    id: '010',
    children: [
      {
        name: '东城区',
        id: '0101',
        children: [
          { name: '长安街', id: '010101' },
          { name: '长椿街', id: '010102' }
        ]
      },
      {
        name: '西城区',
        id: '0102',
        children: [
          { name: '西长安街', id: '010201' },
          { name: '西长椿街', id: '010202' }
        ]
      }
    ]
  }
]
```

但是，现在接口获取到的dataList是这样的
```js
[
  {
    name: '北京',
    id: '010',
    dataList: [
      {
        name: '东城区',
        id: '0101',
        dataList: [
          { name: '长安街', id: '010101' },
          { name: '长椿街', id: '010102' }
        ]
      },
      {
        name: '西城区',
        id: '0102',
        dataList: [
          { name: '西长安街', id: '010201' },
          { name: '西长椿街', id: '010202' }
        ]
      }
    ]
  }
]
```

我的组件需要第一种 `children` 类型的数据，那么我就需要拿到数据后做处理；网上查阅了下比较简单直接的方法，觉得很赞，特来记录下(假设需要处理的数据列表是变量 `dataList` )：

```js
const data = JSON.parse(JSON.stringify(dataList).replace(/dataList/g,'children'));
```

`data` 即所想得

> [参考链接](https://segmentfault.com/q/1010000011923504)