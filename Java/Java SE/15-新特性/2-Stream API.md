# 什么是Stream

Stream到底是什么呢？ 

是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。 “集合讲的是数据，Stream讲的是计算！” 

注意： 

①Stream 自己不会存储元素。 

②Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。 

③Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。

# Stream操作的三个步骤

![image-20230104142433998](2-Stream%20API.assets/image-20230104142433998.png)

## 创建

一个数据源（如：集合、数组），获取一个流

下面是创建Stream的方式

### 集合

Java8 中的 Collection 接口被扩展，提供了两个获取流 的方法：

- `default Stream stream()` : 返回一个顺序流

- `default Stream parallelStream()`: 返回一个并行流

### 数组

Java8 中的 Arrays 的静态方法 stream() 可以获取数组流： 

- `static  Stream stream(T[] array)`: 返回一个流

重载形式，能够处理对应基本类型的数组：

- `public static IntStream stream(int[] array) `

- `public static LongStream stream(long[] array) `

- `public static DoubleStream stream(double[] array)`

### Stream的of()

可以调用Stream类静态方法 of(), 通过显示值创建一个 流。它可以接收任意数量的参数。 

- `public static Stream of(T... values)` : 返回一个流

### 创建无限流

可以使用静态方法 Stream.iterate() 和 Stream.generate(),  创建无限流。 

- 迭代 `public static Stream iterate(final T seed, final UnaryOperator f)  `

- 生成 `public static Stream generate(Supplier s)`

![image-20230104142923184](2-Stream%20API.assets/image-20230104142923184.png)



## 中间操作

一个中间操作链，对数据源的数据进行处理

多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止 操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全 部处理，称为“惰性求值”。

### 筛选与切片

![image-20230104143001523](2-Stream%20API.assets/image-20230104143001523.png)

### 映射

![image-20230104143013169](2-Stream%20API.assets/image-20230104143013169.png)

### 排序

![image-20230104143023288](2-Stream%20API.assets/image-20230104143023288.png)





## 终止操作

一旦执行终止操作，就执行中间操作链，并产生结果。之后，不会再被使用

终端操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如：List、Integer，甚至是 void 。

 流进行了终止操作后，不能再次使用。

### 匹配和查找

![image-20230104143057487](2-Stream%20API.assets/image-20230104143057487.png)

![image-20230104143117987](2-Stream%20API.assets/image-20230104143117987-16728138786483.png)

### 归约

![image-20230104143139186](2-Stream%20API.assets/image-20230104143139186.png)

注：map 和 reduce 的连接通常称为 map-reduce 模式，因 Google  用它来进行网络搜索而出名

### 收集

![image-20230104143158286](2-Stream%20API.assets/image-20230104143158286.png)

Collector 接口中方法的实现决定了如何对流执行收集的操作(如收集到 List、Set、 Map)。 

另外， Collectors 实用类提供了很多静态方法，可以方便地创建常见收集器实例， 具体方法与实例如下表：

![image-20230104143219746](2-Stream%20API.assets/image-20230104143219746.png)

![image-20230104143231606](2-Stream%20API.assets/image-20230104143231606.png)

