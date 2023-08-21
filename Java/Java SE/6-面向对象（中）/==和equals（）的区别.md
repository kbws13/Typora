# \==和equals（）的区别
\==：运算符

​		1、可以使用在基本数据类型变量和引用数据类型变量中

​		2、如果比较的是基本数据类型变量：比较两个变量保存的数据是否相同（不一定数据类型相同）如果比较的是引用数据类型变量：比较两个对象的地址值是否相同，即两个对象是否指向同一个对象实体

equals（）方法的使用：

​		1、只是一个方法，而非运算符

​		2、只能适用于引用数据类型

​		3、Object类中equals（）的定义：

```java
public boolean equals(Object obj){
    return(this==obj);
}
```
​		4、像String、Date、File、包装类等都重写了Object类中的equals（）方法，重写之后比较是实体内容是否相同

说明：Object类中定义的equals（）方法和==的作用是相同的：比较两个对象的地址值是否相同，即两个对象是否指向同一个对象实体

例：

```java
Customer cust1=new Customer("Tom",21);
Customer cust2=new Customer("Tom",21);
System.out.println(cust1==cust2);//false
String str1=new String("atguigu");
String str2=new String("atguigu");
System.out.println(str1==str2);//false
System.out.println(cust1.equals(cust2));//false
System.out.println(str1.equals(str2));//true
```
# 重写equals（）方法：
重写的原则：比较两个对象的实体内容是否相同（例如name和age）

```java
public boolean equals(Object obj){
    if(this==obj){
        return true;
    }
    if(obj instanceof Customer){
	Customer cust=(Customer)obj;
	//比较两个对象的每个属性是否相同
	if(this.age==cust.age&&this.name.equals(cust.name)){
	    return true;
	}else{
	    return false;
	}
    }
    return false;
    //或
    return this.age==cust.age&&this.name.equals(cust.name);
}
```
