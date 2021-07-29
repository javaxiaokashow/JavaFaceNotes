## Kafka

#### 1.什么是kafka?

Apache Kafka是由Apache开发的一种发布订阅消息系统。

#### 2.kafka的3个关键功能？

- 发布和订阅记录流，类似于消息队列或企业消息传递系统。
- 以容错的持久方式存储记录流。
- 处理记录流。

#### 3.kafka通常用于两大类应用？

- 建立实时流数据管道，以可靠地在系统或应用程序之间获取数据
- 构建实时流应用程序，以转换或响应数据流

#### 4.kafka特性?

1. 消息持久化
2. 高吞吐量
3. 扩展性
4. 多客户端支持
5. Kafka Streams
6. 安全机制
7. 数据备份
8. 轻量级
9. 消息压缩

#### 5.kafka的5个核心Api?

-  Producer API 

-  Consumer API

-  Streams API 

-  Connector API 

-  Admin API 

#### 6.什么是Broker（代理）?

Kafka集群中，一个kafka实例被称为一个代理(Broker)节点。

#### 7.什么是Producer（生产者）?

消息的生产者被称为Producer。

Producer将消息发送到集群指定的主题中存储，同时也自定义算法决定将消息记录发送到哪个分区?

#### 8.什么是Consumer（消费者）?

消息的消费者，从kafka集群中指定的主题读取消息。

#### 9.什么是Topic（主题）?

主题，kafka通过不同的主题却分不同的业务类型的消息记录。

#### 10.什么是Partition（分区）?

每一个Topic可以有一个或者多个分区(Partition)。

#### 11.分区和代理节点的关系？

一个分区只对应一个Broker,一个Broker可以管理多个分区。

#### 12.什么是副本(Replication)?

每个主题在创建时会要求制定它的副本数（默认1）。

#### 13.什么是记录(Record)?

实际写入到kafka集群并且可以被消费者读取的数据。

每条记录包含一个键、值和时间戳。

#### 14.kafka适合哪些场景？

日志收集、消息系统、活动追踪、运营指标、流式处理、时间源等。

#### 15.kafka磁盘选用上？

SSD的性能比普通的磁盘好，这个大家都知道，实际中我们用普通磁盘即可。它使用的方式多是顺序读写操作，一定程度上规避了机械磁盘最大的劣势，即随机读写操作慢，因此SSD的没有太大优势。

#### 16.使用RAID的优势?

- 提供冗余的磁盘存储空间
- 提供负载均衡

#### 17.磁盘容量规划需要考虑到几个因素？

- 新增消息数
- 消息留存时间
- 平均消息大小
- 备份数
- 是否启用压缩

#### 18.Broker使用单个？多个文件目录路径参数？

log.dirs 多个

log.dir 单个

#### 19.一般来说选择哪个参数配置路径？好处？

log.dirs

好处:

提升读写性能，多块物理磁盘同时读写高吞吐。

故障转移。一块磁盘挂了转移到另一个上。

#### 20.自动创建主题的相关参数是?

auto.create.topics.enable

#### 21.解决kafka消息丢失问题？

- 不要使用 producer.send(msg)，而要使用 producer.send(msg, callback)。
- 设置 acks = all。
- 设置 retries 为一个较大的值。
- 设置 unclean.leader.election.enable = false。
- 设置 replication.factor >= 3。
- 设置 min.insync.replicas > 1。
- 确保 replication.factor > min.insync.replicas。
- 确保消息消费完成再提交。

#### 22.如何自定分区策略？

显式地配置生产者端的参数partitioner.class

参数为你实现类的 全限定类名，一般来说实现partition方法即可。

#### 23.kafka压缩消息可能发生的地方？

Producer 、Broker。

#### 24.kafka消息重复问题？

做好幂等。

数据库方面可以（唯一键和主键）避免重复。

在业务上做控制。

#### 25.你知道的kafka监控工具？

- JMXTool 工具
- Kafka Manager
- Burrow
- JMXTrans + InfluxDB + Grafana
- Confluent Control Center

#### 26.kafka follower如何与leader同步数据

Kafka的复制机制既不是完全的同步复制，也不是单纯的异步复制。完全同步复制要求All Alive Follower都复制完，这条消息才会被认为commit，这种复制方式极大的影响了吞吐率。而异步复制方式下，Follower异步的从Leader复制数据，数据只要被Leader写入log就被认为已经commit，这种情况下，如果leader挂掉，会丢失数据，kafka使用ISR的方式很好的均衡了确保数据不丢失以及吞吐率。Follower可以批量的从Leader复制数据，而且Leader充分利用磁盘顺序读以及send file(zero copy)机制，这样极大的提高复制性能，内部批量写磁盘，大幅减少了Follower与Leader的消息量差。

#### 27.什么情况下一个 broker 会从 isr中踢出去

leader会维护一个与其基本保持同步的Replica列表，该列表称为ISR(in-sync Replica)，每个Partition都会有一个ISR，而且是由leader动态维护 ，如果一个follower比一个leader落后太多，或者超过一定时间未发起数据复制请求，则leader将其重ISR中移除 。



#### 参考:

《Kafka并不难学》

《kafka入门与实践》

极客时间：Kafka核心技术与实战

http://kafka.apache.org/

https://blog.csdn.net/qq_28900249/article/details/90346599

![WechatIMG360](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/common1.png)
