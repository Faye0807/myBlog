> `this.$router`访问路由器，`this.$route`访问当前路由
> 
> `this.$router.history.current === this.$route`
> 
> [参考链接](https://router.vuejs.org/zh/guide/essentials/navigation.html)

### this.$route

```js
{
  fullPath: "/a/b/9567216?c=1#one",
  hash: "#one",
  matched: [{…}],
  meta: {},
  name: "a-b-id",
  params: {id: "9567216"},
  path: "/a/b/9567216",
  query: {c: "1"}
}

```

### this.$router
主要应用他的方法导航链接

```
// 前进 n 个历史记录
this.$router.go(n)
// 接受字符串
this.$router.push('/login');
// 不会产生历史记录
this.$router.replace('/login');
// 或接受一个对象
this.$router.push({path: `user/${id}`, query:{name: 'lee'}})

```

注意的是：传入一个对象时，如果存在 `path` 的时候，`param`参数将被忽略，应该将 `param` 部分动态拼接到 `path` 中