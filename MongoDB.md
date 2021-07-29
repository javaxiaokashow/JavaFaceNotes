## MongoDB

#### 1.什么是MongoDB

```
MongoDB是一个文档数据库，提供好的性能，领先的非关系型数据库。采用BSON存储文档数据。
BSON（）是一种类json的一种二进制形式的存储格式，简称Binary JSON.
相对于json多了date类型和二进制数组。
```

#### 2.MongoDB的优势有哪些

- 面向文档的存储：以 JSON 格式的文档保存数据。
- 任何属性都可以建立索引。
- 复制以及高可扩展性。
- 自动分片。
- 丰富的查询功能。
- 快速的即时更新。

#### 3.什么是数据库

```
数据库可以看成是一个电子化的文件柜,用户可以对文件中的数据运行新增、检索、更新、删除等操作。数据库是一个
所有集合的容器，在文件系统中每一个数据库都有一个相关的物理文件。
```

#### 4.什么是集合(表)

```
集合就是一组 MongoDB 文档。它相当于关系型数据库（RDBMS）中的表这种概念。集合位于单独的一个数据库中。
一个集合内的多个文档可以有多个不同的字段。一般来说，集合中的文档都有着相同或相关的目的。
```

#### 5 什么是文档(记录)

```
　　文档由一组key value组成。文档是动态模式,这意味着同一集合里的文档不需要有相同的字段和结构。在关系型
数据库中table中的每一条记录相当于MongoDB中的一个文档
```

#### 6 MongoDB和关系型数据库术语对比图

![img](https://img2018.cnblogs.com/blog/1521877/201904/1521877-20190429170250020-1693717595.png)

#### 7.什么是非关系型数据库

```
　非关系型数据库的显著特点是不使用SQL作为查询语言，数据存储不需要特定的表格模式。
```

#### 8.为什么用MOngoDB？

- 架构简单
- 没有复杂的连接
- 深度查询能力,MongoDB支持动态查询。
- 容易调试
- 容易扩展
- 不需要转化/映射应用对象到数据库对象
- 使用内部内存作为存储工作区,以便更快的存取数据。

#### 9.MongoDB中的命名空间是什么意思?

```
MongoDB内部有预分配空间的机制，每个预分配的文件都用0进行填充。

数据文件每新分配一次，它的大小都是上一个数据文件大小的2倍，每个数据文件最大2G。

MongoDB每个集合和每个索引都对应一个命名空间，这些命名空间的元数据集中在16M的*.ns文件中，平均每个命名占用约 628 字节，也即整个数据库的命名空间的上限约为24000。

如果每个集合有一个索引（比如默认的_id索引），那么最多可以创建12000个集合。如果索引数更多，则可创建的集合数就更少了。同时，如果集合数太多，一些操作也会变慢。

要建立更多的集合的话，MongoDB 也是支持的，只需要在启动时加上“--nssize”参数，这样对应数据库的命名空间文件就可以变得更大以便保存更多的命名。这个命名空间文件（.ns文件）最大可以为 2G。

每个命名空间对应的盘区不一定是连续的。与数据文件增长相同，每个命名空间对应的盘区大小都是随分配次数不断增长的。目的是为了平衡命名空间浪费的空间与保持一个命名空间数据的连续性。

需要注意的一个命名空间$freelist，这个命名空间用于记录不再使用的盘区（被删除的Collection或索引）。每当命名空间需要分配新盘区时，会先查看$freelist是否有大小合适的盘区可以使用，如果有就回收空闲的磁盘空间。
```

#### 10.在哪些场景使用MongoDB

- 大数据
- 内容管理系统
- 移动端Apps
- 数据管理

#### 11.monogodb 中的分片什么意思

```
分片是将数据水平切分到不同的物理节点。当应用数据越来越大的时候，数据量也会越来越大。当数据量增长
时，单台机器有可能无法存储数据或可接受的读取写入吞吐量。利用分片技术可以添加更多的机器来应对数据量增加
以及读写操作的要求。
```

#### 12.为什么要在MongoDB中使用分析器

```
mongodb中包括了一个可以显示数据库中每个操作性能特点的数据库分析器.通过这个分析器你可以找到比预期慢
的查询(或写操作);利用这一信息,比如,可以确定是否需要添加索引。
```

#### 13.MongoDB支持主键外键关系吗

```
默认MongoDB不支持主键和外键关系。 用Mongodb本身的API需要硬编码才能实现外键关联，不够直观且难度较大
```

####  14.MongoDB支持哪些数据类型


- String
- Integer
- Double
- Boolean
- Object
- Object ID
- Arrays
- Min/Max Keys
- Datetime
- Code
- Regular Expression等

#### 15.为什么要在MongoDB中用"Code"数据类型

```
"Code"类型用于在文档中存储 JavaScript 代码。
```

#### 16.32位系统上有什么细微差别?

journaling会激活额外的内存映射文件。这将进一步抑制32位版本上的数据库大小。因此，现在journaling在32位系统上默认是禁用的。

#### 17.为什么在MongoDB中使用"Object ID"数据类型

```
"ObjectID"数据类型用于存储文档id
```

#### 18."ObjectID"有哪些部分组成

```
一共有四部分组成:时间戳、客户端ID、客户进程ID、三个字节的增量计数器

_id是一个 12 字节长的十六进制数，它保证了每一个文档的唯一性。在插入文档时，需要提供_id。如果你不提供，那么 MongoDB 就会为每一文档提供一个唯一的 id。_id的头 4 个字节代表的是当前的时间戳，接着的后 3 个字节表示的是机器 id 号，接着的 2 个字节表示 MongoDB 服务器进程 id，最后的 3 个字节代表递增值。
```

#### 19.在MongoDb中什么是索引

```
索引用于高效的执行查询,没有索引的MongoDB将扫描整个集合中的所有文档,这种扫描效率很低,需要处理大量的数据.
索引是一种特殊的数据结构,将一小块数据集合保存为容易遍历的形式.索引能够存储某种特殊字段或字段集的值,并按照索引指定的方式将字段值进行排序.
```

#### 20.如何添加索引

```
使用db.collection.createIndex()在集合中创建一个索引
```

#### 21.如何查询集合中的文档

```
db.collectionName.find({key:value})
```

#### 22.用什么方法可以格式化输出结果

```
db.collectionName.find().pretty()
```

#### 23.如何使用"AND"或"OR"条件循环查询集合中的文档

```
db.mycol.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```

####  24.你怎么比较MongoDB、CouchDB及CouchBase?

MongoDB和CouchDB都是面向文档的数据库。MongoDB和CouchDB都是开源NoSQL数据库的最典型代表。 除了都以文档形式存储外它们没有其他的共同点。MongoDB和CouchDB在数据模型实现、接口、对象存储以及复制方法等方面有很多不同。

细节可以参见下面的链接：

MongDB vs CouchDB

CouchDB vs CouchBase

#### 25.名字空间(namespace)是什么?

MongoDB存储BSON对象在丛集(collection)中。数据库名字和丛集名字以句点连结起来叫做名字空间(namespace)。

#### 26.如果用户移除对象的属性，该属性是否从存储层中删除?

是的，用户移除属性然后对象会重新保存(re-save())。

#### 27.什么是聚合

聚合操作能够处理数据记录并返回计算结果。聚合操作能将多个文档中的值组合起来，对成组数据执行各种操作，返回单一的结果。它相当于 SQL 中的 count(*) 组合 group by。对于 MongoDB 中的聚合操作，应该使用`aggregate()`方法。

```
db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)
```

#### 28.在MongoDB中什么是副本集（避免单点故障）

```
在MongoDB中副本集由一组MongoDB实例组成，包括一个主节点多个次节点，MongoDB客户端的所有数据都
写入主节点(Primary),副节点从主节点同步写入数据，以保持所有复制集内存储相同的数据，提高数据可用性。
```

#### 29.什么是NoSQL数据库？NoSQL和RDBMS有什么区别？在哪些情况下使用和不使用NoSQL数据库？

```
NoSQL是非关系型数据库，NoSQL = Not Only SQL。  关系型数据库采用的结构化的数据，NoSQL采用的是键值对的方式存储数据。

在处理非结构化/半结构化的大数据时；在水平方向上进行扩展时；随时应对动态增加的数据项时可以优先考虑使用NoSQL数据库。

在考虑数据库的成熟度；支持；分析和商业智能；管理及专业性等问题时，应优先考虑关系型数据库。
```

#### 30.MongoDB支持存储过程吗？如果支持的话，怎么用？

```
MongoDB支持存储过程，它是javascript写的，保存在db.system.js表中。
```

#### 31.如何理解MongoDB中的GridFS机制，MongoDB为何使用GridFS来存储文件？

GridFS是一种将大型文件存储在MongoDB中的文件规范。使用GridFS可以将大文件分隔成多个小文档存放，这样我们能够有效的保存大文档，而且解决了BSON对象有限制的问题。

#### 32.如何执行事务/加锁?

MongoDB没有使用传统的锁或者复杂的带回滚的事务，因为它设计的宗旨是轻量，快速以及可预计的高性能。可以把它类比成MySQL MylSAM的自动提交模式。通过精简对事务的支持，性能得到了提升，特别是在一个可能会穿过多个服务器的系统里。

#### 33.启用备份故障恢复需要多久?

从备份数据库声明主数据库宕机到选出一个备份数据库作为新的主数据库将花费10到30秒时间。这期间在主数据库上的操作将会失败--包括写入和强一致性读取(strong consistent read)操作。然而，你还能在第二数据库上执行最终一致性查询(eventually consistent query)(在slaveOk模式下)，即使在这段时间里。

#### 34.我应该启动一个集群分片(sharded)还是一个非集群分片的 MongoDB 环境?

为开发便捷起见，我们建议以非集群分片(unsharded)方式开始一个 MongoDB 环境，除非一台服务器不足以存放你的初始数据集。从非集群分片升级到集群分片(sharding)是无缝的，所以在你的数据集还不是很大的时候没必要考虑集群分片(sharding)。

#### 35.分片(sharding)和复制(replication)是怎样工作的?

每一个分片(shard)是一个分区数据的逻辑集合。分片可能由单一服务器或者集群组成，我们推荐为每一个分片(shard)使用集群。

#### 36.数据在什么时候才会扩展到多个分片(shard)里?

MongoDB 分片是基于区域(range)的。所以一个集合(collection)中的所有的对象都被存放到一个块(chunk)中。只有当存在多余一个块的时候，才会有多个分片获取数据的选项。现在，每个默认块的大小是 64Mb，所以你需要至少 64 Mb 空间才可以实施一个迁移。

#### 37.我可以把moveChunk目录里的旧文件删除吗?

没问题，这些文件是在分片(shard)进行均衡操作(balancing)的时候产生的临时文件。一旦这些操作已经完成，相关的临时文件也应该被删除掉。但目前清理工作是需要手动的，所以请小心地考虑再释放这些文件的空间。

#### 38.分片(sharding)和复制(replication)是怎样工作的?

每一个分片(shard)是一个分区数据的逻辑集合.分片可能由单一服务器或者集群组成,我们推荐为每一个分片(shard)使用集群。

#### 39.如果块移动操作(movechunk)失败了,我需要手动清除部分转移的文档吗?

 不需要,移动操作是一致(consistent)并且是确定性的(deterministic);一次失败后,移动操作会不断重试;当完成后,数据只会出现在新的分片里(shard)。

#### 40.mongodb是否支持事务

MongoDB 4.0的新特性——事务（Transactions）：MongoDB 是不支持事务的，因此开发者在需要用到事务的时候，不得不借用其他工具，在业务代码层面去弥补数据库的不足。

事务和会话(Sessions)关联，一个会话同一时刻只能开启一个事务操作，当一个会话断开，这个会话中的事务也会结束。

#### 41.哪些语言支持MongoDB?

- C
- C++
- C#
- Java
- Node.js
- Perl
- Php 等

#### 42.如何使用"AND"或"OR"条件循环查询集合中的文档

在`find()`方法中，如果传入多个键，并用逗号(`,`)分隔它们，那么 MongoDB 会把它看成是**AND**条件。

```text
>db.mycol.find({key1:value1, key2:value2}).pretty()
```

若基于**OR**条件来查询文档，可以使用关键字**$or**。

```text
>db.mycol.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```

#### 43.如何删除文档

MongoDB 利用 `remove()` 方法 清除集合中的文档。它有 2 个可选参数：

- deletion criteria：（可选）删除文档的标准。
- justOne：（可选）如果设为 true 或 1，则只删除一个文档。

```text
>db.collectionName.remove({key:value})
```

#### 44.在MongoDB中如何排序

MongoDB 中的文档排序是通过`sort()`方法来实现的。`sort()`方法可以通过一些参数来指定要进行排序的字段，并使用 1 和 -1 来指定排序方式，其中 1 表示升序，而 -1 表示降序。

```text
>db.connectionName.find({key:value}).sort({columnName:1})
```

#### 45.举例说明您将从Redis和MongoDB一起使用中受益的情况？

Redis和MongoDB可以一起使用，效果很好。Craiglist是一家以运行MongoDB和Redis（以及MySQL和Sphinx）而闻名的公司。请参阅Jeremy
Zawodny的[演示文稿](http://www.slideshare.net/jzawodn/living-with-sql-and-nosql-at-craigslist-a-pragmatic-approach)。

MongoDB对于以各种方式索引的持久性，面向文档的数据很有趣。对于易失性数据或对延迟敏感的半永久性数据，Redis更有趣。

以下是在MongoDB之上具体使用Redis的一些示例。

- 2.2版之前的MongoDB还没有到期机制。上限集合不能真正用于实现真正的TTL。Redis具有基于TTL的过期机制，可以方便地存储易失性数据。例如，用户会话通常存储在Redis中，而用户数据将存储在MongoDB中并建立索引。请注意，MongoDB 2.2在集合级别引入了一种低精度的过期机制（例如，用于清除数据）。
- Redis提供了一种方便的集合数据类型及其关联的操作（联合，交集，多个集合的差等）。在此功能之上实现基本的多面搜索或标记引擎非常容易，这是对MongoDB更传统的索引功能的有趣补充。
- Redis支持有效地阻止列表上的弹出操作。这可用于实现临时分布式排队系统。它比MongoDB可尾游标IMO更具灵活性，因为后端应用程序可以在超时的情况下侦听多个队列，原子地将项目转移到另一个队列，等等…如果应用程序需要排队，则将队列存储在Redis中是有意义的，并将持久性功能数据保留在MongoDB中。
- Redis还提供了发布/订阅机制。在分布式应用程序中，事件传播系统可能会有用。对于持久性数据保留在MongoDB中而言，这也是Redis的绝佳用例。

由于使用MongoDB设计数据模型要比使用Redis容易得多（Redis更底层），因此可以从MongoDB的主要持久性数据灵活性和Redis提供的额外功能（低延迟）中受益。
，项目到期，队列，发布/订阅，原子块等）。这确实是一个很好的组合。

请注意，您永远不要在同一台机器上运行Redis和MongoDB服务器。MongoDB内存被设计为可以换出，Redis不是。如果MongoDB触发某些交换活动，则Redis的性能将是灾难性的。它们应该在不同的节点上隔离。

#### 46.MongoDB + Azure + Android：com.mongodb.WriteConcernException err：“非主用户”代码：“ 10058”

**背景** ：

嗨，我正在Azure上运行MongoDB副本集，并已从Android应用程序中远程连接到它。我已使读取在所有实例上都能很好地工作（已更新：因为允许它们在主节点和辅助节点上读取）。但是，对数据库的写入仍然会出现间歇性错误，并出现以下错误，因为写入必须仅在主节点上进行。

另外，如果您可以提供更多具体资源来解决此问题，那么这也将非常有帮助。我已经阅读了大多数文档，并搜索了很多此错误。

**问题** ：

如何防止此错误并允许100％的时间写入？

```
E/AndroidRuntime(): com.mongodb.WriteConcernException: {
        "serverUsed" : "/<my-remote-ip>:27017" , "err" : "not master" , 
        "code" : 10058 , "n" : 0 , "lastOp" : { "$ts" : 0 , "$inc" : 0} , 
        "connectionId" : 1918 , "ok" : 1.0}
```

**堆栈跟踪** ：

```
E/AndroidRuntime(13731): FATAL EXCEPTION: Thread-7629
E/AndroidRuntime(13731): Process: com.myapplication.examplemongodb, PID: 13731
E/AndroidRuntime(13731): com.mongodb.WriteConcernException: { "serverUsed" : "/<my-remote-ip>:27017" , "err" : "not master" , "code" : 10058 , "n" : 0 , "lastOp" : { "$ts" : 0 , "$inc" : 0} , "connectionId" : 1918 , "ok" : 1.0}
E/AndroidRuntime(13731):    at com.mongodb.CommandResult.getException(CommandResult.java:77)
E/AndroidRuntime(13731):    at com.mongodb.CommandResult.throwOnError(CommandResult.java:110)
E/AndroidRuntime(13731):    at com.mongodb.DBTCPConnector._checkWriteError(DBTCPConnector.java:102)
E/AndroidRuntime(13731):    at com.mongodb.DBTCPConnector.say(DBTCPConnector.java:142)
E/AndroidRuntime(13731):    at com.mongodb.DBTCPConnector.say(DBTCPConnector.java:115)
E/AndroidRuntime(13731):    at com.mongodb.DBApiLayer$MyCollection.insert(DBApiLayer.java:248)
E/AndroidRuntime(13731):    at com.mongodb.DBApiLayer$MyCollection.insert(DBApiLayer.java:204)
E/AndroidRuntime(13731):    at com.mongodb.DBCollection.insert(DBCollection.java:76)
E/AndroidRuntime(13731):    at com.mongodb.DBCollection.insert(DBCollection.java:60)
E/AndroidRuntime(13731):    at com.mongodb.DBCollection.insert(DBCollection.java:105)
E/AndroidRuntime(13731):    at com.myapplication.examplemongodb.ActivityMain$1.run(ActivityMain.java:83)
E/AndroidRuntime(13731):    at java.lang.Thread.run(Thread.java:841)
```

**注意事项** ：

- 我正在使用[mongo-java-driver v2.11.3](http://central.maven.org/maven2/org/mongodb/mongo-java-driver/)。

- 我使用了

  mongo-azure库

  来帮助创建具有两个工作角色的MongoDB副本集。

  - （如果您还有其他资源，那么我很乐意阅读。我已经阅读了GitHub自述文件[this](http://docs.mongodb.org/ecosystem/tutorial/deploy-mongodb-worker-roles-in-azure/)，[this](http://docs.mongodb.org/ecosystem/tutorial/configure-worker-roles-in-azure/)和其他一些与MongoDB / Azure不相关的内容。但是，这些资源不是更新，也不详细。）

**可能的解决方案** ：

- 我认为这与设置副本集有关。
- 我不确定这种情况是否会发生，因为我只有两个实例副本集（一个主要副本和一个次要副本），并且他们正在为谁想成为主要副本而进行争夺（阅读：投票）。也许需要仲裁员？但是，我目前不知道该怎么做。

**更新** ：

- 感谢@David Makogon的帮助，我非常确定问题在于如何建立与Azure的连接以及如何访问辅助角色。因此，这是我关于系统配置方式的最新注释：
  - 两个工作角色（MongoDB.WindowsAzure.MongoDBRole），我通过`TCP Input Endpoint`Android应用程序通过端口27017 直接连接到这些角色。正如@David所说，我目前无法控制连接到哪个实例。
  - 我不做任何事情的一个Web角色（MongoDB.WindowsAzure.Manager）`HTTP Input Endpoint`在端口80上有一个。默认情况下，这是我上面提到的mongo-azure库的默认设置。我不确定是否应该对此做任何事情。

#### 47.使用Spring Security + Spring数据+ MongoDB进行身份验证

我想将Spring安全性与MongoDB结合使用（使用Spring数据），并从我自己的数据库中检索用户以获取Spring安全性。但是，由于我的用户服务类型似乎不受支持，所以我不能这样做。

这是我的UserService类：

```
public class UserService {
    private ApplicationContext applicationContext;
    private MongoOperations mongoOperations;

    public UserService() {
        applicationContext = new AnnotationConfigApplicationContext(MongoConfig.class);
        mongoOperations = (MongoOperations) applicationContext.getBean("mongoTemplate");
    }

    public User find(String username) {
        return mongoOperations.findOne(Query.query(Criteria.where("username").is(username)), User.class);
    }
}
```

和我的SecurityConfig类：

```
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    UserService userService;

    @Autowired
    public void configAuthBuilder(AuthenticationManagerBuilder builder) throws Exception {
        builder.userDetailsService(userService); //THIS DOES NOT WORK
        builder.inMemoryAuthentication().withUser("username").password("password").roles("USER");
    }

}
```

我评论的那句话说：

```
The inferred type UserService is not a valid substitute for the bounded parameter <T extends UserDetail
```

#### 48.Java ORM for MongoDB的开销是多少

将Java ORM用于MongoDB的开销是多少？或者更好的是，我们在基本驱动程序级别进行读写？

我们将为我们的要求之一添加Mongo DB。

有对Java夫妇的Java ORM映射工具
-morphia
-Spring数据
\- [其他](http://www.mongodb.org/display/DOCS/Java+Language+Center)

Morphia的最新版本已于一年多以前发布，
但Spring数据得到了积极维护。如果我现在要开始使用哪一个，

#### 49.使用Jackson PTH和Spring Data MongoDB DBRef的Java到JSON序列化生成额外的目标属性

从Java序列化为JSON时，`target`当使用`@DBRef`带有延迟加载和Jackson的多态类型处理的Spring Data MongoDB
批注时，Jackson会为引用的实体生成一个额外的属性。为什么会发生这种情况，并且可以省略多余的`target`属性？

**代码示例**

```
@Document(collection = "cdBox")
public class CDBox {
  @Id
  public String id;

  @DBRef(lazy = true)
  public List<Product> products;
}

@Document(collection = "album")
public class Album extends Product {
  @DBRef(lazy = true)
  public List<Song> songs;
}

@Document(collection = "single")
public class Single extends Product {
  @DBRef(lazy = true)
  public List<Song> songs;
}

@Document(collection = "song")
public class Song {
  @Id
  public String id;

  public String title;
}

@JsonTypeInfo(use = JsonTypeInfo.Id.NAME,
                    property = "productType",
                    include = JsonTypeInfo.As.EXTERNAL_PROPERTY)
@JsonSubTypes(value = {
    @JsonSubTypes.Type(value = Single.class),
    @JsonSubTypes.Type(value = Album.class)
})
public abstract class Product {
  @Id
  public String id;
}
```

**生成的JSON**

```
{
  "id": "someId1",
  "products": [
    {
      "id": "someId2",
      "songs": [
        {
        "id": "someId3",
        "title": "Some title",
        "target": {
          "id": "someId3",
          "title": "Some title"
          }
        }
      ]
    }
  ]
}
```

#### 50.表示MongoDB中具有属性的多对多关系的最佳模型

代表具有属性的多对多关系的最“ mongo”方式是什么？

因此，例如：

##### 介绍

------

MYSQL表

```
people` => `firstName, lastName, ...
Movies` => `name, length ..
peopleMovies` => `movieId, personId, language, role
```

##### 解决方案1

------

将人们嵌入电影中…？

在MongoDB中，我知道这样`denormalize and embed`做很好，但是我不想让`embed`人们看电影，从逻辑上讲这没有任何意义。因为人们不一定只属于电影。

##### 解决方案2

------

```
People`并且`Movies`将两个单独的集合。 `People`=>嵌入`[{movieId: 12, personId: 1, language: "English", role: "Main"} ...]
Movies` =>嵌入 `[{movieId: 12, personId: 1, language: "English", role: "Main"} ...]
```

该解决方案的问题在于，当我们要`role`为特定对象更新人员时，`movie`我们需要运行两个更新查询以确保两个集合中的数据同步。

##### 解决方案3

------

我们还可以做一些与关系有关的事情，最后得到三个集合

```
People`=> `firstName, lastName, ...` `Movies`=> `name, length ..`
`Castings`=>`movieId, personId, language, role
```

问题在于，由于MongoDB中缺少join语句，因此需要`3 queries`从人那里去->电影，反之亦然。

这是我的问题，还有什么其他方式可以对此类事物进行建模`MongoDB`以及更多`NoSQL`方式。就提供的解决方案而言，在mongo的性能和约定方面哪一种是最好的。

#### 参考

https://www.cnblogs.com/angle6-liu/p/10791875.html

https://www.jb51.net/article/179908.htm

https://zhuanlan.zhihu.com/p/37437657

https://blog.csdn.net/weixin_45669794/article/details/102991806

http://www.zzvips.com/article/69641.html

https://funyan.cn/p/6173.html

http://www.mianshigee.com/question/132761aes/

http://www.mianshigee.com/question/65730fcs/

http://www.mianshigee.com/question/67979kov/

http://www.mianshigee.com/question/159947bya/