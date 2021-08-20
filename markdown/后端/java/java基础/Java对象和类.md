# Java对象和类

---

## 多态

---

## 继承

---

## 封装

---

## 抽象

---

## 类

> 一个类可以包含以下类型变量：
>
> - **局部变量**：在方法、构造方法或者语句块中定义的变量被称为局部变量。变量声明和初始化都是在方法中，方法结束后，变量就会自动销毁。
> - **成员变量**：成员变量是定义在类中，方法体之外的变量。这种变量在创建对象的时候实例化。成员变量可以被类中方法、构造方法和特定类的语句块访问。
> - **类变量**：类变量也声明在类中，方法体之外，但必须声明为 static 类型。
>
> 一个类可以拥有多个方法，在上面的例子中：eat()、run()、sleep() 和 name() 都是 Dog 类的方法。
>
> ```java
> public class Dog {
>     //类的属性
>     String breed; //品种
>     int size;
>     String colour;
>     int age;
> 
>     //类的方法
>     void eat(){}
>     void run(){}
>     void sleep(){}
>     void name(){}
>    
> }
> ```

---

## 对象

> 看看周围真实的世界，会发现身边有很多对象，车，狗，人等等。所有这些对象都有自己的状态和行为。
>
> 拿一条狗来举例，它的状态有：名字、品种、颜色，行为有：叫、摇尾巴和跑。
>
> 对比现实对象和软件对象，它们之间十分相似。
>
> 软件对象也有状态和行为。软件对象的状态就是属性，行为通过方法体现。
>
> 在软件开发中，方法操作对象内部状态的改变，对象的相互调用也是通过方法来完成。

---

## 实例

### 访问实例变量和方法

> ```java
> /* 实例化对象 */ 
> Object referenceVariable = new Constructor();
> /* 访问类中的变量 */
> referenceVariable.variableName;
> /* 访问类中的方法 */
> referenceVariable.methodName();
> 
> 
> public static void main(String[] args){
>       /* 创建对象 */
>       Puppy myPuppy = new Puppy( "tommy" );
>       /* 通过方法来设定age */
>       myPuppy.setAge( 2 );/对象访问实例方法
>       /* 调用另一个方法获取age */
>       myPuppy.getAge( );
>       /*你也可以像下面这样访问成员变量 */
>       System.out.println("变量值 : " + myPuppy.puppyAge ); //对象访问实例变量
>    }
> }
> ```
> 写一个类和对象的基本步骤：
>
> - 声明成员变量在主类中，其次编写出该类对应的构造方法（个人认为构造函数用来创建对象的主键，例如姓名，位置等等且名字跟类名一致），其次编写去给声明的变量赋值的构建方法，最后根据要求在Test的main函数中用新建的对象调用构造函数赋值，输出。 
>
> - ```java
>   package com.company.Test.object;
>   import java.io.*;
>   public class Employee {
>     
>       String name;
>       int age;
>       String designation;
>       double salary;
>     
>       //Employee类的构造器
>       public Employee(String name){
>           this.name=name;
>       }
>       //设置年龄
>       public void empAge(int empAge){
>           age=empAge;
>       }
>       /*设置designation的值*/
>       public void empDesignation(String empDesig){
>           designation=empDesig;
>       }
>       /*设置salary的值*/
>       public void empSalary(double empSalary){
>           salary=empSalary;
>       }
>       /*打印信息*/
>     public void printEmployee(){
>         System.out.println("名字"+name);
>         System.out.println("年龄"+age);
>         System.out.println("职位"+designation);
>         System.out.println("薪水"+salary);
>         System.out.println();
>     	}
>     }
>

### 创建对象

>
>   ```java
>   Site site = new Site("Runoob")； 
>   ```
>   括号内加引号
>

### 源文件声明规则

> - 一个源文件中只能有一个 public 类
> - 一个源文件可以有多个非 public 类
> - 源文件的名称应该和 public 类的类名保持一致。例如：源文件中 public 类的类名是 Employee，那么源文件应该命名为Employee.java。
> - 如果一个类定义在某个包中，那么 package 语句应该在源文件的首行。
> - 如果源文件包含 import 语句，那么应该放在 package 语句和类定义之间。如果没有 package 语句，那么 import 语句应该在源文件中最前面。
> - import 语句和 package 语句对源文件中定义的所有类都有效。在同一源文件中，不能给不同的类不同的包声明。
>
> 类有若干种访问级别，并且类也分不同的类型：抽象类和 final 类等。
>
> 除了上面提到的几种类型，Java 还有一些特殊的类，如：[内部类](https://www.runoob.com/java/java-inner-class.html)、[匿名类](https://www.runoob.com/java/java-anonymous-class.html)。

---

## 方法

---

## 重载

---

# [返回首页](https://nanaoy.github.io/)
