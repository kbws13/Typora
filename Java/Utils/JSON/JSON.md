# 什么是JSON

JSON(JavaScript Object Notation，JS对象标记)是一种轻量级的数据交换格式

采用完全独立于编程语言的文本格式来存储和表示数据

在JS中一切皆对象。因此，任何JS支持的类型都可以通过JSON表示，例如字符串、数字、对象、数组等，看他的要求和语法格式：

- 对象表示为键值对
- 数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

**JSON键值对**是用来保存JS对象的一种方式，和JS对象的写法也大同小异，键/值对组合中的键名写在前面用双引号包裹，使用冒号分隔，然后紧接着值：

```json
{"name":"hsy"}
{"age":"3"}
{"sex":"男"}
```

JSON和JS对象的关系：

JSON是JS对象的字符串表示法，它使用文本表示一个JS对象的信息，本质是一个字符串：

```js
var obj = {a:"hello",b:"world"};//这是一个对象，注意键名也可以用引号包裹
var json = {"a":"hello","b":"world"};//这是一个JSON字符串，本质是一个字符串
```

# JSON和JS对象互换

JSON字符串转换为JS对象，使用`JSON.parse()`方法

```js
var obj = JSON.parse('{a:"hello",b:"world"}');//结果是	{a:'hello',b:'world'}
```

JS对象转换为JSON字符串，使用`JSON.stringify()`方法

```js
var json = JSON.stringify({a:'hello',b:'world'});//结果是'{"a":"hello","b":"world"}'
```

# 使用Controller实现返回JSON数据

引入第三方jar包的依赖，jackson包

```xml
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.7.1</version>
</dependency>
```

fastjson包

```xml
 <dependency>
     <groupId>com.alibaba</groupId>
     <artifactId>fastjson</artifactId>
     <version>1.2.74</version>
</dependency>
```



一般Controller中的方法返回字符串后会经过视图解析器，返回resources文件夹中的html文件。但json需要返回的是一个字符串

解决方法：在方法前面加上`@ResponseBody`注解，此注解会将服务器返回的对象转换为JSON对象响应回去

**注意：**出现了乱码问题，我们需要设置编码格式为UTF-8，以及它的返回类型，通过`@RequestMapping`的`produces`属性来实现，`produces`指定响应体返回类型和编码

```java
@RequestMapping(value="/json1",produces="application/json;charset=utf-8")
@ResponseBody
public String json1(){
    //需要使用一个jackson的对象映射器，本质就是一个类，使用它可以直接将对象转换为json字符串
    ObjectMapper mapper = new ObjectMapper();
    //创建一个对象
    User user = new User("hsy",2,"男");
    //将Java对象转化成json字符串
    String str = mapper.writeValueAsString(user);
    
    return str;
}
```

发现问题：时间默认返回的json字符串变成了时间戳的格式

解决方法：

```java
@RequestMapping("/time1")
@ResponseBody
public String json2() throws JsonProcessingException {
    ObjectMapper mapper = new ObjectMapper();
    //1.关闭时间戳功能
    mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);
    //2.时间格式化问题，自定义日期格式对象
    SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    //3.让mapper指定时间格式为format
    mapper.setDateFormat(format);
    //写一个日期对象
    Date date = new Date();
    return mapper.writeValueAsString(date);
}
```

可以将返回JSON数据的代码封装成工具类

JsonUtils类

```java
package xyz.kbws.utils;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * @author hsy
 * @date 2023/1/25
 */
public class JsonUtils {

    public static String getJson(Object object){
        return getJson(object,"yyyy-MM-dd HH:mm:ss");
    }

    public static String getJson(Object object,String dateformat){
        ObjectMapper mapper = new ObjectMapper();
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);
        SimpleDateFormat format = new SimpleDateFormat(dateformat);
        mapper.setDateFormat(format);

        try {
            return mapper.writeValueAsString(object);
        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }
    }
}
```

Controller层使用

```java
@RequestMapping("/time2")
@ResponseBody
public String json3(){
    Date date = new Date();
    return JsonUtils.getJson(date);
}
```
