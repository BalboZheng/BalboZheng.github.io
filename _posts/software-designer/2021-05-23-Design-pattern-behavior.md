---
layout:     post
title:      "设计模式-行为模式"
subtitle:   
date:       2021-05-23 20:54:10
author:     "Balbo"
header-img: "img/post-bg-2021.jpg"
tags:
    - software-designer
---

## 策略模式

### 策略模式的定义与特点

策略（Strategy）模式的定义：该模式定义了一系列算法，并将每个算法封装起来，使它们可以相互替换，且算法的变化不会影响使用算法的客户。策略模式属于对象行为模式，它通过对算法进行封装，把使用算法的责任和算法的实现分割开来，并委派给不同的对象对这些算法进行管理。

#### 策略模式的主要优点如下。

1. 多重条件语句不易维护，而使用策略模式可以避免使用多重条件语句，如 if...else 语句、switch...case 语句。
2. 策略模式提供了一系列的可供重用的算法族，恰当使用继承可以把算法族的公共代码转移到父类里面，从而避免重复的代码。
3. 策略模式可以提供相同行为的不同实现，客户可以根据不同时间或空间要求选择不同的。
4. 策略模式提供了对开闭原则的完美支持，可以在不修改原代码的情况下，灵活增加新算法。
5. 策略模式把算法的使用放到环境类中，而算法的实现移到具体策略类中，实现了二者的分离。


#### 其主要缺点如下。

1. 客户端必须理解所有策略算法的区别，以便适时选择恰当的算法类。
2. 策略模式造成很多的策略类，增加维护难度。

### 策略模式的结构与实现

策略模式是准备一组算法，并将这组算法封装到一系列的策略类里面，作为一个抽象策略类的子类。策略模式的重心不是如何实现算法，而是如何组织这些算法，从而让程序结构更加灵活，具有更好的维护性和扩展性，现在我们来分析其基本结构和实现方法。

1. 模式的结构

   策略模式的主要角色如下。

   1. 抽象策略（Strategy）类：定义了一个公共接口，各种不同的算法以不同的方式实现这个接口，环境角色使用这个接口调用不同的算法，一般使用接口或抽象类实现。
   2. 具体策略（Concrete Strategy）类：实现了抽象策略定义的接口，提供具体的算法实现。
   3. 环境（Context）类：持有一个策略类的引用，最终给客户端调用。


   其结构图如图 1 所示。

   

   ![策略模式的结构图](http://c.biancheng.net/uploads/allimg/181116/3-1Q116103K1205.gif)

   <center>图1 策略模式的结构图</center>

2. 模式的实现

   策略模式的实现代码如下：

   ```java
   public class StrategyPattern {
       public static void main(String[] args) {
           Context c = new Context();
           Strategy s = new ConcreteStrategyA();
           c.setStrategy(s);
           c.strategyMethod();
           System.out.println("-----------------");
           s = new ConcreteStrategyB();
           c.setStrategy(s);
           c.strategyMethod();
       }
   }
   
   //抽象策略类
   interface Strategy {
       public void strategyMethod();    //策略方法
   }
   
   //具体策略类A
   class ConcreteStrategyA implements Strategy {
       public void strategyMethod() {
           System.out.println("具体策略A的策略方法被访问！");
       }
   }
   
   //具体策略类B
   class ConcreteStrategyB implements Strategy {
       public void strategyMethod() {
           System.out.println("具体策略B的策略方法被访问！");
       }
   }
   
   //环境类
   class Context {
       private Strategy strategy;
   
       public Strategy getStrategy() {
           return strategy;
       }
   
       public void setStrategy(Strategy strategy) {
           this.strategy = strategy;
       }
   
       public void strategyMethod() {
           strategy.strategyMethod();
       }
   }
   ```

   程序运行结果如下：

   ```tex
   具体策略A的策略方法被访问！
   -----------------
   具体策略B的策略方法被访问！
   ```


## 模板方法模式

### 模式的定义与特点

模板方法（Template Method）模式的定义如下：定义一个操作中的算法骨架，而将算法的一些步骤延迟到子类中，使得子类可以不改变该算法结构的情况下重定义该算法的某些特定步骤。它是一种类行为型模式。

#### 该模式的主要优点如下。

1. 它封装了不变部分，扩展可变部分。它把认为是不变部分的算法封装到父类中实现，而把可变部分算法由子类继承实现，便于子类继续扩展。
2. 它在父类中提取了公共的部分代码，便于代码复用。
3. 部分方法是由子类实现的，因此子类可以通过扩展方式增加相应的功能，符合开闭原则。


#### 该模式的主要缺点如下。

1. 对每个不同的实现都需要定义一个子类，这会导致类的个数增加，系统更加庞大，设计也更加抽象，间接地增加了系统实现的复杂度。
2. 父类中的抽象方法由子类实现，子类执行的结果会影响父类的结果，这导致一种反向的控制结构，它提高了代码阅读的难度。
3. 由于继承关系自身的缺点，如果父类添加新的抽象方法，则所有子类都要改一遍。

### 模式的结构与实现

模板方法模式需要注意抽象类与具体子类之间的协作。它用到了虚函数的多态性技术以及“不用调用我，让我来调用你”的反向控制技术。现在来介绍它们的基本结构。

1. 模式的结构

   模板方法模式包含以下主要角色。

   #### 1）抽象类/抽象模板（Abstract Class）

   抽象模板类，负责给出一个算法的轮廓和骨架。它由一个模板方法和若干个基本方法构成。这些方法的定义如下。

   ① 模板方法：定义了算法的骨架，按某种顺序调用其包含的基本方法。

   ② 基本方法：是整个算法中的一个步骤，包含以下几种类型。

   - 抽象方法：在抽象类中声明，由具体子类实现。
   - 具体方法：在抽象类中已经实现，在具体子类中可以继承或重写它。
   - 钩子方法：在抽象类中已经实现，包括用于判断的逻辑方法和需要子类重写的空方法两种。

   #### 2）具体子类/具体实现（Concrete Class）

   具体实现类，实现抽象类中所定义的抽象方法和钩子方法，它们是一个顶级逻辑的一个组成步骤。

   模板方法模式的结构图如图 1 所示。

   

   ![模板方法模式的结构图](http://c.biancheng.net/uploads/allimg/181116/3-1Q116095405308.gif)

   <center>图1 模板方法模式的结构图</center>

2. 模式的实现

   模板方法模式的代码如下：

   ```java
   public class TemplateMethodPattern {
       public static void main(String[] args) {
           AbstractClass tm = new ConcreteClass();
           tm.TemplateMethod();
       }
   }
   
   //抽象类
   abstract class AbstractClass {
       //模板方法
       public void TemplateMethod() {
           SpecificMethod();
           abstractMethod1();
           abstractMethod2();
       }
   
       //具体方法
       public void SpecificMethod() {
           System.out.println("抽象类中的具体方法被调用...");
       }
   
       //抽象方法1
       public abstract void abstractMethod1();
   
       //抽象方法2
       public abstract void abstractMethod2();
   }
   
   //具体子类
   class ConcreteClass extends AbstractClass {
       public void abstractMethod1() {
           System.out.println("抽象方法1的实现被调用...");
       }
   
       public void abstractMethod2() {
           System.out.println("抽象方法2的实现被调用...");
       }
   }
   ```

   程序的运行结果如下：

   ```tex
   抽象类中的具体方法被调用...
   抽象方法1的实现被调用...
   抽象方法2的实现被调用...
   ```


## 观察者模式

### 模式的定义与特点

观察者（Observer）模式的定义：指多个对象间存在一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。这种模式有时又称作发布-订阅模式、模型-视图模式，它是对象行为型模式。

#### 观察者模式是一种对象行为型模式，其主要优点如下。

1. 降低了目标与观察者之间的耦合关系，两者之间是抽象耦合关系。符合依赖倒置原则。
2. 目标与观察者之间建立了一套触发机制。


#### 它的主要缺点如下。

1. 目标与观察者之间的依赖关系并没有完全解除，而且有可能出现循环引用。
2. 当观察者对象很多时，通知的发布会花费很多时间，影响程序的效率。

### 模式的结构与实现

实现观察者模式时要注意具体目标对象和具体观察者对象之间不能直接调用，否则将使两者之间紧密耦合起来，这违反了面向对象的设计原则。

1. 模式的结构

   观察者模式的主要角色如下。

   1. 抽象主题（Subject）角色：也叫抽象目标类，它提供了一个用于保存观察者对象的聚集类和增加、删除观察者对象的方法，以及通知所有观察者的抽象方法。
   2. 具体主题（Concrete Subject）角色：也叫具体目标类，它实现抽象目标中的通知方法，当具体主题的内部状态发生改变时，通知所有注册过的观察者对象。
   3. 抽象观察者（Observer）角色：它是一个抽象类或接口，它包含了一个更新自己的抽象方法，当接到具体主题的更改通知时被调用。
   4. 具体观察者（Concrete Observer）角色：实现抽象观察者中定义的抽象方法，以便在得到目标的更改通知时更新自身的状态。


   观察者模式的结构图如图 1 所示。

   

   ![观察者模式的结构图](http://c.biancheng.net/uploads/allimg/181116/3-1Q1161A6221S.gif)

   <center>图1 观察者模式的结构图</center>

2. 模式的实现

   观察者模式的实现代码如下：

   ```java
   package net.biancheng.c.observer;
   
   import java.util.*;
   
   public class ObserverPattern {
       public static void main(String[] args) {
           Subject subject = new ConcreteSubject();
           Observer obs1 = new ConcreteObserver1();
           Observer obs2 = new ConcreteObserver2();
           subject.add(obs1);
           subject.add(obs2);
           subject.notifyObserver();
       }
   }
   
   //抽象目标
   abstract class Subject {
       protected List<Observer> observers = new ArrayList<Observer>();
   
       //增加观察者方法
       public void add(Observer observer) {
           observers.add(observer);
       }
   
       //删除观察者方法
       public void remove(Observer observer) {
           observers.remove(observer);
       }
   
       public abstract void notifyObserver(); //通知观察者方法
   }
   
   //具体目标
   class ConcreteSubject extends Subject {
       public void notifyObserver() {
           System.out.println("具体目标发生改变...");
           System.out.println("--------------");
   
           for (Object obs : observers) {
               ((Observer) obs).response();
           }
   
       }
   }
   
   //抽象观察者
   interface Observer {
       void response(); //反应
   }
   
   //具体观察者1
   class ConcreteObserver1 implements Observer {
       public void response() {
           System.out.println("具体观察者1作出反应！");
       }
   }
   
   //具体观察者1
   class ConcreteObserver2 implements Observer {
       public void response() {
           System.out.println("具体观察者2作出反应！");
       }
   }
   ```

   程序运行结果如下：

   ```
   具体目标发生改变...
   --------------
   具体观察者1作出反应！
   具体观察者2作出反应！
   ```


## 迭代器模式

### 模式的定义与特点

#### 迭代器（Iterator）模式的定义：提供一个对象来顺序访问聚合对象中的一系列数据，而不暴露聚合对象的内部表示。迭代器模式是一种对象行为型模式，其主要优点如下。

1. 访问一个聚合对象的内容而无须暴露它的内部表示。
2. 遍历任务交由迭代器完成，这简化了聚合类。
3. 它支持以不同方式遍历一个聚合，甚至可以自定义迭代器的子类以支持新的遍历。
4. 增加新的聚合类和迭代器类都很方便，无须修改原有代码。
5. 封装性良好，为遍历不同的聚合结构提供一个统一的接口。


#### 其主要缺点是：增加了类的个数，这在一定程度上增加了系统的复杂性。

在日常开发中，我们几乎不会自己写迭代器。除非需要定制一个自己实现的数据结构对应的迭代器，否则，开源框架提供的 API 完全够用。

### 模式的结构与实现

迭代器模式是通过将聚合对象的遍历行为分离出来，抽象成迭代器类来实现的，其目的是在不暴露聚合对象的内部结构的情况下，让外部代码透明地访问聚合的内部数据。现在我们来分析其基本结构与实现方法。

1. 模式的结构

   迭代器模式主要包含以下角色。

   1. 抽象聚合（Aggregate）角色：定义存储、添加、删除聚合对象以及创建迭代器对象的接口。
   2. 具体聚合（ConcreteAggregate）角色：实现抽象聚合类，返回一个具体迭代器的实例。
   3. 抽象迭代器（Iterator）角色：定义访问和遍历聚合元素的接口，通常包含 hasNext()、first()、next() 等方法。
   4. 具体迭代器（Concretelterator）角色：实现抽象迭代器接口中所定义的方法，完成对聚合对象的遍历，记录遍历的当前位置。


   其结构图如图 1 所示。

   

   ![迭代器模式的结构图](http://c.biancheng.net/uploads/allimg/181116/3-1Q1161PU9528.gif)

   <center>图1 迭代器模式的结构图</center>

2. 模式的实现

   迭代器模式的实现代码如下：

   ```java
   package net.biancheng.c.iterator;
   
   import java.util.*;
   
   public class IteratorPattern {
       public static void main(String[] args) {
           Aggregate ag = new ConcreteAggregate();
           ag.add("中山大学");
           ag.add("华南理工");
           ag.add("韶关学院");
           System.out.print("聚合的内容有：");
           Iterator it = ag.getIterator();
           while (it.hasNext()) {
               Object ob = it.next();
               System.out.print(ob.toString() + "\t");
           }
           Object ob = it.first();
           System.out.println("\nFirst：" + ob.toString());
       }
   }
   
   //抽象聚合
   interface Aggregate {
       public void add(Object obj);
   
       public void remove(Object obj);
   
       public Iterator getIterator();
   }
   
   //具体聚合
   class ConcreteAggregate implements Aggregate {
       private List<Object> list = new ArrayList<Object>();
   
       public void add(Object obj) {
           list.add(obj);
       }
   
       public void remove(Object obj) {
           list.remove(obj);
       }
   
       public Iterator getIterator() {
           return (new ConcreteIterator(list));
       }
   }
   
   //抽象迭代器
   interface Iterator {
       Object first();
   
       Object next();
   
       boolean hasNext();
   }
   
   //具体迭代器
   class ConcreteIterator implements Iterator {
       private List<Object> list = null;
       private int index = -1;
   
       public ConcreteIterator(List<Object> list) {
           this.list = list;
       }
   
       public boolean hasNext() {
           if (index < list.size() - 1) {
               return true;
           } else {
               return false;
           }
       }
   
       public Object first() {
           index = 0;
           Object obj = list.get(index);
           ;
           return obj;
       }
   
       public Object next() {
           Object obj = null;
           if (this.hasNext()) {
               obj = list.get(++index);
           }
           return obj;
       }
   }
   ```

   程序运行结果如下：

   ```
   聚合的内容有：中山大学    华南理工    韶关学院   
   First：中山大学
   ```


## 责任链模式

### 模式的定义与特点

责任链（Chain of Responsibility）模式的定义：为了避免请求发送者与多个请求处理者耦合在一起，于是将所有请求的处理者通过前一对象记住其下一个对象的引用而连成一条链；当有请求发生时，可将请求沿着这条链传递，直到有对象处理它为止。

注意：责任链模式也叫职责链模式。

在责任链模式中，客户只需要将请求发送到责任链上即可，无须关心请求的处理细节和请求的传递过程，请求会自动进行传递。所以责任链将请求的发送者和请求的处理者解耦了。

#### 责任链模式是一种对象行为型模式，其主要优点如下。

1. 降低了对象之间的耦合度。该模式使得一个对象无须知道到底是哪一个对象处理其请求以及链的结构，发送者和接收者也无须拥有对方的明确信息。
2. 增强了系统的可扩展性。可以根据需要增加新的请求处理类，满足开闭原则。
3. 增强了给对象指派职责的灵活性。当工作流程发生变化，可以动态地改变链内的成员或者调动它们的次序，也可动态地新增或者删除责任。
4. 责任链简化了对象之间的连接。每个对象只需保持一个指向其后继者的引用，不需保持其他所有处理者的引用，这避免了使用众多的 if 或者 if···else 语句。
5. 责任分担。每个类只需要处理自己该处理的工作，不该处理的传递给下一个对象完成，明确各类的责任范围，符合类的单一职责原则。


#### 其主要缺点如下。

1. 不能保证每个请求一定被处理。由于一个请求没有明确的接收者，所以不能保证它一定会被处理，该请求可能一直传到链的末端都得不到处理。
2. 对比较长的职责链，请求的处理可能涉及多个处理对象，系统性能将受到一定影响。
3. 职责链建立的合理性要靠客户端来保证，增加了客户端的复杂性，可能会由于职责链的错误设置而导致系统出错，如可能会造成循环调用。

### 模式的结构与实现

通常情况下，可以通过数据链表来实现职责链模式的数据结构。

1. 模式的结构

   职责链模式主要包含以下角色。

   1. 抽象处理者（Handler）角色：定义一个处理请求的接口，包含抽象处理方法和一个后继连接。
   2. 具体处理者（Concrete Handler）角色：实现抽象处理者的处理方法，判断能否处理本次请求，如果可以处理请求则处理，否则将该请求转给它的后继者。
   3. 客户类（Client）角色：创建处理链，并向链头的具体处理者对象提交请求，它不关心处理细节和请求的传递过程。


   责任链模式的本质是解耦请求与处理，让请求在处理链中能进行传递与被处理；理解责任链模式应当理解其模式，而不是其具体实现。责任链模式的独到之处是将其节点处理者组合成了链式结构，并允许节点自身决定是否进行请求处理或转发，相当于让请求流动起来。

   其结构图如图 1 所示。客户端可按图 2 所示设置责任链。

   

   ![责任链模式的结构图](http://c.biancheng.net/uploads/allimg/181116/3-1Q116135Z11C.gif)

   <center>图1 责任链模式的结构图</center>

   

   ![责任链](http://c.biancheng.net/uploads/allimg/181116/3-1Q11613592TF.gif)

   <center>图2 责任链</center>

2. 模式的实现

   职责链模式的实现代码如下：

   ```java
   package chainOfResponsibility;
   
   public class ChainOfResponsibilityPattern {
       public static void main(String[] args) {
           //组装责任链
           Handler handler1 = new ConcreteHandler1();
           Handler handler2 = new ConcreteHandler2();
           handler1.setNext(handler2);
           //提交请求
           handler1.handleRequest("two");
       }
   }
   
   //抽象处理者角色
   abstract class Handler {
       private Handler next;
   
       public void setNext(Handler next) {
           this.next = next;
       }
   
       public Handler getNext() {
           return next;
       }
   
       //处理请求的方法
       public abstract void handleRequest(String request);
   }
   
   //具体处理者角色1
   class ConcreteHandler1 extends Handler {
       public void handleRequest(String request) {
           if (request.equals("one")) {
               System.out.println("具体处理者1负责处理该请求！");
           } else {
               if (getNext() != null) {
                   getNext().handleRequest(request);
               } else {
                   System.out.println("没有人处理该请求！");
               }
           }
       }
   }
   
   //具体处理者角色2
   class ConcreteHandler2 extends Handler {
       public void handleRequest(String request) {
           if (request.equals("two")) {
               System.out.println("具体处理者2负责处理该请求！");
           } else {
               if (getNext() != null) {
                   getNext().handleRequest(request);
               } else {
                   System.out.println("没有人处理该请求！");
               }
           }
       }
   }
   ```

   程序运行结果如下：

   ```
   具体处理者2负责处理该请求！
   ```

   在上面代码中，我们把消息硬编码为 String 类型，而在真实业务中，消息是具备多样性的，可以是 int、String 或者自定义类型。因此，在上面代码的基础上，可以对消息类型进行抽象 Request，增强了消息的兼容性。

## 命令模式

### 命令模式的定义与特点

命令（Command）模式的定义如下：将一个请求封装为一个对象，使发出请求的责任和执行请求的责任分割开。这样两者之间通过命令对象进行沟通，这样方便将命令对象进行储存、传递、调用、增加与管理。

#### 命令模式的主要优点如下。

1. 通过引入中间件（抽象接口）降低系统的耦合度。
2. 扩展性良好，增加或删除命令非常方便。采用命令模式增加与删除命令不会影响其他类，且满足“开闭原则”。
3. 可以实现宏命令。命令模式可以与[组合模式](http://c.biancheng.net/view/1373.html)结合，将多个命令装配成一个组合命令，即宏命令。
4. 方便实现 Undo 和 Redo 操作。命令模式可以与后面介绍的[备忘录模式](http://c.biancheng.net/view/1400.html)结合，实现命令的撤销与恢复。
5. 可以在现有命令的基础上，增加额外功能。比如日志记录，结合装饰器模式会更加灵活。


#### 其缺点是：

1. 可能产生大量具体的命令类。因为每一个具体操作都需要设计一个具体命令类，这会增加系统的复杂性。
2. 命令模式的结果其实就是接收方的执行结果，但是为了以命令的形式进行架构、解耦请求与实现，引入了额外类型结构（引入了请求方与抽象命令接口），增加了理解上的困难。不过这也是[设计模式](http://c.biancheng.net/design_pattern/)的通病，抽象必然会额外增加类的数量，代码抽离肯定比代码聚合更加难理解。

### 命令模式的结构与实现

可以将系统中的相关操作抽象成命令，使调用者与实现者相关分离，其结构如下。

1. 模式的结构

   命令模式包含以下主要角色。

   1. 抽象命令类（Command）角色：声明执行命令的接口，拥有执行命令的抽象方法 execute()。
   2. 具体命令类（Concrete Command）角色：是抽象命令类的具体实现类，它拥有接收者对象，并通过调用接收者的功能来完成命令要执行的操作。
   3. 实现者/接收者（Receiver）角色：执行命令功能的相关操作，是具体命令对象业务的真正实现者。
   4. 调用者/请求者（Invoker）角色：是请求的发送者，它通常拥有很多的命令对象，并通过访问命令对象来执行相关请求，它不直接访问接收者。


   其结构图如图 1 所示。

   

   ![命令模式的结构图](http://c.biancheng.net/uploads/allimg/181116/3-1Q11611335E44.gif)

   <center>图1 命令模式的结构图</center>

2. 模式的实现

   命令模式的代码如下：

   ```java
   package command;
   
   public class CommandPattern {
       public static void main(String[] args) {
           Command cmd = new ConcreteCommand();
           Invoker ir = new Invoker(cmd);
           System.out.println("客户访问调用者的call()方法...");
           ir.call();
       }
   }
   
   //调用者
   class Invoker {
       private Command command;
   
       public Invoker(Command command) {
           this.command = command;
       }
   
       public void setCommand(Command command) {
           this.command = command;
       }
   
       public void call() {
           System.out.println("调用者执行命令command...");
           command.execute();
       }
   }
   
   //抽象命令
   interface Command {
       public abstract void execute();
   }
   
   //具体命令
   class ConcreteCommand implements Command {
       private Receiver receiver;
   
       ConcreteCommand() {
           receiver = new Receiver();
       }
   
       public void execute() {
           receiver.action();
       }
   }
   
   //接收者
   class Receiver {
       public void action() {
           System.out.println("接收者的action()方法被调用...");
       }
   }
   ```

   程序的运行结果如下：

   ```
   客户访问调用者的call()方法...
   调用者执行命令command...
   接收者的action()方法被调用...
   ```


## 备忘录模式

### 模式的定义与特点

备忘录（Memento）模式的定义：在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，以便以后当需要时能将该对象恢复到原先保存的状态。该模式又叫快照模式。

#### 备忘录模式是一种对象行为型模式，其主要优点如下。

- 提供了一种可以恢复状态的机制。当用户需要时能够比较方便地将数据恢复到某个历史的状态。
- 实现了内部状态的封装。除了创建它的发起人之外，其他对象都不能够访问这些状态信息。
- 简化了发起人类。发起人不需要管理和保存其内部状态的各个备份，所有状态信息都保存在备忘录中，并由管理者进行管理，这符合单一职责原则。


#### 其主要缺点是：资源消耗大。如果要保存的内部状态信息过多或者特别频繁，将会占用比较大的内存资源。

### 模式的结构与实现

备忘录模式的核心是设计备忘录类以及用于管理备忘录的管理者类，现在我们来学习其结构与实现。

1. 模式的结构

   备忘录模式的主要角色如下。

   1. 发起人（Originator）角色：记录当前时刻的内部状态信息，提供创建备忘录和恢复备忘录数据的功能，实现其他业务功能，它可以访问备忘录里的所有信息。
   2. 备忘录（Memento）角色：负责存储发起人的内部状态，在需要的时候提供这些内部状态给发起人。
   3. 管理者（Caretaker）角色：对备忘录进行管理，提供保存与获取备忘录的功能，但其不能对备忘录的内容进行访问与修改。


   备忘录模式的结构图如图 1 所示。

   

   ![备忘录模式的结构图](http://c.biancheng.net/uploads/allimg/181119/3-1Q119130413927.gif)

   <center>图1 备忘录模式的结构图</center>

2. 模式的实现

   备忘录模式的实现代码如下：

   ```java
   package net.biancheng.c.memento;
   
   public class MementoPattern {
       public static void main(String[] args) {
           Originator or = new Originator();
           Caretaker cr = new Caretaker();
           or.setState("S0");
           System.out.println("初始状态:" + or.getState());
           cr.setMemento(or.createMemento()); //保存状态
           or.setState("S1");
           System.out.println("新的状态:" + or.getState());
           or.restoreMemento(cr.getMemento()); //恢复状态
           System.out.println("恢复状态:" + or.getState());
       }
   }
   
   //备忘录
   class Memento {
       private String state;
   
       public Memento(String state) {
           this.state = state;
       }
   
       public void setState(String state) {
           this.state = state;
       }
   
       public String getState() {
           return state;
       }
   }
   
   //发起人
   class Originator {
       private String state;
   
       public void setState(String state) {
           this.state = state;
       }
   
       public String getState() {
           return state;
       }
   
       public Memento createMemento() {
           return new Memento(state);
       }
   
       public void restoreMemento(Memento m) {
           this.setState(m.getState());
       }
   }
   
   //管理者
   class Caretaker {
       private Memento memento;
   
       public void setMemento(Memento m) {
           memento = m;
       }
   
       public Memento getMemento() {
           return memento;
       }
   }
   ```

   程序运行的结果如下：

   ```
   初始状态:S0
   新的状态:S1
   恢复状态:S0
   ```


## 状态模式

### 状态模式的定义与特点

状态（State）模式的定义：对有状态的对象，把复杂的“判断逻辑”提取到不同的状态对象中，允许状态对象在其内部状态发生改变时改变其行为。

#### 状态模式是一种对象行为型模式，其主要优点如下。

1. 结构清晰，状态模式将与特定状态相关的行为局部化到一个状态中，并且将不同状态的行为分割开来，满足“单一职责原则”。
2. 将状态转换显示化，减少对象间的相互依赖。将不同的状态引入独立的对象中会使得状态转换变得更加明确，且减少对象间的相互依赖。
3. 状态类职责明确，有利于程序的扩展。通过定义新的子类很容易地增加新的状态和转换。


#### 状态模式的主要缺点如下。

1. 状态模式的使用必然会增加系统的类与对象的个数。
2. 状态模式的结构与实现都较为复杂，如果使用不当会导致程序结构和代码的混乱。
3. 状态模式对开闭原则的支持并不太好，对于可以切换状态的状态模式，增加新的状态类需要修改那些负责状态转换的源码，否则无法切换到新增状态，而且修改某个状态类的行为也需要修改对应类的源码。

### 状态模式的结构与实现

状态模式把受环境改变的对象行为包装在不同的状态对象里，其意图是让一个对象在其内部状态改变的时候，其行为也随之改变。现在我们来分析其基本结构和实现方法。

1. 模式的结构

   状态模式包含以下主要角色。

   1. 环境类（Context）角色：也称为上下文，它定义了客户端需要的接口，内部维护一个当前状态，并负责具体状态的切换。
   2. 抽象状态（State）角色：定义一个接口，用以封装环境对象中的特定状态所对应的行为，可以有一个或多个行为。
   3. 具体状态（Concrete State）角色：实现抽象状态所对应的行为，并且在需要的情况下进行状态切换。


   其结构图如图 1 所示。

   ![状态模式的结构图](http://c.biancheng.net/uploads/allimg/181116/3-1Q11615412U55.gif)

   <center>图1 状态模式的结构图</center>

2. 模式的实现

   状态模式的实现代码如下：

   ```java
   public class StatePatternClient {
       public static void main(String[] args) {
           Context context = new Context();    //创建环境      
           context.Handle();    //处理请求
           context.Handle();
           context.Handle();
           context.Handle();
       }
   }
   
   //环境类
   class Context {
       private State state;
   
       //定义环境类的初始状态
       public Context() {
           this.state = new ConcreteStateA();
       }
   
       //设置新状态
       public void setState(State state) {
           this.state = state;
       }
   
       //读取状态
       public State getState() {
           return (state);
       }
   
       //对请求做处理
       public void Handle() {
           state.Handle(this);
       }
   }
   
   //抽象状态类
   abstract class State {
       public abstract void Handle(Context context);
   }
   
   //具体状态A类
   class ConcreteStateA extends State {
       public void Handle(Context context) {
           System.out.println("当前状态是 A.");
           context.setState(new ConcreteStateB());
       }
   }
   
   //具体状态B类
   class ConcreteStateB extends State {
       public void Handle(Context context) {
           System.out.println("当前状态是 B.");
           context.setState(new ConcreteStateA());
       }
   }
   ```


   程序运行结果如下：

   ```
   当前状态是 A.
   当前状态是 B.
   当前状态是 A.
   当前状态是 B.
   ```


## 访问者模式

### 模式的定义与特点

访问者（Visitor）模式的定义：将作用于某种数据结构中的各元素的操作分离出来封装成独立的类，使其在不改变数据结构的前提下可以添加作用于这些元素的新的操作，为数据结构中的每个元素提供多种访问方式。它将对数据的操作与数据结构进行分离，是行为类模式中最复杂的一种模式。

#### 访问者（Visitor）模式是一种对象行为型模式，其主要优点如下。

1. 扩展性好。能够在不修改对象结构中的元素的情况下，为对象结构中的元素添加新的功能。
2. 复用性好。可以通过访问者来定义整个对象结构通用的功能，从而提高系统的复用程度。
3. 灵活性好。访问者模式将数据结构与作用于结构上的操作解耦，使得操作集合可相对自由地演化而不影响系统的数据结构。
4. 符合单一职责原则。访问者模式把相关的行为封装在一起，构成一个访问者，使每一个访问者的功能都比较单一。


#### 访问者（Visitor）模式的主要缺点如下。

1. 增加新的元素类很困难。在访问者模式中，每增加一个新的元素类，都要在每一个具体访问者类中增加相应的具体操作，这违背了“开闭原则”。
2. 破坏封装。访问者模式中具体元素对访问者公布细节，这破坏了对象的封装性。
3. 违反了依赖倒置原则。访问者模式依赖了具体类，而没有依赖抽象类。

### 模式的结构与实现

访问者（Visitor）模式实现的关键是如何将作用于元素的操作分离出来封装成独立的类，其基本结构与实现方法如下。

1. 模式的结构

   访问者模式包含以下主要角色。

   1. 抽象访问者（Visitor）角色：定义一个访问具体元素的接口，为每个具体元素类对应一个访问操作 visit() ，该操作中的参数类型标识了被访问的具体元素。
   2. 具体访问者（ConcreteVisitor）角色：实现抽象访问者角色中声明的各个访问操作，确定访问者访问一个元素时该做什么。
   3. 抽象元素（Element）角色：声明一个包含接受操作 accept() 的接口，被接受的访问者对象作为 accept() 方法的参数。
   4. 具体元素（ConcreteElement）角色：实现抽象元素角色提供的 accept() 操作，其方法体通常都是 visitor.visit(this) ，另外具体元素中可能还包含本身业务逻辑的相关操作。
   5. 对象结构（Object Structure）角色：是一个包含元素角色的容器，提供让访问者对象遍历容器中的所有元素的方法，通常由 List、Set、Map 等聚合类实现。


   其结构图如图 1 所示。

   

   [![访问者（Visitor）模式的结构图](http://c.biancheng.net/uploads/allimg/181119/3-1Q11910135Y25.gif)](http://c.biancheng.net/uploads/allimg/181119/3-1Q119101429D6.gif)

   <center>图1 访问者（Visitor）模式的结构图</center>

2. 模式的实现

   访问者模式的实现代码如下：

   ```java
   package net.biancheng.c.visitor;
   
   import java.util.*;
   
   public class VisitorPattern {
       public static void main(String[] args) {
           ObjectStructure os = new ObjectStructure();
           os.add(new ConcreteElementA());
           os.add(new ConcreteElementB());
           Visitor visitor = new ConcreteVisitorA();
           os.accept(visitor);
           System.out.println("------------------------");
           visitor = new ConcreteVisitorB();
           os.accept(visitor);
       }
   }
   
   //抽象访问者
   interface Visitor {
       void visit(ConcreteElementA element);
   
       void visit(ConcreteElementB element);
   }
   
   //具体访问者A类
   class ConcreteVisitorA implements Visitor {
       public void visit(ConcreteElementA element) {
           System.out.println("具体访问者A访问-->" + element.operationA());
       }
   
       public void visit(ConcreteElementB element) {
           System.out.println("具体访问者A访问-->" + element.operationB());
       }
   }
   
   //具体访问者B类
   class ConcreteVisitorB implements Visitor {
       public void visit(ConcreteElementA element) {
           System.out.println("具体访问者B访问-->" + element.operationA());
       }
   
       public void visit(ConcreteElementB element) {
           System.out.println("具体访问者B访问-->" + element.operationB());
       }
   }
   
   //抽象元素类
   interface Element {
       void accept(Visitor visitor);
   }
   
   //具体元素A类
   class ConcreteElementA implements Element {
       public void accept(Visitor visitor) {
           visitor.visit(this);
       }
   
       public String operationA() {
           return "具体元素A的操作。";
       }
   }
   
   //具体元素B类
   class ConcreteElementB implements Element {
       public void accept(Visitor visitor) {
           visitor.visit(this);
       }
   
       public String operationB() {
           return "具体元素B的操作。";
       }
   }
   
   //对象结构角色
   class ObjectStructure {
       private List<Element> list = new ArrayList<Element>();
   
       public void accept(Visitor visitor) {
           Iterator<Element> i = list.iterator();
           while (i.hasNext()) {
               ((Element) i.next()).accept(visitor);
           }
       }
   
       public void add(Element element) {
           list.add(element);
       }
   
       public void remove(Element element) {
           list.remove(element);
       }
   }
   ```

   程序的运行结果如下：

   ```
   具体访问者A访问-->具体元素A的操作。
   具体访问者A访问-->具体元素B的操作。
   ------------------------
   具体访问者B访问-->具体元素A的操作。
   具体访问者B访问-->具体元素B的操作。
   ```


## 中介者模式

### 模式的定义与特点

中介者（Mediator）模式的定义：定义一个中介对象来封装一系列对象之间的交互，使原有对象之间的耦合松散，且可以独立地改变它们之间的交互。中介者模式又叫调停模式，它是迪米特法则的典型应用。

#### 中介者模式是一种对象行为型模式，其主要优点如下。

1. 类之间各司其职，符合迪米特法则。
2. 降低了对象之间的耦合性，使得对象易于独立地被复用。
3. 将对象间的一对多关联转变为一对一的关联，提高系统的灵活性，使得系统易于维护和扩展。


#### 其主要缺点是：中介者模式将原本多个对象直接的相互依赖变成了中介者和多个同事类的依赖关系。当同事类越多时，中介者就会越臃肿，变得复杂且难以维护。

### 模式的结构与实现

中介者模式实现的关键是找出“中介者”，下面对它的结构和实现进行分析。

1. 模式的结构

   中介者模式包含以下主要角色。

   1. 抽象中介者（Mediator）角色：它是中介者的接口，提供了同事对象注册与转发同事对象信息的抽象方法。
   2. 具体中介者（Concrete Mediator）角色：实现中介者接口，定义一个 List 来管理同事对象，协调各个同事角色之间的交互关系，因此它依赖于同事角色。
   3. 抽象同事类（Colleague）角色：定义同事类的接口，保存中介者对象，提供同事对象交互的抽象方法，实现所有相互影响的同事类的公共功能。
   4. 具体同事类（Concrete Colleague）角色：是抽象同事类的实现者，当需要与其他同事对象交互时，由中介者对象负责后续的交互。


   中介者模式的结构图如图 1 所示。

   

   ![中介者模式的结构图](http://c.biancheng.net/uploads/allimg/181116/3-1Q1161I532V0.gif)

   <center>图1 中介者模式的结构图</center>

2. 模式的实现

   中介者模式的实现代码如下：

   ```java
   package net.biancheng.c.mediator;
   
   import java.util.*;
   
   public class MediatorPattern {
       public static void main(String[] args) {
           Mediator md = new ConcreteMediator();
           Colleague c1, c2;
           c1 = new ConcreteColleague1();
           c2 = new ConcreteColleague2();
           md.register(c1);
           md.register(c2);
           c1.send();
           System.out.println("-------------");
           c2.send();
       }
   }
   
   //抽象中介者
   abstract class Mediator {
       public abstract void register(Colleague colleague);
   
       public abstract void relay(Colleague cl); //转发
   }
   
   //具体中介者
   class ConcreteMediator extends Mediator {
       private List<Colleague> colleagues = new ArrayList<Colleague>();
   
       public void register(Colleague colleague) {
           if (!colleagues.contains(colleague)) {
               colleagues.add(colleague);
               colleague.setMedium(this);
           }
       }
   
       public void relay(Colleague cl) {
           for (Colleague ob : colleagues) {
               if (!ob.equals(cl)) {
                   ((Colleague) ob).receive();
               }
           }
       }
   }
   
   //抽象同事类
   abstract class Colleague {
       protected Mediator mediator;
   
       public void setMedium(Mediator mediator) {
           this.mediator = mediator;
       }
   
       public abstract void receive();
   
       public abstract void send();
   }
   
   //具体同事类
   class ConcreteColleague1 extends Colleague {
       public void receive() {
           System.out.println("具体同事类1收到请求。");
       }
   
       public void send() {
           System.out.println("具体同事类1发出请求。");
           mediator.relay(this); //请中介者转发
       }
   }
   
   //具体同事类
   class ConcreteColleague2 extends Colleague {
       public void receive() {
           System.out.println("具体同事类2收到请求。");
       }
   
       public void send() {
           System.out.println("具体同事类2发出请求。");
           mediator.relay(this); //请中介者转发
       }
   }
   ```

   程序的运行结果如下：

   ```
   具体同事类1发出请求。
   具体同事类2收到请求。
   -------------
   具体同事类2发出请求。
   具体同事类1收到请求。
   ```


## 解释器模式

### 模式的定义与特点

解释器（Interpreter）模式的定义：给分析对象定义一个语言，并定义该语言的文法表示，再设计一个解析器来解释语言中的句子。也就是说，用编译语言的方式来分析应用中的实例。这种模式实现了文法表达式处理的接口，该接口解释一个特定的上下文。

这里提到的文法和句子的概念同编译原理中的描述相同，“文法”指语言的语法规则，而“句子”是语言集中的元素。例如，汉语中的句子有很多，“我是中国人”是其中的一个句子，可以用一棵语法树来直观地描述语言中的句子。

#### 解释器模式是一种类行为型模式，其主要优点如下。

1. 扩展性好。由于在解释器模式中使用类来表示语言的文法规则，因此可以通过继承等机制来改变或扩展文法。
2. 容易实现。在语法树中的每个表达式节点类都是相似的，所以实现其文法较为容易。


#### 解释器模式的主要缺点如下。

1. 执行效率较低。解释器模式中通常使用大量的循环和递归调用，当要解释的句子较复杂时，其运行速度很慢，且代码的调试过程也比较麻烦。
2. 会引起类膨胀。解释器模式中的每条规则至少需要定义一个类，当包含的文法规则很多时，类的个数将急剧增加，导致系统难以管理与维护。
3. 可应用的场景比较少。在软件开发中，需要定义语言文法的应用实例非常少，所以这种模式很少被使用到。

### 模式的结构与实现

解释器模式常用于对简单语言的编译或分析实例中，为了掌握好它的结构与实现，必须先了解编译原理中的“文法、句子、语法树”等相关概念。

#### 1) 文法

文法是用于描述语言的语法结构的形式规则。没有规矩不成方圆，例如，有些人认为完美爱情的准则是“相互吸引、感情专一、任何一方都没有恋爱经历”，虽然最后一条准则较苛刻，但任何事情都要有规则，语言也一样，不管它是机器语言还是自然语言，都有它自己的文法规则。例如，中文中的“句子”的文法如下。

```
〈句子〉::=〈主语〉〈谓语〉〈宾语〉
〈主语〉::=〈代词〉|〈名词〉
〈谓语〉::=〈动词〉
〈宾语〉::=〈代词〉|〈名词〉
〈代词〉你|我|他
〈名词〉7大学生I筱霞I英语
〈动词〉::=是|学习
```


注：这里的符号“::=”表示“定义为”的意思，用“〈”和“〉”括住的是非终结符，没有括住的是终结符。

#### 2) 句子

句子是语言的基本单位，是语言集中的一个元素，它由终结符构成，能由“文法”推导出。例如，上述文法可以推出“我是大学生”，所以它是句子。

#### 3) 语法树

语法树是句子结构的一种树型表示，它代表了句子的推导结果，它有利于理解句子语法结构的层次。图 1 所示是“我是大学生”的语法树。



![句子“我是大学生”的语法树](http://c.biancheng.net/uploads/allimg/181119/3-1Q119150550114.gif)

<center>图1 句子“我是大学生”的语法树</center>


有了以上基础知识，现在来介绍解释器模式的结构就简单了。解释器模式的结构与[组合模式](http://c.biancheng.net/view/1373.html)相似，不过其包含的组成元素比组合模式多，而且组合模式是对象结构型模式，而解释器模式是类行为型模式。

1. 模式的结构

   解释器模式包含以下主要角色。

   1. 抽象表达式（Abstract Expression）角色：定义解释器的接口，约定解释器的解释操作，主要包含解释方法 interpret()。
   2. 终结符表达式（Terminal Expression）角色：是抽象表达式的子类，用来实现文法中与终结符相关的操作，文法中的每一个终结符都有一个具体终结表达式与之相对应。
   3. 非终结符表达式（Nonterminal Expression）角色：也是抽象表达式的子类，用来实现文法中与非终结符相关的操作，文法中的每条规则都对应于一个非终结符表达式。
   4. 环境（Context）角色：通常包含各个解释器需要的数据或是公共的功能，一般用来传递被所有解释器共享的数据，后面的解释器可以从这里获取这些值。
   5. 客户端（Client）：主要任务是将需要分析的句子或表达式转换成使用解释器对象描述的抽象语法树，然后调用解释器的解释方法，当然也可以通过环境角色间接访问解释器的解释方法。


   解释器模式的结构图如图 2 所示。

   

   ![解释器模式的结构图](http://c.biancheng.net/uploads/allimg/181119/3-1Q119150626422.gif)

   <center>图2 解释器模式的结构图</center>

2. 模式的实现

   解释器模式实现的关键是定义文法规则、设计终结符类与非终结符类、画出结构图，必要时构建语法树，其代码结构如下：

   ```java
   package net.biancheng.c.interpreter;
   
   //抽象表达式类
   interface AbstractExpression {
       public void interpret(String info);    //解释方法
   }
   
   //终结符表达式类
   class TerminalExpression implements AbstractExpression {
       public void interpret(String info) {
           //对终结符表达式的处理
       }
   }
   
   //非终结符表达式类
   class NonterminalExpression implements AbstractExpression {
       private AbstractExpression exp1;
       private AbstractExpression exp2;
   
       public void interpret(String info) {
           //非对终结符表达式的处理
       }
   }
   
   //环境类
   class Context {
       private AbstractExpression exp;
   
       public Context() {
           //数据初始化
       }
   
       public void operation(String info) {
           //调用相关表达式类的解释方法
       }
   }
   ```

   
