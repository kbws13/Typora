# final修饰类和方法
final可以用来修饰的结构：类、方法、变量

①final修饰一个类：此类不能被其他类所继承

​	比如：String类、System类、StringBuffer类

②final修饰一个方法：表明此方法不可被重写

​	比如：Object类中的getClass（）；

③final修饰变量：此时的“变量”就称为时一个常量

​    final修饰属性：可以考虑赋值的位置有：显式初始化、代码块、代码块中初始化、构造器中初始化

​    final修饰局部变量：尤其是使用final修饰形参时，表明此形参是一个常量。当我们调用此方法时，给常量形参赋一个实参。一但赋值以后，就只能在方法体内使用此形参，但不能进行重新赋值

 

static final 用来修饰属性：全局常量

