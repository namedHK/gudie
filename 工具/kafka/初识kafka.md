# 初识kafka

##基本体系结构

![image-20210409160220350](/Users/kh/Library/Application Support/typora-user-images/image-20210409160220350.png)

producer：消息生产者

consumer：消费消息

broker：kafka的消费节点

> topic：主题，进行消息消费的单位
>
> Partition：分区，一个主题包含多个分区
>
> offset：偏移量，用来表示消费消息的位置，不可跨分区



##同步机制

leader副本

> 负责读写的副本，consumer只能从leader中读取消息

follower副本

>只负责同步的副本，

 AR（Assigned Replicas）

> 所有副本的统称

 OSR（Out-of-Sync Replicas）

> 滞后较多的副本的集合

ISR（In-Sync Replicas）

> 保持一定程度同步的副本的集合，当leader副本出现问题时，会在ISR的副本中选举出新的leader副本

**容灾处理**

> 当leader副本挂了以后，只会从ISR中选举follower副本作为leader副本。ISR中的副本滞后太多会被扔到OSR中，反之OSR中同步到一定范围会回到ISR中

## 读取机制

![image-20210514093504811](/Users/kh/Library/Application Support/typora-user-images/image-20210514093504811.png)

LSO（LogStartOffset）

> 第一条消息的offset

HW（high water）

> 表示一个特定的偏移量（offset），消费者（consumer）只能读取 LSO 和 HW 之前的消息。

LEO (Log End Offset )

> 表示当前文件中待写入消息的 offset

HW 如何定义

> ISR 中所有 follower 中最小的 LEO 为整个分区的 HW 。

kafka 的这种同步机制能够在保证性能的同时保障大部分数据的有效性。

#基本命令

在 bin 目录下，kafka 提供了很多实用脚本

**kafka-topics.sh**

> 创建topic
>
> bin/kafka-topics.sh --zookeeper localhost: 2181/kafka --create --topic topic-demo --replication-factor 3 --partitions 4

