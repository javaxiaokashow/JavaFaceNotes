## Netty

#### 1.Netty的特点？

- 一个高性能、异步事件驱动的NIO框架，它提供了对TCP、UDP和文件传输的支持
- 使用更高效的socket底层，对epoll空轮询引起的cpu占用飙升在内部进行了处理，避免了直接使用NIO的陷阱，简化了NIO的处理方式。
- 采用多种decoder/encoder 支持，对TCP粘包/分包进行自动化处理
- 可使用接受/处理线程池，提高连接效率，对重连、心跳检测的简单支持
- 可配置IO线程数、TCP参数， TCP接收和发送缓冲区使用直接内存代替堆内存，通过内存池的方式循环利用ByteBuf
- 通过引用计数器及时申请释放不再引用的对象，降低了GC频率
- 使用单线程串行化的方式，高效的Reactor线程模型
- 大量使用了volitale、使用了CAS和原子类、线程安全类的使用、读写锁的使用

#### 2.Netty的线程模型？

- Netty通过Reactor模型基于多路复用器接收并处理用户请求，内部实现了两个线程池，boss线程池和work线程池，其中boss线程池的线程负责处理请求的accept事件，当接收到accept事件的请求时，把对应的socket封装到一个NioSocketChannel中，并交给work线程池，其中work线程池负责请求的read和write事件，由对应的Handler处理。
- 单线程模型：所有I/O操作都由一个线程完成，即多路复用、事件分发和处理都是在一个Reactor线程上完成的。既要接收客户端的连接请求,向服务端发起连接，又要发送/读取请求或应答/响应消息。一个NIO 线程同时处理成百上千的链路，性能上无法支撑，速度慢，若线程进入死循环，整个程序不可用，对于高负载、大并发的应用场景不合适。
- 多线程模型：有一个NIO 线程（Acceptor） 只负责监听服务端，接收客户端的TCP 连接请求；NIO 线程池负责网络IO 的操作，即消息的读取、解码、编码和发送；1 个NIO 线程可以同时处理N 条链路，但是1 个链路只对应1 个NIO 线程，这是为了防止发生并发操作问题。但在并发百万客户端连接或需要安全认证时，一个Acceptor 线程可能会存在性能不足问题。
- 主从多线程模型：Acceptor 线程用于绑定监听端口，接收客户端连接，将SocketChannel 从主线程池的Reactor 线程的多路复用器上移除，重新注册到Sub 线程池的线程上，用于处理I/O 的读写等操作，从而保证mainReactor只负责接入认证、握手等操作；

#### 3.TCP 粘包/拆包的原因及解决方法？

- TCP是以流的方式来处理数据，一个完整的包可能会被TCP拆分成多个包进行发送，也可能把小的封装成一个大的数据包发送。
- TCP粘包/分包的原因：
  - 应用程序写入的字节大小大于套接字发送缓冲区的大小，会发生拆包现象，而应用程序写入数据小于套接字缓冲区大小，网卡将应用多次写入的数据发送到网络上，这将会发生粘包现象；
  - 进行MSS大小的TCP分段，当TCP报文长度-TCP头部长度>MSS的时候将发生拆包
  - 以太网帧的payload（净荷）大于MTU（1500字节）进行ip分片。
- 解决方法
  - 消息定长：FixedLengthFrameDecoder类
  - 包尾增加特殊字符分割：行分隔符类：LineBasedFrameDecoder或自定义分隔符类 ：DelimiterBasedFrameDecoder
  - 将消息分为消息头和消息体：LengthFieldBasedFrameDecoder类。分为有头部的拆包与粘包、长度字段在前且有头部的拆包与粘包、多扩展头部的拆包与粘包。

#### 4.了解哪几种序列化协议？

- 序列化（编码）是将对象序列化为二进制形式（字节数组），主要用于网络传输、数据持久化等；而反序列化（解码）则是将从网络、磁盘等读取的字节数组还原成原始对象，主要用于网络传输对象的解码，以便完成远程调用。
- 影响序列化性能的关键因素：序列化后的码流大小（网络带宽的占用）、序列化的性能（CPU资源占用）；是否支持跨语言（异构系统的对接和开发语言切换）。
- Java默认提供的序列化：无法跨语言、序列化后的码流太大、序列化的性能差
- XML，优点：人机可读性好，可指定元素或特性的名称。缺点：序列化数据只包含数据本身以及类的结构，不包括类型标识和程序集信息；只能序列化公共属性和字段；不能序列化方法；文件庞大，文件格式复杂，传输占带宽。适用场景：当做配置文件存储数据，实时数据转换。
- JSON，是一种轻量级的数据交换格式，优点：兼容性高、数据格式比较简单，易于读写、序列化后数据较小，可扩展性好，兼容性好、与XML相比，其协议比较简单，解析速度比较快。缺点：数据的描述性比XML差、不适合性能要求为ms级别的情况、额外空间开销比较大。适用场景（可替代ＸＭＬ）：跨防火墙访问、可调式性要求高、基于Web browser的Ajax请求、传输数据量相对小，实时性要求相对低（例如秒级别）的服务。
- Fastjson，采用一种“假定有序快速匹配”的算法。优点：接口简单易用、目前java语言中最快的json库。缺点：过于注重快，而偏离了“标准”及功能性、代码质量不高，文档不全。适用场景：协议交互、Web输出、Android客户端
- Thrift，不仅是序列化协议，还是一个RPC框架。优点：序列化后的体积小, 速度快、支持多种语言和丰富的数据类型、对于数据字段的增删具有较强的兼容性、支持二进制压缩编码。缺点：使用者较少、跨防火墙访问时，不安全、不具有可读性，调试代码时相对困难、不能与其他传输层协议共同使用（例如HTTP）、无法支持向持久层直接读写数据，即不适合做数据持久化序列化协议。适用场景：分布式系统的RPC解决方案
- Avro，Hadoop的一个子项目，解决了JSON的冗长和没有IDL的问题。优点：支持丰富的数据类型、简单的动态语言结合功能、具有自我描述属性、提高了数据解析速度、快速可压缩的二进制数据形式、可以实现远程过程调用RPC、支持跨编程语言实现。缺点：对于习惯于静态类型语言的用户不直观。适用场景：在Hadoop中做Hive、Pig和MapReduce的持久化数据格式。
- Protobuf，将数据结构以.proto文件进行描述，通过代码生成工具可以生成对应数据结构的POJO对象和Protobuf相关的方法和属性。优点：序列化后码流小，性能高、结构化数据存储格式（XML JSON等）、通过标识字段的顺序，可以实现协议的前向兼容、结构化的文档更容易管理和维护。缺点：需要依赖于工具生成代码、支持的语言相对较少，官方只支持Java 、C++ 、python。适用场景：对性能要求高的RPC调用、具有良好的跨防火墙的访问属性、适合应用层对象的持久化
- 其它
  - protostuff 基于protobuf协议，但不需要配置proto文件，直接导包即可
  - Jboss marshaling 可以直接序列化java类， 无须实java.io.Serializable接口
  - Message pack 一个高效的二进制序列化格式
  - Hessian 采用二进制协议的轻量级remoting onhttp工具
  - kryo 基于protobuf协议，只支持java语言,需要注册（Registration），然后序列化（Output），反序列化（Input）

#### 5.如何选择序列化协议？

- 具体场景
  - 对于公司间的系统调用，如果性能要求在100ms以上的服务，基于XML的SOAP协议是一个值得考虑的方案。
  - 基于Web browser的Ajax，以及Mobile app与服务端之间的通讯，JSON协议是首选。对于性能要求不太高，或者以动态类型语言为主，或者传输数据载荷很小的的运用场景，JSON也是非常不错的选择。
  - 对于调试环境比较恶劣的场景，采用JSON或XML能够极大的提高调试效率，降低系统开发成本。
  - 当对性能和简洁性有极高要求的场景，Protobuf，Thrift，Avro之间具有一定的竞争关系。
  - 对于T级别的数据的持久化应用场景，Protobuf和Avro是首要选择。如果持久化后的数据存储在hadoop子项目里，Avro会是更好的选择。
  - 对于持久层非Hadoop项目，以静态类型语言为主的应用场景，Protobuf会更符合静态类型语言工程师的开发习惯。由于Avro的设计理念偏向于动态类型语言，对于动态语言为主的应用场景，Avro是更好的选择。
  - 如果需要提供一个完整的RPC解决方案，Thrift是一个好的选择。
  - 如果序列化之后需要支持不同的传输层协议，或者需要跨防火墙访问的高性能场景，Protobuf可以优先考虑。
- protobuf的数据类型有多种：bool、double、float、int32、int64、string、bytes、enum、message。protobuf的限定符：required: 必须赋值，不能为空、optional:字段可以赋值，也可以不赋值、repeated: 该字段可以重复任意次数（包括0次）、枚举；只能用指定的常量集中的一个值作为其值；
- protobuf的基本规则：每个消息中必须至少留有一个required类型的字段、包含0个或多个optional类型的字段；repeated表示的字段可以包含0个或多个数据；[1,15]之内的标识号在编码的时候会占用一个字节（常用），[16,2047]之内的标识号则占用2个字节，标识号一定不能重复、使用消息类型，也可以将消息嵌套任意多层，可用嵌套消息类型来代替组。
- protobuf的消息升级原则：不要更改任何已有的字段的数值标识；不能移除已经存在的required字段，optional和repeated类型的字段可以被移除，但要保留标号不能被重用。新添加的字段必须是optional或repeated。因为旧版本程序无法读取或写入新增的required限定符的字段。
- 编译器为每一个消息类型生成了一个.java文件，以及一个特殊的Builder类（该类是用来创建消息类接口的）。如：UserProto.User.Builder builder = UserProto.User.newBuilder();builder.build()；
- Netty中的使用：ProtobufVarint32FrameDecoder 是用于处理半包消息的解码类；ProtobufDecoder(UserProto.User.getDefaultInstance())这是创建的UserProto.java文件中的解码类；ProtobufVarint32LengthFieldPrepender 对protobuf协议的消息头上加上一个长度为32的整形字段，用于标志这个消息的长度的类；ProtobufEncoder 是编码类
- 将StringBuilder转换为ByteBuf类型：copiedBuffer()方法

#### 6.Netty的零拷贝实现？

- Netty的接收和发送ByteBuffer采用DIRECT BUFFERS，使用堆外直接内存进行Socket读写，不需要进行字节缓冲区的二次拷贝。堆内存多了一次内存拷贝，JVM会将堆内存Buffer拷贝一份到直接内存中，然后才写入Socket中。ByteBuffer由ChannelConfig分配，而ChannelConfig创建ByteBufAllocator默认使用Direct Buffer
- CompositeByteBuf 类可以将多个 ByteBuf 合并为一个逻辑上的 ByteBuf, 避免了传统通过内存拷贝的方式将几个小Buffer合并成一个大的Buffer。addComponents方法将 header 与 body 合并为一个逻辑上的 ByteBuf, 这两个 ByteBuf 在CompositeByteBuf 内部都是单独存在的, CompositeByteBuf 只是逻辑上是一个整体
- 通过 FileRegion 包装的FileChannel.tranferTo方法 实现文件传输, 可以直接将文件缓冲区的数据发送到目标 Channel，避免了传统通过循环write方式导致的内存拷贝问题。
- 通过 wrap方法, 我们可以将 byte[] 数组、ByteBuf、ByteBuffer等包装成一个 Netty ByteBuf 对象, 进而避免了拷贝操作。
- Selector BUG：若Selector的轮询结果为空，也没有wakeup或新消息处理，则发生空轮询，CPU使用率100%，
- Netty的解决办法：对Selector的select操作周期进行统计，每完成一次空的select操作进行一次计数，若在某个周期内连续发生N次空轮询，则触发了epoll死循环bug。重建Selector，判断是否是其他线程发起的重建请求，若不是则将原SocketChannel从旧的Selector上去除注册，重新注册到新的Selector上，并将原来的Selector关闭。

#### 7.Netty的高性能表现在哪些方面？

- **心跳**，对服务端：会定时清除闲置会话inactive(netty5)，对客户端:用来检测会话是否断开，是否重来，检测网络延迟，其中idleStateHandler类 用来检测会话状态
- **串行无锁化设计**，即消息的处理尽可能在同一个线程内完成，期间不进行线程切换，这样就避免了多线程竞争和同步锁。表面上看，串行化设计似乎CPU利用率不高，并发程度不够。但是，通过调整NIO线程池的线程参数，可以同时启动多个串行化的线程并行运行，这种局部无锁化的串行线程设计相比一个队列-多个工作线程模型性能更优。
- **可靠性**，链路有效性检测：链路空闲检测机制，读/写空闲超时机制；内存保护机制：通过内存池重用ByteBuf;ByteBuf的解码保护；优雅停机：不再接收新消息、退出前的预处理操作、资源的释放操作。
- **Netty安全性**：支持的安全协议：SSL V2和V3，TLS，SSL单向认证、双向认证和第三方CA认证。
- **高效并发编程的体现**：volatile的大量、正确使用；CAS和原子类的广泛使用；线程安全容器的使用；通过读写锁提升并发性能。IO通信性能三原则：传输（AIO）、协议（Http）、线程（主从多线程）
- **流量整型**的作用（变压器）：防止由于上下游网元性能不均衡导致下游网元被压垮，业务流中断；防止由于通信模块接受消息过快，后端业务线程处理不及时导致撑死问题。
- **TCP参数配置**：SO_RCVBUF和SO_SNDBUF：通常建议值为128K或者256K；SO_TCPNODELAY：NAGLE算法通过将缓冲区内的小封包自动相连，组成较大的封包，阻止大量小封包的发送阻塞网络，从而提高网络应用效率。但是对于时延敏感的应用场景需要关闭该优化算法；

#### 8.客户端关闭的时候会抛出异常，死循环

解决方案


	int read = channel.read(buffer);
	if(read > 0){
	byte[] data = buffer.array();
	String msg = new String(data).trim();
	System.out.println(“服务端收到信息：” + msg);
		//回写数据
		ByteBuffer outBuffer = ByteBuffer.wrap("好的".getBytes());
		channel.write(outBuffer);// 将消息回送给客户端
	}else{
		System.out.println("客户端关闭");
		key.cancel();
	}


#### 9、selector.select();阻塞，那为什么说nio是非阻塞的IO？

```
selector.select()
selector.select(1000);不阻塞 会继续往下执行程序
selector.wakeup();也可以唤醒selector 继续往下执行程序
selector.selectNow();也可以立马返回程序 死循环
```

#### 10、SelectionKey.OP_WRITE是代表什么意思

OP_WRITE表示底层缓冲区是否有空间，是则响应返还true
netty版本大致版本分为 netty3.x 和 netty4.x、netty5.x

netty可以运用在那些领域？

#### 11.分布式进程通信

例如: hadoop、dubbo、akka等具有分布式功能的框架，底层RPC通信都是基于netty实现的，这些框架使用的版本通常都还在用netty3.x

#### 12、游戏服务器开发

最新的游戏服务器有部分公司可能已经开始采用netty4.x 或 netty5.x

#### 13、netty服务端hello world案例

```
SimpleChannelHandler 处理消息接收和写
{
messageReceived接收消息

channelConnected新连接，通常用来检测IP是否是黑名单

channelDisconnected链接关闭，可以再用户断线的时候清楚用户的缓存数据等
}
```

#### 14、netty客户端hello world案例

channelDisconnected与channelClosed的区别？

channelDisconnected只有在连接建立后断开才会调用
channelClosed无论连接是否成功都会调用关闭资源

#### 15.一个NIO是不是只能有一个selector？

不是，一个系统可以有多个selector

#### 16.selector是不是只能注册一个ServerSocketChannel？

不是，可以注册多个
一个thread + 队列 == 一个单线程线程池 =====> 线程安全的，任务是线性串行执行的
线程安全，不会产生阻塞效应 ，使用对象组
线程不安全，会产生阻塞效应， 使用对象池

#### 17.心跳其实就是一个普通的请求，特点数据简单，业务也简单

心跳对于服务端来说，定时清除闲置会话inactive(netty5) channelclose(netty3)
心跳对客户端来说，用来检测会话是否断开，是否重连！ 用来检测网络延时！

#### 18.Netty IdleStateHandler出现问题-我是否以错误的方式对其进行了测试？

我有一个玩具Netty服务器，并且尝试在客户端的通道未发生任何事件时向其发送心跳消息。我正在通过telnet到服务器，编写消息然后不发送任何内容来对此进行测试，但是我听不到任何声音！

安慰：

```
>>telnet localhost 6969
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
>>foo
Did you say 'foo'?
```

MyPipelineFactory.java

```
public class MyPipelineFactory implements ChannelPipelineFactory {
    private final Timer timer;
    private static final ChannelHandler stringDecoder = new StringDecoder();
    private static final ChannelHandler stringEncoder = new StringEncoder();
    private final ChannelHandler idleStateHandler;

    public MyPipelineFactory(Timer t) {
        this.timer = t;
        this.idleStateHandler = new IdleStateHandler(timer, 5, 5, 5);
    }

    public ChannelPipeline getPipeline() {
        // create default pipeline from static method
        ChannelPipeline pipeline = Channels.pipeline();
        pipeline.addLast("idleStateHandler", this.idleStateHandler); // heartbeat
        pipeline.addLast("framer", new DelimiterBasedFrameDecoder(1024, Delimiters.lineDelimiter()));
        //pipeline.addLast("frameDecoder", new LengthFieldBasedFrameDecoder(1024,0,1)); // get header from message
        pipeline.addLast("stringDecoder", stringDecoder);
        pipeline.addLast("stringEncoder", stringEncoder);
        pipeline.addLast("ServerHandler", new ServerHandler()); // goes at the end

        return pipeline;
    }
}
```

HeartbeatHandler.java

```
public class HeartbeatHandler extends IdleStateAwareChannelHandler {

    @Override
    public void channelIdle(ChannelHandlerContext ctx, IdleStateEvent e) {
        if (e.getState() == IdleState.READER_IDLE) {
            System.out.println("Reader idle, closing channel");
            //e.getChannel().close();
            e.getChannel().write("heartbeat-reader_idle");
        }
        else if (e.getState() == IdleState.WRITER_IDLE) {
            System.out.println("Writer idle, sending heartbeat");
            e.getChannel().write("heartbeat-writer_idle");
        }
        else if (e.getState() == IdleState.ALL_IDLE) {
            System.out.println("All idle, sending heartbeat");
            e.getChannel().write("heartbeat-all_idle");
        }
    }
}
```

------

固定：

我忘记了HeartbeatHandler，它需要IdleStateHandler（这部分对我来说并不明显）。这样可行。

```
public class MyPipelineFactory implements ChannelPipelineFactory {
    private final Timer timer;
    private static final ChannelHandler stringDecoder = new StringDecoder();
    private static final ChannelHandler stringEncoder = new StringEncoder();
    private final ChannelHandler idleStateHandler;
    private final ChannelHandler heartbeatHandler;

    public MyPipelineFactory(Timer t) {
        this.timer = t;
        this.idleStateHandler = new IdleStateHandler(timer, 5, 5, 5);
        this.heartbeatHandler = new HeartbeatHandler();
    }

    public ChannelPipeline getPipeline() {
        // create default pipeline from static method
        ChannelPipeline pipeline = Channels.pipeline();
        pipeline.addLast("idleStateHandler", this.idleStateHandler);
        pipeline.addLast("heartbeatHandler", this.heartbeatHandler); // heartbeat
        pipeline.addLast("framer", new DelimiterBasedFrameDecoder(1024, Delimiters.lineDelimiter()));
        //pipeline.addLast("frameDecoder", new LengthFieldBasedFrameDecoder(1024,0,1)); // get header from message
        pipeline.addLast("stringDecoder", stringDecoder);
        pipeline.addLast("stringEncoder", stringEncoder);
        pipeline.addLast("ServerHandler", new ServerHandler()); // goes at the end

        return pipeline;
    }
}
```

#### 19.Netty如何使用线程池？

您能解释一下Netty如何使用线程池工作吗？我是否正确理解，线程池有两种：老板线程和工人线程。老板用于执行I /
O，而worker用于调用用户回调（messageReceived）来处理数据？



这是来自NioServerSocketChannelFactory文档

> 一个ServerSocketChannelFactory，它创建一个基于NIO的服务器端ServerSocketChannel。它利用NIO引入的非阻塞I
> / O模式来有效地服务许多并发连接。
>
> 线程如何工作
> NioServerSocketChannelFactory中有两种类型的线程：一个是老板线程，另一个是工作线程。
>
> 老板线程
>
> 每个绑定的ServerSocketChannel都有自己的老板线程。例如，如果您打开了两个服务器端口（例如80和443），则将有两个老板线程。Boss线程接受传入的连接，直到未绑定端口。一旦成功接受了连接，老板线程就将接受的Channel传递给NioServerSocketChannelFactory管理的工作线程之一。
>
> 工作线程
> 一个NioServerSocketChannelFactory可以具有一个或多个工作线程。工作线程以非阻塞模式对一个或多个通道执行非阻塞读写。

在Nio模型中，bossThread照顾所有有界套接字（监听套接字），workerThread照顾Accepted-
socket（包括IO和调用messageMethod等接收事件的方法）。

#### 20.Netty IllegalReferenceCountException

尽管我的业务逻辑没有问题，但事实证明我没有使用Netty
`ByteBuf`。更新要使用的测试代码后`ByteBuf`，我遇到了[IllegalReferenceCountException](http://netty.io/wiki/reference-counted-objects.html)的无尽循环。我承认对Netty还是陌生的，但这并不能证明在手动分配和释放资源的日子里回来。创建GC就是为了避免这种混乱。迪斯科，有人吗？那贝尔底呢？

```
public class StringDecoder extends AbstractDecoder<String> {
    private static final IntPredicate NEWLINE_DELIMITER = b -> b == '\n' || b == '\r';

    @Override
    public Flux<String> decode(Publisher<DataBuffer> publisher, ResolvableType elementType, MimeType mimeType, Map<String, Object> hints) {
        return Flux.from(publisher)
            .scan(Tuples.<Flux<DataBuffer>, Optional<DataBuffer>>of(Flux.empty(), Optional.empty()),
                    (acc, buffer) -> {
                        List<DataBuffer> results = new ArrayList<>();
                        int startIdx = 0, endIdx = 0, limit = buffer.readableByteCount();
                        Optional<DataBuffer> incomplete = acc.getT2();

                        while (startIdx < limit && endIdx != -1) {
                            endIdx = buffer.indexOf(NEWLINE_DELIMITER, startIdx);
                            int length = (endIdx == -1 ? limit : endIdx) - startIdx;
                            DataBuffer slice = buffer.slice(startIdx, length);
                            DataBuffer tmp = incomplete.map(b -> b.write(slice))
                                    .orElse(buffer.factory().allocateBuffer(length).write(slice));
                            tmp = DataBufferUtils.retain(tmp);

                            if (endIdx != -1) {
                                startIdx = endIdx + 1;
                                results.add(tmp);
                                incomplete = Optional.empty();
                            } else {
                                incomplete = Optional.of(tmp);
                            }
                        }
                        releaseBuffer(buffer);

                        return Tuples.of(Flux.fromIterable(results), incomplete);
                    })
            .flatMap(t -> {
                t.getT2().ifPresent(this::releaseBuffer);
                return t.getT1();
            })
            .map(buffer -> {
                // charset resolution should in general use supplied mimeType
                String s = UTF_8.decode(buffer.asByteBuffer()).toString();
                releaseBuffer(buffer);

                return s;
            })
            .log();
    }

    private void releaseBuffer(DataBuffer buffer) {
        boolean release = DataBufferUtils.release(buffer);
        if (release) {
            System.out.println("Buffer was released.");
        }
    }
}

public class StringDecoderTest {
    private StringDecoder stringDecoder = new StringDecoder();
    DataBufferFactory dataBufferFactory = new NettyDataBufferFactory(UnpooledByteBufAllocator.DEFAULT);

    @Test
    public void testDecode() {
        Flux<DataBuffer> pub = Flux.just("abc\n", "abc", "def\n", "abc", "def\nxyz\n", "abc", "def", "xyz\n")
                .map(s -> dataBufferFactory.wrap(s.getBytes(UTF_8)));

        StepVerifier.create(stringDecoder.decode(pub, null, null, null))
                .expectNext("abc", "abcdef", "abcdef", "xyz", "abcdefxyz")
                .verifyComplete();
    }
}
```

我不断得到：

```
[ERROR] (main) onError(io.netty.util.IllegalReferenceCountException: refCnt: 0)
[ERROR] (main)  - io.netty.util.IllegalReferenceCountException: refCnt: 0
io.netty.util.IllegalReferenceCountException: refCnt: 0
    at io.netty.buffer.AbstractByteBuf.ensureAccessible(AbstractByteBuf.java:1415)
    at io.netty.buffer.UnpooledHeapByteBuf.nioBuffer(UnpooledHeapByteBuf.java:314)
    at io.netty.buffer.AbstractUnpooledSlicedByteBuf.nioBuffer(AbstractUnpooledSlicedByteBuf.java:434)
    at io.netty.buffer.CompositeByteBuf.nioBuffers(CompositeByteBuf.java:1496)
    at io.netty.buffer.CompositeByteBuf.nioBuffer(CompositeByteBuf.java:1468)
    at io.netty.buffer.AbstractByteBuf.nioBuffer(AbstractByteBuf.java:1205)
    at org.springframework.core.io.buffer.NettyDataBuffer.asByteBuffer(NettyDataBuffer.java:234)
    at org.abhijitsarkar.java.StringDecoder.lambda$decode$4(StringDecoder.java:61)
```

工作代码：

```
public class StringDecoder extends AbstractDecoder<String> {
    private static final IntPredicate NEWLINE_DELIMITER = b -> b == '\n' || b == '\r';

    @Override
    public Flux<String> decode(Publisher<DataBuffer> publisher, ResolvableType elementType, MimeType mimeType, Map<String, Object> hints) {
        DataBuffer incomplete = new NettyDataBufferFactory(UnpooledByteBufAllocator.DEFAULT).allocateBuffer(0);

        return Flux.from(publisher)
                .scan(Tuples.<Flux<DataBuffer>, DataBuffer>of(Flux.empty(), retain(incomplete)),
                      (acc, buffer) -> {
                          List<DataBuffer> results = new ArrayList<>();
                          int startIdx = 0, endIdx = 0, limit = buffer.readableByteCount();

                          while (startIdx < limit && endIdx != -1) {
                              endIdx = buffer.indexOf(NEWLINE_DELIMITER, startIdx);
                              int length = (endIdx == -1 ? limit : endIdx) - startIdx;

                              DataBuffer slice = buffer.slice(startIdx, length);
                              byte[] slice1 = new byte[length];
                              slice.read(slice1, 0, slice1.length);

                              if (endIdx != -1) {
                                  byte[] slice2 = new byte[incomplete.readableByteCount()];
                                  incomplete.read(slice2, 0, slice2.length);
                                  // call retain to match release during decoding to string later
                                  results.add(retain(
                                          incomplete.factory().allocateBuffer()
                                                  .write(slice2)
                                                  .write(slice1)
                                  ));
                                  startIdx = endIdx + 1;
                              } else {
                                  incomplete.write(slice1);
                              }
                          }

                          return Tuples.of(Flux.fromIterable(results), incomplete);
                      })
                .flatMap(Tuple2::getT1)
                .map(buffer -> {
                    // charset resolution should in general use supplied mimeType
                    String s = UTF_8.decode(buffer.asByteBuffer()).toString();

                    return s;
                })
                .doOnTerminate(() -> release(incomplete))
                .log();
    }
}
```

该代码可能更[简洁](https://jira.spring.io/browse/SPR-16351)一些，但是适用于Spring bug
[SPR-16351](https://jira.spring.io/browse/SPR-16351)。

#### 21.Java Netty负载测试问题

我编写了使用文本协议接受连接和轰炸消息（〜100字节）的服务器，并且我的实现能够与3rt客户端发送约400K /
sec的回送消息。我为此任务选择了Netty，即SUSE 11 RealTime，JRockit
RTS。但是，当我开始基于Netty开发自己的客户端时，吞吐量却急剧下降（从400K msg / sec降低到1.3K msg /
sec）。客户端的代码非常简单。能否请您提供建议或示例，说明如何编写更有效的客户。实际上，我实际上更关心延迟，但是从吞吐量测试开始，我认为在环回中以1.5Kmsg
/ sec的速度正常是不正常的。PS客户端的目的只是接收来自服务器的消息，很少发送心跳信号。



```
Client.java

public class Client {

private static ClientBootstrap bootstrap;
private static Channel connector;
public static boolean start()
{
    ChannelFactory factory =
        new NioClientSocketChannelFactory(
                Executors.newCachedThreadPool(),
                Executors.newCachedThreadPool());
    ExecutionHandler executionHandler = new ExecutionHandler( new OrderedMemoryAwareThreadPoolExecutor(16, 1048576, 1048576));

    bootstrap = new ClientBootstrap(factory);

    bootstrap.setPipelineFactory( new ClientPipelineFactory() );

    bootstrap.setOption("tcpNoDelay", true);
    bootstrap.setOption("keepAlive", true);
    bootstrap.setOption("receiveBufferSize", 1048576);
    ChannelFuture future = bootstrap
            .connect(new InetSocketAddress("localhost", 9013));
    if (!future.awaitUninterruptibly().isSuccess()) {
        System.out.println("--- CLIENT - Failed to connect to server at " +
                           "localhost:9013.");
        bootstrap.releaseExternalResources();
        return false;
    }

    connector = future.getChannel();

    return connector.isConnected();
}
public static void main( String[] args )
{
    boolean started = start();
    if ( started )
        System.out.println( "Client connected to the server" );
}

}

ClientPipelineFactory.java

public class ClientPipelineFactory  implements ChannelPipelineFactory{

private final ExecutionHandler executionHandler;
public ClientPipelineFactory( ExecutionHandler executionHandle )
{
    this.executionHandler = executionHandle;
}
@Override
public ChannelPipeline getPipeline() throws Exception {
    ChannelPipeline pipeline = pipeline();
    pipeline.addLast("framer", new DelimiterBasedFrameDecoder(
              1024, Delimiters.lineDelimiter()));
    pipeline.addLast( "executor", executionHandler);
    pipeline.addLast("handler", new MessageHandler() );

    return pipeline;
}

}

MessageHandler.java
public class MessageHandler extends SimpleChannelHandler{

long max_msg = 10000;
long cur_msg = 0;
long startTime = System.nanoTime();
@Override
public void messageReceived(ChannelHandlerContext ctx, MessageEvent e) {
    cur_msg++;

    if ( cur_msg == max_msg )
    {
        System.out.println( "Throughput (msg/sec) : " + max_msg* NANOS_IN_SEC/(     System.nanoTime() - startTime )   );
        cur_msg = 0;
        startTime = System.nanoTime();
    }
}

@Override
public void exceptionCaught(ChannelHandlerContext ctx, ExceptionEvent e) {
    e.getCause().printStackTrace();
    e.getChannel().close();
}

}
```

更新。在服务器端，存在一个定期线程，该线程写入已接受的客户端通道。而且该频道很快就无法写入。更新N2。在管道中添加了OrderedMemoryAwareExecutor，但是吞吐量仍然很低（大约4k
msg / sec）

固定。我将执行程序放在整个管道堆栈的前面，结果成功了！



如果服务器正在发送固定大小（〜100字节）的消息，则可以将ReceiveBufferSizePredictor设置为客户端引导程序，这将优化读取

```
bootstrap.setOption("receiveBufferSizePredictorFactory",
            new AdaptiveReceiveBufferSizePredictorFactory(MIN_PACKET_SIZE, INITIAL_PACKET_SIZE, MAX_PACKET_SIZE));
```

根据您发布的代码段：客户端的nio工作线程正在做管道中的所有事情，因此它将忙于解码和执行消息处理程序。您必须添加一个执行处理程序。

您已经说过，通道从服务器端变得不可写，因此您可能必须在服务器引导程序中调整水印大小。您可以定期监视写缓冲区大小（写队列大小），并确保由于消息无法写到网络而使通道变得不可写。可以通过以下类似的util类来完成。

```
package org.jboss.netty.channel.socket.nio;

import org.jboss.netty.channel.Channel;

public final class NioChannelUtil {
  public static long getWriteTaskQueueCount(Channel channel) {
    NioSocketChannel nioChannel = (NioSocketChannel) channel;
    return nioChannel.writeBufferSize.get();
  }
}
```

#### 22.

#### 参考

https://www.cnblogs.com/xuxinstyle/p/9872915.html

https://blog.csdn.net/qq_38772518/article/details/105834573

http://www.mianshigee.com/