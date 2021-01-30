# protoBuff

> [proto3](https://developers.google.com/protocol-buffers/docs/proto3) 

> [[转]Protobuf3 语法指南](https://colobu.com/2017/03/16/Protobuf3-language-guide/)

-   [Protocol Buffers 入门详解](https://segmentfault.com/a/1190000020286021?utm_source=tag-newest)

## 大纲

1. 定义消息

```js
syntax = "proto3";
message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}
```

2. option
    > 定义.proto 文件时能够标注一系列的 option。一些选项是文件级别的，意味着它可以作用于最外范围，不包含在任何消息内部、enum 或服务定义中。一些选项是消息级别的，意味着它可以用在消息定义的内部。当然有些选项可以作用在域、enum 类型、enum 值、服务类型及服务方法中。到目前为止，并没有一种有效的选项能作用于所有的类型。

-   Oneoof

## 安装 protoc

-[Mac 端安装 protobuf 及其简单使用](https://blog.csdn.net/love666666shen/article/details/89228450)

protoc 命令的安装地址 `/usr/local/bin/protoc`

-   [jdk 安装](https://blog.csdn.net/mr_muli/article/details/103107532)

## protoc 命令

[protoc 命令](https://www.cnblogs.com/ghj1976/p/5435565.html)

1. 简单的编译

`protoc --js_out=./java/ ./proto/helloworld.proto`

`--js_out`表示编译输出的 js 目录【当然，如果编译出 java 语言的话 就是 --java_out】

> 注意：指定的路径必须是已经存在的文件夹

`./proto/helloworld.proto` 指定 proto 文件位置

2. 参数 --proto_path 简写 -I
   `protoc --go_out=./go/ -I proto ./proto/helloworld.proto`
   这个参数 还没搞懂，而且对 import 影响还不知道，但是会去 `proto/proto/helloworld.proto`找 proto 文件进行编译

```
// 文件目录
├── proto
│   ├── employee.proto
│   └── employeeCopy.proto
```

```js
// ===== employee =====
syntax = "proto3";
import 'employeeCopy.proto';
// 命名空间
package employee;

// ===== employeeCopy =====
syntax = "proto3";
// 命名空间
package employeeCopy;
```

执行下面命令行 👇 指定 `proto_path`

`protoc --proto_path=proto --js_out=output_I employee.proto employeeCopy.proto`

```js
// ===== employee =====
syntax = "proto3";
// 增加 proto 文件路径
import 'proto/employeeCopy.proto';
// 命名空间
package employee;

// ===== employeeCopy =====
syntax = "proto3";
// 命名空间
package employeeCopy;
```

执行下面命令行 👇 去掉 `proto_path`

`protoc --js_out=output_I employee.proto employeeCopy.proto`

都会得到下面 👇 的文件

```
├── output_I
│   ├── employeerequest.js
│   ├── employeeresponse.js
│   ├── proto.employeecopy_employeeresponse.js
│   ├── proto.employeecopy_reportemployeerequest.js
│   ├── proto.employeecopy_reportemployeeresponse.js
│   ├── reportemployeerequest.js
│   └── reportemployeeresponse.js
```

> 同上注意：指定的路径必须是已经存在的文件夹

3. grpc_out 命令

指定 `xxx_grpc_pb.js` 文件的输出目录

`_grpc_pb.js` 文件是跟服务相关，创建，调用，绑定，实现相关

4. import_style=commonjs

binary: 输出文件目录，commonjs 时候 libary 不起作用
`protoc --js_out=import_style=commonjs,binary:output_I --grpc_out=deome/path employee.proto employeeCopy.proto`

-   commonjs 跟 Closure Imports 两个方式生成的 grpc_pd 文件是一样的【同时内部都引入`_pg.js`
-   commnjs 会生成 `_pb.js` 文件
-   Closure 默认是 以 message 为单位 生成 message 小写名称命名的文件,可以通过 `libary` 指定生成文件名称 合并成同一个文件,`libary` 的值也可以是 `demo/somefilename` 其中 demo 文件路径可以当前 不存在，protoc 编译时自动生成

5. plugin 命令

## 对 proto 文件有两种使用方式

1. 使用 `@grpc/proto-loader` 动态加载

客户端跟服务端都是使用该方法去加载 proto 文件，两端的区别：

服务端

-   需要实例化一个服务
-   绑定 ip 跟端口号
-   创建存根

客户端

-   链接服务
-   创建存根
-   调用方法

2. protoc 自行编译后 导入文件使用

编译生成的文件作用大致：提供对方法入参、出参的校验与取、赋值
与动态加载方法的区别：

-   执行效率的话 demo 看不出太多差异
-   使用方法有些出入 如下 👇[同官方 node demo](https://github.com/grpc/grpc/tree/master/examples) 【对比来看，其实动态加载的方法更利于开发，不需要一个一个的拿字段、给字段赋值】

例如下 proto

```js
package helloworld;

// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}
```

```js
// 静态引入法： client端
// 通过 setName 给name赋值
// getMessage 获取 message 值
var messages = require("./helloworld_pb")
var services = require("./helloworld_grpc_pb")

var grpc = require("grpc")

function main() {
    var client = new services.GreeterClient("localhost:50051", grpc.credentials.createInsecure())
    var request = new messages.HelloRequest()
    var user
    if (process.argv.length >= 3) {
        user = process.argv[2]
    } else {
        user = "world"
    }
    request.setName(user)
    client.sayHello(request, function (err, response) {
        console.log("Greeting:", response.getMessage())
    })
}

main()

// ======= 以下是动态加载法 ==========
// 直接给 name 赋值，用 message 获取
var PROTO_PATH = __dirname + "/../../protos/helloworld.proto"

var grpc = require("grpc")
var protoLoader = require("@grpc/proto-loader")
var packageDefinition = protoLoader.loadSync(PROTO_PATH, {
    keepCase: true,
    longs: String,
    enums: String,
    defaults: true,
    oneofs: true,
})
var hello_proto = grpc.loadPackageDefinition(packageDefinition).helloworld

function main() {
    var client = new hello_proto.Greeter("localhost:50051", grpc.credentials.createInsecure())
    var user
    if (process.argv.length >= 3) {
        user = process.argv[2]
    } else {
        user = "world"
    }
    client.sayHello({ name: user }, function (err, response) {
        console.log("Greeting:", response.message)
    })
}

main()

// ======= 同理 服务端也是
// 动态加载 - 服务端
var PROTO_PATH = __dirname + "/../../protos/helloworld.proto"

var grpc = require("grpc")
var protoLoader = require("@grpc/proto-loader")
var packageDefinition = protoLoader.loadSync(PROTO_PATH, {
    keepCase: true,
    longs: String,
    enums: String,
    defaults: true,
    oneofs: true,
})
var hello_proto = grpc.loadPackageDefinition(packageDefinition).helloworld

/**
 * Implements the SayHello RPC method.
 */
function sayHello(call, callback) {
    callback(null, { message: "Hello " + call.request.name })
}

/**
 * Starts an RPC server that receives requests for the Greeter service at the
 * sample server port
 */
function main() {
    var server = new grpc.Server()
    server.addService(hello_proto.Greeter.service, { sayHello: sayHello })
    server.bind("0.0.0.0:50051", grpc.ServerCredentials.createInsecure())
    server.start()
}

main()

// 静态引入 - 服务端
var messages = require("./helloworld_pb")
var services = require("./helloworld_grpc_pb")

var grpc = require("grpc")

/**
 * Implements the SayHello RPC method.
 */
function sayHello(call, callback) {
    var reply = new messages.HelloReply()
    reply.setMessage("Hello " + call.request.getName())
    callback(null, reply)
}

/**
 * Starts an RPC server that receives requests for the Greeter service at the
 * sample server port
 */
function main() {
    var server = new grpc.Server()
    server.addService(services.GreeterService, { sayHello: sayHello })
    server.bind("0.0.0.0:50051", grpc.ServerCredentials.createInsecure())
    server.start()
}

main()
```
