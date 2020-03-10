---
title: 你不知道的JSON.stringify
date: 2018-10-17 11:37:21
tags: JSON
description: 转：你不知道的JSON.stringify
keywords: JSON
categories: JSON
---

> 本文转自 [你不知道的JSON.stringify](https://blog.fundebug.com/2017/08/17/what-you-didnt-know%20about-json-stringify/)

JSON已经逐渐替代XML被全世界的开发者广泛使用。本文深入讲解JavaScript中使用JSON.stringify的一些细节问题。首先简单回顾一下JSON和JavaScript：

不是所有的合法的JSON都是有效的JavaScript；
JSON只是一个文本格式；
JSON中的数字是十进制。
# JSON.stringify
```js
let foo = { a: 2, b: function() {} };
JSON.stringify(foo);
// "{ "a": 2 }"
```
JSON.stringify函数将一个JavaScript对象转换成文本化的JSON。不能被文本化的属性会被忽略。foo中属性b的值是函数定义，没有被转换而丢失。

# 还有哪些属性也不能转换？
## 循环引用
如果一个对象的属性值通过某种间接的方式指回该对象本身，那么就是一个循环引用。比如：
```js
var bar = {
  a: {
     c: foo
  }
};
var foo = {
  b: bar
};
```
属性c指向自己，如果层层解析，将会进入一个无限循环。我们尝试将其打印出来看看：
```js
let fooStringified = JSON.stringify(foo);
console.log(fooStringified); // {"b":{"a":{}}}
```
c的属性指向foo对象，foo对象中的b属性又指向bar对象而无法处理，整个被忽略而返回空对象。

如下定义(原文中的例子)是无法通过编译的：
```js
let foo = {b : foo};
```
错误信息：
```js
ReferenceError: foo is not defined
    at repl:1:14
```
在函数式语言Haskell中，因为有Lazy Evaluation技术，可以使用类似的定义方法。

备注：原文中的例子不正确，感谢SegmentFault用户xueshen1106指出，现在将他的评论贴在下方，供大家学习。
![error](https://blog.fundebug.com/2017/08/17/what-you-didnt-know%20about-json-stringify/segmentfault-user-comment-1.png)

## Symbol和undefined
```js
let foo = { b: undefined };
JSON.stringify(foo);
// {}
// Symbols
foo.b = Symbol();
JSON.stringify(foo);
// {}
```
例外情况
在数组中，不可被stringify的元素用null填充。
```js
let foo = [Symbol(), undefined, function() {}, 'works']
JSON.stringify(foo);
// "[null,null,null,'works']"
```
这样可以保持数组本身的“形状”，也就是每一个元素原本的索引。

为什么有些属性无法被stringify呢？
因为JSON是一个通用的文本格式，和语言无关。设想如果将函数定义也stringify的话，如何判断是哪种语言，并且通过合适的方式将其呈现出来将会变得特别复杂。特别是和语言相关的一些特性，比如JavaScript中的Symbol。

ECMASCript官方也特意强调了这一点：

> It does not attempt to impose ECMAScript’s internal data representations on other programming languages. Instead, it shares a small subset of ECMAScript’s textual representations with all other programming languages.

# 重写对象toJSON函数
一个绕过对象某些属性无法 `stringify` 的方法就是实现对象的 `toJSON` 方法来自定义被 `stringify` 的对象。因为几乎每一个`AJAX`调用都会使用 `JSON.stringify`，掌握该技巧将会对处理服务器交互有很大帮助。

和`toString`允许你将对象中的元素以字符串(string)的形式返回类似，`toJSON`提供了一种可以将对象中不能stringify的属性转换的方法，使得接下来调用的`JSON.stringify`可以将其转换成JSON格式。
```js
function Person (first, last) {
    this.firstName = first;
    this.last = last;
}

Person.prototype.process = function () {
   return this.firstName + ' ' +
          this.lastName;
};

let ade = new Person('Ade', 'P');
JSON.stringify(ade);
// "{"firstName":"Ade","last":"P"}"
```
Person实例ade的process函数没有被stringify。假想如果服务器只想要ade的全称，而不是分别获取姓和名，我们可以直接定义toJSON来达到目的：
```js
Person.prototype.toJSON = function () {
    return { fullName: this.process(); };
};

let ade = new Person('Ade', 'P');
JSON.stringify(ade);
// "{"fullName":"Ade P"}"
```
定义toJSON的优点是复用性和稳定性，你可以将ade配合任何库使用，传输的数据都将是你通过toJSON定义而返回的fullName。
```js
// jQuery
$.post('endpoint', ade);

// Angular 2
this.httpService.post('endpoint', ade)
```
## 可选参数
JSON.stringify完整的定义如下：
```js
JSON.stringify(value, replacer?, space?)
```
replacer和space都是可选参数，接下来我们来分别讲解。

### Replacer
replacer是一个过滤函数或则一个数组包含要被stringify的属性名。如果没有定义，默认所有属性都被stringify。

- 数组
只有在数组中的属性被stringify：
```js
let foo = {
  a : 1,
  b : "string",
  c : false
};
JSON.stringify(foo, ['a', 'b']);
//"{"a":1,"b":"string"}"
```
嵌套属性也同样会被过滤：
```js
let bar = {
  a : 1,
  b : { c : 2 }
};
JSON.stringify(bar, ['a', 'b']);
//"{"a":1,"b":{}}"

JSON.stringify(bar, ['a', 'b', 'c']);
//"{"a":1,"b":{"c":2}}"
```
定义过滤数组有时候并不能满足需求，那么可以自定义过滤函数。

-  函数
过滤函数以对象中的每一个属性和值作为输入，返回值有以下几种情况：

返回undefined表示忽略该属性；
返回字符串，布尔值或则数字将会被stringify；
返回对象将会触发递归调用知道遇到基本类型的属性；
返回无法stringify的值将会被忽略；
```js
let baz = {
  a : 1,
  b : { c : 2 }
};

// 返回大于1的值
let replacer = function (key, value) {
    if(typeof === 'number') {
        return value > 1 ? value: undefined;
    }
    return value;
};

JSON.stringify(baz, replacer);
// "{"b":{"c":2}}"
```
通过改写上面的函数加入适当的输出，可以看到具体的执行步骤：
```js
let obj = {
  a : 1,
  b : { c : 2 }
};

let tracer = function (key, value){
  console.log('Key: ', key);
  console.log('Value: ', value);
  return value;
};

JSON.stringify(obj, tracer);
// Key:
// Value: Object {a: 1, b: Object}
// Key: a
// Value: 1
// Key: b
// Value: Object {c: 2}
// Key: c
// Value: 2
```
### space
你是否意识到调用默认的JSON.stringify返回的值只要一行，而且完全没有空格？如果想要更加美观的打印出来，那么就需要使用space这个参数了。

我告诉你一个非常简单的方法：通过tab(‘\t’)来分割即可。
```js
let space = {
  a : 1,
  b : { c : 2 }
};

// 使用制表符
JSON.stringify(space, undefined, '\t');
// "{
//  "a": 1,
//  "b": {
//   "c": 2
//  }
// }"

JSON.stringify(space, undefined, '');
// {"a":1,"b":{"c":2}}

// 自定义分隔符
JSON.stringify(space, undefined, 'a');
// "{
//  a"a": 1,
//  a"b": {
//   aa"c": 2
//  a}
// }"
```
一道三颗星的思考题：为什么打印结果的倒数第三行有两个a呢？

结论
本文介绍了一些使用toJSON的技巧：

无法stringify的几种类型
使用toJSON来自定义JSON.stringify的属性
可选参数replacer的两种定义方法来过滤属性
可选参数space用来格式化输出结果
数组和对象中如果包含无法stringify的元素的时候的区别

