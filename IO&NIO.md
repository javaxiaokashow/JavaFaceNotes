## IO&NIO



![image-20200414192748596](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200414192748596.png)

#### 1.**什么是IO流？**

它是一种数据的流从源头流到目的地。比如文件拷贝，输入流和输出流都包括了。输入流从文件中读取数据存储到进程(process)中，输出流从进程中读取数据然后写入到目标文件。

#### 2.java中有几种类型的流？

按照单位大小：字符流、字节流。按照流的方向：输出流、输入流。

#### 3.字节流和字符流哪个好？怎么选择？

1. 缓大多数情况下使用字节流会更好，因为字节流是字符流的包装，而大多数时候 IO 操作都是直接操作磁盘文件，所以这些流在传输时都是以字节的方式进行的（图片等都是按字节存储的）
2. 如果对于操作需要通过 IO 在内存中频繁处理字符串的情况使用字符流会好些，因为字符流具备缓冲区，提高了性能

#### 4.读取数据量大的文件时，速度会很慢，如何选择流？

字节流时，选择BufferedInputStream和BufferedOutputStream。
字符流时，选择BufferedReader 和 BufferedWriter

#### 5. IO模型有几种？

阻塞IO、非阻塞IO、多路复用IO、信号驱动IO以及异步IO。

#### 6.阻塞IO**（blocking IO）**

 应用程序调用一个IO函数，导致应用程序阻塞，如果数据已经准备好，从内核拷贝到用户空间，否则一直等待下去。一个典型的读操作流程大致如下图，当用户进程调用recvfrom这个系统调用时，kernel就开始了IO的第一个阶段：准备数据，就是数据被拷贝到内核缓冲区中的一个过程（很多网络IO数据不会那么快到达，如没收一个完整的UDP包），等数据到操作系统内核缓冲区了，就到了第二阶段：将数据从内核缓冲区拷贝到用户内存，然后kernel返回结果，用户进程才会解除block状态，重新运行起来。**blocking IO的特点就是在IO执行的两个阶段用户进程都会block住；**

![image-20200416135005801](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200416135005801.png)

#### 7.**非阻塞I/O（nonblocking IO）**

 非阻塞I/O模型，我们把一个套接口设置为非阻塞就是告诉内核，当所请求的I/O操作无法完成时，不要将进程睡眠，而是返回一个错误。这样我们的I/O操作函数将不断的测试数据是否已经准备好，如果没有准备好，继续测试，直到数据准备好为止。在这个不断测试的过程中，**会大量的占用CPU的时间**。

​    当用户进程发出read操作时，如果kernel中数据还没准备好，那么并不会block用户进程，而是立即返回error，用户进程判断结果是error，就知道数据还没准备好，用户可以再次发read，直到kernel中数据准备好，并且用户再一次发read操作，产生system call，那么kernel 马上将数据拷贝到用户内存，然后返回；所以**nonblocking IO的特点是用户进程需要不断的主动询问kernel数据好了没有。**

　　阻塞IO一个线程只能处理一个IO流事件，要想同时处理多个IO流事件要么多线程要么多进程，这样做效率显然不会高，而非阻塞IO可以一个线程处理多个流事件，只要不停地询所有流事件即可，当然这个方式也不好，当大多数流没数据时，也是会大量浪费CPU资源；为了避免CPU空转，引进代理(select和poll，两种方式相差不大)，代理可以观察多个流I/O事件，空闲时会把当前线程阻塞掉，当有一个或多个I/O事件时，就从阻塞态醒过来，把所有IO流都轮询一遍，于是没有IO事件我们的程序就阻塞在select方法处，即便这样依然存在问题，我们从select出只是知道有IO事件发生，却不知道是哪几个流，还是只能轮询所有流，**epoll**这样的代理就可以把哪个流发生怎样的IO事件通知我们；　

![image-20200416135208788](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200416135208788.png)

#### 8.**I/O多路复用模型(IO multiplexing)**

 I/O多路复用就在于单个进程可以同时处理多个网络连接IO,基本原理就是select，poll，epoll这些个函数会不断轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程，这三个functon会阻塞进程，但和IO阻塞不同，这些函数可以同时阻塞多个IO操作，而且可以同时对多个读操作，写操作IO进行检验，直到有数据到达，才真正调用IO操作函数，调用过程如下图；**所以IO多路复用的特点是通过一种机制一个进程能同时等待多个文件描述符，而这些文件描述符(套接字描述符)其中任意一个进入就绪状态，select函数就可以返回。**

　　IO多路复用的优势在于并发数比较高的IO操作情况，可以同时处理多个连接，和bloking IO一样socket是被阻塞的，只不过在多路复用中socket是被select阻塞，而在阻塞IO中是被socket IO给阻塞。

![image-20200416135308008](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200416135308008.png)

#### 9.信号驱动I/O模型

可以用信号，让内核在描述符就绪时发送SIGIO信号通知我们，通过sigaction系统调用安装一个信号处理函数。该系统调用将立即返回，我们的进程继续工作，也就是说它没有被阻塞。当数据报准备好读取时，内核就为该进程产生一个SIGIO信号。我们随后既可以在信号处理函数中调用recvfrom读取数据报，并通知主循环数据已经准备好待处理。**特点：等待数据报到达期间进程不被阻塞。主循环可以继续执行，只要等待来自信号处理函数的通知：既可以是数据已准备好被处理，也可以是数据报已准备好被读取**

![image-20200416140944940](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200416140944940.png)

#### 10.异步 I/O(asynchronous IO)

异步IO告知内核启动某个操作，并让内核在整个操作(包括将内核数据复制到我们自己的缓冲区)完成后通知我们，调用aio_read（Posix异步I/O函数以aio_或lio_开头）函数，给内核传递描述字、缓冲区指针、缓冲区大小（与read相同的3个参数）、文件偏移以及通知的方式，然后系统立即返回。我们的进程不阻塞于等待I/0操作的完成。当内核将数据拷贝到缓冲区后，再通知应用程序。 

用户进程发起read操作之后，立刻就可以开始去做其它的事。而另一方面，从kernel的角度，当它受到一个asynchronous read之后，首先它会立刻返回，所以不会对用户进程产生任何block。然后，kernel会等待数据准备完成，然后将数据拷贝到用户内存，当这一切都完成之后，kernel会给用户进程发送一个signal，告诉它read操作完成了

![image-20200416141152742](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200416141152742.png)

#### 11.NIO与IO的区别？

 NIO即New IO，这个库是在JDK1.4中才引入的。NIO和IO有相同的作用和目的，但实现方式不同，NIO主要用到的是块，所以NIO的效率要比IO高很多。在Java API中提供了两套NIO，一套是针对标准输入输出NIO，另一套就是网络编程NIO。

![image-20200416143304767](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200416143304767.png)

#### 12.NIO和IO适用场景

 NIO是为弥补传统IO的不足而诞生的，但是尺有所短寸有所长，**NIO也有缺点，因为NIO是面向缓冲区的操作，每一次的数据处理都是对缓冲区进行的，那么就会有一个问题，在数据处理之前必须要判断缓冲区的数据是否完整或者已经读取完毕，如果没有，假设数据只读取了一部分，那么对不完整的数据处理没有任何意义。**所以每次数据处理之前都要检测缓冲区数据。
  那么NIO和IO各适用的场景是什么呢？
  如果需要管理同时打开的成千上万个连接，这些**连接每次只是发送少量的数据**，例如聊天服务器，这时候用NIO处理数据可能是个很好的选择。
  而如果只**有少量的连接**，而这些连接每次要发送大量的数据，这时候传统的IO更合适。使用哪种处理数据，需要在数据的响应等待时间和检查缓冲区数据的时间上作比较来权衡选择。

#### 13.NIO核心组件

channel、buffer、selector

#### 14.什么是channel

一个Channel（通道）代表和某一实体的连接，这个实体可以是文件、网络套接字等。也就是说，通道是Java NIO提供的一座桥梁，用于我们的程序和操作系统底层I/O服务进行交互。

通道是一种很基本很抽象的描述，和不同的I/O服务交互，执行不同的I/O操作，实现不一样，因此具体的有FileChannel、SocketChannel等。

通道使用起来跟Stream比较像，可以读取数据到Buffer中，也可以把Buffer中的数据写入通道。

![image-20200416150623938](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200416150623938.png)

当然，也有区别，主要体现在如下两点：

- 一个通道，既可以读又可以写，而一个Stream是单向的（所以分 InputStream 和 OutputStream）
- 通道有非阻塞I/O模式

#### 15.Java NIO中最常用的通道实现?

- FileChannel：读写文件
- DatagramChannel: UDP协议网络通信
- SocketChannel：TCP协议网络通信
- ServerSocketChannel：监听TCP连接

#### 16.Buffer是什么?

NIO中所使用的缓冲区不是一个简单的byte数组，而是封装过的Buffer类，通过它提供的API，我们可以灵活的操纵数据。

与Java基本类型相对应，NIO提供了多种 Buffer 类型，如ByteBuffer、CharBuffer、IntBuffer等，区别就是读写缓冲区时的单位长度不一样（以对应类型的变量为单位进行读写）。

#### 17.核心Buffer实现有哪些？

核心的buffer实现有这些：ByteBuffer、CharBuffer、DoubleBuffer、FloatBuffer、IntBuffer、LongBuffer、ShortBuffer，涵盖了所有的基本数据类型（4类8种，除了Boolean）。也有其他的buffer如MappedByteBuffer。

#### 18.buffer读写数据基本操作

1）、将数据写入buffer
2）、调用buffer.flip()
3）、将数据从buffer中读取出来
4）、调用buffer.clear()或者buffer.compact()

在写buffer的时候，buffer会跟踪写入了多少数据，需要读buffer的时候，需要调用flip()来将buffer从写模式切换成读模式，读模式中只能读取写入的数据，而非整个buffer。
  当数据都读完了，你需要清空buffer以供下次使用，可以有2种方法来操作：调用clear() 或者 调用compact()。
  区别：clear方法清空整个buffer，compact方法只清除你已经读取的数据，未读取的数据会被移到buffer的开头，此时写入数据会从当前数据的末尾开始。

```java
// 创建一个容量为48的ByteBuffer
ByteBuffer buf = ByteBuffer.allocate(48);
// 从channel中读（取数据然后写）入buffer
int bytesRead = inChannel.read(buf); 
// 下面是读取buffer
while (bytesRead != -1) {
    buf.flip();  // 转换buffer为读模式
    System.out.print((char) buf.get()); // 一次读取一个byte
    buf.clear();  //清空buffer准备下一次写入
}

```

#### 19.Selector是什么?

Selector（选择器）是一个特殊的组件，用于采集各个通道的状态（或者说事件）。我们先将通道注册到选择器，并设置好关心的事件，然后就可以通过调用select()方法，静静地等待事件发生。

#### 20.通道可以监听那几个事件？

通道有如下4个事件可供我们监听：

- Accept：有可以接受的连接
- Connect：连接成功
- Read：有数据可读
- Write：可以写入数据了

#### 21.为什么要用Selector？

如果用阻塞I/O，需要多线程（浪费内存），如果用非阻塞I/O，需要不断重试（耗费CPU）。Selector的出现解决了这尴尬的问题，非阻塞模式下，通过Selector，我们的线程只为已就绪的通道工作，不用盲目的重试了。比如，当所有通道都没有数据到达时，也就没有Read事件发生，我们的线程会在select()方法处被挂起，从而让出了CPU资源。

#### 22.Selector处理多Channel图文说明

![image-20200416161430793](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200416161430793.png)



要使用一个Selector，你要先注册这个Selector的Channels。然后你调用Selector的select()方法。这个方法会阻塞，直到它注册的Channels当中有一个准备好了的事件发生了。当select()方法返回的时候，线程可以处理这些事件，如新的连接的到来，数据收到了等。

#### 23.代码示例：如何使用流的基本接口来读写文件内容

```
try {
 
DataInputStream in =
 
    new DataInputStream(

    new BufferedInputStream(

        new FileInputStream("Test.java")

    )
 
);	
 
while ((currentLine = in.readLine()) != null){
 
	System.out.println(currentLine);
 
}
 
} catch (IOException e){
 
	System.err.println("Error: " + e);
 
}
```



#### 参考:

https://www.cnblogs.com/sharing-java/p/10791802.html

https://blog.csdn.net/zengxiantao1994/article/details/88094910

https://www.cnblogs.com/xueSpring/p/9513266.html

https://zhuanlan.zhihu.com/p/163506337


![WechatIMG360](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/common1.png)