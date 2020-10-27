---
typora-root-url: images
---

Java设计模式

# 第 1 章 内容和授课方式

### 1.1 Java设计模式内容介绍

先看几个经典的面试题

原型设计模式问题：

1、有请使用UML类图画出原型模式核心角色

2、原型设计模式的深拷贝和浅拷贝是什么，并写出深拷贝和浅拷贝的两种方式的源码(重写clone方法实现深拷贝、使用序列化来实现深拷贝)

3、在Spring框架中那里使用到了原型模式，并对源码进行分析

beans.xml

<bean id="id01" class="com.atguigu.spring.bean.Monster" scope="prototype" />



- ### 先看几个经典设计模式面试题

设计模式七大原则 要求1) 七大设计原则核心思想 2) 能够以类图说明设计原则 3)在项目实际开发中，你那里用了 ocp 原则

![](/Snipaste_2020-09-28_09-05-13.png)

- ### 解释器设计模式

1、介绍解释器设计模式是什么？

2、画出解释器设计模式的 UML 类图，分析设计模式中各个角色是什么？

3、请说明 Spring 框架中，那里使用到了解释器设计模式，并作出源码级别的分析

4、Spring 框架中 SpelExpressionParser 就使用到了解释器模式

5、代码分析+Debug源码+模式角色分析说明

![](/Snipaste_2020-09-28_09-09-42.png)



- ### 单例设计模式一共有几种实现方式？ 请分别用代码实现，并说明各个实现方式的优点和缺点?

单例设计模式一共有8中写法，后面会依次讲到

饿汉式 两种

懒汉式 两种

双重检查

静态内部类

枚举



- ### 设计模式的重要性

1、软件工程中，**设计模式 (design pattern)** 是对软件设计中**普通存在(反复出现)** 的各种问题，所提出的解决方案 这个术语是埃里希·伽玛 等人 在1990年代从建筑设计领域引入到计算机科学的

2、大厦 VS 简易房

3、拿实际工作经历来说，当一个项目开发完后，如果**客户提出增新功能**，怎么办？(可扩展性,使用了设计模式，软件具有很好的扩展性)

4、如果项目开发完后，原来程序员离职，你接手维护的项目怎么办?（**维护性**(可读性、规范性)）

5、目前程序员门槛越来越高，一线公司(大厂)，都会问你在实际项目中**使用过什么设计模式，怎么使用的，解决了什么问题**

6、**设计模式在软件那里**？面向对象(oo)=>功能设计模块(设计模式+算法(数据结构))=>框架[使用到多种设计模式]=>架构[服务i集群]

7、如果想要成为合格的软件**工程师**，那就花时间来研究下设计模式是非常必要的



# 第 2 章 设计模式七大原则

### 2.1设计模式的目的

编写软件过程中，程序员面临着**耦合性，内聚性以及可维护性，可扩展性，重用性，灵活性**，等多方面的条件，设计模式是为了让程序(软件),具有更好

1、代码重用性(即：相同功能的代码，不用多次编写)
2、可读性(即：编程规范性，便于其他程序员的阅读和理解)

3、可扩展性(即：当需要增加新功能的时候，非常的方便，称为可维护)

4、可靠性(即：当我们增加新功能的时候，对原来的功能没有影响)

5、使程序程先高内聚，低耦合的特性

​	分享金句

6、设计模式包含了面向对象的精髓，“懂了设计模式，你就懂了面向对象分析和设计，(OOP/D)的精要”

7、Scott Myers 在其巨著《Effective C++》就曾经说过 C++的老手和 C++的新手区别是前者手背上有很多伤疤



### 2.2 设计模式七大原则

设计模式原则，其实就是程序员在编程时，应当遵守的原则，也是各种设计模式的基础(即：**设计模式为什么这样的依据**)

设计模式常用的七大原则是

1、单一职责原则

2、接口隔离原则

3、依赖倒转(倒置)原则

4、里式替换原则

5、开闭原则

6、迪米特原则

7、**合成复用原则**



### 2.3 单一职责原则

#### 	2.3.1 基本介绍

对类来说的，**即一个类应该只负责一项职责**，如类A负责两个不同的职责，职责1，职责2，当职责1需求变更而改变A时，可能造成职责2 执行错误，所以需要将类A的粒度分解为A1 A2

#### 	2.3.2 应用实例

​	以交通工具案例讲解

​	看老师代码演示

1. 方案1

   ```java
   
   /**
    * @author gcq
    * @Create 2020-09-28
    */
   public class SingleResponsibility1 {
       public static void main(String[] args) {
           Vehicle vehicle = new Vehicle();
           vehicle.run("摩托车");
           vehicle.run("汽车");
           vehicle.run("飞机");
       }
   }
   
   // 交通工具类
   // 方式1
   // 1、在方式1的run方法中，违反了单一职责原则
   // 2、解决的方案非常简单，根据交通工具的运行方法不同，分解成不同类即可
   class Vehicle {
       public void run(String vehicle) {
           System.out.println(vehicle + " 在公路上运行...");
       }
   }
   
   ```

   

2. 方案2

   ```java
   /**
    * @author gcq
    * @Create 2020-09-28
    */
   public class SingleResponsibility2 {
       public static void main(String[] args) {
           RoadVehicle roadVehicle = new RoadVehicle();
           roadVehicle.run("摩托车");
           roadVehicle.run("汽车");
           AirVehicle airVehicle = new AirVehicle();
           airVehicle.run("飞机");
       }
   }
   //方案2 分析
   // 1、遵守单一职责原则
   // 2、但是这样做的改动很大，即将类分解，同时修改客户端
   // 3、改进：直接修改Vehicle类，改动代码会比较少=>方案3
   class RoadVehicle {
       public void run(String vehicle) {
           System.out.println(vehicle + " 公路运行");
       }
   }
   class AirVehicle {
       public void run(String vehicle) {
           System.out.println(vehicle + " 天空运行");
       }
   }
   ```

3. 方案3

   ```java
   
   /**
    * @author gcq
    * @Create 2020-09-28
    */
   public class SingleResponsibility3 {
       public static void main(String[] args) {
           Vehicle2 vehicle2 = new Vehicle2();
           vehicle2.run("骑车");
           vehicle2.runAir("飞机");
           vehicle2.runWater("船");
       }
   }
   
   // 方式3的分析
   // 1、这种修改方法没有对原来的类做打的修改，只是增加方法
   // 2、这里虽然没有在类这个级别遵守单一职责原则，但是在方法级别上，任然是遵守单一职责
   class Vehicle2 {
       public void run(String vehicle) {
           System.out.println(vehicle +" 在公路上运行....");
       }
   
       public void runAir(String vehicle) {
           System.out.println(vehicle + " 在天空上运行....");
       }
   
       public void runWater(String vehicle) {
           System.out.println(vehicle + " 在水上运行");
       }
   }
   ```

   #### 2.3.3 单一职责原则注意事项和细节

   1、降低类的复杂度，一个类只负责一项职责

   2、提高类的可读性，可维护性

   3、降低变更引起的风险

   4、通常情况下，我们应当遵守单一职责原则，只有逻辑足够简单，才可以在代码违反单一职责原则，只有类中方法数量足够少，可以在方法级别保持单一职责原则



### 2.4 接口隔离原则(Interface Segregation Principle)

#### 	2.4.1 基本介绍

​	1、客户端不应该依赖它不需要的接口，即一个**类对另一个类的依赖应该建立在最小接口**上

​	2、先看一张图

![](/Snipaste_2020-09-28_13-17-23.png)

​	3、类 A 通过接口 Interface1 依赖B ，类C 通过接口 Interface 依赖D，如果接口 Interface1 对于类 A 和类 C 说不是最小接口，那么类 B 和类 D必须去实现他们不需要的方法

​	4、按隔离原则应当这样处理

​	**将接口 Interface1** 拆分为独立的几个接口(**这里我们拆分成三个接口)**，类 A 和 类 C 分别与他们需要的接口建立依赖关系，也就是采用接口隔离原则

#### 	2.4.2 应用实例

​	1、类A通过接口 interface1 依赖B 类C通过接口 Interface1依赖类D 请编写代码完成此实例，

​	2、老师代码，没有用接口隔离原则代码

```java
/**
 * @author gcq
 * @Create 2020-09-28
 */
public class Segregation1 {

}

interface Interface1 {
    void operation1();
    void operation2();
    void operation3();
    void operation4();
    void operation5();
}
class B implements Interface1 {
    @Override
    public void operation1() {
        System.out.println("B 实现了Operation1");
    }

    @Override
    public void operation2() {
        System.out.println("B 实现了Operation2");
    }

    @Override
    public void operation3() {
        System.out.println("B 实现了Operation3");
    }

    @Override
    public void operation4() {
        System.out.println("B 实现了Operation4");
    }

    @Override
    public void operation5() {
        System.out.println("B 实现了Operation5");
    }
}
class D implements Interface1 {
    @Override
    public void operation1() {
        System.out.println("D 实现了Operation1");
    }

    @Override
    public void operation2() {
        System.out.println("D 实现了Operation2");
    }

    @Override
    public void operation3() {
        System.out.println("D 实现了Operation3");
    }

    @Override
    public void operation4() {
        System.out.println("D 实现了Operation4");
    }

    @Override
    public void operation5() {
        System.out.println("D 实现了Operation5");
    }
}

class A { // A 类通过接口Interface1 依赖使用B类 但是只会用到1，2，3方饭
    public void depend1(Interface1 i){
        i.operation1();
    }
    public void depend(Interface1 i) {
        i.operation2();
    }
    public void depend3(Interface1 i) {
        i.operation3();
    }
}
class C { //C 类通过接口Interface1 依赖使用 D类 但是只会用到 1，4，5 方法
    public void depend1(Interface1 i) {
        i.operation1();
    }
    public void depend4(Interface1 i) {
        i.operation4();
    }
    public void depend5(Interface1 i) {
        i.operation5();
    }
}
```

#### 	2.4.3 应传统方法的问题和使用接口隔离原则改进

类 A 通过接口 Interface1 依赖类B 类C 通过接口 Interface1 依赖类D ，如果接口 Interface1对类A 和类C来说不是最小接口，那么类B和类D 必须去实现他们不需要的方法

将接口 Interface1 拆分成独立的几个接口，类A 和类C 分别与他们需要的接口建立依赖关系，也就是采用接口隔离原则

接口 interface1出现的方法，根据实际情况拆分为三个接口

![](/Snipaste_2020-09-28_13-17-34.png)

代码实现

```java
package com.atguigu.principle.segregation.improve;

/**
 * @author gcq
 * @Create 2020-09-28
 */
public class Segregation1 {
    public static void main(String[] args) {
        A a = new A();
        // A类通过接口去依赖B类
        a.depend1(new B());
        a.depend2(new B());
        a.depend3(new B());
        C c = new C();
        // C类通过接口去依赖(使用)D类
        c.depend1(new D());
        c.depend4(new D());
        c.depend5(new D());

    }

}

//接口1
interface Interface1 {
    void operation1();
}
// 接口2
interface Interface2 {
    void operation2();
    void operation3();
}
// 接口3
interface Interface3{
    void operation4();
    void operation5();
}

class B implements Interface1,Interface2 {
    @Override
    public void operation1() {
        System.out.println("B 实现了Operation1");
    }

    @Override
    public void operation2() {
        System.out.println("B 实现了Operation2");
    }

    @Override
    public void operation3() {
        System.out.println("B 实现了Operation3");
    }
}

class D implements Interface1,Interface3 {
    @Override
    public void operation1() {
        System.out.println("D 实现了Operation1");
    }
    @Override
    public void operation4() {
        System.out.println("D 实现了Operation4");
    }

    @Override
    public void operation5() {
        System.out.println("D 实现了Operation5");
    }
}

class A { // A 类通过接口Interface1,Interface2 依赖使用B类 但是只会用到1，2，3方饭
    public void depend1(Interface1 i){ i.operation1(); }
    public void depend2(Interface2 i){
        i.operation2();
    }
    public void depend3(Interface2 i){
        i.operation3();
    }
}

class C { //C 类通过接口Interface1 Interface3 依赖使用 D类 但是只会用到 1，4，5 方法
    // 参数是接口
    public void depend1(Interface1 i) {
        i.operation1();
    }
    public void depend4(Interface3 i) {
        i.operation4();
    }
    public void depend5(Interface3 i) {
        i.operation5();
    }
}
```



### 2.5 依赖倒转原则



#### 2.5.1基本介绍

1. 依赖倒转原则(Dependence Inversion Principle)是指

2. 高层模块不应该依赖底层模块，二者都应该实现其抽象

3. **抽象不应该依赖细节，细节应该依赖对象**

4. 依赖倒转(倒置)的中心思想是**面向接口编程**

5. 依赖倒转原则是基于这样的设计理念，相对于细节多变性，抽象的东西要稳定得多，以抽象为基础搭建的架构比细节为基础的架构要稳定的多，在Java中，重选ing指的是接口或抽象类，细节就是具体的实现类

6. 使用**接口或抽象类**目的是制**定好规范**，而不涉及任何具体的操作，把**展现细节的任务交给他们实现类**去完成

   

#### 2.5.2 应用案例

请编程完成Person接收消息功能

1、实现方案1+分析说明

```java
package com.atguigu.principle.inversion;

/**
 * @author gcq
 * @Create 2020-09-28
 */
public class DependecyInversion {
    public static void main(String[] args) {
        Person person = new Person();
        person.receive(new Email());
    }
}
class Email {
    public String getInfo() {
        return "电子邮件消息：hello world";
    }
}
// 完成Person接收消息功能
//方式1完成分析
// 1、简单，比较容易想到
// 2、如果我们获取的对象是微信，短信等等，则新增类，同时Persons也要增加相应的接收方法
// 3、解决思路 引入一个抽象的接口 IReceiver 表示接收者，这样Person类与接口IReceiver发送依赖
// 因为Email、WeXin,等等属于接收的范围，他们各自实现IReceiver 接口就ok，这样我们就符合依赖倒转原则
class Person {
    public void receive(Email email) {
        System.out.println(email.getInfo());
    }
}
```

2、实现方案2+分析说明

```java
package com.atguigu.principle.inversion.improve;

/**
 * @author gcq
 * @Create 2020-09-28
 */
public class DependecyInversion {
    public static void main(String[] args) {
        // 客户端无需改变
        Person person = new Person();
        person.receive(new Email());
        person.receive(new WeiXin());
    }
}

interface IReceiver {
    public String getInfo();
}
class Email implements IReceiver {
    @Override
    public String getInfo() {
        return "电子邮件消息：hello world";
    }
}
// 增加微信
class WeiXin implements IReceiver {

    @Override
    public String getInfo() {
        return "微信消息: hello ok";
    }
}

class Person {
    public void receive(IReceiver receiver) {
        System.out.println(receiver.getInfo());
    }
}
```

 2.5.3 依赖关系传递的三种方式和应用案例

1、接口传递

2、构造方法传递

3、setter方式传递

代码

```java
package com.atguigu.principle.inversion.improve;

public class DependencyPass {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        ChangHong changHong = new ChangHong();
        // 接口
//        OpenAndClose openAndClose = new OpenAndClose();
//        openAndClose.open(changHong);
        // 通过构造器
//        OpenAndClose openAndClose = new OpenAndClose(changHong);
//        openAndClose.open();
        // 通过set
        OpenAndClose openAndClose = new OpenAndClose();
        openAndClose.setTv(changHong);
        openAndClose.open();

    }

}

// 方式1： 通过接口传递实现依赖
// 开关的接口
//interface IOpenAndClose {
//    public void open(ITV tv); //抽象方法,接收接口
//}
//
//interface ITV { //ITV接口
//    public void play();
//}
//
class ChangHong implements ITV {

    @Override
    public void play() {
        System.out.println("长虹电视机,打开");
    }
}
//// 实现接口
//class OpenAndClose implements IOpenAndClose {
//    public void open(ITV tv) {
//        tv.play();
//    }
//}

// 方式2: 通过构造方法依赖传递
/*interface IOpenAndClose {
    public void open(); //抽象方法
}

interface ITV { //ITV接口
    public void play();
}

class OpenAndClose implements IOpenAndClose {
    public ITV tv;

    public OpenAndClose(ITV tv) {
        this.tv = tv;
    }

    public void open() {
        this.tv.play();
    }
 }*/

    // 方式3 , 通过setter方法传递
    interface IOpenAndClose {
        public void open(); // 抽象方法

        public void setTv(ITV tv);
    }

    interface ITV { // ITV接口
        public void play();
    }

    class OpenAndClose implements IOpenAndClose {
        private ITV tv;

        public void setTv(ITV tv) {
            this.tv = tv;
        }

        public void open() {
            this.tv.play();
        }
    }
```

注意事项 

低层模块尽量都要有抽象类或接口，或者两者都有，程先稳定性更好

变量的**声明类型尽量是抽象类或接口**，这样我们变量引用和实际对象间，就存在一个**缓冲层**，利于程先扩展和优化

继承时遵守里氏替换原则



### 2.6 里氏替换原则

#### 	2.6.1  oo中的继承性思考和说明

1、继承包含这样一层含义，父类中凡是已经实现好的方法，实际上时在设定规划和契约，虽然他不强制要求所有的子类必须遵守这些契约，但是如果子类对这些已经实现的方法任意修改，就会对整个继承体系造成破坏



2、**继承在程序设计带来便利的同时，也带来了弊端**，比如使用继承会给程序带来**侵入性**，程序的可移植性降低，增加对象间的耦合性，如果一个类被其他类所继承，则当这个类需要修改时，必须考虑到所有的子类，并且父类修改后，所有涉及到子类的功能都有可能产生故障

3、问题提出，在编程中，如何正确使用继承=》里氏替换原则



基本介绍

1. 里氏替换原则 在 19988年 由麻省理工学院的一位姓里的女士提出的，
2. 如果对每个类型为T1的对象o1，都有类型为T2的对象 o2,时的以 T1定义的程序 P在所有对象o1都代换成o2时，程序P的行为没有发生变化，那么类型T2是类型T1的子类型，**换句话说，所有引用基类的地方必须能透明的使用其子类的对象**
3. 在使用继承时，遵循里氏替换原则，在**子类中尽量不要重写父类方法**
4. 里氏替换原则告诉我们，继承实际上让两个类耦合性增强了，在适当情况下，可以通过聚合，组合，依赖来解决问题



#### 2.6.3 一个程序引出的问题和思考

```java
package com.atguigu.principle.liskow;

/**
 * @author gcq
 * @Create 2020-09-28
 */
public class Liskov {

    public static void main(String[] args) {
        A a = new A();
        System.out.println("11-3=" + a.func1(11,3));
        System.out.println("1-8=" +a.func1(1,8));

        System.out.println("---------------");
        B b = new B();
        System.out.println("11-3=" + b.func1(11,3));
        System.out.println("1-8=" + b.func1(1,8));
        System.out.println("11+3+9=" + b.func2(11,3));
    }
}

class A {
    // 返回连个数的差
    public int func1(int num1,int num2) {
        return num1 - num2;
    }
}
// B 继承了 A
// 增加一个新功能，完成两个数相加，然后和9求和
class B extends A {
    // 这里重写了A类的方法，可能时无意识
    @Override
    public int func1(int a, int b) {
        return a + b;
    }
    public int func2(int a, int b) {
        return func1(a,b) + 9;
    }

}
```

#### 2.6.4 解决方法

1. 我们发现原来运行正常的相减功能发生了错误，原因就是类B无意中重写了父类的方法，造成原有功能出现错误，在实际编程中，我们常常会通过重写父类的方法完成新的功能，这样写虽然简单，但整个继承体系的复用性比较差，特别是运行多态比较频繁的时候

2. 通用的做法是：原来的父类和子类都继承了一个更通俗的基类，原有的继承关系去掉，采用依赖、聚合、组合等关系替代

3. 解决方案

   ![](/Snipaste_2020-09-28_15-29-14.png)

代码实现

```java
package com.atguigu.principle.liskow.improve;

/**
 * @author gcq
 * @Create 2020-09-28
 */
public class Liskov {

    public static void main(String[] args) {
        A a = new A();
        System.out.println("11-3=" + a.func1(11,3));
        System.out.println("1-8=" +a.func1(1,8));

        System.out.println("---------------");
        B b = new B();
        System.out.println("11-3=" + b.func1(11,3));
        System.out.println("1-8=" + b.func1(1,8));
        System.out.println("11+3+9=" + b.func2(11,3));


        System.out.println("11+3+9=" + b.func3(11,3));
    }
}

// 创建一个更加基础的基类
class Base {
    // 把更加基础的方法和成员写到Base类

}
class A extends Base{
    // 返回连个数的差
    public int func1(int num1,int num2) {
        return num1 - num2;
    }
}

// B 继承了 A
// 增加一个新功能，完成两个数相加，然后和9求和
class B extends Base {
    // 如果B需要使用A类的方法，使用组合关系
    private A a = new A();
    // 这里重写了A类的方法，可能时无意识
    public int func1(int a, int b) {
        return a + b;
    }
    public int func2(int a, int b) {
        return func1(a,b) + 9;
    }
    public int func3(int a, int b) {
        return this.a.func1(a,b);
    }
}
```

 

### 2.7 开闭原则

#### 	2.7.1基本介绍

1、开闭原则(Open Closed Principle)是编程中**最基础、最重要**的设计原则

2、一个软件实体如类，模块和函数应该对扩展开放**(对提供方)，对修改关闭(对使用**
**方。**用抽象构建框架，用实现扩展细节。

3、当软件需要变化时，尽量**通过扩展软件实体**的行为来实现变化，而不是**通过修改**已
有的代码来实现变化。

4、编程中遵循其它原则，以及使用设计模式的目的就是遵循开闭原则。

#### 	2.7.2 看下面一段代码

看一个画图形的功能

![](/Snipaste_2020-09-29_11-56-10.png)



代码演示

```java
package com.atguigu.principle.ocp;


/**
 * @author gcq
 * @Create 2020-09-28
 */
public class Ocp {

    public static void main(String[] args) {
        // 使用看看存在的问题
        GraphicEditor graphicEditor = new GraphicEditor();
        graphicEditor.drawShape(new Rectangle());
        graphicEditor.drawShape(new Circle());
        graphicEditor.drawShape(new Triangle());
        graphicEditor.drawShape(new A());
    }
}
// 这是一个用于绘图的类
class GraphicEditor {
    public void drawShape(Shape s) {
        if (s.m_type == 1) {
            drawRectangle(s);
        } else  if (s.m_type == 2) {
            drawCircle(s);
        } else if (s.m_type == 3) {
            drawTriangle(s);
        } else if (s.m_type == 4) {

        }
    }
    public void drawRectangle(Shape r) {
        System.out.println("绘制矩形");
    }
    public void drawCircle(Shape r) {
        System.out.println("绘制圆形");
    }
    public void drawTriangle(Shape r) {
        System.out.println("绘制三角形");
    }
    public void drawA(Shape r) {
        System.out.println("绘制A");
    }
}
// Shape类 基类
class Shape {
    int m_type;
}
class Rectangle extends Shape {
    Rectangle() {
        super.m_type = 1;
    }
}
class Circle extends Shape {
    Circle() {
        super.m_type = 2;
    }
}
//新增画三角形
class Triangle extends Shape {
    Triangle() {
        super.m_type = 3;
    }
}
class A extends Shape {
    A() {
        super.m_type = 4;
    }
}
```



####  2.7.3 方式1 的优缺点

1. 优点是比较好理解，简单易操作
2. 缺点是违反了设计模式的ocp原则，即对扩展开放(提供方)，对修改关闭(使用方)。即当我们给类增加新功能的时候，尽量不修改代码，或者尽可能少修改代码.
3. 比如我们这时要新增加一个图形种类三角形，我们需要做如下修改，修改的地方较多
4. 代码演示



方式 1改进思路分析

#### 	2.7.4 改进思路分析

思路:把创建Shape类做成抽象类，并提供- -个抽象的draw方法，让子类去实现即可，这样我们有新的图形种类时，只需要让新的图形类继承Shape,并实现draw方法即可，使用方的代码就不需要修>满足了 开闭原则

改进后代码

```java
package com.atguigu.principle.ocp.improve;


/**
 * @author gcq
 * @Create 2020-09-28
 */
public class Ocp {

    public static void main(String[] args) {
        // 使用看看存在的问题
        GraphicEditor graphicEditor = new GraphicEditor();
        graphicEditor.drawShape(new Rectangle());
        graphicEditor.drawShape(new Circle());
        graphicEditor.drawShape(new Triangle());
        graphicEditor.drawShape(new OtherGraphic());
    }
}

// 这是一个用于绘图的类
class GraphicEditor {
    // 接收Shape对象，然后根据type，来绘制不同的图形
    public void drawShape(Shape s) {
        s.draw();
    }

}

// Shape类 基类
abstract class Shape {
    int m_type;
    // 抽象方法
    public abstract void draw();
}

class Rectangle extends Shape {
    Rectangle() {
        super.m_type = 1;
    }

    @Override
    public void draw() {
        System.out.println("绘制矩形");
    }
}

class Circle extends Shape {
    Circle() {
        super.m_type = 2;
    }

    @Override
    public void draw() {
        System.out.println("绘制圆形");
    }
}

//新增画三角形
class Triangle extends Shape {
    Triangle() {
        super.m_type = 3;
    }

    @Override
    public void draw() {
        System.out.println("绘制三角形");
    }
}

//新增一个图形
class OtherGraphic extends Shape {
    OtherGraphic() {
        super.m_type = 4;
    }

    @Override
    public void draw() {
        System.out.println("七边形");
    }
}
```



### 2.8 迪米特法则

#### 	2.8.1 基本介绍

1、一个对象应该对其他对象保持最少的了解

2、类与类关系越密切，耦合度越大

3、迪米特法则(Demeter Principle)又叫**最少知道原则**，即一个**类对自己依赖的类知道的越少越好**。也就是说，对于被依赖的类不管多么复杂，都尽量将逻辑封装在类的内部。对外除了提供的public方法，不对外泄露任何信息

4、迪米特法则还有个更简单的定义:只与直接的朋友通信

5、**直接的朋友**: 每个对象都会与其他对象有**耦合关系**，只要两个对象之间有耦合关系，我们就说这两个对象之间是朋友关系。耦合的方式很多，依赖，关联，组合，聚合等。其中，我们称出现**成员变量，方法参数，方法返回值**中的类为直接的朋友，而出现在**局部变量中的类不是直接的朋友**。也就是说，陌生的类最好不要以局部变量的形式出现在类的内部。

#### 	2.8.2 应用实例

1、有一个学校，下属有各个学院和总部，现要求打印出学校总部员工ID和学院员工的id 
2、编程实现上面的功能，看代码演示

3、代码演示

```java
package com.atguigu.principle.demeter;

import java.util.ArrayList;
import java.util.List;

/**
 * @author gcq
 * @Create 2020-09-29
 */
// 客户端
public class Demter {

    public static void main(String[] args) {
        SchoolManager schoolManager = new SchoolManager();
        schoolManager.printAllEmployee(new CollegeManager());
    }
}
// 学校总部员工类
class Employee {
    private String id;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }
}

// 学院员工类
class CollegeEmployee {
    private String id;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }
}
// 学院管理类
class CollegeManager {
    // 返回学院所有员工
    public List<CollegeEmployee> getAllEmployee() {
        List<CollegeEmployee> list = new ArrayList<CollegeEmployee>();
        // 增加了10个员工到list集合
        for(int i = 0; i < 10; i++) {
            CollegeEmployee emp = new CollegeEmployee();
            emp.setId("学院员工id=" + i);
            list.add(emp);
        }
        return list;
    }

}

// 分析 SchoolManager 类的直接朋友类有那些 Employee CollegeManager
// CollegeEmployee 不是直接朋友而是一个陌生类，违反了迪米特法则
// 学校管理类
class SchoolManager {
    // 返回学校总部的员工
    public List<Employee> getAllEmployee() {
        List<Employee> list = new ArrayList<Employee>();
        for(int i = 0; i < 5; i++) {
            Employee emp = new Employee();
            emp.setId("学校员工id=" + i);
            list.add(emp);
        }
        return list;
    }
    // 该方法完成输出学校总部和学院员工的信息id
    void printAllEmployee(CollegeManager sub) {
        // 分析问题
        // 1、这里的CollegeEmployee 不是 SchoolManager 直接朋友
        //  2、CollegeEmployee 是以局部变量的方式出现在 SchoolManager
        // 3、违反了迪米特法则

        // 将输出学院的员工方法，封装到 CollegeManager
        // 获取到学院员工
        List<CollegeEmployee> list1 = sub.getAllEmployee();
        System.out.println("---------------分公司员工----------------");
        for (CollegeEmployee e: list1) {
            System.out.println(e.getId());
        }

        // 获取到学校总部员工
        List<Employee> list2 = this.getAllEmployee();
        System.out.println("---------------学校总部员工---------------");
        for (Employee e : list2) {
            System.out.println(e.getId());
        }
    }
}
```



#### 	2.8.3 应案例改进

1、前面设计的问题在于SchoolManager中，CollegeEmployee 类并不是SchoolManager类的直接朋(分析)

2、按照迪米特法则，应该避免类中出现这样非直接朋友关系的耦合

3、对代码按照迪米特法则进行改进. (看老师演示)

```java
package com.atguigu.principle.demeter.improve;

import java.util.ArrayList;
import java.util.List;

/**
 * @author gcq
 * @Create 2020-09-29
 */
// 客户端
public class Demter {

    public static void main(String[] args) {
        System.out.println("使用迪米特法则改进");
        SchoolManager schoolManager = new SchoolManager();
        schoolManager.printAllEmployee(new CollegeManager());
    }
}

// 学校总部员工类
class Employee {
    private String id;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }
}

// 学院员工类
class CollegeEmployee {
    private String id;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }
}

// 学院管理类
class CollegeManager {
    // 返回学院所有员工
    public List<CollegeEmployee> getAllEmployee() {
        List<CollegeEmployee> list = new ArrayList<CollegeEmployee>();
        // 增加了10个员工到list集合
        for(int i = 0; i < 10; i++) {
            CollegeEmployee emp = new CollegeEmployee();
            emp.setId("学院员工id=" + i);
            list.add(emp);
        }
        return list;
    }
    // 输出学院员工信息
    public void printEmployee() {
        List<CollegeEmployee> list1 = getAllEmployee();
        System.out.println("---------------分公司员工----------------");
        for (CollegeEmployee e: list1) {
            System.out.println(e.getId());
        }
    }
}

// 分析 SchoolManager 类的直接朋友类有那些 Employee CollegeManager
// CollegeEmployee 不是直接朋友而是一个陌生类，违反了迪米特法则
// 学校管理类
class SchoolManager {
    // 返回学校总部的员工
    public List<Employee> getAllEmployee() {
        List<Employee> list = new ArrayList<Employee>();
        for(int i = 0; i < 5; i++) {
            Employee emp = new Employee();
            emp.setId("学校员工id=" + i);
            list.add(emp);
        }
        return list;
    }
    // 该方法完成输出学校总部和学院员工的信息id
    void printAllEmployee(CollegeManager sub) {
        // 分析问题
        // 1、这里的CollegeEmployee 不是 SchoolManager 直接朋友
        //  2、CollegeEmployee 是以局部变量的方式出现在 SchoolManager
        // 3、违反了迪米特法则

        // 将输出学院的员工方法，封装到 CollegeManager
        // 获取到学院员工
        sub.printEmployee();

        // 获取到学校总部员工
        List<Employee> list2 = this.getAllEmployee();
        System.out.println("---------------学校总部员工---------------");
        for (Employee e : list2) {
            System.out.println(e.getId());
        }
    }
}
```



#### 	2.8.4 迪米特法则注意事项和细节

1、迪米特法则核心是降低类之间的耦合

2、但是注意：由于每个类都减少了不必要的依赖因此迪米特法则只是要求降低类间(对象间)耦合关系，并不是要求完全没有依赖关系



### 2.9 合成复用原则(Composite Reuse Principle)



#### 	2.9.1 基本介绍

原则尽量使用合成/聚合的方式，而不是使用继承

![](/Snipaste_2020-09-29_12-15-38.png)

#### 	

#### 	2.9.10 设计原则核心思想

1、找出应用中可能需要变化之处，把他们独立出来，不要和那些不需要变化的代码混在一起

2、针对接口编程，而不是针对实现编程

3、为了交互对象之间的松耦合设计而努力



# 第 3 章 UML 类图



### 	3.1 UML 基本介绍

1、UML--Unified modeling language UML(统一建模语言)，是一种用于软件系统分析和设计的语言工具，它用于帮助软件开发人员进行思考和记录思路的结果

2、 UML本身是一套符号的规定，就像数学符号和化学符号一样，这些符号用于描述软件模型中的各个元素和他们之间的关系，比如类、接口、实现、泛化、依赖、组合、聚合等，如右图:

![](/Snipaste_2020-09-29_12-56-43.png)

3、使用UML来建模，常用的工具有RationalRose ,也可以使用一些插件来建模

![](/Snipaste_2020-09-29_12-53-30.png)



#### 	3.2 UML 图

画UML图与写文章差不多，都是把自己的思想描述给别人看，关键在于思路和条理，UML图分类:

1、用例图(use case)
2、静态结构图:类图、对象图、包图、组件图、部署图
3、动态行为图:交互图(时序图与协作图)、状态图、活动图

说明：

1、类图是描述类与类之间的关系的，是UML图中最核心的
2、在讲解设计模式时，我们必然会使用类图，为了让学员们能够把设计模式学到位，需要先给大家讲解类图
3、温馨提示:如果已经掌握UML类图的学员，可以直接听设计模式的章节



#### 	3.3 UML 类图

1、用于描述系统中的**类(对象)本身的组成和类(对象)之间的各种静态关系。**
2、类之间的关系:依赖、泛化(继承)、实现、关联、聚合与组合。
3、类图简单举例

```java
public class Person { // 代码形式 ---> 类图

    private Integer id;
    private String name;


    public void setName(String name) {
        this.name = name;
    }
    public String getName() {
        return name;
    }

}
```

![](/Snipaste_2020-09-29_13-21-21.png)



#### 	3.4 类图--依赖关系(Dependence)

只要是在类中用到了对方，那么他们之间就存在依赖关系，如果没有对方，连编译都过不了

```java
public class PersonServiceBean{
    private PersonDao personDao;
    public void save(Person person){}
    public IDCard getIDCard(Integer personid) {}
    public void modify(){
        Department department = new Department();
    }
}
public class PersonDao{}
public class IDCard{}
public Person{}
public class Department{}
```

![](/Snipaste_2020-09-29_16-16-53.png)

**小结**

1、类中用到了对方

2、如果是**类的成员属性**

3、如果是**方法的返回类型**

4、是方法的**接收参数类型**

5、**方法中使用到**



#### 	3.5 类图--泛化关系(generalization)

泛化关系实**际上就是继承关系**，他是依赖关系的特例

```java
public abstract class DaoSupport {
    public void save(Object entity) {
        
    }
    public void deelte(Object id) {
        
    }
}
public class PersonServiceBean extends DaoSupport {
    
}
```

对应的类图

![](/Snipaste_2020-09-29_16-27-22.png)

**小结：**

1、泛型关系实际**上就是继承关系**

2、如果 A 类继承了 B 类，我们就说 A 和 B 存在泛化关系



#### 	3.6 类图--实现关系(Implementation)

实现关系实际上就是**A类实现B接口**，他是依赖关系的特例

```java
public interface PersonService{
    public void deelte(Integer id);
}
public class PersonServiceBean implements PersonService{
    public void deletge(Integer id){}
}
```

类图

![](/Snipaste_2020-09-29_16-29-36.png)



#### 	3.7 类图--关联关系(Association)

![](/Snipaste_2020-09-29_16-32-19.png)



#### 3.8 类图--聚合关系(Aggregation)

 	**3.8.1基本介绍**	 

​		聚合关系(Aggregation) 表示的是**整体和部分的关系**，**整体与部分可以分开**，聚合关系是**关联关系的特例**，所以他具有关联的**导航性与多重性**

​		如：一台电脑由键盘(keyboard)，显示器(monitor)，鼠标等组成电脑的各个配件可以从电脑上分离出来的，使用带空心菱形的实现来表示

![](/Snipaste_2020-09-29_16-38-16.png)

​	**3.8.1 应用实例**

![](/Snipaste_2020-09-29_16-38-05.png)



#### 3.9 类图--组合关系(composition)

​		**3.9.1 基本介绍**

组合关系：也是整体与部分的关系，但是**整体与部分不可以分开**，

在看一个案例：在程序中我们定义实体，Person与IDCard 、Head,那么 Head 和 Person 就是 组合 IDCard和 Person就是聚合



但是如果在程序中 Person实体中定义了对IDCard进行**级联删除**，即删除 Person时连同IDCard 一起删除，那么 IDcard 和 Person就是组合



 	**3.9.2 应用案例**

```java
public class Person{
    private IDCard card;
    private Head head = new Head();
}
public class IDCard{}
public class Head{}
```

对应类图

![](/Snipaste_2020-09-29_16-46-06.png)

案例二

```java
package com.atguigu.aggregation;

/**
 * @author gcq
 * @Create 2020-09-29
 */
public class Computer {

    // 鼠标可以和computer不能分离
    private Moniter moniter = new Mouse();
    // 显示器可以和computer不能分离
    private Mouse mouse = new Mouse();

    public void setMoniter(Moniter moniter) {
        this.moniter = moniter;
    }

    public void setMouse(Mouse mouse) {
        this.mouse = mouse;
    }
}
```

```java
/**
 * @author gcq
 * @Create 2020-09-29
 */
public class Moniter {

}
```

```java
/**
 * @author gcq
 * @Create 2020-09-29
 */
public class Mouse {

}
```

![](/Snipaste_2020-09-29_16-48-34.png)





# 第 4 章 设计模式概述 

### 		4.1 掌握设计模式的层次



1、第一层：刚开始学编程不久，听说过什么是设计模式

2、第二层，有很长时间的编程经验，自己写了很多代码，其中用到了设计模式，但是自己却还不知道

3、第三层：学习过了设计模式，发现自己已经在使用了，并且发现了一些新的模式挺好用的

4、第四层：阅读了很多别人的源码和框架，在其中看到别人的设计模式，并且能够领会设计模式的精妙和带来的好处

5、第五层：代码写着写着，自己都没有意识到使用了设计模式，并且熟练的写了出来



### 	4.2 设计模式介绍



1、设计模式是程序在面对同类得软件工程师设计问题所总结出来有用的经验，**模式不是代码，而是某类问题的通用解决方案**。设计模式(Design patten)代表了最佳实践，这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的

2、设计模式本质是提高 **软件维护性，通用性和扩展性，并降低了软件的复杂度**

3、<<**设计模式**>>是经典的书，作者是Erich Gamma ,Richard Helm Ralph Johnson 和Vissides Design(俗称四人组GOF)

4、设计模式并不局限某种语言，java,php,c++ 都有设计模式



### 	4.3 设计模式类型



设计模式分为**三种类型**，共23种

1、**创建型模式**，单例模式，抽象工厂模式，原型模式，建造者模式，工厂模式

2、**结构型**末是：适配器模式，桥接模式，装饰模式，装饰模式，组合模式，外观模式，享元模式，代理模式

3、行为型模式：模板方法模式，命令模式，访问者模式，迭代器模式，观察者模式，中介者模式，备忘录模式，解释器模式(Interpreter模式)、状态模式，策略模式，职责链模式(责任链模式)

注意：不同的书籍对分类和名称略有差别





# 第 5 章 单例设计模式

### 	5.1 单例设计模式介绍

所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类只能存在一个对象实例

并且该类只提供一个取得其对象实例的方法(静态方法)



比如 Hibernate 的 SessionFactory 它充当数据存储源的代理，并创建 Session 对象 SessionFactory 并不是轻量级的，一般情况下，一个项目通常只需要一个 SessionFactory 就够，这是就会使用到单例设计模式



### 	5.2 单例设计模式八种方式

单例设计模式有八种方式：



1、**饿汉式(静态常量)**

2、**饿汉式(静态代码块)**

3、懒汉式(线程不安全)

4、懒汉式(线程安全，同步代码)

5、懒汉式(线程安全，同步代码块)

6、**双重检查**

7、**静态内部类**

8、**枚举**



### 	5.3 饿汉式(静态常量)

饿汉式(静态常量)应用实例

步骤如下：

构造器私有化

类的内部创建对象

向外暴露一个静态的公共方法，getInstance

代码实现:

```java
package com.atguigu.principle.singleton.type1;

/**
 * @author gcq
 * @Create 2020-09-30
 */
public class SingletonTest01 {

    public static void main(String[] args) {
        Singleton instance = Singleton.getInstance();
        Singleton instance1 = Singleton.getInstance();
        System.out.println(instance == instance1);
        System.out.println(instance.hashCode() + "--" +instance1.hashCode());
    }

}
// 饿汉式(静态变量)
class Singleton {
    // 构造器私有化 外部不能new
    private Singleton() {}

    // 2、本类内部创建对象实例
    private final static Singleton instance = new Singleton();

    // 3、提供一个公有静态方法
    public static Singleton getInstance() {
        return instance;
    }

}
```



**优缺点说明**

1. 优点:这种写法比较简单，就是在类装载的时候就完成实例化。避免了线程同步问题
2. 缺点:在类装载的时候就完成实例化，没有达到Lazy Loading的效果。如果从始至终从未使用过这个实例，则会造成内存的浪费 
3. 这种方式基于classloder机制避免了多线程的同步问题，不过，instance在类装载时就实例化，在单例模式中大多数都是调用getlnstance方法，但 是导致类装载的原因有很多种，因此不能确定有其他的方式(或者其他的静态方法)导致类装载，这时候初始化instance就没有达到lazy loading的效果
4. 结论:这种单例模式可用，可能造成内存浪费



### 	5.4 饿汉式(静态代码块)

代码演示

```java
package com.atguigu.principle.singleton.type2;

/**
 * @author gcq
 * @Create 2020-09-30
 */
public class SingletonTest02 {

    public static void main(String[] args) {
        Singleton instance = Singleton.getInstance();
        Singleton instance1 = Singleton.getInstance();
        System.out.println(instance == instance1);
        System.out.println(instance.hashCode() + "--" +instance1.hashCode());
    }

}

// 饿汉式(静态变量)
class Singleton {
    // 构造器私有化 外部不能new
    private Singleton() {}

    // 2、本类内部创建对象实例
    private  static Singleton instance;

    // 静态代码块中创建单例对象
    static {
        instance = new Singleton();
    }

    // 3、提供一个公有静态方法
    public static Singleton getInstance() {
        return instance;
    }
}
```

**优缺点说明**

1. 这种方式和上面的方式其实类似，只不过将类实例化的过程放在了静态代码块中，也是在类装载的时候，就执行静态代码块中的代码，初始化类的实例。优缺点和上面是一样的。
2. 结论:这种单例模式可用，但是可能造成内存浪费



### 	5.5 懒汉式(线程不安全)

代码演示

```java
package com.atguigu.principle.singleton.type3;

/**
 * @author gcq
 * @Create 2020-09-30
 */
public class SingletonTest03 {
    public static void main(String[] args) {
        Singleton instance = Singleton.getInstance();
        Singleton instance1 = Singleton.getInstance();
        System.out.println(instance == instance1);
    }
}
class Singleton {
    private static Singleton instance;

    private Singleton() {};

    // 提供一个静态的公有方法，当使用到该方法时，才去创建
    public static Singleton getInstance() {
        if (instance == null ){
            instance = new Singleton();
        }
        return instance;
    }

}
```

优缺点说明：

1. 起到了**Lazy Loading**的效果，但是只能在单线程下使用。
2. 如果在多线程下，一个线程进入了if (singleton = nul1)判断语句块，还未来得及往下执行，另一个线程也通过了这
   个判断语句，这时便**会产生多个实例**。所以在多线程环境下不可使用这种方式
3. 结论:在实际开发中，**不要使用这种方式.**



### 	5.6 懒汉式(线程安全，同步方法)

代码演示：

```java
package com.atguigu.principle.singleton.type4;

/**
 * @author gcq
 * @Create 2020-09-30
 */
public class SingletonTest04 {
    public static void main(String[] args) {
        Singleton instance = Singleton.getInstance();
        Singleton instance1 = Singleton.getInstance();
        System.out.println(instance == instance1);
    }
}

// 懒汉式 线程安全 同步方法
class Singleton {
    private static Singleton instance;

    private Singleton() {};

    // 提供一个静态的公有方法，当使用到该方法时，才去创建
    public static synchronized Singleton getInstance() {
        if (instance == null ){
            instance = new Singleton();
        }
        return instance;
    }

}
```



**优缺点说明**

1. 解决了**线程安全问题**
2. 效率太低了，每个线程在想获得类的实例时候，执行getInstance()方法都要进行同步。而其实这个方法只执行一次实例化代码就够了，后面的想获得该类实例，直接retum就行了。**方法进行同步效率太低**
3. 结论:在实际开发中，**不推荐**使用这种方式



### 	5.7 懒汉式(线程安全，同步代码块)

![](/Snipaste_2020-09-30_12-05-53.png)

不推荐使用



### 	5.8 双重检查

代码演示：

```java
package com.atguigu.principle.singleton.type6;

/**
 * @author gcq
 * @Create 2020-09-30
 */
public class SingletonTest06 {
    public static void main(String[] args) {
        System.out.println("双重检查");
        Singleton instance = Singleton.getInstance();
        Singleton instance1 = Singleton.getInstance();
        System.out.println(instance == instance1);
    }
}

// 懒汉式 线程安全 同步方法
class Singleton {
    private static volatile Singleton instance;

    private Singleton() {};

    // 提供一个静态的公有方法，加入双重检查，解决线程安全问题，同时解决懒加载问题
    public static Singleton getInstance() {
        if (instance == null ){
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }

}
```

**优缺点说明**:

1. Double-Check概念是多线程开发中常使用到的，如代码中所示，我们进行了两次if (singleton == nul)检查，这样就可以保证线程安全了。
2. 这样，实例化代码只用执行一次,后面再次访问时，判断if (singleton == null),直接return实例化对象，也避免的反复进行方法同步.
3. 线程安全;**延迟加载,效率较高**
4. 结论:在实际开发中，**推荐使用这种单例设计模式**



### 	5.9 静态内部类

代码演示：

```java
package com.atguigu.principle.singleton.type7;

/**
 * @author gcq
 * @Create 2020-09-30
 */
public class SingletonTest07 {
    public static void main(String[] args) {
        System.out.println("静态内部类");
        Singleton instance = Singleton.getInstance();
        Singleton instance1 = Singleton.getInstance();
        System.out.println(instance == instance1);
    }
}

// 静态内部类
class Singleton {
    private static volatile Singleton instance;

    private Singleton() {};
    // 写一个静态内部类 该类中有一个静态属性 Singleton
    private static class SingletonInstance {
        private static final Singleton INSTACE = new Singleton();
    }

    // 提供一个静态的公有方法，直接返回SingletonInstance.INSTANCE
    public static Singleton getInstance() {
        return SingletonInstance.INSTACE;
    }

}
```

**优缺点说明**：

1. 这种方式采用了类装载的机制来保证初始化实例时只有一个线程。
2. 静态内部类方式在Singleton类被装载时并不会立即实例化，而是在需要实例化时，调用getInstance方法， 才会装载SingletonInstance类，从而完成Singleton的实例化。
3. 类的静态属性只会在第一次加载类的时候初始化，所以在这里，JVM帮助我们保证了线程的安全性，在类进行初始化时，别的线程是无法进入的。
4. 优点:避免了线程不安全，利用静态内部类特点实现延迟加载，效率高
5. 结论:推荐使用.



### 	5.10 枚举

代码演示:

```java
package com.atguigu.principle.singleton.type8;

/**
 * @author gcq
 * @Create 2020-09-30
 */
public class SingletonTest08 {
    public static void main(String[] args) {
        Singleton s = Singleton.INSTANCE;
        Singleton s2 = Singleton.INSTANCE;
        System.out.println(s == s2);
        s.sayOk();
    }
}
enum Singleton {
    INSTANCE;
    public void sayOk() {
        System.out.println("ok~");
    }
}
```

**优缺点说明：**

1. 这借助JDK1.5中添加的枚举来实现单例模式。不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象。
2. 这种方式是Effective Java作者Josh Bloch提倡的方式
3. 结论:推荐使用



### 	5.11 单例模式在 JDK 应用中源码分析

1. 我们JDK中，java.lang.Runtime就是经典的单例模式
2. 代码分析 + Debug源码+代码说明

![](/Snipaste_2020-09-30_12-14-23.png)



### 	5.12 单例模式注意事项和细节说明

1. 单例模式保证了系统内存中该类只存在一个对象，节省了系统资源，对于一些需要频繁创建销毁的对象，使用单例模式可以提高系统性能
2. 当想实例化一个单例类的时候，必须要记住使用相应的获取对象的方法，而不是使用new
3. 单例模式**使用的场景**:需要**频繁的进行创建和销毁的对象**、创建对象时耗时过多或耗费资源过多(即:**重量级对象**)，但又经常用到的对象、**工具类对象**、频繁访问数据库或文件的对象(比如**数据源、session**工厂等)





# 第 6 章 工厂模式

### 	6.1 简单工厂模式

### 		6.1.1 看一个具体的需求

看一个披萨项目，要便于披萨种类的扩展，要便于维护

1、披萨的种类很多，比如GreekPizz，CheesePiz 等

2、披萨的制作有 prepare准备，bake烤，cut 切 box 包装

3、完成披萨店订购功能



### 6.1.2 使用传统方式来完成

1、思路分析(类图)

![](/Snipaste_2020-09-30_22-00-03.png)

编写 OrderPizza.java 去订购需要的各种 pizza

2、看老师代码演示

```java
/* public OrderPizza() {
     Pizza pizza = null;
     String orderType; //披萨订购的类型
     do{
         orderType = gettype();
         if (orderType.equals("greek")) {
             pizza = new GreekPizza();
             pizza.setName(" 希腊披萨 ");
         } else if (orderType.equals("cheese")) {
             pizza = new CheesePizza();
             pizza.setName(" 奶酪披萨 ");
         } else if(orderType.equals("pepper")) {
             pizza = new PepperPizza();
             pizza.setName("胡椒披萨");
         } else {
             break;
         }
         // 输出pizza制作过程
         pizza.prepare();
         pizza.bake();
         pizza.cut();
         pizza.box();
     }while (true);
 }*/
```



### 6.1.3 传统方式的优缺点

1. 优点是比较好理解，简单易操作
2. 缺点是违反了设计模式的**ocp原则**，即对扩展开放，对修改关闭。即当我们给类增加新功能的时候，尽量不修改代码，或者尽可能少修改代码.
3. **比如我们这时要新增加一个**Pizza的种类(Pepper披萨)，我们需要做如下修改.

如果我们增加一个Pizza类，只要是订购Pizza的代码都需要修改

![](/Snipaste_2020-09-30_22-05-25.png)

 4. 改进思路分析

    分析:修改代码可以接受,但是如果我们在其它的地方也有创建Pizza的代码，就意味着，也需要修改,而创建Pizza的代码，往往有多处。.
    思路:**把创建Pizza 对象封装到一个类中,这样我们有新的Pizza种类时，只需要修改该类就可**,其它有创建到Pizza对象的代码就不需要修改了.->**简单工厂模式**



### 6.1.4  简单工厂模式基本介绍

1. 简单工厂模式是属于**创建型模式**，是工厂模式的一种。简单工厂模式是由一-个工厂对象决定创建出哪一种产品类的实例。简单工厂模式是**工厂模式家族中最简单实用的模式.**
2. 简单工厂模式:定义了一个创建对象的类，由这个类来**封装实例化对象的行为**(代码)
3. 在软件开发中，当我们会用到大量的创建某种、某类或者某批对象时，就会使用到工厂模式.



### 6.1.5 使用简单工厂模式

1. 简单工厂模式设计方案，定义一个实例化 Pizza对象的类，封装创建对象的代码

   ![](/Snipaste_2020-09-30_22-09-08.png)

2. 看代码示例：

   ```java
   package com.atguigu.factory.simplefactory.pizzastore.order;
   
   import com.atguigu.factory.simplefactory.pizzastore.pizza.CheesePizza;
   import com.atguigu.factory.simplefactory.pizzastore.pizza.GreekPizza;
   import com.atguigu.factory.simplefactory.pizzastore.pizza.PepperPizza;
   import com.atguigu.factory.simplefactory.pizzastore.pizza.Pizza;
   
   /**
    * 简单工厂
    *
    * @author gcq
    * @Create 2020-09-30
    */
   public class SimpleFactory {
   
       /**
        * 根据orderType 返回对应的Pizza 对象
        * @param orderType
        * @return
        */
       public static Pizza createPizza(String orderType) {
   
           Pizza pizza = null;
           System.out.println("使用简单工厂模式");
           if (orderType.equals("greek")) {
               pizza = new GreekPizza();
               pizza.setName(" 希腊披萨 ");
           } else if (orderType.equals("cheese")) {
               pizza = new CheesePizza();
               pizza.setName(" 奶酪披萨 ");
           } else if(orderType.equals("pepper")) {
               pizza = new PepperPizza();
               pizza.setName("胡椒披萨");
           }
           return pizza;
       }
       // 简单工厂模式也叫静态工厂模式
   
   }
   ```

```java
package com.atguigu.factory.simplefactory.pizzastore.order;

import com.atguigu.factory.simplefactory.pizzastore.pizza.CheesePizza;
import com.atguigu.factory.simplefactory.pizzastore.pizza.GreekPizza;
import com.atguigu.factory.simplefactory.pizzastore.pizza.PepperPizza;
import com.atguigu.factory.simplefactory.pizzastore.pizza.Pizza;

import java.io.BufferedReader;
import java.io.InputStreamReader;

/**
 * @author gcq
 * @Create 2020-09-30
 */
public class OrderPizza {

   /* public OrderPizza() {
        Pizza pizza = null;
        String orderType; //披萨订购的类型
        do{
            orderType = gettype();
            if (orderType.equals("greek")) {
                pizza = new GreekPizza();
                pizza.setName(" 希腊披萨 ");
            } else if (orderType.equals("cheese")) {
                pizza = new CheesePizza();
                pizza.setName(" 奶酪披萨 ");
            } else if(orderType.equals("pepper")) {
                pizza = new PepperPizza();
                pizza.setName("胡椒披萨");
            } else {
                break;
            }
            // 输出pizza制作过程
            pizza.prepare();
            pizza.bake();
            pizza.cut();
            pizza.box();
        }while (true);
    }*/

    //定义一个简单工厂对象
    SimpleFactory simpleFactory;

    Pizza pizza = null;

    //构造器
    public OrderPizza(SimpleFactory simpleFactory) {
        setFactory(simpleFactory);
    }
    public void setFactory(SimpleFactory simpleFactory) {
        // 用户输入的
        String orderType = "";

        // 设置简单工厂对象
        this.simpleFactory = simpleFactory;
        do{
            orderType = gettype();
            pizza = this.simpleFactory.createPizza(orderType);

            if (pizza != null) {
                pizza.prepare();
                pizza.bake();
                pizza.cut();
                pizza.box();
            } else {
                System.out.println(" 订购披萨失败");
                break;
            }
        }while(true);
    }

    //写一个方法 可以获取客户希望订购的披萨类
    private String gettype() {
        try {
            BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
            System.out.println("input pizza type:");
            String str = strin.readLine();
            return str;
        } catch (Exception e) {
            e.printStackTrace();
            return "";
        }
    }
}
```



### 6.2 工厂方法模式

### 6.2.1 看一个新的需求

​	披萨项目新的需求:客户在点披萨时，可以点**不同口味的披萨**，比如北京的奶酪pizza、北京的胡椒pizza或者是伦敦的奶酪pizza、伦敦的胡椒pizza。

### 6.2.1 思路1

​	使用**简单工厂模式**，创建**不同的简单工厂类**，比如BJPizzaSimpleFactory、LDPizzaSimpleFactory等等从当前这个案例来说，也是可以的，但是考虑到项目的规模，以及软件的可维护性、可扩展性并不是特别好

### 6.2.2 思路2

​	使用工厂方法模式

### 6.2.4 工厂方法模式介绍

1、工厂方法模式设计方案:将披萨项目的实例化功能抽象成抽象方法，在不同的口味点餐子类中具体实现。

2、工厂方法模式**:定义了一个创建对象的抽象方法**，由**子类决定要实例化的类。**工厂方法模式将**对象的实例化推迟到子类。**

### 6.2.5 工厂方法模式应用案例

1、披萨项目新的需求:客户在点披萨时，可以点不同口味的披萨，比如北京的奶酪pizza、北京的胡椒pizza或者是伦敦的奶酪pizza、伦敦的胡椒pizza

2、思路分析

![](/Snipaste_2020-09-30_22-15-33.png)



3、代码实现

```java
package com.atguigu.factory.factorymethod.pizzastore.order;


import com.atguigu.factory.factorymethod.pizzastore.pizza.Pizza;

import java.io.BufferedReader;
import java.io.InputStreamReader;

/**
 * @author gcq
 * @Create 2020-09-30
 */
public abstract class OrderPizza {

    // 定义一个抽象方法 createPizza 让各个工厂子类自己实现
    abstract Pizza createPizza(String orderType);

    public OrderPizza() {
        Pizza pizza = null;
        String orderType; //披萨订购的类型
        do{
            orderType = gettype();
            // 抽象方法 由工厂子类完成
          pizza = createPizza(orderType);
            // 输出pizza制作过程
            pizza.prepare();
            pizza.bake();
            pizza.cut();
            pizza.box();
        }while (true);
    }



    //写一个方法 可以获取客户希望订购的披萨类
    private String gettype() {
        try {
            BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
            System.out.println("input pizza type:");
            String str = strin.readLine();
            return str;
        } catch (Exception e) {
            e.printStackTrace();
            return "";
        }
    }
}
```

子类实现

```java
package com.atguigu.factory.factorymethod.pizzastore.order;

import com.atguigu.factory.factorymethod.pizzastore.pizza.BJCheesePizza;
import com.atguigu.factory.factorymethod.pizzastore.pizza.BJPepperPizza;
import com.atguigu.factory.factorymethod.pizzastore.pizza.Pizza;


/**
 * @author gcq
 * @Create 2020-09-30
 */
public class BJOrderPizza extends OrderPizza{

    @Override
    Pizza createPizza(String orderType) {
        Pizza pizza = null;
        if (orderType.equals("cheese")) {
            pizza = new BJCheesePizza();
        } else if (orderType.equals("pepper")) {
            pizza = new BJPepperPizza();
        }
        return pizza;
    }
}
```



```java
package com.atguigu.factory.factorymethod.pizzastore.order;

import com.atguigu.factory.factorymethod.pizzastore.pizza.*;
import com.sun.org.apache.bcel.internal.generic.LDC;


/**
 * @author gcq
 * @Create 2020-09-30
 */
public class LDOrderPizza extends OrderPizza{

    @Override
    Pizza createPizza(String orderType) {
        Pizza pizza = null;
        if (orderType.equals("cheese")) {
            pizza = new LDCheesePizza();
        } else if (orderType.equals("pepper")) {
            pizza = new LDPepperPizzza();
        }
        return pizza;
    }
}
```



### 6.3 抽象工厂模式

### 	6.3.1基本介绍

1. 抽象工厂模式:定义了一个**interface用于创建相关或有依赖关系的对象簇**，而无需指明具体的类
2. 抽象工厂模式可以将简单工厂模式和工厂方法模式进行整合。
3. 从设计层面看，抽象工厂模式就是对简单工厂模式的改进(或者称为进一 步的抽象)。
4. 将工厂抽象成**两层**，**AbsFactory(抽象工厂 )**和**具体实现的工厂子类**。程序员可以根据创建对象类型使用对应的工厂子类。这样将单个的简单工厂类变成了工厂簇，更利于代码的维护和扩展。
5. 类图
6. ![](/Snipaste_2020-09-30_22-18-43.png)

### 6.3.2 抽象工厂模式应用实例

使用抽象工厂模式来完成披萨项目

```java
package com.atguigu.factory.absfactory.pizzastore.order;

import com.atguigu.factory.absfactory.pizzastore.pizza.Pizza;

/**
 * 一个抽象工厂的抽象层(接口)
 *
 * @author gcq
 * @Create 2020-09-30
 */
public interface AbsFactory {

    // 让下main的工厂子类来具体实现
    public Pizza createPizza(String orderType);

}
```

OrderPizza.java

```java
package com.atguigu.factory.absfactory.pizzastore.order;

import com.atguigu.factory.absfactory.pizzastore.pizza.Pizza;

import java.io.BufferedReader;
import java.io.InputStreamReader;

/**
 * @author gcq
 * @Create 2020-09-30
 */
public class OrderPizza {

    AbsFactory factory;

    public OrderPizza(AbsFactory factory) {
        setFactory(factory);
    }

    private void setFactory(AbsFactory absFactory) {
        Pizza pizza = null;
        //    用户输入
        String orderType = "";
        this.factory = absFactory;

        do {
            orderType = gettype();
            // factory 可能是北京的工厂子类，也可能是伦敦的工厂子类
            pizza = factory.createPizza(orderType);
            if (pizza != null) {
                pizza.prepare();
                pizza.bake();
                pizza.cut();
                pizza.box();
            } else {
                System.out.println("订购失败");
                break;
            }
        } while (true);
    }
    //写一个方法 可以获取客户希望订购的披萨类
    private String gettype() {
        try {
            BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
            System.out.println("input pizza type:");
            String str = strin.readLine();
            return str;
        } catch (Exception e) {
            e.printStackTrace();
            return "";
        }
    }
}
```



### 6.4 工厂模式在 JDK-Calendar 应用的源码分析

1、JDK 中 Calendar 类中，就使用到了简单工厂模式

2、源码+Debug源码+分析

```java
package com.atguigu.jdk;

import java.util.Calendar;

/**
 * @author gcq
 * @Create 2020-09-30
 */
public class Factory {
    public static void main(String[] args) {
        //  getInstance 是 Calendar的静态方法
        Calendar instance = Calendar.getInstance();
        System.out.println("年:" + instance.get(Calendar.YEAR));
    }
}
```

```java
public static Calendar getInstance()
{
    return createCalendar(TimeZone.getDefault(), Locale.getDefault(Locale.Category.FORMAT));
}
```

```java
private static Calendar createCalendar(TimeZone zone,
                                       Locale aLocale) // 根据TimeZone Local 创建对应的实例
{
    CalendarProvider provider =
        LocaleProviderAdapter.getAdapter(CalendarProvider.class, aLocale)
                             .getCalendarProvider();
    if (provider != null) {
        try {
            return provider.getInstance(zone, aLocale);
        } catch (IllegalArgumentException iae) {
            // fall back to the default instantiation
        }
    }

    Calendar cal = null;

    if (aLocale.hasExtensions()) {
        String caltype = aLocale.getUnicodeLocaleType("ca");
        if (caltype != null) {
            switch (caltype) {
            case "buddhist":
            cal = new BuddhistCalendar(zone, aLocale);
                break;
            case "japanese":
                cal = new JapaneseImperialCalendar(zone, aLocale);
                break;
            case "gregory":
                cal = new GregorianCalendar(zone, aLocale);
                break;
            }
        }
    }
    if (cal == null) {
        // If no known calendar type is explicitly specified,
        // perform the traditional way to create a Calendar:
        // create a BuddhistCalendar for th_TH locale,
        // a JapaneseImperialCalendar for ja_JP_JP locale, or
        // a GregorianCalendar for any other locales.
        // NOTE: The language, country and variant strings are interned.
        if (aLocale.getLanguage() == "th" && aLocale.getCountry() == "TH") {
            cal = new BuddhistCalendar(zone, aLocale);
        } else if (aLocale.getVariant() == "JP" && aLocale.getLanguage() == "ja"
                   && aLocale.getCountry() == "JP") {
            cal = new JapaneseImperialCalendar(zone, aLocale);
        } else {
            cal = new GregorianCalendar(zone, aLocale);
        }
    }
    return cal;
}
```



# 第 7 章 原型模式

### 7.1 克隆羊问题

现在有一只羊tom,姓名为:tom,年龄为: 1， 颜色为:白色，请编写程序创建和tom羊属性完全相同的10
只羊。



7.2 传统方式解决克隆羊问题

1、思路分析(图解)

![](/Snipaste_2020-10-02_16-29-07.png)

2、看老师代码演示

```java
package com.atguigu.prototype;

/**
 * @author gcq
 * @Create 2020-10-02
 */
public class Client {
    public static void main(String[] args) {
        Sheep sheep = new Sheep("tom", 1, "白色");

        Sheep sheep1 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
        Sheep sheep2 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
        Sheep sheep3 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
        Sheep sheep4 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
        Sheep sheep5 = new Sheep(sheep.getName(), sheep.getAge(), sheep.getColor());
        ///....

        System.out.println(sheep1);
        System.out.println(sheep2);
    }
}
```



### 7.3 传统方式的优缺点

1. 有点是比较好理解，简单易操作

2. 在创建新的对象时，总是需要重新获取原始对象的属性，如果创建的对象比较复杂时，效率较低

3. 总是需要重新初始化对象，而不是动态地获得对象运行时的状态，不够灵活

4. 改进的思路分析

   **思路**: Java中Object类是所有类的根类，Object 类提供了一个clone0方法，该方法可以将-一个Java对象复制一份， 但是需要实现clone的Java类必须要实现-一个接口Cloneable,该接口表示该类能够复制且具有复制的能力=>

   **原型模式**

### 7.4 原型模式-基本介绍

1、原型模式(Prototype模式)是指: 用**原型实例指定创建对象的种类**，**并且通过拷贝这些原型，创建新的**对象
2、 原型模式是一种创建型设计模式，允许一个对象再创建另外一个可定制的对象，无需知道如何创建的细节
3、工作原理是:通过将一个原型对象传给那个要发动创建的对象，这个要发动创建的对象通过请求原型对象拷贝它们自己来实施创建，**即对象.clone()**
4、形象的理解:孙大圣拔出猴毛， 变出其它孙大圣



### 7.5 原型模式原理结构图 uml 类图

![](/Snipaste_2020-10-02_16-32-58.png)

**原型结构图说明**

Prototype:原型类，声明一个克隆自己的接口

ConcretePrototype：具体的原型类，实现一个克隆自己的操作

Cient 让一个原型对象克隆自己，从而创建一个新的对象(属性一样)



### 7.5 原型模式解决克隆羊问题的应用实例

使用原型模式改进传统方式，让程序具有更高的效率和扩展性

代码实现

```java
![Snipaste_2020-10-02_16-38-31](/Snipaste_2020-10-02_16-38-31.png)package com.atguigu.prototype.improve;

/**
 * @author gcq
 * @Create 2020-10-02
 */

public class Sheep implements Cloneable {

    private String name;
    private int age;
    private String color;
    public Sheep firend; // 是对象 克隆是会如何处理，默认是浅拷贝

    public Sheep(String name, int age, String color) {
        this.name = name;
        this.age = age;
        this.color = color;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    @Override
    public String toString() {
        return "Sheep{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", color='" + color + '\'' +
                '}';
    }

    @Override
    protected  Object clone()  {
        Sheep sheep = null;
        try {
            sheep = (Sheep) super.clone();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return sheep;
    }
}
```

```java
package com.atguigu.prototype.improve;

/**
 * @author gcq
 * @Create 2020-10-02
 */
public class Client {
    public static void main(String[] args) throws CloneNotSupportedException {
        System.out.println("原型模式完成对象创建");
        Sheep sheep = new Sheep("tom",1,"白色");
        Sheep sheep1 = (Sheep) sheep.clone(); // 克隆
        Sheep sheep2 = (Sheep) sheep.clone();
        Sheep sheep3 = (Sheep) sheep.clone();
        Sheep sheep4 = (Sheep) sheep.clone();

        System.out.println("sheep2=" + sheep2);

    }
}
```

###  

### 7.7 原型模式在 Spring 框架中源码分析

Spring 中原型 bean 的创建 ，就是原型模式的应用

代码分析+Debug源码

![](/Snipaste_2020-10-02_16-38-31.png)



### 7.8 深入讨论-浅拷贝和深拷贝

### 	7.9.1浅拷贝的介绍

1. 对于数据类型是基本数据类型的成员变量，浅拷贝会直接进行值传递，也就是将.
   该属性值复制一-份给新的对象。

2. 对于数据类型是引用数据类型的成员变量，比如说成员变量是某个数组、某个类的对象等，那么浅拷贝会进行引用传递，也就是只是将该成员变量的引用值(内存地址)复制一份给新的对象。因为实际上两个对象的该成员变量都指向同一个.
   实例。在这种情况下，在一个对象中修改该成员变量会影响到另一个对象的该成员变量值

3. 前面我们克隆羊就是浅拷贝

4. 浅拷贝是使用默认的clone()方法来实现

   **sheep = (Sheep) super.clone();**



### 7.8.2 深拷贝基本介绍

1. 复制对象的所有基本数据类型的成员变量值
2. 为所有引用数据类型的成员变量申请存储空间，并复制每个引用数据类型成员变量所引用的对象，直到该对象可达的所有对象。也就是说，**对象进行深拷贝要对整个对象进行拷贝(包括对象的引用类型进行拷贝)**
3. 深拷贝实现方式1:重写**clone**方法来实现深拷贝
4. 深拷贝实现方式2:通过**对象序列化实现深拷贝(推荐)**。



### 7.9 深拷贝应用分析

1. 使用重写 clone方法 实现深拷贝
2. 使用序列化实现深拷贝
3. 代码演示



```java
package com.atguigu.prototype.deepclone;

import java.io.Serializable;

/**
 * @author gcq
 * @Create 2020-10-02
 */
public class DeepCloneableTarget implements Serializable,Cloneable {

    private static final long serialVersionUID = 1L;

    private String cloneName;

    private String cloneClass;


    public DeepCloneableTarget(String cloneName, String cloneClass) {
        this.cloneName = cloneName;
        this.cloneClass = cloneClass;
    }

    // 该类属性都是String 默认clone方法 浅拷贝
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```



```java
package com.atguigu.prototype.deepclone;

import java.io.*;

/**
 * @author gcq
 * @Create 2020-10-02
 */
public class DeepProtoType implements Serializable,Cloneable {

    public String name;
    public DeepCloneableTarget deepCloneableTarget;

    public DeepProtoType() {
        super();
    }
    //深拷贝-- method1 使用clone方法
    @Override
    protected Object clone() throws CloneNotSupportedException {
        Object deep = null;
        // 这是完成对基本数据类型(属性)和String的克隆
        deep = super.clone();
        //对引用类型的属性,进行单独处理
        DeepProtoType deepProtoType = (DeepProtoType)deep;
        deepProtoType.deepCloneableTarget = (DeepCloneableTarget) deepCloneableTarget.clone();

        return deepProtoType;
    }

    // 深拷贝 method2 通过对象序列化实现(推荐)
    public Object deepClone() {

        // 创建流对象
        ByteArrayOutputStream bos = null;
        ObjectOutputStream oos = null;
        ByteArrayInputStream bis = null;
        ObjectInputStream ois = null;

        try {
            // 序列化
            bos = new ByteArrayOutputStream();
            oos = new ObjectOutputStream(bos);
            // 当前这个对象以对象流的方式输出
            oos.writeObject(this);

            // 反序列化
            bis = new ByteArrayInputStream(bos.toByteArray());
            ois = new ObjectInputStream(bis);
            DeepProtoType copyObj = (DeepProtoType) ois.readObject();

            return copyObj;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        } finally {
            try {
                bos.close();
                oos.close();
                bis.close();
                ois.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```



```java
package com.atguigu.prototype.deepclone;

/**
 * @author gcq
 * @Create 2020-10-02
 */
public class Client {

    public static void main(String[] args) throws CloneNotSupportedException {
        DeepProtoType p = new DeepProtoType();
        p.name = "宋江";
        p.deepCloneableTarget = new DeepCloneableTarget("大牛","大牛的类");

        // 方式1 完成深拷贝
//        DeepProtoType p2 = (DeepProtoType) p.clone();
//        System.out.println("p.name=" + p.name + "p.deepCloneableTraget=" + p.deepCloneableTarget.hashCode());
//        System.out.println("p2.name=" + p2.name + "p2.deepCloneableTraget=" + p2.deepCloneableTarget.hashCode());

        // 方式2 完成深拷贝
        DeepProtoType p2 = (DeepProtoType) p.deepClone();
        System.out.println("p.name=" + p.name + "p.deepCloneableTraget=" + p.deepCloneableTarget.hashCode());
        System.out.println("p2.name=" + p2.name + "p2.deepCloneableTraget=" + p2.deepCloneableTarget.hashCode());


    }
}
```

### 7.10 原型模式的注意事项和细节

1. 创建新的对象比较复杂时，可以利用原型模式**简化对象的创建过程，同时也能够提高**效率
2. 不用重新初始化对象，而是动态地获得对象运行时的状态
3. 如果原始对象发生变化(增加或者减少属性)，其它克隆对象的也会发生相应的变化，无需修改代码
4. 在实现深克隆的时候可能需要比较复杂的代码
5. **缺点**:需要为每一个类配备一个克隆方法，这对全新的类来说不是很难，但对已有
6. 的类进行改造时，需要修改其源代码，违背了ocp原则，这点请同学们注意.



# 第 8 章 建造者模式

### 8.1 盖房项目需求

1、需要建房子:这一过程为打桩、砌墙、封顶

2、房子有各种各样的，比如普通房，高楼，别墅，各种房子的过程虽然一样，但是要求不要相同的.

3、请编写程序， 完成需求.

### 8.2 传统方式解决盖房需求

思路分析(图解)

![](/Snipaste_2020-10-03_19-43-42.png)

看老师代码的演示



```java
package com.atguigu.builder;

/**
 * @author gcq
 * @Create 2020-10-03
 */
public abstract class AbstrctHouse {

    // 打地基
    public abstract void buildBasic();
    // 砌墙
    public abstract void buildWalls();
    //封顶
    public abstract void roofed();

    public void build() {
        buildBasic();
        buildWalls();
        roofed();
    }

}
```

```java
package com.atguigu.builder;

/**
 * @author gcq
 * @Create 2020-10-03
 */
public class CommonHouse extends AbstrctHouse {

    @Override
    public void buildBasic() {
        System.out.println(" 普通房子打地基");
    }

    @Override
    public void buildWalls() {
        System.out.println(" 普通方式砌墙");
    }

    @Override
    public void roofed() {
        System.out.println(" 普通房子封顶");
    }
}
```

```java
package com.atguigu.builder;

/**
 * @author gcq
 * @Create 2020-10-03
 */
public class Client {
    public static void main(String[] args) {
        CommonHouse commonHouse = new CommonHouse();
        commonHouse.build();
    }

}
```

### 8.3 传统方式解决盖房子需求问题分析

1、优点是比较好理解，简单易操作。

2、设计的程序结构，过于简单，没有设计缓存层对象，程序的扩展和维护不好.也就是说，这种设计方案，把产品(即:房子)和创建产品的过程(即:建房子流程)封
装在一起，耦合性增强了。

3、解决方案:将产品和产品建造过程解耦=>**建造者模式**.



### 8.4 建造者模式基本介绍

1、建造者模式(**Builder Pattern)** ]又叫**生成器模式**，是一种对象**构建模式**。它可以将复杂对象的建造过程抽象出来(抽象类别)，使这个抽象过程的不同实现方法可以构造出不同表现(属性)的对象。
2、建造者模式是一步--步创建一个复杂的对象，它允许用户只通过指定复杂对象的类型和内容就可以构建它们，
用户不需要知道内部的具体构建细节。



### 8.5 建造者模式的四个角色

1、Product (产品角色) :一个具体的产品对象。

2、Builder (抽象建造者) :创建个Product对 象的各 个部件指定的接口/抽象类。

3、ConcreteBuilder (具体建造者) :实现接口， 构建和装配各个部件。

4、 Director (指挥者) :构建-个使用Builder接 口的对象。 它主要是用于创建一个复杂的对象。它主要有两个作用，一是:隔离了客户与对象的生产过程，二是:负责控制产品对象的生产过程。

![](/Snipaste_2020-10-03_19-51-03.png)



### 8.7 建造者模式解决盖房需求应用实例

1、需要建房子:这一过程为打桩、砌墙、封顶。不管是普通房子也好，别墅也好都需要经历这些过程，下 面我们使用建造者模式(Builder Pattern)来完成

2、思路分析图解(类图)

![](/Snipaste_2020-10-03_19-51-39.png)

3、代码实现

```java
package com.atguigu.builder.improve;

/**
 * 指挥者 这里指定制作流程
 *
 * @author gcq
 * @Create 2020-10-03
 */
public class HouseDirector {
    HouseBuilder houseBuilder = null;

    // 构造器传入

    public HouseDirector(HouseBuilder houseBuilder) {
        this.houseBuilder = houseBuilder;
    }
    // set传入
    public void setHouseBuilder(HouseBuilder builder) {
        this.houseBuilder = builder;
    }

    // 如何处理建造房子的流程，交给指挥者
    public House constructHouse() {
        houseBuilder.buildBasic();
        houseBuilder.buildWalls();
        houseBuilder.roofed();
        return houseBuilder.buildHouse();
    }

}
```

```java
package com.atguigu.builder.improve;

/**
 * @author gcq
 * @Create 2020-10-03
 */
public abstract class HouseBuilder {

    protected House house = new House();

    // 建造流程写好，抽象的方法
    public abstract void buildBasic();
    public abstract void buildWalls();
    public abstract void roofed();

    // 建造房子好，将产品(房子) 返回
    public House buildHouse() {
        return house;
    }
}
```

```java
package com.atguigu.builder.improve;

/**
 * 产品
 * @author gcq
 * @Create 2020-10-03
 */
public class House {
    private String baise;
    private String wall;
    private String rooed;

    public String getBaise() {
        return baise;
    }

    public void setBaise(String baise) {
        this.baise = baise;
    }

    public String getWall() {
        return wall;
    }

    public void setWall(String wall) {
        this.wall = wall;
    }

    public String getRooed() {
        return rooed;
    }

    public void setRooed(String rooed) {
        this.rooed = rooed;
    }
}
```

```java
package com.atguigu.builder.improve;

/**
 * @author gcq
 * @Create 2020-10-03
 */
public class HighBuilding extends HouseBuilder {

    @Override
    public void buildBasic() {
        System.out.println(" 高楼地基100米");
    }

    @Override
    public void buildWalls() {
        System.out.println(" 高楼砌墙20cm");
    }

    @Override
    public void roofed() {
        System.out.println(" 高楼透明房顶");
    }

}
```

```java
package com.atguigu.builder.improve;

/**
 * @author gcq
 * @Create 2020-10-03
 */
public class CommonHouse extends HouseBuilder {

    @Override
    public void buildBasic() {
        System.out.println(" 普通房子打地基5米");
    }

    @Override
    public void buildWalls() {
        System.out.println(" 普通房子砌墙10cm");
    }

    @Override
    public void roofed() {
        System.out.println(" 普通房子封顶");
    }
}
```

```java
package com.atguigu.builder.improve;

/**
 * @author gcq
 * @Create 2020-10-03
 */
public class Client {
    public static void main(String[] args) {

        // 盖普通房子
        CommonHouse commonHouse = new CommonHouse();
        // 准备创建房子的指挥者
        HouseDirector houseDirector = new HouseDirector(commonHouse);

        // 完成盖房子，返回产品(房子)
        House house = houseDirector.constructHouse();

//        System.out.println("输出流程");

        System.out.println("--------------------------");
        // 盖高楼
        HighBuilding highBuilding = new HighBuilding();
        //  重置建造者
        houseDirector.setHouseBuilder(highBuilding);
        // 完成盖房子 返回产品
        houseDirector.constructHouse();

    }
}
```



### 8.8 建造者模式在JDK的应用和源码分析

1、java.lang.StringBuilder 中建造者模式

2、代码说明+Debug源码

![](/Snipaste_2020-10-03_19-54-35.png)

3、源码中建造者模式角色分析

1、Appendable接口定义了多个append方法(抽象方法)，即Appendable为抽象建造者，定义了抽象方法
2、AbstractStringBuilder实现了Appendable 接口方法，这里的AbstractStringBuilder已经 是建造者，只是不能实例化
3、StringBuilder即充当了指挥者角色，同时充当了具体的建造者,建造方法的实现是由AbstractStringBuilder 完成，而
4、StringBuilder继承 了AbstractStringBuilder



### 8.9 建造者模式的注意事项和细节

1、客户端(使用程序)不必知道产品内部组成的细节，将产品本身与产品的创建过程解耦，使得相同的创建过程可以创建不同的产品对象

2、每一个具体建造者都相对独立，而与其他的具体建造者无关，因此可以很方便地替换具体建造者或增加新的具体建造者，用户使用不同的具体建造者即可得到不同的产品对象

3、可以更加精细地控制产品的创建过程。将复杂产品的创建步骤分解在不同的方法中，使得创建过程更加清晰，也更方便使用程序来控制创建过程

4、增加新的具体建造者无须修改原有类库的代码，指挥者类针对抽象建造者类编程，系统扩展方便，符合“开闭原则”

5、建造者模式所创建的产品一般具有较多的共同点，其组成部分相似，**如果产品之间**
**的差异性很大，则不适合使用建造者模式**，因此其使用范围受到一定的限制。

6、如果产品的内部变化复杂，可能会导致需要定义很多具体建造者类来实现这种变化，导致系统变得很庞大，因此在这种情况下，要考虑是否选择建造者模式.

7、抽象工厂模式VS建造者模式抽象工厂模式实现对产品家族的创建，- 一个产品家族是这样的一系列产品:具有不同分类维度的产品组合，采用抽象工厂模式不需要关心构建过程，只关心什么产品由什么工厂生产即可。而建造者模式则是要求按照指定的蓝图建造产品，它的主要目的是通过组装零配件而产生-一个新产品



# 第 9 章 适配器设计模式

### 9.1 现实生活中的适配器例子

泰国插座用的是两孔的(欧标)，可以买个多功能转换插头(适配器)，这样就可以使用了。

![](/../../Docker/image/Snipaste_2020-10-04_12-59-00.png)

### 9.2 基本介绍

1. 适配器模式(Adapter Pttern)将某个类的接口转换成客户端期望的另一个接口表示，**主的目的是兼容性**，让原本因接口不匹配不能一起工作的两个类可以协同工作。其别名为包装器(Wrapper)
2. 适配器模式属于结构型模式
3. 主要分为三类**:类适配器模式、对象适配器模式、接口适配器模式**



### 9.3 工作原理

1. 适配器模式:将一个类的接口转换成另一种接口.让原本接口不兼容的类可以兼容
2. 从用户的角度看不到被适配者，是解耦的
3. 用户调用适配器转化出来的目标接口方法，适配器再调用被适配者的相关接口方法
4. 用户收到反馈结果，感觉只是和目标接口交互，如图

![](/Snipaste_2020-10-04_15-54-45.png)



### 9.4 类适配器模式

### 9.4.1类适配器模式介绍

基本介绍: Adapter类， 通过继承src类，实现dst类接口，完成src->dst的适配。

### 9.4.2类适配器模式应用实例

1. 应用实例说明

   ​	外以生活中充电器的例子来讲解适配器，充电器本身相当于Adapter，220V 交流电相当于sre (即被适配者)，我们的目dst(即目标)是5V直流电

2. 思路分析(类图)

   ![](/Snipaste_2020-10-04_15-56-04.png)

3. 代码实现

4. ![](/Snipaste_2020-10-04_15-57-56.png)



### 9.4.3 类适配器模式注意事项和细节

1、Java是单继承机制，所以类适配器需要继承src类这一点算是一个缺点，因为这要中求dst必须是接口，有一定局限性;

2、 src类的方法在Adapter中都会暴露出来，也增加了使用的成本。

3、由于其继承了src类，所以它可以根据需求重写src类的方法，使得Adapter的灵
活性增强了。

### 9.5 对象适配器

1. 基本思路和类的适配器模式相同，只是将Adapter类作修改，不是继承src类，而是持有sre类的实例，以解决
   兼容性的问题。即: 持有sre类，实现dst类接口，完成sre->dst的适配
2. 根据“**合成复用原则**’，在系统中尽量使用**关联关系(聚合)来替代继承**关系。
3. 对象适配器模式是适配器模式常用的一-种

### 9.5.2 对象适配器模式应用实例

1、应用实例说明
	以生活中充电器的例子来讲解适配器，充电器本身相当于Adapter, 220V交流电相当于src (即被适配者)，我们的目dst(即目标)是5V直流电，使用对象适配器模式完成。

2、思路分析(类图): 只需修改适配器即可，如下:

![](/Snipaste_2020-10-04_16-00-27.png)

3、代码实现

![](/Snipaste_2020-10-04_16-02-21.png)

### 9.5.3 对象适配器模式注意事项和细节

1. 对象适配器和类适配器其实算是同一种思想，只不过实现方式不同。根据合成复用原则，使用组合替代继承，所以它解决 了类适配器必须继承src的局限性问题，也不再要求dst必须是接口。
2. 使用成本更低，更灵活。



### 9.6 接口适配器

1. 一-些书籍称为:适配器模式(DefaultAdapterPattern)或**缺省适配器模式**。
2. 当**不需要全部实现接口提供的方法**时，可先**设计一个抽象类**实现**接口**，并为该接.口中每个方法提供-一个**默认实现(空方法)**，那么该**抽象类的子类可有选择地覆盖父类的某些方法**来实现需求
3. 适用于一个接口不想使用其所有的方法的情况。



### 9.6.2 接口适配器模式应用实例

1. Android中的属性动画ValueAnimator类可以通过addListener(AnimatorListener listener)方法添加监听器，那么
   常规写法如右:

   ![](/Snipaste_2020-10-04_16-06-06.png)

2. 有时候我们不想实现lAnimator.AnimatorListener接口的全部方法，我们只想监听onAnimationStart,我们会如下写

![](/Snipaste_2020-10-04_16-06-01.png)

​	3.AnimatorListenerAdapter类，就是一个接口适配器，代码如右图:它空实现了Animator.AnimatorListener类(src)的所有方法.

![](/Snipaste_2020-10-04_16-07-22.png)

​	4.AnimatorListener是一个接口.

![](/Snipaste_2020-10-04_16-07-14.png)

5、程序里的匿名内部类就是Listener，具体实现类

![](/Snipaste_2020-10-04_16-08-23.png)

6、案例说明

![](/Snipaste_2020-10-04_16-09-09.png)

```java
package com.atguigu.adapter.interfaceadapter;

/**
 * @author gcq
 * @Create 2020-10-04
 */
public interface Interface4 {
    public void m1();
    public void m2();
    public void m3();
    public void m4();

}
```

```java
package com.atguigu.adapter.interfaceadapter;

/**
 * @author gcq
 * @Create 2020-10-04
 */
public class AbsAdapter implements Interface4 {

    @Override
    public void m1() {

    }

    @Override
    public void m2() {

    }

    @Override
    public void m3() {

    }

    @Override
    public void m4() {

    }
}
```

```java
![Snipaste_2020-10-04_16-11-56](/Snipaste_2020-10-04_16-11-56.png)package com.atguigu.adapter.interfaceadapter;

/**
 * @author gcq
 * @Create 2020-10-04
 */
public class Client {
    public static void main(String[] args) {
        AbsAdapter absAdapter = new AbsAdapter() {
            // 只需要覆盖我们需要使用 接口方法
            @Override
            public void m1() {
                System.out.println("使用了m1的方法");
            }
        };
        absAdapter.m1();
    }

}
```

### 9.7 适配器在SpringMVC框架应用的源码分析

1、SpringMvc中 的**HandlerAdapter**,就使用了适配器模式

2、SpringMVC处理 请求的流程回顾

3、使用HandlerAdapter的原因分析:

​	可以看到处理器的类型不同，有多实现方式，那么调用方式就不是确定的，如果需要直接调用**Controller**方法，需要调用的时候就得不断是使用**if else来**进行判断是哪一种子类然后执行。那么如果后面要扩展**Controller**,就得修改原来的代码，这样违背了**OCP**原则。

4、源码+Debug+分析

![](/Snipaste_2020-10-04_16-11-56.png)

5、动手写SpringMVC通过适配器设计模式获取到对应的Controller源码

![](/Snipaste_2020-10-04_16-12-52.png)

类图

![](/Snipaste_2020-10-04_16-13-03.png)

### 9.8 适配器模式注意事项和细节

1. 三种命名方式，是根据src是以怎样的形式给到Adapter (在Adapter里的形式)来命名的。
2. 类适配器:以类给到，在Adapter里， 就是将src当做类， 继承
   	对象适配器:以对象给到，在Adapter里， 将src作为一个对象， 持有
   	接口适配器:以接口给到，在Adapter里， 将src作为一个接口， 实现
3. Adapter模式最大的作用还是将原本不兼容的接口融合在一起工作。
4. 实际开发中，实现起来不拘泥于我们讲解的**三种经典形式**