# System类

`java.lang.System`类提供的`public static long currentTimeMillis()`用来返回当前时间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差

![image-20230102103345619](2-%E6%97%A5%E6%9C%9F%E6%97%B6%E9%97%B4API.assets/image-20230102103345619.png)

# Date类

`java.util.Date`类中有一个类是`java.sql.Date`类

## 构造器

- `Date()`使用无参构造器创建的对象可以获取到本地当前的时间
- `Date(long date)`

## 常用方法

- `getTime()`返回自1970年1月1日0时0分0秒GMT以来此Date对象
- `toString()`把此Date对象转换为以下形式的String：`dow mon dd hh:mm:ss zzz yyyy`其中dow是一周中的某一天，zzz是时间标准

创建`java.sql.Date`对象

```java
java.sql.Date date3 = new Date(155030620410L);
System.out.println(date3);
```

将`java.util.Date`对象转换为`java.sql.Date`对象：

方式一：

```java
Date date4 = new java.sql.Date(155030620410L);
java.sql.Date date5 = new (java.sql.Date)date4;
```

方式二：

```java
Date date6 = new Date();
java.sql.Date date7 = new java.sql.Date(date6.getTime());
```

# SimpleDateFormat类

`java.text.SimpleDate`类

这个类允许进行格式化：日期->文本、解析：文本->日期

格式化：

- `SimpleDateFormat()`：默认的模式和语言环境创建对象
- `public SimpleDateFormat(String pattern)`：该构造方法可以用参数parrent指定的格式创建一个对象，该对象调用：
- `public String formet(Date date)`：方法格式化时间对象date

解析：

- `public Date parse(String source)`：从给定的字符串开始解析文本，以生成一个日期

# Calendar类

Calendar类是一个抽象基类，主要用于日期字段之间相互操作的功能

获取Calendar实例的方法：

- 调用`Calendar.getInstance()`方法
- 调用它的子类`GregorianCalendar`的构造器

![image-20230102132703403](2-%E6%97%A5%E6%9C%9F%E6%97%B6%E9%97%B4API.assets/image-20230102132703403.png)

# LocalDate、LocalTime、LocalDateTime

这三个类的实例是不可变对象，分别表示日期、时间、日期和时间。

- `LocalDate`表示ISO格式（yyyy-MM-dd）的日期，可以存储生日、纪念日等日期
- `LocalTime`表示一个时间而不是日期
- `LocalDateTime`是用来表示日期时间的，是最常用的类之一

![image-20230102133310647](2-%E6%97%A5%E6%9C%9F%E6%97%B6%E9%97%B4API.assets/image-20230102133310647.png)

# 格式化与解析日期或时间

`java.time.format.DateTimeFormatter`类：该类提供了三种格式化方法：

![image-20230102133528245](2-%E6%97%A5%E6%9C%9F%E6%97%B6%E9%97%B4API.assets/image-20230102133528245.png)

