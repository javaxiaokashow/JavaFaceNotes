#### Java8

#### 1.阐述 Java 7 和 Java 8 的区别

实话说，两者有很多不同。如果你能列出最重要的，应该就足够了。你应该解释 Java 8 中的新功能。想要获得完整清单，请访问官网：Java 8 JDK。

你应该知道以下几个重点：

- **lambda 表达式**，Java 8 版本引入的一个新特性。lambda 表达式允许你将功能当作方法参数或将代码当作数据。lambda 表达式还能让你以更简洁的方式表示只有一个方法的接口 (称为函数式接口) 的实例。
- **方法引用**，为已命名方法提供了易于阅读的 lambda 表达式。
- 默认方法，支持将新功能添加到类库中的接口，并确保与基于这些接口的旧版本的代码的二进制兼容性。
- **重复注解**，支持在同一声明或类型上多次应用同一注解类型。
- **类型注解**，支持在任何使用类型的地方应用注解，而不仅限于声明。此特性与可插入型系统一起使用时，可增强对代码的类型检查。

#### 2.Java SE 8中最流行和最著名的最新功能是什么？

Java SE 8最受欢迎和最著名的最新功能包括以下内容：

功能接口。集合API增强功能。Lambda表达式。分流器。流API等。

#### 3.是什么使Java SE 8优于其他？

Java SE 8具有以下功能，使其优于其他功能：

它编写并行代码。它提供了更多可用的代码。它具有改进的性能应用程序。它具有更易读和简洁的代码。它支持编写包含促销的数据库。

#### 4.在Java SE 8中定义Lambda表达式？

Lambda表达式是Java SE 8，是匿名函数的名称，该匿名函数有助于接受一组不同的输入参数，并提供各种结果结果。

#### 5.为什么将Lambda Expression创造为代码块？

Lambda表达式是作为代码块创造的，因为它没有名称，可以带有或不带有参数和结果。

#### 6.Lambda表达式和功能接口之间有什么联系？

当我们使用Lambda表达式时，这意味着我们正在使用功能接口。因此，它们都是相互关联的。这意味着Lambda表达式是Functional接口的一部分，Functional接口是一个承载各种其他功能和表达式的更大平台。

#### 7.在Java SE 8中定义Nashorn？

Nashorn是在Java SE 8的Java平台上使用的最新Javascript处理引擎。

#### 8.Map和FlatMap流操作之间的主要区别是什么？

Map和FlatMap流操作之间的主要区别在于，前者将返回值包装在其序数类型内，而后者则没有。

#### 9.Map和Flat map流操作之间的相似之处是什么？

Map和FlatMap流操作都是中间流操作，它们接收一个函数并将这些函数应用于流的不同元素。

#### 10.定义流管道？

Java SE 8中的流管道用于通过拆分可能在一个流上发生的操作来将操作链接在一起。

#### 11.什么是使用Stream Pipeline的强制性？

使用Stream Pipeline的强制性在于存在终端操作，该操作有助于返回最终值并支持管道的终止。

#### 12.新日期和时间API的作用是什么？

新的日期和时间API是在Java SE 8中的java time软件包下设计的，因此可以避免与JDK或Java.util.date相关的问题。

#### 13.Java SE 8的核心API类是什么？

Java SE 8的核心API类包括LocalDate，LocalTime和LocalDateTime。

#### 14.Metaspace与PermGen相比有什么优势？

PerGen的大小是固定的，不能动态增长，而Metaspace可以动态增长，并且确实具有任何类型的大小约束。

#### 15.功能接口和SAM接口之间有什么区别吗？

不，功能接口和SAM接口之间没有区别。 SAM接口或单一抽象方法接口是Java SE 8 API中定义的一种功能接口。

#### 16.接口默认方法和静态方法

    interface IDefaultFunction{
        default void study(){
            System.out.println("什么时候能找到工作");
    }
    static void finJob(){
        System.out.println("开始找工作");
    }
    }
    
    
    class DefaultFunction implements IDefaultFunction{
    
    }

Java 8用默认方法与静态方法这两个新概念来扩展接口的声明。默认方法与抽象方法不同之处在于抽象方法必须要求实现，但是默认方法则没有这个要求，就是接口可以有实现方法，而且不需要实现类去实现其方法。我们只需在方法名前面加个default关键字即可实现默认方法。为什么要有这个特性？以前当需要修改接口的时候，需要修改全部实现该接口的类。而引进的默认方法的目的是为了解决接口的修改与现有的实现不兼容的问题。

#### 17.引入了流Stream

Stream可以链式书写代码，需要几行搞定的代码，Stream可以一行搞定，Stream是使用内部迭代，而且Stream支持并行操作

    @Test
    public void streamTest(){
    List<Integer>list = new ArrayList<>();
    	list.add(2);
    	list.add(4);
    	list.add(5);
    	list.forEach(integer -> {
    	System.out.println(integer);
    });
     System.out.println("=======分割线=========");
        List<Integer> list2 = list.stream().filter((i)-> i>2).collect(Collectors.toList());
        list2.forEach(i-> System.out.println(i));
    }
输出

```
2
4
5
=======分割线=========
4
5
```

filter：接受lamada表达式，从中截取一符合条件的元素
limit：截流limit(5)，表示截取五个元素
skip(n)：跳过元素，返回一个扔掉了前n个元素的流，若流中元素不足n个，则返回一个空流，与limit(n)互补
distinct：筛选，通过流所生成元素的hashCode()和equals()去除重复元素
map--接收Lambda，将元素转换成其他形式或提取信息

#### 18.可以重复注解

@Repeatable注解

#### 19.集合引入了很多parallel开头的并行操作的方法

特别是parallelSort,这个将排序拆分很多小的集合，运用多线程进行排序，最后合并，得到最终想要的结果。

#### 20.日期时间

Clock

LocalDate  只保存有ISO-8601日期系统的日期部分，有时区信息

LocalTime  只保存ISO-8601日期系统的时间部分，没有时区信息

LocalDateTime类合并了LocalDate和LocalTime，它保存有ISO-8601日期系统的日期和时间，但是没有时区信息

ZonedDateTime，它保存有ISO-8601日期系统的日期和时间，而且有时区信息

Duration类，Duration持有的时间精确到纳秒。它让我们很容易计算两个日期中间的差异

#### 21.Nashorn javascript 引擎

Java 8提供了一个新的Nashorn javascript引擎，它允许我们在JVM上运行特定的javascript应用

#### 22.Base64

新的Base64API也支持URL和MINE的编码解码

#### 23.并行数组

Java 8新增加了很多方法支持并行的数组处理**parallelSort()**

#### 24.并发

java.util.concurrent.ConcurrentHashMap中加入了一些新方法来支持聚集操作

java.util.concurrent.ForkJoinPool类中加入了一些新方法来支持共有资源池

java.util.concurrent.locks.StampedLock类提供一直基于容量的锁，这种锁有三个模型来控制读写操作（它被认为是不太有名的

java.util.concurrent.locks.ReadWriteLock类的替代者）

#### 25.什么是Lambda表达式？

Lambda Expression可以定义为允许用户将方法作为参数传递的匿名函数。这有助于删除大量的样板代码。Lambda函数没有访问修饰符（私有，公共或受保护），没有返回类型声明和没有名称。

Lambda表达式允许用户将“函数”传递给代码。所以，与以前需要一整套的接口/抽象类想必，我们可以更容易地编写代码。例如，假设我们的代码具有一些复杂的循环/条件逻辑或工作流程。使用lambda表达式，在那些有难度的地方，可以得到很好的解决。



#### 参考

https://zhuanlan.zhihu.com/p/215758969

https://baijiahao.baidu.com/s?id=1667297300847848263&wfr=spider&for=pc

https://blog.csdn.net/wangbiao007/article/details/103229950

https://www.cnblogs.com/wangwanchao/p/5269648.html