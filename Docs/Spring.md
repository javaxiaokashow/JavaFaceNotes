## Spring

#### 1.Spring框架?

Spring框架是由于软件开发的复杂性而创建的，Spring使用的是基本的JavaBean来完成以前只可能由EJB完成的事。从简单性、可测性和松耦合性角度而言，绝大部分Java应用都可以用Spring。

#### 2.Spring的整体架构？

![image-20200425151510055](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200425151510055.png)



大约分为20个模块。

#### 3.Spring可以做什么？

![image-20200425152654798](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200425152654798.png)

#### 4.Spring的优点?缺点？

优点：

- Spring属于低侵入设计。
- IOC将对象之间的依赖关系交给Spring,降低组件之间的耦合，实现各个层之间的解耦，让我们更专注于业务逻辑。
- 提供面向切面编程。
- 对各种主流插件提供很好的集成支持。
- 对事务支持的很好，只要配置即可，无须手动控制。

缺点：

- 依赖反射，影响性能。

#### 5.你能说几个Spring5的新特性吗？

- spring5整个框架基于java8
- 支持http/2
- Spring Web MVC支持最新API
- Spring WebFlux 响应式编程
- 支持Kotlin函数式编程

#### 6.IOC?

负责创建对象、管理对象(通过依赖注入)、整合对象、配置对象以及管理这些对象的生命周期。

#### 7.什么是依赖注入?

依赖注入是Spring实现IoC的一种重要手段，将对象间的依赖关系的控制权从开发人员手里转移到容器。

#### 8.IOC注入哪几种方式？

1.构造器注入

2.setter注入

3.接口注入（我们几乎不用）

#### 9.IOC优点？缺点？

优点：

- 组件之间的解耦，提高程序可维护性、灵活性。

缺点：

- 创建对象步骤复杂，有一定学习成本。
- 利用反射创建对象，效率上有损。（对于代码的灵活性和可维护性来看，Spring对于我们的开发带来了很大的便利，这点损耗不算什么哦）

#### 10.bean的生命周期?

1.Spring 对bean进行实例化。

2.Spring将值和bean的引用注入到 bean对应的属性中。

3.如果bean实现了BeanNameAware接口，Spring将bean的ID传递给setBeanName()方法。

4.如果bean实现了BeanFactoryAware接口， Spring将调用setBeanFactory()方法，将 bean所在的应用引用传入进来。

5.如果bean实现了ApplicationContextAware接口，Spring将调用setApplicationContext()方法，将bean所在的应用引用传入进来。

6.如果bean实现了BeanPostProcessor 接口，Spring将调用他们的post-ProcessBeforeInitalization()方法。

7.如果bean实现了InitializingBean接口，Spring将调用他们的after-PropertiesSet()方法，类似地，如果bean使用init-method声明了初始化方法，该方法也会被调用。

8.如果bean实现了BeanPostProcessor接口，Spring将调用它们的post-ProcessAfterInitialization()方法。

9.此时， bean已经准备就绪，可以被应用程序使用了，他们将一直驻留在应用上下文中,直到该应用被销毁。

10.如果bean实现了DisposableBean接口，Spring将调用它的destory()接口方法，同样，如果bean使用destroy-method声明了销毁方法，该方法也会被调用。

#### 11.Spring有几种配置方式？

- 基于xml
- 基于注解
- 基于Java

#### 12.Spring中的bean有几种scope?

- singleton: 单例，每一个bean只创建一个对象实例。
- prototype，原型，每次对该bean请求调用都会生成各自的实例。
- request，请求，针对每次HTTP请求都会生成一个新的bean。表示在一次 HTTP 请求内有效。
- session，在一个http session中，一个bean定义对应一个bean实例。
- global session:在一个全局http session中，一个bean定义对应一个bean实例。

#### 13.什么是AOP(面向切面编程)？

在软件业，AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期间动态代理实现程序功能的统一维护的一种技术。

#### 14.切面有几种类型的通知？分别是？

前置通知(Before): 目标方法被调用之前调用通知功能。

后置通知(After): 目标方法完成之后调用通。

返回通知(After-returning): 目标方法成功执行之后调用通知。

异常通知(After-throwing): 目标方法抛出异常后调用通知。

环绕通知(Around):  在被通知的方法调用之前和调用之后执行自定义的行为。

#### 15.什么是连接点 （Join point)?

连接点是在应用执行过程中能够插入切面的一个点。这个点可以是调用方法时、抛出异常时、甚至修改一个字段时。

#### 16.什么是切点（Pointcut)?

切点的定义会匹配通知所要织入的一个或多个连接点。我们通常使用明确的类和方法名称，或是利用正则表达式定义所匹配的类和方法名称来指定这些切点。有些AOP框架允许我们创建动态的切点，可以根据运行时的决策(比如方法的参数值)来决定是否应用通知。

#### 17.什么是切面(Aspect)?

切面是通知和切点的结合。通知和切点共同定义了切面的全部内容。

#### 18.织入(Weaving)?

织入是把切面应用到目标对象并创建新的代理对象的过程。切面在指定的连接点被织入到目标对象中。

#### 19.引入（Introduction）？

􏶸􏰫􏶹􏵡􏰸􏱲􏰇􏲘􏱴􏰉􏵇􏶺􏶻􏶼􏰽􏱸􏵼􏶽􏶾􏱎􏶸􏰫􏶹􏵡􏰸􏱲􏰇􏲘􏱴􏰉􏵇􏶺􏶻􏶼􏰽􏱸􏵼􏶽􏶾􏱎引入允许我们向现有的类添加新方法或属性。

#### 20.在目标对象的生命周期里有多个点可以进行织入？

- 编译期：切面在目标类编译时被织入。AspectJ的织入编译器就是以这种方式织入切面的。
- 类加载期：切面在目标类加载到JVM时被织入。它可以在目标类被引入应用之前增强该目标类的字节码。AspectJ 5的加载时织入(load-time weaving，LTW)就支持以这种方式织入切面。
- 运行期：切面在应用运行的某个时刻被织入。一般情况下，在织入切面时，AOP容器会为目标对象动态地创建一个代理对象。Spring AOP就是以这种方式织入切面的。

#### 21.AOP动态代理策略？

- 如果目标对象实现了接口，默认采用JDK 动态代理。可以强制转为CgLib实现AOP。
- 如果没有实现接口，采用CgLib进行动态代理。

#### 22.什么是MVC框架？

MVC全名是Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。

MVC被独特的发展起来用于映射传统的输入、处理和输出功能在一个逻辑的图形化用户界面的结构中。

#### 23.什么是SpringMVC?

SpringMVC是Spring框架的一个模块。是一个基于MVC的框架。

#### 24.SpringMVC的核心？

DispatcherServlet

#### 25.SpringMVC的几个组件？

DispatcherServlet : 前端控制器，也叫中央控制器。相关组件都是它来调度。

HandlerMapping : 处理器映射器，根据URL路径映射到不同的Handler。

HandlerAdapter : 处理器适配器，按照HandlerAdapter的规则去执行Handler。

Handler  : 处理器,由我们自己根据业务开发。

ViewResolver : 视图解析器，把逻辑视图解析成具体的视图。

View : 一个接口，它的实现支持不同的视图类型（freeMaker，JSP等）

#### 26.SpringMVC工作流程？

1.用户请求旅程的第一站是DispatcherServlet。

2.收到请求后，DispatcherServlet调用HandlerMapping，获取对应的Handler。

3.如果有拦截器一并返回。

4.拿到Handler后，找到HandlerAdapter,通过它来访问Handler,并执行处理器。

5.执行Handler的逻辑。

6.Handler会返回一个ModelAndView对象给DispatcherServlet。

7.将获得到的ModelAndView对象返回给DispatcherServlet。

8.请求ViewResolver解析视图，根据逻辑视图名解析成真正的View。

9.返回View给DispatcherServlet。

10.DispatcherServlet对View进行渲染视图。

11.DispatcherServlet响应用户。

#### 27.SpringMVC的优点？

1.具有Spring的特性。

2.可以支持多种视图(jsp,freemaker)等。

3.配置方便。

4.非侵入。

5.分层更清晰，利于团队开发的代码维护，以及可读性好。

Tips:Jsp目前很少有人用了。

#### 28.单例bean是线程安全的吗？

不是。具体线程问题需要开发人员来处理。

#### 29.Spring从哪两个角度实现自动装配？

组件扫描(component scanning):Spring会自动发现应用上下文中所创建的bean。

自动装配(autowiring):Spring自动满足bean之间的依赖。

#### 30.自动装配有几种方式？分别是？

no -  默认设置，表示没有自动装配。

byName ： 根据名称装配。

byType ： 根据类型装配。

constructor ： 把与Bean的构造器入参具有相同类型的其他Bean自动装配到Bean构造器的对应入参中。

autodetect ：先尝试constructor装配，失败再尝试byType方式。

default：由上级标签<beans>的default-autowire属性确定。

#### 31.说几个声明Bean 的注解？

- @Component 
- @Service 
- @Repository 
- @Controller 

#### 32.注入Java集合的标签？

- <list> 允许有相同的值。
- <set> 不允许有相同的值。
- <props>   键和值都只能为String类型。
- < map >    键和值可以是任意类型。

#### 33.**Spring支持的ORM**？

1. Hibernate
2. iBatis
3. JPA (Java Persistence API)
4. TopLink
5. JDO (Java Data Objects)
6. OJB

#### 34.@Repository注解？

 Dao 层实现类注解，扫描注册 bean。 

#### 35.@Value注解?

讲常量、配置中的变量值、等注入到变量中。

#### 36.@Controller注解?

定义控制器类。

#### 37.声明一个切面注解是哪个？

@Aspect 

#### 38.映射web请求的注解是？

@RequestMapping

#### 39.@ResponseBody注解？

作用是将返回对象通过适当的转换器转成置顶格式，写进response的body区。通常用来返回json、xml等。

#### 40.@ResponseBody + @Controller =?

@RestController

#### 41.接收路径参数用哪个注解？

@PathVariable

#### 42.@Cacheable注解？

用来标记缓存查询。

#### 43.清空缓存是哪个注解？

@CacheEvict

#### 44.@Component注解？

泛指组件，不好归类时，可以用它。

#### 45.**BeanFactory 和 ApplicationContext**区别？

![image-20200428210402245](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200428210402245.png)

#### 46.@Qualifier注解？

当创建多个相同类型的 bean 时，并且想要用一个属性只为它们其中的一个进行装配，在这种情况下，你可以使用 @Qualifier 注释和 @Autowired 注释通过指定哪一个真正的 bean 将会被装配来消除混乱。

#### 47.事务的注解是?

@Transaction

#### 48.Spring事务实现方式有？

声明式：声明式事务也有两种实现方式。

- xml 配置文件的方式。
- 注解方式（在类上添加 @Transaction 注解）。

编码式：提供编码的形式管理和维护事务。

#### 49.什么是事务传播？

事务在嵌套方法调用中如何传递，具体如何传播，取决于事务传播行为。

#### 50.Spring事务传播行为有哪些？

![image-20200428223544334](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200428223544334.png)

参考：

- 《Spring in action 4》
- 《SPRING技术内幕》
- 《Spring源码深度解析》
- 《Spring5企业级开发实战》
- https://spring.io
- 百度百科