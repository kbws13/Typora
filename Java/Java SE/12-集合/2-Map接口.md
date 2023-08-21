# Map接口继承树

![image-20230104095327788](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104095327788.png)

# 概述

![image-20230104095342733](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104095342733.png)

# 常用方法

## 添加、删除、修改

```java
Object put(Object key,Object value)：将指定key-value添加到(或修改)当前map对象中
void putAll(Map m):将m中的所有key-value对存放到当前map中
Object remove(Object key)：移除指定key的key-value对，并返回value
void clear()：清空当前map中的所有数据
```

## 查询元素

```java
Object get(Object key)：获取指定key对应的value
boolean containsKey(Object key)：是否包含指定的key
boolean containsValue(Object value)：是否包含指定的value
int size()：返回map中key-value对的个数
boolean isEmpty()：判断当前map是否为空
boolean equals(Object obj)：判断当前map和参数对象obj是否相等
```

## 元视图操作

```java
Set keySet()：返回所有key构成的Set集合
Collection values()：返回所有value构成的Collection集合
Set entrySet()：返回所有key-value对构成的Set集合
```

## 例子

```java
Map map = new HashMap();
//map.put(..,..)省略
System.out.println("map的所有key:");
Set keys = map.keySet();// HashSet
for (Object key : keys) {
	System.out.println(key + "->" + map.get(key));
}
System.out.println("map的所有的value：");
Collection values = map.values();
Iterator iter = values.iterator();
while (iter.hasNext()) {
	System.out.println(iter.next());
}
System.out.println("map所有的映射关系：");
// 映射关系的类型是Map.Entry类型，它是Map接口的内部接口
Set mappings = map.entrySet();
for (Object mapping : mappings) {
Map.Entry entry = (Map.Entry) mapping;
	System.out.println("key是：" + entry.getKey() + "，value是：" + entry.getValue())；
}
```

# HashMap

![image-20230104100054244](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104100054244.png)

## 存储结构

![image-20230104100115541](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104100115541-16727976759041.png)

![image-20230104100208973](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104100208973.png)

**HashMap什么时候进行扩容和树形化呢？**

![image-20230104100313605](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104100313605.png)

**映射关系的key是否可以修改？**

不要修改

映射关系存储到HashMap中会存储key的hash值，这样就不用在每次查找时重新计算 每一个Entry或Node（TreeNode）的hash值了，因此如果已经put到Map中的映射关系，再修改key的属性，而这个属性又参与hashcode值的计算，那么会导致匹配不上。

## 重要常量

![image-20230104100136842](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104100136842.png)

## 面试题

![image-20230104100420992](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104100420992.png)

# LinkedHashMap

![image-20230104100444771](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104100444771.png)

## 内部类

![image-20230104100457064](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104100457064.png)

# TreeMap

![image-20230104100527941](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104100527941.png)

# Hashtable

![image-20230104100610283](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104100610283.png)

# Properties

![image-20230104100624694](2-Map%E6%8E%A5%E5%8F%A3.assets/image-20230104100624694-16727979850133.png)

