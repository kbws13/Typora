# Java集合框架

Java集合可以分为Collection和Map两种体系

- Collection接口：单列数据，定义了存取一组对象的方法的集合
    - List：元素有序、可重复的集合
    - Set：元素无序、不可重复的集合
- Map接口：双列数据，保存具有映射关系的`key-value对`的集合

## 集合框架

![image-20230102155543164](1-%E9%9B%86%E5%90%88.assets/image-20230102155543164.png)

## Collection接口

![image-20230102161026643](1-%E9%9B%86%E5%90%88.assets/image-20230102161026643.png)

### Collection接口的常用方法

1. 添加
    - `add(Object obj)`
    - `addAll(Collection coll)`
2. 获取有效元素的个数
    - `int size()`
3. 清空集合
    - `void clear()`
4. 是否是空集合
    - `boolean isEmpty()`
5. 是否包含某个元素
    - `boolean contains(Object obj)`：是通过元素的`equals`方法来判断是否是同一个对象
    - `boolean containsAll(Collection c)`：同上
6. 删除
    - `boolean remove(Object obj)`：通过元素的`equals`方法来判断是否是要删除的那个元素。只会删除找到的第一个元素
    - `boolean removeAll(Collection coll)`：取当前集合的差集
7. 取两个集合的交集
    - `boolean retainAll(Collection c)`：把交集的结果存在当前集合中，不影响c
8. 集合是否相同
    - `boolean equals(Object obj)`
9. 转换成对象数组
    - `Object[] toArray()`
10. 获取集合对象的哈希值
    - `hashCode()`
11. 遍历
    - `iterator()`：返回迭代器对象，用于集合遍历

# Iterator迭代器接口

![image-20230104085933965](1-%E9%9B%86%E5%90%88.assets/image-20230104085933965.png)

## Iterator接口的方法

![image-20230104090056674](1-%E9%9B%86%E5%90%88.assets/image-20230104090056674.png)

```java
public void test1(){
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add(new Person("Jerry",20));
    coll.add(new String("Tom"));
    coll.add(false);
    
    Iterator iterator = coll.iterator();
    //方式一：不推荐
     System.out.println(iterator.next());
     System.out.println(iterator.next());
     System.out.println(iterator.next());
     System.out.println(iterator.next());
     System.out.println(iterator.next());
    //方式二：不推荐
    for(int i=0;i<coll.size();i++){
        System.out.println(iterator.next());
    }
    //方式三：推荐
    while(iterator.hasNext()){
         System.out.println(iterator.next());
    }
}
```

## 迭代器Iterator的执行原理

![image-20230104090758220](1-%E9%9B%86%E5%90%88.assets/image-20230104090758220.png)

### 两种错误的遍历方式

```java
public void test2(){
    //错误方式一
    Iterator iterator = coll.iterator();
    while(iterator.next()!=null){
        System.out.println(iterator.next());
    }
    //错误方式二
    //集合对象每次调用iterator方法都会产生一个新的迭代器对象
    while(coll.iterator().hasNext()){
        System.out.println(iterator.next());
    }
}
```

## remove方法的使用

```java
public void test3(){
    Iterator iterator = coll.iterator();
    while(iterator.hasNext()){
        Object obj = iterator.naex();
        if("Tom".equals(obj)){
            iterator.remove();
        }
    }
}
```

注意：

- Iterator可以删除集合的元素，但是是遍历过程中通过迭代器对象的remove方 法，不是集合对象的remove方法。
- 如果还未调用next()或在上一次调用 next 方法之后已经调用了 remove 方法， 再调用remove都会报IllegalStateException。

# List接口

Collection子接口之一

## 概述

- List集合类中元素有序、且可重复，集合中的每个元素都有其对应的顺序索引
- List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素。
- JDK API中List接口的实现类常用的有：ArrayList、LinkedList和Vector

### ArrayList

![image-20230104093628743](1-%E9%9B%86%E5%90%88.assets/image-20230104093628743.png)

### LinkedList

![image-20230104093653183](1-%E9%9B%86%E5%90%88.assets/image-20230104093653183.png)

![image-20230104093713408](1-%E9%9B%86%E5%90%88.assets/image-20230104093713408.png)

### Vector

![image-20230104093728555](1-%E9%9B%86%E5%90%88.assets/image-20230104093728555.png)

### 面试题

![image-20230104093744698](1-%E9%9B%86%E5%90%88.assets/image-20230104093744698.png)



## 接口方法

```java
void add(int index, Object ele):在index位置插入ele元素

boolean addAll(int index, Collection eles):从index位置开始将eles中 的所有元素添加进来

Object get(int index):获取指定index位置的元素

int indexOf(Object obj):返回obj在集合中首次出现的位置

int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置

Object remove(int index):移除指定index位置的元素，并返回此元素

Object set(int index, Object ele):设置指定index位置的元素为ele

List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex 位置的子集合
```

## 遍历List

1. Iterator迭代器方式
2. 增强for循环
3. 普通循环

# Set接口

## 概述

- Set接口是Collection的子接口，set接口没有提供额外的方法
- Set 集合不允许包含相同的元素，如果试把两个相同的元素加入同一个 Set 集合中，则添加操作失败。
- Set 判断两个对象是否相同不是使用 == 运算符，而是根据 equals() 方法

Set接口的实现类有三个：HashSet、LinkedHashSet、TreeSet

### HashSet

![image-20230104094427707](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094427707.png)

![image-20230104094514086](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094514086.png)

![image-20230104094522985](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094522985.png)

#### 重写`hashCode()`方法的基本原则

![image-20230104094559922](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094559922.png)

#### 重写`equals()`方法的基本原则

![image-20230104094653421](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094653421.png)

#### IDEA工具里面`hashCode()`的重写

![image-20230104094731493](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094731493.png)

### LinkedHashSet

![image-20230104094836803](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094836803.png)

![image-20230104094845646](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094845646.png)

### TreeSet

![image-20230104094858775](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094858775.png)

![image-20230104094907968](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094907968.png)

#### TreeSet的自然排序

![image-20230104094947561](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094947561.png)

![image-20230104094956178](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104094956178.png)

#### TreeSet的定制排序

![image-20230104095023315](1-Collection%E6%8E%A5%E5%8F%A3.assets/image-20230104095023315.png)

