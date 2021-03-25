---
description: 来自 @abserari
---

# Dapr 源码解析分享 v1

**Dapr**

![&#x5B98;&#x7F51;&#x56FE;](../.gitbook/assets/image%20%281%29%20%281%29.png)

**Ability Introduction**

![&#x5B98;&#x7F51;&#x56FE;](../.gitbook/assets/image%20%282%29.png)

**Runtime**

![](../.gitbook/assets/image%20%283%29.png)

通过配置生成 Runtime 初始化各组件, Actor 服务, gRPC 和 HTTP 等服务. 通过 AppChannel 与 UserServer 通信.

**Runtime Components Init**

![](https://gblobscdn.gitbook.com/assets%2F-MLH28DO1fdfvhmPKY0h%2F-MLT0RuAufuEyJImC46b%2F-MLT2gu-Nb8CSUAoYhy0%2Fdapr-runtime-init%20%281%29.jpg?alt=media&token=f5335440-4639-490d-8fa4-5bd96097b5ec)

如果 UserCode 有监听端口传入 Dapr flag, 则为 runtime 新建 AppChannel 负责和 UserServer 通信.

组件可插拔, 同一个外部应用可以扮演不同组件角色.

**Components**

**Registry**

![](../.gitbook/assets/dapr-components-3-.jpg)

组件的实现都在另一个库 dapr/components-contrib 里.

Registry 接口含 Register 和 Create 两个函数签名. 组件实现之后注册到相应组件的 Registry 中.

Create 时, 参数带 Component Name 调用指定的 FactoryMethod 方法生成组件实例.

![&#x4E24;&#x79CD;&#x90E8;&#x7F72;&#x6A21;&#x5F0F;&#x4E0B;&#x7684; Components ](../.gitbook/assets/dapr-container-update-1-.jpg)

**BindingComponents**

**Binding API**

![](../.gitbook/assets/dapr-api-name-1-.jpg)

Dapr 的 API 命名机制使用 components 的 YAML 配置文件中 components 的 name 来作为 API 的命名.

**PubSub**

**Pubsub API**

![](../.gitbook/assets/dapr-pub_sub-3-.jpg)

用户使用 Dapr 的 Pubsub 功能时, 需提供 Handler 以供 Dapr 访问得到用户的订阅信息.

**StateStore**

**Interface**

![](../.gitbook/assets/dapr-statestore-2-.jpg)

StateStore 用于 Key-Value State 存储. Options 用于标识 Concurrency 和 Consistency

**AppChannel**

**Interaction with user**

![&#x5B98;&#x7F51;&#x56FE; - Kubernetes &#x4E0B;&#x7684;&#x4EA4;&#x4E92;](https://docs.dapr.io/images/overview-sidecar-kubernetes.png)

![](../.gitbook/assets/dapr-channel-2-.jpg)

AppChannel 是 Dapr 和 用户代码交互的桥梁.

**DirectMessage**

![](../.gitbook/assets/dapr-directmessage.jpg)

**Snippets**

**Watch**

![image](../.gitbook/assets/0.jpeg)

通过 fsnotify 包, 监控文件夹下的变动并通知 事件 Channel

**Signal**

![image](../.gitbook/assets/1.jpeg)

捕获 signal 的同时创建 Context. 使关闭时, 优雅通知使用该 Context 的函数关闭.

**Actor \[WIP\]**

**Internal**

![](../.gitbook/assets/dapr-actors-2-.jpg)

Placement 是单独的 HTTP server, 用于存储 Actors 在不同 Host 上的信息. Dapr init 会启动 Placement 的 docker 容器.

Actors 中保存有方法的 API 路径的 string 到不同 Host 的映射, 如果该 actor 应该在本地运行, 则直接调用 AppChannel 和 UserServer 通信, 如果查询该 actor 在远端, 则通过 gRPC 调用远端服务.

