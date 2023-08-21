# 一、Maven简介

## 1.1在项目中如何导入jar包？

- 下载jar包
- 将下载的jar包拷贝到项目中（WEB-INF/lib）
- 选择jar文件--右键--Add as Library

## 1.2传统导入jar包的方式存在什么问题？

- 步骤多（相对）--繁琐
- 在不同的项目中如果需要相同的jar包，需要分别存储这个jar文件--冗沉、项目体积大
- 在不同的环境下可能因为jar文件版本不一致导致项目无法运行（重新配置）--移植性差

## 1.3项目生命周期

>项目从编译到运行的整个过程
>
>完整的生命周期：清理缓存--校验--编译--测试--打包--安装--部署

- IDEA 提供了一键构建项目的功能，但是如果我们需要自定义的生命周期管理，却没有现成的工具（清理缓存）

## 1.4Maven简介

> Maven是`基于项目对象模型（POM）用于进行项目的依赖管理、生命周期管理的`工具软件



# 二、Maven安装及配置

==注意：== 创建Maven项目时出现，如下错误时：

```xml
java.lang.RuntimeException: java.lang.RuntimeException: org.codehaus.plexus.component.repository.exception.ComponentLookupException:\n 
com.google.inject.ProvisionException: Unable to provision, see the following errors:
```

原因是Maven版本过高，解决方法为更改Maven版本（使用IDEA自带的Maven即可）

# 三、Maven的项目结构

> 使用Maven进行项目开发还有一个好处：无论什么样的开发工具（eclipse/idea）项目的结构都是统一的

## 3.1Maven的项目结构

```
fmwy(项目名称)
--src
  --main(存放项目的源文件)
    --java(存放Java代码，相当于传统项目中的src目录)
    --resoutces(存放配置文件和静态资源的目录，相当于传统项目中的web目录)
  --test(存放项目的单元测试代码)
--pom.xml
```

## 3.2 pom.xml

> POM :Project Object Model，Maven可以根据pom文件的配置对此项目进行依赖管理；也就是说项目中需要的依赖，直接在pom.xml中进行配置即可

```xml
<?xml version="1.0" encoding="utf-8">
<project 配置规则>
	<!--指定项目模型版本-->
 	<modelVersion>4.0</modelVersion>
	<!--指定项目的标识：G 企业标识 A 项目标识 V 版本-->
	<groupId>com.qfedu</groupId>一般使用包的名称
	<artifactId>fmwy</artifactId>项目名
	<version>1.0.0</version>
	
	<dependencies>

	</dependencies>
</project>
```

> 如果需要在当前项目中添加依赖，只需要在pom.xml文件中配置即可

**配置依赖：**找到依赖的坐标，添加到dependencies标签中

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connection-java</artifactId>
    <version>5.1.4</version>
</dependency>
```

# 四、依赖管理

## 4.1Maven是如何进行依赖管理的？

![image-20220719103104789](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103104789.png)

## 4.2Maven仓库介绍

- ==本地仓库== 就是本地计算机上的某个文件夹（可以说自定义的任何文件夹）
- ==远程仓库== 就是远程主机上的jar仓库
  - ==中央仓库== Maven官方提供的仓库，包含了所需的一切依赖（免配置）
  - ==公共仓库== 除了中央仓库以外的第三方仓库都是公共仓库，例如aliyun（需要配置）
  - ==私服== 企业搭建的供内部使用的Maven仓库

![image-20220719103109828](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103109828.png)

## 4.3Maven仓库配置

- 在maven_home/conf/setting.xml中进行配置

**配置本地仓库**

```xml
<localRepository>d:\repo</localRepository>
```

**配置公共仓库**

```xml
<mirrors>
    <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>central</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
</mirrors>
```

# 五、项目生命周期管理

## 5.1生命周期介绍

> 项目构建的生命周期：项目开发结束之后部署到运行环境运行的过程
>
> - 清除缓存
> - 检查
> - 编译（将java文件编译成class文件）
> - 测试（就会执行Maven项目中test目录下的单元测试方法）
> - 打包（Java项目打包成jar包、web项目打包成war包）
> - 安装（jar、war包会被安装到本地仓库）
> - 部署（将项目生成的包放到服务器中）

## 5.2生命周期管理指令

> 在项目的根目录下执行mvn指令（此目录必须包含pom.xml）

- 清除缓存

  ```
  mvn clean
  ```

- 检查

  ```
  mvn check
  ```

- 编译

  ```
  mvn compile
  ```

- 测试

  ```
  mvn test
  ```

- 打包

  ```
  mvn package
  ```

- 安装

  ```
  mvn install
  ```

- 部署

  ```
  mvn deploye
  ```

# 六、基于IDEA的Maven使用

## 6.1在IDEA中关联Maven

![image-20220719103115323](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103115323.png)

![image-20220719103119072](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103119072.png)

==说明：==IDEA本身集成了Maven，考虑到IDEA和Maven版本的兼容性，IDEA不建议配置比默认版本更新的版本，建议使用IDEA自带的Maven

## 6.2使用IDEA创建Maven项目

### 6.2.1 Java项目

![image-20220719103122880](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103122880.png)

![image-20220719103126568](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103126568.png)

==版本：== 版本常用的两个后缀：SNAPSHOP(快照版) RELEASE(发布版)

==版本号结构：== 版本号通常由三位数字构成	

- 第一个数字：大版本
- 第二个数字：小版本
- 第三个数字：bug修复量

### 6.2.2 web项目

- 创建Maven项目

- 在pom.xml文件中设置打包方式为war

  ```xml
  <!--设置打包方式为war-->
  <packaging>war</packaging>
  ```

- 完成web项目结构

  ![image-20220719103131010](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103131010.png)

- 配置web组件--Tomcat

  ![image-20220719103134641](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103134641.png)

  ![image-20220719103138159](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103138159.png)

  ![image-20220719103141646](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103141646.png)

- 部署web项目

  ![image-20220719103145693](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220719103145693.png)

## 6.3在IDEA中使用Maven进行依赖管理

### 6.3.1 查找依赖坐标

- https://mvnrepository.com/

### 6.3.2 添加依赖

- 将依赖的坐标配置到项目的pom.xml文件dependencies标签中

### 6.3.3 依赖范围

> 在通过dependency添加依赖时，可以通过==scope== 标签配置当前依赖的适用范围。例：

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.18</version>
    <scoe>provided</scoe>
</dependency>
```

- test 只在项目测试阶段引入当前的依赖（编译、测试）
- runtime 只在运行时使用（编译、运行、测试运行）
- provided 在（编译、测试、运行）
- compile 在（编译、测试、运行、打包）都引入

## 6.4 在IDEA中使用Maven进行项目构建

### 6.4.1 Maven项目构建生命周期说明

- clean 清理缓存 清理项目生成的缓存
- validate 校验 验证项目需要是正确的（项目信息、依赖）
- compile 编译 编译项目专供的代码
- test 测试 运行项目中的单元测试
- package 打包 将项目编译后的代码打包成发布格式
- verify 检查 对集成测试的结果进行检查、确保项目的质量是达标的
- install 安装 将包安装到Maven的本地仓库，以便在本地的其他项目中可以引用此项目（聚合工程）
- deploy 部署 将包安装到私服的仓库，以供其他开发人员共享