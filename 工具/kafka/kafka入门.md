#kafka入门

- zookeeper安装和启动

  ```shell
   cd conf
   cp zoo_sample.cfg zoo.cfg
  ```

  配置参考

  ```properties
  # ZooKeeper服务器心跳时间，单位为ms
  tickTime=2000
  # 允许follower连接并同步到leader的初始化连接时间，以tickTime的倍数来表示
  initLimit=10
  # leader与follower心跳检测最大容忍时间，响应超过syncLimit*tickTime，leader认为
  # follower“死掉”，从服务器列表中删除follower
  syncLimit=5
  # 数据目录
  dataDir=/tmp/zookeeper/data
  # 日志目录
  dataLogDir=/tmp/zookeeper/log
  # ZooKeeper对外服务端口
  clientPort=2181		
  ```

  集群配置

  ```properties
  server.0=192.168.0.2:2888:3888
  server.1=192.168.0.3:2888:3888
  server.2=192.168.0.4:2888:3888
  ```

  查看节点内容

  ```shell
  sh bin/zkCli.sh -server 127.0.0.1:2181 
  #查看节点
  ls /brokers
  #查看消费者
  ls /consumers
  ```

- kafka安装和启动

  主要配置

  ```properties
  # broker的编号，如果集群中有多个broker，则每个broker的编号需要设置的不同
  broker.id=0
  # broker对外提供的服务入口地址
  listeners=PLAINTEXT://localhost:9092
  # 存放消息日志文件的地址
  log.dirs=/tmp/kafka-logs
  # Kafka所需的ZooKeeper集群地址，为了方便演示，我们假设Kafka和ZooKeeper都安装在本机
  zookeeper.connect=localhost:2181/kafka
  ```

  后台启动

  ```shell
  bin/kafka-server-start.sh –daemon config/server.properties
  # 或者
  bin/kafka-server-start.sh config/server.properties &
  ```

  生产主题

  ```shell
  #其中 --zookeeper 指定了 Kafka 所连接的 ZooKeeper 服务地址，--topic 指定了所要创建主题的名称，--replication-factor 指定了副本因子，--partitions 指定了分区个数，--create 是创建主题的动作指令。
  bin/kafka-topics.sh --zookeeper localhost: 2181/kafka --create --topic topic-demo --replication-factor 3 --partitions 4
  
  #还可以通过 --describe 展示主题的更多具体信息
  bin/kafka-topics.sh --zookeeper localhost: 2181/kafka --describe --topic topic-demo
  
  ```

  

  

