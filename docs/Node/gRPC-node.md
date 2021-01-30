# gRPC-node

> 👉 [参考文档](http://doc.oschina.net/grpc?t=58008)
>
> 👉 [egg-grp](https://github.com/eggjs/egg-grpc/blob/master/config/config.default.js)

## RPC、gRPC、gRPC-node 都是个啥？

### 1、RPC

Remote Procedure Call: 远程过程调用

> 使得我们可以像调用本地方法一样地调用远程机器上的方法
> 一般基于 TCP 协议 也可基于 http 协议

### 2、gRPC

> gRPC 是一个高性能、开源和通用的 RPC 框架，面向移动和 HTTP/2 设计

-   正如其他 RPC 系统，gRPC 基于如下思想：定义一个服务， 指定其可以被远程调用的方法及其参数和返回类型。gRPC 默认使用 [protocol buffers](https://developers.google.com/protocol-buffers/)，这是 Google 开源的一套成熟的结构数据序列化机制（当然也可以使用其他数据格式如 JSON）

## gRPC 概念

### 1、服务定义

```
service HelloService {
  rpc SayHello (HelloRequest) returns (HelloResponse);
}

message HelloRequest {
  required string greeting = 1;
}

message HelloResponse {
  required string reply = 1;
}

```

#### gRPC 允许你定义四类服务方法：

1. 单项 RPC
   即客户端发送一个请求给服务端，从服务端获取一个应答，就像一次普通的函数调用。

    ```
    rpc SayHello(HelloRequest) returns (HelloResponse){
    }
    ```

2. 服务端流式 RPC
   即客户端发送一个请求给服务端，可获取一个数据流用来读取一系列消息。客户端从返回的数据流里一直读取直到没有更多消息为止。
    ```
    rpc LotsOfReplies(HelloRequest) returns (stream HelloResponse){
    }
    ```

3) 客户端流式 RPC
   即客户端用提供的一个数据流写入并发送一系列消息给服务端。一旦客户端完成消息写入，就等待服务端读取这些消息并返回应答。
    ```
    rpc LotsOfGreetings(stream HelloRequest) returns (HelloResponse) {
    }
    ```

4. 双向流式 RPC
   即两边都可以分别通过一个读写数据流来发送一系列消息。这两个数据流操作是相互独立的，所以客户端和服务端能按其希望的任意顺序读写，例如：服务端可以在写应答前等待所有的客户端消息，或者它可以先读一个消息再写一个消息，或者是读写相结合的其他方式。每个数据流里消息的顺序会被保持。
    ```
    rpc BidiHello(stream HelloRequest) returns (stream HelloResponse){
    }
    ```

## 附

-   vscode 插件 `vscode-proto3`

## 疑问

### 1.定义服务

从 proto 文件加载服务描述符
Node.js 的类库在运行时加载 .proto 中的客户端存根并动态生成服务描述符。

要加载一个 .proto 文件，只需要 require gRPC 类库，然后使用它的 load() 方法：

```js
var grpc = require("grpc")
var protoDescriptor = grpc.load(__dirname + "/route_guide.proto")
// The protoDescriptor object has the full package hierarchy
var example = protoDescriptor.examples
```

一旦你完成这个，存根构造函数是在 examples 命名空间（protoDescriptor.examples.RouteGuide）中而服务描述符（用来创建服务器）是存根（protoDescriptor.examples.RouteGuide.service）的一个属性。

> 描述符是干啥的
> 为什么是在 examples 命名空间，因为在他文件夹下？？=> 关联 proto 文件的 package 的值
> 存根有是干啥的
> 描述符是干啥的

## 包的作用

-   `@grpc/grpc-js` 是一个纯 js grpc 客户端，几乎可以替代 grpc 。 [对比](https://github.com/grpc/grpc-node/tree/master/packages/grpc-js)
-   [`@grpc/proto-loader`](https://github.com/grpc/grpc-node/tree/master/packages/proto-loader) 用以加载 proto 文件的工具包
-   [`@grpc/grpc-tools`](https://github.com/grpc/grpc-node/tree/master/packages/grpc-tools) node 编译 proto 工具;需要全局安装

安装报错

```s
> grpc-tools@1.9.1 install /usr/local/lib/node_modules/grpc-tools
> node-pre-gyp install

node-pre-gyp WARN Using needle for node-pre-gyp https download
node-pre-gyp ERR! Completion callback never invoked!
node-pre-gyp ERR! System Darwin 19.4.0
node-pre-gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/grpc-tools/node_modules/.bin/node-pre-gyp" "install"
node-pre-gyp ERR! cwd /usr/local/lib/node_modules/grpc-tools
node-pre-gyp ERR! node -v v10.15.1
node-pre-gyp ERR! node-pre-gyp -v v0.15.0
node-pre-gyp ERR! This is a bug in `node-pre-gyp`.
node-pre-gyp ERR! Try to update node-pre-gyp and file an issue if it does not help:
node-pre-gyp ERR! <https://github.com/mapbox/node-pre-gyp/issues>
npm ERR! code ELIFECYCLE
npm ERR! errno 6
npm ERR! grpc-tools@1.9.1 install: `node-pre-gyp install`
npm ERR! Exit status 6
npm ERR!
npm ERR! Failed at the grpc-tools@1.9.1 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR! /Users/qitmac/.npm/\_logs/2020-08-05T10_08_38_099Z-debug.log
```

[解决方案 👇](https://github.com/mapbox/node-pre-gyp/issues/433)

`sudo npm install --unsafe-perm -g grpc-tools`

-   [grpc_tools_node_protoc_ts](https://www.npmjs.com/package/grpc_tools_node_protoc_ts) 根据 grpc_tools_node_protoc 生成的 js 代码生成相应的 TypeScript d.ts 代码
