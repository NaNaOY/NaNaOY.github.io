# Java Number & Math 类

---

​		一般地，当需要使用数字的时候，我们通常使用内置数据类型，如：**byte、int、long、double** 等。

```java
int a = 5000;
float b = 13.65f;
byte c = 0x4a;
```

​		然而，在实际开发过程中，我们经常会遇到需要使用对象，而不是内置数据类型的情形。为了解决这个问题，Java 语言为每一个内置数据类型提供了对应的包装类。

​		所有的包装类**（Integer、Long、Byte、Double、Float、Short）**都是抽象类 Number 的子类。

| 包装类    | 基本数据类型 |
| :-------- | :----------- |
| Boolean   | boolean      |
| Byte      | byte         |
| Short     | short        |
| Integer   | int          |
| Long      | long         |
| Character | char         |
| Float     | float        |
| Double    | double       |

​		当运用包装类时，声明的都是对象。这种由编译器特别支持的包装称为装箱，所以当内置数据类型被当作对象使用的时候，编译器会把内置类型装箱为包装类。相似的，编译器也可以把一个对象拆箱为内置类型。Number 类属于 java.lang 包。

## Integer实例

> ```java
> public class Test {
>     public static void main(String[] args){
>         Integer x=5;
>         x=x+10;
>         System.out.println(x);
>     }
> }
> ```
>
> ​		当 x 被赋为整型值时，由于x是一个对象，所以编译器要对x进行装箱。然后，为了使x能进行加运算，所以要对x进行拆箱。
>
> ![包装类](C:\雨鱼\NaNaOY.github.io\img\包装类.png)

## Java Math 类

> ```java
> System.out.println("90度的正弦值："+Math.sin(Math.PI/2));
> System.out.println("0度的余弦值："+Math.cos(0));
> System.out.println("60度的正切值：" + Math.tan(Math.PI/3));
> System.out.println("1的反正切值： " + Math.atan(1));
> System.out.println("π/2的角度值：" + Math.toDegrees(Math.PI/2));
> System.out.println(Math.PI);
> ```

