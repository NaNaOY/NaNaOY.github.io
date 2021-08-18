# Pom.xml文件配置

---

### pom文件基础配置

>  在每次配置完pom文件之后只需**点击**![maven刷新](../../../../img/maven刷新.png)即可。
>
> ```javascript
> <?xml version="1.0" encoding="UTF-8"?>
> <project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
>     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
>   <modelVersion>4.0.0</modelVersion>
> 
>   <groupId>com.company.work1</groupId>
>   <artifactId>work1</artifactId>
>   <version>1.1</version>
> 
> <packaging>jar</packaging>
> <properties>
>     ...
> </properties>
> 
> <dependencies>
>     <dependency>
>         <groupId>commons-logging</groupId>
>         <artifactId>commons-logging</artifactId>
>         <version>1.2</version>
>     </dependency>
> </dependencies>
> 
>   <build>
>     <plugins>
>       <plugin>
>         <groupId>org.apache.maven.plugins</groupId>
>         <artifactId>maven-compiler-plugin</artifactId>
>         <configuration>
>           <source>1.7</source>
>           <target>1.7</target>
>         </configuration>
>       </plugin>
>     </plugins>
>   </build>
> 
>  </project>
> ```
>
> 



### Pinyin4j

>  其中1-8行几乎一样，10-13行properties可以自行添加，之后到了重要的**dependencies模块**。这里用pinyin4j举例，在pom文件中配置下面的dependency模块，用maven自动引入后就可以使用汉字拼音互相转化的功能。
>
> ```javascript
> 	<dependency>
>     	<groupId>net.sourceforge.pinyin4j</groupId>
>  		<artifactId>pinyin4j</artifactId>
>  		<version>2.5.0</version>
> 	</dependency>
> ```

### 解决 -source 7版本以上报错问题

> 1_000_000数字中需要java -source到达7以上版本才可使用，解决方法在**dependencies模块**下加入以下**build模块**即可正常使用
>
> ```javascript
> 	  <build>
>     <plugins>
>       <plugin>
>         <groupId>org.apache.maven.plugins</groupId>
>         <artifactId>maven-compiler-plugin</artifactId>
>         <configuration>
>           <source>1.7</source>
>           <target>1.7</target>
>         </configuration>
>       </plugin>
>     </plugins>
>   </build>
> ```

