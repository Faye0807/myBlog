---
title: cookie与HTML5离线存储等
date: 2018-10-16 16:10:08
tags: HTML
description: cookie、HTML5离线存储、localStorage、sessionStorage、
keywords: HTML
categories: HTML
---
# cookies

- 读取cookie `document.cookie`
- 设置 cookie
  - name 设置的时候需要 `encodeURIComponent`；同样的读取的时候 `decodeURIComponent`
  - value 设置的时候需要 `encodeURIComponent`；同样的读取的时候 `decodeURIComponent`
  - domain 可访问该 cookie 字段的域名
  - path 可访问该 cookie 字段的 路径（比如 `/path` 则path路径下及其子路径都可以访问到该 cookie字段）
  - max-age 单位 s 表示多少秒后该cookie失效
    - -1 默认值 表示会话结束即失效
    - 0 即时失效即表示删除该cookie
  - expires 指定cookie的生存期 （未来一个用毫秒数表示的日期）
  - secure 表示是否安全传输（非键值对 即只要存在改键 即表示HTTPS的时候才会传输）
  - HttpOnly 如果这个属性设置为true，就不能通过js脚本来获取cookie的值，能有效的防止xss攻击 (该属性只在服务端使用，js控制不了该属性)

  # Storage
  - 只能存储字符串 (可以结合 JSON)
  - 属性
    - length 键数
  - 方法
    - setItem('key', value) 添加属性
    - getItem('key') 获取属性值
    - removeItem('key') 删除属性
    - key(index) 获取index处的键的名字
    - clear() 清除所有属性
  localStorage 和 sessionStorage 都是 Storage的实例，所以 localStorage 和 sessionStorage 继承其方法


  # localStorage
  在 HTML5 取代了 globalStorage
  - 跨越会话的存储
  - 只要没有被 removeItem 等删除，也没有被用户清除缓存，则localStorage数据将一直存储在磁盘上

  # sessionStorage
  - 数据只存储到会话关闭
  - 主要用于存储会话 小段数据的存储
  - 存储到 sessionStorage 的数据可以跨越页面刷新（同一个会话窗口） 而存在
