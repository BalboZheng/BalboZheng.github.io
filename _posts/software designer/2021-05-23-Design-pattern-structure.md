---
layout: post
title: 设计模式-结构模式
subtitle: 
author: Balbo Cheng
categories: software_designer
tags: [pandas, numpy]
---

## 适配器模式

### 模式的定义与特点

适配器模式（Adapter）的定义如下：将一个类的接口转换成客户希望的另外一个接口，使得原本由于接口不兼容而不能一起工作的那些类能一起工作。适配器模式分为类结构型模式和对象结构型模式两种，前者类之间的耦合度比后者高，且要求程序员了解现有组件库中的相关组件的内部结构，所以应用相对较少些。

该模式的**主要优点**如下。

- 客户端通过适配器可以透明地调用目标接口。
- 复用了现存的类，程序员不需要修改原有代码而重用现有的适配者类。
- 将目标类和适配者类解耦，解决了目标类和适配者类接口不一致的问题。
- 在很多业务场景中符合开闭原则。

**其缺点是：**

- 适配器编写过程需要结合业务场景全面考虑，可能会增加系统的复杂性。
- 增加代码阅读难度，降低代码可读性，过多使用适配器会使系统代码变得凌乱。

### 模式的结构与实现

类适配器模式可采用多重继承方式实现，如 [C++](http://c.biancheng.net/cplus/) 可定义一个适配器类来同时继承当前系统的业务接口和现有组件库中已经存在的组件接口；[Java](http://c.biancheng.net/java/) 不支持多继承，但可以定义一个适配器类来实现当前系统的业务接口，同时又继承现有组件库中已经存在的组件。

对象适配器模式可釆用将现有组件库中已经实现的组件引入适配器类中，该类同时实现当前系统的业务接口。现在来介绍它们的基本结构。

1. 模式的结构

   适配器模式（Adapter）包含以下主要角色。

   1. 目标（Target）接口：当前系统业务所期待的接口，它可以是抽象类或接口。
   2. 适配者（Adaptee）类：它是被访问和适配的现存组件库中的组件接口。
   3. 适配器（Adapter）类：它是一个转换器，通过继承或引用适配者的对象，把适配者接口转换成目标接口，让客户按目标接口的格式访问适配者。

   ![Adapter1.jpg](https://i.loli.net/2021/05/27/VuOcMb2yJzSNYgt.png)

   <center>图1 类适配器模式的结构图</center>

   ![Adapter2.jpg](https://i.loli.net/2021/05/27/Fhq7WCAXoz4PZdc.png)

   <center>图2 对象适配器模式的结构图</center>

2. 模式的实现

   (1) 类适配器模式的代码如下。

   ```java
   package adapter;
   //目标接口
   interface Target
   {
       public void request();
   }
   //适配者接口
   class Adaptee
   {
       public void specificRequest()
       {       
           System.out.println("适配者中的业务代码被调用！");
       }
   }
   //类适配器类
   class ClassAdapter extends Adaptee implements Target
   {
       public void request()
       {
           specificRequest();
       }
   }
   //客户端代码
   public class ClassAdapterTest
   {
       public static void main(String[] args)
       {
           System.out.println("类适配器模式测试：");
           Target target = new ClassAdapter();
           target.request();
       }
   }
   ```


   程序的运行结果如下：

   ```tex
   类适配器模式测试：
   适配者中的业务代码被调用！
   ```


   (2)对象适配器模式的代码如下。

   ```java
   package adapter;
   //对象适配器类
   class ObjectAdapter implements Target
   {
       private Adaptee adaptee;
       public ObjectAdapter(Adaptee adaptee)
       {
           this.adaptee=adaptee;
       }
       public void request()
       {
           adaptee.specificRequest();
       }
   }
   //客户端代码
   public class ObjectAdapterTest
   {
       public static void main(String[] args)
       {
           System.out.println("对象适配器模式测试：");
           Adaptee adaptee = new Adaptee();
           Target target = new ObjectAdapter(adaptee);
           target.request();
       }
   }
   ```


   说明：对象适配器模式中的“目标接口”和“适配者类”的代码同类适配器模式一样，只要修改适配器类和客户端的代码即可。

   程序的运行结果如下：

   ```tex
   对象适配器模式测试：
   适配者中的业务代码被调用！
   ```


## 装饰器模式

### 装饰器模式的定义与特点

装饰器（Decorator）模式的定义：指在不改变现有对象结构的情况下，动态地给该对象增加一些职责（即增加其额外功能）的模式，它属于对象结构型模式。

装饰器模式的**主要优点**有：

- 装饰器是继承的有力补充，比继承灵活，在不改变原有对象的情况下，动态的给一个对象扩展功能，即插即用
- 通过使用不用装饰类及这些装饰类的排列组合，可以实现不同效果
- 装饰器模式完全遵守开闭原则

其**主要缺点**是：装饰器模式会增加许多子类，过度使用会增加程序得复杂性。

### 装饰器模式的结构与实现

通常情况下，扩展一个类的功能会使用继承方式来实现。但继承具有静态特征，耦合度高，并且随着扩展功能的增多，子类会很膨胀。如果使用组合关系来创建一个包装对象（即装饰对象）来包裹真实对象，并在保持真实对象的类结构不变的前提下，为其提供额外的功能，这就是装饰器模式的目标。下面来分析其基本结构和实现方法。

1. 模式的结构

   装饰器模式主要包含以下角色。

   1. 抽象构件（Component）角色：定义一个抽象接口以规范准备接收附加责任的对象。
   2. 具体构件（ConcreteComponent）角色：实现抽象构件，通过装饰角色为其添加一些职责。
   3. 抽象装饰（Decorator）角色：继承抽象构件，并包含具体构件的实例，可以通过其子类扩展具体构件的功能。
   4. 具体装饰（ConcreteDecorator）角色：实现抽象装饰的相关方法，并给具体构件对象添加附加的责任。


   装饰器模式的结构图如图 1 所示。

   

   ![Decorator.jpg](https://i.loli.net/2021/05/27/nKMl5ijm9JDNZyt.png)

   <center>图1 装饰器模式的结构图</center>

2. 模式的实现

   装饰器模式的实现代码如下：

   ```java
   package decorator;
   
   public class DecoratorPattern {
       public static void main(String[] args) {
           Component p = new ConcreteComponent();
           p.operation();
           System.out.println("---------------------------------");
           Component d = new ConcreteDecorator(p);
           d.operation();
       }
   }
   
   //抽象构件角色
   interface Component {
       public void operation();
   }
   
   //具体构件角色
   class ConcreteComponent implements Component {
       public ConcreteComponent() {
           System.out.println("创建具体构件角色");
       }
   
       public void operation() {
           System.out.println("调用具体构件角色的方法operation()");
       }
   }
   
   //抽象装饰角色
   class Decorator implements Component {
       private Component component;
   
       public Decorator(Component component) {
           this.component = component;
       }
   
       public void operation() {
           component.operation();
       }
   }
   
   //具体装饰角色
   class ConcreteDecorator extends Decorator {
       public ConcreteDecorator(Component component) {
           super(component);
       }
   
       public void operation() {
           super.operation();
           addedFunction();
       }
   
       public void addedFunction() {
           System.out.println("为具体构件角色增加额外的功能addedFunction()");
       }
   }
   ```


   程序运行结果如下：

   ```tex
   创建具体构件角色
   调用具体构件角色的方法operation()
   ---------------------------------
   调用具体构件角色的方法operation()
   为具体构件角色增加额外的功能addedFunction()
   ```


## 代理模式

### 代理模式的定义与特点

代理模式的定义：由于某些原因需要给某对象提供一个代理以控制对该对象的访问。这时，访问对象不适合或者不能直接引用目标对象，代理对象作为访问对象和目标对象之间的中介。

代理模式的**主要优点**有：

- 代理模式在客户端与目标对象之间起到一个中介作用和保护目标对象的作用；
- 代理对象可以扩展目标对象的功能；
- 代理模式能将客户端与目标对象分离，在一定程度上降低了系统的耦合度，增加了程序的可扩展性

**其主要缺点是：**

- 代理模式会造成系统设计中类的数量增加
- 在客户端和目标对象之间增加一个代理对象，会造成请求处理速度变慢；
- 增加了系统的复杂度；

> 那么如何解决以上提到的缺点呢？答案是可以使用动态代理方式

### 代理模式的结构与实现

代理模式的结构比较简单，主要是通过定义一个继承抽象主题的代理来包含真实主题，从而实现对真实主题的访问，下面来分析其基本结构和实现方法。

1. 模式的结构

   代理模式的主要角色如下。

   1. 抽象主题（Subject）类：通过接口或抽象类声明真实主题和代理对象实现的业务方法。
   2. 真实主题（Real Subject）类：实现了抽象主题中的具体业务，是代理对象所代表的真实对象，是最终要引用的对象。
   3. 代理（Proxy）类：提供了与真实主题相同的接口，其内部含有对真实主题的引用，它可以访问、控制或扩展真实主题的功能。

   

   ![Proxy.jpg](https://i.loli.net/2021/05/27/2oqtvfkbUM6x8zR.png)

   <center>图1 代理模式的结构图</center>


   在代码中，一般代理会被理解为代码增强，实际上就是在原代码逻辑前后增加一些代码逻辑，而使调用者无感知。

   根据代理的创建时期，代理模式分为静态代理和动态代理。

   - 静态：由程序员创建代理类或特定工具自动生成源代码再对其编译，在程序运行前代理类的 .class 文件就已经存在了。
   - 动态：在程序运行时，运用反射机制动态创建而成


2. 模式的实现

   代理模式的实现代码如下：

   ```java
   package proxy;
   
   public class ProxyTest {
       public static void main(String[] args) {
           Proxy proxy = new Proxy();
           proxy.Request();
       }
   }
   
   //抽象主题
   interface Subject {
       void Request();
   }
   
   //真实主题
   class RealSubject implements Subject {
       public void Request() {
           System.out.println("访问真实主题方法...");
       }
   }
   
   //代理
   class Proxy implements Subject {
       private RealSubject realSubject;
   
       public void Request() {
           if (realSubject == null) {
               realSubject = new RealSubject();
           }
           preRequest();
           realSubject.Request();
           postRequest();
       }
   
       public void preRequest() {
           System.out.println("访问真实主题之前的预处理。");
       }
   
       public void postRequest() {
           System.out.println("访问真实主题之后的后续处理。");
       }
   }
   ```

   程序运行的结果如下：

   ```tex
   访问真实主题之前的预处理。
   访问真实主题方法...
   访问真实主题之后的后续处理。
   ```


## 桥接模式

### 桥接模式的定义与特点

桥接（Bridge）模式的定义如下：将抽象与实现分离，使它们可以独立变化。它是用组合关系代替继承关系来实现，从而降低了抽象和实现这两个可变维度的耦合度。

通过上面的讲解，我们能很好的感觉到桥接模式遵循了里氏替换原则和依赖倒置原则，最终实现了开闭原则，对修改关闭，对扩展开放。这里将桥接模式的优缺点总结如下。

**桥接（Bridge）模式的优点是：**

- 抽象与实现分离，扩展能力强
- 符合开闭原则
- 符合合成复用原则
- 其实现细节对客户透明

**缺点是：**由于聚合关系建立在抽象层，要求开发者针对抽象化进行设计与编程，能正确地识别出系统中两个独立变化的维度，这增加了系统的理解与设计难度。

### 桥接模式的结构与实现

可以将抽象化部分与实现化部分分开，取消二者的继承关系，改用组合关系。

1. 模式的结构

   桥接（Bridge）模式包含以下主要角色。

   1. 抽象化（Abstraction）角色：定义抽象类，并包含一个对实现化对象的引用。
   2. 扩展抽象化（Refined Abstraction）角色：是抽象化角色的子类，实现父类中的业务方法，并通过组合关系调用实现化角色中的业务方法。
   3. 实现化（Implementor）角色：定义实现化角色的接口，供扩展抽象化角色调用。
   4. 具体实现化（Concrete Implementor）角色：给出实现化角色接口的具体实现。

   

   ![Bridge.jpg](https://i.loli.net/2021/05/27/KVgB2xXrj6qMWSk.png)

   <center>图1 桥接模式的结构图</center>

2. 模式的实现

   桥接模式的代码如下：

   ```java
   package bridge;
   
   public class BridgeTest {
       public static void main(String[] args) {
           Implementor imple = new ConcreteImplementorA();
           Abstraction abs = new RefinedAbstraction(imple);
           abs.Operation();
       }
   }
   
   //实现化角色
   interface Implementor {
       public void OperationImpl();
   }
   
   //具体实现化角色
   class ConcreteImplementorA implements Implementor {
       public void OperationImpl() {
           System.out.println("具体实现化(Concrete Implementor)角色被访问");
       }
   }
   
   //抽象化角色
   abstract class Abstraction {
       protected Implementor imple;
   
       protected Abstraction(Implementor imple) {
           this.imple = imple;
       }
   
       public abstract void Operation();
   }
   
   //扩展抽象化角色
   class RefinedAbstraction extends Abstraction {
       protected RefinedAbstraction(Implementor imple) {
           super(imple);
       }
   
       public void Operation() {
           System.out.println("扩展抽象化(Refined Abstraction)角色被访问");
           imple.OperationImpl();
       }
   }
   ```


   程序的运行结果如下：

   ```tex
   扩展抽象化(Refined Abstraction)角色被访问
   具体实现化(Concrete Implementor)角色被访问
   ```


## 外观模式

### 外观模式的定义与特点

外观（Facade）模式又叫作门面模式，是一种通过为多个复杂的子系统提供一个一致的接口，而使这些子系统更加容易被访问的模式。该模式对外有一个统一接口，外部应用程序不用关心内部子系统的具体细节，这样会大大降低应用程序的复杂度，提高了程序的可维护性。

在日常编码工作中，我们都在有意无意的大量使用外观模式。只要是高层模块需要调度多个子系统（2个以上的类对象），我们都会自觉地创建一个新的类封装这些子系统，提供精简的接口，让高层模块可以更加容易地间接调用这些子系统的功能。尤其是现阶段各种第三方SDK、开源类库，很大概率都会使用外观模式。

外观（Facade）模式是“迪米特法则”的典型应用，它有以下**主要优点。**

1. 降低了子系统与客户端之间的耦合度，使得子系统的变化不会影响调用它的客户类。
2. 对客户屏蔽了子系统组件，减少了客户处理的对象数目，并使得子系统使用起来更加容易。
3. 降低了大型软件系统中的编译依赖性，简化了系统在不同平台之间的移植过程，因为编译一个子系统不会影响其他的子系统，也不会影响外观对象。

外观（Facade）模式的**主要缺点**如下。

1. 不能很好地限制客户使用子系统类，很容易带来未知风险。
2. 增加新的子系统可能需要修改外观类或客户端的源代码，违背了“开闭原则”。

### 外观模式的结构与实现

外观（Facade）模式的结构比较简单，主要是定义了一个高层接口。它包含了对各个子系统的引用，客户端可以通过它访问各个子系统的功能。现在来分析其基本结构和实现方法。

1. 模式的结构

   外观（Facade）模式包含以下主要角色。

   1. 外观（Facade）角色：为多个子系统对外提供一个共同的接口。
   2. 子系统（Sub System）角色：实现系统的部分功能，客户可以通过外观角色访问它。
   3. 客户（Client）角色：通过一个外观角色访问各个子系统的功能。


   其结构图如图 2 所示。

   

   ![Facade.jpg](https://i.loli.net/2021/05/27/jwsvPmUQYH3Zugz.png)

   <center>图2 外观（Facade）模式的结构图</center>

2. 模式的实现

   外观模式的实现代码如下：

   ```java
   package facade;
   
   public class FacadePattern {
       public static void main(String[] args) {
           Facade f = new Facade();
           f.method();
       }
   }
   
   //外观角色
   class Facade {
       private SubSystem01 obj1 = new SubSystem01();
       private SubSystem02 obj2 = new SubSystem02();
       private SubSystem03 obj3 = new SubSystem03();
   
       public void method() {
           obj1.method1();
           obj2.method2();
           obj3.method3();
       }
   }
   
   //子系统角色
   class SubSystem01 {
       public void method1() {
           System.out.println("子系统01的method1()被调用！");
       }
   }
   
   //子系统角色
   class SubSystem02 {
       public void method2() {
           System.out.println("子系统02的method2()被调用！");
       }
   }
   
   //子系统角色
   class SubSystem03 {
       public void method3() {
           System.out.println("子系统03的method3()被调用！");
       }
   }
   ```


   程序运行结果如下：

   ```tex
   子系统01的method1()被调用！
   子系统02的method2()被调用！
   子系统03的method3()被调用！
   ```


## 组合模式

### 组合模式的定义与特点

组合（Composite Pattern）模式的定义：有时又叫作整体-部分（Part-Whole）模式，它是一种将对象组合成树状的层次结构的模式，用来表示“整体-部分”的关系，使用户对单个对象和组合对象具有一致的访问性，属于结构型[设计模式](http://c.biancheng.net/design_pattern/)。

组合模式一般用来描述整体与部分的关系，它将对象组织到树形结构中，顶层的节点被称为根节点，根节点下面可以包含树枝节点和叶子节点，树枝节点下面又可以包含树枝节点和叶子节点，树形结构图如下。

![node.jpg](https://i.loli.net/2021/05/27/3hNwa6nSEGAudrT.png)


由上图可以看出，其实根节点和树枝节点本质上属于同一种数据类型，可以作为容器使用；而叶子节点与树枝节点在语义上不属于用一种类型。但是在组合模式中，会把树枝节点和叶子节点看作属于同一种数据类型（用统一接口定义），让它们具备一致行为。

这样，在组合模式中，整个树形结构中的对象都属于同一种类型，带来的好处就是用户不需要辨别是树枝节点还是叶子节点，可以直接进行操作，给用户的使用带来极大的便利。

组合模式的**主要优点**有：

1. 组合模式使得客户端代码可以一致地处理单个对象和组合对象，无须关心自己处理的是单个对象，还是组合对象，这简化了客户端代码；
2. 更容易在组合体内加入新的对象，客户端不会因为加入了新的对象而更改源代码，满足“开闭原则”；

**其主要缺点是：**

1. 设计较复杂，客户端需要花更多时间理清类之间的层次关系；
2. 不容易限制容器中的构件；
3. 不容易用继承的方法来增加构件的新功能；

### 组合模式的结构与实现

组合模式的结构不是很复杂，下面对它的结构和实现进行分析。

1. 模式的结构

   组合模式包含以下主要角色。

   1. 抽象构件（Component）角色：它的主要作用是为树叶构件和树枝构件声明公共接口，并实现它们的默认行为。在透明式的组合模式中抽象构件还声明访问和管理子类的接口；在安全式的组合模式中不声明访问和管理子类的接口，管理工作由树枝构件完成。（总的抽象类或接口，定义一些通用的方法，比如新增、删除）
   2. 树叶构件（Leaf）角色：是组合中的叶节点对象，它没有子节点，用于继承或实现抽象构件。
   3. 树枝构件（Composite）角色 / 中间构件：是组合中的分支节点对象，它有子节点，用于继承和实现抽象构件。它的主要作用是存储和管理子部件，通常包含 Add()、Remove()、GetChild() 等方法。


   组合模式分为透明式的组合模式和安全式的组合模式。

   #### (1) 透明方式

   在该方式中，由于抽象构件声明了所有子类中的全部方法，所以客户端无须区别树叶对象和树枝对象，对客户端来说是透明的。但其缺点是：树叶构件本来没有 Add()、Remove() 及 GetChild() 方法，却要实现它们（空实现或抛异常），这样会带来一些安全性问题。其结构图如图 1 所示。

   

   ![Composite Pattern1.jpg](https://i.loli.net/2021/05/27/J4HzNAvhKSgyklR.png)

   <center>图1 透明式的组合模式的结构图</center>

   

   #### (2) 安全方式

   在该方式中，将管理子构件的方法移到树枝构件中，抽象构件和树叶构件没有对子对象的管理方法，这样就避免了上一种方式的安全性问题，但由于叶子和分支有不同的接口，客户端在调用时要知道树叶对象和树枝对象的存在，所以失去了透明性。其结构图如图 2 所示。

   

   ![Composite Pattern2.jpg](https://i.loli.net/2021/05/27/FCGBnb7LeU9mhfI.png)

   <center>图2 安全式的组合模式的结构图</center>

2. 模式的实现

   假如要访问集合 c0={leaf1,{leaf2,leaf3}} 中的元素，其对应的树状图如图 3 所示。

   

   ![gather.jpg](https://i.loli.net/2021/05/27/jaPRAwXLGBu3Vly.png)

   <center>图3 集合c0的树状图</center>

   #### 透明组合模式

   下面为透明式的组合模式的实现代码。

   ```java
   public class CompositePattern {
       public static void main(String[] args) {
           Component c0 = new Composite();
           Component c1 = new Composite();
           Component leaf1 = new Leaf("1");
           Component leaf2 = new Leaf("2");
           Component leaf3 = new Leaf("3");
           c0.add(leaf1);
           c0.add(c1);
           c1.add(leaf2);
           c1.add(leaf3);
           c0.operation();
       }
   }
   
   //抽象构件
   interface Component {
       public void add(Component c);
   
       public void remove(Component c);
   
       public Component getChild(int i);
   
       public void operation();
   }
   
   //树叶构件
   class Leaf implements Component {
       private String name;
   
       public Leaf(String name) {
           this.name = name;
       }
   
       public void add(Component c) {
       }
   
       public void remove(Component c) {
       }
   
       public Component getChild(int i) {
           return null;
       }
   
       public void operation() {
           System.out.println("树叶" + name + "：被访问！");
       }
   }
   
   //树枝构件
   class Composite implements Component {
       private ArrayList<Component> children = new ArrayList<Component>();
   
       public void add(Component c) {
           children.add(c);
       }
   
       public void remove(Component c) {
           children.remove(c);
       }
   
       public Component getChild(int i) {
           return children.get(i);
       }
   
       public void operation() {
           for (Object obj : children) {
               ((Component) obj).operation();
           }
       }
   }
   ```

   程序运行结果如下：

   ```
   树叶1：被访问！
   树叶2：被访问！
   树叶3：被访问！
   ```

   #### 安全组合模式

   安全式的组合模式与透明式组合模式的实现代码类似，只要对其做简单修改就可以了，代码如下。

   首先修改 Component 代码，只保留层次的公共行为。

   ```java
   interface Component {
       public void operation();
   }
   ```

   然后修改客户端代码，将树枝构件类型更改为 Composite 类型，以便获取管理子类操作的方法。

   ```java
   public class CompositePattern {
       public static void main(String[] args) {
           Composite c0 = new Composite();
           Composite c1 = new Composite();
           Component leaf1 = new Leaf("1");
           Component leaf2 = new Leaf("2");
           Component leaf3 = new Leaf("3");
           c0.add(leaf1);
           c0.add(c1);
           c1.add(leaf2);
           c1.add(leaf3);
           c0.operation();
       }
   }
   ```


## 享元模式

### 享元模式的定义与特点

享元（Flyweight）模式的定义：运用共享技术来有效地支持大量细粒度对象的复用。它通过共享已经存在的对象来大幅度减少需要创建的对象数量、避免大量相似类的开销，从而提高系统资源的利用率。

享元模式的**主要优点**是：相同对象只要保存一份，这降低了系统中对象的数量，从而降低了系统中细粒度对象给内存带来的压力。

**其主要缺点是：**

1. 为了使对象可以共享，需要将一些不能共享的状态外部化，这将增加程序的复杂性。
2. 读取享元模式的外部状态会使得运行时间稍微变长。

### 享元模式的结构与实现

享元模式的定义提出了两个要求，细粒度和共享对象。因为要求细粒度，所以不可避免地会使对象数量多且性质相近，此时我们就将这些对象的信息分为两个部分：内部状态和外部状态。

- 内部状态指对象共享出来的信息，存储在享元信息内部，并且不回随环境的改变而改变；
- 外部状态指对象得以依赖的一个标记，随环境的改变而改变，不可共享。


比如，连接池中的连接对象，保存在连接对象中的用户名、密码、连接URL等信息，在创建对象的时候就设置好了，不会随环境的改变而改变，这些为内部状态。而当每个连接要被回收利用时，我们需要将它标记为可用状态，这些为外部状态。

享元模式的本质是缓存共享对象，降低内存消耗。

1. 模式的结构

   享元模式的主要角色有如下。

   1. 抽象享元角色（Flyweight）：是所有的具体享元类的基类，为具体享元规范需要实现的公共接口，非享元的外部状态以参数的形式通过方法传入。
   2. 具体享元（Concrete Flyweight）角色：实现抽象享元角色中所规定的接口。
   3. 非享元（Unsharable Flyweight)角色：是不可以共享的外部状态，它以参数的形式注入具体享元的相关方法中。
   4. 享元工厂（Flyweight Factory）角色：负责创建和管理享元角色。当客户对象请求一个享元对象时，享元工厂检査系统中是否存在符合要求的享元对象，如果存在则提供给客户；如果不存在的话，则创建一个新的享元对象。


   图 1 是享元模式的结构图，其中：

   - UnsharedConcreteFlyweight 是非享元角色，里面包含了非共享的外部状态信息 info；
   - Flyweight 是抽象享元角色，里面包含了享元方法 operation(UnsharedConcreteFlyweight state)，非享元的外部状态以参数的形式通过该方法传入；
   - ConcreteFlyweight 是具体享元角色，包含了关键字 key，它实现了抽象享元接口；
   - FlyweightFactory 是享元工厂角色，它是关键字 key 来管理具体享元；
   - 客户角色通过享元工厂获取具体享元，并访问具体享元的相关方法。

   

   ![Flyweight.jpg](https://i.loli.net/2021/05/27/9tfi1AmGXQzEBbL.png)

   <center>图1 享元模式的结构图</center>

2. 模式的实现

   享元模式的实现代码如下：

   ```java
   public class FlyweightPattern {
       public static void main(String[] args) {
           FlyweightFactory factory = new FlyweightFactory();
           Flyweight f01 = factory.getFlyweight("a");
           Flyweight f02 = factory.getFlyweight("a");
           Flyweight f03 = factory.getFlyweight("a");
           Flyweight f11 = factory.getFlyweight("b");
           Flyweight f12 = factory.getFlyweight("b");
           f01.operation(new UnsharedConcreteFlyweight("第1次调用a。"));
           f02.operation(new UnsharedConcreteFlyweight("第2次调用a。"));
           f03.operation(new UnsharedConcreteFlyweight("第3次调用a。"));
           f11.operation(new UnsharedConcreteFlyweight("第1次调用b。"));
           f12.operation(new UnsharedConcreteFlyweight("第2次调用b。"));
       }
   }
   
   //非享元角色
   class UnsharedConcreteFlyweight {
       private String info;
   
       UnsharedConcreteFlyweight(String info) {
           this.info = info;
       }
   
       public String getInfo() {
           return info;
       }
   
       public void setInfo(String info) {
           this.info = info;
       }
   }
   
   //抽象享元角色
   interface Flyweight {
       public void operation(UnsharedConcreteFlyweight state);
   }
   
   //具体享元角色
   class ConcreteFlyweight implements Flyweight {
       private String key;
   
       ConcreteFlyweight(String key) {
           this.key = key;
           System.out.println("具体享元" + key + "被创建！");
       }
   
       public void operation(UnsharedConcreteFlyweight outState) {
           System.out.print("具体享元" + key + "被调用，");
           System.out.println("非享元信息是:" + outState.getInfo());
       }
   }
   
   //享元工厂角色
   class FlyweightFactory {
       private HashMap<String, Flyweight> flyweights = new HashMap<String, Flyweight>();
   
       public Flyweight getFlyweight(String key) {
           Flyweight flyweight = (Flyweight) flyweights.get(key);
           if (flyweight != null) {
               System.out.println("具体享元" + key + "已经存在，被成功获取！");
           } else {
               flyweight = new ConcreteFlyweight(key);
               flyweights.put(key, flyweight);
           }
           return flyweight;
       }
   }
   ```

   程序运行结果如下：

   ```tex
   具体享元a被创建！
   具体享元a已经存在，被成功获取！
   具体享元a已经存在，被成功获取！
   具体享元b被创建！
   具体享元b已经存在，被成功获取！
   具体享元a被调用，非享元信息是:第1次调用a。
   具体享元a被调用，非享元信息是:第2次调用a。
   具体享元a被调用，非享元信息是:第3次调用a。
   具体享元b被调用，非享元信息是:第1次调用b。
   具体享元b被调用，非享元信息是:第2次调用b。
   ```
