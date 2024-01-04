# 核心概念

1. 生产者
2. 消费者
3. 交换机
4. 队列
5. 路由



##六种工作模式

简单模式：一个生产者，一个消费者

work模式：一个生产者，多个消费者，每个消费者获取到的消息唯一。

订阅模式：一个生产者发送的消息会被多个消费者获取。

路由模式：发送消息到交换机并且要指定路由key ，消费者将队列绑定到交换机时需要指定路由key

topic模式：将路由键和某模式进行匹配，此时队列需要绑定在一个模式上，“#”匹配一个词或多个词，“*”只匹配一个词。

RPC模式：使用RabbitMQ构建RPC系统：客户端和可伸缩RPC服务器

| 模式               | 内容                                                         |
| ------------------ | ------------------------------------------------------------ |
| 简单模式           | 一个生产者，一个消费者(不需要exchange)                       |
| WORK QUEUE         | 一个生产者，多个消费者，每个消费者获取到的消息唯一(不需要exchange) |
| 订阅模式           | 当前exchange为fanout模式，不需要路由                         |
| 路由模式           | 当前exchange未direct模式，只要发送的消息中包含路由对应的key，则发送到对应绑定的队列中 |
| topics模式         | 在 RabbitMQ 中，Topic 模式使用通配符匹配消息的 Routing Key。其中，* 可以匹配一个单词，# 可以匹配零个或多个单词。而这些通配符必须跟 "." 一起使用。 |
| rpc模式            | rpc的具体实现在spring中已经都封装在convertSendAndReceive中，可以当作正常模式的其他模式使用，调用时使用convertSendAndReceive https://www.cnblogs.com/viviel/p/11890216.html |
| Publisher Confirms | Publisher Confirms 机制只能保证消息能够被正确地投递到 RabbitMQ Broker 中，但无法保证消息能够被正确地处理。 https://www.jianshu.com/p/0db95a3e972c |



