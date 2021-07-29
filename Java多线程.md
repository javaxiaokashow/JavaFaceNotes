## Java多线程

#### 1.**什么是进程**?

进程是系统中正在运行的一个程序，程序一旦运行就是进程。

进程可以看成程序执行的一个实例。进程是系统资源分配的独立实体，每个进程都拥有独立的地址空间。一个进程无法访问另一个进程的变量和数据结构，如果想让一个进程访问另一个进程的资源，需要使用进程间通信，比如管道，文件，套接字等。

#### 2.什么是线程？

是操作系统能够进行运算调度的最小单位。它被包含在进程之中，是进程中的实际运作单位。一条线程指的是进程中一个单一顺序的控制流，一个进程中可以并发多个线程，每条线程并行执行不同的任务。

#### 3.线程的实现方式?

1.继承Thread类

2.实现Runnable接口

3.使用Callable和Future

#### 4.**Thread 类中的start() 和 run() 方法有什么区别**?

1.start（）方法来启动线程，真正实现了多线程运行。这时无需等待run方法体代码执行完毕，可以直接继续执行下面的代码；通过调用Thread类的start()方法来启动一个线程， 这时此线程是处于就绪状态， 并没有运行。 然后通过此Thread类调用方法run()来完成其运行操作的， 这里方法run()称为线程体，它包含了要执行的这个线程的内容， Run方法运行结束， 此线程终止。然后CPU再调度其它线程。
2.run（）方法当作普通方法的方式调用。程序还是要顺序执行，要等待run方法体执行完毕后，才可继续执行下面的代码； 程序中只有主线程——这一个线程， 其程序执行路径还是只有一条， 这样就没有达到写线程的目的。

#### 5.线程NEW状态

new创建一个Thread对象时，并没处于执行状态，因为没有调用start方法启动改线程，那么此时的状态就是新建状态。

#### 6.线程RUNNABLE状态

线程对象通过start方法进入runnable状态，启动的线程不一定会立即得到执行，线程的运行与否要看cpu的调度，我们把这个中间状态叫可执行状态（RUNNABLE)。

#### 7.线程的RUNNING状态

一旦cpu通过轮询货其他方式从任务可以执行队列中选中了线程，此时它才能真正的执行自己的逻辑代码。

#### 8.线程的BLOCKED状态

线程正在等待获取锁。

- 进入BLOCKED状态，比如调用了sleep,或者wait方法
- 进行某个阻塞的io操作，比如因网络数据的读写进入BLOCKED状态
- 获取某个锁资源，从而加入到该锁的阻塞队列中而进入BLOCKED状态

#### 9.线程的TERMINATED状态

TERMINATED是一个线程的最终状态，在该状态下线程不会再切换到其他任何状态了，代表整个生命周期都结束了。

下面几种情况会进入TERMINATED状态:

- 线程运行正常结束，结束生命周期
- 线程运行出错意外结束
- JVM Crash 导致所有的线程都结束

#### 10.线程状态转化图

![image-20200501131131886](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200501131131886.png)

#### 11.i——————与System.out.println()的异常

示例代码:

```java
public class XkThread extends Thread {

    private int i = 5;

    @Override
    public void run() {
       System.out.println("i=" + (i——————) + " threadName=" + Thread.currentThread().getName());
    }

    public static void main(String[] args) {
        XkThread xk = new XkThread();
        Thread t1 = new Thread(xk);
        Thread t2 = new Thread(xk);
        Thread t3 = new Thread(xk);
        Thread t4 = new Thread(xk);
        Thread t5 = new Thread(xk);

        t1.start();
        t2.start();
        t3.start();
        t4.start();
        t5.start();
    }
}

```

结果:

```sqlite
i=5 threadName=Thread-1
i=2 threadName=Thread-5
i=5 threadName=Thread-2
i=4 threadName=Thread-3
i=3 threadName=Thread-4
```

虽然println()方法在内部是同步的，但i——————的操作却是在进入println()之前发生的，所以有发生非线程安全的概率。

 println()源码:

```java
    public void println(String x) {
        synchronized (this) {
            print(x);
            newLine();
        }
    }
```

#### 12.如何知道代码段被哪个线程调用？

```java
   System.out.println(Thread.currentThread().getName());
```

#### 13.线程活动状态？

```java
public class XKThread extends Thread {

    @Override
    public void run() {
        System.out.println("run run run is "  + this.isAlive() );
    }

    public static void main(String[] args) {
        XKThread xk = new XKThread();
        System.out.println("begin ——— " + xk.isAlive());
        xk.start();
        System.out.println("end ————— " + xk.isAlive());

    }
}

```

#### 14.sleep()方法

方法sleep()的作用是在指定的毫秒数内让当前的“正在执行的线程”休眠（暂停执行）。

#### 15.如何优雅的设置睡眠时间?

jdk1.5 后，引入了一个枚举TimeUnit,对sleep方法提供了很好的封装。

比如要表达2小时22分55秒899毫秒。

```java
Thread.sleep(8575899L);
TimeUnit.HOURS.sleep(3);
TimeUnit.MINUTES.sleep(22);
TimeUnit.SECONDS.sleep(55);
TimeUnit.MILLISECONDS.sleep(899);
```

可以看到表达的含义更清晰，更优雅。

#### 16.停止线程

run方法执行完成，自然终止。

stop()方法，suspend()以及resume()都是过期作废方法，使用它们结果不可预期。

大多数停止一个线程的操作使用Thread.interrupt()等于说给线程打一个停止的标记, 此方法不回去终止一个正在运行的线程，需要加入一个判断才能可以完成线程的停止。

#### 17.interrupted 和 isInterrupted

interrupted :  判断当前线程是否已经中断,会清除状态。

isInterrupted ：判断线程是否已经中断，不会清除状态。

#### 18.yield

放弃当前cpu资源，将它让给其他的任务占用cpu执行时间。但放弃的时间不确定，有可能刚刚放弃，马上又获得cpu时间片。

测试代码:(cpu独占时间片)

```java
public class XKThread extends Thread {

    @Override
    public void run() {
        long beginTime = System.currentTimeMillis();
        int count = 0;
        for (int i = 0; i < 50000000; i++) {
            count = count + (i + 1);
        }
        long endTime = System.currentTimeMillis();
        System.out.println("用时 = " + (endTime - beginTime) + " 毫秒! ");
    }

    public static void main(String[] args) {
        XKThread xkThread = new XKThread();
        xkThread.start();
    }

}
```

结果：

```sqlite
用时 = 20 毫秒! 
```

加入yield，再来测试。(cpu让给其他资源导致速度变慢)

```java
public class XKThread extends Thread {

    @Override
    public void run() {
        long beginTime = System.currentTimeMillis();
        int count = 0;
        for (int i = 0; i < 50000000; i++) {
            Thread.yield();
            count = count + (i + 1);
        }
        long endTime = System.currentTimeMillis();
        System.out.println("用时 = " + (endTime - beginTime) + " 毫秒! ");
    }

    public static void main(String[] args) {
        XKThread xkThread = new XKThread();
        xkThread.start();
    }

}

```

结果:

```sqlite
用时 = 38424 毫秒! 
```

#### 19.线程的优先级

在操作系统中，线程可以划分优先级，优先级较高的线程得到cpu资源比较多，也就是cpu有限执行优先级较高的线程对象中的任务，但是不能保证一定优先级高，就先执行。

Java的优先级分为1～10个等级，数字越大优先级越高，默认优先级大小为5。超出范围则抛出：java.lang.IllegalArgumentException。

#### 20.优先级继承特性

线程的优先级具有继承性，比如a线程启动b线程，b线程与a优先级是一样的。

#### 21.谁跑的更快？

设置优先级高低两个线程，累加数字，看谁跑的快，上代码。

```java
public class Run extends Thread{

    public static void main(String[] args) {
        try {
            ThreadLow low = new ThreadLow();
            low.setPriority(2);
            low.start();

            ThreadHigh high = new ThreadHigh();
            high.setPriority(8);
            high.start();

            Thread.sleep(2000);
            low.stop();
            high.stop();
            System.out.println("low  = " + low.getCount());
            System.out.println("high = " + high.getCount());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

    }

}

class ThreadHigh extends Thread {
    private int count = 0;

    public int getCount() {
        return count;
    }


    @Override
    public void run() {
        while (true) {
            count++;
        }
    }
}

class ThreadLow extends Thread {
    private int count = 0;

    public int getCount() {
        return count;
    }


    @Override
    public void run() {
        while (true) {
            count++;
        }
    }
}
```

结果:

```sqlite
low  = 1193854568
high = 1204372373

```

#### 22.线程种类

Java线程有两种，一种是用户线程，一种是守护线程。

#### 23.守护线程的特点

守护线程是一个比较特殊的线程，主要被用做程序中后台调度以及支持性工作。当Java虚拟机中不存在非守护线程时，守护线程才会随着JVM一同结束工作。

#### 24.Java中典型的守护线程

GC（垃圾回收器）

#### 25.如何设置守护线程

Thread.setDaemon(true)

PS:Daemon属性需要再启动线程之前设置，不能再启动后设置。

#### 25.Java虚拟机退出时Daemon线程中的finally块一定会执行？

Java虚拟机退出时Daemon线程中的finally块并不一定会执行。

代码示例:

```java
public class XKDaemon {
    public static void main(String[] args) {
        Thread thread = new Thread(new DaemonRunner(),"xkDaemonRunner");
        thread.setDaemon(true);
        thread.start();

    }

    static class DaemonRunner implements Runnable {

        @Override
        public void run() {
            try {
                SleepUtils.sleep(10);
            } finally {
                System.out.println("Java小咖秀 daemonThread finally run …");
            }

        }
    }
}
```

结果：

```

```

 没有任何的输出，说明没有执行finally。

#### 26.设置线程上下文类加载器

​    获取线程上下文类加载器

```java
public ClassLoader getContextClassLoader() 
```

​    设置线程类加载器（可以打破Java类加载器的父类委托机制）

```java
public void setContextClassLoader(ClassLoader cl)
```

#### 27.join

join是指把指定的线程加入到当前线程，比如join某个线程a,会让当前线程b进入等待,直到a的生命周期结束，此期间b线程是处于blocked状态。

#### 28.什么是synchronized?

synchronized关键字可以时间一个简单的策略来防止线程干扰和内存一致性错误，如果一个对象是对多个线程可见的，那么对该对想的所有读写都将通过同步的方式来进行。

#### 29.synchronized包括哪两个jvm重要的指令？

monitor enter 和 monitor exit

#### 30.synchronized关键字用法?

可以用于对代码块或方法的修饰

#### 31.synchronized锁的是什么?

普通同步方法 —————> 锁的是当前实力对象。

静态同步方法—————> 锁的是当前类的Class对象。

同步方法快 —————> 锁的是synchonized括号里配置的对象。

#### 32.Java对象头

synchronized用的锁是存在Java对象头里的。对象如果是数组类型，虚拟机用3个字宽(Word)存储对象头，如果对象是非数组类型，用2字宽存储对象头。

Tips:32位虚拟机中一个字宽等于4字节。

#### 33.Java对象头长度

![image-20200418215447539](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200418215447539.png)

#### 34.Java对象头的存储结构

 32位JVM的Mark Word 默认存储结构

![image-20200418220122794](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200418220122794.png)

#### 35.Mark Word的状态变化

Mark Word 存储的数据会随着锁标志为的变化而变化。

![image-20200418220322880](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200418220322880.png)

64位虚拟机下，Mark Word是64bit大小的

![image-20200418224055558](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200418224055558.png)

#### 36.锁的升降级规则

Java SE 1.6 为了提高锁的性能。引入了“偏向锁”和轻量级锁“。

Java SE 1.6 中锁有4种状态。级别从低到高依次是：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态。

锁只能升级不能降级。

#### 37.偏向锁

大多数情况，锁不仅不存在多线程竞争，而且总由同一线程多次获得。当一个线程访问同步块并获取锁时，会在对象头和栈帧中记录存储锁偏向的线程ID,以后该线程在进入和退出同步块时不需要进行 cas操作来加锁和解锁，只需测试一下对象头 Mark Word里是否存储着指向当前线程的偏向锁。如果测试成功，表示线程已经获得了锁，如果失败，则需要测试下Mark Word中偏向锁的标示是否已经设置成1（表示当前时偏向锁),如果没有设置，则使用cas竞争锁，如果设置了，则尝试使用cas将对象头的偏向锁只想当前线程。

#### 38.关闭偏向锁延迟

java6和7中默认启用，但是会在程序启动几秒后才激活，如果需要关闭延迟，

-XX:BiasedLockingStartupDelay=0。

#### 39.如何关闭偏向锁

JVM参数关闭偏向锁:-XX:-UseBiasedLocking=false,那么程序默认会进入轻量级锁状态。

Tips:如果你可以确定程序的所有锁通常情况处于竞态，则可以选择关闭。

#### 40.轻量级锁

线程在执行同步块，jvm会现在当前线程的栈帧中创建用于储存锁记录的空间。并将对象头中的Mark Word复制到锁记录中。然后线程尝试使用cas将对象头中的Mark Word替换为之乡锁记录的指针。如果成功，当前线程获得锁，如果失败，表示其他线程竞争锁，当前线程便尝试使用自旋来获取锁。

#### 41.轻量锁的解锁

轻量锁解锁时，会使原子操作cas将 displaced Mark Word 替换回对象头，如果成功则表示没有竞争发生，如果失败，表示存在竞争，此时锁就会膨胀为重量级锁。

#### 42.锁的优缺点对比

![image-20200419110938271](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200419110938271.png)

#### 43.什么是原子操作

不可被中断的一个或一系列操作

#### 44.Java如何实现原子操作

Java中通过锁和循环cas的方式来实现原子操作，JVM的CAS操作利用了处理器提供的CMPXCHG指令来实现的。自旋CAS实现的基本思路就是循环进行CAS操作直到成功为止。

#### 45.CAS实现原子操作的3大问题

ABA问题，循环时间长消耗资源大，只能保证一个共享变量的原子操作

#### 46.什么是ABA问题

问题：

因为cas需要在操作值的时候，检查值有没有变化，如果没有变化则更新，如果一个值原来是A,变成了B,又变成了A,那么使用cas进行检测时会发现发的值没有发生变化，其实是变过的。

解决：

添加版本号，每次更新的时候追加版本号，A-B-A —> 1A-2B-3A。

从jdk1.5开始,Atomic包提供了一个类AtomicStampedReference来解决ABA的问题。

#### 47.CAS循环时间长占用资源大问题

如果jvm能支持处理器提供的pause指令，那么效率会有一定的提升。

一、它可以延迟流水线执行指令(de-pipeline),使cpu不会消耗过多的执行资源，延迟的时间取决于具体实现的版本，有些处理器延迟时间是0。

二、它可以避免在退出循环的时候因内存顺序冲突而引起的cpu流水线被清空，从而提高cpu执行效率。

#### 48.CAS只能保证一个共享变量原子操作

一、对多个共享变量操作时，可以用锁。

二、可以把多个共享变量合并成一个共享变量来操作。比如,x=1,k=a,合并xk=1a，然后用cas操作xk。

Tips:java 1.5开始,jdk提供了AtomicReference类来保证饮用对象之间的原子性，就可以把多个变量放在一个对象来进行cas操作。

#### 49.volatile关键字

volatile 是轻量级的synchronized,它在多处理器开发中保证了共享变量的“可见性“。

Java语言规范第3版对volatile定义如下，Java允许线程访问共享变量，为了保证共享变量能准确和一致的更新，线程应该确保排它锁单独获得这个变量。如果一个字段被声明为volatile,Java线程内存模型所有线程看到这个变量的值是一致的。

#### 50.等待/通知机制

一个线程修改了一个对象的值，而另一个线程感知到了变化，然后进行相应的操作。

#### ![WechatIMG360](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/common1.png)51.wait

方法wait()的作用是使当前执行代码的线程进行等待，wait()是Object类通用的方法，该方法用来将当前线程置入“预执行队列”中，并在 wait()所在的代码处停止执行，直到接到通知或中断为止。

在调用wait之前线程需要获得该对象的对象级别的锁。代码体现上，即只能是同步方法或同步代码块内。调用wait()后当前线程释放锁。

#### 52.notify

notify()也是Object类的通用方法，也要在同步方法或同步代码块内调用，该方法用来通知哪些可能灯光该对象的对象锁的其他线程，如果有多个线程等待，则随机挑选出其中一个呈wait状态的线程，对其发出 通知 notify，并让它等待获取该对象的对象锁。

#### 53.notify/notifyAll

notify等于说将等待队列中的一个线程移动到同步队列中，而notifyAll是将等待队列中的所有线程全部移动到同步队列中。

#### 54.等待/通知经典范式

等待

```java
synchronized(obj) {
		while(条件不满足) {
				obj.wait();
		}
		执行对应逻辑
}
```

通知

```java
synchronized(obj) {
	  改变条件
		obj.notifyAll();
}
```

##### 55.ThreadLocal

主要解决每一个线程想绑定自己的值，存放线程的私有数据。

##### 56.ThreadLocal使用

获取当前的线程的值通过get(),设置set(T) 方式来设置值。

```java
public class XKThreadLocal {

    public static ThreadLocal threadLocal = new ThreadLocal();

    public static void main(String[] args) {
        if (threadLocal.get() == null) {
            System.out.println("未设置过值");
            threadLocal.set("Java小咖秀");
        }
        System.out.println(threadLocal.get());
    }

}
```

输出:

```sqlite
未设置过值
Java小咖秀
```

Tips:默认值为null

#### 57.解决get()返回null问题

通过继承重写initialValue()方法即可。

代码实现：

```java
public class ThreadLocalExt extends ThreadLocal{

    static ThreadLocalExt threadLocalExt = new ThreadLocalExt();

    @Override
    protected Object initialValue() {
        return "Java小咖秀";
    }

    public static void main(String[] args) {
        System.out.println(threadLocalExt.get());
    }
}

```

输出结果:

```java
Java小咖秀
```

#### 58.Lock接口

锁可以防止多个线程同时共享资源。Java5前程序是靠synchronized实现锁功能。Java5之后，并发包新增Lock接口来实现锁功能。

#### 59.Lock接口提供 synchronized不具备的主要特性

![image-20200419193439709](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200419193439709.png)

#### 60.重入锁 ReentrantLock

支持重进入的锁，它表示该锁能够支持一个线程对资源的重复加锁。除此之外，该锁的还支持获取锁时的公平和非公平性选择。

#### 61.重进入是什么意思？

重进入是指任意线程在获取到锁之后能够再次获锁而不被锁阻塞。

该特性主要解决以下两个问题：

一、锁需要去识别获取锁的线程是否为当前占据锁的线程，如果是则再次成功获取。

二、所得最终释放。线程重复n次是获取了锁，随后在第n次释放该锁后，其他线程能够获取到该锁。

#### 62.ReentrantLock默认锁？

默认非公平锁

代码为证:

```java
 final boolean nonfairTryAcquire(int acquires) {
            final Thread current = Thread.currentThread();
            int c = getState();
            if (c == 0) {
                if (compareAndSetState(0, acquires)) {
                    setExclusiveOwnerThread(current);
                    return true;
                }
            }
            else if (current == getExclusiveOwnerThread()) {
                int nextc = c + acquires;
                if (nextc < 0) // overflow
                    throw new Error("Maximum lock count exceeded");
                setState(nextc);
                return true;
            }
            return false;
        }
```



#### 63.公平锁和非公平锁的区别

公平性与否针对获取锁来说的，如果一个锁是公平的，那么锁的获取顺序就应该符合请求的绝对时间顺序，也就是FIFO。

#### 64.读写锁

#### 读写锁允许同一时刻多个读线程访问，但是写线程和其他写线程均被阻塞。读写锁维护一个读锁一个写锁，读写分离，并发性得到了提升。

Java中提供读写锁的实现类是ReentrantReadWriteLock。

#### 65.LockSupport工具

定义了一组公共静态方法，提供了最基本的线程阻塞和唤醒功能。

![image-20200419223303672](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200419223303672.png)

#### 66.Condition接口

提供了类似Object监视器方法，与 Lock配合使用实现等待/通知模式。

#### 67.Condition使用

代码示例:

```java
public class XKCondition {
    Lock lock = new ReentrantLock();
    Condition cd = lock.newCondition();

    public void await() throws InterruptedException {
        lock.lock();
        try {
            cd.await();//相当于Object 方法中的wait()
        } finally {
            lock.unlock();
        }
    }

    public void signal() {
        lock.lock();
        try {
            cd.signal(); //相当于Object 方法中的notify()
        } finally {
            lock.unlock();
        }
    }
  
}

```

#### 68.ArrayBlockingQueue?

一个由数据支持的有界阻塞队列，此队列FIFO原则对元素进行排序。队列头部在队列中存在的时间最长，队列尾部存在时间最短。

#### 69.PriorityBlockingQueue?

一个支持优先级排序的无界阻塞队列，但它不会阻塞数据生产者，而只会在没有可消费的数据时，阻塞数据的消费者。

#### 70.DelayQueue?

是一个支持延时获取元素的使用优先级队列的实现的无界阻塞队列。队列中的元素必须实现Delayed接口和 Comparable接口，在创建元素时可以指定多久才能从队列中获取当前元素。

#### 71.Java并发容器，你知道几个？

ConcurrentHashMap、CopyOnWriteArrayList 、CopyOnWriteArraySet 、ConcurrentLinkedQueue、

ConcurrentLinkedDeque、ConcurrentSkipListMap、ConcurrentSkipListSet、ArrayBlockingQueue、

LinkedBlockingQueue、LinkedBlockingDeque、PriorityBlockingQueue、SynchronousQueue、

LinkedTransferQueue、DelayQueue

#### 72.ConcurrentHashMap

并发安全版HashMap,java7中采用分段锁技术来提高并发效率，默认分16段。Java8放弃了分段锁，采用CAS，同时当哈希冲突时，当链表的长度到8时，会转化成红黑树。（如需了解细节，见jdk中代码）

#### 73.ConcurrentLinkedQueue

基于链接节点的无界线程安全队列，它采用先进先出的规则对节点进行排序，当我们添加一个元素的时候，它会添加到队列的尾部，当我们获取一个元素时，它会返回队列头部的元素。它采用cas算法来实现。（如需了解细节，见jdk中代码）

#### 74.什么是阻塞队列？

阻塞队列是一个支持两个附加操作的队列，这两个附加操作支持阻塞的插入和移除方法。

1、支持阻塞的插入方法：当队列满时，队列会阻塞插入元素的线程，直到队列不满。

2、支持阻塞的移除方法：当队列空时，获取元素的线程会等待队列变为非空。

#### 75.阻塞队列常用的应用场景？

常用于生产者和消费者场景，生产者是往队列里添加元素的线程，消费者是从队列里取元素的线程。阻塞队列正好是生产者存放、消费者来获取的容器。

#### 76.Java里的阻塞的队列

```sqlite
ArrayBlockingQueue：    数组结构组成的 |有界阻塞队列
LinkedBlockingQueue：   链表结构组成的|有界阻塞队列
PriorityBlockingQueue:  支持优先级排序|无界阻塞队列
DelayQueue：            优先级队列实现|无界阻塞队列
SynchronousQueue：      不存储元素| 阻塞队列
LinkedTransferQueue：   链表结构组成|无界阻塞队列
LinkedBlockingDeque：   链表结构组成|双向阻塞队列
```

#### 77.Fork/Join

java7提供的一个用于并行执行任务的框架，把一个大任务分割成若干个小任务，最终汇总每个小任务结果的后得到大任务结果的框架。

#### 78.工作窃取算法

是指某个线程从其他队列里窃取任务来执行。当大任务被分割成小任务时，有的线程可能提前完成任务，此时闲着不如去帮其他没完成工作线程。此时可以去其他队列窃取任务，为了减少竞争，通常使用双端队列，被窃取的线程从头部拿，窃取的线程从尾部拿任务执行。

#### 79.工作窃取算法的有缺点

优点：充分利用线程进行并行计算，减少了线程间的竞争。

缺点：有些情况下还是存在竞争，比如双端队列中只有一个任务。这样就消耗了更多资源。

#### 80.Java中原子操作更新基本类型，Atomic包提供了哪几个类?

AtomicBoolean:原子更新布尔类型

AtomicInteger:原子更新整形

AtomicLong:原子更新长整形

#### 81.Java中原子操作更新数组，Atomic包提供了哪几个类?

AtomicIntegerArray:       原子更新整形数据里的元素

AtomicLongArray:          原子更新长整形数组里的元素

AtomicReferenceArray:  原子更新饮用类型数组里的元素

AtomicIntegerArray:      主要提供原子方式更新数组里的整形

#### 82.Java中原子操作更新引用类型，Atomic包提供了哪几个类?

如果原子需要更新多个变量，就需要用引用类型了。

AtomicReference :  原子更新引用类型

AtomicReferenceFieldUpdater: 原子更新引用类型里的字段。

AtomicMarkableReference: 原子更新带有标记位的引用类型。标记位用boolean类型表示，构造方法时AtomicMarkableReference(V initialRef,boolean initialMark)

#### 83.Java中原子操作更新字段类，Atomic包提供了哪几个类?

AtomiceIntegerFieldUpdater:  	原子更新整形字段的更新器

AtomiceLongFieldUpdater:     	原子更新长整形字段的更新器

AtomiceStampedFieldUpdater:   原子更新带有版本号的引用类型，将整数值

#### 84.JDK并发包中提供了哪几个比较常见的处理并发的工具类？

提供并发控制手段: CountDownLatch、CyclicBarrier、Semaphore

线程间数据交换:     Exchanger

#### 85.CountDownLatch

允许一个或多个线程等待其他线程完成操作。

CountDownLatch的构造函数接受一个int类型的参数作为计数器，你想等待n个点完成，就传入n。

两个重要的方法:

countDown() : 调用时，n会减1。

await() : 调用会阻塞当前线程，直到n变成0。

await(long time,TimeUnit unit) : 等待特定时间后，就不会继续阻塞当前线程。

tips:计数器必须大于等于0，当为0时，await就不会阻塞当前线程。

不提供重新初始化或修改内部计数器的值的功能。

#### 86.CyclicBarrier

可循环使用的屏障。

让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续运行。

CyclicBarrier默认构造放时CyclicBarrier(int parities) ,其参数表示屏障拦截的线程数量，每个线程调用await方法告诉CyclicBarrier我已经到达屏障，然后当前线程被阻塞。

#### 87.CountDownLatch与CyclicBarrier区别

CountDownLatch：

计数器：计数器只能使用一次。

等待： 一个线程或多个等待另外n个线程完成之后才能执行。

CyclicBarrier：

计数器：计数器可以重置（通过reset()方法)。

等待： n个线程相互等待，任何一个线程完成之前，所有的线程都必须等待。

#### 88.Semaphore

用来控制同时访问资源的线程数量，通过协调各个线程，来保证合理的公共资源的访问。

应用场景：流量控制，特别是公共资源有限的应用场景，比如数据链接，限流等。

#### 89.Exchanger

Exchanger是一个用于线程间协作的工具类，它提供一个同步点，在这个同步点上，两个线程可以交换彼此的数据。比如第一个线程执行exchange()方法，它会一直等待第二个线程也执行exchange，当两个线程都到同步点，就可以交换数据了。

一般来说为了避免一直等待的情况，可以使用exchange(V x,long timeout,TimeUnit unit),设置最大等待时间。

Exchanger可以用于遗传算法。

#### 90.为什么使用线程池

几乎所有需要异步或者并发执行任务的程序都可以使用线程池。合理使用会给我们带来以下好处。

- 降低系统消耗：重复利用已经创建的线程降低线程创建和销毁造成的资源消耗。
- 提高响应速度： 当任务到达时，任务不需要等到线程创建就可以立即执行。
- 提供线程可以管理性： 可以通过设置合理分配、调优、监控。

#### 91.线程池工作流程

1、判断核心线程池里的线程是否都有在执行任务，否->创建一个新工作线程来执行任务。是->走下个流程。

2、判断工作队列是否已满，否->新任务存储在这个工作队列里，是->走下个流程。

3、判断线程池里的线程是否都在工作状态，否->创建一个新的工作线程来执行任务，

是->走下个流程。

4、按照设置的策略来处理无法执行的任务。

#### 92.创建线程池参数有哪些，作用？

```java
 public ThreadPoolExecutor(   int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler)
```

1.corePoolSize:核心线程池大小，当提交一个任务时，线程池会创建一个线程来执行任务，即使其他空闲的核心线程能够执行新任务也会创建，等待需要执行的任务数大于线程核心大小就不会继续创建。

2.maximumPoolSize:线程池最大数，允许创建的最大线程数，如果队列满了，并且已经创建的线程数小于最大线程数，则会创建新的线程执行任务。如果是无界队列，这个参数基本没用。

3.keepAliveTime: 线程保持活动时间，线程池工作线程空闲后，保持存活的时间，所以如果任务很多，并且每个任务执行时间较短，可以调大时间，提高线程利用率。

4.unit: 线程保持活动时间单位，天（DAYS)、小时(HOURS)、分钟(MINUTES、毫秒MILLISECONDS)、微秒(MICROSECONDS)、纳秒(NANOSECONDS)

5.workQueue: 任务队列，保存等待执行的任务的阻塞队列。

一般来说可以选择如下阻塞队列：

ArrayBlockingQueue:基于数组的有界阻塞队列。

LinkedBlockingQueue:基于链表的阻塞队列。

SynchronizedQueue:一个不存储元素的阻塞队列。

PriorityBlockingQueue:一个具有优先级的阻塞队列。

6.threadFactory：设置创建线程的工厂，可以通过线程工厂给每个创建出来的线程设置更有意义的名字。

7. handler: 饱和策略也叫拒绝策略。当队列和线程池都满了，即达到饱和状态。所以需要采取策略来处理新的任务。默认策略是AbortPolicy。

   AbortPolicy:直接抛出异常。

   CallerRunsPolicy: 调用者所在的线程来运行任务。

   DiscardOldestPolicy:丢弃队列里最近的一个任务，并执行当前任务。

   DiscardPolicy:不处理，直接丢掉。

   当然可以根据自己的应用场景，实现RejectedExecutionHandler接口自定义策略。

#### 93.向线程池提交任务

可以使用execute()和submit() 两种方式提交任务。

execute():无返回值，所以无法判断任务是否被执行成功。

submit():用于提交需要有返回值的任务。线程池返回一个future类型的对象，通过这个future对象可以判断任务是否执行成功，并且可以通过future的get()来获取返回值，get()方法会阻塞当前线程知道任务完成。get(long timeout,TimeUnit unit)可以设置超市时间。

#### 94.关闭线程池

可以通过shutdown()或shutdownNow()来关闭线程池。它们的原理是遍历线程池中的工作线程，然后逐个调用线程的interrupt来中断线程，所以无法响应终端的任务可以能永远无法停止。

shutdownNow首先将线程池状态设置成STOP,然后尝试停止所有的正在执行或者暂停的线程，并返回等待执行任务的列表。

shutdown只是将线程池的状态设置成shutdown状态，然后中断所有没有正在执行任务的线程。

只要调用两者之一，isShutdown就会返回true,当所有任务都已关闭，isTerminaed就会返回true。

一般来说调用shutdown方法来关闭线程池，如果任务不一定要执行完，可以直接调用shutdownNow方法。

#### 95.线程池如何合理设置

配置线程池可以从以下几个方面考虑。

- 任务是cpu密集型、IO密集型或者混合型

- 任务优先级，高中低。

- 任务时间执行长短。

- 任务依赖性：是否依赖其他系统资源。

  cpu密集型可以配置可能小的线程,比如 n + 1个线程。

  io密集型可以配置较多的线程，如 2n个线程。

  混合型可以拆成io密集型任务和cpu密集型任务，

  如果两个任务执行时间相差大，否->分解后执行吞吐量将高于串行执行吞吐量。

  否->没必要分解。

  可以通过Runtime.getRuntime().availableProcessors()来获取cpu个数。

  建议使用有界队列，增加系统的预警能力和稳定性。

#### 96.Executor

从JDK5开始，把工作单元和执行机制分开。工作单元包括Runnable和Callable,而执行机制由Executor框架提供。

#### 97.Executor框架的主要成员

ThreadPoolExecutor  :可以通过工厂类Executors来创建。

可以创建3种类型的ThreadPoolExecutor：SingleThreadExecutor、FixedThreadPool、CachedThreadPool。

ScheduledThreadPoolExecutor ：可以通过工厂类Executors来创建。

可以创建2中类型的ScheduledThreadPoolExecutor：ScheduledThreadPoolExecutor、SingleThreadScheduledExecutor

Future接口:Future和实现Future接口的FutureTask类来表示异步计算的结果。

Runnable和Callable:它们的接口实现类都可以被ThreadPoolExecutor或ScheduledThreadPoolExecutor执行。Runnable不能返回结果，Callable可以返回结果。

#### 98.FixedThreadPool

可重用固定线程数的线程池。

查看源码：

```java
public static ExecutorService newFixedThreadPool(int nThreads) {
   return new ThreadPoolExecutor(nThreads, nThreads,
                                 0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>());}
```

corePoolSize 和maxPoolSize都被设置成我们设置的nThreads。

当线程池中的线程数大于corePoolSize ,keepAliveTime为多余的空闲线程等待新任务的最长时间，超过这个时间后多余的线程将被终止，如果设为0，表示多余的空闲线程会立即终止。

工作流程：

1.当前线程少于corePoolSize,创建新线程执行任务。

2.当前运行线程等于corePoolSize,将任务加入LinkedBlockingQueue。

3.线程执行完1中的任务，会循环反复从LinkedBlockingQueue获取任务来执行。

LinkedBlockingQueue作为线程池工作队列（默认容量Integer.MAX_VALUE)。因此可能会造成如下赢下。

1.当线程数等于corePoolSize时，新任务将在队列中等待，因为线程池中的线程不会超过corePoolSize。

2.maxnumPoolSize等于说是一个无效参数。

3.keepAliveTime等于说也是一个无效参数。

4.运行中的FixedThreadPool(未执行shundown或shundownNow))则不会调用拒绝策略。

5.由于任务可以不停的加到队列，当任务越来越多时很容易造成OOM。

#### 99.SingleThreadExecutor

是使用单个worker线程的Executor。 

查看源码：

```java
public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }
```

corePoolSize和maxnumPoolSize被设置为1。其他参数和FixedThreadPool相同。

执行流程以及造成的影响同FixedThreadPool.

#### 100.CachedThreadPool

根据需要创建新线程的线程池。

查看源码：

```java
    public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
```

corePoolSize设置为0，maxmumPoolSize为Integer.MAX_VALUE。keepAliveTime为60秒。

工作流程：

1.首先执行SynchronousQueue.offer (Runnable task)。如果当前maximumPool 中有空闲线程正在执行S ynchronousQueue.poll(keepAliveTIme,TimeUnit.NANOSECONDS)，那么主线程执行offer操作与空闲线程执行的poll操作配对成功，主线程把任务交给空闲线程执行,execute方 法执行完成;否则执行下面的步骤2。

2. 当初始maximumPool为空或者maximumPool中当前没有空闲线程时，将没有线程执行 SynchronousQueue.poll (keepAliveTime，TimeUnit.NANOSECONDS)。这种情况下，步骤 1将失 败。此时CachedThreadPool会创建一个新线程执行任务，execute()方法执行完成。

3.在步骤2中新创建的线程将任务执行完后，会执行SynchronousQueue.poll (keepAliveTime，TimeUnit.NANOSECONDS)。这个poll操作会让空闲线程最多在SynchronousQueue中等待60秒钟。如果60秒钟内主线程提交了一个新任务(主线程执行步骤1)，那么这个空闲线程将执行主线程提交的新任务;否则，这个空闲线程将终止。由于空闲60秒的空闲线程会被终止,因此长时间保持空闲的CachedThreadPool不会使用任何资源。

一般来说它适合处理时间短、大量的任务。

参考：

- 《Java多线程编程核心技术》

- 《Java高并发编程详解》

- 《Java 并发编程的艺术》

  ![WechatIMG360](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/common1.png)

