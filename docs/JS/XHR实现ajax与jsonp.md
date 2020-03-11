> 查看本章代码 [demo](https://github.com/Faye0807/task.git)
## XMLHttpRequest对象
现在浏览器均内建 `XMLHttpRequest` 对象（IE5 和 IE6 使用 ActiveXObject）
```js
// 获取 xhr 实例
const xhr = new window.XMLHttpRequest();
// 或者
const xhr1 = new XMLHttpRequest();
```

## XMLHttpRequest 的用法

- open("method(不区分大小写)", 'url', 是否异步的布尔值);
- send(发送请求时的参数数据若没有须填写null);(get方法时请求参数需要拼接到url后面，放到send方法里面无效)
- abort() 在取得响应前取消异步请求
调用 `send` 方法后，请求就会别分派到服务器，在收到响应后，响应的数据会自动填充 XHR 对象的属性，如下：

- responseText 作为响应主体返回的文本
- responseXML 如果响应的内容类型是 `text/xml` 或 `application/xml`，这个属性中将保存包含着相应数据的 XML DOM文档
- status 响应的 HTTP状态
- statusText HTTP状态的说明

如果异步请求，需要监测请求状态（xhr对象的 `readyState` 属性），根据状态的变化（xhr的 `readystatechange` 事件）来做响应处理, `readystatechange`事件必须放在调用 `open` 方法前
- readyState 属性值
  - 0 未初始化
  - 1 启动 已经调用open方法
  - 2 发送 已经调用send方法
  - 3 接收 已经接受到部分响应数据
  - 4 完成

  自动定义头部信息方法： setRequestHeader()
  - 该方法必须在open之后send之前调用
  - 该方法接受两个参数： 头部字段名称，该字段名称的值
  
```js
xhr.setRequestHeader('Content-Type', 'application/json; charset=utf-8');
```

简单例子如下：

```js
const xhr = new window.XMLHttpRequest();
const data = {
  id: 10
};
xhr.onreadystatechange = function() {
  if (xhr.readyState === 4) {
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304){
      console.log('request success');
    } else {
      console.log('request was unseccessful');
    }
  }
};
xhr.open('GET', someurl, true);
xhr.setRequestHeader('Content-Type', 'application/json; charset=utf-8');
xhr.send(data);
```

### get请求
get是常用的请求方式，必要时可在URL后面追加查询字符串；查询字符串中的每个参数的名称和值都必须经过 `encodeURIComponent()` 进行编码才能放到URL的末尾，且不同的键值需要用 `&` 进行分割

> 注意：get请求的参数是需要拼接到url后面的，放到send方法里面此时是无效的
### XMLHttpRequest 2级
#### 超时设定
- xhr.timeout = 1000
- xhr.ontimeout = function() {}

### JSONP(JSON with padding 填充式json)
JSONP 由两部分组成（回调函数 + 数据）
缺点
- 只能使用Get请求
- 不能注册success、error等事件监听函数，不能很容易的确定JSONP请求是否失败
- JSONP是从其他域中加载代码执行，容易受到跨站请求伪造的攻击，其安全性无法确保