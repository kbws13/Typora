# 关键字this
# 关键字this的使用
它在方法内部使用，即这个方法所属对象的引用

它在构造器内部使用，表示该构造器正在初始化的对象

---
this表示当前对象，可以调用类的属性、方法和构造器

在类的方法中，可以使用"this.属性"或"this.方法"的方式，调用当前对象的属性或方法。但是通常情况下选择省略"this."如果方法的形参与类的属性同名时，我们必须显式的使用"this.变量"的方式，表明此变量是属性而非形参。

什么时候使用this关键字：

​			当在方法中需要用到调用该方法的对象时，就用this

​			具体的：我们可以用this区分局部变量和属性

​			比如：this.name=name;

```java
		class Person{
			private String name;
			private int age;
			public void setName(String name){
				this.name=name
			}
			public String getName(){
				return name;
			}
			public void setAge(int age){
				this.age=age;
			}
			public int getAge(){
				return age;
			}
		}
//		其中this.name指的是当前对象的name属性
```
# this调用构造器：
  1、我们在类的构造器中可以显式的使用"this(形参列表)"方式，调用本类中的其他构造器

  2、构造器不能调用自己

  3、如果一个类中有n个构造器，则最多有n+1个构造器中使用了"this(形参列表)"

  4、规定:"this(形参列表)"必须声明在当前构造器的首行

  5、构造器内部，最多只能声明一个"this(形参列表)"，来调用其他的构造器