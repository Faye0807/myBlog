# 汇总js操作DOM的常用方法及属性
> 获取元素节点的方法，返回包含元素节点的 `对象` 或者 `对象数组`
>
> 没有标注说明，则方法可以用于 `document` 和 元素
# 常用方法
## `getElementById`
```js
// 获取id为myId的元素
document.getElementById('myId');
```
只可以用于 `document`，而不可用于其他元素后

## `getElementsByTagName`
```js
// 获取所有的div元素
element.getElementsByTagName('div');
```
## `getElementsByClassName`
```js
// 获取所有class值为myClass的元素
element.getElementsByClassName('myClass');
```
## `querySelector`
```js
// 接受与css选择器同样规则的字符串参数 如下：返回 id 为 myid 元素下的第一个类为 myclass 的元素对象
document.querySelector('#myid .myclass')
```
## `querySelectorAll`
```js
// 接受与css选择器同样规则的字符串参数 如下：返回 id 为 myid 元素下的所有类为 myclass 的数组对象
document.querySelectorAll('#myid .myclass')
```
## `getAttribute` 只能通过元素节点调用
```js
// 获取 element 元素的title 属性值
element.getAttribute('title');
```
## `setAttribute` 只能通过元素节点调用
```js
// 设置 element 元素的title 属性值
element.setAttribute('title', 'myTitle');
```

> 读取和设置元素的属性其实也可以直接通过元素节点对象来实现

```js
const ele = document.querySelector('#myid');
console.log(ele.title);
ele.className = 'myClass';
```

## 插入节点 `appendChild`、`insertBefore`、`insertAfter`
> 如果需要添加的元素已经存在于文档中，则上述方法会将元素先从文档树中删除再添加回指定的地方，相当于做了移动功能
> 
> 如果被添加的元素包含子元素，则一并将子元素添加

```js
// 给body元素的最后添加一个p元素
p = document.createElement('p');
document.appendChild(p);
// 将p元素添加到 myId元素的前面
const myId = document.getElementById('myId');
myId.parentNode.insertBefore(p,myId);
```
## 创建元素节点 `createElement()`
```js
const p = document.createElement('p');
document.body.appendChild(p);
```
只可以用于 `document`，而不可用于其他元素后

## 创建文本节点 `createTextNode()`
```js
const text = document.createTextNode('hello world');
const p = document.createElement('p');
p.appendChild(text);
document.body.appendChild(p);
```
只可以用于 `document`，而不可用于其他元素后

## 复制节点 `node.cloneNode(isDeep)`
> 接受一个布尔值表示是否深克隆，即是否赋值当前节点的所有子节点；若false则只克隆当前的元素节点（其内的文本节点等都不会被克隆），否则会将子节点连同当前元素节点一同克隆
> 克隆出的节点不被自动添加到文档里去，因为没有nodeParent属性

```js
// 深度克隆p元素节点
p.cloneNode(true);
document.body.appendChild(p);
```

## 删除节点 `removeChild()`
```js
// 移出myId元素 如果被删除的节点包含子节点，则子节点将一并被删除
const myId = document.getElementById('myId');
myId.parentNode.removeChild(myId);
```

## 替换节点 `replaceChild()`
```js
// 用新增p元素替换 myId元素
// 如果被插入的节点，是文档树上有的节点，则会把该节点删除后再去体会旧节点
// 如果被插入节点，含有子节点，则子节点也一同被添加至指定位置
const myId = document.getElementById('myId');
const p = document.createElement('p');
myId.parentNode.replaceChild(p,myId);
```
## 是否含有子节点 `hasChildNodes`
```js
// 返回布尔值
const myId = document.getElementById('myId');
const isChild = myId.hasChildNodes;
```
# 常用元素节点对象的属性
> 除了 `nodeValue` 其它都是只读属性;
> `nodeValue`只能重设原值为非 `null` 的属性值

## `nodeType`（返回数字）
- 1：元素节点
- 2：属性节点
- 3：文本节点

## `nodeValue`
```js
// 元素节点 => 返回null
// 属性节点 => 返回属性值
// 文本节点 => 返回文本节点内容
```

## `children` 和 `childNodes`
- `childen` 返回元素的所有子元素节点（HTMLList）
-  `childNodes` 返回元素的所有子节点（NodesList）

## `firstChild` 和 `firstElementChild`
- `firstChild` 返回第一个子节点
- `firstElementChild` 返回第一个子元素节点

## `lastChild` 和 `lastElementChild`
- `lastChild` 返回最后一个子节点
- `lastElementChild` 返回最后一个子元素节点

## `nextSibling` & `nextElementSibling`
```js
// 返回当前节点的下一个兄弟节点
const newnode = node.nextSibling;
// 返回当前节点的下一个兄弟元素节点
const newElement = node.nextElementSibling;
```
## `previousSibling` & `previousElementSibling`
```js
// 返回当前节点的上一个兄弟节点
const newnode = node.previousSibling;
// 返回当前节点的上一个兄弟元素节点
const newElement = node.previousElementSibling;
```

## `parentNode`
```js
// 返回指定节点的父节点（肯定是个元素节点）
// document例外没有父节点，所以他的 父节点属性返回null
const parentNode = node.parentNode;
```