##1、进程和线程

1. 地址空间和其他资源：进程间相互独立，进程中包括多个线程，线程间共享进程资源，某进程内的线程在其他进程内不可见
2. 通信：进程间通信通过IPC机制，线程间通信通过数据段（如：全局变量）的读写，需要进程同步和互斥手段的辅助，以保证数据的一致性 
3. 调度和切换：进程是资源分配单位，线程是cpu调度单位，跟cpu真正打交道的是线程，线程上下文切换比进程上下文切换要快得多
		
##2、内存溢出和内存泄漏
- 内存溢出：指程序在申请内存时，没有足够的空间供其使用
- 内存泄漏：指程序分配出去的内存不再使用，无法进行回收

##3、面向对象和面向过程

####1、面向过程

- 优点：性能比面向对象高，因为类的调用需要实例化，开销比较大
- 缺点：相对于面向对象，不易维护、不易复用、不易拓展

####2、面向对象

- 优点：易维护、易复用、易拓展，由于面向对象有封装、继承、多态的特性，可以设计出低耦合的程序
- 缺点：性能比面向过程低

##4、Java的四个基本特性

1. 抽象：把现实生活某一类东西提取出来，成为该类东西的共有特性。抽象一般分为数据抽象和过程抽象，数据抽象是对象的属性，过程抽象是对象的行为特征
2. 封装：把客观事物进行封装成抽象类，该类的数据和方法只让可信的类操作，对不可信的类隐藏。封装分为属性的封装和方法的封装
3. 继承：从已有的类中派生出新的类，新的类能吸收已有类的数据属性和行为，并能扩展新的能力
4. 多态：同一个行为具有多个不同表现形式或形态的能力，多态的前提是类与类之间必须存在关系，要么继承，要么实现

##5、抽象类和接口的区别

1. 抽象类和接口分别给出了不同的语法定义（包括方法体、成员变量修饰符、方法修饰符）
2. 抽象是对类的抽象，接口是对行为的抽象
3. 抽象所体现的是继承关系，是一种”is-a”的关系，接口仅仅实现接口定义的契约，是一种”like-a”的关系
4. 抽象是自底向上抽象的，接口是自顶向下设计出来的

##6、自动装箱与拆箱

Java采用了自动装箱和拆箱机制，节省了常用数值的内存开销和创建对象的开销，提高了效率

1. 装箱：将基本数据类型包装成它们的引用类型
2. 拆箱：将包装类型转换成基本数据类型

##7、序列化与反序列化

对象的序列化：是把对象转换成字节序列的过程 
对象的反序列化：是把字节序列恢复为对象的过程

对象序列化的主要用途：

1. 可以将字节序列永久的保存在硬盘中，通常放在文件中
2. 可以在网络上传送字节序列
3. 两个线程在进行远程通信时，彼此可以发送各种类型。发送方需要把这个Java对象转换为字节序列，接收方则需要把字节序列再恢复为Java对象

##8、编译和运行

1. 编译时和运行时

- 编译时：将Java文件编译成.class文件的过程，不涉及到内存的分配
- 运行时：将虚拟机执行.class文件的过程，涉及到内存的分配

2. 编译时类型和运行时类型

- 编译时类型：由声明该变量时使用的类型决定
- 运行时类型：由实际赋给该变量的对象决定

3. 编译时和运行时的动态绑定

- 在编译时，调用的是声明类型的成员方法
- 在运行时，调用的是实际类型的成员方法
- 对于调用引用实例的成员变量，无论是编译还是运行时，均是编译时类型的成员变量

##9、GC简述

当程序员创建对象时，GC就开始监控这个对象的地址、大小以及使用情况。通常，GC采用有向图的方式记录和管理堆（heap）中的所有对象，通过这种方式确定哪些对象是”可达的”，哪些对象是”不可达的”。当GC确定一些对象为”不可达”时，GC就有责任回收这些内存空间。但是，为了保证 GC 能够在不同平台实现的问题，Java规范对GC的很多行为都没有进行严格的规定

##10、serialVersionUID简述

对于实现java.io.Serializable接口的实体类来说，往往都会手动声明serialVersionUID，因为只要你实现了序列化，java自己就会默认给实体类加上一个serialVersionUID。java默认添加的serialVersionUID是会根据实体类的成员（成员变量，成员方法）变化而变化。当我们把实体类序列化到本地后，如果实体类的成员发生了变化，默认添加的serialVersionUID就会发生变化。此时硬盘上序列化对象的serialVersionUID与实体类中的serialVersionUID对不上，就会反序列化失败爆出异常。所以，通常对于实现了SerialVersionUID接口的实体类来说，都会手动声明serialVersionUID

##11、Java中4种引用类型

- StrongReference（强引用）：从不回收，对象一直存在，当JVM停止的时候才被终止
- SoftReference（软引用）：可以和引用队列（ReferenceQueue）联合使用，当内存不足时被终止
- WeakReference（弱引用）：可以和引用队列（ReferenceQueue）联合使用，当内存不足时，触发GC后被终止
- PhantomReference（虚引用）：必须和引用队列（ReferenceQueue）联合使用，随时会被回收，触发GC后被终止
SoftReference和WeakReference的区别：

- WeakReference的生命周期比SoftReference的生命周期短
- SoftReference多用于图片的缓存，而WeakReference多用于内存泄漏的解决
- WeakReference可以通过手动GC进行清除

#字符串相关

##1、String、StringBuffer和StringBuilder

1. 可变性

- String类是用字符数组保存字符串，即private final char value[]，所以String对象不可变
- StringBuffer类与StringBuilder类都是继承AbstractStringBuilder类，AbstractStringBuilder是用字符数组保存字符串，即char value[]，所以StringBuffer与StringBuilder对象是可变的
2. 线程安全性

- String类对象不可变，所以理解为常量，线程安全
- StringBuffer类对方法加了同步锁，线程安全
- StringBuilder类对方法未加同步锁，线程不安全
3. 性能

- String类进行改变的时候，都会产生新的String对象，然后将指针指向新的String对象
- StringBuffer进行改变的时候，都会复用自身对象，性能比String高
- StringBuilder进行改变的时候，都会复用自身对象，相比StringBuffer能获得10%~15%左右的性能提升，但是得承担多线程的不安全的风险

##2、构造器Constructor是否可被override

1. 构造器不能被重写
2. 构造器不能被static修饰
3. 构造器只能用private、protected、public修饰
4. 构造器不能有返回值

##3、String类是否可以继承

String类由final类，故不可以继承，凡是由final修饰的类都不能继承

#集合相关
可查看我的博客，按顺序阅读，需要您具备数据结构基础，且基于JDK1.7

- [Java基础——HashMap源码分析](https://blog.csdn.net/qq_30379689/article/details/72582279)
- [Java基础——HashSet源码分析](https://blog.csdn.net/qq_30379689/article/details/72599197)
- [Java基础——HashTable源码分析](https://blog.csdn.net/qq_30379689/article/details/72618060)
- [Java基础——LinkedHashMap源码分析](https://blog.csdn.net/qq_30379689/article/details/72637922)
- [Java基础——LinkedHashSet源码分析](https://blog.csdn.net/qq_30379689/article/details/72677427)
- [Java基础——ArrayList源码分析](https://blog.csdn.net/qq_30379689/article/details/72725792)
- [Java基础——LinkedList源码分析](https://blog.csdn.net/qq_30379689/article/details/72764566)
- [Java基础——ConcurrentHashMap源码分析](https://blog.csdn.net/qq_30379689/article/details/72775356)
- [Java基础——Vector源码分析](https://blog.csdn.net/qq_30379689/article/details/72792445)

###1、TreeMap和HashMap

TreeMap|HashMap
---|:---
有序的|	无序的
不允许null|	允许null
实现Comparable接口|	不实现Comparable接口
底层是红黑二叉树，线程不同步|	底层是哈希表，线程不同步

###2、TreeSet和HashSet

TreeSet|HashSet
---|:---
有序的|无序的
不允许null|允许null
底层是红黑二叉树，线程不同步|底层是哈希表，线程不同步
TreeSet是通过TreeMap实现的，用的只是Map的key|HashSet是通过HashMap实现的，用的只是Map的key

###3、Collection和Collections

- Collection：是一个集合接口，它提供了对集合对象进行基本操作的通用接口方法
- Collections：是一个集合的工具类，它包含有各种有关集合操作的静态方法

###4、hashCode和equals

equals相等，hashCode必相等；hashCode相等，equals不一定相等

#错误相关


###1、Error和Exception

Error类和Exception类的父类都是throwable类

- Error：一般指与虚拟机相关的问题，如系统崩溃，虚拟机错误，内存空间不足，方法调用栈溢等。对于这类错误导致的应用程序中断，仅靠程序本身无法恢复和和预防，遇到这样的错误，建议让程序终止
- Exception：表示程序可以处理的异常，可以捕获且可能恢复。遇到这类异常，应该尽可能处理异常，使程序恢复运行，而不应该随意终止异常

###2、Unchecked Exception和Checked Exception

1. Unchecked Exception

- 指的是不可被控制的异常，或称运行时异常，主要体现在程序的瑕疵或逻辑错误，并且在运行时无法恢复
- 包括Error与RuntimeException及其子类，如：OutOfMemoryError，IllegalArgumentException， NullPointerException，IllegalStateException，IndexOutOfBoundsException等
- 语法上不需要声明抛出异常也可以编译通过

2. Checked Exception

- 指的是可被控制的异常，或称非运行时异常
- 除了Error和RuntimeException及其子类之外，如：ClassNotFoundException, NamingException, ServletException, SQLException, IOException等
- 需要try catch处理或throws声明抛出异常

#数据相关

##1、xml解析方式

- Dom解析：将XML文件的所有内容读取到内存中（内存的消耗比较大），然后允许您使用DOM API遍历XML树、检索所需的数据
- Sax解析：Sax是一个解析速度快并且占用内存少的xml解析器，Sax解析XML文件采用的是事件驱动，它并不需要解析完整个文档，而是按内容顺序解析文档的过程
- Pull解析：Pull解析器的运行方式与 Sax 解析器相似。它提供了类似的事件，可以使用一个switch对感兴趣的事件进行处理
- [Android基础——XML数据的三种解析方式](https://blog.csdn.net/qq_30379689/article/details/53170075)

#线程相关

##1、线程实现的方式

继承Thread类、实现Runnable接口、使用ExecutorService、Callable、Future实现有返回结果的线程

##2、如何停止一个线程

1. 创建一个标识（flag），当线程完成你所需要的工作后，可以将标识设置为退出标识
2. 使用Thread的stop()方法，这种方法可以强行停止线程，不过已经过期了，因为其在停止的时候可能会导致数据的紊乱
3. 使用Thread的interrupt()方法和Thread的interrupted()方法，两者配合break退出循环，或者return来停止线程，有点类似标识（flag）
4. （推荐）当我们想要停止线程的时候，可以使用try-catch语句，在try-catch语句中抛出异常，强行停止线程进入catch语句，这种方法可以将错误向上抛，使线程停止事件得以传播

##3、线程的状态转换

![Thread](https://img-blog.csdn.net/20170607135231115?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzAzNzk2ODk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "Thread")

1. 新建(new)：创建了一个线程对象 
2. 可运行(runnable)：线程对象创建后，线程调用start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获取cpu的使用权 
3. 运行(running)：可运行状态(runnable)的线程获得了cpu使用权，执行程序代码 
4. 阻塞(block)：线程因为某种原因放弃了cpu使用权，即让出了cpu使用权，暂时停止运行，直到线程进入可运行(runnable)状态，才有机会再次获得cpu使用权转到运行(running)状态。阻塞的情况分三种：

- 等待阻塞：运行(running)的线程执行o.wait()方法，JVM会把该线程放入等待队列(waitting queue)中
- 同步阻塞：运行(running)的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入锁池(lock pool)中
- 其他阻塞：运行(running)的线程执行Thread.sleep(long ms)或t.join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入可运行(runnable)状态
5. 死亡(dead)：线程run()、main() 方法执行结束，或者因异常退出了run()方法，则该线程结束生命周期，且死亡的线程不可再次复生

##4、什么是线程安全

在多线程访问同一代码的时候，不会出现不确定的结果

##5、如何保证线程安全

- 对非安全的代码进行加锁操作
- 使用线程安全的类
- 不要跨线程访问共享变量
- 使用final类型作为共享变量
- 对共享变量加锁操作

##6、Synchronized如何使用

Synchronized是Java的关键字，是一种同步锁，它可以修饰的对象有以下几种

- 修饰代码块：该代码块被称为同步代码块，作用的主要对象是调用这个代码块的对象
- 修饰方法：该方法称为同步方法，作用的主要对象是调用这个方法的对象
- 修饰静态方法：作用范围为整个静态方法，作用的主要对象为这个类的所有对象
- 修饰类：作用范围为Synchronized后面括号括起来的部分，作用的主要对象为这个类的所有对象

##7、Synchronized和Lock的区别

- 相同点：Lock能完成Synchronized所实现的所有功能 
- 不同点：
	1. Synchronized是基于JVM的同步锁，JVM会帮我们自动释放锁。
	2. Lock是通过代码实现的，Lock要求我们手工释放，必须在finally语句中释放。
2. Lock锁的范围有局限性、块范围。Synchronized可以锁块、对象、类
3. Lock功能比Synchronized强大，可以通过tryLock方法在非阻塞线程的情况下拿到锁

##8、多线程的等待唤醒主要方法

1. void notify()：唤醒在此对象监视器上等待的单个线程
2. void notifyAll()：唤醒在此对象监视器上等待的所有线程
3. void wait()：导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法
4. void wait(long timeout)：导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法，或者超过指定的时间量
5. void wait(long timeout, int nanos)：导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法，或者其他某个线程中断当前线程，或者已超过某个实际时间量

##9、sleep和wait的区别

sleep()|wait()
---|:---
是Thread类的方法|是Object类中的方法
调用sleep()，在指定的时间里，暂停程序的执行，让出CPU给其他线程，当超过时间的限制后，又重新恢复到运行状态，在这个过程中，线程不会释放对象锁|调用wait()时，线程会释放对象锁，进入此对象的等待锁池中，只有此对象调用notify()时，线程进入运行状态
##10、多线程中的死锁

死锁：指两个或两个以上的线程在执行的过程中，因抢夺资源而造成互相等待，导致线程无法进行下去

产生死锁的4个必要条件

1. 循环等待：线程中必须有循环等待
2. 不可剥夺：线程已获得资源，再未使用完成之前，不可被剥夺抢占
3. 资源独占：线程在某一时间内独占资源
4. 申请保持：线程因申请资源而阻塞，对已获得的资源保持不放

##11、什么叫守护线程，用什么方法实现守护线程

守护线程：指为其他线程的运行提供服务的线程，可通过setDaemon(boolean on)方法设置线程的Daemon模式，true为守护模式，false为用户模式

##12、Java中的BIO，NIO，AIO分别是什么，应用场景是什么

- BIO：同步并阻塞，服务器实现模式是一个连接对应一个线程，即客户端有连接请求时，服务器就会开启一个线程进行处理，如果这个连接不做任何事情时，会造成不必要的线程开销，可以使用线程池进行改善。其应用场景适用于连接数目比较小且固定的架构，这种方式对服务器资源要求较高，对线程并发有局限性
- NIO：同步非阻塞，服务器实现模式是一个请求对应一个线程，即客户端的连接请求都会注册在多路复用器上，当多路复用器轮询到有I/O请求时才启动一个线程进行处理。其应用场景适用于连接数目多且连接短的架构，对线程并发有局限性
- AIO：异步非阻塞，服务器实现模式是一个有效请求对应一个线程，即客户端的I/O请求完成之后，再通知服务器去启动一个线程进行处理。其应用场景适用于连接数目多且连接长的架构，充分体现出并发性

##13、Java中的IO和NIO的区别

1. IO是面向流的，NIO是面向缓冲区的
2. IO的各种流是阻塞的，NIO是非阻塞模式

##14、volatile关键字

用volatile修饰的变量，线程在每次修改变量的时候，都会读取变量修改后的值，可以简单的理解为volatile修饰的变量保存的是变量的地址。volatile变量具有synchronized的可见性，但是不具备原子性

- 可见性：在多线程并发的条件下，对于变量的修改，其他线程中能获取到修改后的值
- 原子性：在多线程并发的条件下，对于变量的操作是线程安全的，不会受到其他线程的干扰
volatile不是线程安全的，要使volatile变量提供理想的线程安全，必须同时满足下面两个条件

- 对变量的写操作不依赖于当前值
- 该变量没有包含在具有其他变量的不变式中
比如增量操作（x++）看上去类似一个单独操作，实际上它是一个由[读取－修改－写入]操作序列组成的组合操作，必须以原子方式执行，而volatile不能提供必须的原子特性。实现正确的操作，应该使x的值在操作期间保持线程安全，而volatile变量无法实现这点

然而，Java提供了java.util.concurrent.atomic.*包下的变量或引用，让变量或对象的操作具有原子性，在高并发的情况下，依然能保持获取到最新修改的值，常见的有AtomicBoolean、AtomicReference等

- volatile原理：对于值的操作，会立即更新到主存中，当其他线程获取最新值时会从主存中获取
- atomic原理：对于值的操作，是基于底层硬件处理器提供的原子指令，保证并发时线程的安全

#反射相关

##1、什么是反射

在运行状态中，对于任意一个类，都可以获得这个类的所有属性和方法，对于任意一个对象，都能够调用它的任意一个方法和属性

##2、反射使用步骤

获取类的字节码（getClass()、forName()、类名.class）
根据类的方法名或变量名，获取类的方法或变量
执行类的方法或使用变量，如果不使用，也可以创建该类的实例对象（通过获取构造函数执行newInstance方法）

#进程相关

##1、进程的优先级

优先级从低到高排列，优先级最高的进程，最先获取资源，最后释放

1. 空进程，这是Android系统优先杀死的，因为此时该进程已经没有任何用途
2. 后台进程，包含不可见的Activity，即跳转到其他Activity后，由于资源不足，系统会将原来的Activity杀死（即跳转的来源）
3. 服务进程，即Service，当系统资源不足时，系统可能会杀掉正在执行任务的Service，因此在Service执行比较耗时的操作，并不能保证一定能执行完毕
4. 可见进程，当前屏幕上可以看到的Activity，例如显示一个对话框的Activity，那么对话框变成了前台进程，而调用他的Activity是可见进程，但并不是前台的
5. 前台进程，当前处于最前端的Activity，也就是Android最后考虑杀死的对象，一般来说，前台进程Android系统是不会杀死的，只有当前4个都杀掉资源依旧不够才可能会发生

##2、IPC机制

名称|优点|缺点|使用场景
---|:---|:---|:---
Bundle|简单易用|只能传输Binder支持的数据|四大组件间的进程通讯
文件共享|简单易用|不适合高并发场景，并且无法做到进程间的及时通信|无并发访问情形，交换单间的数据实时性不高的场景
AIDL|功能强大，支持一对多并发通信，支持实时通信|使用稍复杂，需要处理好线程同步|一对多通讯切具有RPC需求
Messenger|功能一般，支持一对串行发通信，支持实时通讯|不能很好处理高并发情形，不支持RPC，数据通过Message进行传输，因此只能传输Binder支持的数据|地并发的一对多即时通信，无RPC需求，或者无需要返回结果的RPC
ContentProvider|在数据源访问方便功能强大，支持一对并发数据共享，可通过Call方式扩展|可以理解为约束的AIDL，主要是提供数据源CRUD|一对多的进程间数据分享
Socket|功能强大，可以通过网络传输字节流，支持一对多并发实时通讯|实现细节稍微繁琐，不支持直接RPC|网络数据交换

#类加载器

##1、什么是类加载器

ClassLoader是用来动态加载class文件到内存中

##2、类加载器类型

- App ClassLoader（应用类加载器或系统加载器）：这个加载器使用java实现，使用广泛，负责加载classPath中指定的类。具体的使用场合，在加载classPath中指定的而扩展类加载器没有加载的类，若扩展类加载器加载了classPath中的类，则系统类加载器则没有机会加载。用户定义的类一般都是系统类加载器加载的。可以通过：ClassLoader.getSystemClassLoader()获得
- Extension ClassLoader（扩展类加载器）：它负责加载Java的标准扩展，一般使用Java实现的，负责加载jre/lib/ext中的类。和普通的类加载器一样。可以通过：ClassLoader.getSystemClassLoader().getParent()获得
- BootStrap ClassLoader（引导类加载器）：纯C++实现的加载器，没有对应的Java类，它负责加载jdk中jre/lib目录下的系统核心库。对于java程序无法获得它，像上文中获得扩展类加载器的父类加载器是null。像String，Integer，Double类都是由引导类加载器加载的

##3、双亲委托模型（父委托加载机制）

当加载一个类时，首先会判断当前类是否已经被加载，如果被加载直接返回当前类加载器，如果没有被加载，则把机会让给父类，先让父类加载，若是父类中不能加载，则会去找到Bootstrap加载器，如果Bootstrap加载器加载失败，则会退回上层，自己通过findClass自己去加载对应的路径（这是孝顺型的，先想到父类，但是他们不是通过继承来实现的）

![class](https://img-blog.csdn.net/20170811212257353?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzAzNzk2ODk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "class")


具体实现代码在ClassLoader类中的loadClass方法中，API19和API24加载过程有少许区别（以下是API24的源码），但大部分是一致的，下面执行的逻辑与我们上面所说的一致

```
protected Class<?> loadClass(String name, boolean resolve) throws ClassNotFoundException {
    //首先会判断当前类是否已经被加载
    Class c = findLoadedClass(name);
    if (c == null) {
        long t0 = System.nanoTime();
        try {
            if (parent != null) {
                //把机会让给父类，先让父类加载
                c = parent.loadClass(name, false);
            } else {
                //如果父类无法加载，则会去找Bootstrap加载器
                c = findBootstrapClassOrNull(name);
            }
        } catch (ClassNotFoundException e) {
        }
        if (c == null) {
            long t1 = System.nanoTime();
            //如果前面的都加载失败，则调用findClass用自己的加载器加载
            c = findClass(name);
        }
    }
    return c;
}
```

优点：

- 父委托机制会先去加载系统自带的class，而不会去加载我们自定义的高仿的系统class（比如ArrayList），可以保证我们的程序的安全而不会被恶意注入

##4、类加载过程

![class](https://img-blog.csdn.net/20170811212309124?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzAzNzk2ODk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "class")

- 加载：将类的信息从文件中获取并且载入到JVM内存中
- 验证：检查读入的结构是否符合JVM规范的描述
- 准备：分配一个结构用来存储类信息
- 解析：把这个类的常量池的所有符号引用改变成直接引用
- 初始化：执行静态初始化程序、类构造器方法的过程

##5、自定义类加载器

```
public class MyClassLoader extends DexClassLoader {

    public MyClassLoader(String dexPath, String optimizedDirectory, String librarySearchPath, ClassLoader parent) {
        super(dexPath, optimizedDirectory, librarySearchPath, parent);
    }

    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        byte[] classData = getClassData(name);
        if (classData != null) {
            return defineClass(name, classData, 0, classData.length);
        } else {
            throw new ClassNotFoundException();
        }
    }

    /**
     * 读取文件的字节数组
     *
     * @param name
     * @return
     */
    private byte[] getClassData(String name) {
        try {
            InputStream inputStream = new FileInputStream(name);
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            int buffSize = 4096;
            byte[] buffer = new byte[buffSize];
            int bytesRead = -1;
            while ((bytesRead = inputStream.read(buffer)) != -1) {
                baos.write(buffer, 0, bytesRead);
            }
            return baos.toByteArray();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

##6、类加载主要方法的区别

- findClass()：查找指定路径下的class文件
- loadClass()：加载class字节码文件
- defineClass()：将字节数组流转换成字节码


##7、线程池

Android中的线程池都是之间或间接通过配置ThreadPoolExecutor来实现不同特性的线程池.Android中最常见的四类具有不同特性的线程池分别为FixThreadPool、CachedThreadPool、SingleThreadPool、ScheduleThreadExecutor.

1).FixThreadPool
只有核心线程,并且数量固定的,也不会被回收,所有线程都活动时,因为队列没有限制大小,新任务会等待执行.
优点:更快的响应外界请求.
2).SingleThreadPool
只有一个核心线程,确保所有的任务都在同一线程中按顺序完成.因此不需要处理线程同步的问题.
3).CachedThreadPool
只有非核心线程,最大线程数非常大,所有线程都活动时,会为新任务创建新线程,否则会利用空闲线程(60s空闲时间,过了就会被回收,所以线程池中有0个线程的可能)处理任务.
优点:任何任务都会被立即执行(任务队列SynchronousQueue相当于一个空集合);比较适合执行大量的耗时较少的任务.
4).ScheduledThreadPool
核心线程数固定,非核心线程(闲着没活干会被立即回收)数没有限制.
优点:执行定时任务以及有固定周期的重复任务

#易错题

##1、静态代码块、构造代码块、构造方法的执行顺序

```
class Fu{
    static {
        System.out.println("父静态代码块");
    }
    {
        System.out.println("父代码块");
    }
    public Fu(){
        System.out.println("父构造函数");
    }
}

class Zi extends Fu{
    static {
        System.out.println("子静态代码块");
    }
    {
        System.out.println("子代码块");
    }
    public Zi(){
        System.out.println("子构造函数");
    }
}

public class Text{
    public static void main(String[] args) {
        Zi zi = new Zi();
    }
}
```

输出结果

父静态代码块
子静态代码块
父代码块
父构造函数
子代码块
子构造函数