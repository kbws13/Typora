# 包装类（Wrapper）的使用

![image-20220719102743376](%E5%8C%85%E8%A3%85%E7%B1%BB%EF%BC%88Wrapper%EF%BC%89%E7%9A%84%E4%BD%BF%E7%94%A8.assets/image-20220719102743376.png)

![image-20220719102746544](%E5%8C%85%E8%A3%85%E7%B1%BB%EF%BC%88Wrapper%EF%BC%89%E7%9A%84%E4%BD%BF%E7%94%A8.assets/image-20220719102746544.png)

1、Java提供了8种基本数据类型对应的包装类，使得基本数据类型的变量具有类的特征

2、掌握：基本数据类型、包装类、String三者之间的转换

==基本数据类型----->包装类==：调用包装类的构造器：

```java
public void test1(){																		    int num1=10;
   Integer int1=new Integer(num1);															
}
```
==包装类------>基本数据类型==：调用包装类Xxx的xxxValue（）：

```java
public void test2(){																			    Integer in1=new Integer(12);																int i1=int1.intValue();																		System.out.println("i1+1")											
}
```
==基本数据类型、包装类---->String类型==:方法1：连接运算：String  str=num1+"";

​								 方法2：调用String重载的valueof（Xxx xxx）：

```java
float f1=12.3f;																				String str2=String.valueof(f1);
```
==String类型--->基本数据类型、包装类==：调用包装类的parseXxx(String s)方法：

```java
String str1="123";
int num2=Integer.parseInt(str1);
```
# 新特性：
自动装箱：例：

```java
int num2=10;
Integer in1=num2;
```
自动拆箱：例：

```java
System.out.println(in1.toString());
int num3=in1;
```
# 包装类面试题：
```java
public void test(){
	Integer i=new Integer(1);
	Integer j=new Integer(1);
	System.out,println(i==j);//false
	Integer m=1;
	Integer n=1;
	System.out,println(m=n);//true
	Integer x=128;//相当于new了一个Integer对象
	Integer y=128;//相当于new了一个Integer对象
	System.out,println(x=y);//false
}
```
注：Integer内部定义了IntegerCache结构，IntegerCache中定义了Integer\[ \]，保持了-128\~127范围的整数。如果我们使用自动装箱的方式，给Integer赋值的范围在-128\~127范围内时，可以直接使用数组中的元素，不用再去new。目的：提高效率。