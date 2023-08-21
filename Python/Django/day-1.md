# Django介绍

## 起源

- 2005年发布，采用python语言编写的web框架
- 早期的时候，Django主做新闻和内容管理的
- 重量级的python web框架，Django配备了常用的大部分组件

## 组件

- 基本配置文件/路由系统
- 模型层(M)/模板层(T)/视图层(V)
- Cookies和Session
- 分页及发邮件
- Admin管理后台

## 用途

1. 网站/微信公众号/小程序后端开发
2. 人工智能平台融合

## 官网

官网：http://www.djangoproject.com

官方文档：https://docs.djangoproject.com/zh-hans/4.1/

## 安装

使用pip install django==3.2.13下载django

检查是否成功 sudo pip3 freeze|grep -i 'Django'

# 项目结构

## 创建项目

成功安装Django后，虚拟机终端会有django-admin命令

执行`django-admin startproject 项目名`即可创建出对应项目文件夹

例如，终端执行`django-admin startproject mysite1;`则创建出mysite1项目

创建完成后可以在终端输入`ls`查看当前目录下的文件夹

## 启动服务

1. 终端`cd`进入项目文件夹，例如，`cd mysite1;`

2. 进入到项目文件夹后，执行`python manager.py runserver`启动Django服务

    注：在此启动方式下，Django在前台启动服务，默认监听8000端口

3. 浏览器访问`http://127.0.0.1:8000`可看到Django的启动页面

    注：如果想要更换端口，则可以使用`python3 manager.py runserver 端口号`

## 关闭服务

1. 在runsever启动终端下，执行Ctrl+C可关闭Django服务
2. 在其他终端下，执行`sudo lsof -i:8000`查询出Django的进程id；执行`kill -9 对应Django进程的id`

## 创建应用

执行下面这段代码，创建Django应用

```shell
python manage.py startapp ying'y
```

## 常见问题

启动时报错`Error:That port is already in use`

原因：端口已被占用，默认端口8000已被其他进程占用

解决：关闭服务

## 结构解析

### manage.py

展开mysite1项目，结构如下：

![image-20220831124011164](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220831124011164.png)

manager.py包含项目管理的所有子命令，如：

- python3 manager.py runserver 启动服务
- python3 manager.py startapp 创建应用
- python3 manager.py migrate 数据库迁移
- 直接执行python3 manager.py 可以列出所有的Django子命令

### 项目同名文件夹

- —int—:Python包的初始化文件
- wsgi.py:WEB服务网关的配置文件-Django正式启动的时候，需要用到
- urls.py:项目的主路由配置-HTTP请求进入Django时，优先调用该文件
- settings.py:项目的配置文件-包含项目启动时需要的配置

### settings.py

- settings.py包含了Django项目启动的所有配置项
- 配置项分为公有配置和自定义配置
- 配置项格式例：`BASE_DIR = 'XXXX'`
- 公有配置-Django官方提供的公有配置

![image-20220831131442719](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220831131442719.png)

DEBUG是项目的启动模式：

True：调试模式

检测到代码改动后，立即重启服务

False：上线模式

报错页面

![image-20220831161732223](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220831161732223-16619338526011.png)

ALLOWED_HOSTS

设置允许访问到本项目的Host头值

- []空列表，表示只有请求头中host为127.0.0.1，localhost才能访问此项目 -DEBUG="TRUE"时有效
- [*]，表示任何请求头的host都能访问到当前项目
- ['192.168.1.3','127.0.0.1']表示只有前面两个hsot头的值可以访问当前项目

示例：如果要在局域网其他主机也能访问此主机的Django服务，启动方式如下：

- `python3 manager.py runserver 0.0.0.0:5000`
- 指定网络设备如果内网环境下其他主机想正常访问该站点，需加`ALLOWED_HOSTS = ['内网ip']`

BASE_DIR

用于绑定当前项目的绝对路径（动态计算出来的），所有文件夹都可以依赖于此路径

INSTALLED_APPS:指定当前目录中安装的应用列表

MIDDLEWARE:用于注册中间件

TEMPLATES:用于指定模板的配置信息

DATEBASES:用于指定数据库的配置信息

LANGUAGE_CODE:用于指定语言配置

	- 英文："en-us"
	- 中文："zh-Hans"

TIME_ZONE:用于指定当前服务器端时区

	- 世界标准时间："UTF"
	- 中国时区："Asia/Shanghai"

ROOT_URLCONF: 用于配置主url配置'mysites1.urls'

​	ROOT_URLCONF = 'mysites.urls'

setting.py中也可以添加开发人员自定义的配置

配置建议：名字尽量个性化，以免覆盖掉公共配置

例如：ALIPAY_KEY='XXXX'

setting.py中的所有配置项，都可以在代码中引入

引入方式：`from django.conf import settings`

# URL与视图函数

## URL

### 定义

统一资源定位符 Uniform Resource Locator

### 作用

用来表示互联网上某个资源的地址

### 语法

URL的一般语法为（注[ ]中内容可省略）

- protocol://hostname[:port]/path[?query] [#fragment]
- http://tts.tmooc.cn/video/showVideos?menuld=657421&version=AID999#subject

### 结构

#### protocol（协议）

- http 通过HTTP访问资源。格式：http：//
- https通过安全的HTTPS访问该资源。格式：https：//
- file资源是本地计算机上的文件。格式：file：///

#### hostname（主机名）

- 是指存放资源的服务器的域名系统（DNS）主机名、域名或IP地址

#### port（端口号）

- 整数，可选，省略时使用方案吗，默认的端口号
- 各种传输协议都有默认的端口号，如http的默认端口为80

#### path（路由地址）

由零或多个“ / ”符号隔开的字符串，一般用来表示主机上的一个目录或文件地址。路由地址决定了服务端如何处理这个请求

#### query（查询）

可选，用于给动态网页传递参数，可有多个参数，用“ & ”符隔开，每个参数的名和值用“ = ”符号隔开

#### fargment（信息片断）

字符串，用于指定网络资源中的片断。例如一个网页中有多个名词解释，可使用fargment直接定位到某一个名词解释

## Django处理URL请求

浏览器 地址栏 - - ->http://127.0.0.1:8000/page/2003

1. Django从配置文件中根据ROOT_URLCONF找到主路由文件；默认情况下该文件在项目同名目录下的urls；例如mysite1/mysite1/urls.py
2. Django加载主路由文件中的urlpatterns变量（包含很多路由的数组）
3. 依次匹配urlpatterns中的path，匹配到第一个合适的中断后续匹配
4. 匹配成功，调用相应的视图函数处理请求，返回响应
5. 匹配失败，返回404响应

主路由-urls.py样例：

![image-20220831183000938](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220831183000938.png)

## 视图函数

视图函数是用于接收一个浏览器请求（HttpRequest对象）并通过HttpResponse对象返回响应的函数。此函数可以接收浏览器的请求并根据业务逻辑返回相应的相应内容给浏览器

语法：

```python
def xxx_view(request[,其他参数]):
    return HttpResponse对象;
```

样例：

#file:<项目同名文件夹下>/views.py

```python
from django.http import HttpResponse
def page1_view(request):
    html = "<h1>这是第一个页面</h1>"
    return HttpRespon()
```

![image-20220831191422030](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220831191422030.png)

![image-20220831191437457](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220831191437457.png)

![image-20220908202536330](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220908202536330.png)

# 路由配置

## path

### path( )函数

#### 导入

`from django.urls import path`

#### 语法

`path(route,views,name=None)`

#### 参数

1. route：字符串类型，匹配的请求路径
2. views：指定路径所对应的视图处理函数的名称，注意不要在函数后面加( )，不然引用的将是函数的结果
3. name：为地址起别名，在模板中地址反向解析时使用

### path转换器

#### 语法

`<转换器类型：自定义名>`

#### 作用

若转换器类型匹配到对应类型的数据，则将数据按照关键字传参的方式传递给视图函数

#### 例子

```python
path('page/<int:page>',views.xxx)
```

![image-20220831212847057](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220831212847057.png)

urls：

![image-20220831213131585](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220831213131585.png)

views：

![image-20220831213147668](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220831213147668.png)

l

#### 练习

小计算器

定义一个路由格式为：`http://127.0.0.1:8000/整数/操作字符串/[add,sub/mul]/整数`

从路由中提取数据，做相应操作后返回给浏览器

效果如下：

输入：`http://127.0.0.1:8000/100/add/200`

页面显示结果：300

urls：

![image-20220901100319932](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220901100319932.png)

views：

![image-20220901100337464](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220901100337464.png)

### re_path( )函数

在url的匹配过程中可以使用正则表达式进行精确匹配

语法：

```python
re_path(reg,view,name=xxx)
```

正则表达式命名分组模式（?P< name >pattern）；匹配提取参数之后用关键字传参方式传递给视图函数

#### 样例

![image-20220901162342938](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/image-20220901162342938.png)



