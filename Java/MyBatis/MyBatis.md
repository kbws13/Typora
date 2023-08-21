# 一、 MyBatis简介

## 1.1 框架概念

> 框架，就是软件的半成品，完成了软件开发中的通用操作，程序员只需很少或者不用进行加工就能够实现特定功能，从而简化开发人员在软件开发中的步骤，提高开发效率

## 1.2 常用框架

- MVC框架：简化了Servlet的开发步骤
  - Struts2
  - ==SpringMVC==
- 持久层框架：完成数据库操作的框架
  - apache DBUtils
  - Hibernate
  - Spring JPA
  - ==MyBatis==
- 胶水框架：Spring

SSM：Spring  SpringMVC  MyBatis

SSH： Spring  Struts2  Hibernate

## 1.3 MyBatis介绍

> MyBatis是一个==半自动== 的==ORM== 框架
>
> ORM（Object Relational Mapping）对象关系映射，将Java中的一个对象与数据库中一行记录一一对应
>
> ORM框架提供了实体类和数据表的映射关系，通过映射文件的配置，实现对象的持久化

MyBatis特点：

- 支持自定义SQL、存储过程
- 对原有的JDBC进行了封装，几乎消除了所有的JDBC代码，让开发者只需关注SQL本身
- 支持XML和注解配置方式自动完成ORM映射，实现结果映射

# 二、MyBatis框架部署

> 框架部署，就是将框架引入到我们的项目中

## 2.1 创建Maven项目

- Java工程
- Web工程

==注意：== 创建Maven项目时出现，如下错误时：

```xml
java.lang.RuntimeException: java.lang.RuntimeException: org.codehaus.plexus.component.repository.exception.ComponentLookupException:\n 
com.google.inject.ProvisionException: Unable to provision, see the following errors:
```

原因是Maven版本过高，解决方法为更改Maven版本（使用IDEA自带的Maven即可）

## 2.2 在项目中添加MyBatis依赖

- 在pom.xml中添加依赖

  - mybatis

    ```xml
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.4</version>
    </dependency>
    ```

  - mysql driver

    ```xml
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.19</version>
    </dependency>
    ```

## 2.3 创建MyBatis配置文件

- 创建自定义模板：选择recources---右键New---编辑文件模板

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
		"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

![image-20220719103026162](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103026162.png)

- 在resources中创建名为==mybatis-config.xml==的文件

- 在==mybatis-config.xml== 文件配置数据库连接信息

  ```xml
  <environments default="">
          <environment id="">
              <transactionManager type=""></transactionManager>
              <dataSource type=""></dataSource>
          </environment>
      	<environment id="">
              <transactionManager type=""></transactionManager>
              <dataSource type=""></dataSource>
          </environment>
  </environments>
  ```

  在environments配置数据库连接信息

  在enviroments标签中可以定义多个environment标签，每个environment标签可以定义一套连接配置

  default属性，用来指定使用哪个environment标签

  ```xml
  <environments default="mysql">
          <environment id="mysql">
              <!--transactionManager标签用于配置数据库管理方式-->
              <transactionManager type="JDBC"></transactionManager>
              <!--dataSource标签用来配置数据库连接信息-->
              <dataSource type="POOLED">
              	<property name="driver" value="com.mysql.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://localhost:3306/java_test?characterEncoding=utf-8"/>
                  <property name="username" value="root"/>
                  <property name="password" value="hsy031122"/>
              </dataSource>
          </environment>
      </environments>
  ```

  

# 三、MyBatis框架使用

> 学生信息的数据库操作

## 3.1 创建数据表

```mysql
create table tb_students(
    sid int primary key auto_increment,
    stu_num char(5) not null unique ,
    stu_name varchar(20) not null ,
    stu_gender char(2) not null ,
    stu_age int not null
);
```

## 3.2 创建实体类

导入lombok包

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
    <scope>provided</scope>
</dependency>
```

创建类

![image-20220719103032179](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103032179.png)

## 3.3 创建DAO接口，定义操作方法

```java
public interface StudentDAO {
    public int insertStudent(Student student);
}
```

## 3.4 创建DAO接口的映射文件

- 在==resources== 目录下，新建名为==mappers== 文件夹

- 在==mappers== 中新建名为==StudentMapper.xml== 的映射文件（根据模板创建）

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="">
  
  </mapper>
  ```

  mapper文件相当于DAO接口的实现类，namespace属性要指定实现DAO接口的全名称

- 在映射文件对DAO中定义的方法进行实现

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="com.java.dao.StudentDAO">
      <insert id="insertStudent" parameterType="com.java.pojo.Student">
          insert into tb_students(stu_num,stu_name,stu_gender,stu_age)
          values(#{stuNum},#{stuName},#{stuGender},#{stuAge})
      </insert>
      <delete id="deleteStudent">
          delete from tb_students where stu_num=#{stuNum}
      </delete>
  </mapper>
  ```

  其中如果接口中的方法定义了形参类型则parameterType可以不写

## 3.5 将映射文件添加到主配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/java_test?serverTimezone=UTC"/>
                <property name="username" value="root"/>
                <property name="password" value="hsy031122"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper resource="mappers/StudentMapper.XML"></mapper>
    </mappers>
</configuration>
```

**==注意：映射文件的后缀名必须大写（StudentMapper.XML）==** 

# 项目结构

![image-20220719103037255](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103037255.png)



# 四、单元测试

## 4.1 添加单元测试依赖

```xml
<dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
```



## 4.2 创建单元测试类

![image-20220719103042356](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103042356.png)

在类名后面右键---生成---测试

![image-20220719103045518](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103045518.png)

## 4.3 测试代码

```java
package com.java.dao;

import com.java.pojo.Student;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;


public class StudentDAOTest {

    @org.junit.Test
    public void insertStudent(){
        try {
            //获取核心配置文件是输入流
            InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
            //获取SqlSessionFactoryBuilder对象
            SqlSessionFactoryBuilder builder=new SqlSessionFactoryBuilder();
            //获取SqlSessionFactory对象
            SqlSessionFactory factory=builder.build(is);
            //获取sql的绘画对象sqlSession（不会自动提交事务），是Mybatis提供的操作数据库的对象
            SqlSession sqlSession=factory.openSession();
            //获取sql的绘画对象sqlSession（会自动提交事务），是Mybatis提供的操作数据库的对象
            //SqlSession sqlSession=factory.openSession(true);
            //获取StudentDAO接口的代理实现类对象
            StudentDAO studentDAO=sqlSession.getMapper(StudentDAO.class);
            //测试studentDAO接口中的方法，实现添加用户信息的功能
            int i = studentDAO.insertStudent(new Student(0, "10001", "张三", "男", 21));
            //提交事务
            sqlSession.commit();
            
            System.out.println(i);
			
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @org.junit.Test
    public void deleteStudent() {
    }
}
```

# 五、MyBatis的CRUD操作

> 案例：学生信息的增删改查

## 5.1 添加操作

略

## 5.2 删除操作

> 根据学号删除一条学生信息

- 在StudentDAO中定义删除方法

  ![image-20220719103051918](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103051918.png)

- 在StudentMapper.XML中对接口方法进行“实现”

  ![image-20220719103055008](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103055008.png)

- 测试：在Student DAO的测试类中添加测试方法

==注：编写代码完成后必须提交事务==

```java
@org.junit.Test
    public void deleteStudent() {
        try {
            //加载mybatis配置文件
            InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
            SqlSessionFactoryBuilder builder=new SqlSessionFactoryBuilder();
            //会话工厂
            SqlSessionFactory factory=builder.build(is);
            //会话（连接）
            SqlSession sqlSession=factory.openSession();
            //通过会话获取DAO的对象
            StudentDAO studentDAO=sqlSession.getMapper(StudentDAO.class);
            //测试studentDAO中的方法
            int i = studentDAO.deleteStudent("10001");
            sqlSession.commit();
            System.out.println(i);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

## 5.3 修改操作

> 根据学生学号，修改其他字段的信息

