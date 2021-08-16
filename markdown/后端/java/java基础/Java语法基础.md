## Java对标识符的规范

Java 所有的组成部分都需要名字。类名、变量名以及方法名都被称为标识符。

关于 Java 标识符，有以下几点需要注意：

- 所有的标识符都应该以字母（A-Z 或者 a-z）,美元符（$）、或者下划线（_）开始
- 首字符之后可以是字母（A-Z 或者 a-z）,美元符（$）、下划线（_）或数字的任何字符组合
- 关键字不能用作标识符
- 标识符是大小写敏感的
- 合法标识符举例：age、$salary、_value、__1_value
- 非法标识符举例：123abc、-salary

---



### 规范标识符的重要性

> 没有规范的命名不是一个科班出身的软件开发者应有的行为。

### Java标识符规范

> 类名称：Mammal
>
> 函数名：getAge
>
> 常量：MAX_HEIGHT

### 标识符起名

>  应该尽量做到"望名知义"。

---

## Java修饰符

像其他语言一样，Java可以使用修饰符来修饰类中方法和属性。主要有两类修饰符：

- 访问控制修饰符 : default, public , protected, private
- 非访问控制修饰符 : final, abstract, static, synchronized

在后面的章节中我们会深入讨论 Java 修饰符。

---

## Java 变量

Java 中主要有如下几种类型的变量

- 局部变量
- 类变量（静态变量）
- 成员变量（非静态变量）

---



##  Java常量的常用数据类型

### 整形

> byte（8）、short（16）、int（32）、long（64）

### 浮点型

> float(32)、double（64）

### 使用科学计数法定义浮点数值

> 123.456=1.23456e+2

### 布尔型

> true,false

### 字符型

> 'a','A'

### 字符串

> "Hello,China"

---



### 定义常量

> ​	利用关键字final声明常量，对于全局的常量（即在整个项目中都可用），通常用一下模式声明：
>
> public static **final** int MAX_VALUE=512
>
> ​	如果某常量只在本类使用，则应将其定义为private的。常量名字通常采用大写字母

## 数据类型

### 原始数据类型与类

> ​	int、float等这些数据类型称为"原始数据类型（primitive type）"。Java中除了int、float等少数几个数据类型，区域的数据类型都用来引用对象

### 枚举类型

> ```java
> 定义：*enum Size{SMALL,MEDIUM,LARGE}
> 使用：Size s=Size.SMALL;
> //从字串转换为枚举
> Size t=Size.valueof("SMALL");
> ```

### 枚举值的foreach迭代

> ```java
> private enum  MyEnum{
> 		ONE,TWO,THREE
> }
> public static viod main(String[] args){
> 		for(MyEnum value:MyEnum.values()){
> 				System.out.println(value);
> 		}
> }
> ```
>
> 注意：枚举可以用于switch语句中,枚举类型是引用类型。

---

## Java关键字


