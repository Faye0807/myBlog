# 开发中遇到的问题
## 1. 服务端渲染时 classnames 生成 [Object object] 字符串

debug调试代码如下👇🏻
![debug](../static/imgs/210408.png)

这一行服务端渲染时，永远返回`false`；
因为我们框架有一套自己的沙河环境，在这个环境内部 这个判断不相等