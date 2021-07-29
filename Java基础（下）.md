## 基础（下）

#### 1.Java和C++的区别?

我知道很多人没学过 C++，但是面试官就是没事喜欢拿咱们 Java 和 C++ 比呀！没办法！！！就算没学过C++，也要记下来！

都是面向对象的语言，都支持封装、继承和多态

Java 不提供指针来直接访问内存，程序内存更加安全

Java 的类是单继承的，C++ 支持多重继承；虽然 Java 的类不可以多继承，但是接口可以多继承。

Java 有自动内存管理机制，不需要程序员手动释放无用内存

#### 2.什么是 Java 程序的主类 应用程序和小程序的主类有何不同?

一个程序中可以有多个类，但只能有一个类是主类。在 Java 应用程序中，这个主类是指包含 main（）方法的类。而在 Java 小程序中，这个主类是一个继承自系统类 JApplet 或 Applet 的子类。应用程序的主类不一定要求是 public 类，但小程序的主类要求必须是 public 类。主类是 Java 程序执行的入口点。

#### 3.Java 应用程序与小程序之间有哪些差别?

简单说应用程序是从主线程启动(也就是 `main()` 方法)。applet 小程序没有 `main()` 方法，主要是嵌在浏览器页面上运行(调用`init()`或者`run()`来启动)，嵌入浏览器这点跟 flash 的小游戏类似。

#### 4.import java和javax有什么区别？

刚开始的时候 JavaAPI 所必需的包是 java 开头的包，javax 当时只是扩展 API 包来使用。然而随着时间的推移，javax 逐渐地扩展成为 Java API 的组成部分。但是，将扩展从 javax 包移动到 java 包确实太麻烦了，最终会破坏一堆现有的代码。因此，最终决定 javax 包将成为标准API的一部分。 所以，实际上java和javax没有区别。这都是一个名字。

#### 5.object-c中的协议和java中的接口概念有何不同？

OC中的协议有2层含义，官方定义为 formal和informal protocol。前者和Java接口一样。 informal protocol中的方法属于设计模式考虑范畴，不是必须实现的，但是如果有实现，就会改变类的属性。 其实关于正式协议，类别和非正式协议我很早前学习的时候大致看过，也写在了学习教程里 “非正式协议概念其实就是类别的另一种表达方式“这里有一些你可能希望实现的方法，你可以使用他们更好的完成工作”。 这个意思是，这些是可选的。比如我门要一个更好的方法，我们就会申明一个这样的类别去实现。然后你在后期可以直接使用这些更好的方法。 这么看，总觉得类别这玩意儿有点像协议的可选协议。" 现在来看，其实protocal已经开始对两者都统一和规范起来操作，因为资料中说“非正式协议使用interface修饰“， 现在我们看到协议中两个修饰词：“必须实现(@requied)”和“可选实现(@optional)”。

#### 6.Javascipt的本地对象，内置对象和宿主对象

本地对象： Object、Function、Array、String、Boolean、Number、Date、RegExp、Error、EvalError、RangeError、ReferenceError、SyntaxError、TypeError、URIError, 简单来说，本地对象就是 ECMA一262 定义的类.

内置对象： ECMA一262 把内置对象（built一in object）定义为“由 ECMAScript 实现提供的、独立于宿主环境的所有对象，在 ECMAScript 程序开始执行时出现”。这意味着开发者不必明确实例化内置对象，它已被实例化了。 同样是“独立于宿主环境”。根据定义我们似乎很难分清“内置对象”与“本地对象”的区别。而ECMA一262 只定义了两个内置对象，即 Global 和 Math （它们也是本地对象，根据定义，每个内置对象都是本地对象）。 如此就可以理解了。内置对象是本地对象的一种。而其包含的两种对象中，Math对象我们经常用到，可这个Global对象是啥东西呢？ Global对象是ECMAScript中最特别的对象，因为实际上它根本不存在，有点玩人的意思。大家要清楚，在ECMAScript中，不存在独立的函数，所有函数都必须是某个对象的方法。 类似于isNaN()、parseInt()和parseFloat()方法等，看起来都是函数，而实际上，它们都是Global对象的方法。而且Global对象的方法还不止这些.

宿主对象: ECMAScript中的“宿主”就是我们网页的运行环境，即“操作系统”和“浏览器”。所有非本地对象都是宿主对象（host object），即由 ECMAScript 实现的宿主环境提供的对象。所有的BOM和DOM对象都是宿主对象。因为其对于不同的“宿主”环境所展示的内容不同。其实说白了就是，ECMAScript官方未定义的对象都属于宿主对象，因为其未定义的对象大多数是自己通过ECMAScript程序创建的对象。自定义的对象也是宿主对象。

#### 7.在javascript中什么是伪数组，如何将伪数组转化为标准数组 

这里把符合以下条件的对象称为伪数组：

1，具有length属性

2，按索引方式存储数据

3，不具有数组的push,pop等方法 伪数组（类数组）：无法直接调用数组方法或期望length属性有什么特殊的行为，不具有数组的push,pop等方法，但仍可以对真正数组遍历方法来遍历它们。典型的是函数的argument参数，还有像调用document.getElementsByTagName, document.childNodes之类的,它们返回的NodeList对象都属于伪数组。

#### 8.请问EJB与JAVA BEAN的区别是什么？

Java Bean 是可复用的组件，对Java Bean并没有严格的规范，理论上讲，任何一个Java类都可以是一个Bean。但通常情况下，由于Java Bean是被容器所创建（如Tomcat）的，所以Java Bean应具有一个无参的构造器，另外，通常Java Bean还要实现Serializable接口用于实现Bean的持久性。Java Bean实际上相当于微软COM模型中的本地进程内COM组件，它是不能被跨进程访问的。EnterpriseJava Bean 相当于DCOM，即分布式组件。它是基于Java的远程方法调用（RMI）技术的，所以EJB可以被远程访问（跨进程、跨计算机）。但EJB必须被布署在诸如Webspere、WebLogic这样的容器中，EJB客户从不直接访问真正的EJB组件，而是通过其容器访问。EJB容器是EJB组件的代理， EJB组件由容器所创建和管理。客户通过容器来访问真正的EJB组件。

#### 9.请你说说Java和PHP的区别？

PHP暂时还不支持像Java那样JIT运行时编译热点代码,但是PHP具有opcache机制,能够把脚本对应的opcode缓存在内存,PHP7中还支持配置opcache.file_cache导出opcode到文件.第三方的Facebook HHVM也支持JIT.另外PHP官方基于LLVM围绕opcache机制构建的Zend JIT分支也正在开发测试中.在php-src/Zend/bench.php测试显示,PHP JIT分支速度是PHP 5.4的10倍. PHP的库函数用C实现,而Java核心运行时类库(jdk/jre/lib/rt.jar,大于60MB)用Java编写(jdk/src.zip), 所以Java应用运行的时候,用户编写的代码以及引用的类库和框架都要在JVM上解释执行. Java的HotSpot机制,直到有方法被执行10000次才会触发JIT编译, 在此之前运行在解释模式下,以避免出现JIT编译花费的时间比方法解释执行消耗的时间还要多的情况.

PHP内置模板引擎,自身就是模板语言.而Java Web需要使用JSP容器如Tomcat或第三方模板引擎.

PHP也可以运行在多线程模式下,比如Apache的event MPM和Facebook的HHVM都是多线程架构.不管是多进程还是多线程的PHP Web运行模式,都不需要PHP开发者关心和控制,也就是说PHP开发者不需要写代码参与进程和线程的管理,这些都由PHP-FPM/HHVM/Apache实现.PHP-FPM进程管理和并发实现并不需要PHP开发者关心,而Java多线程编程需要Java开发者编码参与.PHP一个worker进程崩溃,master进程会自动新建一个新的worker进程,并不会导致PHP服务崩溃.而Java多线程编程稍有不慎(比如没有捕获异常)就会导致JVM崩溃退出.对于PHP-FPM和Apache MOD_PHP来说,服务进程常驻内存,但一次请求释放一次资源,这种内存释放非常彻底. PHP基于引用计数的GC甚至都还没发挥作用程序就已经结束了。

#### 10.请你谈谈Java中是如何支持正则表达式操作的？

Java中的String类提供了支持正则表达式操作的方法，包括：matches()、replaceAll()、replaceFirst()、split()。此外，Java中可以用Pattern类表示正则表达式对象，它提供了丰富的API进行各种正则表达式操作，如：

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
class RegExpTest {
    public static void main(String[] args) {
        String str = "成都市(成华区)(武侯区)(高新区)";
        Pattern p = Pattern.compile(".*?(?=\\()");
        Matcher m = p.matcher(str);
        if(m.find()) {
            System.out.println(m.group());
        }
    }
}
```

#### 11.请你说明一下，在Java中如何跳出当前的多重嵌套循环？

在最外层循环前加一个标记如A，然后用break A;可以跳出多重循环。（Java中支持带标签的break和continue语句，作用有点类似于C和C++中的goto语句，但是就像要避免使用goto一样，应该避免使用带标签的break和continue，因为它不会让你的程序变得更优雅，很多时候甚至有相反的作用，所以这种语法其实不知道更好），根本不能进行字符串的equals比较，否则会产生NullPointerException异常。

#### 12.请说明Java的接口和C++的虚类的相同和不同处

由于Java不支持多继承，而有可能某个类或对象要使用分别在几个类或对象里面的方法或属性，现有的单继承机制就不能满足要求。 与继承相比，接口有更高的灵活性，因为接口中没有任何实现代码。当一个类实现了接口以后，该类要实现接口里面所有的方法和属性，并且接口里面的属性在默认状态下面都是public static,所有方法默认情况下是public.一个类可以实现多个接口。

#### 13.请说明Java是否支持多继承？

Java中类不支持多继承，只支持单继承（即一个类只有一个父类）。 但是java中的接口支持多继承，，即一个子接口可以有多个父接口。（接口的作用是用来扩展对象的功能，一个子接口继承多个父接口，说明子接口扩展了多个功能，当类实现接口时，类就扩展了相应的功能）。

#### 14.请讲讲Java有哪些特性，并举一个和多态有关的例子。

封装、继承、多态。多态：指允许不同类的对象对同一消息做出响应。即同一消息可以根据发送对象的不同而采用多种不同的行为方式。（发送消息就是函数调用）

#### 15.什么是Java虚拟机？为什么Java被称作是“平台无关的编程语言”？

Java虚拟机是一个可以执行Java字节码的虚拟机进程。Java源文件被编译成能被Java虚拟机执行的字节码文件。 Java被设计成允许应用程序可以运行在任意的平台，而不需要程序员为每一个平台单独重写或者是重新编译。Java虚拟机让这个变为可能，因为它知道底层硬件平台的指令长度和其他特性。

#### 16.请列举一下，在JAVA虚拟机中，哪些对象可作为ROOT对象？

虚拟机栈中的引用对象

方法区中类静态属性引用的对象

方法区中常量引用对象

本地方法栈中JNI引用对象

#### 17.C++,Java，JavaScript这三种语言的区别

参考回答：

从静态类型还是动态类型来看

静态类型，编译的时候就能够知道每个变量的类型，编程的时候也需要给定类型，如Java中的整型int，浮点型float等。C、C++、Java都属于静态类型语言。

动态类型，运行的时候才知道每个变量的类型，编程的时候无需显示指定类型，如JavaScript中的var、PHP中的$。JavaScript、Ruby、Python都属于动态类型语言。

静态类型还是动态类型对语言的性能有很大影响。

对于静态类型，在编译后会大量利用已知类型的优势，如int类型，占用4个字节，编译后的代码就可以用内存地址加偏移量的方法存取变量，而地址加偏移量的算法汇编很容易实现。

对于动态类型，会当做字符串通通存下来，之后存取就用字符串匹配。

从编译型还是解释型来看

编译型语言，像C、C++，需要编译器编译成本地可执行程序后才能运行，由开发人员在编写完成后手动实施。用户只使用这些编译好的本地代码，这些本地代码由系统加载器执行，由操作系统的CPU直接执行，无需其他额外的虚拟机等。

源代码=》抽象语法树=》中间表示=》本地代码

解释性语言，像JavaScript、Python，开发语言写好后直接将代码交给用户，用户使用脚本解释器将脚本文件解释执行。对于脚本语言，没有开发人员的编译过程，当然，也不绝对。

源代码=》抽象语法树=》解释器解释执行。

对于JavaScript，随着Java虚拟机JIT技术的引入，工作方式也发生了改变。可以将抽象语法树转成中间表示（字节码），再转成本地代码，如JavaScriptCore，这样可以大大提高执行效率。也可以从抽象语法树直接转成本地代码，如V8

Java语言，分为两个阶段。首先像C++语言一样，经过编译器编译。和C++的不同，C++编译生成本地代码，Java编译后，生成字节码，字节码与平台无关。第二阶段，由Java的运行环境也就是Java虚拟机运行字节码，使用解释器执行这些代码。一般情况下，Java虚拟机都引入了JIT技术，将字节码转换成本地代码来提高执行效率。

注意，在上述情况中，编译器的编译过程没有时间要求，所以编译器可以做大量的代码优化措施。

对于JavaScript与Java它们还有的不同：

对于Java，Java语言将源代码编译成字节码，这个同执行阶段是分开的。也就是从源代码到抽象语法树到字节码这段时间的长短是无所谓的。

对于JavaScript，这些都是在网页和JavaScript文件下载后同执行阶段一起在网页的加载和渲染过程中实施的，所以对于它们的处理时间有严格要求。

#### 18.java nextLine 与 next 两个方法的区别？

1. next 不会接收回车符，tab 或者空格键，在接收有效数据之前会忽略这些符号，若已经读取了有效数据，遇到这些符号会直接退出
2. nextLine 可以接收空格或者 tab 键，其输入以 enter 键结束

#### 19.什么是 JavaConfig？

Spring JavaConfig 是 Spring 社区的产品，它提供了配置 Spring IoC 容器的纯Java 方法。因此它有助于避免使用 XML 配置。使用 JavaConfig 的优点在于：

（1）面向对象的配置。由于配置被定义为 JavaConfig 中的类，因此用户可以充分利用 Java 中的面向对象功能。一个配置类可以继承另一个，重写它的@Bean 方法等。

（2）减少或消除 XML 配置。基于依赖注入原则的外化配置的好处已被证明。但是，许多开发人员不希望在 XML 和 Java 之间来回切换。JavaConfig 为开发人员提供了一种纯 Java 方法来配置与 XML 配置概念相似的 Spring 容器。从技术角度来讲，只使用 JavaConfig 配置类来配置容器是可行的，但实际上很多人认为将JavaConfig 与 XML 混合匹配是理想的。

（3）类型安全和重构友好。JavaConfig 提供了一种类型安全的方法来配置 Spring容器。由于 Java 5.0 对泛型的支持，现在可以按类型而不是按名称检索 bean，不需要任何强制转换或基于字符串的查找。

#### 20.停止非循环Java线程

这可能是我误读了我所读内容的一种情况，但是在Java中杀死线程的所有示例似乎都表明您必须发出信号以杀死自己。您不能在没有严重风险的情况下从外面杀死它。问题是，所有有关如何“礼貌地”要求线程死亡的示例都有某种循环，因此您要做的就是观察每次迭代中的标志。

因此，我得到的是一个线程，该线程执行的操作仅需要一段时间（一系列SQL查询）。我当然可以在每个步骤之后进行检查，但是它们并没有处于循环中，并且我没有一种非常优雅的方式可以解决此问题。这是我正在做的事的一个例子：

```
new Thread(new Runnable(){
    public void run(){
        //query 1
        Connection conn = db.getConnection();
        Statement s = conn.createStatement();
        ResultSet rs = s.executeQuery("SELECT ...");
        while(rs.next()){
            //do stuff
        }

        //query 2
        rs = s.executeQuery("SELECT ...");
        while(rs.next()){
            //do stuff
        }

        //query 3
        rs = s.executeQuery("SELECT ...");
        while(rs.next()){
            //do stuff
        }
    }
}).start();
```

这是一个示例，我没有使用匿名内部类，但是它说明了我的run（）方法无法优雅地停止自身。此外，即使我在每个步骤之后都进行检查，如果特定查询需要很长时间才能运行，则该代码将无法在查询完成后停止。

这段代码是针对GUI应用程序的，我真的很想找到一种无需使用Thread.stop（）即可快速杀死线程的好方法。

**编辑**
-yshavit的回答很有帮助，因为我不知道它的`Statement.cancel()`存在。如果您感到好奇，那么对我的特定问题的答案是建立一个更抽象的数据库访问类。该类必须创建一个子线程以在运行时执行查询和循环，并检查每次迭代是否当前线程（不是子线程）被中断。如果确实被中断，它将仅调用Statement.cancel（），并且子线程将引发异常并死亡。并非所有的JDBC驱动程序都支持`Statement.cancel()`，但是Oracle
11g支持。



#### 21.在java中使用最简单的方法打印数组内容？

从Java 5开始，你可以将Arrays.toString(arr)或Arrays.deepToString(arr)用于数组中的数组。
请注意，Object[]版本会调用.toString()数组中的每个对象。输出甚至按照您的要求进行修饰。

**例子：**

简单数组：

```
String[] array = new String[] {"John", "Mary", "Bob"};
System.out.println(Arrays.toString(array));
```

输出：

```
[John, Mary, Bob]
```

嵌套数组：

```
String[][] deepArray = new String[][] {{"John", "Mary"}, {"Alice", "Bob"}};
System.out.println(Arrays.toString(deepArray));
//output: [[Ljava.lang.String;@106d69c, [Ljava.lang.String;@52e922]
System.out.println(Arrays.deepToString(deepArray));
```

输出：

```
[[John, Mary], [Alice, Bob]]
```

double 数组：

```
double[] doubleArray = { 7.0, 9.0, 5.0, 1.0, 3.0 };
System.out.println(Arrays.toString(doubleArray));
```

输出：

```
[7.0, 9.0, 5.0, 1.0, 3.0 ]
```

int 数组：

```
int[] intArray = { 7, 9, 5, 1, 3 };
System.out.println(Arrays.toString(intArray));
```

输出：

```
[7, 9, 5, 1, 3 ]
```

#### 22.为什么打印java对象得到SomeType@2f92e0f4这样的结果？

##### 背景

所有Java对象都有一个`toString()`方法，当你尝试打印该对象时会调用该方法。

```
System.out.println(myObject);  // invokes myObject.toString()
```

此方法在`Object`类（所有Java对象的超类）中定义。该`Object.toString()`方法返回一个看起来很难看的字符串，该字符串由类的名称，`@`符号和对象的哈希码（十六进制）组成。此代码如下所示：

```
// Code of Object.toString()
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

这样的结果`com.foo.MyType@2f92e0f4`可以解释为：

- com.foo.MyType -类的名称，即类MyType在package中com.foo。
- @ -将字符串连接在一起
- 2f92e0f4 对象的哈希码。

数组类的名称看起来有些不同，这在Javadocs for中得到了很好的解释`Class.getName()`。例如，`[Ljava.lang.String`表示：

- [-一维数组（相对于[[或[[[等）
- L -数组包含一个类或接口
- java.lang.String -数组中对象的类型

##### 自定义输出

要在调用时打印不同的内容`System.out.println(myObject)`，必须重写`toString()`自己类中的方法。这是一个简单的例子：

```
public class Person {
  private String name;
  // constructors and other methods omitted
  @Override
  public String toString() {
    return name;
  }
}
```

现在，如果我们打印一个Person，我们将看到它们的名称，而不是`com.foo.Person@12345678`。

请记住，这toString()只是将对象转换为字符串的一种方法。通常，此输出应以简洁明了的方式完全描述你的对象。toString()对于我们的Person班级来说，更好的选择可能是：

```
@Override
public String toString() {
  return getClass().getSimpleName() + "[name=" + name + "]";
}
```

将打印，例如`Person[name=Henry]`。这对于调试/测试来说是非常有用的数据。

如果你只想关注对象的一个方面或包含许多爵士乐的格式，则最好定义一个单独的方法，例如String toElegantReport() {…}。

##### 自动生成输出

许多IDEtoString()基于类中的字段提供了对自动生成方法的支持。例如，请参阅Eclipse和IntelliJ的文档。

一些流行的Java库也提供此功能。一些示例包括：

- ToStringBuilder来自Apache Commons Lang
- MoreObjects.ToStringHelper来自Google Guava
- @ToString龙目岛项目的注释

##### 打印对象组

因此，你已经`toString()`为课程创建了一个不错的选择。如果将该类放入数组或集合，会发生什么情况？

##### 数组

如果你有一个对象数组，则可以调用Arrays.toString()以生成该数组内容的简单表示。例如，考虑以下Person对象数组：

```
Person[] people = { new Person("Fred"), new Person("Mike") };
System.out.println(Arrays.toString(people));
```

// Prints: [Fred, Mike]

注意：这是对Arrays类中调用的静态方法的调用toString()，这与我们上面讨论的内容不同。

如果你具有多维数组，则可以用于Arrays.deepToString()实现相同类型的输出。

##### 集合

大多数集合都会基于.toString()对每个元素的调用而产生漂亮的输出。

```
List<Person> people = new ArrayList<>();
people.add(new Person("Alice"));
people.add(new Person("Bob"));    
System.out.println(people);
```

// Prints [Alice, Bob]
因此，你只需要确保列表元素定义一个`toString()`如上所述的尼斯。

#### 23.如何理解和使用Java中的增强型for循环foreach？

```
for (Iterator<String> i = someIterable.iterator(); i.hasNext();) {
    String item = i.next();
    System.out.println(item);
}
```

请注意，如果你需要`i.remove()`;在循环中使用或以某种方式访问实际的迭代器，则不能使用该`for(:)`惯用语，因为实际的迭代器只是推断出来的。

正如Denis Bueno所指出的那样，此代码适用于实现该Iterable接口的任何对象。

同样，如果`for(:)`习惯用法的右侧是一个`array`而不是一个`Iterable`对象，则内部代码将使用一个`int`索引计数器并进行检查`array.length`。请参阅Java语言规范。

#### 24.在java中为什么 1/3 == 0?

运行下面的代码结果为0？

```
public static void main(String[] args) {
    double g = 1 / 3;
    System.out.printf("%.2f", g);
}
```

#### 25.Java 7中的菱形运算符（<>）有什么意义？

Java 7中的菱形运算符允许如下代码：

```
List<String> list = new LinkedList<>();
```

但是，在Java 5/6中，我可以简单地编写：

```
List<String> list = new LinkedList();
```

我对类型擦除的理解是这些完全相同。（无论如何，泛型都会在运行时删除）。

```
List<String> list = new LinkedList();
```

是在左侧，你使用的是通用类型`List<String>`，而在右侧，你使用的是原始类型LinkedList。Java中的原始类型实际上仅存在于与前泛型代码的兼容性，并且除非绝对必要，否则绝对不能在新代码中使用。

现在，如果Java从一开始就具有泛型，并且没有LinkedList最初在具有泛型之前创建的类型（例如），则它可能已经做到了，这样泛型类型的构造函数会自动从左侧推断出其类型参数-尽可能在作业的另一侧。但事实并非如此，为了向后兼容，必须对原始类型和泛型类型进行不同的处理。这使得他们需要采取一种稍微不同但同样方便的方式来声明泛型对象的新实例，而不必重复其类型参数……菱形运算符。

就你的原始示例而言`List<String> list = new LinkedList()`，编译器会为该分配生成警告，因为它必须这样做。考虑一下：

```
List<String> strings = ... // some list that contains some strings

// Totally legal since you used the raw type and lost all type checking!
List<Integer> integers = new LinkedList(strings);
```

存在泛型以提供编译时保护以防止做错事。在上面的示例中，使用原始类型意味着你没有获得此保护，并且在运行时会收到错误消息。这就是为什么你不应该使用原始类型的原因。

```
// Not legal since the right side is actually generic!
List<Integer> integers = new LinkedList<>(strings);
```

但是，菱形运算符允许将赋值的右侧定义为具有与左侧相同类型参数的真实泛型实例，而不必再次键入这些参数。它使你可以与使用原始类型几乎相同的工作来保持泛型的安全。

我认为关键要理解的是，原始类型（不带`<>`）不能与泛型类型相同。声明原始类型时，不会获得任何好处和泛型的类型检查。你还必须记住，泛型是Java语言的通用组成部分 ……它们不仅仅适用于Collections 的无参数构造函数！