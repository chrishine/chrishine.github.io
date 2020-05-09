---
title: "设计模式：组合、委托、接口继承及类继承"
date: 2020-05-09T09:57:42+08:00
---

本篇博客本应一个礼拜前就写好的，因琐事缠身，兼喝酒缘故，故延后了一个礼拜。
本文内容大多从 thinking in Java 和 Gof 寻来，故也没太多可看的。先摘一句 Gof 的:

> 程序设计语言的选择非常重要，它将影响人们理解问题的出发点。我们的设计模式采用了Smalltalk和C++层的语言特性，这个选择实际上**决定了哪些机制可以方便的实现**  ，而哪些不能。若我们采用过程式语言，可能就要包括诸如“继承”、“封装”和“多态”的设计模式。

继承在设计模式中的重要性可见一斑，行文即为解释一些设计模式中的基础知识。



## 组合(黑箱复用：black-box reuse)

* 对象组合(object composition)是通过获得对其他对象的引用而在**运行时**动态定义的。

* 设计模式即是为了代码的高复用性及可扩展性。组合方式实现了代码的高复用性(显而易见)，又不破坏对象的封装性(因为是黑箱复用，我们只需要知道他对外提供的接口即可)。

* 组合使得我们可以从繁冗的细节中解放出来，可以在"知其然而不知其所以然的情况"下复用对象，降低了认知负担。

* 设计模式中有组合模式(composite pattern)，将对象组合成树形结构以表示“部分-整体”的层次结构，使得用户可以对单个对象和组合对象的使用具有一致性。个人认为此种模式类似于树的数据结构：
```java
  typedef struct tree{
    T data;
    tree[] *child;
  }
```
我们不需要区分叶子节点和树中节点：他们都是节点，不过叶子节点的孩子为空，如果想让叶子节点变成树中节点，只需要添加孩子(同时应该删除数据？)。

* 对象组合和组合模式只不过是名字相似("组合")，在我看来，对象组合与外观模式(Facade pattern)倒更为相似一些。外观模式为子系统中的一组接口提供了一个一致的界面，使得整个子系统更容易使用。假如外观模式是这样的：
```java
  class DemoFacade{
    //these methods may from other class, I ignore them.
    aMethod(); 
    anotherMethod();
    ...
    lastMethod();
  }      
```
如果不嫌麻烦，我们可以自己了解子系统中每一个方法是做什么的，然后组合这些数目可能庞大的方法来使用。然而通过外观模式，这些工作可以由那些更需要做这些工作的人去做(创建子系统的人)，我们(使用子系统的人)仅需要知道一个对外简单的接口即可。显然，这是一个黑箱复用，除了在有些时候我们这些使用子系统的人需要自己编写一个外观模式。

* 组合，是软件史伟大的进步。"知其然而不必知其所以然"使得生产力得到了巨大的解放。想一想"必须知其所以然"的情景吧：1960 年，所谓"软件开发"就是 IBM 公司的那种形式，慢慢一屋子的人，他们都戴着牛角质眼镜架，系着细细黑黑的领带，勤勉地埋头写代码，每人每天可以完成十行([黑客与画家](http://book.douban.com/subject/6021440/))。组合，让机器码变成了汇编以及 C 等，让写程序成为一个不太难的事情；对象组合，让抽象能力进一步提升，让写软件成为一个不太难的事情。



## 委托(delegation)

* 委托也是一种组合方法，不过它可使得组合具有与继承同样的复用能力。委托单独拿出来说的原因有以下：
  1. Java 没有对委托提供直接支持([thinking in Java](http://book.douban.com/subject/2130190/))。
  2. 有一些设计模式用到了委托：比如状态(Sate)模式、策略(Stratage)模式、访问者(Visitor)模式([设计模式](http://book.douban.com/subject/1052241/))等。

* 委托与那些通过对象组合以取得以取得软件灵活性的技术一样，具有如下不足之处：动态的、高度参数化的软件比静态软件更难于理解，同时运行低效(大概因为运行时确定)。

* 委托的形式大概为：
```java
  class DemoDelegation(){
    //there will be many subclasses inherit or implement Delegator
    //so when we use Delegator to new a subclass
    //we use the upcast
    private Delegator delegator;        
    public DemoDelegation(Delegator delegator){
        this.delegator = delegator;
    }
    //there will be many doSomething methods
    //it depends on the delegator 
    public doSomething(Object... args){
        delegator.doSomething(Object... args);
    }
  }
```



## 接口继承

* 将继承分为接口继承和非接口继承主要是因为两者目的不同。接口继承侧重于对象替代对象，非接口继承侧重于实现定义另一个实现，是代码和表示的共享机制(设计模式 P12)，这句话十分精妙，言传意会。

* 初看了一下 Head First设计模式，自己是通过自由程度区分接口继承和非接口继承的。接口继承大概相当于定义了战略，具体战术自己实现，自由度较高；非接口继承大概相当于定义了战术，自己只能去按照战术进行安排。接口继承，如三国时合肥之战，曹操锦囊：若孙权带军前来，张辽李典出站，乐进守城云，最后战果不错——威震逍遥津是张辽武勇的巅峰。如三国演义中，诸葛亮每次嘱咐诸将出站只须如此如此，即是非接口继承。诸葛亮在三国演义中有后人评价"状诸葛多智而近于妖"，非接口继承的要求之高可见一斑。

* 什么叫做接口继承，抽象类继承属于接口继承还是非接口继承？在我看来，抽象类继承也可属于接口继承。抽象类一般都有具体方法(如果没有完全可以用接口代替，还能"实现多继承")，这些方法应该服务于抽象方法，他们之所以具体的原因是所有子类的具体实现都是同样的，原则上不允许被重写，故抽象类继承可属于接口继承。

* 接口继承对象的替代性主要表现在向上转型(upcast)，向下转型(downcast)。向上转型向下转型大概类似于：
```java
  interface A{methodA();}
  interface B{methodB();}
  class Parent{methodParent(){System.out.println("methodParent");}}
  class Sub extends Parent implements A,B{
    methodA(System.out.println("methodA"));
    methodB(System.out.println("methodB"));
    methodSub(System.out.println("methodSub"));//its own method
  }        
  public static void main(String[] args){
    //use upcast, a is a instance of A,so it only has the methodA;B as with A 
    A a = new Sub();
    a.methodA(); 
    B b = new Sub();
    b.methodB();
    //use upcast, p is a instance of Parent,so it only has the methodParent
    Parent p = new Sub();
    p.methodParent();
    //use downcast, a is a instance of Sub,it has all the methods
    ((Sub)a).methodB();
    ((Sub)a).methodParent();
    ((Sub)a).methodSub();
  }   
```



## 非接口继承(white-box reuse)

* 非接口继承基本就是类继承。类继承，当然要求对父类有足够的理解，然后通过继承来定义一个新类的实现；由于父类的内部细节对子类可见，因而也叫做白箱复用。类继承在编译时定义，所以无法在运行时改变从父类继承的实现。由于父类通常至少定义了部分子类的具体表示，所以继承通常被认为"破坏了封装性"。子类中的实现与它的父类有紧密的依赖关系，以至于父类实现中的任何变化必然会导致子类发生变化。

* 显然，类继承支持的变化比较少，因而往往是优先级最低的复用方式。

* 按照thinking in Java中所说"程序开发人员按照角色分为类创建者(那些创建新数据类型的程序员)和客户端程序员(那些在其应用中使用部分数据类型的消费者)"。类继承往往是类创建者使用较多，客户端程序员不应该频繁使用类继承。

* 常见的类继承原因(本人所知甚少，故待补充)：
  1. 重写类名。类名要求简单直白易懂，但是这样做到很难。比如异常类，大多是从```Exception```类继承而来，大多的子类重写方法都是一路super，很少有自己新加的方法。这个时候的类继承主要目的就是重写类名，如果只是简单抛出一个```Exception```，我们还要猜测异常可能的原因，但是如果是抛出一个```ClassCastException```，或一个```NullPointerException```，我们马上可以猜测到原因。
  2. 留给将来吧。



## 吐槽下Java

Java 的 getter/setter 有时占着一些代码，虽说丢在代码最下面，也不影响读代码。但是能自动生成的方法，为何不考虑一个更好的方法？变量注解似乎不错。不过有时候要覆盖 getter/setter 方法，这个时候的注解数目，略壮观。