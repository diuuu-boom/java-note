<center>Java 二十三种设计模式</center>

[TOC]

### 设计模式的七大原则

设计模式的原则就是程序员在编程过程中，应当遵循的原则，也是各种设计模式的基础（即：设计模式为什么这样设计的依据）

#### 设计模式常用的七大原则有：

#### 1). 单一职责原则

* 基本介绍：对类来说，即一个类应该应该只负责这一项职责，如果类A负责两个不同职责：职责1，职责2.

  ​		   当职责1 需求变更为 A 时，可能造成职责2执行错误，所以需要将类 A 的粒度分解为A1，A2

* 应用实例：

  * 以交通工具案例讲解

  * 方案一 [分析说明]

    ```java
    public class SingleResponsibility1{
        public static void main(String[] args){
            
        }
    }
    // 交通工具类
    // 方式1
    // 	1.在方式1的 run 方法中，违反了但一职责原则
    //  2.解决的方案非常简单，根据交通工具运行方法不同，分解成不同类即可
    class Vehicle{
        public void run(String vehicle){
            System.out.println(vehicle + "自公路上行驶。。。");
        }
    }
    ```

    

  * 方案二 [分析说明]

    ```java
    public class SingleResponsibility2{
        
        public static void main(String[] args){
            RoadVehicle roadVehicle = new RoadVehicle();
    		roadVehicle.run("摩托");
    		roadVehicle.run("汽车");
    		
    		AirVehicle airVehicle = new AirVehicle();
    		airVehicle.run("飞机");
        }
        
    }
    
    // 方案2 的分析
    //1.遵守单一职责原则
    //2.但是这样的改动很大，即将类分解，同时修改客户端
    //3.改进：直接修改Vehicle 类，改动的代码会比较少=>方案3
    class RoadVehicle{
    	
    	public void run(String vehicle) {
    		System.out.println(vehicle+"公路运行。。。");
    	}
    	
    }
    
    class AirVehicle{
    	public void run(String vehicle) {
    		System.out.println(vehicle+"空中飞行。。。");
    	}
    }
    
    class WaterVehicle{
    	public void run(String vehicle) {
    		System.out.println(vehicle + "水上航行。。。");
    	}
    }
    
    
    ```

    

  * 方案三 [分析说明]

    ```java
    public class SingleResponsibility3 {
    
    	public static void main(String[] args) {
    		Vehicle2 vehicle2 =  new Vehicle2();
    		vehicle2.runRoad("汽车");
    		vehicle2.runAir("飞机");
    		vehicle2.runWater("快艇");
    	}
    	
    }
    
    // 方式3
    //1. 这种修改方法没有对原来的类做大的修改只是增加方法
    //2. 这里虽然没有在类这个级别上遵循单一职责原则，但是在方法级别上，仍是遵守单一职责得
    class Vehicle2 {
    	
    	public void runRoad(String vehicle) {
    		// 处理
    		if()else if else if else...
    		System.out.println(vehicle + "在 公路上 运行。。。");
    
    	}
    	public void runAir(String vehicle) {
    
    		System.out.println(vehicle + "在 空中 运行。。。");
    
    	}
    	public void runWater(String vehicle) {
    
    		System.out.println(vehicle + "在 水上 运行。。。");
    
    	}
    
    }
    ```

    

* 单一职责原则注意事项和细节
  * 降低类的复杂度，一个类只负责一项职责。
  * 提高类的可读性，可能维护性
  * 降低变更引起的风险
  * 通常情况下，**我们应当遵循单一职责原则**，只有逻辑简单，才可以在代码级违反单一职责原则, 只有类中方法数量足够少，可以在方法级别保持单一指责原则。

#### 2). 接口隔离原则（Interface Segregation Principle）

* 基本介绍: 

  * 客户端不应该依赖它不需要的接口，即一个类对另一个类的依赖应该j建立在最小的接口上

  * 先看一张图: 

    <img src="C:\Users\winer\Desktop\公司\23种设计模式\image\接口隔离(1).png" style="width:350px height:450px" />

  * 类A通过接口Interface1 依赖类B，类C通过接口Interface1依赖类D，如果接口 Interface1对于类A和类C来说不是最小接口那么类B和类D必须去实现她们不需要的方法。

  * 按隔离原则应当这样处理: 

    **将接口Interface1**拆分为**独立的几个接口(这里我们拆分成 3 个接口)**，类A 和类C 分别与他们需要的接口建立依赖关系。也就是采用接口隔离原则

  * 应传统方法的问题和使用接口隔离原则改进

    * 类 A 通过接口 Interface1 依赖接口 B，类 C 通过接口 Interface1 依赖类 D ，如果接口Interface1 对于类 A 和类 C 来说不是最小接口，那么类 B 和类 D 必须去实现她们不需要的方法。

    * 将接口 Interface1 拆分为独立的几个接口，类 A 和 类 C 分别与他们需要的接口建立依赖关系。也就是采用接口隔离原则

    * 接口 Interface1 中出现的方法，根据实际情况拆分为三个接口

    * 代码实现

      ```java
      public class Segregation1{
          public static void main(String[] args){
              A a = new A();
              a.operation1(new B());
              a.operation2(new B());
              a.operation3(new B());
              
              C c = new C();
              c.operation1(new D());
              c.operation4(new D());
              c.operation5(new D());
          }
      }
      interface Interface1{
          void operation1();
          void operation2();
          void operation3();
          void operation4();
          void operation5();
      }
      class B implements Interface1{
          public void operation1(){System.out.println("B 实现了 operation1 方法");}
          public void operation2(){System.out.println("B 实现了 operation2 方法");}
          public void operation3(){System.out.println("B 实现了 operation3 方法");}
          public void operation4(){System.out.println("B 实现了 operation4 方法");}
          public void operation5(){System.out.println("B 实现了 operation5 方法");}
      }
      class D implements Interface1{
          public void operation1(){System.out.println("D 实现了 operation1 方法");}
          public void operation2(){System.out.println("D 实现了 operation2 方法");}
          public void operation3(){System.out.println("D 实现了 operation3 方法");}
          public void operation4(){System.out.println("D 实现了 operation4 方法");}
          public void operation5(){System.out.println("D 实现了 operation5 方法");}
      }
      
      class A {  // 类 A 只需要 依赖 类 B 里面的 operation1、operation2、operation3
          public void depend1(Interface i){i.operation1();}
          public void depend2(Interface i){i.operation2();}
          public void depend3(Interface i){i.operation3();}
      }
      
      class C { // 类 C 只需要 依赖 类 D 里面的 operation1、operation4、operation5
          public void depend1(Interface i){i.operation1();}
          public void depend4(Interface i){i.operation4();}
          public void depend5(Interface i){i.operation5();}
      }
      ```

      接口 分离 :

      ```java
      public class Segregation2{
          public static void main(String[] args){
              
          }
      }
      interface Interface1 {
          void operation1();
      }
      interface Interface2 {
          void operation2();
          void operation3();
      }
      interface Interface3 {
          void operation4();
          void operation5();
      }
      
      class B implements Interface1,Interface2 {
          public void operation1(){System.out.println("类B 实现 operation1 方法");}
          public void operation2(){System.out.println("类B 实现 operation2 方法");}
          public void operation3(){System.out.println("类B 实现 operation3 方法");}
      }
      class C implements Interface1,Interface3 {
          public void operation1(){System.out.println("类B 实现 operation1 方法");}
          public void operation4(){System.out.println("类B 实现 operation4 方法");}
          public void operation5(){System.out.println("类B 实现 operation5 方法");}
      }
      
      class A {
          public void depend1(Interface1 i){i.operation1();}
          public void depend2(Interface2 i){i.operation2();}
          public void depend3(Interface2 i){i.operation3();}
      }
      class B {
          public void depend1(Interface1 i){i.operation1();}
          public void depend2(Interface3 i){i.operation4();}
          public void depend3(Interface3 i){i.operation5();}
      }
      ```

      

      

#### 3). 依赖倒转（倒置）原则

#### 4). 里氏替换原则

#### 5). 开闭原则

#### 6). 迪米特法则

#### **7). 合成复用原则**



### 设计模式的目的

​	在编写软件的过程中，程序员面临着来自 耦合性，内聚性以及可维护性，可扩展性，重用性，灵活性 等多方面的挑战，设计模式为了让程序（软件），具有更好的:

​	1). 代码重用性<font color=red size=3>（即: *相同功能的代码,不用多次编写*）</font>

​	2). 可读性<font color=red size=3>（即：*编程规范性，便于其他程序员的阅读和理解*）</font>

​	3). 可扩张<font color=red size=3>（即：*当需要增加新的功能时，非常的方便，称为可维护*）</font>

​	4). 可靠性<font color=red size=3>（即：*当我们增加新的功能后，对原来的功能没有影响*）</font>

​	5). 使程序呈现高内聚，低耦合的特性

​	总结:

​		<font color=red size=4><u>设计模式包含了面向对象的精髓，“懂了设计模式你就懂了面向对象分析和设计（OOA/D）的精要”</u></font>



```uml

```





















































