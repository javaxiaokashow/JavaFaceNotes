## Mybatis

#### 1.什么是Mybatis?

MyBatis 是一款优秀的支持自定义 SQL 查询、存储过程和高级映射的持久层框架，消除了 几乎所有的 JDBC 代码和参数的手动设置以及结果集的检索 。 MyBatis 可以使用 XML 或注解进 行配置和映射， MyBatis 通过将参数映射到配置的 SQL 形成最终执行的 SQL 语句 ，最后将执行 SQL 的结果映射成 Java对象返回。

#### 2.Hibernate优点？

Hibernate建立在POJO和数据库表模型的直接映射关系上。通过xml或注解即可和数据库表做映射。通过pojo直接可以操作数据库的数据。它提供的是全表的映射模型。

- 消除代码映射规则，被分离到xml或注解里配置。
- 无需在管理数据库连接，配置在xml中即可。
- 一个会话中，不要操作多个对象，只要操作Session对象即可。
- 关闭资源只需关闭Session即可。

#### 3.Hibernate缺点？

- 全表映射带来的不便，比如更新需要发送所有的字段。
- 无法根据不同的条件组装不同的sql。
- 对多表关联和复杂的sql查询支持较差，需要自己写sql，返回后，需要自己将数据组成pojo。
- 不能有效支持存储过程。
- 虽然有hql,但是性能较差，大型互联网需要优化sql，而hibernate做不到。

#### 4.Mybatis优点？

1. 小巧，学习成本低，会写sql上手就很快了。
2. 比jdbc，基本上配置好了，大部分的工作量就专注在sql的部分。
3. 方便维护管理，sql不需要在Java代码中找，sql代码可以分离出来，重用。
4. 接近jdbc,灵活，支持动态sql。
5. 支持对象与数据库orm字段关系映射。

#### 5.Mybatis缺点?

- 由于工作量在sql上，需要 sql的熟练度高。
- 移植性差。sql语法依赖数据库。不同数据库的切换会因语法差异，会报错。

#### 6.什么时候用Mybatis?

如果你需要一个灵活的、可以动态生成映射关系的框架。目前来说，因为适合，互联网项目用Mybatis的比例还是很高滴。

#### 7.Mybatis的核心组件有哪些？分别是？

SqlSessionFactoryBuilder(构造器) :它会根据配置信息或者代码来生成SqlSessionFactory。

SqlSessionFactory（工厂接口）:依靠工厂来生成SqlSession。

SqlSession（会话):是一个既可以发送 sql去执行返回结果，也可以获取Mapper接口。

SQL Mapper:它是新设计的组件，是由一个Java接口和XML文件（或注解）构成的。需要给出对象的SQl和映射规则。它负责发送SQL去执行，并返回结果。

#### 8.#{}和${}的区别是什么？

${}是字符串替换，#{}是预编译处理。一般用#{}防止 sql注入问题。

#### 9.Mybatis中9个动态标签是？

- if
- c h o o s e (when 、 oterwise)
- trim (where、 set)
- foreach
- bind

#### 10.xml映射文件中，有哪些标签？

select|insert|updae|delete|resultMap|parameterMap|sql|include|selectKey 加上9个动态标签，其中<sql>为sql片段标签，通过<include>标签引入sql片段，<selectKey>为不支持自增的主键生成策略标签。

#### 11.Mybatis支持注解吗？优点？缺点？

支持。

优点：对于需求简单sql逻辑简单的系统，效率较高。

缺点： 当sql变化需要重新编译代码，sql复杂时，写起来更不方便，不好维护。

#### 12.Mybatis动态sql？

Mybatis 动态 sql 可以让我们在 Xml 映射文件内，以标签的形式编写动态 sql，完成逻辑
判断和动态拼接 sql 的功能

#### 13.**Mybatis 是如何进行分页的?分页插件的原理是什么?**

1）Mybatis 使用 RowBounds 对象进行分页，也可以直接编写 sql 实现分页，也可以使用
Mybatis 的分页插件。
2）分页插件的原理：实现 Mybatis 提供的接口，实现自定义插件，在插件的拦截方法内拦
截待执行的 sql，然后重写 sql。
举例：select from student，拦截 sql 后重写为：select t. from （select from student）t
limit 0，10

#### 14.如何获取自增主键?

注解：

```java
@Options(useGeneratedKeys =true, keyProperty =”id”)

int insert( );
```

Xml：

```xml
<insert id=”insert” useGeneratedKeys=”true” keyProperty=” id”>

sql

</insert>
```

#### 15.为什么Mapper接口没有实现类，却能被正常调用?

这是因为MyBatis在Mapper接口上使用了动态代理。

#### 16.用注解好还是xml好?

简单的增删改查可以注解。

复杂的sql还是用xml,官方也比较推荐xml方式。

xml的方式更便于统一维护管理代码。

#### 17.如果不想手动指定别名，如何用驼峰的形式自动映射？

mapUnderscoreToCamelCase=true

#### 18.当实体属性名和表中字段不一致，怎么办？

 一、别名:  比如 order_num

select order_num orderNum

二、通过<resultMap>做映射

​    <result property = “order_Num" column =”order_num"/>

三、如果是驼峰注解用17问的方式。

#### 19.嵌套查询用什么标签？

association 标签的嵌套查询常用的属性如下 。

select:另一个映射查询的 id, MyBatis会额外执行这个查询获取嵌套对象的结果。

column:列名(或别名)，将主查询中列的结果作为嵌套查询的 参数，配置 方式如 column={propl=coll , prop2=col2}, propl 和 prop2 将作为嵌套查询的参数。

fetch Type:数据加载方式，可选值为 lazy 和 eager，分别为延迟加载和积极加载， 这个配置会覆盖全局的 lazyLoadingEnabled 配置 。

#### 20.like模糊查询怎么写？

（1）'%${value}%'  不推荐

（2）CONCAT('%',#{value},'%')     推荐

  (3)   like '%'|| #{value} || '%'

（4）使用bind 

<bind name="pattern" value="'%' + _parameter.username + '%'" />

   LIKE #{pattern}

#### 21.Mybatis支持枚举吗？

支持。有转换器EnumTypeHandler和EnumOrdinalTypeHandler

```xml
<typeHandlers>
    <typeHandler handler="org.apache.ibatis.type.EnumOrdinalTypeHandler" javaType="枚举类（com.xx.StateEnum)"/>
</typeHandlers>
```

#### 22.SqlSessionFactoryBuilder生命周期?

利用xml或者Java代码获得资源构建SqlSessionFactoryBuilder，它可以创建多个SessionFactory,一旦构建成功SessionFactory,就可以回收它了。

#### 23.一级缓存的结构?如何开启一级缓存？如何不使用一级缓存？

Map 。默认情况下，一级缓存是开启的。< select >标签内加属性flushCache=true。

#### 24.二级缓存如何配置？

```xml
<settings>
〈 !一其他自己直一 〉
<setting name=” cacheEnabled” value=” true ” />
</settings>
```

这个参数是二级缓存的全局开关，默认值是 true，初始状态 为启用状态 。 如果把这个参数设置为 false，即使有后面 的 二级缓 存配置，也不会生效 。

#### 25.**简述 Mybatis 的插件运行原理，以及如何编写一个插件？**

1）Mybatis 仅可以编写针对 ParameterHandler、ResultSetHandler、StatementHandler、
Executor 这 4 种接口的插件，Mybatis 通过动态代理，为需要拦截的接口生成代理对象以实
现接口方法拦截功能，每当执行这 4 种接口对象的方法时，就会进入拦截方法，具体就是
InvocationHandler 的 invoke()方法，当然，只会拦截那些你指定需要拦截的方法。
2）实现 Mybatis 的 Interceptor 接口并复写 intercept()方法，然后在给插件编写注解，指定
要拦截哪一个接口的哪些方法即可，记住，别忘了在配置文件中配置你编写的插件。

#### 26.二级缓存的回收策略有哪些？

eviction (收回策略)

LRU(最近最少使用的) : 移除最长时间不被使用的对象，这是默认值 。

 IFO(先进先出〉 :  按对象进入缓存的顺序来移除它们 。

 SOFT(软引用) : 移除基于垃圾回收器状态和软引用规则的对象 。

WEAK (弱引用) : 更积极地移除基于垃圾收集器状态和弱引用规则的对象 。

#### 27.Mybatis的Xml文件中id可以重复吗?

同一namespace下，id不可重复。不同namespace下，可以重复。

#### 28. 和Mybatis搭配java框架中比较好用的缓存框架？有哪些特点?

EhCache是一个纯牌的 Java进程内的缓存框架，具有快速、精干等特点。

具体来说， EhCache主要的特性如下 。 

- 快速。
- 简单。
- 多种缓存策略 。
- 缓存数据有内存和磁盘两级，无须担心容量问题 。
- 缓存数据会在虚拟机重启 的过程中写入磁盘。
- 可 以通过 RMI、可插入 API 等方式进行分布式缓存。
- .具有缓存和缓存管理器的侦 昕接口。
- 支持多缓存管理器实例 以及一个实例的多个缓存区域。

#### 29.说一下resultMap和resultType

resultmap是手动提交，人为提交，resulttype是自动提交
MyBatis中在查询进行select映射的时候，返回类型可以用resultType，也可以用resultMap，resultType是直接表示返回类型的，而resultMap则是对外部ResultMap的引用，但是resultType跟resultMap不能同时存在。
在MyBatis进行查询映射时，其实查询出来的每一个属性都是放在一个对应的Map里面的，其中键是属性名，值则是其对应的值。
1.当提供的返回类型属性是resultType时，MyBatis会将Map里面的键值对取出赋给resultType所指定的对象对应的属性。所以其实MyBatis的每一个查询映射的返回类型都是ResultMap，只是当提供的返回类型属性是resultType的时候，MyBatis对自动的给把对应的值赋给resultType所指定对象的属性。
2.当提供的返回类型是resultMap时，因为Map不能很好表示领域模型，就需要自己再进一步的把它转化为对应的对象，这常常在复杂查询中很有作用。

#### 30.Mybatis动态sql有什么用？执行原理？有哪些动态sql？

有九种动态sql标签：trim,where,set,foreach,if,choose,when,bind,otherwise
Mybatis的动态sql可以在xml映射文件内，以标签的形式编写动态sql，执行原理是根据表达式的值，完成逻辑判断并动态拼接sql的功能！

#### 参考

《 MyBatis从入门到精通》

《深入浅出 MyBatis技术原理与实战》

《MyBatis技术》

https://zhuanlan.zhihu.com/p/60257737


![WechatIMG360](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/common1.png)

