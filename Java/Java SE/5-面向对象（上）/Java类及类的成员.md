# Java类及类的成员

主要内容：

1、Java类及类的成员：属性、方法、构造器；代码块、内部类

2、面向对象的三大特征：封装、继承、多态

3、关键字：this、super、static、final、abstract、interface、package、import

当代码较多时，面向对象比面向过程方便

---
# 类和对象

- 类（class）：对一类事物的抽象描述，是构造对象的模板或蓝图。

- 对象：实际存在的该类事物的每个个体，也被称为实例（instance）

  由类构造(construct)对象的过程称为创建类的实例(instance)

- ==封装==(encapsulation)是处理对象的一个重要概念。从形式上看，封装就是将数据和行为组合在一个包中，并对对象的使用者隐藏具体的实现方式，操作数据的过程称为==方法==(method)

- 所有的类都源自一个“神通广大的超类”，它就是Object，所有其他类都拓展自这个Object类，扩展后的新类具有被扩展的类的全部属性和方法。通过扩展一个类来建立另外一个类的过程称为==继承==(inheritance)

面向对象程序设计的重点是**类的设计**。

设计类，就是设计**类的成员**

## 类之间的关系

在类之间，最常见的关系有：

- 依赖("uses-a")
- 聚合("has-a")
- 继承("is-a")

==依赖：==一个类的方法使用或操纵另一个类的对象，我们就说一个类依赖另一个类，应该尽可能地将相互依赖的类减至最少。这里的关键是，如果类A不知道B的存在，它就不会关心B的任何改变（==这意味着B的改变不会导致A出现任何bug==），就是尽可能减少类的耦合

==聚合：==例如：一个Order对象包含一些Item对象。包含关系意味着类A的对象包含类B的对象

---
常见的类的成员：

​	属性：对应类中的成员变量

​	行为：对应类中的成员方法

==Field=属性=成员变量   Method=（成员）方法=函数==

##  对象与对象变量

**Java类的实例化，即创造类的对象**：

使用构造器(constructor，或称构造函数)构造新实例

格式：类名 对象名=new 类名（）；例如：

```java
Person p1=new Person();
```
**p1是一个对象变量**

**调用属性**：格式：“对象.属性”

**调用方法**：格式：  "对象.方法"

如果创建了一个类的多个对象，则每个对象拥有一套相互独立的属性，若我们修改一个对象的属性则不影响其他对象

注意：对象变量并没有实际包含一个对象，它只是==引用==一个对象

---
## 属性（成员变量）与局部变量的区别：

不同点：

1. 在类中声明的位置不同：

				属性：定义在一对{ }内
		
		成员变量：声明在方法内、方法形参、代码块内、构造器形参、构造器内部的变量

```java
public void talk(String language){}  //language:形参
public void eat(){
    String food//局部变量
}
```
2. 关于权限修饰符的不同：

   ​		属性：可以在声明属性时，指明其权限，使用权限修饰符。

   ​		==常用权限修饰符：private、public、缺省、protected==

   ​		局部变量：不可以使用权限修饰符

3. 默认初始化值的情况：

   ​		属性：有默认初始化值：整型（0）、浮点型（0.0）、字符型（0或 '\\u0000' ）、布尔型（false）

   ​			    引用数据类型（类、数组、接口）：null

   ​		局部变量：没有默认初始化值，在调用局部变量之前一定要显示赋值。

   ​		特别的：形参在调用时，赋值即可。

4. 在内存中加载的位置：

	属性：加载到堆空间中
	局部变量：加载到栈空间中

类中方法的调用和使用：

![image-20230908172608760](Java%E7%B1%BB%E5%8F%8A%E7%B1%BB%E7%9A%84%E6%88%90%E5%91%98.assets/image-20230908172608760.png)

方法的声明：

```java
权限修饰符+返回值类型+方法名（形参列表）{

  	方法体

}
```

返回值类型：

如果方法有返回值则必须在方法声明时，**指定返回值的类型**。同时，方法中需要使用return关键字来返回指                      定类型的变量或常量：“return 数据；”

如果没有返回值，则方法声明时，使用void来表示；如要使用return则只能“return；”来表示结束方法的意思



形参列表：方法可以声明0个，1个，或多个形参

     **格式：**数据类型1 形参1，数据类型2 形参2，……

方法的使用：

​		可以调用当前类的属性和方法

​		特殊的：方法A中调用方法A（递归方法）

==注：方法中不可以定义方法==

## 用var声明局部变量

在Java10中，如果可以从变量的初始值推导出他们的类型，那么可以用var关键字声明局部变量，而无须指定类型。例如：

```java
var harry=new Employee("Harry Hacker",50000,1989,10,1)
```

>  注意:var关键字只能用于方法中的局部变量

## 使用null引用

一个对象变量包含一个对象的引用，或者包含一个特殊值null，后者表示没有引用任何对象。

听上去这是一种处理特殊情况的便捷机制，如未知的名字或雇用日期。不过使用null值的时候要小心：

​	如果对null值应用一个方法，会产生一个NullPointerException异常

```java
LocalDate birthday=null;
String s=birthday.toString();//NullPointerException
```

这是一个很严重的错误，类似于“索引越界“异常。如果你的程序没有捕获异常，程序就会终止。

对此有两种解决方法：

- ”宽容型“方法

    把null参数转换为一个适当的非null值

    ```java
    if(n==null) name="unknow";else name=n;
    ```

    在Java9中，Object类对此提供了一个便利方法：

    ```java
    name=Ojects.requireNonNullElse(n,"unknow")l
    ```

- ”严格型“

    直接拒绝null参数

    ```java
    Objects.requireNonNull(n,"The name cannot be null");
    ```

    

---

 **Java定义随机数：**

​		`(Math.random()\*(max-min)+min)`

​		注：返回的为double型的数值需要转换数据类型

---
理解”万事万物皆对象“：

1. 在Java语言范畴中，我们将功能、结构等封装到类中，通过类的实例化，来调用具体的功能结构

   2. 涉及到Java语言与前端HTML、后端的数据库交互时，前后端的结构在Java层面交互时，都体现为类和对象

---

## 匿名对象

即创建的对象没有显式的赋给一个变量名而直接调用类中的属性和方法。

      匿名对象只能调用一次。例：

```java
//创建一个phone类
class phone{
    double price;
    int showprice;
}                              
    new phone().price;
    new phone().showprice;
```
==**注：两者虽调用同一个类中的属性但两者并不是同一个对象**==

使用：当调用的方法需要输入形参时，课直接输入匿名对象（new 类名）

---
## 方法的重载

即在一个类中，允许存在一个以上的重名方法，只有它们的参数个数或参数类型不同即可

   特点：与返回值类型无关，只看参数列表，且参数列表必须不同。（参数个数或参数类型）调用时，根据方法参数列表的不同来区分。示例：

```java
//返回两个整数的值
int add(int x,int y){return x+y;}
//返回三个整数的和
int add(int x,int y ,int z){return x+y+z;}
```
可变个数的形参：格式：数据类型 . . . 变量名。例：

```java
public void show(String . . . strs){   }
```
- 可以直接定义能与多个实参想匹配的形参（没有指定形参数据类型时才调用） 
- 可变个数形参的方法与本类中方法名相同，形参不同的方法之间构成重载 
- 可变个数形参的方法与本类中方法名相同，形参类型也相同的数组之间不构成重载，即二者不能同时存在 
-  可变个数的形参在方法的形参中，**必须声明在末尾，且最多只能声明一个可变形参**。 

方法参数的值传递机制：

​		**方法，必须由其所在类或对象调用才有意义。若方法含参数：**

​		**形参：**方法声明时的参数

​		**实参：**方法调用时实际传给形参的参数值

==Java的实参值如何传入方法==：

- Java里方法的参数传递方式只有一种：**值传递**。即将实际参数值的复制品传入方法内，而参数本身不受影响。
- 形参是基本数据类型：将实参基本数据类型变量的“数据值”传递给形参
- 形参是引用数据类型：将实参引用数据类型变量的“地址值”传递给形参



# 日历小程序

```java
package com.java;

import java.time.DayOfWeek;
import java.time.LocalDate;

public class 日历 {
    public static void main(String[] args) {
        LocalDate date=LocalDate.now();
        int month=date.getMonthValue();
        int today=date.getDayOfMonth();

        date=date.minusDays(today-1);
        DayOfWeek weekday=date.getDayOfWeek();
        int value= weekday.getValue();

        System.out.println("Mon Tue Wed Thu Fri Sat Sun");
        for (int i = 1; i < value; i++) {
            System.out.print("    ");
        }
        while (date.getMonthValue()==month){
            System.out.printf("%3d",date.getDayOfMonth());
            if (date.getDayOfMonth()==today){
                System.out.print("*");
            }else {
                System.out.print(" ");
            }
            date=date.plusDays(1);
            if (date.getDayOfWeek().getValue()==1){
                System.out.println();
            }
        }
        if(date.getDayOfWeek().getValue()!=1){
            System.out.println();
        }
    }
}

```

运行结果：

![image-20230908172646315](Java%E7%B1%BB%E5%8F%8A%E7%B1%BB%E7%9A%84%E6%88%90%E5%91%98.assets/image-20230908172646315.png)

```java
LocalDate newYearsEve=LocalDate.of(1999,12,31);
```

一旦有了一个LocalDate对象，可以用方法getYear、getMonthValue和getDayOfMonth得到年、月和日

```java
int year=newYearsEve.getYear();
int month=date.getMonthValue();
int today=date.getDayOfMonth();
```

- static LocakDate now()构造一个表示当前日期的对象

- static LocalDate of(int year,int month,int day)构造一个表示给定日期的对象

- 得到当前日期的年、月和日

  - int getYear()
  - int getMonthValue()
  - int GetDayOfMonth()

- DayOfWeek getDayOfWeek

  得到当前日期是星期几，作为DayOfWeek类的一个示例返回。调用getValue来得到1~7之间的数，表示这是星期几，1表示星期几，7表示星期日

- 生成当前日期之后或之前n天的日期

  - LocalDate plusDays(int n)	之后
  - LocalDate minusDays(int n)    之前 