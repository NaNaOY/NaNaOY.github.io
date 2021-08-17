# Java修饰符

---

### static方法

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
>
> 
>
> 







