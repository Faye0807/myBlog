# gRPC-node

> 👉[参考文档](http://doc.oschina.net/grpc?t=58008)

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

-   [Protocol Buffers 入门详解](https://segmentfault.com/a/1190000020286021?utm_source=tag-newest)
