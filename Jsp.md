## JSP

#### 1.浏览器jsp，html之间的关系

1.JSP与Java Servlet一样，是在服务器端执行的，通常返回该客户端的就是一个HTML文本，因此客户端只要有浏览器就能浏览

2.在大多数Browser/Server结构的Web应用中，浏览器直接通过HTML或者JSP的形式与用户交互，响应用户的请求

3.JSP在服务器上执行，并将执行结果输出到客户端浏览器，我们可以说基本上与浏览器无关

#### 2.自定义标签要继承哪个类

这个类可以继承TagSupport或者BodyTagSupport，两者的差别是前者适用于没有主体的标签，而后者适用于有主体的标签。如果选择继承TagSupport，可以实现doStartTag和doEndTag两个方法实现Tag的功能，如果选择继承BodyTagSupport，可以实现doAfterBody这个方法。

#### 3.  jsp内置对象和作用？

有九个内置对象：request、response、out、session、application、pageContext、config、page、exception

作用如下：

(1) HttpServletRequest类的Request对象

作用：代表请求对象，主要用于接受客户端通过HTTP协议连接传输到服务器端的数据。

(2) HttpServletResponse类的Respone对象

作用：代表响应对象，主要用于向客户端发送数据

(3) JspWriter类的out对象

作用：主要用于向客户端输出数据; 

​    Out的基类是JspWriter

(4) HttpSession类的session对象

作用：主要用于来分别保存每个用户信息，与请求关联的会话；

​     会话状态维持是Web应用开发者必须面对的问题。

(5) ServletContex类的application对象

作用：主要用于保存用户信息，代码片段的运行环境；

​    它是一个共享的内置对象，即一个容器中的多个用户共享一个application对象，故其保存的信息被所有用户所共享.

(6) PageContext类的PageContext对象

作用：管理网页属性，为JSP页面包装页面的上下文，管理对属于JSP中特殊可见部分中已命名对象的访问，它的创建和初始化都是由容器来完成的。

(7) ServletConfig类的Config对象

作用：代码片段配置对象，表示Servlet的配置。

(8) Object类的Page（相当于this）对象

作用：处理JSP网页，是Object类的一个实例，指的是JSP实现类的实例，即它也是JSP本身，只有在JSP页面范围之内才是合法的。

(9)Exception

作用：处理JSP文件执行时发生的错误和异常

#### 4.jsp乱码如何解决，几种解决方案

一、JSP页面显示乱码
<%@ page contentType=”text/html; charset=gb2312″%>

二、表单提交中文时出现乱码

request.seCharacterEncoding(̶gb2312″)对请求进行统一编码

三、数据库连接出现乱码
要涉及中文的地方全部是乱码，解决办法：在数据库的数据库URL中加上useUnicode=true&characterEncoding=GBK就OK了。

四、通过过滤器完成

五、在server.xml中的设置编码格式

#### 5.页面间对象传递的方法

request，session，application，cookie等
request.setAttribute(key，value)
session.setAttribute(key，value)
application.setAttribute(key，value)

#### 6.BS与CS的联系与区别

B/S模式是指在TCP/IP的支持下，以HTTP为传输协议，客户端通过Browser访问Web服务器以及与之相连的后台数据库的技术及体系结构。它由浏览器、Web服务器、应用服务器和数据库服务器组成。客户端的浏览器通过URL访问Web服务器，Web服务器请求数据库服务器，并将获得的结果以HTML形式返回客户端浏览器。 

c/s在系统机构上和B/S相似，不过需要在客户端安装一个客户端软件，由这个软件对服务器的数据进行读写，就像我们常用的qq，就是这种模式。 

#### 7.描述Jsp页面的运行过程？

第一步：

请求进入Web容器，将JSP页面翻译成Servlet代码

第二步：

编译Servlet代码，并将编译过的类文件装入Web容器（JVM）环境

第三步：

Web容器为JSP页面创建一个Servlet类实例，并执行jspInit方法

第四步：

Web容器为该JSP页面调用Servlet实例的_jspService方法；将结果发送给用户

#### 8.Jsp工作原理

JSP是一种Servlet，但是与HttpServlet的工作方式不太一样。HttpServlet是先由源代码编译为class文件后部署到服务器下，为先编译后部署。而JSP则是先部署后编译。JSP会在客户端第一次请求JSP文件时被编译为HttpJspPage类（接口Servlet的一个子类）。该类会被服务器临时存放在服务器工作目录里面。

由于JSP只会在客户端第一次请求的时候被编译 ，因此第一次请求JSP时会感觉比较慢，之后就会感觉快很多。如果把服务器保存的class文件删除，服务器也会重新编译JSP。

开发Web程序时经常需要修改JSP。Tomcat能够自动检测到JSP程序的改动。如果检测到JSP源代码发生了改动。Tomcat会在下次客户端请求JSP时重新编译JSP，而不需要重启Tomcat。

```java
虽然servlet和jsp本质都是servlet，运行时都是运行.class文件，但是它们的部署方式不一样。
servlet是先编译后部署，修改完以后，MyEclipse进行编译，然后部署.class文件到servlet容器中。如果web服务器已启动，则之前的.class文件已被servlet容器加载，修改后的.class文件不会被servlet容器执行。
而jsp是web服务器进行编译，而不是预先编译好，编译后再加载，tomcat会监视jsp文件的改动，改动之后则重新编译、执行，所以jsp改动时不需要重启服务器。
```

#### 9.Jsp包含的部分

```
指令: <%@  %>
java小脚本: <% java代码 %>   语句带封号;
方法声明: <%!  %>	
表达式: <%=  %>		表达式不带分号;
注释: <%-- 注释内容 --%>
	  java中单行,多行
	  html中<!--  -->
html:
js:
css:
标签:
```

#### 10.getAttribute()与getParameter()

**从获取方向来看：**

`getParameter()`是获取 POST/GET 传递的参数值；

`getAttribute()`是获取对象容器中的数据值；

**从用途来看：**

`getParameter()`用于客户端重定向时，即点击了链接或提交按扭时传值用，即用于在用表单或url重定向传值时接收数据用。

`getAttribute()` 用于服务器端重定向时，即在 sevlet 中使用了 forward 函数。getAttribute 只能收到程序用 setAttribute 传过来的值。

另外，可以用 `setAttribute()`,`getAttribute()` 发送接收对象.而 `getParameter()` 显然只能传字符串。
`setAttribute()` 是应用服务器把这个对象放在该页面所对应的一块内存中去，当你的页面服务器重定向到另一个页面时，应用服务器会把这块内存拷贝另一个页面所对应的内存中。这样`getAttribute()`就能取得你所设下的值，当然这种方法可以传对象。session也一样，只是对象在内存中的生命周期不一样而已。`getParameter()`只是应用服务器在分析你送上来的 request页面的文本时，取得你设在表单或 url 重定向时的值。

**总结：**

`getParameter()`返回的是String,用于读取提交的表单中的值;（获取之后会根据实际需要转换为自己需要的相应类型，比如整型，日期类型啊等等）

`getAttribute()`返回的是Object，需进行转换,可用`setAttribute()`设置成任意对象，使用很灵活，可随时用

#### 11.静态导入与动态导入

静态导入：

```java
<%@include file="validate.jsp" %>
将被导入页面和导入页面，合在一起进行翻译，编译。最后产生一个Servlet，那么两个页面的变量名不能重复。
```

动态导入：

```java
<jsp:include page="validate.jsp"></jsp:include>
动态导入，被导入页面和导入页面分别翻译，编译，产生两个Servlet，所以两个页面的变量名可以重复.都会被执行。
```

#### 12.四种作用域

SP中的四种作用域包括page、request、session和application，具体来说：

- **page**代表与一个页面相关的对象和属性。
- **request**代表与Web客户机发出的一个请求相关的对象和属性。一个请求可能跨越多个页面，涉及多个Web组件；需要在页面显示的临时数据可以置于此作用域。
- **session**代表与某个用户与服务器建立的一次会话相关的对象和属性。跟某个用户相关的数据应该放在用户自己的session中。
- **application**代表与整个Web应用程序相关的对象和属性，它实质上是跨越整个Web应用程序，包括多个页面、请求和会话的一个全局作用域。

#### 13.会话跟踪技术

**1)使用Cookie**

向客户端发送Cookie

```java
Cookie c =new Cookie("name","value"); //创建Cookie 
c.setMaxAge(60*60*24); //设置最大时效，此处设置的最大时效为一天
response.addCookie(c); //把Cookie放入到HTTP响应中
```

从客户端读取Cookie

```java
String name ="name"; 
Cookie[]cookies =request.getCookies(); 
if(cookies !=null){ 
   for(int i= 0;i<cookies.length;i++){ 
    Cookie cookie =cookies[i]; 
    if(name.equals(cookis.getName())) 
    //something is here. 
    //you can get the value 
    cookie.getValue(); 
       
   }
 }
```

**优点:** 数据可以持久保存，不需要服务器资源，简单，基于文本的Key-Value

**缺点:** 大小受到限制，用户可以禁用Cookie功能，由于保存在本地，有一定的安全风险。

**2)URL 重写**

在URL中添加用户会话的信息作为请求的参数，或者将唯一的会话ID添加到URL结尾以标识一个会话。

**优点：** 在Cookie被禁用的时候依然可以使用

**缺点：** 必须对网站的URL进行编码，所有页面必须动态生成，不能用预先记录下来的URL进行访问。

**3)隐藏的表单域**

```html
<input type="hidden" name ="session" value="..."/>
```

**优点：** Cookie被禁时可以使用

**缺点：** 所有页面必须是表单提交之后的结果。

**4)HttpSession**

在所有会话跟踪技术中，HttpSession对象是最强大也是功能最多的。当一个用户第一次访问某个网站时会自动创建 HttpSession，每个用户可以访问他自己的HttpSession。可以通过HttpServletRequest对象的getSession方 法获得HttpSession，通过HttpSession的setAttribute方法可以将一个值放在HttpSession中，通过调用 HttpSession对象的getAttribute方法，同时传入属性名就可以获取保存在HttpSession中的对象。与上面三种方式不同的 是，HttpSession放在服务器的内存中，因此不要将过大的对象放在里面，即使目前的Servlet容器可以在内存将满时将HttpSession 中的对象移到其他存储设备中，但是这样势必影响性能。添加到HttpSession中的值可以是任意Java对象，这个对象最好实现了 Serializable接口，这样Servlet容器在必要的时候可以将其序列化到文件中，否则在序列化时就会出现异常。

#### 14.<%…%>和<%!…%>的区别

<%…%>用于在JSP页面中嵌入Java脚本

<%!…%>用于在JSP页面中申明变量或方法，可以在该页面中的<%…%>脚本中调用，声明的变量相当于Servlet中的定义的成员变量。

#### 15.描述Jsp页面的指令标记的功能、写法、并示例

指令标记影响JSP页面的翻译阶段

<%@ page session=”false” %>

<%@ include file=”incl/copyright.html” %>

<%@ taglib %>

#### 16.描述Jsp页面的声明标记的功能、写法、并示例

声明标记允许JSP页面开发人员包含类级声明

写法：

<%! JavaClassDeclaration %>

```text
例：
  
<%! public static final String DEFAULT_NAME = “World”; %>
  
<%! public String getName(HttpServletRequest request) {
  
                return request.getParameter(“name”);
  
    }
  
%>
  
<%! int counter = 0; %>
```

#### 17.描述Jsp页面翻译成Servlet的规则

jsp中的注释标记被翻译成Servlet类中的注释

jsp中的指令标记被翻译成Servlet类中的import语句等

jsp中的声明标记被翻译成Servlet类中的属性

jsp中的脚本标记被转移到Servlet类中service方法中的代码

jsp中的表达式标记被翻译成Serlvet类中的write()或者print()方法括号中的代码

#### 18.page指令的功能，写法、并示例，并描述它的如下属性的功能和用法：import、session、buffer、errorPage、isErrorPage、ContentType、pageEncoding

import ： import 定义了一组servlet类定义必须导入的类和包，值是一个由逗号分隔的完全类名或包的列表。

session ： session 定义JSP页面是否参与HTTP会话，值可以为true（缺省）或false。

buffer ： buffer 定义用于输出流（JspWriter对象）的缓冲区大小，值可以为none或Nkb，缺省为8KB或更大。

errorPage： 用来指定由另一个jsp页面来处理所有该页面抛出的异常

isErrorPage ： 定义JSP页面为其它JSP页面errorPage属性的目标，值为true或false（缺省）。

ContentType ： 定义输出流的MIME类型，缺省为text/html。

pageEncoding ：定义输出流的字符编码，缺省为ISO-8859-1

#### 19.描述MVC各部分的功能？

1.Model

封装应用状态

响应状态查询

暴露应用的功能

2.Controller

验证HTTP请求的数据

将用户数据与模型的更新相映射

选择用于响应的视图

3.View

产生HTML响应

请求模型的更新

提供HTML form用于用户请求

#### 20.什么是JavaBean?

用户可以使用JavaBean将功能、处理、值、数据库访问和其他任何可以用java代码

创造的对象进行打包，并且其他的开发者可以通过内部的JSP页面、Servlet、其

他JavaBean、applet程序或者应用来使用这些对象。

#### 21.JavaBean的规则？

使用get和set方法定义属性

一个无参构造方法

无public实例变量

#### 22.什么是jsp标准动作？包含那些？分别都是什么功能？如何使用？

JSP页面中使用类似于XML的标记表示运行时的动作

jsp:userBean

jsp:setProperty

jsp:getProperty

jsp:parameter

jsp:include

jsp:forward

#### 23.用代码示例如下标准动作的使用：useBean、getProperty、setProperty

```text
<jsp:useBean
  
id="myForms" 
  
class="com.base.mystruts.forms.MyActionForm" scope="session" />
  
<jsp:setProperty name="MyForms" property="name" />
  
<jsp:getProperty name="MyForms" property="id" />
```

#### 24.描述说明页面上的字段和Bean中属性的对应规则

id 指javabean的变量名

class指javabean类的全路径

scope指javabean的应用范围

name指所用到的javabean的变量名

property指javabean中的属性

#### 25.描述useBean动作的处理过程

使用id声明变量

试图在指定的范围内查找对象

如果没找到

创建一个类的实例

执行useBean标记体初始化对象

如果找到

将对象转换为类指定的类型

#### 参考：

https://blog.csdn.net/woshizoe2/article/details/78993677

https://zhuanlan.zhihu.com/p/252313998

https://www.cnblogs.com/itzlg/p/11380691.html
