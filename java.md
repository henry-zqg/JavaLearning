##			__JAVA语言学习笔记__

**1. 面向对象特性：抽象，封装，继承，多态**

多态的概念：

1. 基本类型的多态，自动拆装箱
2. 方法的多态：重载，重写
3. 类或接口的多态：父类的引用指向子类的对象

##### 2.JAVA只有值传递

Java语言的方法调用只支持参数的**值传递**, 当一个对象实例作为一个参数被传递到方法中时，**参数的值就是对该对象的引用**。对象的属性可以在被调用过程中被改变，但对对象引用的改变是不会影响到调用者的

Integer.parseInt(string)  java将字符串转为整型

executeQuery

##### 3. 重载的特征


重载的方法，实际是完全不同的方法，只是名称相同而已!

构成方法重载的条件：

1. 不同的含义：形参类型、形参个数、形参顺序不同

2. 只有返回值不同不构成方法的重载

// 每一个源文件必须有且只有一个public class，并且类名和文件名保持一致！

##### 4.JVM相关知识

* **JVM**

Java 虚拟机（JVM）是运行 Java 字节码的虚拟机。JVM 有针对不同系统的特定实现（Windows，Linux，macOS），目的是使用相同的字节码，它们都会给出相同的结果。

java到可执行的程序有一下三部：

![图片](https://uploader.shimo.im/f/Npa9VfvHvyQFMaNd.png!thumbnail)

* **JDK 和 JRE**

​      JDK 是 Java Development Kit，它是功能齐全的 Java SDK。它拥有 JRE 所拥有的一切，还有编译器（javac）和工具（如 javadoc 和 jdb）。它能够创建和编译程序。

​       JRE 是 Java 运行时环境。它是运行已编译 Java 程序所需的所有内容的集合，包括 Java 虚拟机（JVM），Java 类库，java 命令和其他的一些基础构件。但是，它不能用于创建新程序。

![JDK关系图](https://uploader.shimo.im/f/l8T7MKowhas4H3l8.png!thumbnail "JDK、JRE、JVM关系图")

##### 4.1 JVM内存区域

Java虚拟机的内存可以分为三个区域：**栈stack、堆heap、方法区method area, 本地方法区，程序计数器**

**4.1.1 栈的特点**：（存储的内容， 是否被共享， 是否连续的存储空间 ）：

​	存储内容：

* 栈描述的是方法执行的内存模型。每个方法被调用都会创建一个栈帧(存储局部变量、操作数、方法出口，实际参数等)

与线程关系：

* JVM为每个线程创建一个栈，用于存放该线程执行方法的信息(实际参数、局部变量等)

* 栈属于线程私有，不能实现线程间的共享!

​	特性：

栈的存储特性是“先进后出，后进先出”

* 栈是由系统自动分配，**速度快**!栈是一个连续的内存空间!

**4.1.2 堆的特点**

存储内容：

* 堆用于存储创建好的对象和数组(数组也是对象)

* JVM只有一个堆，被所有线程共享

* 堆是一个不连续的内存空间，分配灵活，**速度慢**!

**4.1.3 方法区(又叫静态区)特点如下：**

+ JVM只有一个方法区，被所有线程共享!

* 方法区实际也是堆，只是用于存储类、常量相关的信息!

* 用来存放程序中永远是不变或唯一的内容。(类信息【Class对象】、静态变量、字符串常量等)

**4.1.4 程序计数器：**

存储字节码的行号，JVM中**唯一无内存溢出**

##### 4.2 垃圾回收机制：分代收集法

###### **4.2.1 确定垃圾的方法**

* 引用计数法  
* 可达性分析

###### 4.2.2 常用垃圾回收算法

新生代 常用复制算法，老年代使用标记清楚算法

Java堆 = 年老代 + 年轻代 + 永久代

(空间大小比例一般是3:1)

年轻代 = Eden区 + From Space区 + To Space区

(空间大小比例一般是8:1:1)

任何一种垃圾回收算法一般要做两件基本事情：

1. 发现无用的对象

2. 回收无用对象占用的内存空间。

垃圾回收相关算法：引用可达法

垃圾回收过程：

1、新创建的对象，绝大多数都会存储在Eden中，

2、当Eden满了（达到一定比例）不能创建新对象，则触发垃圾回收（GC），将无用对象清理掉，然后剩余对象复制到某个Survivor中，如S1，同时清空Eden区

3、当Eden区再次满了，会将S1中的不能清空的对象存到另外一个Survivor中，如S2，同时将Eden区中的不能清空的对象，也复制到S1中，保证Eden和S1，均被清空。

4、重复多次(默认15次)Survivor中没有被清理的对象，则会复制到老年代Old(Tenured)区中，

5、当Old区满了，则会触发一个一次完整地垃圾回收（FullGC），之前新生代的垃圾回收称为（minorGC）

![图片](https://uploader.shimo.im/f/40QPr3KvC8Iv2wHx.png!thumbnail)

##### 5 this最常的用法：

1.  在程序中产生二义性之处，应使用this来指明当前对象;普通方法中，this总是指向调用该方法的对象。构造方法中，this总是指向正要初始化的对象。

2.  使用this关键字调用重载的构造方法，避免相同的初始化代码。但只能在构造方法中用，并且必须位于构造方法的第一句。

3.  this不能用于static方法中。

java只有一个父类  默认父类object

instanceof是二元运算符，左边是对象，右边是类。判断是否为当前的实例

数组声明后只是分配了空间，还需初始化；声明的时候并没有实例化任何对象，只有在实例化数组对象时，JVM才分配空间，这时才与长度有关。

构造方法第一句中无super则会默认调用super

##### **6 抽象类和接口和类：**

**抽象类：**

* 含有抽象方法的类(至少有一个)

* 不能实例化，只能用来被继承

* 抽象方法必须被子类实现

**接口：**

* 常量：接口中的属性只能是常量，总是：public static final 修饰。不写也是。

* 方法：接口中的方法只能是：public abstract。 省略的话，也是public abstract。

* 一个类实现了接口，必须实现接口中的方法，并且这些方法只能是public的（Default方法无需实现）。

java类单继承 接口可以多继承（接口可以继承多个类，继承多个接口)如：

interface A extends interface1, interface2, interface3

**接口与抽象方法的区别：**

相同点：

（1）都不能被实例化

（2）接口的实现类或抽象类的子类都只有实现了接口或抽象类中的方法后才能实例化。不同点：

（1）接口只有定义，不能有方法的实现，java 1.8中可以定义default方法体，而抽象类可以有定义与实现，方法可在抽象类中实现。

（2）实现接口的关键字为implements，继承抽象类的关键字为extends。一个类可以实现多个接口，但一个类只能继承一个抽象类。所以，使用接口可以间接地实现多重继承。

（3）接口强调特定功能的实现，而抽象类强调所属关系。

（4）接口成员变量默认为public static final，必须赋初值，不能被修改；其所有的成员方法都是public、abstract的。抽象类中成员变量默认default，可在子类中被重新定义，也可被重新赋值；抽象方法被abstract修饰，不能被private、static、synchronized和native等修饰，必须以分号结尾，不带花括号。

（5）接口被用于常用的功能，便于日后维护和添加删除，而抽象类更倾向于充当公共类的角色，不适用于日后重新对立面的代码修改。功能需要累积时用抽象类，不需要累积时用接口。字节流与字符流：

**抽象类必须要有抽象方法吗？**

不需要，抽象类不一定非要有抽象方法。

abstract class Cat {

public static void sayHi() {

System.out.println("hi~");

}

**6.1 类的访问控制**

| 访问控制符 | 当前类 | 同一个包 | 子类  | 不同包 |
| :--------: | :----: | :------: | :---: | :----: |
|   public   | **√**  |  **√**   | **√** | **√**  |
| protected  | **√**  |  **√**   | **√** |        |
|  default   | **√**  |  **√**   |       |        |
|  private   | **√**  |          |       |        |

**6.2 类**

getClass（）是一个类的实例所具备的方法，而class（）方法是一个类的方法。

对象去重要重写equals方法 和 hashCode方法

编译型 or 解释型不是一门语言的特型，而是语言具体实现的特性，针对于实现过程

**6.3 Java 四种引用**

这4种级别由高到低依次为:强引用、软引用、弱引用和虚引用。

1. <u>强引用(StrongReference)</u>

强引用是使用最普遍的引用。如果一个对象具有强引用,那垃圾回收器绝 不会回收它。当内存空间不足, Java 虚拟机宁愿抛出 OutOfMemoryError 错误,使程序异常终止,也不会靠随意回收具有强引用的对象来解决内存不足的问 题。 ps:强引用其实也就是我们平时 A a = new A()这个意思。

2. 软引用(SoftReference)

如果一个对象只具有软引用, 则内存空间足够, 垃圾回收器就不会回收它; 如果内存空间不足了,就会回收这些对象的内存。只要垃圾回收器没有回收它, 该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存(下文给出示例)。

软引用可以和一个引用队列(ReferenceQueue)联合使用,如果软引用所 引用的对象被垃圾回收器回收,Java 虚拟机就会把这个软引用加入到与之关联 的引用队列中。

​	示例:实现学生信息查询操作时有两套数据操作的方案。

​	一、将得到的信息存放在内存中, 后续查询则直接读取内存信息(优点: 读取速度快; 缺点: 内存空间一直被占, 若资源访问量不高,则浪费内存空间)。

​	二、每次查询均从数据库读取, 然后填充到 TO 返回。(优点: 内存空间 将被 GC 回收, 不会一直被占用;缺点: 在 GC 发生之前已有的 TO 依然存在, 但还是执行了一次数据库查询,浪费 IO)。可以通过软引用来解决。

3. 弱引用(WeakReference)

弱引用与软引用的区别在于: 只具有弱引用的对象拥有更短暂的生命周期。 在垃圾回收器线程扫描它所管辖的内存区域的过程中, 一旦发现了只具有弱引 用的对象, 不管当前内存空间足够与否, 都会回收它的内存。不过,由于垃圾 回收器是一个优先级很低的线程, 因此不一定会很快发现那些只具有弱引用的对象。

弱引用可以和一个引用队列(ReferenceQueue)联合使用,如果弱引用所 引用的对象被垃圾回收,Java 虚拟机就会把这个弱引用加入到与之关联的引用 队列中。

4. 虚引用(PhantomReference)

“虚引用”顾名思义,就是形同虚设,与其他几种引用都不同,虚引用并不会 决定对象的生命周期。如果一个对象仅持有虚引用,那么它就和没有任何引用 一样,在任何时候都可能被垃圾回收器回收。

##### 7. 内部类

内部类只是一个编译时概念，一旦我们编译成功，就会成为完全不同的两个类。

广泛意义上的内部类一般来说包括这四种：成员内部类、局部内部类、匿名内部类和静态内部类。

1. 成员内部类

   成员内部类可以无条件访问外部类的所有成员属性和成员方法（包括private成员和静态成员）。

成员内部类是依附外部类而存在的，也就是说，如果要创建成员内部类的对象，前提是必须存在一个外部类的对象。

```java
public class Test {
    public static void main(String[] args)  {
        //第一种方式：
        Outter outter = new Outter();
        Outter.Inner inner = outter.new Inner();  //必须通过Outter对象来创建

        //第二种方式：
        Outter.Inner inner1 = outter.getInnerInstance();
    }
}
```

##### 8. 数据类型

​	8.1 java Integer :缓存在{-128~127}，就是系统初始的时候，创建了一个在该范围之间的一个缓存数组

如果在该范围，就直接那缓存的；否则重新创建新对象。

  8.2  **String StringBuilder StringBuffer**

* String 类对象代表不可变对象，简单的来说：String 类中使用 final 关键字修饰字符数组来保存字符串，private final char value[]，所以 String 对象是不可变的（JDK1.8之后中改为private final byte[] value)。

* StringBuilder 可变对象，效率高

* StringBuffer线程安全

Java-String intern（）方法   此方法返回字符串对象的规范表示形式。因此，对于任何两个字符串s和t，当且仅当s.equals（t）为true时，s.intern（）== t.intern（）为true。

实际上字符串只有字面量是共享的，而+或substring等操作得到的字符串并不共享。

空串是”“长度为0的字符串，null串表示没有任何对象与该变量进行关联。

##### 9. 泛型 （T,K,V,E）

泛型E像一个占位符一样表示“未知的某个数据类型”，我们在真正调用的时候传入这个“数据类型”

##### 10. 容器

容器中所有元素的比较操作都是用equal方法

**10.1 如何选用ArrayList、LinkedList、Vector?**

1. 需要线程安全时，用Vector。

2. 不存在线程安全问题时，并且查找较多用ArrayList(一般使用它)。

3. 不存在线程安全问题时，增加或删除元素较多用LinkedList

**10.2 Java中equals和==的区别**

java中的数据类型，可分为两类：

1. **8种基本数据类型**，也称原始数据类型。byte,short,char,int,long,float,double,boolean

他们之间的比较，应用双等号（==）,比较的是他们的值。

2. **复合数据类型(类) **

​     当他们用（==）进行比较的时候，比较的是他们在内存中的存放地址，所以，除非是同一个new出来的对象，他们的比较后的结果为true，否则比较后结果为false。 JAVA当中所有的类都是继承于Object这个基类的，在Object中的基类中定义了一个equals的方法，这个方法的初始行为是比较对象的内存地址，但在一些类库当中这个方法被覆盖掉了，如String,Integer,Date在这些类当中equals有其自身的实现，而不再是比较类在堆内存中的存放地址了。

​     对于复合数据类型之间进行equals比较，在没有覆写equals方法的情况下，他们之间的比较还是基于他们在内存中的存放位置的地址值的，因为Object的equals方法也是用双等号（==）进行比较的，所以比较后的结果跟双等号（==）的结果相同。

因为toString方法是Object里面已经有了的方法，而所有类都是继承Object，所以“所有对象都有这个方法”。

它通常只是为了方便输出，比如System.out.println(xx)，括号里面的“xx”如果不是String类型的话，就自动调用xx的toString()方法

**10.3 Collection 接口**

![图片](https://uploader.shimo.im/f/CrfKblEZAOMRGqzD.png!thumbnail)

1. Collections 与 Arrays为 包装类
2. collection 为接口 <font color="red">set list map 均为接口 </font> 

* set：无顺序 不可重复

* list:   有序    可重复

2. Treeset 、TreeMap等实现自主排序， 要实现comparable接口

如果遇到遍历容器时，判断删除元素的情况，使用迭代器遍历! collection.iterator()返回一个迭代器

3. HashMap

   * HashMap可以把null 做为key或者value,而HashTable不可以
   * HashMap不是线程安全的

**正确使用`Map`必须保证：**

* 作为`key`的对象必须正确覆写`equals()`方法，相等的两个`key`实例调用`equals()`必须返回`true`；

* 作为`key`的对象还必须正确覆写`hashCode()`方法，且`hashCode()`方法要严格遵循以下规范：

4. EnumMap

如果作为key的对象是`enum`类型，那么，还可以使用Java集合库提供的一种`EnumMap`，它在内部以一个非常紧凑的数组存储value，并且根据`enum`类型的key直接定位到内部数组的索引

容器中线程安全的如：vectory,hashtable,stack 。非线程安全的如：hashmap,arrylist等。

对于原定义非线程的容器如：hashmap,arraylist可以使用Collections中的

HashMap的插入、删除更快 TreeMap的遍历更快

ArrayList与LinkedList的区别：

数据结构：ArrayList是动态数组的数据实现，而LinkedList是双向链表的数据结构实现。

访问效率：ArrayList > LinkedList

增加和删除效率：非首尾， LinkedList比ArrayList效率高

4. Queue队列的使用方法（LinkedList）

* offer，add 区别：

  一些队列有大小限制，因此如果想在一个满的队列中加入一个新项，多出的项就会被拒绝。

  这时新的 offer 方法就可以起作用了。它不是对调用 add() 方法抛出一个 unchecked 异常，而只是得到由 offer() 返回的 false。

* poll，remove 区别：**

  remove() 和 poll() 方法都是从队列中删除第一个元素。remove() 的行为与 Collection 接口的版本相似， 但是新的 poll() 方法在用空集合调用时不是抛出异常，只是返回 null。因此新的方法更适合容易出现异常条件的情况。

* peek，element区别：**

  element() 和 peek() 用于在队列的头部查询元素。与 remove() 方法类似，在队列为空时， element() 抛出一个异常，而 peek() 返回 null。
  
  4. 1 DeQueue (双端队列)

Docker三个概念：1.Repository 2.Image  3. Container

5. <font color="red">我们发现`LinkedList`真是一个全能选手，它即是`List`，又是`Queue`，还是`Deque`</font>

##### 11. I/O流

![图片](https://uploader.shimo.im/f/cozHVYMrNOgNNtfr.png!thumbnail)



UTF-8 一个中文占三个字节

I/O流的操作步骤：

a.选择源   b.选择流   c.操作   d.释放

装饰器设计模式：

a.抽象组件：需要装饰的抽象对象

b.具体组件：需要装饰的对象

c.抽象装饰类：包含了对抽象组件的引用以及装饰着共有的方法

d.具体装饰类：被装饰的对象

Interface中的方法，在编译之后生成 class 文件时，都会自动加上 public abstract

**java中提供了专用于输入输出功能的包Java.io,其中包括：**

InputStream,OutputStream,Reader,Writer

InputStream 和OutputStream,两个是为字节流设计的,主要用来处理字节或二进制对象,

Reader和 Writer.两个是为字符流（一个字符占两个字节）设计的,主要用来处理字符或字符串.

switch(var)

case var1:

int value = 1;

case var2:

sout(value)

在case中声明的value为同一个switch可见，缩小作用域需要加括号

##### 11. 多线程

**11.1 多线程的方法** 

* Yield方法礼让，返回就绪态，cpu重新调度  静态方法
* Join “插队线程” 合并线程 待此线程执行完成后，其他才可执行 对象方法。如：

```java
t.start();
t.join();
System.out.println("2");
//输出结果 1， 2
//t插到主线程之前执行完毕，join可以用来让线程顺序执行 
```

守护线程：是为用户线程服务 jvm不用等待守护线程完毕

默认 jvm等待用户线程执行完毕后停止

**11.2 锁**

Character是char的包装类，注意它是一个类(同Integer 和int)synchornized  锁的是不变的对象   尽可能锁定合理的范围 多用同步块 少用同步方法

PV操作：wait 阻塞同时会释放锁，sleep不释放锁   notify

volatile:**1.保证数据可见性（及时写回内存） 2.严禁指令重排**   

 用于保证数据的同步性，及时写回主内存，可见性quarz的使用在MVC模式中，应用程序被划分成了模型（Model）、视图（View）和控制器（Controller）三个部分。其中，模型部分包含了应用程序的业务逻辑和业务数据；视图部分封装了应用程序的输出形式，也就是通常所说的页面或者是界面；而控制器部分负责协调模型和视图，根据用户请求来选择要调用哪个模型来处理业务，以及最终由哪个视图为用户做出应答。java 中数组也是对象，直等会指向同一个地址，使用静态方法拷贝较好

int[] a ={1,2,3};

int[] b = new int[3];

System.arraycopy(a,0,b,0,3);

b.分析上下文环境 起点  1.构造器 哪里调用就是哪里 找线程体

2.run方法 线程本身的

inheritableThreadlocal 继承上下文环境，拷贝一份数据给子线程可重入锁：锁可以延续使用 +计数器容器类总结：



final关键字修饰的类无法被继承，抽象类不能用final修饰

并行（parallel) : 多个处理器同时处理多个任务,同一时刻多个进程同时发生

并发（concurrency): 多个任务在同一个cpu上交替进行，同一时间间隔内多个进程发生java设计模式

* 

##### 12. JavaWeb

**13.1 Get方法与Post方法的区别：**

a.get请求提交的数据会在地址栏显示出来，而post请求不会再在地址栏显示出来(放在http报文)

b. 传输数据的大小： Get请求由于浏览器对地址栏长度的限制而受限。而Post请求不会因为地址长度的限制

c.安全性forward():服务器的转向，不在地址栏显示，是一次请求中完成

**13.2 Cookie和Session**

redirect():客户端的跳转，在地址栏显示， 是重新发起请求**session与cookie的区别：**

session在存储在服务器端， cookie存储在客户浏览器。cookie不安全。

seesion占空间。

cookie存储在购物车中，session 存储登录信息HashMap 与HashTable的区别：

##### 13.异常

**14.1 异常的分类**

exception分为运行期间异常，和非运行期间异常。运行期间异常程序抛出，非运行期间异常需由程序员手动处理

Throwable

/                \

Error             **Exception**

/                          /                   \

OutOfMemoryError**CheckedException    RuntimeException**

NoClassDeFoundError                 /                   \

StackOverflowErrorxxxxxx            ArithmeticException

在 Java 5 中提供了变长参数，允许在调用方法时传入不定长度的参数。变长参数是 Java 的一个语法糖，本质上还是基于数组的实现：

```plain
void foo(String... args);
void foo(String[] args);
```

volatile不能不能保证原子性吧

静态方法由于随着类的加载而加载，不能访问类的泛型（因为在创建对象的时候才确定），因此必须定义自己的泛型类型。例如：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { ... }
    public T getLast() { ... }
    // 静态泛型方法应该使用其他类型区分:
    public static <K> Pair<K> create(K first, K last) {
        return new Pair<K>(first, last);
    }
}
```

**内存泄露的解决方案(重要!!):**
1、避免在循环中创建对象。
2、尽早释放无用对象的引用。(最基本的建议)
3、尽量少用静态变量,因为静态变量存放在永久代(方法区),永久代基本不参与垃圾回收。
4、使用字符串处理,避免使用 String,应大量使用 StringBuffer,每一个 String对象都得独立占用内存一块区域。

##### 14. Git的使用

<img src="https://uploader.shimo.im/f/uVOAqGneooEvlihJ.png!thumbnail" alt="图片" style="zoom:50%;" />

<img src="https://uploader.shimo.im/f/98kZuiAHxkLMXruS.png!thumbnail" alt="图片" style="zoom:50%;" />

##### 15. 设计模式

**12.1 单例模式：**

（饿汉模式）套路，在多线程环境下，对外存在一个对象

* 构造器私有化 ---->避免外部new构造器

* 提供私有的静态属性---->存储对象的地址

* 提供公共的静态方法---->获取属性ThreadLocal  a.每个线程自身变量的存储空间，更改不影响其他线程

  懒汉模式

​	直接加载 

单例模式：某一个类只有一个实例，向其他类提供一个公共的访问点

a. 饿汉式 ：类初始化时，立即加载这个对象。加载类时，天然的线程安全的

b. 懒汉式： 类初始化时， 不立即加载这个对象。延时加载，方法同步，调用效率低静态代码块在类被加载的时候就运行了，而且只运行一次，并且优先于各种代码块以及构造函数。如果一个类中有多个静态代码块，会按照书写顺序依次执行。后面在比较的时候会通过具体实例来证明

**12.2 装饰器设计模式**

* 抽象组件：需要装饰的抽象对象

* 具体组件：需要装饰的对象

* 抽象装饰类：包含了对抽象组件的引用以及装饰着共有的方法

* 具体装饰类：被装饰的对象

good algorithms are poeptry of computation