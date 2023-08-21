# 静态文件

## 什么是静态文件

如：照片、css、js、音频、视频

## 静态文件配置

setting.py中

配置静态文件的访问路径【该配置默认存在】

通过哪个url地址找静态文件

`STATIC_URL = '/static/'`

说明：

指定访问静态文件时是需要通过`/static/xxx`或`http://127.0.0.1:8000/static/xxx`[xxx表示具体的静态资源位置]

配置静态文件存储路径STATICFILES_DIRS

STATICFILES_DIRS保持的是静态文件在服务器端的存储位置

```python
# file:setting.py
STATICFILES_DIRS = (
	os.path.join(BASE_DIR,'static'),
)
```

## 模板中访问静态文件

以img标签为例

通过`{% static %}`标签访问静态文件

1. 加载static - `{% load static %}`

2. 使用静态文件 - `{% static'静态资源路径' %}`

3. 样例：`<img src="{% static 'images/lena.jpg' %}">`

    ![image-20220914213717347](C:/Users/XYDN/AppData/Roaming/Typora/typora-user-images/image-20220914213717347.png)

当这里发生修改时

![image-20220914213819460](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220914213819460.png)

前两种方法都会失效，只有第三种方法仍然有效



# Django应用及分布式路由

# 模型层及ORM介绍

## 定义

负责跟数据库之间进行通信

## Django配置MySQL

- 安装mysqlclient[版本 mysqlclient 1.3.13以上]
- 安装前确认ubuntu是否安装python3-dev 和 default-libmysqlclient-dev
    1. `sudo apt list --installed|grep -E 'libmysqlclient-dev|python3-dev'`
    2. 若命令无输出则需要安装 - `sudo apt-get install python3-dev-default-libmysqlclient-dev`
- `sudo pip3 install mysqlclient`

- 创建数据库
- 进入mysql数据库 执行
    - create database  数据库名 default charset utf8
    - 通常数据库名根项目名保持一致
- settings.py里进行数据库的配置
    - 修改DATABASES配置项中的内容，由sqlite3变成mysql

 在__init__.py中文件中写入两行代码

```python
import pymysql
pymysql.install_as_MySQLdb()
```

配置数据库

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',//指定数据库存储引擎
        'NAME': 'django',//数据库名
        'USER': 'root',//用户名
        'PASSWORD': 'hsy031122',//数据库密码
        'HOST': '127.0.0.1',//ip地址（可以说服务器或本地）
        'PORT': '3306',//端口
    }
}
```

## 什么是模型

模型是一个Python类，它是由django.db.models.Model派生出的子类

一个模型类代表数据库中的一张数据表

模型类中的每一个类属性都代表数据库中的一个字段

模型是数据交互的接口，是表示和操作数据库的方法和方式

## ORM框架

### 定义

ORM（Object Relational Mapping）即对象关系映射，它是一种程序技术，它允许你使用类和对象对数据库进行操作，从而避免通过SQL语句操作数据库

### 作用

1. 建立模型类和表之间的对应关系，允许我们通过面向对象的方式来操作数据库
2. 根据设计的模型类生成数据库中的表格
3. 通过简单的配置就可以进行数据库的切换

### 优点

- 只需要面向对象编程，不需要面向数据库编写代码

    对数据库的操作都转化成对类属性和方法的操作

    不用编写各种数据库的SQL语句

- 实现了数据模型与数据库的解耦，屏蔽了不同数据库操作上的差异

    不在关注用的是mysql、orcale等数据库的内部细节

    通过简单的配置就可以轻松更换数据库，而不需要修改代码

### 缺点

对于复杂业务，使用成本较高

根据对象的操作转换成SQL语句，根据查询的结果转换成对象，在映射过程中有映射损失

### 映射图

![image-20220909150851453](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220909150851453.png)

### 模型示例

添加一个bookstore_book数据表来存放图书馆中数目信息

#### 添加app

```python
python manage.py startapp bookstore
```

#### 添加模型类并注册app

![image-20220909153201665](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220909153201665.png)

模型类代码示例

```python
#file:bookstore/model.py
class Book(models.Model):
    title = models.CharField('书名', max_length=50, default='')
    price = models.DecimalField('价格', max_digits=7, decimal_places=2)
#default:指定默认值
#max_digis:数字最大位数
#decimal_places:指定有几位小数
```

#### 数据库迁移

迁移Django同步你对模型所做的更改（添加字段，删除模型等）到你数据库模式的方式

生成迁移文件 - 执行`python3 manage.py makemigrations`

将应用下的models.py文件生成一个中间文件，并保存在migrations文件夹中

执行迁移脚本程序 - 执行`python3 manage.py migrate`

执行迁移程序实现迁移，将每个应用下的migrations目录中的中间文件同步会数据库

### 模型类 - 创建

```python
from django.db import models
class 模型类名(models.Model):
    字段名 = models.字段类型(字段选项)
```



# 基础字段及选项

## 创建模型类流程

- 创建应用

- 在应用下的models.py中编写模型类

    ```python
    from django.db import models
    class 模型类名(models.Model):
        字段名 = models.字段类型(字段选项)
    ```

- 迁移同步 makemigrations&migrate

- 任何关于表结构的修改，务必在对应模型类上修改

- 例：为bookstore_book表添加一个info字段varchar(100)

    解决方案：模型类中添加对应属性，执行数据库迁移

## 字段类型

### BooleanField()

数据库类型：tinyint(1)

编程语言中：使用True或False来表示值

在数据库中：使用1或者0来表示具体的值

### CharField()

数据库类型：varchar

注意：==必须要指定max_length参数值==

### DateField()

数据库类型：date

作用：表示日期

参数：

1. auto_now：每次保存对象时，自动设置该字段为当前时间（取值：True/False）
2. auto_now_add：当对象第一次被创建时自动设置当前时间（取值：True/False）
3. default：设置当前时间（取值：字符串格式时间如：'2019-6-1'）

==以上参数只能多选一==

### DateTimeField()

数据库类型：datetime(6)

作用：表示日期和时间

参数同DateField

### FloatField()

数据库类型：double

编程语言中和数据库中都使用小数表示值

### DecimalField()

数据库类型：decimal(x,y)

编程语言中：用小数表示该列的值

在数据库中：使用小数

参数：

max_digits：位数总数，包括小数点后的位数。该值必须大于等于decimal_places

decimal_places：小数点后的数字总量

### EmailField()

数据库类型：varchar

编程语言和数据库中使用字符串

### IntegerField()

数据库类型：int

编程语言和数据库中使用整数

### ImageField()

数据库类型：varchar(100)

作用：在数据库中为了保存图片的路径

编程语言和数据库中使用字符串

### TextField()

数据库类型：longtext

作用：表示不定长的字符数据

## 字段选项

字段选项，指定创建的列的额外的信息

允许出现多个字段选项，多个字段之间使用`,`隔开

### primary_key

如果设置为True，表示该列为主键，如果指定一个字段为主键，则ci数据库表不会创建id字段

### blank

设置为True时，字段可以为空。设置为False时，字段是必须填写的

### null

如果设置为True，表示该列值允许为空

默认为False，如果此项为False建议加入default选项来设置默认值

### default

设置所在列的默认值，如果字段选项null=False建议添加此项

### db_index

如果设置为True，表示为该列增加索引

### unique

如果设置为True，表示该字段在数据库中的值必须是唯一的（不难重复出现）

### db_column

指定列的名称，如果不指定的话则采用属性名作为列名

### verbose_name

设置此字段在admin界面上显示的名称

### 样例

```python
#创建一个属性，表示用户名称，长度30字符，必须是唯一的，不能为空，添加索引
name = 	models.CharField(max_length=30, unique=True, null=False, de_index=True)
```



## Meta类

### 定义

使用内部类Meta类来给模型赋予属性，Meta类下有很多内建的类属性，可对模型类做一些控制

```python
#file:bookstore/models.py
from django.db import models

class Book(models.Model):
    title = models.CharField('书名', max_length=50, default='')
    price = models.DecimalField('价格', max_digits=7, decimal_places=2)
    class Meta:
        db_table = 'book' #可改变当前模型类对应的表名
```

### 修改模型类

![image-20220909173633842](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220909173633842.png)

![image-20220909173930096](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220909173930096.png)

![image-20220909174000261](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220909174000261.png)



# 基本操作 - 创建数据

## 常见问题汇总

### 问题1

执行`python3 manage.py makemigrations`出现如下迁移错误时的处理方法

![image-20220909174725136](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220909174725136.png)

#### 原因

当对模型类新添加一个字段时可出现该错误

原因是添加新字段之后，数据库不知道原来已有的数据对于新建字段如何赋值，所以新增字段时，务必要添加default默认值

#### 处理方法

选择1：进入到shell中，手动输入一个默认值

选择2：退出目前生成迁移文件的过程，自己去修改models.py，新增加一个`default=xxx`的缺省值（推荐）

### 问题2

数据库迁移文件混乱的解决方法

数据库中django_migrations表记录了migrate的'全过程'，项目各应用中的migrate文件与之相对应，否则migrate会报错

#### 解决方法

1. 删除所有migrations里的所有000?_XXXX.py( _init _.py除外)

2. 删除数据库

3. 重新创建数据库

4. 重新生成migrations里所有的000?_XXXX.py

    ```python
    python3 manage.py makemigrations
    ```

5. 重新更新数据库

    ```python
    python3 manage.py migrate
    ```

## 创建数据

### 操作

基本操作包括增删改查操作，即（CRUD操作）

ORM CRUD 核心 - > 模型类.管理器对象

### 管理器对象

每个继承自models.Model的模型类，都会有一个objects对象被同样继承下来。这个对象叫管理器对象

数据库的增删改查可以通过模型的管理器实现

```python
class MyModel(models.Model):
    . . .
    MyModel.objects.create(...)//objects是管理器对象
```

### 创建数据

Django ORM使用一种直观的方式把数据库表中的数据表示成Python对象

创建数据中的每一条记录就是创建一个数据对象

#### 方式一

```python
MyModel.objects.create(属性1=值1，属性2=值1,...)
```

成功：返回创建好的实体对象

失败：抛出异常

#### 方式二

创建MyModel实例对象，并调用save()进行保存

```python
obj = MyModel(属性=值，属性=值)
obj.属性=值
obj.save()
```

### Django Shell

在Django提供了一个交互式的操作项目叫Django Shell 它能够在交互模式用项目工程的代码执行相应的操作

利用Django Shell 可以代替编写view的代码来直接进行操作

注意：项目代码发生变化时，重新进入Django Shell

#### 启动方式

`python3 manage.py shell`

![image-20220914200741059](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220914200741059.png)

mysql中的数据表

![image-20220914200828039](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220914200828039.png)
