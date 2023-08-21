# toString方法
toString方法在Object类中定义，其返回值是String类型，返回类名和它的引用地址

​	在进行String与其他类型数据的连接操作时，自动调用toString（）方法

```java
Date now=new Date();
System.out.println("now="+now);//相当于
System.out.println("now="+now.toString());
```
可以根据需要在用户自定义类型中==重写toString（）方法==

​	如String类重写了toString（）的方法，返回字符串的值

```java
si="Hello";
System.out.println(s1);//相当于System.out.println(s1.toString());
```
基本数据类型==转换为String类型==时，调用了对于包装类的toString（）方法。即：

```java
int a=10; System.out.println("a="+a);
```
Object类中toString（）的使用：

​	1、当我们输出一个对象的引用时，实际上是调用当前对象的toString（）

​	2、Object类中toString（）的定义：

```java
public String toString(){
    return getClass().getName()+"@"+Integer.toHexString(hashCode());
}
```
​	3、像String、Date、File、包装类等都重写了Object类中的toString（）方法，使得在调用对象的toString（）方法时，返回“实体内容”信息

​	4、自定义类也可以重写toString（）方法，当调用此方法时，返回对象的“实体内容”

以Customer类中的name和age属性为例：

```java
public String toString(){
    return "Customer[name="+name+",age="+age+"]"
}
```
