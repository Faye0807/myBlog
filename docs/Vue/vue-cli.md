---
title: vue-cli项目部署到gitee Pages
date: 2018-08-14 23:50:33
description: 通过 vue-cli 搭建的spa项目,可以部署到giteePages
keywords: vue vue-cli pages spa
tags: [Vue, vue-cli, spa]
categories: Vue
---

> 通过 `vue-cli` 搭建的spa项目,可以部署到giteePages

## 首先 通过 vue-cli 搭建自己的vue项目

确定 `npm run dev`可以正常启动项目
并将该项目放到 码云上

## 更改 `config/index.js` 文件


将给文件下 `build` 对象下的 `assetsPublicPath` 修改为 `./`

![](/teresa/images/config_index.js.jpg)

## 执行 `npm run build`

会生成一个`dist`文件
编译后提交推送代码（ps： 记得将dist文件从gitignore中移出）

## 部署gitee Pages

部署的时候指定文件夹为`dist`即可

部署成功后即可按照提示链接访问你的Vue页面
