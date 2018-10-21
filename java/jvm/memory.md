# 内存（运行时数据区域）

## 线程私有

### 程序计数器 PC
* 记录正在执行的虚拟机字节码的地址
* 每个线程都有自己计数器，是私有内存空间，该区域是整个内存中较小的一块

### 虚拟机栈 VM Stack
* 方法执行的内存区，每个方法执行时会在虚拟机栈中创建栈帧
* 生命周期与线程相同，是Java方法执行的内存模型
* 每个方法(不包含native方法)执行的同时都会创建一个栈帧结构
* 栈帧 Stack Frame
    * 局部变量表
        * locals大小，编译期确定
        * 一组变量存储空间， 容量以slot为最小单位
    * 操作栈
        * stack大小，编译期确定
        * 操作栈元素的数据类型必须与字节码指令序列严格匹配
    * 方法返回地址
        * 正常退出
            * 虚拟机规范没有明确规定，由具体虚拟机实现
        * 异常退出
            * 遇到Exception,并且方法未捕捉异常，那么不会有任何返回值
            * StackOverFlowError
                * 线程请求栈深度超出虚拟机栈所允许的深度时抛出
            * OutOfMemoryError
                * Java虚拟机动态扩展到无法申请足够内存时抛出
    * 动态连接
        * 指向运行时常量池中该栈帧所属方法的引用
    * 额外附加信息
        * 虚拟机规范没有明确规定，由具体虚拟机实现

### 本地方法栈 Native Method Stack
* 虚拟机的Native方法执行的内存区

## 共享数据

### 堆 Heap
* 对象分配内存的区域

### 方法区 Method Area
* 常量池
    * 存放编译器生成的各种字面量和符号引用，是方法区的一部分
* 存放类信息、常量、静态变量、编译器编译后的代码等数据

## Skill Tree
![memory](https://ws1.sinaimg.cn/large/006tNbRwgy1fw0lttkxgvj31kw0oggz6.jpg)