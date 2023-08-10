# 数组与枚举

## 数组

```java
package com.java.iweb.day20230325;

import java.util.ArrayList;

public class ArrList {
    public static void main(String[] args) {
        int width=12,high=8,n=15;
        int[] matrix =new int[width*high];

        //        置零
        for (int i = 0;i<matrix.length;i++){
            matrix[i]=0;
        }

        // 通过src和dst的数列生成一组不重复的随机整数做地雷下标
        ArrayList<Integer> src=new ArrayList<>();
        ArrayList<Integer> dst=new ArrayList<>();
        for (int i=0;i<20;i++){
            src.add(i);
        }
        int num =10;
        for (int i=0;i<10;i++){
            int id=(int)(Math.random()*src.size());
            dst.add(src.remove(id));
        }

        // 取出下下标置为1
        for (int y=0;y<high;y++){
            
        }

        String line="";
        for (int i =0;i<dst.size();i++){
            line+=dst.get(i)+" ";
        }
        System.out.println(line);
    }
}
```

## ArrayList
Java ArrayList 是一个动态数组，它是 Java 集合框架的一部分。ArrayList 实现了 List 接口，允许我们存储和操作元素集合。相对于固定大小的数组，ArrayList 的大小可以动态地增长和缩小。以下是一些使用 Java ArrayList 的常见方法和操作。

1. 创建 ArrayList：要创建一个 ArrayList，需要使用泛型来指定元素的类型。例如，要创建一个用于存储整数的 ArrayList，可以使用以下代码：

```java
import java.util.ArrayList;

ArrayList<Integer> arrayList = new ArrayList<>();
```

2. 添加元素：可以使用 `add` 方法将元素添加到 ArrayList 的末尾。例如：
```java
arrayList.add(10);
arrayList.add(20);
arrayList.add(30);
```
3. 访问元素：可以使用 `get` 方法通过索引访问 ArrayList 中的元素。例如：
```java
int firstElement = arrayList.get(0); // 获取第一个元素，即 10
```

4. 修改元素：可以使用 set 方法修改 ArrayList 中指定索引的元素。例如：

```java
arrayList.set(1, 25); // 将索引为 1 的元素修改为 25
```

5. 删除元素：可以使用 remove 方法删除指定索引的元素。例如：
```java
arrayList.remove(0); // 删除索引为 0 的元素
```

6. 获取 ArrayList 大小：可以使用 `size` 方法获取 ArrayList 中的元素数量。例如：
int size = arrayList.size(); // 获取 ArrayList 的大小
7. 检查 ArrayList 是否为空：可以使用 isEmpty 方法检查 ArrayList 是否为空。例如：
boolean isEmpty = arrayList.isEmpty(); // 检查 ArrayList 是否为空

8. 查找元素：可以使用 indexOf 和 lastIndexOf 方法查找 ArrayList 中指定元素的索引。例如：
int index = arrayList.indexOf(25); // 查找元素 25 在 ArrayList 中的索引
9. 遍历 ArrayList：可以使用 for 循环或增强型 for 循环遍历 ArrayList 中的元素。例如：
```java
// 使用 for 循环
for (int i = 0; i < arrayList.size(); i++) {
    System.out.println(arrayList.get(i));
}

// 使用增强型 for 循环
for (Integer element : arrayList) {
    System.out.println(element);
}
```
10. 清空 ArrayList：可以使用 clear 方法清空 ArrayList 中的所有元素。例如：
```java
arrayList.clear(); // 清空 ArrayList
```

## Scanner
Java Scanner 类是 java.util 包中的一个实用类，用于从不同输入源（如键盘、文件、字符串等）读取和解析各种数据类型。以下是使用 Java Scanner 的常见方法和操作。

1. 导入 Scanner 类：要在代码中使用 Scanner 类，首先需要导入 java.util 包中的 Scanner 类。例如：
```java
import java.util.Scanner;
```
2. 创建 Scanner 对象：创建 Scanner 对象时，需要指定输入源。最常见的输入源是键盘输入，可以通过将 System.in 传递给 Scanner 构造函数来创建用于读取键盘输入的 Scanner 对象。例如：
```java
Scanner scanner = new Scanner(System.in);
```
3. 读取整数：可以使用 nextInt 方法读取整数输入。例如：
```java
System.out.print("请输入一个整数：");
int number = scanner.nextInt();
System.out.println("输入的整数是：" + number);
```
4. 读取浮点数：可以使用 nextFloat 或 nextDouble 方法读取浮点数输入。例如：
```java
System.out.print("请输入一个浮点数：");
double decimalNumber = scanner.nextDouble();
System.out.println("输入的浮点数是：" + decimalNumber);
```
5. 读取字符串：可以使用 next 方法读取一个单词，或使用 nextLine 方法读取一行文本。例如：
```java
System.out.print("请输入一个单词：");
String word = scanner.next();
System.out.println("输入的单词是：" + word);

System.out.print("请输入一行文本：");
String line = scanner.nextLine();
System.out.println("输入的文本是：" + line);
```
6. 读取布尔值：可以使用 nextBoolean 方法读取布尔值输入。例如：
```java
System.out.print("请输入一个布尔值（true/false）：");
boolean flag = scanner.nextBoolean();
System.out.println("输入的布尔值是：" + flag);
```
7. 检查是否还有输入：可以使用 hasNext、hasNextInt、hasNextDouble 等方法检查是否还有下一个输入。例如：
```java
System.out.print("请输入一个整数：");
while (scanner.hasNextInt()) {
    int number = scanner.nextInt();
    System.out.println("输入的整数是：" + number);
    System.out.print("请输入一个整数：");
}
```
8. 关闭 Scanner：在使用完 Scanner 之后，应关闭 Scanner 以释放相关资源。可以使用 close 方法关闭 Scanner。例如：
```java
scanner.close();
```
注意：如果 Scanner 用于从文件或其他外部资源读取输入，关闭 Scanner 尤为重要，以避免资源泄漏。