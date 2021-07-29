## Dubbo

#### 1.什么是Dubbo?

Dubbo是基于Java的高性能轻量级的RPC分布式服务框架，现已成为 Apache 基金会孵化项目。

官网：http://dubbo.apache.org/en-us/

#### 2.为什么要使用Dubbo?

背景:

随着互联网的快速发展，Web应用程序的规模不断扩大，最后我们发现传统的垂直体系结构（整体式）已无法解决。分布式服务体系结构和流计算体系结构势在必行，迫切需要一个治理系统来确保体系结构的有序发展。

- 开源免费
- 一些核心业务被提取并作为独立的服务提供服务，逐渐形成一个稳定的服务中心，这样前端应用程序就可以更好地响应变化多端的市场需求
- 分布式框架能承受更大规模的流量
- 内部基于netty性能高

#### 3.Dubbo提供了哪3个关键功能？

基于接口的远程调用

容错和负载均衡

自动服务注册和发现

#### 4.你知道哪些机构在用Dubbo吗？

![image-20200423105332840](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200423105332840.png)

#### 5.Dubbo服务的关键节点有哪些?

![image-20200423105925624](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200423105925624.png)

#### 6.说一下Dubbo服务注册流程?

1. 服务容器负责启动，加载，运行服务提供者。
2. 服务提供者在启动时，向注册中心注册自己提供的服务。
3. 服务消费者在启动时，向注册中心订阅自己所需的服务。
4. 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
5. 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
6. 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

#### 7.能画一下服务注册流程图吗？

![image-20200423110344448](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200423110344448.png)

#### 8.Dubbo架构的特点？

连通性、健壮性、伸缩性、以及向未来架构的升级性。

#### 9.对jdk的最小版本需求？

jdk1.6+

#### 10.注册中心的选择？

一般来说选中Zookeeper更稳定更合适。

除了Zookeeper还有Redis注册中心、Multicast注册中心、Simple注册中心。

#### 11.Dubbo的核心配置？用途？

![image-20200423111538534](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200423111538534.png)

#### 12.配置优先级规则？

![image-20200423112118958](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200423112118958.png)

优先级从高到低：

- JVM -D参数，当你部署或者启动应用时，它可以轻易地重写配置，比如，改变dubbo协议端口；
- XML, XML中的当前配置会重写dubbo.properties中的；
- Properties，默认配置，仅仅作用于以上两者没有配置时。

#### 13.如何用代码方式绕过注册中心点对点直连？

```java
…
 
ReferenceConfig<XxxService> reference = new ReferenceConfig<XxxService>(); // 此实例很重，封装了与注册中心的连接以及与提供者的连接，请自行缓存，否则可能造成内存和连接泄漏
// 如果点对点直连，可以用reference.setUrl()指定目标地址，设置url后将绕过注册中心，
// 其中，协议对应provider.setProtocol()的值，端口对应provider.setPort()的值，
// 路径对应service.setPath()的值，如果未设置path，缺省path为接口名
reference.setUrl("dubbo://10.20.130.230:20880/com.xxx.XxxService"); 
 
…
```

#### 14.Dubbo配置来源有几种？分别是？

4种

- JVM System Properties，-D参数
- Externalized Configuration，外部化配置
- ServiceConfig、ReferenceConfig等编程接口采集的配置
- 本地配置文件dubbo.properties

#### 15.如何禁用某个服务的启动检查？

```xml
<dubbo:reference interface = "com.foo.BarService" check = "false" />
```

#### 16.Dubbo 负载均衡策略？默认是？

- 随机负载平衡(默认)

- RoundRobin负载平衡

- 最小活动负载平衡

- 一致的哈希负载平衡

#### 17.上线兼容老版本？

多版本号(version)

#### 18.开发测试环境，想绕过注册中心如何配置？

- xml

```xml
 <dubbo:reference id="xxxService" interface="com.alibaba.xxx.XxxService" url="dubbo://localhost:20890" />

```

- -D

  ```powershell
  java -Dcom.alibaba.xxx.XxxService=dubbo://localhost:20890
  ```

- .properties

  ```properties
  java -Ddubbo.resolve.file=xxx.properties
  ```

```properties
com.alibaba.xxx.XxxService=dubbo://localhost:20890
```

#### 19.集群容错几种方法？

![image-20200423121735540](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200423121735540.png)

#### 20.Dubbo有几种配置方式？

1. Spring
2. Java API 

#### 21.Dubbo有哪些协议？推荐？

-  dubbo://(推荐)
-  rmi://
-  hessian://
-  http://
-  webservice://
-  thrift://
-  memcached://
-  redis://
-  rest://

#### 22.Dubbo使用什么通信框架?

dubbo使用netty。

#### 23.dubbo协议默认端口号？http协议默认端口？hessian?rmi?

- dubbo:20880
- http:80
- hessian:80
- rmi:80

#### 24.Dubbo默认序列化框架?其他的你还知道？

- dubbo协议缺省为hessian2
- rmi协议缺省为java
- http协议缺省为json

#### 25.一个服务有多重实现时，如何处理？

可以用group分组，服务提供方和消费放都指定同一个group。

#### 26.Dubbo服务调用默认是阻塞的？还有其他的？

默认是同步等待结果阻塞的，同时也支持异步调用。

Dubbo 是基于 NIO 的非阻塞实现并行调用，客户端不需要启动多线程即可完成并行调用多个远程服务，相对多线程开销较小，异步调用会返回一个 Future 对象。

#### 27.Dubbo服务追踪解决方案?

- Zipkin  
- Pinpoint
- SkyWalking

#### 28.Dubbo不维护了吗？Dubbo和Dubbox有什么区别？

现在进入了Apache,由apache维护。

Dubbox是当当的扩展项目。

#### 29.Dubbox有什么新功能？

- 支持REST风格远程调用（HTTP + JSON/XML)

- 支持基于Kryo和FST的Java高效序列化实现

- 支持基于嵌入式Tomcat的HTTP remoting体系
- 升级Spring
- 升级ZooKeeper客户端

#### 30.io线程池大小默认？

cpu个数 + 1

#### 31.dubbo://协议适合什么样的服务调用？

采用单一长链接和NIO异步通讯，适用于小数量大并发的服务调用，以及服务消费者机器数远大于服务提供者机器数的情况。

不适合传送大数据量的服务，比如传文件，传视频等，除非请求量很低。

![image-20200423154308365](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200423154308365.png)

#### 32.自动剔除服务什么原理？

zookeeper临时节点，会话保持原理。

#### 33.从 `2.0.5` 版本开始，dubbo支持通过x命令来进行服务治理?

telnet

#### 34.如何用命令查看服务列表？

```powershell
telnet localhost 20880
```

进入命令行。然后执行 ls相关命令:

- `ls`: 显示服务列表
- `ls -l`: 显示服务详细信息列表
- `ls XxxService`: 显示服务的方法列表
- `ls -l XxxService`: 显示服务的方法详细信息列表

#### 35.Dubbo框架设计是怎样的？

![image-20200423162348971](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200423162348971.png)

各层说明:

- **config 配置层**：对外配置接口，以 `ServiceConfig`, `ReferenceConfig` 为中心，可以直接初始化配置类，也可以通过 spring 解析配置生成配置类
- **proxy 服务代理层**：服务接口透明代理，生成服务的客户端 Stub 和服务器端 Skeleton, 以 `ServiceProxy` 为中心，扩展接口为 `ProxyFactory`
- **registry 注册中心层**：封装服务地址的注册与发现，以服务 URL 为中心，扩展接口为 `RegistryFactory`, `Registry`, `RegistryService`
- **cluster 路由层**：封装多个提供者的路由及负载均衡，并桥接注册中心，以 `Invoker` 为中心，扩展接口为 `Cluster`, `Directory`, `Router`, `LoadBalance`
- **monitor 监控层**：RPC 调用次数和调用时间监控，以 `Statistics` 为中心，扩展接口为 `MonitorFactory`, `Monitor`, `MonitorService`
- **protocol 远程调用层**：封装 RPC 调用，以 `Invocation`, `Result` 为中心，扩展接口为 `Protocol`, `Invoker`, `Exporter`
- **exchange 信息交换层**：封装请求响应模式，同步转异步，以 `Request`, `Response` 为中心，扩展接口为 `Exchanger`, `ExchangeChannel`, `ExchangeClient`, `ExchangeServer`
- **transport 网络传输层**：抽象 mina 和 netty 为统一接口，以 `Message` 为中心，扩展接口为 `Channel`, `Transporter`, `Client`, `Server`, `Codec`
- **serialize 数据序列化层**：可复用的一些工具，扩展接口为 `Serialization`, `ObjectInput`, `ObjectOutput`, `ThreadPool`

#### 36.你读过Dubbo的源码吗？

这个问题其实面试中如果问dubbo的话，基本就会带这个问题。有时间的话，大家可以下载源码，读一读，如果大家有兴趣的话，我会出后续文章。

参考：http://dubbo.apache.org/en-us/

![WechatIMG360](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/common1.png)

