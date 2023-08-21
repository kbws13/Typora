# Java比较器

在Java中经常会涉及到对象数组的排序问题，那么就涉及到对象之间的比较问题

Java实现对象排序的方式有两种：

- 自然排序：`java.lang.Comparable`
- 定制排序：`java.util.Comparator`

## Comparable接口

Comparable接口的使用举例：

1. 像String、包装类等实现了Comparable接口，重写了compareTo方法，给出了两个对象大小的方式

2. 像String、包装类重写CompareTo方法以后

3. 重写compareTo方法的规则：

    如果当前对象this大于形参对象obj，则返回正整数，

    如果当前对象this小于形参对象obj，则返回负整数，

    如果当前对象this等于形参对象obj，则返回零

4. 对于自定义类来说，如果需要排序，可以让自定义类实现Comparable接口，重写compareTo方法，在compareTo方法中指明如何排序

![image-20230102140152417](3-Java%E6%AF%94%E8%BE%83%E5%99%A8.assets/image-20230102140152417.png)

Goods类

```java
package xyz.kbws;

/**
 * @author hsy
 * @date 2023/1/2
 */
public class Goods implements Comparable{
    private String name;
    private double price;
    public Goods() {
    }
    public Goods(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Goods{" +
                "name='" + name + '\'' +
                ", price=" + price +
                '}';
    }

    @Override
    public int compareTo(Object o) {
        if(o instanceof Goods){
            Goods goods = (Goods) o;
            //方式一
            if(this.price>goods.price){
                return 1;
            }else if (this.price< goods.price){
                return -1;
            }else {
                return 0;
            }
            //方式二
            //return Double.compare(this.price, goods.price);
        }
        throw new RuntimeException("传入数据类型不一致!");
    }
}
```

Test

```java
@Test
public void Test2(){
    Goods[] arr = new Goods[4];
    arr[0] = new Goods("A",34);
    arr[1] = new Goods("B",30);
    arr[2] = new Goods("C",34);
    arr[3] = new Goods("D",24);
    Arrays.sort(arr);
    System.out.println(Arrays.toString(arr));
}
```



## Comparator接口

![image-20230102143155773](3-Java%E6%AF%94%E8%BE%83%E5%99%A8.assets/image-20230102143155773.png)

```java
@Test
public void Test3(){
    String[] arr = new String[]{"AA","CC","MM","BB"};
    Arrays.sort(arr, new Comparator() {
        @Override
        public int compare(Object o1, Object o2) {
            if(o1 instanceof String&&o2 instanceof String){
                String s1 = (String) o1;
                String s2 = (String) o2;
                return -s1.compareTo(s2);
            }
            throw new RuntimeException("输入的数据类型不一致!");
        }
    });
    System.out.println(Arrays.toString(arr));
}
```

Comparable接口和Comparator的使用的对比：

Comparable接口的方式一旦确定，保证Comparable接口实现类的对象在任何位置都可以比较大小

Comparator接口属于临时性bi'jiao