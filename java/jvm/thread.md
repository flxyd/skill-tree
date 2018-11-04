
# 线程

## 内存模型

### Java内存模型
* JMM(Java Memory Model) 用来屏蔽各种硬件和操作系统的内存访问差异
* JMM与运行时数据区域（堆、栈、方法区等）不是一个层次的内存划分，两者没有太大关系
* 所有变量都存储在主内存中，每个线程有一个自己的工作内存
* 内存的交互操作
    * lock（锁定）
        * 作用于主内存
        * 一个变量标识为线程独占
    * unlock（解锁）
        * 作用于主内存
        * 一个锁定状态的变量释放出来
    * read（读取）
        * 作用于主内存
        * 把一个变量的值从主内存传输到工作内存
    * load（载入）
        * 作用于工作内存
        * 把read到工作内存的变量值放入工作内存的变量副本中
    * use（使用）
        * 作用于工作内存
        * 把变量值传递给执行引擎
    * assign（赋值）
        * 作用于工作内存
        * 把从执行引擎得到的值赋给工作内存的变量
    * store（存储）
        * 作用于工作内存
        * 把工作内存的变量值传送到主内存中
    * write（写入）
        * 作用于主内存

        * 把store得到的变量值放入到主内存的变量中

![内存模型](https://ws3.sinaimg.cn/large/006tNbRwly1fww9rjn7l7j31kw1mc7m4.jpg)

## volatile

### 定位
* 最轻量的同步机制

### 特性
* 被修饰的变量对所有线程「可见」
    * volatile变量在各个线程的工作内存中不存在一致性问题
    * 因为每次使用volatile变量之前都需要刷新
* 禁止指令重排序优化
    * 使用内存屏障
    * 1、当执行到volatile变量的读写操作时，在其前面的操作已经全部进行，且结果已经对后面的操作可见，在其后面的操作肯定还没有进行
    * 2、在进行指令优化时，不能将在对volatile变量访问的语句放在其后面执行，也不能把volatile变量后面的语句放到其前面执行

![volatile](https://ws4.sinaimg.cn/large/006tNbRwly1fww9scb4aaj31jk0lw79b.jpg)



## 内存模型的有序性

* 在本线程内观察，所有操作都是有序的
	* 线程内表现为串行的语义（Within-Thread As-If-Serial Semantics）

* 在一个线程中观察另一个线程，所有操作都是无序的
	* 指令重排序
	* 工作内存与主内存同步延时

![内存模型的有序性](https://ws3.sinaimg.cn/large/006tNbRwly1fww9sptclhj31ji0f20va.jpg)

## 先行发生（happens-before）

* 判断数据是否存在竞争、线程是否安全的主要依据

## 线程实现

### 使用内核线程实现
* 基于内核线程（Kernel-Level Thread，KLT）的高级接口——轻量级进程（Light Weight Process，LWP）来实现
* 优点
    * 基于KLT，实现较为简单，不用管理线程的创建、切换、调度等问题
* 缺点
    * 系统调用代价高，需要在用户态和内核台中来回切换
    * 比较消耗内核资源

### 使用用户线程实现
* 不使用LWP，完全由建立在用户空间上实现的用户线程（User Thread，UT）
* 优点
    * 非常快速切低消耗，可以支持规模更大的线程数量
* 缺点
    * 线程的生命周期全都要自己管理，实现非常复杂

### 使用用户线程+轻量级进程混合实现
* 混合使用了LWP和UT，LWP作为UT和KLT之间的桥梁
* 优点
    * 线程切换廉价，支持大规模线程数量
    * 可以使用内核提供的线程调度功能及处理器映射

![线程实现](https://ws1.sinaimg.cn/large/006tNbRwly1fww9svum6tj31kw0tw46c.jpg)

## 线程状态

### 5种状态
* 新建 New
    * 创建后未启动
* 运行 Runable
    * 正在执行或正在等待CPU分配时间
* 无限期等待 Waiting
    * 不会被CPU分配时间，除非被显示的唤醒
* 限期等待 Timed Waiting
    * 不会被CPU分配时间，在一定时间后会由系统自动唤醒
* 阻塞 Blocked
    * 进入同步区域，等待一个排他锁
* 结束 Terminated
    * 已终止

![线程状态](https://ws1.sinaimg.cn/large/006tNbRwly1fww9ud34znj31kw0w5k0h.jpg)