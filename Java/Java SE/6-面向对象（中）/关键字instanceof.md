# 关键字instanceof
x  instanceof A ：检验x是否为类A的对象，返回值为Boolean型。如果是，返回true；如果不是，返回false。

 要求x所属的类与类A必须是子类与父类的关系。否则编译错误

 如果x属于类A的子类B，x instanceof A的值也为true

 

使用情景：为了避免在向下转型是出现异常，先进行instanceof判断，一旦为true则向下转型；若不是则为false。

如，类B是类A的父类，如果a instanceof A返回true，则a instanceof B也返回true。

# 向下转型时出现的问题：
问题一：编译时通过，运行时不通过。

```java
//例一
Person p3=new Woman();
Man m3=(Man)p3;
//例二：
Person p4=new Person();
Man m4=(Man)p4;
```
问题二：编译时通过，运行时也通过。

```java
Object obj=new Woman;
Person p1=(Person)obj;
```
问题三：编译不通过。

```java
Man m5=new Woman;
```
**右边应为左边的子类**

