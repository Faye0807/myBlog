## Vues数据双向绑定实现
[code](https://github.com/Faye0807/task/tree/master/fe/views/demo/vue%E5%8F%8C%E5%90%91%E7%BB%91%E5%AE%9A%E5%AE%9E%E7%8E%B0)

## inupt输入框的`change`事件在失去焦点后触发  
此时跟blur事件很像，查阅后的方案是将处理程序添加到input事件上面 [参考链接](https://forum.vuejs.org/t/v-on-change/8670)

## vue input file 多次上传不触发change事件问题 
有以下几种方法（[参考链接](https://www.cnblogs.com/exhuasted/p/6185874.html)）：
- 清空input的value值
- 替换掉原来的input 渲染一个新的input
- 将input放到一个表单里，change事件触发时将表单reset
- 替换原input，渲染新input并重新绑定change事件（这个方法对我有效）

## Vue 父组件异步获取数据，子组件props获取不到
由于我子组件需要拿到props后做数据处理，但发现无论是放到data，还是mounted，props里面值还是undefined；

查阅后了解到，异步请求数据有关；解决办法就是：引用子组件时，v-if先判断先props是否有值，如下
``` js
<component-a v-if="dataList.length>0" :prop-a="dataList" />
```

## 事件修饰符
事件修饰符不仅在绑定处理程序时候可以用，不绑定事件处理程序同样可用；比如阻止事件的传播
```js
<component-a @touch.stop />
```

## 深度作用选择器 `/deep/` or `>>>`
当你的style标签添加`scoped` 使css有了作用域后，若想让自己的css可以更深的控制元素样式，则可以使用`/deep/` or `>>>`; 比如你想在父组件更改子组件样式的时候，可能会需要这个选择器

[参考链接](https://vue-loader-v14.vuejs.org/zh-cn/features/scoped-css.html)

<!-- ## 组件标签上添加class、style 与 元素上添加class、style的区别

```js
<component-a class="class-a" style="positon: absoult;color: #fff;" />
<div class="class-a" style="positon: absoult;color: #fff;" />
``` -->

## img 动态src属性值

```js
// 是加载不到图片的
<img :src="flag ? './image/img.png' : './image.img2.png'" />
// 需要这样
<img :src="flag ? require('./image/img.png') : require('./image.img2.png')" />
// 或者 将图片放到 static文件夹
```
[vue 动态加载图片src的解决办法](https://blog.csdn.net/keji_123/article/details/79977210)

## v-for

> 注意 :key 的值若不唯一会报错

```js
[Vue warn]: Duplicate keys detected: '{key}'. This may cause an update error.  found in  ---> <Anonymous>        <Nuxt>          <Default> at layouts/default.vue            <Root>
```

## 引用文件
若引用路径文件名字母大小与文件命名不一致，或多处引用路径大小不一致，vue会有警告⚠️