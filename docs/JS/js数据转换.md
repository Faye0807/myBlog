---
title: js数据转换
date: 2018-08-21 22:47:49
tags: [JavaScript]
description: 记录设计js数据类型转换相关
categories: JavaScript
keywords: js中的数据类型转换
---
## 加减乘除运算
> - 运算符会将其两边非数字类型的值经过 `Number` 转换为数值类型
> - `字符串 + 数值` 时，返回拼接字符串
> - `字符串 - 或 * 或 / 或 % 数值` 时，返回数值类型，字符串 `Number` 转换失败返回 `NaN`
> - 一切与`NaN`有关的运算操作，均返回 `NaN`

## Number()
> `Number` 转换相比 `parseInt`、`parseFloat` 要相对严格的多，只要存在转换失败的因素即返回 `NaN`

```js
Number(123) // 123
Number('123') // 123
Number('123sss') // NaN
Number(undefined) // NaN
Number(null) // 0
Number(false) // 0
Number(true) // 1
Number('') // 0
Number([1, 2, 3]) // NaN
Number([5]) // 5
Number({}) // NaN
// Number转换对象时，先调用对象的valueOf方法，如果该方法返回对象，
// 则再调用对象的toString方法，如若toString返回对象则Number报错；
parseFloat('12.00a') //12
parseInt('12.001a') //12
parseFloat('12.001a') //12.001
parseFloat('a12.001a') //NaN
parseFloat('a12.001') //NaN
parseInt('12.001') //12
parseInt(110,2) //6 以2进制 取整
```

## String()
```js
String(123) // "123"
String('abc') // "abc"
String(true) // "true"
String(undefined) // "undefined"
String(null) // "null"
String([1,2,3]) // "1,2,3"
String({a: 1}) // "[object Object]"
// String转换对象时，同Number，只不过调用valueOf、toString的顺序相反，先toString在valueOf
```

## Boolean()
> 除了以下五个值的转换结果为 `false`，其他的值全部为 `true`
> - `undefined`
> - `null`
> - `-0`或`+0`
> - `NaN`
> - `''`（空)
> 注意点是 `Boolean([])` => `true`，但是`[] == true` => `false`; `{} == true` 报错

## 自动转换
> 自动转换的规则是这样的：预期什么类型的值，就调用该类型的转换函数。比如，某个位置预期为字符串，就调用`String`函数进行转换。如果该位置即可以是字符串，也可能是数值，那么默认转为数值。

### 自动转换为布尔值
> 比如 `if` 的判断语句，三元运算符；他们内部均调用 `Boolean` 隐式转换数据类型

### 自动转换为字符串
> 主要是字符串的拼接， `'string' + {}`等类似表达式加号后面将转换为字符串
> 规则： 复杂类型 =》 原始类型 =》 `string`

### 自动转换为数值
> 系统在需要出现数值的地方，通过`Number`隐式转换数据类型
> 
> 除了加法运算符（`+`）有可能把运算子转为字符串，其他运算符都会把运算子自动转成数值。
> 一元运算符也存在隐式转换： `-true // -1`

### `==`的隐形转换
> - 如果比较的两者中有布尔值 `Boolean`，会把 `Boolean` 先转换为对应的 `Number` ，即 `0` 和 `1`，然后进行比较。
> - 如果比较的双方中有一方为 `Boolean` ，一方为 `String` 时，则会先将双方转换为数字，然后进行比较。
> - 如果比较的双方中有一方为 `Number` ，一方为 `String` 时，会把 `String` 通过 `Number()` 方法转换为数字，然后进行比较。
> - 如果比较的双方中有一方为 `Number` ，一方为 `Object` 时，则会调用 `Number` 方法将 `Object` 转换为数字，然后进行比较。
> 
> 其实总结来看，尤其是复杂类型的数据，隐式转换可能不止一次，比如下例中的 变量`a`，`a == 1` 比较时，`a`的`valueOf`返回`'1'`
>
> 此时就相当于 `'1' == 1` 的比较，`'1'`要继续转换成数字1与其比较
>
> 给我的感觉就是在不存在`string`的情况下，一切都在向`number`转化

```js
var a = {a: 222}
a == 1 //false
a.valueOf() //{a: 222}
a.toString() //"[object Object]"
a == "[object Object]" //true
a.valueOf = function(){return '1'}
a == "[object Object]" //false
a == 1 //true
a.toString = function() {return '2'}
a == 1 //true
1 == a  //true
2 == a  //false
a == '1' //true
a == '2' //false
//=================
var b = [1]
b==1 //true
b=='1' //true
b.valueOf() //[1]
b.toString() //"1"
b.valueOf = function(){return '2'}
b=='1' //false
b=='2' //true
// =========================以上总结Object + string 或 Object + number 是先valueOf在toString
```
## 四舍五入 & 取整

- 保留小数点后 n 位 （四舍五入）返回字符串
  - num.toFixed(n)
- 直接取整，不考虑小数点后面的值
  - parseInt(num or string, n)
  - n 表示以n进制取整
- 向上取整 返回数字
  - Math.ceil(num)
- 向下取整 返回数字
  - Math.floor(num)
- 四舍五入取整 返回数字
  - Math.round(num)

> 本片文章参考自[https://wangdoc.com/javascript/features/conversion.html](https://wangdoc.com/javascript/features/conversion.html)

> [js取float型小数点后两位数的方法](https://blog.csdn.net/superdog007/article/details/50800979)