# 例子

![image-20230104141320039](1-Lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F.assets/image-20230104141320039.png)

![image-20230104141329836](1-Lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F.assets/image-20230104141329836.png)

# 语法

Lambda 表达式：在Java 8 语言中引入的一种新的语法元素和操 作符。这个操作符为 “->” ， 该操作符被称为 Lambda 操作符 或箭头操作符。它将 Lambda 分为两个部分：

左侧：指定了 Lambda 表达式需要的参数列表

右侧：指定了 Lambda 体，是抽象方法的实现逻辑，也即 Lambda 表达式要执行的功能。

![image-20230104141753081](1-Lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F.assets/image-20230104141753081.png)

![image-20230104141802440](1-Lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F.assets/image-20230104141802440.png)

## 类型判断

上述 Lambda 表达式中的参数类型都是由编译器推断得出的。Lambda  表达式中无需指定类型，程序依然可以编译，这是因为 javac 根据程序 的上下文，在后台推断出了参数的类型。Lambda 表达式的类型依赖于 上下文环境，是由编译器推断出来的。这就是所谓的“类型推断”。

![image-20230104141958312](1-Lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F.assets/image-20230104141958312.png)

