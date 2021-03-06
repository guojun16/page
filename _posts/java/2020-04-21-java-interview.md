---
layout: post
title: java整理
category: java
tags: [java,myBatis]
excerpt: 2020 java整理
---

## 一. Java基础
#### Java中8大基本类型占用的字节数?
- 1字节： byte , boolean
- 2字节： short , char
- 4字节： int , float
- 8字节： long , double

>注：1字节(byte)=8位(bits)

#### ==和equals的区别是什么?
==对于基本类型来说是值比较,对于引用类型来说是比较的引用;而equals默认情况下是引用比较,只是很多类重写了equals方法,比如String,Integer等把它变成了值比较,所以一般情况下equals比较的是值是否相等

#### final在java中有什么作用?
- final修饰的类叫最终类,该类不能被继承.  
- final修饰的方法不能被重写     
- final修饰的变量是常量,常量必须初始化,初始化之后不能被修改    

#### java容器有哪些?
java容器分为Collection和Map两大类.其下又有很多子类.如下所示:

- Collection  
	- **List**    
	    - **ArrayList**
	    - **LinkedList**
	    - **Vector**
	    - **Stack**
	- **Set**   
		- **HashSet**
		- **LinkedHashSet**
		- **TreeSet**
	
- Map
	- **HashMap**	
	- **LinkedHashMap**
	- **TreeMap**
	- **ConcurrentHashMap**
	- **HashTable**

#### HashMap和Hashtable有什么区别?
- 存储: HashMap运行key和value为null,而Hashtable不允许.
- 线程安全: Hashtable是线程安全的,而HashMap是非线程安全的.
- 推荐使用: 在Hashtable的类注释可以看到,Hashtbale是保留类不建议使用,推荐在单线程环境下使用HashMap替代,如果需要多线程则用ConcurrentHashMap替代

#### HashMap的实现原理?
HashMap是使用拉链法.HashMap基于Hash算法实现,通过put(key,value)存储,get(key)来获取.当传入key时,HashMap会根据
key.hashCode()计算hash值,根据hash值将value保存在bucket里.当计算出的hash值相同时,我们称之为hash冲突
HashMap的做法是用链表和红黑树存储相同的hash值得value.当hash冲突的个数比较少时,使用链表,当大于8个时使用红黑树.

#### ArrayList和LinkedList的区别是什么?
- 数据结构实现: ArrayList是动态数组的数据结构实现,而LinkedList是双向链表的数据结构实现.
- 随机访问效率: ArrayList在随机访问的时候效率要高,因为LinkedList是线性的数据存储方式,所以需要移动指针从前往后一次查找.
- 增加和删除效率: 在非首尾的增加和删除操作,LinkedList要比ArrayList效率要高,因为ArrayList增删操作要影响数组内的其他数据的下标.    
综合来说,在需要频繁读取集合中的元素时,更推荐使用ArrayList,而在插入和删除操作比较多时,更推荐使用LinkedList.

#### HashMap和TreeMap 区别
- 相同点
    - 都是线程非安全
    - key都不可以重复,value都允许重复
- 不同点
    - HashMap允许null作为key和value, TreeMap不允许null作为key
    - HashMap是数组加链表方式存储key/value, TreeMap基于红黑树
    - HashMap不保证元素迭代顺序是按照插入时的顺序是无序的, TreeMap存入元素应当实现Comparable接口(内部比较器)或实现Comparator接口(外部比较器)才能按照排序后顺序遍历元素

#### HashSet有什么功能，基于哪个类实现
有去重的功能,也就是元素不重复. 基于HashMap实现

#### ConcurrentHashMap并发能力为什么好于Hashtable
- Hashtable是通过对hash表整体进行锁定，是阻塞式的，当一个线程占有这个锁时，其他线程必须阻塞等待其释放锁
而ConcurrentHashMap是如下实现：
- jdk1.6的实现：ConcurrentHashMap是采用Segment分段锁的方式，它并没有对整个数据结构进行锁定，而是局部锁定，
- jdk1.8的实现： 采用一种乐观锁CAS算法来实现同步问题，但其底层还是“数组+链表->红黑树”的实现

#### sleep()和wait()有什么区别?
- 类的不同: sleep()来自Thread,wait()来自object.
- 释放锁: sleep()不释放锁;wait()释放锁.
- 用法不同: sleep()时间到会自动恢复;wait()可以使用notify()/notifyAll()直接唤醒.

#### 在java程序中怎么保证多线程的运行安全?
- 方法一: 使用安全类,比如Java.util.concurrent下的类.
- 方法二: 使用自动锁synchronized
- 方法三: 使用手动锁Lock

#### 什么是反射
反射是在运行状态中,对于任意一个类,都能够知道这个类的所有属性和方法;对于任意一个对象,都能够调用他的任意一个方法和属性;
这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制.

#### Java中元注解有哪些，并分别说一下作用
- @Retention: 定义注解的保留策略
- @Target：定义注解的作用目标
- @Document：说明该注解将被包含在javadoc中
- @Inherited：说明子类可以继承父类中的该注解

>元注解是指注解的注解

#### tcp和 udp的区别
tcp和udp是OSI模型中的运输层中的协议.tcp提供可靠的通信传输.而udp则常被用于让广播和细节控制交给应用的通信传输.
两者的区别大致如下:
- tcp面向连接,udp面向非连接即发送数据前不需要建立连接;
- tcp提供可靠的服务(数据传输),udp无法保证;
- tcp面向字节流,udp面向报文;
- tcp数据传输慢,udp数据传输快;

#### 如何实现跨域?
- 服务器端运行跨域 设置CORS等于*;
- 在单个接口使用注解@CorssOrigin运行跨域;
- 使用jsonp跨域;

#### 说一下你熟悉的设计模式?
- 单例模式: 保证被创建一次,节省系统开销.
- 工厂模式: 解耦代码.
- 观察者模式: 定义了对象之间的一对多的依赖,这样一来,当一个对象改变时,它的所有的依赖都会受到通知并自动更新.
- 外观模式: 提供一个统一的接口,用来访问子系统的一群接口,外观定义了一个高层的接口,让子系统更容易使用.
- 模板方法模式: 定义了一个算法的骨架,而将一些不走延迟到子类中,模板方法使得子类可以在不改变算法结构的情况下,重新定义算法的步骤.
- 状态模式: 允许对象在内部状态改变时改变它的行为,对象看起来好像修改了它的类.

## 二. 多线程
#### 创建线程的方式
- 继承 Thread 类，重写 run 方法
- 实现 Runnable 接口，实现 run 方法
- 实现 Callable 接口，实现 call 方法

#### sleep() 和 wait() 有什么区别
- 类的不同：sleep() 来自 Thread，wait() 来自 Object。
- 释放锁：sleep() 不释放锁；wait() 释放锁。
- 用法不同：sleep() 时间到会自动恢复；wait() 可以使用 notify()/notifyAll()直接唤醒。

#### 如何保证一个线程执行完再执行第二个线程？
- 使用join
- countDownLatch

#### 如何预防死锁？
- 尽量使用 tryLock(long timeout, TimeUnit unit) 的方法 (ReentrantLock、ReentrantReadWriteLock)，设置超时时间，超时可以退出防止死锁；
- 尽量使用 Java. util. concurrent 并发类代替自己手写锁；
- 尽量降低锁的使用粒度，尽量不要几个功能用同一把锁；
- 尽量减少同步的代码块。

#### Executors 可以创建以下六种线程池?

- FixedThreadPool(n)：创建一个数量固定的线程池，超出的任务会在队列中等待空闲的线程，可用于控制程序的最大并发数。
- CachedThreadPool()：短时间内处理大量工作的线程池，会根据任务数量产生对应的线程，并试图缓存线程以便重复使用，如果限制 60 秒没被使用，则会被移除缓存。
- SingleThreadExecutor()：创建一个单线程线程池。
- ScheduledThreadPool(n)：创建一个数量固定的线程池，支持执行定时性或周期性任务。
- SingleThreadScheduledExecutor()：此线程池就是单线程的 newScheduledThreadPool。
- WorkStealingPool(n)：Java 8 新增创建线程池的方法，创建时如果不设置任何参数，则以当前机器处理器个数作为线程个数，此线程池会并行处理任务，不能保证执行顺序。 

#### 线程池7个参数
- corePoolSize 线程池核心线程大小
- maximumPoolSize 线程池最大线程数量
- keepAliveTime 空闲线程存活时间
- unit 空间线程存活时间单位
- workQueue 工作队列
- threadFactory 线程工厂
- handler 拒绝策略

#### 线程的工作原理
- 当线程池中有任务需要执行时，线程池会判断如果线程数量没有超过核心数量就会新建线程池进行任务执行
- 如果线程池中的线程数量已经超过核心线程数，这时候任务就会被放入任务队列中排队等待执行
- 如果任务队列超过最大队列数，并且线程池没有达到最大线程数，就会新建线程来执行任务
- 如果超过了最大线程数，就会执行拒绝执行策略

#### ThreadLocal 为什么是线程安全的？
ThreadLocal 为每一个线程维护变量的副本，把共享数据的可见范围限制在同一个线程之内，因此 ThreadLocal 是线程安全的，每个线程都有属于自己的变量。

#### ThreadLocal 和 Synchronized 有什么区别？
Synchronized 用于实现同步机制，是利用锁的机制使变量或代码块在某一时刻只能被一个线程访问，是一种 “以时间换空间” 的方式；而 ThreadLocal 为每一个线程提供了独立的变量副本，这样每个线程的（变量）操作都是相互隔离的，这是一种 “以空间换时间” 的方式。

#### CAS 与 ABA
CAS（Compare and Swap）比较并交换，是一种乐观锁的实现，是用非阻塞算法来代替锁定，其中 java.util.concurrent 包下的 AtomicInteger 就是借助 CAS 来实现的。
但 CAS 也不是没有任何副作用，比如著名的 ABA 问题就是 CAS 引起的。       
常见解决 ABA 问题的方案加版本号，来区分值是否有变动。
>CAS指令执行时，当且仅当旧值与预期值A相等时，才可以把值修改为B，否则就什么都不做
>整个比较并替换的操作是一个原子操作，通过硬件指令支持

#### volatile 的作用是什么？
volatile 是 Java 虚拟机提供的最轻量级的同步机制。
当变量被定义成 volatile 之后，具备两种特性：

保证此变量对所有线程的可见性，当一条线程修改了这个变量的值，修改的新值对于其他线程是可见的（可以立即得知的）            
禁止指令重排序优化，普通变量仅仅能保证在该方法执行过程中，得到正确结果，但是不保证程序代码的执行顺序。 

#### volatile 对比 synchronized 有什么区别？
synchronized 既能保证可见性，又能保证原子性，而 volatile 只能保证可见性，无法保证原子性。比如，i++ 如果使用 synchronized 修饰是线程安全的，而 volatile 会有线程安全的问题。

#### java有哪些可重入锁
- ReentrantLock 是基于AQS实现的，分为公平锁和非公平锁
- Synchronized 是非公平锁

>概念：同一线程可多次获取在同一对象上的锁

## 三. JVM
#### JVM 主要组成部分有哪些？
- 类加载器（ClassLoader）
- 运行时数据区（Runtime Data Area）
- 执行引擎（Execution Engine）
- 本地库接口（Native Interface）

#### JVM 是如何工作的？
首先程序在执行之前先要把 Java 代码（.java）转换成字节码（.class），JVM 通过类加载器（ClassLoader）把字节码加载到内存中，但字节码文件是 JVM 的一套指令集规范，并不能直接交给底层操作系统去执行，因此需要特定的命令解析器执行引擎（Execution Engine） 将字节码翻译成底层机器码，再交由 CPU 去执行，CPU 执行的过程中需要调用本地库接口（Native Interface）来完成整个程序的运行。

#### 类加载器
- 启动类加载器：Bootstrap ClassLoader，负责加载存放在JDK\jre\lib(JDK代表JDK的安装目录，下同)下，或被-Xbootclasspath参数指定的路径中的，并且能被虚拟机识别的类库（如rt.jar，所有的java.开头的类均被Bootstrap ClassLoader加载）。启动类加载器是无法被Java程序直接引用的。
- 扩展类加载器：Extension ClassLoader，该加载器由sun.misc.Launcher$ExtClassLoader实现，它负责加载JDK\jre\lib\ext目录中，或者由java.ext.dirs系统变量指定的路径中的所有类库（如javax.开头的类），开发者可以直接使用扩展类加载器。
- 应用程序类加载器：Application ClassLoader，该类加载器由sun.misc.Launcher$AppClassLoader来实现，它负责加载用户类路径（ClassPath）所指定的类，开发者可以直接使用该类加载器，如果应用程序中没有自定义过自己的类加载器，一般情况下这个就是程序中默认的类加载器。

#### 类加载执行流程
- 加载：根据查找路径找到相应的 class 文件然后导入；
- 检查：检查加载的 class 文件的正确性；
- 准备：给类中的静态变量分配内存空间；
- 解析：虚拟机将常量池中的符号引用替换成直接引用的过程。符号引用就理解为一个标示，而在直接引用直接指向内存中的地址；
- 初始化：对静态变量和静态代码块执行初始化工作。
- 使用
- 卸载

>其中类加载的过程包括了加载、验证、准备、解析、初始化五个阶段。在这五个阶段中，加载、验证、准备和初始化这四个阶段发生的顺序是确定的，而解析阶段则不一定，它在某些情况下可以在初始化阶段之后开始，这是为了支持Java语言的运行时绑定（也成为动态绑定或晚期绑定）。另外注意这里的几个阶段是按顺序开始，而不是按顺序进行或完成，因为这些阶段通常都是互相交叉地混合进行的，通常在一个阶段执行的过程中调用或激活另一个阶段。

#### 类加载机制
- 全盘负责，当一个类加载器负责加载某个Class时，该Class所依赖的和引用的其他Class也将由该类加载器负责载入，除非显示使用另外一个类加载器来载入
- 父类委托，先让父类加载器试图加载该类，只有在父类加载器无法加载该类时才尝试从自己的类路径中加载该类
- 缓存机制，缓存机制将会保证所有加载过的Class都会被缓存，当程序中需要使用某个Class时，类加载器先从缓存区寻找该Class，只有缓存区不存在，系统才会读取该类对应的二进制数据，并将其转换成Class对象，存入缓存区。这就是为什么修改了Class后，必须重启JVM，程序的修改才会生效

#### JVM 内存布局是怎样的？
- 程序计数器（Program Counter Register）
- Java 虚拟机栈（Java Virtual Machine Stacks）
- 本地方法栈（Native Method Stack）
- Java 堆（Java Heap）
- 方法区（Method Area）

#### JVM堆内存模型
- JDK1.7的堆内存模型  
    - Young 年轻代
    - Tenured 年老代
    - Perm 永久代
- JDK1.8的堆内存模型 
    - Young 年轻代
    - Tenured 年老代
    
> 在jdk1.8中变化最大的Perm区，用Metaspace（元数据空间）进行了替换。
  需要特别说明的是：Metaspace所占用的内存空间不是在虚拟机内部，而是在本地内存
  空间中，这也是与1.7的永久代最大的区别所在
  
#### JDK内置分析工具
- jps 
    - 查找进程
- jstat
    - 查看class加载统计   
    jstat ‐class 6219   
    - 查看编译统计    
    jstat ‐compiler 6219    
    - 垃圾回收统计    
    jstat ‐gc 6219
    - 也可以指定打印的间隔和次数，每1秒中打印一次，共打印5次      
    jstat ‐gc 6219 1000 5
- jmap  
    - 查看内存使用情况  
    jmap ‐heap 6219
    - 查看所有对象，包括活跃以及非活跃的     
    jmap ‐histo <pid> | more
    - 查看活跃对象    
    jmap ‐histo:live <pid> | more
    - 将内存使用情况dump到文件中   
    jmap ‐dump:format=b,file=dumpFileName <pid>
    
- jhat
    - 通过jhat对dump文件进行分析 jhat ‐port port file     
    jhat ‐port 9999 /tmp/dump.dat  

- jstack
    - 查看jvm中的线程执行情况

- MAT工具分析dump文件
    - 一个基于Eclipse的内存分析工具，是一个快速、功能丰富的JAVA heap分析工具，

- VisualVM工具的使用
    - VisualVM使用简单，几乎0配置，功能还是比较丰富的，几乎囊括了上面JDK自带命令的所有功能。


#### JVM怎样判断某个对象应被回收
- 【引用计数法】系统会为对象添加一个计数器，当有新的引用时加1，引用失效时减1。但是此方法无法解决两个对象循环引用的问题。
- 【可达性分析法】通过对象的引用链来判断该对象是否需要被回收，通过一系列的GC Roots的对象作为起始点，从这些起节点开始向下搜索，搜索所走过的路径称为引用链（Reference Chain），当一个对象到GC Roots没有任何引用链相连时，则证明此对象是不可用的，就需要回收此对象。

#### 常见的GC Roots有哪些
- 虚拟机栈中引用的对象
- 方法区中类静态属性引用的对象
- 方法区中常量引用的对象
- 本地方法栈JNI引用的对象

#### 垃圾收集算法
- 标记-清除算法    
- 标记-整理算法
- 复制算法
- 分代收集算法

>分代收集算法把java堆分为新生代和老年代, 新生代选用复制算法,老年代采用标记-清理算法或标记-整理算法;     
默认情况下新生代和老生代的内存比例是 1:2  
新生代是由：Eden、Form Survivor、To Survivor 三个区域组成的，它们内存默认占比是 8:1:1

#### 新生代垃圾回收是怎么执行的？
- Eden 区 + From Survivor 区存活着的对象复制到 To Survivor 区；
- 清空 Eden 和 From Survivor 分区；
- From Survivor 和 To Survivor 分区交换（From 变 To，To 变 From）。

#### 垃圾回收器
- CMS（Concurrent Mark Sweep）一种以获得最短停顿时间为目标的收集器，采用标记-清理算法, 非常适用 B/S 系统。
> 1. 初始标记 -- 仅仅关联GC Roots 能直接关联的对象, 速度很快
2. 并发标记 -- 进行GC Roots tracing的过程
3. 重新标记 -- 为了修正并发标记期间,因用户程序运行而导致标记产生变动的那一部分对象的标记记录
4. 并发清楚

> CMS收集器的三大缺点
1. CMS收集器对CPU资源非常敏感
2. 无法处理浮动垃圾
3. 因为基于标记清除算法, 所以会有大量的垃圾碎片产生

- G1设计原则就是简单可行的性能调优, G1 垃圾回收器是一种兼顾吞吐量和停顿时间的 GC 实现，是 JDK 9 以后的默认 GC 选项。G1 可以直观的设定停顿时间的目标(通过-XX:MaxGCPauseMills=200)，相比于 CMS CG，G1 未必能做到 CMS 在最好情况下的延时停顿，但是最差情况要好很多

#### 垃圾回收的调优参数有哪些？
- -Xmx:512 设置最大堆内存为 512 M；
- -Xms:215 初始堆内存为 215 M；
- -XX:MaxNewSize 设置最大年轻区内存；
- -XX:MaxTenuringThreshold=5 设置新生代对象经过 5 次 GC 晋升到老年代；
- -XX:PretrnureSizeThreshold 设置大对象的值，超过这个值的大对象直接进入老生代；
- -XX:NewRatio 设置分代垃圾回收器新生代和老生代内存占比；
- -XX:SurvivorRatio 设置新生代 Eden、Form Survivor、To Survivor 占比。

## 四. Mysql

#### 事务的四大特性?
- 原子性
- 一致性
- 隔离性
- 持久性

#### MyISAM和InnoDB的区别
- MyISAM只有表级锁; InnoDB支持行级锁和表级锁,默认为行级锁
- MyIsAm不支持事务,外键; InnoDb提供事务,外键,支持
- MyISAM崩溃后无法安全恢复; InnoDB拥有崩溃修复能力

#### 四种隔离级别?
- Serializable (串行化)：可避免脏读、不可重复读、幻读的发生。
- Repeatable read (可重复读)：可避免脏读、不可重复读的发生。
- Read committed (读已提交)：可避免脏读的发生。
- Read uncommitted (读未提交)：最低级别，任何情况都无法保证。

#### 不考虑事务的隔离性,会发生什么问题?
- 脏读
- 不可重复读
- 幻读

#### 数据库的索引有哪些?
- 普通索引(INDEX)：最基本的索引，没有任何限制
- 唯一索引(UNIQUE)：与"普通索引"类似，不同的就是：索引列的值必须唯一，但允许有空值。
- 主键索引(PRIMARY)：它 是一种特殊的唯一索引，不允许有空值。
- 全文索引(FULLTEXT )：仅可用于 MyISAM 表， 用于在一篇文章中，检索文本信息的, 针对较大的数据，生成全文索引很耗时好空间。
- 组合索引：为了更多的提高mysql效率可建立组合索引，遵循”最左前缀“原则。

#### 简述Mysql聚集索引和非聚集索引区别
- 聚集索引在叶子节点存储的是表中的数据(主键就是聚集索引)
- 非聚集索引在叶子节点存储的是主键和索引列
- 使用非聚集索引查询出数据时，拿到叶子上的主键再去查到想要查找的数据。(拿到主键再查找这个过程叫做回表)

#### 什么是覆盖索引
select的数据列仅从索引中就能够取得并且不必从数据表中读取，换句话说查询列要被所使用的索引覆盖。
例子：如以country表a（表示城市）、b（表示省）两列做复合索引，select a from country where b='浙江省'

#### 什么操作会破坏索引?
- 1.如果条件中有or，即使其中有条件带索引也不会使用;要想使用or，又想让索引生效，只能将or条件中的每个列都加上索引.
- 2.like查询是以%开头
- 3.如果列类型是字符串，那一定要在条件中将数据使用引号引用起来,否则不使用索引
- 4.对于多列索引，不是使用的第一部分，则不会使用索引
- 5.如果mysql估计使用全表扫描要比使用索引快,则不使用索引

#### MySQL 问题排查都有哪些手段？
- 使用 show processlist 命令查看当前所有连接信息。
- 使用 explain 命令查询 SQL 语句执行计划。
- 开启慢查询日志，查看慢查询的 SQL。

#### Mysql 性能优化
- 为搜索字段创建索引。
- 避免使用 select *，列出需要查询的字段。
- 垂直分割分表。
- 选择正确的存储引擎。
- 使用join时,小表驱大表
- 避免字段为null

#### Mysql优化方法
##### 1.索引的优化?
- 只要列中含有NULL值，就最好不要在此例设置索引，复合索引如果有NULL值，此列在使用时也不会使用索引
- 尽量使用短索引，如果可以，应该制定一个前缀长度
- 对于经常在where子句使用的列，最好设置索引，这样会加快查找速度
- 对于有多个列where或者order by子句的，应该建立复合索引
- 对于like语句，以%或者‘-’开头的不会使用索引，以%结尾会使用索引
- 尽量不要在列上进行运算（函数操作和表达式操作）
- 尽量不要使用not in和<>操作
##### 2.sql语句的优化?
- 查询时，能不要*就不用*，尽量写全字段名
- 大部分情况连接效率远大于子查询
- 多使用explain和profile分析查询语句
- 查看慢查询日志，找出执行时间长的sql语句优化
- 多表连接时，尽量小表驱动大表，即小表 join 大表
- 在千万级分页时使用limit
- 对于经常使用的查询，可以开启缓存
##### 3.表的优化?
- 表的字段尽可能用NOT NULL
- 字段长度固定的表查询会更快
- 把数据库的大表按时间或一些标志分成小表
- 将表分区

## 五. Spring
#### 为什么要使用 spring？
spring 提供 ioc 技术，容器会帮你管理依赖的对象，从而不需要自己创建和管理依赖对象了，更轻松的实现了程序的解耦。
spring 提供了事务支持，使得事务操作变的更加方便。
spring 提供了面向切片编程，这样可以更方便的处理某一类的问题。
更方便的框架集成，spring 可以很方便的集成其他框架，比如 MyBatis、hibernate 等。

#### 解释一下什么是 aop？
aop 是面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。
简单来说就是统一处理某一“切面”（类）的问题的编程思想，比如统一处理日志、异常等。

#### 解释一下什么是 ioc？
ioc：Inversionof Control（中文：控制反转）是 spring 的核心，对于 spring 框架来说，就是由 spring 来负责控制对象的生命周期和对象间的关系。
简单来说，控制指的是当前对象对内部成员的控制权；控制反转指的是，这种控制权不由当前对象管理了，由其他（类,第三方容器）来管理。

#### spring 常用的注入方式有哪些？
- setter 属性注入 
- 构造方法注入  
- 注解方式注入  

#### spring 中的 bean 是线程安全的吗？
spring 中的 bean 默认是单例模式，spring 框架并没有对单例 bean 进行多线程的封装处理。

实际上大部分时候 spring bean 无状态的（比如 dao 类），所有某种程度上来说 bean 也是安全的，但如果 bean 有状态的话（比如 view model 对象），那就要开发者自己去保证线程安全了，最简单的就是改变 bean 的作用域，把“singleton”变更为“prototype”，这样请求 bean 相当于 new Bean()了，所以就可以保证线程安全了。

有状态就是有数据存储功能。   
无状态就是不会保存数据。    

#### spring中bean生命周期
- 实例化Bean对象
- 设置对象属性
- 检查是否实现Aware相关接口，并设置相关依赖
- 执行BeanPostProcessor前置处理
- 检查是否是InitializingBean以决定是否调用afterPropertiesSet方法
- 检查是否配置有自定义的init-method方法
- 执行BeanPostProcessor后置处理
- 注册必要的Destruction相关回调接口
- 使用中
- 是否实现DisposableBean接口，如果有，则执行相应的方法
- 是否配置有自定义的destroy方法，如果有则执行销毁

#### spring 事务实现方式有哪些？   
声明式事务：声明式事务也有两种实现方式，基于 xml 配置文件的方式和注解方式（在类上添加 @Transaction 注解）。     
编程式事务：提供编码的形式管理和维护事务。 

#### Spring 中都是用了哪些设计模式？
- 工厂模式：通过 BeanFactory、ApplicationContext 来创建 bean 都是属于工厂模式；
- 单例、原型模式：创建 bean 对象设置作用域时，就可以声明 Singleton（单例模式）、Prototype（原型模式）；
- 观察者模式：Spring 可以定义一下监听，如 ApplicationListener 当某个动作触发时就会发出通知；
- 责任链模式：AOP 拦截器的执行；
- 策略模式：在创建代理类时，如果代理的是接口使用的是 JDK 自身的动态代理，如果不是接口使用的是 CGLIB 实现动态代理。

#### Spring MVC 的执行流程?
前端控制器（DispatcherServlet） 接收请求，通过映射从 IoC 容器中获取对应的 Controller 对象和 Method 方法，在方法中进行业务逻辑处理组装数据，组装完数据把数据发给视图解析器，视图解析器根据数据和页面信息生成最终的页面，然后再返回给客户端。

## 六. MyBatis
#### MyBatis 中 #{}和 ${}的区别是什么？
\#{}是预编译处理，${}是字符替换。 在使用 #{}时，MyBatis 会将 SQL 中的 #{}替换成“?”，配合 PreparedStatement 的 set 方法赋值，这样可以有效的防止 SQL 注入，保证程序的运行安全。

#### MyBatis 和 hibernate 的区别有哪些？
- 灵活性：MyBatis 更加灵活，自己可以写 SQL 语句，使用起来比较方便。
- 可移植性：MyBatis 有很多自己写的 SQL，因为每个数据库的 SQL 可以不相同，所以可移植性比较差。
- 学习和使用门槛：MyBatis 入门比较简单，使用门槛也更低。
- 二级缓存：hibernate 拥有更好的二级缓存，它的二级缓存可以自行更换为第三方的二级缓存。

#### mybatis 的 dao 接口跟 xml 文件里面的sql 是如何建立关系的？
Mybatis运行时会使用JDK动态代理为Mapper接口生成代理proxy对象，代理对象proxy会拦截接口方法，从而执行sql. 所以，我们通过@Autowired注入Dao接口的时候，注入的就是这个代理对象，我们调用到Dao接口的方法时，则会调用到MapperProxy对象的invoke方法。
只要保证namespace+id唯一即可

## 七. Spring boot

#### spring boot优势
- 独立运行的Spring项目    
Spring Boot可以以jar包的形式进行独立的运行，使用：java -jar xx.jar 就可以成功的运行项目，或者在应用项目的主程序中运行main函数即可；     
- 内嵌的Servlet容器   
内嵌容器，使得我们可以执行运行项目的主程序main函数，并让项目的快速运行；  
- 提供starter简化Maven配置   
Spring Boot提供了一系列的starter pom用来简化我们的Maven依赖     
- 自动配置Spring   
Spring Boot会根据我们项目中类路径的jar包/类，为jar包的类进行自动配置Bean，这样一来就大大的简化了我们的配置。当然，这只是Spring考虑到的大多数的使用场景，在一些特殊情况，我们还需要自定义自动配置； 
- 应用监控     
Spring Boot提供了基于http、ssh、telnet对运行时的项目进行监控； 

#### spring boot 常用注解
@SpringBootApplication  
@Configuration  
@Bean   

#### 常见的 starter 有哪些?
- spring-boot-starter-web：Web 开发支持
- spring-boot-starter-data-jpa：JPA 操作数据库支持
- spring-boot-starter-data-redis：Redis 操作支持
- spring-boot-starter-data-solr：Solr 权限支持
- mybatis-spring-boot-starter：MyBatis 框架支持

#### 自动配置原理
SpringBoot自动配置最主要的注解就是@enableAutoConfiguration,这个注解会导入一个EnableAutoConfigurationImportSelector的类,而这个类会去读取类路径下所有jar包里META-INF/spring.factories下key为EnableAutoConfiguration的对应值，找到相应得配置类，然后执行相应配置      

## 八. Spring cloud
#### 什么是spring cloud
spring cloud 是一系列框架的有序集合。它利用 spring boot 的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用 spring boot 的开发风格做到一键启动和部署。

## 九. Redis

#### redis持久化方式?
- RDB(redis database)：在指定的时间间隔能对你的数据进行快照存储。
- AOF(append only file)：记录每次对服务器写的操作,当服务器重启的时候会重新执行这些命令来恢复原始的数据。

#### redis有哪些数据类型?
- String
- Hash
- List
- Set
- zset
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190306101116895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VxcXRr,size_16,color_FFFFFF,t_70)

#### redis常见应用场景?
- 排行榜
- 记录帖子点赞数、点击数、评论数；
- 缓存近期热帖；
- 缓存文章详情信息；
- 记录用户会话信息。
>string，用作计数器，统计在线人数等等，可以存储二进制数据如使用它来存储图片等。  
 hash，存放键值对，一般可以用来存某个对象的基本属性信息，例如，用户信息，商品信息等        
 list，列表类型，可以用于实现消息队列，也可以使用它提供的range命令，做分页查询功能。     
 set，可以用作去重功能，例如用户名不能重复等，另外，还可以对集合进行交集，并集操作，来查找某些元素的共同点 

#### 缓存穿透, 缓存击穿, 缓存雪崩
- 缓存穿透:指查询一个一定不存在的数据，如果从存储层查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到 DB 去查询，可能导致 DB 挂掉。     
    解决方案:   
    1.查询返回的数据为空，仍把这个空结果进行缓存，但过期时间会比较短;  
    2.布 隆过滤器:将所有可能存在的数据哈希到一个足够大的 bitmap 中，一个一定不存在的数据 会被这个 bitmap 拦截掉，从而避免了对 DB 的查询。 

- 缓存击穿:对于设置了过期时间的 key，缓存在某个时间点过期的时候，恰好这时间点对 这个 Key 有大量的并发请求过来，这些请求发现缓存过期一般都会从后端 DB 加载数据并 回设到缓存，这个时候大并发的请求可能会瞬间把 DB 压垮。    
    解决方案:   
    1.使用互斥锁:当缓存失效时，不立即去load db，先使用如Redis的setnx去设 置一个互斥锁，当操作成功返回时再进行load db的操作并回设缓存，否则重试get缓存的 方法。   
    2.永远不过期:物理不过期，但逻辑过期(后台异步线程去刷新)。 

- 缓存雪崩:设置缓存时采用了相同的过期时间，导致缓存在某一时刻同时失效，请求全部 转发到 DB，DB 瞬时压力过重雪崩。与缓存击穿的区别:雪崩是很多 key，击穿是某一个key 缓存。     
    解决方案:   
    将缓存失效时间分散开，比如可以在原有的失效时间基础上增加一个随机值， 比如 1-5 分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效 的事件。

#### 如何保证缓存与数据库的双写一致性？
- 先删除缓存,再更新数据库
> 删除缓存后,一个请求查询缓存为空,然后更新旧数据都缓存中,然后更新数据库,这样数据库和缓存就不一致了

- 先更新数据库,再更新缓存
> 数据库更新完成后,如果缓存更新失败,例如断电了,这样缓存中还是旧数据,数据库和缓存数据也不一致

- 删除缓存- 更新数据库 -删除缓存
> 高并发下保证绝对的一致，就需要用到内存队列做异步串行化。非高并发场景，双删策略基本满足了。如果特别要求强一致性，并不适合放在redis

## 十. RabbitMQ
#### 消息队列主要功能
- 解耦
- 削峰
- 异步

#### 消息队列的应用场景有哪些？

- 应用解耦，比如，用户下单后，订单系统需要通知库存系统，假如库存系统无法访问，则订单减库存将失败，从而导致订单失败。订单系统与库存系统耦合，这个时候如果使用消息队列，可以返回给用户成功，先把消息持久化，等库存系统恢复后，就可以正常消费减去库存了。
- 削峰填谷，比如，秒杀活动，一般会因为流量过大，从而导致流量暴增，应用挂掉，这个时候加上消息队列，服务器接收到用户的请求后，首先写入消息队列，假如消息队列长度超过最大数量，则直接抛弃用户请求或跳转到错误页面。
- 日志系统，比如，客户端负责将日志采集，然后定时写入消息队列，消息队列再统一将日志数据存储和转发。

#### RabbitMQ 有哪些优点？
- 可靠性，RabbitMQ 的持久化支持，保证了消息的稳定性；
- 高并发，RabbitMQ 使用了 Erlang 开发语言，Erlang 是为电话交换机开发的语言，天生自带高并发光环和高可用特性；
- 集群部署简单，正是因为 Erlang 使得 RabbitMQ 集群部署变的非常简单；
- 社区活跃度高，因为 RabbitMQ 应用比较广泛，所以社区的活跃度也很高；
- 解决问题成本低，因为资料比较多，所以解决问题的成本也很低；
- 支持多种语言，主流的编程语言都支持，如 Java、.NET、PHP、Python、JavaScript、Ruby、Go 等；
- 插件多方便使用，如网页控制台消息管理插件、消息延迟插件等。

#### RabbitMQ 有哪些重要的角色？
- 生产者：消息的创建者，负责创建和推送数据到消息服务器；
- 消费者：消息的接收方，用于处理数据和确认消息；
- 代理：就是 RabbitMQ 本身，用于扮演“快递”的角色，本身不生产消息，只是扮演“快递”的角色。

#### RabbitMQ 有哪些重要的组件？它们有什么作用？
- ConnectionFactory（连接管理器）：应用程序与 RabbitMQ 之间建立连接的管理器，程序代码中使用；
- Channel（信道）：消息推送使用的通道；
- Exchange（交换器）：用于接受、分配消息；
- Queue（队列）：用于存储生产者的消息；
- RoutingKey（路由键）：用于把生成者的数据分配到交换器上；
- BindingKey（绑定键）：用于把交换器的消息绑定到队列上。

#### RabbitMQ 交换器类型有哪些？
- direct：轮询方式
- headers：轮询方式，允许使用 header 而非路由键匹配消息，性能差，几乎不用
- fanout：广播方式，发送给所有订阅者
- topic：匹配模式，允许使用正则表达式匹配消息

> RabbitMQ 默认的是 direct 方式。

#### RabbitMQ 如何确保每个消息能被消费？
RabbitMQ 使用 ack 消息确认的方式保证每个消息都能被消费，开发者可根据自己的实际业务，选择 channel.basicAck() 方法手动确认消息被消费。

#### RabbitMQ 接收到消息之后必须消费吗？
RabbitMQ 接收到消息之后可以不消费，在消息确认消费之前，可以做以下两件事：

- 拒绝消息消费，使用 channel.basicReject(消息编号, true) 方法，消息会被分配给其他订阅者；
- 设置为死信队列，死信队列是用于专门存放被拒绝的消息队列。

#### RabbitMQ 包含事务功能吗？如何使用？
RabbitMQ 包含事务功能，主要是对信道（Channel）的设置，主要方法有以下三个：

- channel.txSelect() 声明启动事务模式；
- channel.txComment() 提交事务；
- channel.txRollback() 回滚事务。

#### RabbitMQ 的事务在什么情况下是无效的？
RabbitMQ 的事务在 autoAck=true 也就是自动消费确认的时候，事务是无效的。因为如果是自动消费确认，RabbitMQ 会直接把消息从队列中移除，即使后面事务回滚也不能起到任何作用。

