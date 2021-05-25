---
layout:     post
title:      "设计模式-创建模式"
subtitle:   
date:       2021-05-23 20:00:00
author:     "Balbo"
header-img: "img/post-bg-2021.jpg"
tags:
    - software-designer
---

## 工厂方法模式

### 装饰器模式的定义与特点

**优点：**

- 用户只需要知道具体工厂的名称就可得到所要的产品，无须知道产品的具体创建过程。
- 灵活性增强，对于新产品的创建，只需多写一个相应的工厂类。
- 典型的解耦框架。高层模块只需要知道产品的抽象类，无须关心其他实现类，满足迪米特法则、依赖倒置原则和里氏替换原则。

**缺点：**

- 类的个数容易过多，增加复杂度
- 增加了系统的抽象性和理解难度
- 抽象产品只能生产一种产品，此弊端可使用抽象工厂模式解决。

应用场景：

- 客户只知道创建产品的工厂名，而不知道具体的产品名。如 TCL 电视工厂、海信电视工厂等。
- 创建对象的任务由多个具体子工厂中的某一个完成，而抽象工厂只提供创建产品的接口。
- 客户不关心创建产品的细节，只关心产品的品牌

### 模式的结构与实现

1. 模式的结构

   工厂方法模式的主要角色如下。

   1. 抽象工厂（Abstract Factory）：提供了创建产品的接口，调用者通过它访问具体工厂的工厂方法 newProduct() 来创建产品。
   2. 具体工厂（ConcreteFactory）：主要是实现抽象工厂中的抽象方法，完成具体产品的创建。
   3. 抽象产品（Product）：定义了产品的规范，描述了产品的主要特性和功能。
   4. 具体产品（ConcreteProduct）：实现了抽象产品角色所定义的接口，由具体工厂来创建，它同具体工厂之间一一对应。

   ![Factory.jpg](https://i.loli.net/2021/05/25/yKwcleaIdvZ6FtY.png)

   <center>图1 工厂方法模式的结构图</center>

2. 模式的实现

   根据图 1 写出该模式的代码如下：

   ```java
   package FactoryMethod;
   
   public class AbstractFactoryTest {
       public static void main(String[] args) {
           try {
               Product a;
               AbstractFactory af;
               af = (AbstractFactory) ReadXML1.getObject();
               a = af.newProduct();
               a.show();
           } catch (Exception e) {
               System.out.println(e.getMessage());
           }
       }
   }
   
   //抽象产品：提供了产品的接口
   interface Product {
       public void show();
   }
   
   //具体产品1：实现抽象产品中的抽象方法
   class ConcreteProduct1 implements Product {
       public void show() {
           System.out.println("具体产品1显示...");
       }
   }
   
   //具体产品2：实现抽象产品中的抽象方法
   class ConcreteProduct2 implements Product {
       public void show() {
           System.out.println("具体产品2显示...");
       }
   }
   
   //抽象工厂：提供了厂品的生成方法
   interface AbstractFactory {
       public Product newProduct();
   }
   
   //具体工厂1：实现了厂品的生成方法
   class ConcreteFactory1 implements AbstractFactory {
       public Product newProduct() {
           System.out.println("具体工厂1生成-->具体产品1...");
           return new ConcreteProduct1();
       }
   }
   
   //具体工厂2：实现了厂品的生成方法
   class ConcreteFactory2 implements AbstractFactory {
       public Product newProduct() {
           System.out.println("具体工厂2生成-->具体产品2...");
           return new ConcreteProduct2();
       }
   }
   ```

## 抽象工厂模式

### 单例模式的定义与特点

单例（Singleton）模式的定义：指一个类只有一个实例，且该类能自行创建这个实例的一种模式。例如，Windows 中只能打开一个任务管理器，这样可以避免因打开多个任务管理器窗口而造成内存资源的浪费，或出现各个窗口显示内容的不一致等错误。

在计算机系统中，还有 Windows 的回收站、操作系统中的文件系统、多线程中的线程池、显卡的驱动程序对象、打印机的后台处理服务、应用程序的日志对象、数据库的连接池、网站的计数器、Web 应用的配置对象、应用程序中的对话框、系统中的缓存等常常被设计成单例。

单例模式在现实生活中的应用也非常广泛，例如公司 CEO、部门经理等都属于单例模型。J2EE 标准中的 [Servlet](http://c.biancheng.net/servlet/)Context 和 ServletContextConfig、[Spring](http://c.biancheng.net/spring/) 框架应用中的 ApplicationContext、数据库中的连接池等也都是单例模式。

单例模式有 3 个特点：

1. 单例类只有一个实例对象；
2. 该单例对象必须由单例类自行创建；
3. 单例类对外提供一个访问该单例的全局访问点。

**单例模式的优点和缺点**

单例模式的优点：

- 单例模式可以保证内存里只有一个实例，减少了内存的开销。
- 可以避免对资源的多重占用。
- 单例模式设置全局访问点，可以优化和共享资源的访问。


单例模式的缺点：

- 单例模式一般没有接口，扩展困难。如果要扩展，则除了修改原来的代码，没有第二种途径，违背开闭原则。
- 在并发测试中，单例模式不利于代码调试。在调试过程中，如果单例中的代码没有执行完，也不能模拟生成一个新的对象。
- 单例模式的功能代码通常写在一个类中，如果功能设计不合理，则很容易违背单一职责原则。

> 单例模式看起来非常简单，实现起来也非常简单。单例模式在面试中是一个高频面试题。希望大家能够认真学习，掌握单例模式，提升核心竞争力，给面试加分，顺利拿到 Offer。

### 单例模式的应用场景

对于 [Java](http://c.biancheng.net/java/) 来说，单例模式可以保证在一个 JVM 中只存在单一实例。单例模式的应用场景主要有以下几个方面。

- 需要频繁创建的一些类，使用单例可以降低系统的内存压力，减少 GC。
- 某类只要求生成一个对象的时候，如一个班中的班长、每个人的身份证号等。
- 某些类创建实例时占用资源较多，或实例化耗时较长，且经常使用。
- 某类需要频繁实例化，而创建的对象又频繁被销毁的时候，如多线程的线程池、网络连接池等。
- 频繁访问数据库或文件的对象。
- 对于一些控制硬件级别的操作，或者从系统上来讲应当是单一控制逻辑的操作，如果有多个实例，则系统会完全乱套。
- 当对象需要被共享的场合。由于单例模式只允许创建一个对象，共享该对象可以节省内存，并加快对象访问速度。如 Web 中的配置对象、数据库的连接池等。

### 单例模式的结构与实现

单例模式是[设计模式](http://c.biancheng.net/design_pattern/)中最简单的模式之一。通常，普通类的构造函数是公有的，外部类可以通过“new 构造函数()”来生成多个实例。但是，如果将类的构造函数设为私有的，外部类就无法调用该构造函数，也就无法生成多个实例。这时该类自身必须定义一个静态私有实例，并向外提供一个静态的公有函数用于创建或获取该静态私有实例。

下面来分析其基本结构和实现方法。

1. 单例模式的结构

   单例模式的主要角色如下。

   - 单例类：包含一个实例且能自行创建这个实例的类。
   - 访问类：使用单例的类。

   ![Singleton.jpg](https://i.loli.net/2021/05/25/WIKFZcpq5f8naB1.png)

   <center>图1 单例模式的结构图</center>

2. 单例模式的实现

   Singleton 模式通常有两种实现形式。

   #### 第 1 种：懒汉式单例

   该模式的特点是类加载时没有生成单例，只有当第一次调用 getlnstance 方法时才去创建这个单例。代码如下：

   ```java
   public class LazySingleton {
       private static volatile LazySingleton instance = null;    //保证 instance 在所有线程中同步
   
       private LazySingleton() {
       }    //private 避免类在外部被实例化
   
       public static synchronized LazySingleton getInstance() {
           //getInstance 方法前加同步
           if (instance == null) {
               instance = new LazySingleton();
           }
           return instance;
       }
   }
   ```

   注意：如果编写的是多线程程序，则不要删除上例代码中的关键字 volatile 和 synchronized，否则将存在线程非安全的问题。如果不删除这两个关键字就能保证线程安全，但是每次访问时都要同步，会影响性能，且消耗更多的资源，这是懒汉式单例的缺点。

   #### 第 2 种：饿汉式单例

   该模式的特点是类一旦加载就创建一个单例，保证在调用 getInstance 方法之前单例已经存在了。

   ```java
   public class HungrySingleton {
       private static final HungrySingleton instance = new HungrySingleton();
   
       private HungrySingleton() {
       }
   
       public static HungrySingleton getInstance() {
           return instance;
       }
   }
   ```


   饿汉式单例在类创建的同时就已经创建好一个静态的对象供系统使用，以后不再改变，所以是线程安全的，可以直接用于多线程而不会出现问题。

## 建造者模式

### 模式的定义与特点

建造者（Builder）模式的定义：指将一个复杂对象的构造与它的表示分离，使同样的构建过程可以创建不同的表示，这样的[设计模式](http://c.biancheng.net/design_pattern/)被称为建造者模式。它是将一个复杂的对象分解为多个简单的对象，然后一步一步构建而成。它将变与不变相分离，即产品的组成部分是不变的，但每一部分是可以灵活选择的。

该模式的**主要优点**如下：

1. 封装性好，构建和表示分离。
2. 扩展性好，各个具体的建造者相互独立，有利于系统的解耦。
3. 客户端不必知道产品内部组成的细节，建造者可以对创建过程逐步细化，而不对其它模块产生任何影响，便于控制细节风险。

**其缺点如下：**

1. 产品的组成部分必须相同，这限制了其使用范围。
2. 如果产品的内部变化复杂，如果产品内部发生变化，则建造者也要同步修改，后期维护成本较大。


建造者（Builder）模式和工厂模式的关注点不同：建造者模式注重零部件的组装过程，而[工厂方法模式](http://c.biancheng.net/view/1348.html)更注重零部件的创建过程，但两者可以结合使用。

### 模式的结构与实现

建造者（Builder）模式由产品、抽象建造者、具体建造者、指挥者等 4 个要素构成，现在我们来分析其基本结构和实现方法。

1. 模式的结构

   建造者（Builder）模式的主要角色如下。

   1. 产品角色（Product）：它是包含多个组成部件的复杂对象，由具体建造者来创建其各个零部件。
   2. 抽象建造者（Builder）：它是一个包含创建产品各个子部件的抽象方法的接口，通常还包含一个返回复杂产品的方法 getResult()。
   3. 具体建造者(Concrete Builder）：实现 Builder 接口，完成复杂产品的各个部件的具体创建方法。
   4. 指挥者（Director）：它调用建造者对象中的部件构造与装配方法完成复杂对象的创建，在指挥者中不涉及具体产品的信息。

   

   ![Builder.jpg](https://i.loli.net/2021/05/25/cgf31QZWpwhOxVe.png)

   <center>图1 建造者模式的结构图</center>

2. 模式的实现

   图 1 给出了建造者（Builder）模式的主要结构，其相关类的代码如下。

   (1) 产品角色：包含多个组成部件的复杂对象。

   ```java
   class Product {
       private String partA;
       private String partB;
       private String partC;
   
       public void setPartA(String partA) {
           this.partA = partA;
       }
   
       public void setPartB(String partB) {
           this.partB = partB;
       }
   
       public void setPartC(String partC) {
           this.partC = partC;
       }
   
       public void show() {
           //显示产品的特性
       }
   }
   ```


   (2) 抽象建造者：包含创建产品各个子部件的抽象方法。

   ```java
   abstract class Builder {
       //创建产品对象
       protected Product product = new Product();
   
       public abstract void buildPartA();
   
       public abstract void buildPartB();
   
       public abstract void buildPartC();
   
       //返回产品对象
       public Product getResult() {
           return product;
       }
   }
   ```


   (3) 具体建造者：实现了抽象建造者接口。

   ```java
   public class ConcreteBuilder extends Builder {
       public void buildPartA() {
           product.setPartA("建造 PartA");
       }
   
       public void buildPartB() {
           product.setPartB("建造 PartB");
       }
   
       public void buildPartC() {
           product.setPartC("建造 PartC");
       }
   }
   ```


   (4) 指挥者：调用建造者中的方法完成复杂对象的创建。

   ```java
   class Director {
       private Builder builder;
   
       public Director(Builder builder) {
           this.builder = builder;
       }
   
       //产品构建与组装方法
       public Product construct() {
           builder.buildPartA();
           builder.buildPartB();
           builder.buildPartC();
           return builder.getResult();
       }
   }
   ```


   (5) 客户类。

   ```java
   public class Client {
       public static void main(String[] args) {
           Builder builder = new ConcreteBuilder();
           Director director = new Director(builder);
           Product product = director.construct();
           product.show();
       }
   }
   ```


## 原型模式

### 原型模式的定义与特点

原型（Prototype）模式的定义如下：用一个已经创建的实例作为原型，通过复制该原型对象来创建一个和原型相同或相似的新对象。在这里，原型实例指定了要创建的对象的种类。用这种方式创建对象非常高效，根本无须知道对象创建的细节。例如，Windows 操作系统的安装通常较耗时，如果复制就快了很多。在生活中复制的例子非常多，这里不一一列举了。

**原型模式的优点：**

- [Java](http://c.biancheng.net/java/) 自带的原型模式基于内存二进制流的复制，在性能上比直接 new 一个对象更加优良。
- 可以使用深克隆方式保存对象的状态，使用原型模式将对象复制一份，并将其状态保存起来，简化了创建对象的过程，以便在需要的时候使用（例如恢复到历史某一状态），可辅助实现撤销操作。

**原型模式的缺点：**

- 需要为每一个类都配置一个 clone 方法
- clone 方法位于类的内部，当对已有类进行改造的时候，需要修改代码，违背了开闭原则。
- 当实现深克隆时，需要编写较为复杂的代码，而且当对象之间存在多重嵌套引用时，为了实现深克隆，每一层对象对应的类都必须支持深克隆，实现起来会比较麻烦。因此，深克隆、浅克隆需要运用得当。

### 原型模式的结构与实现

由于 Java 提供了对象的 clone() 方法，所以用 Java 实现原型模式很简单。

1. 模式的结构

   原型模式包含以下主要角色。

   1. 抽象原型类：规定了具体原型对象必须实现的接口。
   2. 具体原型类：实现抽象原型类的 clone() 方法，它是可被复制的对象。
   3. 访问类：使用具体原型类中的 clone() 方法来复制新的对象。


   其结构图如图 1 所示。

   

   ![Prototype.jpg](https://i.loli.net/2021/05/25/V4tY6ZWhMjTXBs5.png)

   <center>图1 原型模式的结构图</center>

2. 模式的实现

   原型模式的克隆分为浅克隆和深克隆。

   - 浅克隆：创建一个新对象，新对象的属性和原来对象完全相同，对于非基本类型属性，仍指向原有属性所指向的对象的内存地址。
   - 深克隆：创建一个新对象，属性中引用的其他对象也会被克隆，不再指向原有对象地址。


   Java 中的 Object 类提供了浅克隆的 clone() 方法，具体原型类只要实现 Cloneable 接口就可实现对象的浅克隆，这里的 Cloneable 接口就是抽象原型类。其代码如下：

   ```java
   //具体原型类
   class Realizetype implements Cloneable {
       Realizetype() {
           System.out.println("具体原型创建成功！");
       }
   
       public Object clone() throws CloneNotSupportedException {
           System.out.println("具体原型复制成功！");
           return (Realizetype) super.clone();
       }
   }
   
   //原型模式的测试类
   public class PrototypeTest {
       public static void main(String[] args) throws CloneNotSupportedException {
           Realizetype obj1 = new Realizetype();
           Realizetype obj2 = (Realizetype) obj1.clone();
           System.out.println("obj1==obj2?" + (obj1 == obj2));
       }
   }
   ```

   程序的运行结果如下：

   ```tex
   具体原型创建成功！
   具体原型复制成功！
   obj1==obj2?false
   ```