### Java SE API中的两种包

#### 核心包：

>  以"Java"开头，由JDK所直接提供

#### 扩展包：

> 以"Javax"开头

#### 第三方包：

> 通常以"反写"的公司网址开头，这是一种约定，比呢非强制性的，比如某包名为：com.example，它实际上代表一家公司，其网址通常为：http://www.example.com。

### 方法使用

- 调用类JOptionPane的showMessageDialog方法显示消息框。
- 注意：**方法名开头字母小写，与类名的要求不同。**
- showMessageDialog 是一个静态（static）方法
- 静态方法用 "类名.方法名（参数列表）"的形式调用

```java
//Printing multiple lines in a dialog box
//import class JOptionPane
import javax.swing.JOptionPane;
public class Welcome2 {
    public static void main(String[] args){ 		
      	JOptionPane.showMessageDialog(null,"Welcome\nto\nJava\nPrograming!");
        System.exit(0);	//terminate the program
    }
}
```



### 静态方法

> 无须创建对象即可使用的方法。
>
> 其中System为静态类，exit为静态方法表示结束程序。参数0表示程序顺利结束，非0表示有错误发生。
>
> 类System属于包java.lang，这是一个核心包，自动被每个Java程序所引入，不需要import语句就可以直接使用。
>
> 

```java
        System.exit(0);	//terminate the program
```

# [返回首页](https://nanaoy.github.io/)