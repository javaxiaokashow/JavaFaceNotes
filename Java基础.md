## Java基础

#### 1.Java语言的三大特性

##### 1.封装：

首先，属性可用来描述同一类事物的特征，方法可描述一类事物可做的操作。封装就是把属于同一类事物的共性（包括属性与方法）归到一个类中，以方便使用。



概念：封装也称为信息隐藏，是指利用抽象数据类型将数据和基于数据的操作封装在一起，使其构成一个不可分割的独立实体，数据被保护在抽象数据类型的内部，尽可能地隐藏内部的细节，只保留一些对外接口使之与外部发生联系。系统的其他部分只有通过包裹在数据外面的被授权的操作来与这个抽象数据类型交流与交互。也就是说，用户无需知道对象内部方法的实现细节，但可以根据对象提供的外部接口(对象名和参数)访问该对象。
好处：

(1)实现了专业的分工。将能实现某一特定功能的代码封装成一个独立的实体后，各程序员可以在需要的时候调用，从而实现了专业的分工。

(2)隐藏信息，实现细节。通过控制访问权限可以将可以将不想让客户端程序员看到的信息隐藏起来，如某客户的银行的密码需要保密，只能对该客户开发权限。

##### 2.继承：

就是个性对共性的属性与方法的接受，并加入个性特有的属性与方法
1.概念：一个类继承另一个类，则称继承的类为子类，被继承的类为父类。
2.目的：实现代码的复用。
3.理解：子类与父类的关系并不是日常生活中的父子关系，子类与父类而是一种特殊化与一般化的关系，是is-a的关系，子类是父类更加详细的分类。如class dog extends animal,就可以理解为dog is a animal.注意设计继承的时候，若要让某个类能继承，父类需适当开放访问权限，遵循里氏代换原则，即向修改关闭对扩展开放，也就是开-闭原则。
4.结果：继承后子类自动拥有了父类的属性和方法，但特别注意的是，父类的私有属性和构造方法并不能被继承。
另外子类可以写自己特有的属性和方法，目的是实现功能的扩展，子类也可以复写父类的方法即方法的重写。

##### 3.多态：

多态的概念发展出来，是以封装和继承为基础的。
多态就是在抽象的层面上实施一个统一的行为，到个体（具体）的层面上时，这个统一的行为会因为个体（具体）的形态特征而实施自己的特征行为。（针对一个抽象的事，对于内部个体又能找到其自身的行为去执行。）
1.概念：相同的事物，调用其相同的方法，参数也相同时，但表现的行为却不同。
2.理解：子类以父类的身份出现，但做事情时还是以自己的方法实现。子类以父类的身份出现需要向上转型(upcast)，其中向上转型是由JVM自动实现的，是安全的，但向下转型(downcast)是不安全的，需要强制转换。子类以父类的身份出现时自己特有的属性和方法将不能使用。

#### 2.Java语言主要特性

1. **Java语言是易学的。**Java语言的语法与C语言和C++语言很接近，使得大多数程序员很容易学习和使用Java。

2. **Java语言是强制面向对象的。**Java语言提供类、接口和继承等原语，为了简单起见，只支持类之间的单继承，但支持接口之间的多继承，并支持类与接口之间的实现机制（关键字为implements）。

3. **Java语言是分布式的**。Java语言支持Internet应用的开发，在基本的Java应用编程接口中有一个网络应用编程接口（java net），它提供了用于网络应用编程的类库，包括URL、URLConnection、Socket、ServerSocket等。Java的RMI（远程方法激活）机制也是开发分布式应用的重要手段。

4. **Java语言是健壮的。**Java的强类型机制、异常处理、垃圾的自动收集等是Java程序健壮性的重要保证。对指针的丢弃是Java的明智选择。

5. **Java语言是安全的。**Java通常被用在网络环境中，为此，Java提供了一个安全机制以防恶意代码的攻击。如：安全防范机制（类ClassLoader），如分配不同的名字空间以防替代本地的同名类、字节代码检查。

6. **Java语言是体系结构中立的。**Java程序（后缀为java的文件）在Java平台上被编译为体系结构中立的字节码格式（后缀为class的文件），然后可以在实现这个Java平台的任何系统中运行。

7. **Java语言是解释型的。**如前所述，Java程序在Java平台上被编译为字节码格式，然后可以在实现这个Java平台的任何系统的解释器中运行。（一次编译，到处运行）

8. **Java是性能略高的**。与那些解释型的高级脚本语言相比，Java的性能还是较优的。

9. **Java语言是原生支持多线程的。**在Java语言中，线程是一种特殊的对象，它必须由Thread类或其子（孙）类来创建。

#### 3. JDK 和 JRE 有什么区别

- JDK：Java Development Kit 的简称，Java 开发工具包，提供了 Java 的开发环境和运行环境。
- JRE：Java Runtime Environment 的简称，Java 运行环境，为 Java 的运行提供了所需环境。

具体来说 JDK 其实包含了 JRE，同时还包含了编译 Java 源码的编译器 Javac，还包含了很多 Java 程序调试和分析的工具。简单来说：如果你需要运行 Java 程序，只需安装 JRE 就可以了，如果你需要编写 Java 程序，需要安装 JDK。 

#### 4.Java基本数据类型及其封装类

![image-20200413145454097](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200413145454097.png)

Tips:boolean类型占了单独使用是4个字节，在数组中又是1个字节
基本类型所占的存储空间是不变的。这种不变性也是Java具有可移植性的原因之一。
基本类型放在栈中，直接存储值。
所有数值类型都有正负号，没有无符号的数值类型。

为什么需要封装类？
因为泛型类包括预定义的集合，使用的参数都是对象类型，无法直接使用基本数据类型，所以Java又提供了这些基本类型的封装类。

基本类型和对应的封装类由于本质的不同。具有一些区别：
1.基本类型只能按值传递，而封装类按引用传递。
2.基本类型会在栈中创建，而对于对象类型，对象在堆中创建，对象的引用在栈中创建，基本类型由于在栈中，效率会比较高，但是可能存在内存泄漏的问题。

#### 5.如果main方法被声明为private会怎样？

能正常编译，但运行的时候会提示”main方法不是public的”。在idea中如果不用public修饰，则会自动去掉可运行的按钮。

#### 6.说明一下public static void main(String args[])这段声明里每个关键字的作用

public: main方法是Java程序运行时调用的第一个方法，因此它必须对Java环境可见。所以可见性设置为pulic.

static: Java平台调用这个方法时不会创建这个类的一个实例，因此这个方法必须声明为static。

void: main方法没有返回值。

String是命令行传进参数的类型，args是指命令行传进的字符串数组。

#### 7.==与equals的区别

==比较两个对象在内存里是不是同一个对象，就是说在内存里的存储位置一致。两个String对象存储的值是一样的，但有可能在内存里存储在不同的地方 。

==比较的是引用而equals方法比较的是内容。public boolean equals(Object obj) 这个方法是由Object对象提供的，可以由子类进行重写。默认的实现只有当对象和自身进行比较时才会返回true,这个时候和==是等价的。String, BitSet, Date, 和File都对equals方法进行了重写，对两个String对象 而言，值相等意味着它们包含同样的字符序列。对于基本类型的包装类来说，值相等意味着对应的基本类型的值一样。

```java
public class EqualsTest {
    public static void main(String[] args) {
      String s1 = “abc”;
      String s2 = s1;
      String s5 = “abc”;
      String s3 = new String(”abc”);
      String s4 = new String(”abc”);
      System.out.println(”== comparison : ” + (s1 == s5));
      System.out.println(”== comparison : ” + (s1 == s2));
      System.out.println(”Using equals method : ” + s1.equals(s2));
      System.out.println(”== comparison : ” + s3 == s4);
      System.out.println(”Using equals method : ” + s3.equals(s4));
     }
}
```

结果：

```tex
== comparison : true
== comparison : true
Using equals method : true
false
Using equals method :true
```

#### 8.Object有哪些公用方法

**Object是所有类的父类，任何类都默认继承Object**

**clone**  保护方法，实现对象的浅复制，只有实现了Cloneable接口才可以调用该方法，否则抛出CloneNotSupportedException异常。

**equals**  在Object中与==是一样的，子类一般需要重写该方法。

**hashCode**  该方法用于哈希查找，重写了equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到。

**getClass**  final方法，获得运行时类型

**wait**   使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。 **wait()** 方法一直等待，直到获得锁或者被中断。 **wait(long timeout)** 设定一个超时间隔，如果在规定时间内没有获得锁就返回。

**调用该方法后当前线程进入睡眠状态，直到以下事件发生**

1、其他线程调用了该对象的notify方法。 2、其他线程调用了该对象的notifyAll方法。 3、其他线程调用了interrupt中断该线程。 4、时间间隔到了。 5、此时该线程就可以被调度了，如果是被中断的话就抛出一个InterruptedException异常。

**notify** 唤醒在该对象上等待的某个线程。

**notifyAll** 唤醒在该对象上等待的所有线程。

**toString** 转换成字符串，一般子类都有重写，否则打印句柄。

#### 9.为什么Java里没有全局变量?

全局变量是全局可见的，Java不支持全局可见的变量，因为：全局变量破坏了引用透明性原则。全局变量导致了命名空间的冲突。

#### 10.while循环和do循环有什么不同？

while结构在循环的开始判断下一个迭代是否应该继续。do/while结构在循环的结尾来判断是否将继续下一轮迭代。do结构至少会执行一次循环体。

#### 11.char型变量中能不能存储一个中文汉字?为什么？

可以。Java默认Unicode编码。Unicode码占16位。 char两个字节刚好16位。

#### 12.public，private，protected的区别，继承方法与访问权限

![image-20200413174229242](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200413174229242.png)

 Tips:不写默认default

#### 13.**float f=3.4;是否正确？**

不正确。3.4是双精度数，将双精度型（double）赋值给浮点型（float）属于下转型（down-casting，也称为窄化）会造成精度损失，因此需要强制类型转换float f =(float)3.4; 或者写成float f =3.4F。

#### 14.**short s1 = 1; s1 = s1 + 1;有错吗？short s1 = 1; s1 += 1;有错吗？**

对于short s1 = 1; s1 = s1 + 1；由于1是int类型，因此s1+1运算结果也是int 型，需要强制转换类型才能赋值给short型。而short s1 = 1; s1 += 1；+=操作符会进行**隐式自动类型转换**，是 **Java 语言规定的运算符**；Java编译器会对它进行特殊处理，因此可以正确编译。因为s1+= 1;相当于s1 = (short)(s1 + 1)。

#### 15.**&和&&的区别？**

1. &：(1)按位与；(2)逻辑与。

 按位与: 0 & 1 = 0 ; 0 & 0 = 0; 1 & 1 = 1

 逻辑与: a == b &  b ==c （即使a==b已经是 false了，程序还会继续判断b是否等于c)

2.&&: 短路与

​         a== b && b== c （当a==b 为false则不会继续判断b是否等与c)

比如判断某对象中的属性是否等于某值，则必须用&&,否则会出现空指针问题。

#### 16.IntegerCache

```Java
public class IntegerTest {

    public static void main(String[] args) {

        Integer a = 100, b = 100 ,c = 129,d = 129;
      
        System.out.println(a==b);
      
        System.out.println(c==d);
    }
}

```

 结果:

```tex
true
false
```

小朋友，你是否有很多问号?

来解释一下:

```java
   /**
     * Cache to support the object identity semantics of autoboxing for values between
     * -128 and 127 (inclusive) as required by JLS.
     *
     * The cache is initialized on first usage.  The size of the cache
     * may be controlled by the {@code -XX:AutoBoxCacheMax=<size>} option.
     * During VM initialization, java.lang.Integer.IntegerCache.high property
     * may be set and saved in the private system properties in the
     * sun.misc.VM class.
     */

    private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                try {
                    int i = parseInt(integerCacheHighPropValue);
                    i = Math.max(i, 127);
                    // Maximum array size is Integer.MAX_VALUE
                    h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                } catch( NumberFormatException nfe) {
                    // If the property cannot be parsed into an int, ignore it.
                }
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k < cache.length; k++)
                cache[k] = new Integer(j++);

            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high >= 127;
        }

        private IntegerCache() {}
    }
```

 

```Java
 public static Integer valueOf(int i) {
    assert IntegerCache.high >= 127;
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
  }
```

通过源码，我们可以看出, -128~127之间做了缓存。考虑到高频数值的复用场景，这样做还是很合理的，合理优化。最大边界可以通过-XX:AutoBoxCacheMax进行配置。

#### 17.Locale类是什么？

Locale类用来根据语言环境来动态调整程序的输出。

#### 18.Java中final、finally、finalize的区别与用法

1. final
   final是一个修饰符也是一个关键字。

- 被final修饰的类无法被继承
- 对于一个final变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；
  如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。但是它*指向的对象的内容是可变的*。
- 被final修饰的方法将无法被重写，但*允许重载*
  注意：类的private方法会隐式地被指定为final方法。

2. finally
   finally是一个关键字。

- finally在异常处理时提供finally块来执行任何清除操作。不管有没有异常被抛出或者捕获，finally块都会执行，通常用于释放资源。
- finally块正常情况下一定会被执行。但是有至少两个极端情况：
  如果对应的try块没有执行，则这个try块的finally块并不会被执行
  如果在try块中jvm关机，例如system.exit(n)，则finally块也不会执行（都拔电源了，怎么执行）
- finally块中如果有return语句，则会覆盖try或者catch中的return语句，导致二者无法return，所以强烈建议finally块中不要存在return关键字

3. finalize
   finalize()是Object类的protected方法，子类可以覆盖该方法以实现资源清理工作。
   *GC在回收对象之前都会调用该方法*
   finalize()方法是存在很多问题的：

- java语言规范并不保证finalize方法会被及时地执行，更根本不会保证它们一定会被执行
- finalize()方法可能带来性能问题，因为JVM通常在单独的低优先级线程中完成finalize的执行
- finalize()方法中，可将待回收对象赋值给GC Roots可达的对象引用，从而达到对象再生的目的
- finalize方法最多由GC执行一次（但可以手动调用对象的finalize方法）

#### 19.hashCode()和equals()的区别

下边从两个角度介绍了他们的区别：一个是性能，一个是可靠性。他们之间的主要区别也基本体现在这里。

1.equals()既然已经能实现对比的功能了，为什么还要hashCode()呢？
因为重写的equals（）里一般比较的比较全面比较复杂，这样效率就比较低，而利用hashCode()进行对比，则只要生成一个hash值进行比较就可以了，效率很高。

2.hashCode()既然效率这么高为什么还要equals()呢？
因为hashCode()并不是完全可靠，有时候不同的对象他们生成的hashcode也会一样（生成hash值得公式可能存在的问题），所以hashCode()只能说是大部分时候可靠，并不是绝对可靠，所以我们可以得出（PS：以下两条结论是重点，很多人面试的时候都说不出来）：

equals()相等的两个对象他们的hashCode()肯定相等，也就是用equals()对比是绝对可靠的。

hashCode()相等的两个对象他们的equals()不一定相等，也就是hashCode()不是绝对可靠的。
**扩展**

1.阿里巴巴开发规范明确规定：

只要重写 equals，就必须重写 hashCode；

因为 Set 存储的是不重复的对象，依据 hashCode 和 equals 进行判断，所以 Set 存储的对象必须重写这两个方法；

如果自定义对象做为 Map 的键，那么必须重写 hashCode 和 equals；

String 重写了 hashCode 和 equals 方法，所以我们可以非常愉快地使用 String 对象作为 key 来使用；

2、什么时候需要重写？
一般的地方不需要重载hashCode，只有当类需要放在HashTable、HashMap、HashSet等等hash结构的集合时才会重载hashCode。

3、那么为什么要重载hashCode呢？
如果你重写了equals，比如说是基于对象的内容实现的，而保留hashCode的实现不变，那么很可能某两个对象明明是“相等”，而hashCode却不一样。

这样，当你用其中的一个作为键保存到hashMap、hasoTable或hashSet中，再以“相等的”找另一个作为键值去查找他们的时候，则根本找不到。

4、为什么equals()相等，hashCode就一定要相等，而hashCode相等，却不要求equals相等?

因为是按照hashCode来访问小内存块，所以hashCode必须相等。
HashMap获取一个对象是比较key的hashCode相等和equals为true。
之所以hashCode相等，却可以equal不等，就比如ObjectA和ObjectB他们都有属性name，那么hashCode都以name计算，所以hashCode一样，但是两个对象属于不同类型，所以equals为false。

5、为什么需要hashCode?

通过hashCode可以很快的查到小内存块。
通过hashCode比较比equals方法快，当get时先比较hashCode，如果hashCode不同，直接返回false。

#### 20.深拷贝和浅拷贝的区别是什么?

浅拷贝

**（1）、定义**

被复制对象的所有变量都含有与原来的对象相同的值，而所有的对其他对象的引用仍然指向原来的对象。即对象的浅拷贝会对“主”对象进行拷贝，但不会复制主对象里面的对象。”里面的对象“会在原来的对象和它的副本之间共享。

简而言之，浅拷贝仅仅复制所考虑的对象，而不复制它所引用的对象
**（2）、浅拷贝实例**

```java
package com.test;

public class ShallowCopy {
    public static void main(String[] args) throws CloneNotSupportedException {
        Teacher teacher = new Teacher();
        teacher.setName("riemann");
        teacher.setAge(27);

        Student2 student1 = new Student2();
        student1.setName("edgar");
        student1.setAge(18);
        student1.setTeacher(teacher);

        Student2 student2 = (Student2) student1.clone();
        System.out.println("拷贝后");
        System.out.println(student2.getName());
        System.out.println(student2.getAge());
        System.out.println(student2.getTeacher().getName());
        System.out.println(student2.getTeacher().getAge());

        System.out.println("修改老师的信息后——————");
        // 修改老师的信息
        teacher.setName("Games");
        System.out.println(student1.getTeacher().getName());
        System.out.println(student2.getTeacher().getName());

    }
}

class Teacher implements Cloneable {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

class Student2 implements Cloneable {
    private String name;
    private int age;
    private Teacher teacher;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Teacher getTeacher() {
        return teacher;
    }

    public void setTeacher(Teacher teacher) {
        this.teacher = teacher;
    }

    public Object clone() throws CloneNotSupportedException {
        Object object = super.clone();
        return object;
    }
}


```

输出结果:

```sqlite
拷贝后
edgar
18
riemann
27
修改老师的信息后——————
Games
Games
```

结果分析： 两个引用student1和student2指向不同的两个对象，但是两个引用student1和student2中的两个teacher引用指向的是同一个对象，所以说明是浅拷贝。

![image-20200413210910649](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200413210910649.png)

深拷贝

**（1）、定义**

深拷贝是一个整个独立的对象拷贝，深拷贝会拷贝所有的属性,并拷贝属性指向的动态分配的内存。当对象和它所引用的对象一起拷贝时即发生深拷贝。深拷贝相比于浅拷贝速度较慢并且花销较大。

简而言之，深拷贝把要复制的对象所引用的对象都复制了一遍。

**（2）、深拷贝实例**

```java
package com.test;

public class DeepCopy {
    public static void main(String[] args) throws CloneNotSupportedException {
        Teacher2 teacher = new Teacher2();
        teacher.setName("riemann");
        teacher.setAge(27);

        Student3 student1 = new Student3();
        student1.setName("edgar");
        student1.setAge(18);
        student1.setTeacher(teacher);

        Student3 student2 = (Student3) student1.clone();
        System.out.println("拷贝后");
        System.out.println(student2.getName());
        System.out.println(student2.getAge());
        System.out.println(student2.getTeacher().getName());
        System.out.println(student2.getTeacher().getAge());

        System.out.println("修改老师的信息后——————");
        // 修改老师的信息
        teacher.setName("Games");
        System.out.println(student1.getTeacher().getName());
        System.out.println(student2.getTeacher().getName());
    }
}

class Teacher2 implements Cloneable {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

class Student3 implements Cloneable {
    private String name;
    private int age;
    private Teacher2 teacher;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Teacher2 getTeacher() {
        return teacher;
    }

    public void setTeacher(Teacher2 teacher) {
        this.teacher = teacher;
    }

    public Object clone() throws CloneNotSupportedException {
        // 浅复制时：
        // Object object = super.clone();
        // return object;

        // 改为深复制：
        Student3 student = (Student3) super.clone();
        // 本来是浅复制，现在将Teacher对象复制一份并重新set进来
        student.setTeacher((Teacher2) student.getTeacher().clone());
        return student;

    }
}
```

输出结果：

```sqlite
拷贝后
edgar
18
riemann
27
修改老师的信息后——————
Games
riemann
```

结果分析：
两个引用student1和student2指向不同的两个对象，两个引用student1和student2中的两个teacher引用指向的是两个对象，但对teacher对象的修改只能影响student1对象,所以说是深拷贝。

![image-20200413211228832](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200413211228832.png)

#### 21.Java 中操作字符串都有哪些类？它们之间有什么区别？

String、StringBuffer、StringBuilder。

String 和 StringBuffer、StringBuilder 的区别在于 String 声明的是不可变的对象，每次操作都会生成新的 String 对象，然后将指针指向新的 String 对象，而 StringBuffer、StringBuilder 可以在原有对象的基础上进行操作，所以在经常改变字符串内容的情况下最好不要使用 String。

StringBuffer 和 StringBuilder 最大的区别在于，StringBuffer 是线程安全的，而 StringBuilder 是非线程安全的，但 StringBuilder 的性能却高于 StringBuffer，所以在单线程环境下推荐使用 StringBuilder，多线程环境下推荐使用 StringBuffer。

#### 22.String str="a"与 String str=new String("a")一样吗？

不一样，因为内存的分配方式不一样。

String str="a"; -> 常量池      String str=new String("a") -> 堆内存

#### 23.**抽象类能使用 final 修饰吗？**

不能。定义抽象类就是让其他类继承的，而 final修饰的类不能被继承。

#### 24.static关键字5连问

（1）抽象的（abstract）方法是否可同时是静态的（static）？

​    抽象方法将来是要被重写的，而静态方法是不能重写的，所以这个是错误的。

  (2)是否可以从一个静态（static）方法内部发出对非静态方法的调用？

​    不可以，静态方法只能访问静态成员，非静态方法的调用要先创建对象。 

   (3) static 可否用来修饰局部变量？ 

​    static 不允许用来修饰局部变量 

（4）内部类与静态内部类的区别？

静态内部类相对与外部类是独立存在的，在静态内部类中无法直接访问外部类中变量、方法。如果    要访问的话，必须要new一个外部类的对象，使用new出来的对象来访问。但是可以直接访问静态的变量、调用静态的方法；

普通内部类作为外部类一个成员而存在，在普通内部类中可以直接访问外部类属性，调用外部类的方法。

如果外部类要访问内部类的属性或者调用内部类的方法，必须要创建一个内部类的对象，使用该对象访问属性或者调用方法。

如果其他的类要访问普通内部类的属性或者调用普通内部类的方法，必须要在外部类中创建一个普通内部类的对象作为一个属性，外同类可以通过该属性调用普通内部类的方法或者访问普通内部类的属性

如果其他的类要访问静态内部类的属性或者调用静态内部类的方法，直接创建一个静态内部类对象即可。

（5）Java中是否可以覆盖(override) 一个private或者是static的方法？

Java中static方法不能被覆盖，因为方法覆盖是基于运行时动态绑定的，而static方法是编译时静态绑定的。static方法跟类的任何实例都不相关，所以概念上不适用。

#### 25. 重载（Overload）和重写（Override）的区别。重载的方法能否根据返回类型进行区分？

方法的重载和重写都是实现多态的方式，区别在于前者实现的是编译时的多态性，而后者实现的是运行时的多态性。重载发生在一个类中，同名的方法如果有不同的参数列表（参数类型不同、参数个数不同或者二者都不同）则视为重载；重写发生在子类与父类之间，重写要求子类被重写方法与父类被重写方法有相同的参数列表，有兼容的返回类型，比父类被重写方法更好访问，不能比父类被重写方法声明更多的异常（里氏代换原则）。重载对返回类型没有特殊的要求，不能根据返回类型进行区分。

#### 26.Java的四种引用

**1、强引用**

最普遍的一种引用方式，如String s = "abc"，变量s就是字符串“abc”的强引用，只要强引用存在，则垃圾回收器就不会回收这个对象。

**2、软引用（SoftReference）**

用于描述还有用但非必须的对象，如果内存足够，不回收，如果内存不足，则回收。一般用于实现内存敏感的高速缓存，软引用可以和引用队列ReferenceQueue联合使用，如果软引用的对象被垃圾回收，JVM就会把这个软引用加入到与之关联的引用队列中。

**3、弱引用（WeakReference）**

弱引用和软引用大致相同，弱引用与软引用的区别在于：只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。

**4、虚引用（PhantomReference）**

就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收器回收。 虚引用主要用来跟踪对象被垃圾回收器回收的活动。

**虚引用与软引用和弱引用的一个区别在于：**

虚引用必须和引用队列 （ReferenceQueue）联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。

#### 27.**Java 中Comparator 与Comparable 有什么不同？**

Comparable 接口用于定义对象的自然顺序，是排序接口，而 comparator 通常用于定义用户定制的顺序，是比较接口。我们如果需要控制某个类的次序，而该类本身不支持排序(即没有实现Comparable接口)，那么我们就可以建立一个“该类的比较器”来进行排序。Comparable 总是只有一个，但是可以有多个 comparator 来定义对象的顺序。

#### 28. Java 序列化,反序列化?

Java 序列化就是指将对象转换为字节序列的过程，反序列化是指将字节序列转换成目标对象的过程。

#### 29.什么情况需要Java序列化?

当 Java 对象需要在网络上传输 或者 持久化存储到文件中时。

#### 30.序列化的实现?

让类实现Serializable接口,标注该类对象是可被序列。

#### 31.如果某些数据不想序列化，如何处理?

在字段面前加  transient 关键字，例如:

```java
  transient private String phone;//不参与序列化
```

#### 32.**Java泛型和类型擦除？**

泛型即参数化类型，在创建集合时，指定集合元素的类型，此集合只能传入该类型的参数。

类型擦除：java编译器生成的字节码不包含泛型信息，所以在编译时擦除：1.泛型用最顶级父类替换；2.移除。

#### 33. 如何将字符串反转？

使用 StringBuilder 或者 stringBuffer 的 reverse() 方法。

示例代码：

```java
// StringBuffer reverse
StringBuffer stringBuffer = new StringBuffer();
stringBuffer. append("abcdefg");
System. out. println(stringBuffer. reverse()); // gfedcba
// StringBuilder reverse
StringBuilder stringBuilder = new StringBuilder();
stringBuilder. append("abcdefg");
System. out. println(stringBuilder. reverse()); // gfedcba
```

#### 34.String 类的常用方法都有那些？

- indexOf()：返回指定字符的索引。
- charAt()：返回指定索引处的字符。
- replace()：字符串替换。
- trim()：去除字符串两端空白。
- split()：分割字符串，返回一个分割后的字符串数组。
- getBytes()：返回字符串的 byte 类型数组。
- length()：返回字符串长度。
- toLowerCase()：将字符串转成小写字母。
- toUpperCase()：将字符串转成大写字符。
- substring()：截取字符串。
- equals()：字符串比较。

#### 35.抽象类必须要有抽象方法吗？

不需要，抽象类不一定非要有抽象方法。

示例代码：

```java
abstract class Cat {
    public static void sayHi() {
        System. out. println("hi~");
    }
}
```

上面代码，抽象类并没有抽象方法但完全可以正常运行。

#### 36. 普通类和抽象类有哪些区别？

- 普通类不能包含抽象方法，抽象类可以包含抽象方法。
- 抽象类不能直接实例化，普通类可以直接实例化。

#### 37.接口和抽象类有什么区别？

- 实现：抽象类的子类使用 extends 来继承；接口必须使用 implements 来实现接口。
- 构造函数：抽象类可以有构造函数；接口不能有。
- 实现数量：类可以实现很多个接口；但是只能继承一个抽象类。
- 访问修饰符：接口中的方法默认使用 public 修饰；抽象类中的方法可以是任意访问修饰符。

#### 38.以下代码中,s5`==`s2返回值是什么?

```
String s1="ab";
String s2="a"+"b";
String s3="a";
String s4="b";
String s5=s3+s4;
```

返回false.在编译过程中,编译器会将s2直接优化为"ab",将其放置在常量池当中;而s5则是被创建在堆区,相当于s5=new String(“ab”);

#### 39.String  中的 intern()

Stirng中的intern()是个Native方法,它会首先从常量池中查找是否存在该常量值的字符串,若不存在则先在常量池中创建,否则直接返回常量池已经存在的字符串的引用. 比如

```
 String s1="aa";
 String s2=s1.intern();
 System.out.print(s1==s2);
```


上述代码将返回true.因为在"aa"会在编译阶段确定下来,并放置字符串常量池中,因此最终s1和s2引用的是同一个字符串常量对象。

#### 40.什么是编译器常量?使用它有什么风险?

公共静态不可变,即public static final修饰的变量就是我们所说的编译期常量.这里的public可选的.实际上这些变量在编译时会被替换掉,因为编译器明确的能推断出这些变量的值(如果你熟悉C++,那么这里就相当于宏替换).

编译器常量虽然能够提升性能,但是也存在一定问题:你使用了一个内部的或第三方库中的公有编译时常量,但是这个值后面被其他人改变了,但是你的客户端没有重新编译,这意味着你仍然在使用被修改之前的常量值.

#### 41.3*0.1`==`0.3返回值是什么

false,因为有些浮点数不能完全精确的表示出来.

#### 42.a=a+b与a+=b有什么区别吗?

+=操作符会进行隐式自动类型转换,此处a+=b隐式的将加操作的结果类型强制转换为持有结果的类型,而a=a+b则不会自动进行类型转换.如：

```
byte a = 127;
byte b = 127;
b = a + b; // 报编译错误:cannot convert from int to byte
b += a; 
```

#### 43.this与super的区别

- super:它引用当前对象的直接父类中的成员（用来访问直接父类中被隐藏的父类中成员数据或函数，基类与派生类中有相同成员定义时如：super.变量名 super.成员函数据名（实参）
- this：它代表当前对象名（在程序中易产生二义性之处，应使用this来指明当前对象；如果函数的形参与类中的成员数据同名，这时需用this来指明成员变量名）
- super()和this()类似,区别是，super()在子类中调用父类的构造方法，this()在本类内调用本类的其它构造方法。
- super()和this()均需放在构造方法内第一行。
- 尽管可以用this调用一个构造器，但却不能调用两个。
- this和super不能同时出现在一个构造函数里面，因为this必然会调用其它的构造函数，其它的构造函数必然也会有super语句的存在，所以在同一个构造函数里面有相同的语句，就失去了语句的意义，编译器也不会通过。
- this()和super()都指的是对象，所以，均不可以在static环境中使用。包括：static变量,static方法，static语句块。
- 从本质上讲，this是一个指向本对象的指针, 然而super是一个Java关键字。

#### 44.static存在的主要意义

 static的主要意义是在于创建独立于具体对象的域变量或者方法。 **以致于即使没有创建对象，也能使用属性和调用方法** ！

static关键字还有一个比较关键的作用就是 **用来形成静态代码块以优化程序性能** 。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只会执行一次。

为什么说static块可以用来优化程序性能，是因为它的特性:只会在类加载的时候执行一次。因此，很多时候会将一些只需要进行一次的初始化操作都放在static代码块中进行。



#### 45.static的独特之处

1、被static修饰的变量或者方法是独立于该类的任何对象，也就是说，这些变量和方法 **不属于任何一个实例对象，而是被类的实例对象所共享** 。

怎么理解 “被类的实例对象所共享” 这句话呢？就是说，一个类的静态成员，它是属于大伙的【大伙指的是这个类的多个对象实例，我们都知道一个类可以创建多个实例！】，所有的类对象共享的，不像成员变量是自个的【自个指的是这个类的单个实例对象】…我觉得我已经讲的很通俗了，你明白了咩？

2、在该类被第一次加载的时候，就会去加载被static修饰的部分，而且只在类第一次使用时加载并进行初始化，注意这是第一次用就要初始化，后面根据需要是可以再次赋值的。

3、static变量值在类加载的时候分配空间，以后创建类对象的时候不会重新分配。赋值的话，是可以任意赋值的！

4、被static修饰的变量或者方法是优先于对象存在的，也就是说当一个类加载完毕之后，即便没有创建对象，也可以去访问。

#### 46.static应用场景

因为static是被类的实例对象所共享，因此如果 某个成员变量是被所有对象所共享的，那么这个成员变量就应该定义为静态变量 。

因此比较常见的static应用场景有：

1、修饰成员变量 

2、修饰成员方法

 3、静态代码块

 4、修饰类【只能修饰内部类也就是静态内部类】 

5、静态导包

#### 47.break ,continue ,return 的区别及作用

break 跳出总上一层循环，不再执行循环(结束当前的循环体)

continue 跳出本次循环，继续执行下次循环(结束正在执行的循环 进入下一个循环条件)

return 程序返回，不再执行下面的代码(结束当前的方法 直接返回)

#### 48.Java 中，如何跳出当前的多重嵌套循环

在Java中，要想跳出多重循环，可以在外面的循环语句前定义一个标号，然后在里层循环体的代码中使用带有标号的break 语句，即可跳出外层循环。例如：

```
public static void main(String[] args) {
    ok:
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 10; j++) {
            System.out.println("i=" + i + ",j=" + j);
            if (j == 5) {
                break ok;
            }

        }
    }
}
```

#### 49.在Java中定义一个不做事且没有参数的构造方法的作用

Java程序在执行子类的构造方法之前，如果没有用super()来调用父类特定的构造方法，则会调用父类中“没有参数的构造方法”。因此，如果父类中只定义了有参数的构造方法，而在子类的构造方法中又没有用super()来调用父类中特定的构造方法，则编译时将发生错误，因为Java程序在父类中找不到没有参数的构造方法可供执行。解决办法是在父类里加上一个不做事且没有参数的构造方法。

#### 50.在调用子类构造方法之前会先调用父类没有参数的构造方法，其目的是？

帮助子类做初始化工作。

#### 51.一个类的构造方法的作用是什么？若一个类没有声明构造方法，改程序能正确执行吗？为什么？

主要作用是完成对类对象的初始化工作。可以执行。因为一个类即使没有声明构造方法也会有默认的不带参数的构造方法。

#### 52.构造方法有哪些特性？

名字与类名相同；

没有返回值，但不能用void声明构造函数；

生成类的对象时自动执行，无需调用。

#### 53.静态变量和实例变量区别

静态变量： 静态变量由于不属于任何实例对象，属于类的，所以在内存中只会有一份，在类的加载过程中，JVM只为静态变量分配一次内存空间。

实例变量： 每次创建对象，都会为每个对象分配成员变量内存空间，实例变量是属于实例对象的，在内存中，创建几次对象，就有几份成员变量。

#### 54.静态变量与普通变量区别

static变量也称作静态变量，静态变量和非静态变量的区别是：静态变量被所有的对象所共享，在内存中只有一个副本，它当且仅当在类初次加载时会被初始化。而非静态变量是对象所拥有的，在创建对象的时候被初始化，存在多个副本，各个对象拥有的副本互不影响。

还有一点就是static成员变量的初始化顺序按照定义的顺序进行初始化。

#### 55.静态方法和实例方法有何不同？

静态方法和实例方法的区别主要体现在两个方面：

1. 在外部调用静态方法时，可以使用"类名.方法名"的方式，也可以使用"对象名.方法名"的方式。而实例方法只有后面这种方式。也就是说，调用静态方法可以无需创建对象。
2. 静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），而不允许访问实例成员变量和实例方法；实例方法则无此限制

#### 56.在一个静态方法内调用一个非静态成员为什么是非法的？

由于静态方法可以不通过对象进行调用，因此在静态方法里，不能调用其他非静态变量，也不可以访问非静态变量成员。



#### 参考:

https://blog.csdn.net/qq_41701956/article/details/86773940

https://zhuanlan.zhihu.com/p/94095050

https://www.cnblogs.com/Java-JJ/p/12625888.html