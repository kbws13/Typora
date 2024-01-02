# JavaWeb

## 1、基本概念

### 1.1、前言

web开发：

- web，网页的意思，例如：www.baidu.com
- 静态web
  - html，css
  - 提供给所有人看的数据始终不会发生变化
- 动态web
  - 几乎所有的网站
  - 提供给所有人看的数据始终会发生变化，每个人在不同的时间，不同的地点看到的信息各不相同！
  - 技术栈：Servlet/JSP，ASP，PHP

在Java中，动态web资源开发的技术统称为JavaWeb

### 1.2、web应用程序

web应用程序：可以提供浏览器访问的程序；

- a.html、b.html......多个web资源可以被外界访问，对外界提供服务
- 能访问到的任何一个页面或者资源，都存在于某一台计算机上
- URL
- 统一的web资源会被放在同一个文件夹下，web应用程序-->Tomcat：服务器
- 一个web应用由多部分组成（静态web，动态web）
  - html,css,js
  - jsp,servlet
  - Java程序
  - 配置文件（Properties）

web程序编写完毕后，若想提供给外界访问：需要一个服务器来统一管理

### 1.3、静态Web

- *.htm, *.html,这些都是网页的后缀，如果服务器上一致存在这些东西，我们就可以直接进行读取，通讯
- 静态web存在的缺点
  - Web页面无法动态更新，所有的用户看到的都是同一个页面
    - 轮播图，点击特效：伪动态
    - JavaScript[实际开发中，它用的最多]
    - VBScript
  - 它无法和数据库交互（数据无法持久化，用户无法交互）

### 1.4、动态Web

页面会动态展示：“Web的页面展示因人而异”；

缺点：

- 加入服务器的动态web资源出现了错误，我们需要重新编写**后台程序**，重新发布
  - 停机维护

优点：

- Web页面可以动态更新，所有的用户看到的不是同一个页面
- 它可以和数据库交互（数据持久化：注册，商品信息，用户）

![image-20220719103158032](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103158032.png)





## 2、Web服务器

### 2.1、技术讲解

ASP：

- 微软：国内最早流行的就是ASP
- 在HTML中潜入了VB脚本，ASP+COM
- 在ASP开发中，基本一个页面都有几千行的业务代码，页面极其混乱
- 维护成本高
- C#
- IIS

PHP：

- PHP开发速度很快，功能很强大，跨平台，代码很简单
- 无法承载大访问量情况（局限性）



JSP/Servlet：

B/S：浏览和客户端

C/S：客户端和服务器

- sun公司主推的B/S架构
- 基于Java语言的（所有的大公司，或者一些开源的组件，都是用Java写的）
- 可以承载三高问题带来的影响
- 语法与ASP相似，加强市场强度

### 2.2、web服务器

服务器是一种被动的操作，用来处理用户的一些请求和给客户一些响应信息

IIS：微软

Tomcat：Java

## 3、Tomcat

### 3.1、安装Tomcat

Tomcat官网：https://tomcat.apache.org/

### 3.2、Tomcat启动和配置

**启动、关闭Tomcat**

![image-20220719103202950](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103202950.png)

访问测试：https://localhost:8080/

可以配置启动的端口号(conf\server.xml)

- tomcat的默认端口号为：8080
- mysql：3360
- http：80
- https：443

```xml
<Connector potr="8081" protocol="HTTP/1.1"
		  connectionTimeout="20000"
		  redirectPort="8443"/>
```

可以配置主机的名字

- 默认主机名称为：localhost-->127.0.0.1
- 默认网站应用存放的位置为：webapps

```xml
<Host name="www.qinjiang.com" appbase="wwebapps"
      unpackWARs="true" autoDeploy="true"/>
```

##### 高难度面试题

请你谈谈网站是如何访问的！

1. 输入一个域名，回车

2. 检查本机的C:\Windows\System32\drivers\etc\hosts配置文件下有没有这个域名的映射

   1. 有：直接返回对应的ip地址，这个地址中，有我们需要访问的web程序，可以直接访问

      ```java
      127.0.0.1   	www.qinjiang.com
      ```

   2. 没有：去DNS服务器去找，找到的话就返回，找不到就返回找不到

      ![image-20220719103207381](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103207381.png)

### 3.3、发布一个web网站

- 将自己写的网站，放到服务器（Tomcat）中指定的web应用的文件夹（webapps）下，就可以访问了

网站应有的结构：

```java
--webapps:Tomcat服务器的web目录
	-Root
	-kuangstudy:网站的目录名
		-WEB-INF
			-classes:java程序
			-lib:web应用所依赖的jar包
			-web.xml:网站配置文件
		-index.html 默认的首页
		-static
            -css
            	-style.css
            -js
            -img
         -.......
```

## 4、HTTP

### 4.1、什么是HTTP

超文本传输协议（Hyper Text Transfer Protocol，HTTP）是一个简单的请求-响应协议，它通常运行在[TCP](https://baike.baidu.com/item/TCP/33012)之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。请求和响应消息的头以[ASCII](https://baike.baidu.com/item/ASCII/309296)形式给出；而消息内容则具有一个类似[MIME](https://baike.baidu.com/item/MIME/2900607)的格式。这个简单模型是早期[Web](https://baike.baidu.com/item/Web/150564)成功的有功之臣，因为它使开发和部署非常地直截了当。 

- 文本：html，字符串，~.....
- 超文本：图片，音乐，视频，定位，地图.......
- 默认端口：80

HTPS：安全的

- 默认端口：443

### 4.2、两个时代

- http1.0
  - HTTP/1.0：客户端可以与web服务器连接后，只能获得一个web资源，之后会断开连接
- http2.0
  - HTTP/1.1：客户端可以与web服务器连接后，可以获得多个web资源

### 4.3、HTTP请求

- 客户端---发送请求（Request）---服务器

百度：

```java
Request URL: https://www.baidu.com/		请求地址
Request Method: GET		get方法/post方法
Status Code: 200 OK		状态码：200
Remote（远程） Address: 110.242.68.4:443
```

```java
Accept: text/html
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9		语言
Cache-Control: max-age=0
Connection: keep-alive
```

#### 1、请求行

- 请求行中的请求方式：GET
- 请求方式：Get/Post
  - get：一次请求能够携带的参数较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效
  - post：一次请求能够携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效

#### 2、消息头

```java
Accept:告诉浏览器，它所支持的数据类型
Accept-Encoding:支持哪种编码格式 GBK UTF-8 GB2312  ISo8859-1
Accept-Language:告诉浏览器他的语言环境
Cache-Control:缓存控制
Connection: 告诉浏览器，请求完成是断开还是保持连接
HOST：主机
```



### 4.4、HTTP相应

- 服务器---相应---客户端

百度：

```java
Cache-Control: private		缓存控制
Connection: keep-alive		连接
Content-Encoding: gzip		编码
Content-Type: text/html;charset=utf-8		类型
```

#### 1、响应体

```java
Accept:告诉浏览器，它所支持的数据类型
Accept-Encoding:支持哪种编码格式 GBK UTF-8 GB2312  ISo8859-1
Accept-Language:告诉浏览器他的语言环境
Cache-Control:缓存控制
Connection: 告诉浏览器，请求完成是断开还是保持连接
HOST：主机
Refrush：告诉客户端，多久刷新一次
Location：让网页重新定位
```

#### 2、响应状态码

200：相应成功

3xx：请求重定向

4xx：找不到资源 404

5xx：服务器代码错误 500	502：网关错误

## 5、Maven

**为什么要学习这个技术**

1. 在javaweb开发过程中，需要使用大量的jar包，我们手动去导入
2. 如何能够让一个东西自动导入和配置这个jar包

由此，Maven诞生了



### 5.1Maven项目架构管理工具

目前用来导入jar包

Maven的核心思想：**约定大于配置**

- 有约束不要去违反

Maven会规定好你如何去编写我们的Java代码，必须要按照这个规范来

### 5.2、下载安装Maven

https://maven.apache.org/

### 5.3、配置环境变量

系统环境变量中

- M2_HOME	maven目录下的bin目录
- MAVEN_HOME      maven的目录
- 在系统的path中配置 %MAVEN_HOME%\bin

![image-20220719103215021](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103215021.png)

测试Maven是否安装成功，保证必须配置完毕

### 5.4阿里云镜像

- 镜像：mirrors
  - 作用：加速我们的下载
- 国内建议使用阿里云的镜像

```xml
<mirror> 
    <id>alimaven</id> 
    <name>aliyun maven</name> 
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url> 
    <mirrorOf>central</mirrorOf> 
  </mirror> 
```

### 5.5本地仓库

**建立一个本地仓库**

```xml
<localRepository>D:\Maven\apache-maven-3.8.5\maven-repo</localRepository>
```

### 5.6、在IDEA中使用Maven

1. 启动IDEA
2. 创建一个Maven项目

![image-20220719103218941](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103218941.png)

![image-20220719103223064](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103223064.png)

3. 等待项目初始化完毕
4. IDEA中的Maven设置

![image-20220719103227107](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103227107.png)

注意：手动更改Maven主路径和配置文件地址

5. 到这里Maven在IDEA中的配置和使用就可以了

### 5.7、创建一个普通的Maven项目

### 5.8标记文件夹功能

![image-20220719103230676](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103230676.png)

### 5.9在IDEA中配置Tomcat

![image-20220719103234147](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103234147.png)

![image-20220719103237656](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103237656.png)

![image-20220719103240945](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103240945.png)

 ![image-20220719103248660](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103248660.png)

![image-20220719103252036](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103252036.png)



## 6、Servlet

### 6.1、Servlet简介

- Servlet是sun公司开发动态web的一门技术
- Sun在这些API中提供的一个接口叫做：Servlet，如果你想开发一个Servlet程序，只需要完成两个步骤：
  - 编写一个类，实现Servlet接口
  - 把开发好的Java类部署到web服务器中

把实现了Servlet程序的Java程序叫做，Servlet

### 6.2、HelloServlet

Servlet接口sun公司有两个默认的实现类：==HttpServlet，GenericServlet==

1. 构建一个空白Maven项目，删掉里面的src目录，之后的学习就在这个项目里建立Moudel；这个空的工程就是Maven的主工程；

   Servlet依赖：

   ```xml
   <dependency>
         <groupId>jakarta.servlet</groupId>
         <artifactId>jakarta.servlet-api</artifactId>
         <version>5.0.0</version>
         <scope>provided</scope>
    </dependency>
   ```

   JSP依赖：

   ```xml
   <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
   <dependency>
       <groupId>javax.servlet.jsp</groupId>
       <artifactId>javax.servlet.jsp-api</artifactId>
       <version>2.3.1</version>
       <scope>provided</scope>
   </dependency>
   
   <!--Tomcat10的依赖-->
   <dependency>
         <groupId>jakarta.servlet.jsp</groupId>
         <artifactId>jakarta.servlet.jsp-api</artifactId>
         <version>3.0.0</version>
         <scope>provided</scope>
    </dependency>
   ```

   

2. 关于Maven父子工程的理解：

   父项目中会有

   ```xml
   <modules>
           <module>servlet-01</module>
   </modules>
   ```

   子项目中会有

   ```xml
   <parent>
           <artifactId>demo3</artifactId>
           <groupId>org.example</groupId>
           <version>1.0-SNAPSHOT</version>
   </parent>
   ```

   父项目中的Java子项目可以直接使用

   ```java
   son extends father
   ```

3. Maven环境优化

   1. 修改web.xml为最新的

      ```xml
      <?xml version="1.0" encoding="UTF-8" ?>
      <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                            http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
               version="4.0"
               metadata-complete="true">
        
      </web-app>
      ```

      

   2. 将Maven的结构搭建完整

4. 编写一个Servlet程序

   1. 编写一个普通类
   2. 实现Servlet接口，这里我们直接继承HttpServlet

   ```java
   public class HelloServlet extends HttpServlet {
       //由于get或者post只是请求实现的不同方式，可以相互调用，业务逻辑都一样
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           PrintWriter writer = resp.getWriter();//相应流
           writer.print("Hello,Servlet");
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req,resp);
       }
   }
   ```

5. 编写Servlet的映射(在web.xml中)

   为什么需要映射：我们写的是Java程序，但是需要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要在web服务器中注册我们写的Servlet，还需要给他一个浏览器能够访问的路径

   ```xml
     <!--注册Servlet-->
     <servlet>
       <servlet-name>hello</servlet-name>
       <servlet-class>com.java.Servlet.HelloServlet</servlet-class>
     </servlet>
     <!--Servlet的请求路径-->
     <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello</url-pattern>
     </servlet-mapping>
   ```
   一个Servlet要对应一个Class文件

6. 配置Tomcat

   注意项目发布的路径

7. 启动测试

   问题：Tomcat10无法实例化类

   Tomcat10对servet依赖包级目录改变，maven导入依赖时应该导入： 

   ```xml
   <dependency>
         <groupId>jakarta.servlet.jsp</groupId>
         <artifactId>jakarta.servlet.jsp-api</artifactId>
         <version>3.0.0</version>
         <scope>provided</scope>
    </dependency>
    <dependency>
         <groupId>jakarta.servlet</groupId>
         <artifactId>jakarta.servlet-api</artifactId>
         <version>5.0.0</version>
         <scope>provided</scope>
    </dependency>
   ```

### 6.3、 Servlet详解

#### 6.3.1、Servlet核心接口和类

> 在Servlet体系结构中，除了实现Servlet接口，还可以通过继承GenericServket和HttpServlet类，实现编写

##### Servlet接口

> 在Servlet API中最重要的是Servlet接口，所有的Servlet都会直接或间接的与该窗口发生关系，或是直接实现该接口，或间接继承自实现了该接口的类。
>
> 该接口包含一下五个方法：
>
> init(ServletConfig config)
>
> ServletConfig getServletConfig()
>
> service(ServletRequest req,ServletResponse res)
>
> String getServletInfo()
>
> destroy( )

##### GenericServlet抽象类

> GenericServlet抽象类使得编写Servlet变得容易。它提供的生命周期方法init和destroy的简单实现，要编写一般的Servlet，只需要重写service方法即可

##### HttpServlet类

> HttpServlet是继承GenericServlet的基础上进一步的拓展
>
> 提供将要被子类化以创建适用于Web站点的HttpServlet抽象类。HttpServlet的子类至少重写一个方法，该方法通常是以下这些方法之一：
>
> doGet，如果servlet支持HTTP GET请求
>
> doPost，用于HTTP POST请求
>
> doput，用于HTTP PUT请求
>
> doDelete，用于HTTP DELETE请求

#### 6.3.2、Servlet的两种创建方式

##### 实现接口Servlet

```java
/*
* Servlet创建的第一种方式：实现接口Servlet
* */
public class HelloServlet02 implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {

    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

- 该方式比较麻烦，需要实现接口中的所有方法

##### 继承HttpServlet(推荐)

```java
public class HelloServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException, IOException {
        PrintWriter writer = resp.getWriter();//相应流
        writer.print("Hello,Servlet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}
```

##### 常见错误

> - HTTP Status 404 资源找不到
>     - 第一种情况：地址书写错误
>     - 第二种情况：地址没有问题，把IDEA项目中的out目录删去，然后重新运行
> - Servlet地址配置重复。
> - Servlet地址配置错误。

#### 6.3.3、Servlet两种配置方式

##### 使用web.xml配置(Servlet2.5之前使用)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">

    <!--注册Servlet-->
    <servlet>
        <!--名称-->
        <servlet-name>hello</servlet-name>
        <!--Servlet的全称名称-->
        <servlet-class>com.java.servlet.HelloServlet</servlet-class>
        <!--启动的优先级，数字越小越先起作用-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--映射配置-->
    <!--Servlet的请求路径-->
    <servlet-mapping>
        <!--名称-->
        <servlet-name>hello</servlet-name>
        <!--资源匹配规则：精确匹配-->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
    <!--Error-->
    <servlet>
        <servlet-name>error</servlet-name>
        <servlet-class>com.java.servlet.ErrorServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>error</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>
</web-app>
```

url-pattern定义匹配规则，取值说明：

| 精准匹配   | /具体的名称 | 只有url路径是具体的名称的时候才会触发Servlet   |
| ---------- | ----------- | ---------------------------------------------- |
| 后缀匹配   | *.xxx       | 只要是以xxx结尾的就匹配触发Servlet             |
| 通配符匹配 | /*          | 匹配所有请求，包含服务器的所有资源             |
| 通配符匹配 | /           | 匹配所有请求，包含服务器的所有资源，不包括.jsp |

load-on-startup

1. 元素标记容器是否应该在web应用程序的时候就加载这个Servlet
2. 它的值必须是一个整数，表示servlet被加载的先后顺序
3. 如果该元素的值为负数或没有设置，则容器会当Servlet被请求时再加载
4. 如果值为正整数或者0时，表示容器在应用启动时就加载并初始化这个Servlet，值越小，servlet的优先级越高，就越被优先加载。值相同时，容器就会自己选择顺序来加载

##### 使用注解(Servlet3.0后支持，推荐)

```java
@WebServlet("/hello")
public class HelloServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException, IOException {
        PrintWriter writer = resp.getWriter();//相应流
        writer.print("Hello,Servlet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}
```

##### @WebServlet注解常用属性

> - name：Servlet名字（可选）
> - value：配置url路径，可以配置多个。例：`@WebServlet(value = {"/bs","/bbs"})`
> - urlPatterns：配置url路径，和value作用一样，不能同时使用
> - loadOnStartup配置Servlet的创建的时机，如果是0或者正数，启动程序时创建，如果是负数，则访问时创建。数字越小优先级越高 



### 6.4、Servlet应用【重点】

#### 6.4.1、request对象

> 在Servlet中用来处理客户端请求需要用到doGet或doPost方法的request对象

![image-20220719103302800](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103302800.png)

##### get和post的区别

> ==get请求==
>
> - get请求的数据会放在url之后，以？分隔URL和传输数据，参数之间以&连接
> - get方式明文传递，数据量小，不安全
> - 效率高，浏览器默认请求方法为GET请求
> - 对应的Servlet方法是doGet
>
> ==post请求==
>
> - postff是把提交的数据放在HTTP包的body中
> - 密文传输数据，数据量大，安全
> - 效率相对没有GET高
> - 对应的方法是doPost

##### request主要方法

| 方法名                                    | 说明                         |
| ----------------------------------------- | ---------------------------- |
| String getParameter(String name)          | 根据表单组件名称获取提交数据 |
| void setCharacterEncoding(String charset) | 指定每个请求的编码           |

##### request应用

HTML页面：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
</head>
<body>
    <form action="rs" method="get">
        用户名：<input type="text" name="username"><br/>
        密码：<input type="password" name="password"><br/>
        <input type="submit" value="注册">
    </form>
</body>
</html>
```

Servlet代码：

```java
@WebServlet(value = "/rs")
public class RegisterServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取用户请求发送的数据
        String username=req.getParameter("username");
        String password=req.getParameter("password");
        System.out.println("提交的数据"+username+"\t"+password);
    }
}
```

##### get请求收参问题

> 产生乱码是因为客户端和服务器沟通的编码不一样导致的，解决的办法为：在客户端和服务端设置一个统一的编码，之后就按照此编码进行数据的传输和接收

##### get中文乱码

> 在Tomcat7及以下版本，客户端以UTF-8的编码传输数据到客户端，而服务端的request对象使用的是ISO8859-1这个字符编码来接收数据，服务器与客户端沟通的编码不一致才导致产生中文乱码
>
> 解决办法：在接收到指定数据后，先获取request对象以ISO8859-1字符编码接收到的原始数据的字节数组，然后通过字节数组以指定的编码构建字符串，解决乱码问题
>
> Tomcat8的版本中get方式不会出现乱码，因为服务器对url的编码格式可以进行自动转换

解决get中文乱码的代码：

```java
@WebServlet("/GETServlet")
public class GetServlet extend HttpServlet{
    private static final long serialVersionUID=1L;
    
     protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         //获取表单提交的信息
         String name=req.getParameter("name");
         name=new String(name.getBytes("ISO8859-1"),"UTF-8");
         //获取年龄
         String age=req.getParameter("age");
         //服务端打印输出
         System.out.println(req.getRemoteAddr()+"发来信息：姓名："+name+"--->年龄："+age);
     }
}
```

##### post中文乱码

> 由于客户端是以UTF-8字符编码将表单传输到服务器端的，因此服务器也需要设置以UTF-8字符编码进行接收。
>
> 解决方案：使用从ServletRequest接口继承来的setCharacterEncoding(charset)方法进行统一的编码设置

```java
@WebServlet("/GETServlet")
public class GetServlet extend HttpServlet{
    private static final long serialVersionUID=1L;
    
     protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         //设置请求参数的编码格式---对GET无效
         req.setCharacterEncoding("UTF-8");
         //获取表单提交的信息
         String name=req.getParameter("name");
         //服务端打印输出
         System.out.println(req.getRemoteAddr()+"发来信息：姓名："+name);
     }
}
```

#### 6.4.2、response对象

> response对象用于相应客户请求并向客户端输出信息

![image-20220719103309123](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103309123.png)

##### response主要方法

| 方法名称                     | 作用                               |
| ---------------------------- | ---------------------------------- |
| setHeader(name,value)        | 设置响应信息头                     |
| setContentType(String)       | 设置响应文件类型、响应式的编码格式 |
| setCharacterEncoding(String) | 设置服务端响应内容编码格式         |
| getWriter()                  | 获取字符输出流                     |

> 案例：使用response对象向浏览器输出HTML内容，实现用户登录后，输出Login Success

```java
//
//
PrintWriter writer = resp.getWriter();
out.println("<html>");
out.println("<head><title>login</title></head>");
out.println("<body>");
out.println("<h1>Login Success!</h1>");
out.println("</body>");
out.println("</html>");
```

- 如果输出包含中文，则出现乱码，因为服务器默认使用ISO8859-1编码响应内容

##### 解决输出中文乱码

> - 设置服务器响应的编码格式
> - 设置客户端响应内容的头文件内容的文件类型及编码格式

```java
response.setCharacterEncoding("utf-8");//设置响应编码格式为utf-8
response.setHeader("Content-type","text/html;charset=UTF-8")
```

==不推荐==

> 同时设置服务端的编码格式和响应端的文件类型及响应时的编码格式

```java
response.setContentType("text/html;charset=UTF-8");
```

==推荐==



### 6.5、 Servlet原理

Servlet是由Web服务器调用，Web服务器在收到浏览器请求后，会：

![image-20220719103313381](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103313381.png)

### 6.6、 Mapping问题

1. 一个Servlet可以指定一个映射路径

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   ```

   

2. 一个Servlet可以指定多个映射路径

   ```xml
   	<servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello1</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello2</url-pattern>
       </servlet-mapping>
   ```

3. 一个Servlet可以指定通用映射路径

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello/*</url-pattern>
   </servlet-mapping>
   ```

4. 默认请求路径

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/*</url-pattern>
   </servlet-mapping>
   ```

5. 指定一些后缀或前缀等等

   ```xml
   <!--可以自定义后缀实现请求映射
   	注意，*前面不能加项目映射的路径
   	hello/sdhawuhd.qinjiang-->
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>*.qinjiang</url-pattern>
   </servlet-mapping>
   ```

6. 优先级问题

   指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求；

   ```xml
   <!--404-->
   <servlet>
       <servlet-name>error</servlet-name>
       <servlet-class>com.java.servlet.ErrorServlet</servlet-class>
   </servlet>
   <servlet-mapping>
       <servlet-name>error</servlet-name>
       <url-pattern>/*</url-pattern>
   </servlet-mapping>
   ```

### 问题

IDEA创建maven web项目出现“No archetype found in remote catalog. Defaulting to internal catalog”：

1. 新建一个名为`archetype-catalog.xml ` 的文件，内容为：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <archetype-catalog xmlns="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-catalog/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-catalog/1.0.0 http://maven.apache.org/xsd/archetype-catalog-1.0.0.xsd">
   	<archetypes>
   		<archetype>
   			<groupId>com.fr</groupId>
   			<artifactId>fr-server</artifactId>
   			<version>7.0</version>
   			<repository>
   				http://git.oschina.net/thinkgem/repos/raw/master
   			</repository>
   		</archetype>
   		<archetype>
   			<groupId>com.fr</groupId>
   			<artifactId>fr-third</artifactId>
   			<version>7.0</version>
   			<repository>
   				http://git.oschina.net/thinkgem/repos/raw/master
   			</repository>
   		</archetype>
   	</archetypes>
   </archetype-catalog>
   ```

2. 将`archetype-catalog.xml ` 放到自己创建的仓库里

3. 为IDEA新建项目设置archetype的使用方式为local， File--new project Settings--Settings for new project--Build, Execution, Deployment --Build Tools --Maven -- Runner
   ![image-20220719103318756](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103318756.png)

4. 重新构建Maven项目

5. settings.xml用maven安装目录下的那个，本地仓库用自己创建的那个, 在下边新建一项：键为archetypeCatalog，值为internal。 

   ![image-20220719103322623](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103322623.png) 

6. 问题解决

### 6.7、 ServletContext

web容器在启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用；

#### 1、共享数据

我在这个Servlet中保存的数据，可以在另一个Servlet中拿到

![image-20220719103326469](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103326469.png)

```java
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        ServletContext servletContext = this.getServletContext();
        String name="张三";
        servletContext.setAttribute("username",name);

    }
}
```

```java
public class GetServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        String username = (String) context.getAttribute("username");


        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf-8");
        resp.getWriter().print("名字"+username);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
}
```

```xml
	<servlet>
        <servlet-name>Hello</servlet-name>
        <servlet-class>com.java.servlet.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>Hello</servlet-name>
        <url-pattern>/Hello</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>getc</servlet-name>
        <servlet-class>com.java.servlet.GetServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>getc</servlet-name>
        <url-pattern>/getc</url-pattern>
    </servlet-mapping>
```

测试访问结果；

#### 2、获取初始化参数

在web.xml中

```xml
<!--配置一些web应用初始化参数-->
<context-param>
    <param-name>url</param-name>
    <param-value>jdbc:mysql://localhost:3306/mybatis</param-value>
</context-param>
```

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        String url = context.getInitParameter("url");
    	resp.getWriter().print(url);
}
```

## 7、JSP

### 一、引言

> 之前使用Servlet的时候，服务端通过Servlet响应客户端页面，有说明不足之处？
>
> - 开发方式麻烦：继承父类、覆盖方法、配置web.xml或注释
> - 代码修改麻烦：重新编译、部署、重启服务
> - 显示方式麻烦：获取流，使用print("")；逐行打印
> - 协同开发麻烦：UI负责美化页面，程序员负责编写代码，UI不懂Java，程序员又不能将所有前端页面的内容通过流输出

### 二、JSP（Java Server Pages）

#### 2.1 概念

> 简化的Servlet设计，在HTML标签中嵌套Java代码，用以高效开发Web应用的动态网页

#### 2.2 作用

> 替换显示页面部分的Servlet使用(使用*.jsp文件替换XxxJSP.java)

### 三、JSP开发【重点】

#### 3.1 创建JSP

> 在web目录下新建*.jsp文件（与WEB-INF平级）

##### 3.1.2 JSP编写Java代码

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    NOW:<%= new java.util.Date()%>
</body>
</html>
```

- 使用<%= %>标签编写Java代码在页面中打印当前系统时间

##### 3.1.3 访问JSP

> 在浏览器输入http://ip:port/项目路径/资源名称

#### 3.2 JSP与Servlet

> - 关系
>     - JSP文件在容器中会转换称Servlet执行
>     - JSP是对Servlet的一种高级封装。本质还是Servlet
> - 区别
>     - 与Servlet相比：JSP可以很方便的编写或者修改HTML网页而不用去面对大量的println语句

![image-20220719103333221](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103333221.png)

#### 3.3 JSP实现原理

> Tomcat会将xxx.jsp转换称java代码，进而编译称class文件运行，最终将运行结果通过response返回给客户端

![image-20220719103336541](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103336541.png)

##### 3.3.1 JSP.java源文件存放目录

> 使用IDEA开发工具，Tomcat编译后的JSP文件（Xxx_jsp.class和Xxx_jsp.java）的存放地点：
>
> C:\用户\账户名\ .IntelliJIdea2021.3\system\tomcat\项目名称\work\Catalina\localhost\应用上下文\org\apache\jsp

### 四、JSP与HTML集成开发

#### 4.1 脚本

> 脚本可以编写Java语句、变量、方法或表达式



##### 4.1.1 普通脚本

> 语法：<% Java代码 %>

若`out.println()`无法使用，解决办法为：

1. 找到apache-tomcat-8.5.31

    ![image-20220719103343310](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103343310.png)

2. 在lib目录下找到下面两个jar包
    ![image-20220719103347585](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103347585.png)

3. 把这两个jar包复制到WEB-INF目录下的lib文件，可以自己在WEB-INF下创建lib文件
    ![image-20220719103351124](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103351124.png)
4. 右击这两个jar包，点击add as library，一路确定下去就行。
    ![image-20220719103354847](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103354847.png)

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>脚本的使用</title>
</head>
<body>
    <%
    	//jsp中，使用小脚本嵌入Java代码
        int a=10;
        System.out.println(a);//打印内容在客户端界面
        out.println(a);//打印内容在控制台
    %>
</body>
</html>
```

- 经验：普通脚本可以使用所有Java语法，除了定义函数
- 注意：脚本与脚本之间不可嵌套，脚本与HTML标签不可嵌套

##### 4.1.2 声明脚本

> 语法：`<%! 定义变量、函数 %>`

```jsp
<%! int i=0; %>
<%! int a,b,c;%>
<%! Object object =new Object();%>
<%! 
	//定义方法
	public void m1(){
		System.out.println("你好");
	}
%>
```

- 注意：声明脚本声明的变量是全局变量
- 声明脚本的内容必须在普通脚本<% %>中调用
- 如果声明脚本中的函数有返回值，使用输出脚本调用<%= %>

##### 4.1.3 输出脚本

> 语法：`<%=Java表达式 %>`

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
	<meta charset="utf-8">
    <title>jsp的基本使用</title>
</head>
<body>
    NOW:<%= new java.util.Date()%>
</body>
</html>
```

- 经验：输出脚本可以输出带有返回值的函数
- 注意：输出脚本中不能加；号

#### 4.2 JSP注释

> JSP注释主要有两个作用：为脚本代码做注释以及HTML内容做注释

##### 4.2.1 语法规则

| 语法           | 描述                                                 |
| -------------- | ---------------------------------------------------- |
| <%-- 注释 --%> | JSP注释，注释内容不会被发送给浏览器甚至不会被编译    |
| <!--注释-->    | HTML注释，通过浏览器查看网页源代码时可以看见注释内容 |

#### 4.3 JSP指令

> JSP指令用来设置与整个JSP页面相关的属性

| 指令             | 描述                                                    |
| ---------------- | ------------------------------------------------------- |
| <%@ page...%>    | 定义页面的依赖属性，比如脚本语言、error页面、缓存需求等 |
| <%@ include...%> | 包含其他文件                                            |
| <%@ taglib...%>  | 引入标签库的定义，可以说自定义标签                      |

##### 4.3.1 Page指令

> - 语法：<%@ page attribute="value1" attribute="value2" %>
> - Page指令为容器提供当前页面的使用说明。一个JSP页面可以包含多个指令

| 属性         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| contentType  | 指定当前JSP页面的MIME类型和字符编码格式                      |
| errorPage    | 指定当JSP页面发生异常时需要转向的错误页面                    |
| isErrorPage  | 指定当前页面是否可以作为另一个JSP页面的错误处理页面          |
| import       | 导入要使用的Java类                                           |
| language     | 定义JSP页面使用的脚本语言，默认是Java                        |
| session      | 指定JSP页面是否使用session。默认为true立即创建，false为使用时创建 |
| pageEncoding | 指定JSP页面的解码格式                                        |

##### 4.3.2 include指令

> - 语法：<%@ include file="被包含的JSP路径"%>
> - 提供include指令来包含其他文件
> - 被包含的文件可以说JSP文件、HTML文件或文本文件。包含的文件就好像是当前JSP文件的一部分，会被同时编译执行（静态包含）

```jsp
<% include file="header.jsp"%>
...
...
<% include file="footer.jsp"%>
```

- 注意：可能会有重名冲突问题，不建议使用

##### 4.3.2 taglib指令

> 语法：<%@ taglib uri="外部标签库路径" prefix="前缀"%>
>
> 引入JSP的标准标签库

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
```

#### 4.4 动作标签

> - 语法：`<jsp:action_name attribute="value">`
> - 动作标签指的是JSP页面在运行期间的命令

##### 4.4.1 include

> - 语法：<jsp:include page="相对URL地址"/>
> - <jsp:include >动作元素会将外部文件输出结果包含在JSP中（动态包含）

| 属性 | 描述                      |
| ---- | ------------------------- |
| page | 包含在页面中的相对URL地址 |

```jsp
<jsp:include page="index.jsp"/>
```

- 注意：前面以及介绍过include指令，它是将外部文件的输出代码复制到了当前JSP文件夹中。而这里的jsp:include动作不同，是将外部文件的输出结果引入到了当前的JSP文件夹中

### 七、MVC框架(Model-View-Controller)

#### 7.1 MVC概念

> MVC又称为编程模式，是一种软件设计思想，将数据操作、页面展示、业务逻辑分为三个层级(模块),独立完成，又相互调用
>
> - 模型层（Model）
> - 视图（View）
> - 控制器（Controller）

#### 7.2 MVC模式详解

> MVC并不是Java独有的，现在几乎所有的B/S的架构都采用了MVC模式
>
> - 视图View：视图即是用户看到并与之交互的界面，比如HTML（静态资源），JSP（动态资源）等等
> - 控制器Controller：控制器即是控制请求的处理逻辑，对请求处理，负责流程跳转（转发和重定向）
> - 模型Model：对客观世界的一种代表和模拟（业务模拟、对象模拟）

![image-20220719103403552](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103403552.png)

#### 7.3 优点

> - 低耦合性：模块与模块之间的关联性不强，不与某一具体实现产生密不可分的关联性
> - 高维护性：基于低耦合性，可做到不同层级之间的功能模块灵活更换、插拔
> - 高重用性：相同的数据库操作，可以服务于不同的业务处理。将数据作为独立模块，提高重用性

#### 7.4 MVC在框架中的应用

> MVC模式被广泛用于Java的各种框架中，比如Struts2、SpringMVC等等都用到了这种思想



#### 7.5 三层架构与MVC

##### 7.5.1 三层架构

> View层（表示界面层）、Service层（业务逻辑层）、DAO层（数据访问层）

![image-20220719103407301](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103407301.png)

##### 7.5.2 MVC与三层架构的区别

> - MVC强调的是视图和业务代码的分离。严格的说MVC关注的是Web层。View就是单独的页面，如JSP、HTML等，不负责业务处理，只负责数据的展示。而数据封装到Model里，由Controller负责在V和M之间传递。MVC强调业务和视图分离
> - 三层架构是“数据访问层”、“业务逻辑层”、“表示层”，指的是代码之间的解耦，方便维护和复用
