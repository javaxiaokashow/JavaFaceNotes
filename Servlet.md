## Servlet

#### 1.Servlet生命周期

servlet的生命周期是初始化（init）、服务（service）、销毁（destroy）

初始化（init）：默认第一次请求前，只初始化一次。修改web.xml，允许服务器启动时初始化。
服务（service）：方法被调用时进行服务，在项目启动期间可以进行多次服务（请求一次执行一次）
销毁（destory）：当服务器关闭时进行销毁。只销毁一次
Servlet接口中声明3个方法，tomcat在不同的时候将调用不同的方法。
init 初始化方法，2种情况被调用
情况1：默认，第一次请求前
情况2：在web项目核心配置文件web.xml中，配置初始化，将在服务器启动时初始化。
每次请求时，调用服务
服务器关闭时，调用销毁。

#### 2.什么是jsp？jsp和Servlet有什么区别？

Servlet是服务器端的程序
JSP是服务器页面程序
JSP本质上就是一个Servlet，在访问jsp时，在服务器端会将jsp先转换成servlet，再将生产的servlet的结果响应给浏览器。
jsp是html页面中内嵌Java代码，侧重页面显示；Servlet是中书写Java代码，侧重逻辑控制；

![img](https://img-blog.csdnimg.cn/20200910144535754.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1ODU0NTk5,size_16,color_FFFFFF,t_70#pic_center)

#### 3.Servlet 接口中有哪些方法？

1. init(ServletConfig)：初始化方法，默认第一次请求前执行，完成 servlet 初始化工作

2. service(ServletRequest,ServletResponse)：执行方法，一次请求执行一次。

3. destroy()：销毁方法，Servlet 对象应该从服务中被移除的时候，容器会调用该方法进行销毁操作

4. getServletConfig()：获得 ServletConfig 配置对象，包括初始化参数等。

5. getServletInfo()：获得 Servlet 描述，一般没有用。

   ![img](https://img-blog.csdnimg.cn/20200910144604142.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1ODU0NTk5,size_16,color_FFFFFF,t_70#pic_center)



#### 4.Servlet 3.0 中的异步处理指的是什么？

异步处理允许 Servlet 重新发起一条新线程去调用 耗时业务方法，这样就可以避免等待

![img](https://img-blog.csdnimg.cn/2020091014463133.png#pic_center)

#### 5.Servlet 中如何获取用户提交的查询参数或表单数据？

1. request.getParameterValues(“参数”); // 获得指定参数名的一组参数值 （String[]）
2. request.getParameter(“参数”); // 获得指定参数名的一个参数值（String） ， UserServlet?username=jack , 通过 username 获得值 jack

```
public class TestRequestParam {
    private HttpServletRequest request;

    public void testDemo01(){
        //请求数据：index.html?username=jack&hobby=抽烟&hobby=喝酒&hobby=烫头

        // 获得username的值，一个值
        String username = request.getParameter("username");

        // 获得hobby的值，一组值
        String[] hobbyArr = request.getParameterValues("hobby");

        // 所有值 , map.key 参数名称，map.value 参数的值
        Map<String,String[]> map = request.getParameterMap();

    }
}
```

#### 6.区别请求的转发与重定向?

 可以从以下三个方面进行比较

1.地址栏:

转发: 显示的是请求的URL

重定向: 显示的不是请求的URL, 而是重定向指向的新的URL

2.浏览器发了几次请求?

转发: 1次请求

重定向: 2次请求

1. 是否可以进行Request的数据共享?

转发: 两个资源之间是同一个request对象, 可以共享request中的数据

重定向: 两个资源之间不是同一个request对象, 不可以共享

**经典现实案例:**

![Servlet相关技术常见面试题](https://img-blog.csdnimg.cn/img_convert/48ff7f8346d74c66da6806855a4b4cb7.png)

#### 7. 比较一下Servlet与Filter

从四个方面来区分：

**概念**

　　servlet是一种运行在服务器端的Java应用程序，独立于平台和协议，可以动态的生成web页面，它工作于客户端请求和服务器的中间层

　　filter是一个可以复用的代码片段，可以用来转换请求，响应以及头信息，filter不能产生请求和响应，他只能在请求到达servlet之前对请求进行修改，或者在请求返回客户端之前对响应进行处理

**生命周期**

　　servlet是在系统启动或者请求到达servlet时，通过init（）方法进行初始化，一旦被装入了web服务器，一般不会从Web服务器删除，直到服务器关闭才会调用　　destroy（）方法进行销毁。每次请求，Request都会被初始化，响应请求后，请求被销毁。但是servlet不会随着请求的销毁而销毁

　　如果某个Servlet配置了 <load-on-startup >1 </load-on-startup >，该Servlet也是在Tomcat（Servlet容器）启动时初始化。
　　如果Servlet没有配置<load-on-startup >1 </load-on-startup >，该Servlet不会在Tomcat启动时初始化，而是在请求到来时初始化。

　　filter

　　　　是在系统启动的时候通过init（）初始化的，每次请求都只会调用dofiter方法进行处理，服务器停止的时候调用destroy()进行销毁

**注意**：服务器关闭时，servlet和filter依次销毁

**职责**

　　**servlet**

　　　　可以动态创建基于客户请求的页面；可以读取客户端发来的隐藏数据和显示数据；可以和其他的服务器资源进行通讯；通过状态代码和响应头向客户端返回数据。

　　**filter**

　　　　主要是对请求到达servlet之前对请求和请求头信息进行前处理，和对数据返回客户端之前进行后处理

**区别**

　　servlet的流程比较短，url来了之后就对其进行处理，处理完就返回数据或者转向另一个页面

　　filter的流程比较长，在一个filter处理之后还可以转向另一个filter进行处理，然后再交给servlet，但是servlet处理之后不能向下传递了。

　　filter可用来进行字符编码的过滤，检测用户是否登陆的过滤，禁止页面缓存等

#### 8.我们在web应用开发过程中经常遇到输出某种编码的字符，如iso8859-1等，如何输出一个某种编码的字符串？

```
 Public String translate (String str) {
    String tempStr = "";

    try {
      tempStr = new String(str.getBytes("ISO-8859-1"), "GBK");

      tempStr = tempStr.trim();

    }

    catch (Exception e) {
      System.err.println(e.getMessage());

    }

    return tempStr;

  }
```

#### 9.Servlet执行时一般实现哪几个方法？

public void init(ServletConfig config)

public ServletConfig getServletConfig()

public String getServletInfo()

public void service(ServletRequest request,ServletResponse response)

public void destroy()


#### 10.描述Cookie和Session的作用，区别和各自的应用范围，Session工作原理。

1）cookie 是一种发送到客户浏览器的文本串句柄，并保存在客户机硬盘上，可以用来在某个WEB站点会话间持久的保持数据。

2）session其实指的就是访问者从到达某个特定主页到离开为止的那段时间。 Session其实是利用Cookie进行信息处理的，当用户首先进行了请求后，服务端就在用户浏览器上创建了一个Cookie，当这个Session结束时，其实就是意味着这个Cookie就过期了。

注：为这个用户创建的Cookie的名称是aspsessionid。这个Cookie的唯一目的就是为每一个用户提供不同的身份认证。

3）cookie和session的共同之处在于：cookie和session都是用来跟踪浏览器用户身份的会话方式。

4）cookie 和session的区别是：cookie数据保存在客户端，session数据保存在服务器端。

5）session工作原理：session技术中所有的数据都保存在服务器上，客户端每次请求服务器的时候会发送当前会话的sessionid，服务器根据当前sessionid判断相应的用户数据标志，以确定用户是否登录或具有某种权限。

#### 11.Applet和Servlet有什么区别？

Applet是运行在客户端主机的浏览器上的客户端Java程序。而Servlet是运行在web服务器上的服务端的组件。applet可以使用用户界面类，而Servlet没有用户界面，相反，Servlet是等待客户端的HTTP请求，然后为请求产生响应。

#### 12.GenericServlet和HttpServlet有什么区别？

GenericServlet是一个通用的协议无关的Servlet，它实现了Servlet和ServletConfig接口。继承自GenericServlet的Servlet应该要覆盖service()方法。最后，为了开发一个能用在网页上服务于使用HTTP协议请求的Servlet，你的Servlet必须要继承自HttpServlet。这里有Servlet的例子。

#### 13.什么是服务端包含(Server Side Include)？

服务端包含(SSI)是一种简单的解释型服务端脚本语言，大多数时候仅用在Web上，用servlet标签嵌入进来。SSI最常用的场景把一个或多个文件包含到Web服务器的一个Web页面中。当浏览器访问Web页面的时候，Web服务器会用对应的servlet产生的文本来替换Web页面中的servlet标签。

#### 14.什么是Servlet链(Servlet Chaining)？

Servlet链是把一个Servlet的输出发送给另一个Servlet的方法。第二个Servlet的输出可以发送给第三个Servlet，依次类推。链条上最后一个Servlet负责把响应发送给客户端。

#### 15.如何知道是哪一个客户端的机器正在请求你的Servlet

ServletRequest类可以找出客户端机器的IP地址或者是主机名。getRemoteAddr()方法获取客户端主机的IP地址，getRemoteHost()可以获取主机名。看下这里的例子。

#### 16.什么是cookie？session和cookie有什么区别？

cookie是Web服务器发送给浏览器的一块信息。浏览器会在本地文件中给每一个Web服务器存储cookie。以后浏览器在给特定的Web服务器发请求的时候，同时会发送所有为该服务器存储的cookie。下面列出了session和cookie的区别：

• 无论客户端浏览器做怎么样的设置，session都应该能正常工作。客户端可以选择禁用cookie，但是，session仍然是能够工作的，因为客户端无法禁用服务端的session。

• 在存储的数据量方面session和cookies也是不一样的。session能够存储任意的Java对象，cookie只能存储String类型的对象。

#### 17.浏览器和Servlet通信使用的是什么协议？

浏览器和Servlet通信使用的是HTTP协议。

#### 18.什么是URL编码和URL解码？

URL编码是负责把URL里面的空格和其他的特殊字符替换成对应的十六进制表示，反之就是解码。

JSP

#### 19.什么是Scriptlets？

JSP技术中，scriptlet是嵌入在JSP页面中的一段Java代码。scriptlet是位于标签内部的所有的东西，在标签与标签之间，用户可以添加任意有效的scriplet。

#### 20.声明(Decalaration)在哪里？

声明跟Java中的变量声明很相似，它用来声明随后要被表达式或者scriptlet使用的变量。添加的声明必须要用开始和结束标签包起来。

#### 

#### 参考：

https://blog.csdn.net/qq_25854599/article/details/108513815

https://blog.csdn.net/msjhw_com/article/details/113240556

https://www.cnblogs.com/htyj/p/8619198.html

https://blog.csdn.net/Jeff_Seid/article/details/80761076

