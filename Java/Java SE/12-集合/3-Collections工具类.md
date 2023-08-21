# 概述

![image-20230104100754150](3-Collections%E5%B7%A5%E5%85%B7%E7%B1%BB.assets/image-20230104100754150.png)

# 常用方法

查找、替换

```java
Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
Object min(Collection)
Object min(Collection，Comparator)
int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
void copy(List dest,List src)：将src中的内容复制到dest中
boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换List 对象的所有旧值
```

## 同步控制

![image-20230104100916108](3-Collections%E5%B7%A5%E5%85%B7%E7%B1%BB.assets/image-20230104100916108.png)

