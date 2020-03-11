# webpack 
> [Webpack 中文指南](https://zhaoda.net/webpack-handbook/index.html)
> [深入浅出Webpack](http://webpack.wuhaolin.cn/)
# `script defer async` 区别
> [defer和async的区别](https://segmentfault.com/q/1010000000640869)


# undefined 与 null 的区别
[undefined与null的区别](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)
- `not defined` 
  - 如果我们访问一个未声明的变量 会返回 `not defined` 的错误

# 事件对象及this指向

| 添加事件方式 | IE事件对象   | 非IE事件对象 |  IE this  | 非IE this|
| :---------:  | :-----------: | :----------: | :---------: | :--------: |
| HTML属性    | window.event| window.event| window   | window   |
| DOM0       | window.event| event参数    | event.srcElement(相当于target) | currentTarget|
| DOM2       | event参数    | event参数    | window   |currentTarget |


# 一些小意外
```js
const a = {a: 12};
const b = {a: 12};
console.log(a == b); // false 可以理解
console.log(JSON.stringify(a) == JSON.stringify(b)); // true 也可以理解
console.log(a.__proto__ == b.__proto__); // true ????
// 此时修改 a的原型会影响b的原型
const a.__proto__.name = 'lee';
console.log(b.__proto__.name); // lee 
const t = {};
console.log(t.__proto__.name); // lee
```

# ES5 与 ES6 区别

- 箭头函数
- 模板字符串
- 作用域
- 声明变量的方式
  - 作用域的区别
  - 所声明的变量 不属于window的属性
- 函数传参可以指定默认值
- 引入 class、
- rest参数 用于获取函数的多余参数
- spread 扩展运算符（...） 相当于 rest参数 的逆操作
- Promise
- import export
- 

# 原型链 & 继承
[从一道题解读JS原型链](https://segmentfault.com/a/1190000016736112#articleHeader2)

[Javascript面向对象编程（二）：构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)

[Javascript面向对象编程（三）：非构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html)

# 函数节流（throttle） & 去抖（debounce）
[深入浅出throttle和debounce](https://github.com/stephenLYZ/stephenLYZ.github.io/issues/17)
[JS函数防抖和函数节流](https://juejin.im/post/5a35ed25f265da431d3cc1b1)