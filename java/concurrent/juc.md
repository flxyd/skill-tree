# JUC多线程体系支持
![juc](https://ws2.sinaimg.cn/large/006tNc79gy1fvrme9e02dj310w0jw40n.jpg)
## atomic
* AtomicBoolean
* AtomicInteger
* AtomicIntegerArray
* AtomicLong
* AtomicLongArray
* AtomicIntegerFieldUpdater
* AtomicLongFieldUpdater
* AtomicReference
* AtomicReferenceArray
* AtomicReferenceFieldUpdater
* AtomicMarkableReference
### Skill Tree
![atomic](https://ws1.sinaimg.cn/large/006tNc79gy1fvrnhbd52rj314e0s2tcd.jpg)

## Fork/Join框架
Java7提供了的一个用于并行执行任务的框架， 是一个把大任务分割成若干个小任务，最终汇总每个小任务结果后得到大任务结果的框架
### 工作窃取算法
* 工作窃取（work-stealing）算法是指某个线程从其他队列里窃取任务来执行
### 框架工作流程
* 分割任务
* 执行任务
    * 放在一个双端队列里，线程从 两端拿
* 合并结果
    * 执行完的结果都统一放在一个结果队列里，由一个线程去消费结果
### Skill Tree
![forkjoin](https://ws2.sinaimg.cn/large/006tNc79gy1fvrnikvp8qj31kw0bqwie.jpg)

## collections
### Queue
* BlockingQueue
    * LinkedBlockingQueue
    * ArrayBlockingQueue
* ConcurrentLinkedQueue
    * 使用循环CAS的方式来实现的非阻塞的线程安全队列
### CopyOnWriteArraySet
* 略
### CopyOnWriteArrayList
* 所有可变操作（add、set等等）都是通过对底层数组进行一次新的复制来实现的
* 适用于读多写少的场景，如缓存
* 不存在“扩容”的概念，每次写操作（add or remove）都要copy一个副本，在副本的基础上修改后改变array引用，所以称为“CopyOnWrite”
### ConcurrentMap
* ConcurrentHashMap
    * 核心技术
        * 锁分段技术
            * 将容器中的数据分段（segment），每段持有一把锁，只有在多个线程访问到同一个段（segment）时，才会出现竞争，从而大大降低了锁竞争的概率，提高了并发性
    * 主要数据结构
        * ConcurrentHashMap
            * DEFAULT_INITIAL_CAPACITY
                * 散列映射表的默认初始容量为 16，即初始默认为 16 个桶
            * DEFAULT_LOAD_FACTOR
                * 散列映射表的默认装载因子为 0.75，该值是 table 中包含的 HashEntry 元素的个数与 table 数组长度的比值。当 table 中包含的 HashEntry 元素的个数超过了 table 数组的长度与装载因子的乘积时，将触发 再散列
            * DEFAULT_CONCURRENCY_LEVEL
                * 散列表的默认并发级别为 16。该值表示当前更新线程的估计数
            * segmentShift 和 segmentMask
                * 定位segment时的哈希算法里需要使用
            * segments
                * 见segment
        * Segment
            * count
                * 在本 segment 范围内，包含的 HashEntry 元素的个数
                * 该变量被声明为 volatile 型
            * modCount
                * table 被更新的次数
            * threshold
                * 当 table 中包含的 HashEntry 元素的个数超过本变量值时，触发 table 的再散列
            * table
                * table 是由 HashEntry 对象组成的数组。如果散列时发生碰撞，碰撞的 HashEntry 对象就以链表的形式链接成一个链表。table 数组的数组成员代表散列映射表的一个桶。每个 table 守护整个 ConcurrentHashMap 包含桶总数的一部分。如果并发级别为 16，table 则守护 ConcurrentHashMap 包含的桶总数的 1/16
            * loadFactor
                * 装载因子
        * HashEntry
            * key
                * key 为 final 型
            * hash
                * hash 值为 final 型
            * value
                * value 为 volatile 型
            * next
                * next 为 final 型
    * 主要操作的实现原理
        * 定位Segment
            * 再哈希
                * 插入和获取元素的时候，会首先使用Wang/Jenkins hash的变种算法对元素的hashCode进行一次再哈希
                * 目的是为了减少哈希冲突，使元素能够均匀的分布在不同的Segment上
        * get操作
            * get操作实现非常简单和高效。先经过一次再哈希，然后使用这个哈希值通过哈希运算定位到segment，再通过哈希算法定位到元素
            * 特点
                * 整个get过程不需要加锁，除非读到的值是空的才会加锁重读
            * 原理
                * value定义为volatile，在线程之间保持可见性，并且由于volatile满足java内存模型的happen before的原则，所以在读写同时发生时会优先进行写操作，所以也可以保证不会读到过期的值。因此整个读过程可以做到无锁。
        * put操作
            * 为了线程安全，在操作共享变量时必须得加锁
            * 操作步骤
                * 第一步判断是否需要对Segment里的HashEntry数组进行扩容
                * 第二步定位添加元素的位置然后放在HashEntry数组里
        * size操作
            * 将每个segment内的count累加求和
            * 问题
                * 计数的时候，如果又有put操作就会导致计数错误
            * 传统的解决方案
                * 锁住整个map，然后进行计数。但是这很低效
            * 更优的解决方案
                * 尝试2次通过不锁住Segment的方式来统计各个Segment大小，如果统计的过程中，容器的count发生了变化，也就是modCount发生了变化，则再采用加锁的方式来统计所有Segment的大小
* ConcurrentSkipListMap
    * 使用频率不高
    * 主要特点
        * 支持排序
        * 支持搜索目标返回最接近匹配项的导航方法
### Skill Tree
![collections](https://ws3.sinaimg.cn/large/006tNc79gy1fvrnkyzpsaj31kw0bewkb.jpg)
![ConcurrentHashMap](https://ws3.sinaimg.cn/large/006tNc79gy1fvrnlbj0htj31kw0ta1kx.jpg)

## locks
### Lock
* 关键实现
    * AbstractQueueSynchronizer（AQS）
* ReentrantLock 可重入锁
    * 支持重进入的锁，表示该锁能够支持一个线程对资源的重复加锁
    * 还支持获取锁的公平与非公平选择
    * 原理
        * 辨识请求锁的线程是否为当前持有锁的线程，如果是则计数加1。释放锁的时候计数器减1，减到0的时候释放锁
### ReadWriteLock 读写锁
ReentrantReadWriteLock 可重入的读写锁
读写锁同一时刻可以允许多个读线程访问，但在写线程访问时，所有读线程和其他写线程都会被阻塞。
* 原理
  * 维护了一对锁，一个读锁（共享锁），一个写锁（排他锁）。每个锁有一个计数器，通过计数器的数值来判断读写锁的状态。读锁和写锁都是可重入的，计数器表示了重入的次数。计数器的实现比较巧妙，是通过一个整型变量的高16位（读锁计数器）和低16位（写锁计数器）来实现的。
  * 读锁counter>0，写锁counter=0，表示该锁处于读状态，写线程需要等待
  * 读锁counter=0，写锁counter>0，表示该锁处于写状态，读线程需要等待
  * 锁降级
    * 锁降级表示写锁降级为读锁
    * 在一个线程获取到写锁以后，它依然可以获取对象的读锁，此时会出现 读锁counter>0，写锁counter>0。
    * 锁降级的设计目的是保障一个线程写完数据后可以准确的获取到它自己写入的数据，避免出现脏读
    * 如果不这么锁，线程A写完数据后先释放写锁，然后再获取读锁，在这个间隙中，对象可能被别的线程B获取到另一个写锁并更新了数据，随后线程A获取读锁的时候，并不是自己写入的数据，造成脏读。

### AbstractQueuedSynchronizer（AQS） 队列同步器
* 设计目的
    * AQS用来构建锁或其他同步组件的基础框架，主要面向锁的开发人员，使用者通过继承和实现抽象方法来完成同步状态管理
* 关键实现
    * 一个int成员变量表示同步状态
    * 一个FIFO队列完成资源获取线程的排队
* 原理
    * 独占式同步队列
        * 一个FIFO的双向队列，用来管理等待的线程。当前线程获取同步状态失败时，就会构造一个节点并加入同步队列。
        * 通过CAS来保障加入队列的线程安全性
        * 处于等待队列中的节点，会通过自旋来不断尝试获取同步状态。判断条件 1. 前驱节点为头节点；2. tryAcquire成功
        * 获取同步状态成功后，会从队列头部出列，并将头节点指向其后节点
    * 共享式同步队列
        * 与独占式同步队列的主要区别在于，节点自旋的过程中，成功获取同步状态并退出自旋的条件是 tryAcquireShared(int arg)返回值大于等于0
### LockSupport.park/unpark
* 用来阻塞（park）或唤醒（unpark）一个线程
### Condition
* 定义了等待/通知 两种类型的方法
### Skill Tree
![locks](https://ws3.sinaimg.cn/large/006tNc79gy1fvrnnqnka3j31kw0g41cy.jpg)

## tools
### CountDownLatch
* 允许一个或多个线程等待其他线程完成操作
* CountDownLatch里有个int类型的参数作为计数器，当调用countDown方法时，N就减1，直到N变为0
### CyclicBarrier
* 可循环使用的屏障。让一组线程达到一个屏障（同步点）时被阻塞，知道最后一个线程达到屏障时，屏障才会开门，所有被拦截的线程才会继续执行
* 与CountDownLatch的区别
    * CountDownLatch只能用一次，而CyclicBarrier可以使用reset()方法重置
### Semaphore
* Semaphore（信号量）是用来控制同时访问特定资源的线程数量
### Exchanger
* 用于线程间交换数据
### Executors
* SingleThreadExecutor
    * 创建一个使用单个线程的线程池
    * 适用场景
        * 需要保证执行顺序的场景，在任意时间点，不会有多个线程
* FixedThreadPool
    * 创建一个使用固定线程数的线程池
    * 适用场景
        * 需要限制线程数的场景，如负载比较重的服务器
* CachedThreadPool
    * 创建一个根据需要创建新线程的线程池
    * 适用场景
        * 大小无界的线程池，执行很多短期异步任务，负载较轻的场景
* ScheduledThreadPool
    * 创建一个可以支持延时和定时执行的线程池
    * 适用场景
        * 主要用来在给定的延迟之后运行任务，或者定期执行任务
### ThreadLocal
* ThreadLocal是一个关于创建线程局部变量的类。通常情况下，我们创建的变量是可以被任何一个线程访问并修改的。而使用ThreadLocal创建的变量只能被当前线程访问，其他线程则无法访问和修改
* 方法
    * 有set()，get()，remove()，initialValue() 4个方法
* 工作原理
    * 1. 首先获取当前线程；
    * 2. 利用当前线程作为句柄获取一个ThreadLocalMap的对象；
    * 3. 如果上述ThreadLocalMap对象不为空，则设置值，否则创建这个ThreadLocalMap对象并设置值
* 线程封闭技术
* 和线程私有变量的区别
    * 线程私有变量是在线程的栈内存里的，ThreadLocal是在堆内存中的。所以ThreadLocal是可以实现线程见共享和可见的。
* 使用场景
    * 实现单个线程单例以及单个线程上下文信息存储，比如交易id等
    * 实现线程安全，非线程安全的对象使用ThreadLocal之后就会变得线程安全，因为每个线程都会有一个对应的实例
    * 承载一些线程相关的数据，避免在方法中来回传递参数
### Skill Tree
![tools](https://ws2.sinaimg.cn/large/006tNc79gy1fvrnp1tltij31kw0lkndh.jpg)

## 线程池
### 解决问题
* 线程生命周期开销大
* 服务器资源不足
### 解决思路
* 池化资源
    * 重复利用已有线程
    * 控制资源占用量
### 优点
* 降低资源消耗
* 提高相应速度
* 提高线程的可管理性
### 适用场景
* 大量短小线程的请求时使用线程池技术可以大大减少线程的创建和销毁次数，提高服务器的工作效率
### 实现模块
* 线程池管理器
    * 创建线程池，销毁线程池，添加新任务
* 工作线程
    * 可以循环执行任务的线程，在没有任务时将等待
* 任务列队
    * 提供缓冲机制，将没有处理的任务放在任务列队中
* 任务接口
    * 为所有任务提供统一的接口，以便工作线程处理。任务接口主要规定了任务的入口，任务执行完后的收尾工作，任务的执行状态等
### 实现细节
* 见ThreadPoolExecutor
### Skill Tree
![threadpool](https://ws1.sinaimg.cn/large/006tNc79gy1fvrnpu54bzj31kw0h6grr.jpg)

## executor
### 介绍
* Executor是一种异步任务执行框架，它基于生产者消费者模式，解耦任务的提交和执行过程，提供了多种不同类型的策略来执行任务
### Executor框架组成部分
#### 任务
* Runnable
    * 接口
        * public abstract void run();
* Callable
    * 接口
        * V call() throws Exception;
* Runnable和Callable的区别
    * Callable有返回值，Runnable没有返回值
    * call() 方法可以抛出异常，run() 方法不可以
    * 运行Callable任务可以拿到一个Future对象
#### 任务执行
> ```java
> class ThreadPoolExecutor extends AbstractExecutorService
> abstract class AbstractExecutorService implements ExecutorService
> interface ExecutorService extends Executor
> ```
* ThreadPoolExecutor
    * 参数
        * corePoolSize
            * 核心线程池大小
                * 核心线程会一直存活，及时没有任务需要执行
                * 当线程数小于核心线程数时，即使有线程空闲，线程池也会优先创建新线程处理
                * 设置allowCoreThreadTimeout=true（默认false）时，核心线程会超时关闭
        * maximumPoolSize
            * 最大线程池大小
                * 当线程数>=corePoolSize，且任务队列已满时。线程池会创建新线程来处理任务
                * 当线程数=maxPoolSize，且任务队列已满时，线程池会拒绝处理任务而抛出异常
        * keepAliveTime
            * 线程活动保持时间
                * 当线程空闲时间达到keepAliveTime时，线程会退出，直到线程数量=corePoolSize
                * 如果allowCoreThreadTimeout=true，则会直到线程数量=0
        * TimeUnit
            * 线程活动保持时间的单位
        * BlockingQueue
            * 任务缓冲队列
        * ThreadFactory
            * 用于设置创建线程的工厂
        * RejectedExecutionHandler
            * 线程池关闭或饱和状态时的处理方法
                * 两种情况会拒绝处理任务
                    * 1. 当线程数已经达到maxPoolSize，切队列已满，会拒绝新任务
                    * 2. 当线程池被调用shutdown()后，会等待线程池里的任务执行完毕，再shutdown。如果在调用shutdown()和线程池真正shutdown之间提交任务，会拒绝新任务
                * 有几个内部实现类来处理这种情况
                    * AbortPolicy 丢弃任务，抛运行时异常
                    * CallerRunsPolicy 执行任务 
                    * DiscardPolicy 忽视，什么都不会发生 
                    * DiscardOldestPolicy 从队列中踢出最先进入队列（最后一个执行）的任务
        * allowCoreThreadTimeout
            * 允许核心线程超时
        * queueCapacity
            * 任务队列容量（阻塞队列）：当核心线程数达到最大时，新任务会放在队列中排队等待执行
    * 参数默认值
        * corePoolSize=1 
        * queueCapacity=Integer.MAX_VALUE 
        * maxPoolSize=Integer.MAX_VALUE 
        * keepAliveTime=60s 
        * allowCoreThreadTimeout=false 
        * rejectedExecutionHandler=AbortPolicy()
    * 参数设置策略 #有很多门道
        * 任务的性质
            * CPU密集型：池尽可能小 N（CPU）+1
            * IO密集型：池尽可能大 2N（CPU）
        * 任务的优先级
            * 有优先级：可使用优先队列
            * 无优先级：非优先队列
        * 任务的执行时间
            * 长
            * 短
        * 任务的依赖性
            * 依赖数据库链接
    * 参数设置策略--另一个版本
        * 需要根据几个值来决定
            * tasks ：每秒的任务数，假设为500~1000 
            * taskcost：每个任务花费时间，假设为0.1s 
            * responsetime：系统允许容忍的最大响应时间，假设为1s 
        * 做几个计算
            * corePoolSize = 每秒需要多少个线程处理？ 
                * threadcount = tasks/(1/taskcost) =tasks*taskcout = (500~1000)*0.1 = 50~100 个线程。corePoolSize设置应该大于50 
                * 根据8020原则，如果80%的每秒任务数小于800，那么corePoolSize设置为80即可 
            * queueCapacity = (coreSizePool/taskcost)*responsetime 
                * 计算可得 queueCapacity = 80/0.1*1 = 80。意思是队列里的线程可以等待1s，超过了的需要新开线程来执行 
                * 切记不能设置为Integer.MAX_VALUE，这样队列会很大，线程数只会保持在corePoolSize大小，当任务陡增时，不能新开线程来执行，响应时间会随之陡增。 
            * maxPoolSize = (max(tasks)- queueCapacity)/(1/taskcost)
                * 计算可得 maxPoolSize = (1000-80)/10 = 92 
                * （最大任务数-队列容量）/每个线程每秒处理能力 = 最大线程数 
            * rejectedExecutionHandler
                * 根据具体情况来决定，任务不重要可丢弃，任务重要则要利用一些缓冲机制来处理 
            * keepAliveTime和allowCoreThreadTimeout采用默认通常能满足
#### 异步计算结果
* Future
    * Future是一个接口，代表可以取消的任务，并可以获得任务的执行结果
* FutureTask
    * 实现了Future和runnable接口
        * 可以把FutureTask实例传入到Thread中，在一个新的线程中执行
        * 可以从FutureTask中通过get取到任务的返回结果，也可以取消任务执行
### CompletionService
* CompletionService实际上可以看做是Executor和BlockingQueue的结合体，用来管理多个线程返回的Future结果
### Skill Tree
![executor](https://ws3.sinaimg.cn/large/006tNc79gy1fvrntznllcj31kw0m8qa6.jpg)
![ThreadPoolExecutor](https://ws2.sinaimg.cn/large/006tNc79gy1fvrnu5gx23j31kw0fe4ox.jpg)

![ThreadPoolExecutor](https://ws2.sinaimg.cn/large/006tNc79gy1fvrnwv2idzj31kw19xtz9.jpg)