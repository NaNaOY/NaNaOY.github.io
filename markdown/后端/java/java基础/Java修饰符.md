# Java修饰符

---

### static修饰符

> ​		static方法一般称作静态方法，静态方法可以不依赖于任何对象就可以进行访问，因此在静态方法中是没有this的，也就是说没有对象。所以在静态方法中不能访问类的非静态成员变量和非静态成员方法（只能访问带static的），因为非静态的成员变量/方法必须依赖具体的对象才能够被调用。
>
> ​		但是要注意的是，虽然在静态方法中不能访问非静态成员方法和非静态成员变量，但是在非静态成员方法中是可以访问静态成员方法/变量的。如下：
>
> ```java
> public class MyObject{
> 	private static String str1="staticProperty";
> 	private String str2="property";
> 	
> 	public MyObject(){
> 	
> 	}
> 	
> 	public void print1(){
> 		System.out.println(str1);//正常，可以访问静态成员变量
> 		System.out.println(str2);
> 		print2();//正常，可以访问静态成员方法
> 	}
> 	
> 	public static void print2(){
> 		System.out.println(str1);
> 		System.out.println(str2);//报错，不可访问。str2非静态成员变量
> 		print1();//报错，不可调用。 print1非静态成员方法
>       
>     /*正确解答
>     Sta stA=new Sta();
>     System.out.println(stA.str2);
>     stA.print1();
>     */
> 	}
>   
>   public static void main(String[] args){
>         MyObject sta=new MyObject();    //不管是调用哪个方法都需要建立对象才行，因为其中存在非静态成员变量/方法
>         sta.print2();//不可以用类去调用静态方法，MyObject.print2(); 
>     }
> }
> ```
>
> 
>
> ### static关键字会改变类中成员的访问权限吗？
>
> ```java
> public class Value {
>     static int value=33;
> 
>     public static void main(String[] args)throws Exception{
>         new Value().printValue();
>     }
> 
>     private void printValue(){
>         int value=3;
>         //this.value=value; 如果该对象没有赋值，则value=3不可能跟对象关联，则对象的值是33
>         System.out.println(this.value);
>     }
> }
> ```
>
> ​		这里面主要考察对this和static的理解。this代表什么？this代表当前对象，那么通过new Main()来调用printValue的话，当前对象就是通过new Main()生成的对象。而static变量是被对象所享有的，因此在printValue中的this.value的值毫无疑问是33。在printValue方法内部的value是局部变量，根本不可能与this关联，所以输出结果是33。在这里永远要记住一点：静态成员变量虽然独立于对象，但是不代表不可以通过对象去访问，所有的静态方法和静态变量都可以通过对象访问（只要访问权限足够）。

### 修饰符

> ​		修饰符用来定义类、方法或者变量，通常放在语句的最前端。
>
> ```java
> public class ClassName {
>    // ...
>    private boolean myFlag;
>    static final double weeks = 9.5;
>    protected static final int BOXWIDTH = 42;
>   
> public static void main(String[] arguments) {
> 	// 方法体
>         }
>     }
> ```

### 访问控制修饰符

> Java中，可以使用访问控制符来保护类、变量、方法、和构造方法的访问。
>
> - **default** (即默认，什么也不写）: 在同一包内可见，不使用任何修饰符。使用对象：类、接口、变量、方法。
> - **private** : 在同一类内可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**
> - **public** : 对所有类可见。使用对象：类、接口、变量、方法
> - **protected** : 对同一包内的类和所有子类可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**。
>
> | 修饰符      | 当前类 | 同一包内 | 子孙类(同一包) | 子孙类(不同包)                                               | 其他包 |
> | :---------- | :----- | :------- | :------------- | :----------------------------------------------------------- | :----- |
> | `public`    | Y      | Y        | Y              | Y                                                            | Y      |
> | `protected` | Y      | Y        | Y              | Y/N（[说明](https://www.runoob.com/java/java-modifier-types.html#protected-desc)） | N      |
> | `default`   | Y      | Y        | Y              | N                                                            | N      |
> | `private`   | Y      | N        | N              | N                                                            | N      |
>
> 
>
> ## 私有访问修饰符-private
>
> 私有访问修饰符是最严格的访问级别，所以被声明为 **private** 的方法、变量和构造方法只能被所属类访问，并且类和接口不能声明为 **private**。
>
> 声明为私有访问类型的变量只能通过类中公共的 getter 方法被外部类访问。Private 访问修饰符的使用主要用来隐藏类的实现细节和保护类的数据。
>
> 实例中，Logger 类中的 format 变量为私有变量，所以其他类不能直接得到和设置该变量的值。为了使其他类能够操作该变量，定义了两个 public 方法：getFormat() （返回 format的值）和 setFormat(String)（设置 format 的值）
>
> ```java
> public class Logger {
>     private String format;
> 
>     public String getFormat() {
>         return this.format;
>     }
> 
>     public void setFormat(String format) {
>         this.format = format;
>     }
> }
> ```
>
> 
>
> ## 公有访问修饰符-public
>
> 被声明为 public 的类、方法、构造方法和接口能够被任何其他类访问。
>
> 如果几个相互访问的 public 类分布在不同的包中，则需要导入相应 public 类所在的包。由于类的继承性，类所有的公有方法和变量都能被其子类继承。且Main函数必须是共有的，否则，Java 解释器将不能运行该类。
>
> ```java
> public static void main(String[] arguments) {
>    // ...
> }
> ```
>
> 
>
> ## 受保护的访问修饰符-protected
>
> protected 需要从以下两个点来分析说明：
>
> - **子类与基类在同一包中**：被声明为 protected 的变量、方法和构造器能被同一个包中的任何其他类访问；
> - **子类与基类不在同一包中**：那么在子类中，子类实例可以访问其从基类继承而来的 protected 方法，而不能访问基类实例的protected方法。
>
> protected 可以修饰数据成员，构造方法，方法成员，**不能修饰类（内部类除外）**。
>
> 其中接口及接口的成员变量和成员方法不能声明为protected。
>
> ```java
> class AudioPlayer {
>    protected boolean openSpeaker(Speaker sp) {
>       // 实现细节
>    }
> }
>  
> class StreamingAudioPlayer extends AudioPlayer {
>    protected boolean openSpeaker(Speaker sp) {
>       // 实现细节
>    }
> }
> ```
>
> ### 因此访问和继承需要注意以下规则：
>
> - 父类中声明为 public 的方法在子类中也必须为 public。
> - 父类中声明为 protected 的方法在子类中要么声明为 protected，要么声明为 public，不能声明为 private。
> - 父类中声明为 private 的方法，不能够被继承。

### final修饰符

> final表示“最后的、最终的”含义，变量一旦赋值后，不能被重新赋值。被final修饰的实例变量必须显式指定初始值。
>
> final修饰符通常和static修饰符一起来创建类常量
>
> ```java
> public class Test{
> 	final int value=10;
> 	//下面是声明常量的实例
> 	public static final int BOXWIDTH=6;
> 	static final String TITLE="Manager";
> 	
> 	public void changeValue(){
> 	value=12;//将输出一个错误
> 	}
> }
> ```

### abstract 修饰符

> **抽象类：**
>
> 抽象类不能用来实例化对象，声明抽象类的唯一目的是为了将来对该类进行扩充。
>
> 一个类不能同时被 abstract 和 final 修饰。如果一个类包含抽象方法，那么该类一定要声明为抽象类，否则将出现编译错误。
>
> 抽象类可以包含抽象方法和非抽象方法。
>
> **抽象方法**
>
> 抽象方法是一种没有任何实现的方法，该方法的的具体实现由子类提供。
>
> 抽象方法不能被声明成 final 和 static。
>
> 任何继承抽象类的子类必须实现父类的所有抽象方法，除非该子类也是抽象类。
>
> 如果一个类包含若干个抽象方法，那么该类必须声明为抽象类。抽象类可以不包含抽象方法。
>
> 抽象方法的声明以分号结尾，例如：**public abstract sample();**。
>
> ```java
> public abstract class SuperClass{
>     abstract void m(); //抽象方法
> }
>  
> class SubClass extends SuperClass{
>      //实现抽象方法
>       void m(){
>           .........
>       }
> }
> ```
>
> 

# [返回首页](https://nanaoy.github.io/)
