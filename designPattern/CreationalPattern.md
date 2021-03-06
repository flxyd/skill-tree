# 创建型模式-Creational Pattern

## 目录
> 1. [简单工厂模式-Simple Factory Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/CreationalPattern.md#%E7%AE%80%E5%8D%95%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8F-simple-factory-pattern)
> 2. [工厂方法模式-Factory Method Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/CreationalPattern.md#%E5%B7%A5%E5%8E%82%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8F-factory-method-pattern)
> 3. [抽象工厂模式-Abstract Factory Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/CreationalPattern.md#%E6%8A%BD%E8%B1%A1%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8F-abstract-factory-pattern)
> 4. [单例模式-Singleton Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/CreationalPattern.md#%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F-singleton-pattern)
> 5. [原型模式-Prototype Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/CreationalPattern.md#%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8F-prototype-pattern)
> 6. [建造者模式-Builder Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/CreationalPattern.md#%E5%BB%BA%E9%80%A0%E8%80%85%E6%A8%A1%E5%BC%8F-builder-pattern)

---
## 简单工厂模式-Simple Factory Pattern
### 使用频率
★★★☆☆
### 目标问题
用于多个解决同一类问题的行为相似的类的统一创建
### 解决方案
一个工厂类可以根据参数的不同返回不同类的实例，被创建的实例通常都具有共同的父类。
因为在简单工厂模式中用于创建实例的方法是静态(static)方法，因此简单工厂模式又被称为静态工厂方法(Static Factory Method)模式
#### 主要角色
- Factory（工厂角色）
- Product（抽象产品角色）
- ConcreteProduct（具体产品角色）
### 优缺点
#### 优点
- 实现了对象创建和使用的分离
- 用户只需要知道创建参数即可
- 通过配置文件可以灵活增改产品
#### 缺点
- 工厂类集中了所有产品的创建逻辑，职责过重
- 由于是静态工厂方法，所以无法形成基于继承的等级结构
### 适用场景
工厂类负责创建的对象比较少
客户端只知道传入工厂类的参数
### skill tree
![简单工厂模式-Simple Factory Pattern](https://ws3.sinaimg.cn/large/0069RVTdgy1fv6pnduqk3j31kw0fgq74.jpg)

---
## 工厂方法模式-Factory Method Pattern
### 使用频率
★★★★★
### 目标问题
简单工厂模式在新接入产品时都需要修改工厂类，违反了开闭原则，最终将导致工厂类庞大难以维护。
### 解决方案
定义一个用于创建对象的接口，让子类决定将哪一个类实例化。工厂方法模式让一个类的实例化延迟到其子类。
工厂方法模式又简称为工厂模式(Factory Pattern)，又可称作虚拟构造器模式(Virtual Constructor Pattern)或多态工厂模式(Polymorphic Factory Pattern)
#### 主要角色
- Factory（抽象工厂）
- ConcreteFactory（具体工厂）
- Product（抽象产品角色）
- ConcreteProduct（具体产品角色）
### 优缺点
#### 优点
- 用户只需要关心所需产品对应的工厂
- 工厂角色和产品角色的多态性设计是工厂方法模式的关键，可以屏蔽产品创建细节
- 可扩展性非常好，符合“开闭原则”
#### 缺点
- 每个产品都要有个相对应的工厂类
- 一般会用到配置文件，增加了理解的复杂性
### 适用场景
客户端不需要知道它所需要的具体产品的类
抽象工厂类需要通过其子类来指定创建哪个对象
### skill tree
![工厂方法模式-Factory Method Pattern](https://ws3.sinaimg.cn/large/0069RVTdgy1fv6ptjcnfvj31kw0gj795.jpg)

---
## 抽象工厂模式-Abstract Factory Pattern
### 使用频率
★★★★★
### 目标问题
工厂方法模式会导致系统中存在大量的工厂类，每增加一种产品就要增加大量的类。此外如果产品之间有关联（产品族），工厂方法模式创建这一组关联的产品比较繁琐。
### 解决方案
提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类。 抽象工厂模式为创建一组对象提供了一种解决方案。与工厂方法模式相比，抽象工厂模式中的具体工厂不只是创建一种产品，它负责创建一族产品。
抽象工厂模式又称为Kit模式
#### 主要角色
- AbstractFactory（抽象工厂）
- ConcreteFactory（具体工厂）
- AbstractProduct（抽象产品）
- ConcreteProduct（具体产品）
### 优缺点
#### 优点
- 抽象工厂模式隔离了具体类的生成，客户端不需要知道什么被创建
- 创建一个产品族的产品， 可以保证客户端只使用同一个族中的产品
- 增加新的产品族很方便，无须修改已有系统，符合“开闭原则”
#### 缺点
- 增加新的产品等级结构麻烦，违背了“开闭原则”
### 适用场景
用户不关心对象的创建过程
系统中有多于一个的产品族，而每次只使用其中某一产品族
属于同一个产品族的产品将在一起使用
产品等级结构稳定，不会向系统中增删产品等级结构
### skill tree
![抽象工厂模式-Abstract Factory Pattern](https://ws2.sinaimg.cn/large/0069RVTdgy1fv6pvuhex7j31kw0gx443.jpg)

---
## 单例模式-Singleton Pattern
### 使用频率
★★★★☆
### 目标问题
某些场景中某些类需要或只能被创建一次
### 解决方案
确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例，这个类称为单例类，它提供全局访问的方法。
三个要点
- 某个类只能有一个实例
- 它必须自行创建这个实例
- 它必须自行向整个系统提供这个实例
#### 主要角色
- Singleton（单例）
### 衍生问题
多线程环境下，如果初始化创建实例的方法执行比较慢，会存在多个线程同时出发初始化单例类的场景，会导致创建了多个单例对象
解决方案
- 饿汉模式
  - 将构造对象赋值到保存类实例的静态变量上，当类被加载时，静态变量就会被自动初始化。
  - 示例: private static final EagerSingleton instance = new EagerSingleton();
- 懒汉模式
  - 就是懒加载，创建实例的方法使用同步机制处理（synchronize），保存类的变量要声明为volatile。注意同步机制需要使用双重检查锁定(Double-Check Locking)来保障不会重复创建实例，也就是在后面的线程获取到锁进入方法体后，需要再次确定实例是否不等于null。
- 比较
  - 饿汉模式调用速度更快，但是会额外占用资源；懒汉模式用时才创建，更节省资源，但是判断和加锁动作会使得创建速度变慢
- 更好的解决方案
  - Initialization on Demand Holder (IoDH)
  - 结合饿汉模式和懒汉模式，在单例类中增加一个静态(static)内部类，如同饿汉模式一样，在该内部类中创建单例对象，由于静态内部类在使用的时候才会初始化，所以等到调用这个静态内部类时才会触发类加载，达到了懒汉模式的懒加载效果
### 优缺点
#### 优点
- 提供了对唯一实例的受控访问
- 节约系统资源
- 允许可变数目的实例
#### 缺点
- 单例模式中没有抽象层，扩展困难
- 单例类的职责过重，在一定程度上违背了“单一职责原则”
### 适用场景
系统只需要一个实例对象
客户调用类的单个实例只允许使用一个公共访问点
### skill tree
![单例模式-Singleton Pattern](https://ws3.sinaimg.cn/large/0069RVTdgy1fv6ptxkw6xj31kw0o6dp7.jpg)

---
## 原型模式-Prototype Pattern
### 使用频率
★★★☆☆
### 目标问题
如何在一个面向对象系统中实现对象的复制和粘贴
### 解决方案
使用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象
#### 主要角色
- Prototype（抽象原型类）
- ConcretePrototype（具体原型类）
- Client（客户类）
### 优缺点
#### 优点
- 当创建新的对象实例较为复杂时，使用原型模式可以简化对象的创建过程
- 原型模式提供了简化的创建结构，原型模式中产品的复制是通过封装在原型类中的克隆方法实现的，无须专门的工厂类来创建产品
- 可以使用深克隆的方式保存对象的状态
#### 缺点
- 需要为每一个类配备一个克隆方法，而且该克隆方法位于一个类的内部，当对已有的类进行改造时，需要修改源代码，违背了“开闭原则”
- 在实现深克隆时需要编写较为复杂的代码
### 适用场景
创建新对象成本较大，新的对象可以通过原型模式对已有对象进行复制来获得，如果是相似对象，则可以对其成员变量稍作修改
如果系统要保存对象的状态，而对象的状态变化很小，或者对象本身占用内存较少时，可以使用原型模式配合备忘录模式来实现
### skill tree
![原型模式-Prototype Pattern](https://ws4.sinaimg.cn/large/0069RVTdgy1fv6pu0oiyjj31kw0lsk10.jpg)

---
## 建造者模式-Builder Pattern
### 使用频率
★★☆☆☆
### 目标问题
如何将创建包含多个对象的组合对象
### 解决方案
将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示
#### 主要角色
- Builder（抽象建造者）
- ConcreteBuilder（具体建造者）
- Product（产品角色）
- Director（指挥者）
  - 指挥者又称为导演类，它负责安排复杂对象的建造次序
    - 作用
      - 隔离客户与创建过程
       - 控制产品的创建过程
### 优缺点
#### 优点
- 客户端不必知道产品内部组成的细节，将产品本身与产品的创建过程解耦
- 每一个具体建造者都相对独立，而与其他的具体建造者无关
- 可以更加精细地控制产品的创建过程
#### 缺点
- 如果产品之间的差异性很大，不适合使用建造者模式
- 如果产品的内部变化复杂，可能会导致需要定义很多具体建造者类来实现这种变化
### 适用场景
需要生成的产品对象有复杂的内部结构，这些产品对象通常包含多个成员属性。
需要生成的产品对象的属性相互依赖，需要指定其生成顺序。
对象的创建过程独立于创建该对象的类。
### skill tree
![建造者模式-Builder Pattern](https://ws4.sinaimg.cn/large/0069RVTdgy1fv6pu9hzo5j31kw0osakh.jpg)
