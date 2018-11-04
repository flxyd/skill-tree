# 类加载

## 加载时机

### 类的生命周期
* 加载（Loading）
* 连接（linking）
    * 验证（Verification）
    * 准备（Preparation）
    * 解析（Resolution）
* 初始化（Initialization）
* 使用（Using）
* 卸载（UNloading）

### 加载的时机？
* 没有规定，虚拟机规范没有强制约束

### 初始化时机？
* 有明确规定
  有且只有5中情况
    * 前提
        * 一个类没有初始化
    * 5种情况
        * 遇到new和静态有关的指令时
            * 用new关键字实例化对象时
            * 读或写一个类的静态字段时
            * 调用一个类的静态方法时
        * 进行反射调用时
        * 当前类的父类还没有初始化时
        * 虚拟机启动时会初始化主类（包含main函数的类）
        * 使用动态语言时
          ![加载时机](https://ws3.sinaimg.cn/large/006tNbRwly1fwwa1hsmlwj31iy0xctf4.jpg)

## 加载过程

### 加载
* 全限定名获取类的二进制字节流
* 将字节流代表的静态存储结构转化为方法区的运行时数据结构
* 内存中生成一个代表该类的java.lang.Class对象

### 验证
* 目的
    * 确保Class文件字节流正确
    * 不会危害虚拟机本身
* 步骤
    * 文件格式验证
    * 元数据验证
    * 字节码验证
    * 符号引用验证

### 准备
* 给类变量（被static修饰的变量）在方法区里分配内存

### 解析
* 将常量池内的符号引用替换为直接引用

### 初始化
* 执行类定义里的java代码（如静态变量赋值）
  ![加载过程](https://ws1.sinaimg.cn/large/006tNbRwly1fwwa0zaq74j315y0wcjx3.jpg)

## 类加载器

### 类型
* 启动类加载器（Bootstrap ClassLoader）
    * 负责加载<JAVA_HOME>\lib中的或-Xbootclasspath指定的类库
* 扩展类加载器（Extension ClassLoader）
    * 负责加载<JAVA_HOME>\lib\ext中的或被java.ext.dirs指定的类库
* 应用程序类加载器（Application ClassLoader）
    * 负责加载用户类路径（ClassPath）上的类库

### 双亲委派模型
* 设计目的
    * 防止同样的类在虚拟机中存在不同实现版本
    * 从而保障Java程序稳定运作
* 工作机制
    * 收到类加载请求
    * 请求委派给父类的加载器
    * 一直传到顶层的启动类加载器
    * 父层无法加载子类尝试自己加载

![类加载器](https://ws3.sinaimg.cn/large/006tNbRwly1fwwa1ifq6oj31kw0htgqp.jpg)