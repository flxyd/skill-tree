# 行为型模式-Behavioral Pattern

## 目录
> 1. [职责链模式-Chain of Responsibility Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%81%8C%E8%B4%A3%E9%93%BE%E6%A8%A1%E5%BC%8F-chain-of-responsibility-pattern)
> 2. [命令模式-Command Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E5%91%BD%E4%BB%A4%E6%A8%A1%E5%BC%8F-command-pattern)
> 3. [解释器模式-Interpreter Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A7%A3%E9%87%8A%E5%99%A8%E6%A8%A1%E5%BC%8F-interpreter-pattern)
> 4. [迭代器模式-Iterator Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%BF%AD%E4%BB%A3%E5%99%A8%E6%A8%A1%E5%BC%8F-iterator-pattern)
> 5. [中介者模式-Mediator Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E4%B8%AD%E4%BB%8B%E8%80%85%E6%A8%A1%E5%BC%8F-mediator-pattern)
> 6. [备忘录模式-Memento Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E5%A4%87%E5%BF%98%E5%BD%95%E6%A8%A1%E5%BC%8F-memento-pattern)
> 7. [观察者模式-Observer Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F-observer-pattern)
> 8. [状态模式-State Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E7%8A%B6%E6%80%81%E6%A8%A1%E5%BC%8F-state-pattern)
> 9. [策略模式-Strategy Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8F-strategy-pattern)
> 10. [模板方法模式-Template Method Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E6%A8%A1%E6%9D%BF%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8F-template-method-pattern)
> 11. [访问者模式-Visitor Pattern](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%AE%BF%E9%97%AE%E8%80%85%E6%A8%A1%E5%BC%8F-visitor-pattern)

## 职责链模式-Chain of Responsibility Pattern [:top:](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
### 使用频率
★★☆☆☆
### 目标问题
如何在软件中实现请求链式传递的过程
### 解决方案
避免请求发送者与接收者耦合在一起，让多个对象都有可能接收请求，将这些对象连接成一条链，并且沿着这条链传递请求，直到有对象处理它为止。
### 主要角色
- Handler（抽象处理者）
  - 抽象处理者中定义了一个抽象处理者类型的对象（如结构图中的successor），作为其对下家的引用。通过该引用，处理者可以连成一条链。
- ConcreteHandler（具体处理者）
### 两种类型
- 纯职责链模式
  - 要求一个具体处理者对象只能在两个行为中选择一个：要么承担全部责任，要么将责任推给下家，并且要求一个请求必须被某一个处理者对象所接收
- 不纯职责连模式
  - 允许某个请求被一个具体处理者部分处理后再向下传递，或者一个具体处理者处理完某请求后其后继处理者可以继续处理该请求，而且一个请求可以最终不被任何处理者对象所接收
### 优缺点
- #### 优点
  - 使得一个对象无须知道是其他哪一个对象处理其请求
  - 请求处理对象仅需维持一个指向其后继者的引用
  - 在给对象分派职责时，职责链可以给我们更多的灵活性，可以通过在运行时对该链进行动态的增加或修改来增加或改变处理一个请求的职责
  - 在系统中增加一个新的具体请求处理者时无须修改原有系统的代码，只需要在客户端重新建链即可
- #### 缺点
  - 由于一个请求没有明确的接收者，那么就不能保证它一定会被处理
  - 对于比较长的职责链，请求的处理可能涉及到多个处理对象，系统性能将受到一定影响
  - 如果建链不当，可能会造成循环调用，将导致系统陷入死循环
### 适用场景
有多个对象可以处理同一个请求，具体哪个对象处理该请求待运行时刻再确定
在不明确指定接收者的情况下，向多个对象中的一个提交一个请求
可动态指定一组对象处理请求，客户端可以动态创建职责链来处理请求
### Skill Tree

---
## 命令模式-Command Pattern [:top:](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
### 使用频率
★★★★☆
### 目标问题
请求发送者和请求接受者之间直接耦合，如何降低请求发送者和接收者之间的耦合度，让相同的发送者可以对应不同的接收者
### 解决方案
将一个请求封装为一个对象，从而让我们可用不同的请求对客户进行参数化；对请求排队或者记录请求日志，以及支持可撤销的操作。命令模式是一种对象行为型模式，其别名为动作(Action)模式或事务(Transaction)模式
命令模式可以将请求发送者和接收者完全解耦，发送者与接收者之间没有直接引用关系，发送请求的对象只需要知道如何发送请求，而不必知道如何完成请求。
命令模式的本质是对请求进行封装，一个请求对应于一个命令，将发出命令的责任和执行命令的责任分割开。
### 主要角色
- Command（抽象命令类）
- ConcreteCommand（具体命令类）
- Invoker（调用者）
  - 调用者即请求发送者，它通过命令对象来执行请求
- Receiver（接收者）
### 优缺点
- #### 优点
  - 降低系统的耦合度
  - 新的命令可以很容易地加入到系统中
  - 可以比较容易地设计一个命令队列或宏命令（组合命令）
  - 为请求的撤销(Undo)和恢复(Redo)操作提供了一种设计和实现方案
- #### 缺点
  - 使用命令模式可能会导致某些系统有过多的具体命令类
### 适用场景
系统需要将请求调用者和请求接收者解耦
系统需要在不同的时间指定请求、将请求排队和执行请求
系统需要支持命令的撤销(Undo)操作和恢复(Redo)操作
系统需要将一组操作组合在一起形成宏命令
### Skill Tree

---
## 解释器模式-Interpreter Pattern [:top:](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
### 使用频率
★☆☆☆☆
### 目标问题
如何使用面向对象语言构成一个简单的语言解释器
### 解决方案
定义一个语言的文法，并且建立一个解释器来解释该语言中的句子，这里的“语言”是指使用规定格式和语法的代码。
### 主要角色
- AbstractExpression（抽象表达式）
- TerminalExpression（终结符表达式）
- NonterminalExpression（非终结符表达式）
- Context（环境类）
### 优缺点
- #### 优点
  - 易于改变和扩展文法
  - 每一条文法规则都可以表示为一个类，因此可以方便地实现一个简单的语言
  - 实现文法较为容易
  - 增加新的解释表达式较为方便
- #### 缺点
  - 对于复杂文法难以维护
  - 执行效率较低
### 适用场景
可以将一个需要解释执行的语言中的句子表示为一个抽象语法树。
一些重复出现的问题可以用一种简单的语言来进行表达。
一个语言的文法较为简单。
执行效率不是关键问题。
### Skill Tree

---
## 迭代器模式-Iterator Pattern [:top:](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
### 使用频率
★★★★★
### 目标问题
如何方便地操作存储多个成员对象（元素）的聚合类(Aggregate Classes)，同时可以很灵活地为聚合对象增加不同的遍历方法
### 解决方案
提供一种方法来访问聚合对象，而不用暴露这个对象的内部表示，其别名为游标(Cursor)。迭代器模式是一种对象行为型模式。
### 主要角色
- Iterator（抽象迭代器）
- ConcreteIterator（具体迭代器）
- Aggregate（抽象聚合类）
- ConcreteAggregate（具体聚合类）
### 优缺点
- #### 优点
  - 支持以不同的方式遍历一个聚合对象，在同一个聚合对象上可以定义多种遍历方式
  - 迭代器简化了聚合类
  - 引入了抽象层，增加新的聚合类和迭代器类都很方便
- #### 缺点
  - 设计难度较大，需要充分考虑到系统将来的扩展
### 适用场景
将聚合对象的访问与内部数据的存储分离
需要为一个聚合对象提供多种遍历方式
为遍历不同的聚合结构提供一个统一的接口
### Skill Tree

---
## 中介者模式-Mediator Pattern [:top:](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
### 使用频率
★★☆☆☆
### 目标问题
在有些软件中，某些类/对象之间的相互调用关系错综复杂，如何来协调这些类/对象之间的复杂关系
### 解决方案
用一个中介对象（中介者）来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。中介者模式又称为调停者模式
中介者模式是“迪米特法则”的一个典型应用
### 主要角色
- Mediator（抽象中介者）
  - 职责
    - 中转作用（结构性）
      - 各个同事对象就不再需要显式引用其他同事，当需要和其他同事进行通信时，可通过中介者来实现间接调用。
    - 协调作用（行为性）
      - 对同事之间的关系进行封装，同事可以一致的和中介者进行交互，中介者根据封装在自身内部的协调逻辑，对同事的请求进行进一步处理，将同事成员之间的关系行为进行分离和封装。
- ConcreteMediator（具体中介者）
- Colleague（抽象同事类）
- ConcreteColleague（具体同事类）
### 优缺点
- #### 优点
  - 简化了对象之间的交互
  - 可将各同事对象解耦
  - 减少子类生成
- #### 缺点
  - 具体中介者类中包含了大量同事之间的交互细节，可能会导致具体中介者类非常复杂
### 适用场景
系统中对象之间存在复杂的引用关系，系统结构混乱且难以理解
 一个对象由于引用了其他很多对象并且直接和这些对象通信，导致难以复用该对象
想通过一个中间类来封装多个类中的行为，而又不想生成太多的子类
### Skill Tree

---
## 备忘录模式-Memento Pattern [:top:](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
### 使用频率
★★☆☆☆
### 目标问题
撤销类问题
### 解决方案
在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样可以在以后将对象恢复到原先保存的状态。它是一种对象行为型模式，其别名为Token。
### 主要角色
- Originator（原发器）
- Memento（备忘录)
- Caretaker（负责人）
  - 负责保存备忘录。只负责存储对象，而不能修改对象，也无须知道对象的实现细节
### 优缺点
- #### 优点
  - 提供了一种状态恢复的实现机制
  - 备忘录实现了对信息的封装，一个备忘录对象是一种原发器对象状态的表示，不会被其他代码所改动
- #### 缺点
  - 资源消耗过大，如果需要保存的原发器类的成员变量太多
### 适用场景
保存一个对象在某一个时刻的全部状态或部分状态
防止外界对象破坏一个对象历史状态的封装性
### Skill Tree

---
## 观察者模式-Observer Pattern [:top:](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
### 使用频率
★★★★★
### 目标问题
对象与对象之间存在依赖关系，一个对象发生改变时将自动通知其他对象，其他对象将相应作出反应
### 解决方案
定义对象之间的一种一对多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新。
观察者模式的别名包括发布-订阅（Publish/Subscribe）模式、模型-视图（Model/View）模式、源-监听器（Source/Listener）模式或从属者（Dependents）模式。
### 主要角色
- Subject（目标）
  - 标又称为主题，它是指被观察的对象。在目标中定义了一个观察者集合，提供一系列方法来增加和删除观察者对象，同时它定义了通知方法notify()。
- ConcreteSubject（具体目标）
- Observer（观察者）
  - 观察者将对观察目标的改变做出反应，观察者一般定义为接口，该接口声明了更新数据的方法update()，因此又称为抽象观察者
- ConcreteObserver（具体观察者）
  - 具体观察者中维护一个指向具体目标对象的引用，它存储具体观察者的有关状态，这些状态需要和具体目标的状态保持一致；它实现了在抽象观察者Observer中定义的update()方法。
### JDK的观察者模式
在JDK的java.util包中，提供了Observable类以及Observer接口, 我们可以直接使用Observer接口和Observable类来作为观察者模式的抽象层，再自定义具体观察者类和具体观察目标类
### 优缺点
- #### 优点
  - 实现表示层和数据逻辑层的分离
  - 观察目标和观察者之间建立一个抽象的耦合
  - 观察者模式支持广播通信，观察目标会向所有已注册的观察者对象发送通知，简化了一对多系统设计的难度
- #### 缺点
  - 如果在观察者和观察目标之间存在循环依赖，观察目标会触发它们之间进行循环调用，可能导致系统崩溃
  - 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化
### 适用场景
一个抽象模型有两个方面，其中一个方面依赖于另一个方面，将这两个方面封装在独立的对象中使它们可以各自独立地改变和复用
一个对象的改变将导致一个或多个其他对象也发生改变，而并不知道具体有多少对象将发生改变，也不知道这些对象是谁
### Skill Tree

---
## 状态模式-State Pattern [:top:](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
### 使用频率
★★★☆☆
### 目标问题
在对象具有状态时，如何可以减少条件判断的语句，并且能灵活的支持新增的状态
### 解决方案
允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类
### 主要角色
- Context（环境类）
- State（抽象状态类）
- ConcreteState（具体状态类）
两种实现状态转换的方式
- 统一由环境类来负责状态之间的转换
  - 环境类充当了状态管理器(State Manager)角色，通过对某些属性值的判断实现状态转换
- 由具体状态类来负责状态之间的转换
  - 判断环境类的某些属性值再根据情况为环境类设置新的状态对象，实现状态转换
### 衍生问题
共享状态
- 目标问题
  - 如果某系统要求两个开关对象要么都处于开的状态，要么都处于关的状态，在使用时它们的状态必须保持一致，开关可以由开转换到关，也可以由关转换到开。
  - 如果希望在系统中实现多个环境对象共享一个或多个状态对象，那么需要将这些状态对象定义为环境类的静态成员对象
### 优缺点
- #### 优点
  - 封装了状态的转换规则
  - 将所有与某个状态有关的行为放到一个类中
  - 允许状态转换逻辑与状态对象合成一体，而不是提供一个巨大的条件语句块
  - 让多个环境对象共享一个状态对象
- #### 缺点
  - 如果使用不当将导致程序结构和代码的混乱，增加系统设计的难度
  - 对“开闭原则”的支持并不太好
### 适用场景
当系统中某个对象存在多个状态，这些状态之间可以进行转换，而且对象在不同状态下行为不相同时可以使用状态模式
### Skill Tree

---
## 策略模式-Strategy Pattern [:top:](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
### 使用频率
★★★★☆
### 目标问题
实现某一个功能有多条途径，每一条途径对应一种算法，如何设计系统来进行灵活的选择算法
### 解决方案
定义一系列算法类，将每一个算法封装起来，并让它们可以相互替换，策略模式让算法独立于使用它的客户而变化，也称为政策模式(Policy)。
策略模式的主要目的是将算法的定义与使用分开，也就是将算法的行为和环境分开
策略模式提供了一种可插入式(Pluggable)算法的实现方案
### 主要角色
- Context（环境类）
  - 环境类是使用算法的角色，它在解决某个问题（即实现某个方法）时可以采用多种策略。
- Strategy（抽象策略类）
- ConcreteStrategy（具体策略类）
### 优缺点
- #### 优点
  - 策略模式提供了对“开闭原则”的完美支持，用户可以在不修改原有系统的基础上选择算法或行为，也可以灵活地增加新的算法或行为
  - 策略模式提供了管理相关的算法族的办法
  - 提供了一种可以替换继承关系的办法
  - 可以避免多重条件选择语句
  - 提供了一种算法的复用机制
- #### 缺点
  - 客户端必须知道所有的策略类，并自行决定使用哪一个策略类
  - 策略模式将造成系统产生很多具体策略类
  - 无法同时在客户端使用多个策略类
### 适用场景
动态地在几种算法中选择一种
不希望客户端知道复杂的、与算法相关的数据结构
### Skill Tree

---
## 模板方法模式-Template Method Pattern [:top:](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
### 使用频率
★★★☆☆
### 目标问题
某个方法的实现需要多个步骤（类似“请客”），其中有些步骤是固定的（类似“点单”和“买单”），而有些步骤并不固定，存在可变性（类似“吃东西”），如何处理这样的情况
### 解决方案
定义一个操作中算法的框架，而将一些步骤延迟到子类中。模板方法模式使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。
模板方法模式是一种基于继承的代码复用技术，它是一种类行为型模式
### 主要角色
- AbstractClass（抽象类）
  - 模板方法
    - 定义在抽象类中的、把基本操作方法组合在一起形成一个总算法或一个总行为的方法
  - 基本方法
    - 基本方法是实现算法各个步骤的方法，是模板方法的组成部分。基本方法又可以分为一下三种：
    - 抽象方法(Abstract Method)
    - 具体方法(Concrete Method)
    - 钩子方法(Hook Method)
      - 钩子方法由一个抽象类或具体类声明并实现，而其子类可能会加以扩展。通常在父类中给出的实现是一个空实现（可使用virtual关键字将其定义为虚函数），并以该空实现作为方法的默认实现，当然钩子方法也可以提供一个非空的默认实现
- ConcreteClass（具体子类）
### 优缺点
- #### 优点
  - 实现一种反向控制结构，通过子类覆盖父类的钩子方法来决定某一特定步骤是否需要执行
  - 在父类中形式化地定义一个算法，而由它的子类来实现细节的处理
  - 不同的子类可以提供基本方法的不同实现，更换和增加新的子类很方便
- #### 缺点
  - 如果父类中可变的基本方法太多，将会导致类的个数增加
### 适用场景
对一些复杂的算法进行分割，将其算法中固定不变的部分设计为模板方法和父类具体方法
各子类中公共的行为应被提取出来并集中到一个公共父类中以避免代码重复
需要通过子类来决定父类算法中某个步骤是否执行，实现子类对父类的反向控制
### Skill Tree

---
## 访问者模式-Visitor Pattern [:top:](https://github.com/flxyd/skill-tree/blob/master/designPattern/BehavioralPattern.md#%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F-behavioral-pattern)
### 使用频率
★☆☆☆☆
### 目标问题
如何为同一集合对象中的元素提供多种不同的操作方式
### 解决方案
提供一个作用于某对象结构中的各元素的操作表示，它使我们可以在不改变各元素的类的前提下定义作用于这些元素的新操作
### 主要角色
- Vistor（抽象访问者）
- ConcreteVisitor（具体访问者）
- Element（抽象元素）
- ConcreteElement（具体元素）
- ObjectStructure（对象结构）
### 优缺点
- #### 优点
  - 增加新的访问操作很方便
  - 将有关元素对象的访问行为集中到一个访问者对象中，而不是分散在一个个的元素类中
  - 让用户能够在不修改现有元素类层次结构的情况下，定义作用于该层次结构的操作
- #### 缺点
  - 增加新的元素类很困难
  - 破坏封装
### 适用场景
一个对象结构包含多个类型的对象，希望对这些对象实施一些依赖其具体类型的操作
需要对一个对象结构中的对象进行很多不同的并且不相关的操作
对象结构中对象对应的类很少改变，但经常需要在此对象结构上定义新的操作
