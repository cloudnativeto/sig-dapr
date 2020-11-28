# 问题 FAQ

### SDK 使用 Trace 追踪调用需手动添加 ID

1. SDK\(Go, Java\) 中 zipkin 跟踪信息没有传递成为调用链, traceID 丢失
   1. a -&gt; b -&gt; c 会展示为 a-&gt;b , b-&gt;c

预计解决方案: 通过手动添加 a 的 traceID 到 b 节点, 使得 a -&gt; b -&gt; c 能通过 ID 联系起来.

Todo: 非自动生成调用链, Dapr 现在需手动添加.

官方指导链接： 

