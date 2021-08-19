# 数组

---

## 声明数组变量

​	**必须声明数组变量，才能在程序中使用数组。**

   - dataType[ ] arrayRefVar;  //首选声明   例如double[ ] myList
   - dataType arrayRefVar[ ];  //相同效果，不是首选  例如double myList[ ]



## For-Each循环

​	**JDK 1.5 引进了一种新的循环类型，被称为 For-Each 循环或者加强型循环，它能在不使用下标的情况下遍历数组。****

```java
for(type element:array){
	System.out.println(element);
}
```

### 	实例：

用实例来显示数组myList中的所有元素，并且数组可以作为函数的参数

```java
public class TestArray_one {
    public static void main(String[] args) {
        double[] myList = {1.9, 2.9, 3.4, 3.5};
        int[] array = {1, 2, 4, 3, 5, 6};
        //打印所有数组元素
        for (double element : myList) {
            System.out.print(element + " ");
        }
        System.out.println();
        TestArray_one.printArray(array);
    }

    public static void printArray(int[] array) {
        for (int i = 0; i < array.length; i++) {
            System.out.print(array[i] + " ");
        }
    }
}
```

## 

## Arrays类

​	**java.util.Arrays 类能方便地操作数组，它提供的所有方法都是静态的。**

​	具有以下功能：

   -  给数组赋值：通过 fill 方法。
   -  对数组排序：通过 sort 方法,按升序。
   -  比较数组：通过 equals 方法比较数组中元素值是否相等。
   -  查找数组元素：通过 binarySearch 方法能对排序好的数组进行二分查找法操作。

| 序号 | 方法和说明                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **public static int binarySearch(Object[] a, Object key)** 用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 (-(*插入点*) - 1)。 |
| 2    | **public static boolean equals(long[] a, long[] a2)** 如果两个指定的 long 型数组彼此*相等*，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 |
| 3    | **public static void fill(int[] a, int val)** 将指定的 int 值分配给指定 int 型数组指定范围中的每个元素。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 |

