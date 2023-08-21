# Web基础

![img](Go%20Web.assets/navi3.png)

# Web工作方式

对于普通的上网过程，系统其实是这样做的：浏览器本身是一个客户端，当你输入 URL 的时候，首先浏览器会去请求 DNS 服务器，通过 DNS 获取相应的域名对应的 IP，然后通过 IP 地址找到 IP 对应的服务器后，要求建立 TCP 连接，等浏览器发送完 HTTP Request（请求）包后，服务器接收到请求包之后才开始处理请求包，服务器调用自身服务，返回 HTTP Response（响应）包；客户端收到来自服务器的响应后开始渲染这个 Response 包里的主体（body），等收到全部的内容随后断开与该服务器之间的 TCP 连接。

![image-20230224130715738](Go%20Web.assets/image-20230224130715738.png)

上面这个图展示了用户访问一个 Web 站点的过程

一个 Web 服务器也被称为 HTTP 服务器，它通过 HTTP 协议与客户端通信。这个客户端通常指的是 Web 浏览器 (其实手机端客户端内部也是浏览器实现的)。

Web 服务器的工作原理可以简单地归纳为：

- 客户机通过 TCP/IP 协议建立到服务器的 TCP 连接
- 客户端向服务器发送 HTTP 协议请求包，请求服务器里的资源文档
- 服务器向客户机发送 HTTP 协议应答包，如果请求的资源包含有动态语言的内容，那么服务器会调用动态语言的解释引擎负责处理 “动态内容”，并将处理得到的数据返回给客户端
- 客户机与服务器断开。由客户端解释 HTML 文档，在客户端屏幕上渲染图形结果

一个简单的 HTTP 事务就是这样实现的，看起来很复杂，原理其实是挺简单的。需要注意的是客户机与服务器之间的通信是非持久连接的，也就是当服务器发送了应答后就与客户机断开连接，等待下一次请求

## URL和DNS解析

URL (Uniform Resource Locator) 是 “统一资源定位符” 的英文缩写，用于描述一个网络上的资源，基本格式如下

```
scheme://host[:port#]/path/.../[?query-string][#anchor]
scheme         指定底层使用的协议(例如：http, https, ftp)
host           HTTP 服务器的 IP 地址或者域名
port#          HTTP 服务器的默认端口是 80，这种情况下端口号可以省略。如果使用了别的端口，必须指明，例如 http://www.cnblogs.com:8080/
path           访问资源的路径
query-string   发送给 http 服务器的数据
anchor         锚
```

DNS (Domain Name System) 是 “域名系统” 的英文缩写，是一种组织成域层次结构的计算机和网络服务命名系统，它用于 TCP/IP 网络，它从事将主机名或域名转换为实际 IP 地址的工作。DNS 就是这样的一位 “翻译官”，它的基本工作原理可用下图来表示。

![image-20230224130849501](Go%20Web.assets/image-20230224130849501.png)

更详细的 DNS 解析的过程如下：

1. 在浏览器中输入 www.qq.com 域名，操作系统会先检查自己本地的 hosts 文件是否有这个网址映射关系，如果有，就先调用这个 IP 地址映射，完成域名解析。

2. 如果 hosts 里没有这个域名的映射，则查找本地 DNS 解析器缓存，是否有这个网址映射关系，如果有，直接返回，完成域名解析。

3. 如果 hosts 与本地 DNS 解析器缓存都没有相应的网址映射关系，首先会找 TCP/IP 参数中设置的首选 DNS 服务器，在此我们叫它本地 DNS 服务器，此服务器收到查询时，如果要查询的域名，包含在本地配置区域资源中，则返回解析结果给客户机，完成域名解析，此解析具有权威性。

4. 如果要查询的域名，不由本地 DNS 服务器区域解析，但该服务器已缓存了此网址映射关系，则调用这个 IP 地址映射，完成域名解析，此解析不具有权威性。

5. 如果本地 DNS 服务器本地区域文件与缓存解析都失效，则根据本地 DNS 服务器的设置（是否设置转发器）进行查询，如果未用转发模式，本地 DNS 就把请求发至 “根 DNS 服务器”，“根 DNS 服务器” 收到请求后会判断这个域名 (.com) 是谁来授权管理，并会返回一个负责该顶级域名服务器的一个 IP。本地 DNS 服务器收到 IP 信息后，将会联系负责 .com 域的这台服务器。这台负责 .com 域的服务器收到请求后，如果自己无法解析，它就会找一个管理 .com 域的下一级 DNS 服务器地址 (qq.com) 给本地 DNS 服务器。当本地 DNS 服务器收到这个地址后，就会找 qq.com 域服务器，重复上面的动作，进行查询，直至找到 www.qq.com 主机。

6. 如果用的是转发模式，此 DNS 服务器就会把请求转发至上一级 DNS 服务器，由上一级服务器进行解析，上一级服务器如果不能解析，或找根 DNS 或把转请求转至上级，以此循环。不管是本地 DNS 服务器用是是转发，还是根提示，最后都是把结果返回给本地 DNS 服务器，由此 DNS 服务器再返回给客户机。

![image-20230224131043471](Go%20Web.assets/image-20230224131043471.png)

## HTTP协议详解

HTTP 是一种让 Web 服务器与浏览器 (客户端) 通过 Internet 发送与接收数据的协议，它建立在 TCP 协议之上，一般采用 TCP 的 80 端口。它是一个请求、响应协议 -- 客户端发出一个请求，服务器响应这个请求。在 HTTP 中，客户端总是通过建立一个连接与发送一个 HTTP 请求来发起一个事务。服务器不能主动去与客户端联系，也不能给客户端发出一个回调连接。客户端与服务器端都可以提前中断一个连接。例如，当浏览器下载一个文件时，你可以通过点击 “停止” 键来中断文件的下载，关闭与服务器的 HTTP 连接。

HTTP 协议是无状态的，同一个客户端的这次请求和上次请求是没有对应关系，对 HTTP 服务器来说，它并不知道这两个请求是否来自同一个客户端。为了解决这个问题， Web 程序引入了 Cookie 机制来维护连接的可持续状态。

### HTTP请求包(浏览器信息)

 Request 包的结构，Request 包分为 3 部分，第一部分叫 Request line（请求行）, 第二部分叫 Request header（请求头）, 第三部分是 body（主体）。header 和 body 之间有个空行，请求包的例子所示:

```
GET /domains/example/ HTTP/1.1      // 请求行: 请求方法 请求 URI HTTP 协议/协议版本
Host：www.iana.org               // 服务端的主机名
User-Agent：Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.4 (KHTML, like Gecko) Chrome/22.0.1229.94 Safari/537.4          // 浏览器信息
Accept：text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8  // 客户端能接收的 mine
Accept-Encoding：gzip,deflate,sdch       // 是否支持流压缩
Accept-Charset：UTF-8,*;q=0.5        // 客户端字符编码集
// 空行,用于分割请求头和消息体
// 消息体,请求资源参数,例如 POST 传递的参数
```

HTTP 协议定义了很多与服务器交互的请求方法，最基本的有 4 种，分别是 GET, POST, PUT, DELETE。一个 URL 地址用于描述一个网络上的资源，而 HTTP 中的 GET, POST, PUT, DELETE 就对应着对这个资源的查，增，改，删 4 个操作。我们最常见的就是 GET 和 POST 了。GET 一般用于获取 / 查询资源信息，而 POST 一般用于更新资源信息

GET 和 POST 的区别:

1. 我们可以看到 GET 请求消息体为空，POST 请求带有消息体。
2. GET 提交的数据会放在 URL 之后，以 ? 分割 URL 和传输数据，参数之间以 & 相连，如 EditPosts.aspx?name=test1&id=123456。POST 方法是把提交的数据放在 HTTP 包的 body 中。
    GET 提交的数据大小有限制（因为浏览器对 URL 的长度有限制），而 POST 方法提交的数据没有限制。
3. GET 方式提交数据，会带来安全问题，比如一个登录页面，通过 GET 方式提交数据时，用户名和密码将出现在 URL 上，如果页面可以被缓存或者其他人可以访问这台机器，就可以从历史记录获得该用户的账号和密码。

### HTTP响应包(服务器信息)

HTTP的response包，结构如下：

```
HTTP/1.1 200 OK                     // 状态行
Server: nginx/1.0.8                 // 服务器使用的 WEB 软件名及版本
Date: Tue, 30 Oct 2012 04:14:25 GMT     // 发送时间
Content-Type: text/html             // 服务器发送信息的类型
Transfer-Encoding: chunked          // 表示发送 HTTP 包是分段发的
Connection: keep-alive              // 保持连接状态
Content-Length: 90                  // 主体内容长度
// 空行 用来分割消息头和主体
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"... // 消息体
```

Response 包中的第一行叫做状态行，由 HTTP 协议版本号， 状态码， 状态消息三部分组成。

状态码用来告诉 HTTP 客户端，HTTP 服务器是否产生了预期的 Response。HTTP/1.1 协议中定义了 5 类状态码， 状态码由三位数字组成，第一个数字定义了响应的类别

- 1XX 提示信息 - 表示请求已被成功接收，继续处理
- 2XX 成功 - 表示请求已被成功接收，理解，接受
- 3XX 重定向 - 要完成请求必须进行更进一步的处理
- 4XX 客户端错误 - 请求有语法错误或请求无法实现
- 5XX 服务器端错误 - 服务器未能实现合法的请求

### HTTP 协议是无状态的和 Connection: keep-alive 的区别

无状态是指协议对于事务处理没有记忆能力，服务器不知道客户端是什么状态。从另一方面讲，打开一个服务器上的网页和你之前打开这个服务器上的网页之间没有任何联系。

HTTP 是一个无状态的面向连接的协议，无状态不代表 HTTP 不能保持 TCP 连接，更不能代表 HTTP 使用的是 UDP 协议（面对无连接）。

从 HTTP/1.1 起，默认都开启了 Keep-Alive 保持连接特性，简单地说，当一个网页打开完成后，客户端和服务器之间用于传输 HTTP 数据的 TCP 连接不会关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的 TCP 连接。

Keep-Alive 不会永久保持连接，它有一个保持时间，可以在不同服务器软件（如 Apache）中设置这个时间。



# Go搭建一个Web服务器

Go语言中提供了一个完善的net/http包，通过http包可以很快搭建一个Web服务，还能很简单的对Web的路由，静态文件，模板，cookie等数据进行设置

## http包建立Web服务器

```go
package main

import (
    "fmt"
    "net/http"
    "strings"
    "log"
)

func sayhelloName(w http.ResponseWriter, r *http.Request) {
    r.ParseForm()  // 解析参数，默认是不会解析的
    fmt.Println(r.Form)  // 这些信息是输出到服务器端的打印信息
    fmt.Println("path", r.URL.Path)
    fmt.Println("scheme", r.URL.Scheme)
    fmt.Println(r.Form["url_long"])
    for k, v := range r.Form {
        fmt.Println("key:", k)
        fmt.Println("val:", strings.Join(v, ""))
    }
    fmt.Fprintf(w, "Hello astaxie!") // 这个写入到 w 的是输出到客户端的
}

func main() {
    http.HandleFunc("/", sayhelloName) // 设置访问的路由
    err := http.ListenAndServe(":9090", nil) // 设置监听的端口
    if err != nil {
        log.Fatal("ListenAndServe: ", err)
    }
}
```



## Go如何使Web工作

### Web工作方式的几个概念

Request：用户请求的信息，用来解析用户的请求信息，包括 post、get、cookie、url 等信息

Response：服务器需要反馈给客户端的信息

Conn：用户的每次请求链接

Handler：处理请求和生成返回信息的处理逻辑

### 分析http包运行机制

![image-20230224165413460](Go%20Web.assets/image-20230224165413460.png)

http包执行流程：

1. 创建 Listen Socket, 监听指定的端口，等待客户端请求到来。

2. Listen Socket 接受客户端的请求，得到 Client Socket, 接下来通过 Client Socket 与客户端通信。

3. 处理客户端的请求，首先从 Client Socket 读取 HTTP 请求的协议头，如果是 POST 方法，还可能要读取客户端提交的数据，然后交给相应的 handler 处理请求，handler 处理完毕准备好客户端需要的数据，通过 Client Socket 写给客户端。



# 表单

![image-20230224170411565](Go%20Web.assets/image-20230224170411565.png)

## 处理表单的输入

新建一个`login.gtpl`文件

```gtpl
<html>
<head>
<title></title>
</head>
<body>
<form action="/login" method="post">
    用户名:<input type="text" name="username">
    密码:<input type="password" name="password">
    <input type="submit" value="登录">
</form>
</body>
</html>
```

源代码

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"strings"
	"text/template"
)

func sayHelloName(w http.ResponseWriter, r *http.Request) {
	r.ParseForm()
	for k,v := range r.Form{
		fmt.Println("key",k)
		fmt.Println("val",strings.Join(v,""))
	}
	fmt.Fprintf(w,"Hello astaxie!")
}

func login(w http.ResponseWriter, r *http.Request)  {
	fmt.Println("method:",r.Method)
	if r.Method == "GET" {
		t,_ := template.ParseFiles("login.gtpl")
		log.Println(t.Execute(w,nil))
	}else {
		err := r.ParseForm()
		if err!= nil {
			log.Fatal("ParseForm:",err)
		}
		fmt.Println("username:",r.Form["username"])
		fmt.Println("password:",r.Form["password"])
	}
}

func main(){
	http.HandleFunc("/",sayHelloName)
	http.HandleFunc("/login",login)
	err := http.ListenAndServe(":8000",nil)
	if err != nil{
		log.Fatal("LinstenAndServer",err)
	}
}
```

> 注意：默认情况下，Handler 里面是不会自动解析 form 的，必须显式的调用 `r.ParseForm()` 后，才能对表单数据进行操作

r.Form 里面包含了所有请求的参数，比如 URL 中 query-string、POST 的数据、PUT 的数据，所以当你在 URL 中的 query-string 字段和 POST 冲突时，会保存成一个 slice，里面存储了多个值



`request.Form` 是一个 url.Values 类型，里面存储的是对应的类似 `key=value` 的信息，下面展示了可以对 form 数据进行的一些操作:

```go
v := url.Values{}
v.Set("name", "Ava")
v.Add("friend", "Jess")
v.Add("friend", "Sarah")
v.Add("friend", "Zoe")
// v.Encode() == "name=Ava&friend=Jess&friend=Sarah&friend=Zoe"
fmt.Println(v.Get("name"))
fmt.Println(v.Get("friend"))
fmt.Println(v["friend"])
```

> Tips:
> Request 本身也提供了 FormValue () 函数来获取用户提交的参数。如 r.Form ["username"] 也可写成 r.FormValue ("username")。调用 r.FormValue 时会自动调用 r.ParseForm，所以不必提前调用。r.FormValue 只会返回同名参数中的第一个，若参数不存在则返回空字符串。

## 预防跨站脚本

攻击者通常会在有漏洞的程序中插入 JavaScript、VBScript、 ActiveX 或 Flash 以欺骗用户。一旦得手，他们可以盗取用户帐户信息，修改用户设置，盗取 / 污染 cookie 和植入恶意广告等。

Go 的 html/template 里面带有下面几个函数可以帮助转义：

- `func HTMLEscape (w io.Writer, b [] byte)` // 把 b 进行转义之后写到 w
- `func HTMLEscapeString (s string) string` // 转义 s 之后返回结果字符串
- `func HTMLEscaper (args …interface {}) string` // 支持多个参数一起转义，返回结果字符串

例子：

```go
fmt.Println("username:", template.HTMLEscapeString(r.Form.Get("username"))) // 输出到服务器端
fmt.Println("password:", template.HTMLEscapeString(r.Form.Get("password")))
template.HTMLEscape(w, []byte(r.Form.Get("username"))) // 输出到客户端
```

Go 的 html/template 包默认帮你过滤了 html 标签，但是有时候你只想要输出这个 `<script>alert()</script> `看起来正常的信息，该怎么处理？请使用 text/template。请看下面的例子

```go
import "text/template"
...
t, err := template.New("foo").Parse(`{{define "T"}}Hello, {{.}}!{{end}}`)
err = t.ExecuteTemplate(out, "T", "<script>alert('you have been pwned')</script>")
```

输出

```js
Hello, <script>alert('you have been pwned')</script>!
```

或者使用 template.HTML 类型

```go
import "html/template"
...
t, err := template.New("foo").Parse(`{{define "T"}}Hello, {{.}}!{{end}}`)
err = t.ExecuteTemplate(out, "T", template.HTML("<script>alert('you have been pwned')</script>"))
```

输出

```shell
Hello, <script>alert('you have been pwned')</script>!
```

转换成 `template.HTML` 后，变量的内容也不会被转义

例子：

```go
import "html/template"
...
t, err := template.New("foo").Parse(`{{define "T"}}Hello, {{.}}!{{end}}`)
err = t.ExecuteTemplate(out, "T", "<script>alert('you have been pwned')</script>")
```

```go
Hello, &lt;script&gt;alert(&#39;you have been pwned&#39;)&lt;/script&gt;!
```

## 验证表单的输入

开发 Web 的一个原则就是，不能信任用户输入的任何信息

### 必填字段

为了确保在表单元素中获取一个值，Go中有一个内置函数`len`可以获取数据的长度

```go
if len(r.Form["username"][0])==0{
    // 为空的处理
}
```

r.Form 对不同类型的表单元素的留空有不同的处理， 对于空文本框、空文本区域以及文件上传，元素的值为空值，而如果是未选中的复选框和单选按钮，则根本不会在 r.Form 中产生相应条目，如果我们用上面例子中的方式去获取数据时程序就会报错。所以我们需要通过 r.Form.Get() 来获取值，因为如果字段不存在，通过该方式获取的是空值。但是通过 r.Form.Get() 只能获取单个的值，如果是 map 的值，必须通过上面的方式来获取

数字

判断正整数，先转成int类型，再进行处理

```go
getint,err:=strconv.Atoi(r.Form.Get("age"))
if err!=nil{
    // 数字转化出错了，那么可能就不是数字
}

// 接下来就可以判断这个数字的大小范围了
if getint >100 {
    // 太大了
}
```

正则匹配

```go
if m, _ := regexp.MatchString("^[0-9]+$", r.Form.Get("age")); !m {
    return false
}
```

中文

对于中文我们目前有两种方式来验证，可以使用 unicode 包提供的 func Is(rangeTab *RangeTable, r rune) bool 来验证，也可以使用正则方式来验证，这里使用最简单的正则方式，如下代码所示

```go
if m, _ := regexp.MatchString("^\\p{Han}+$", r.Form.Get("realname")); !m {
    return false
}
```

英文

```go
if m, _ := regexp.MatchString("^[a-zA-Z]+$", r.Form.Get("engname")); !m {
    return false
}
```

电子邮件地址

```go
if m, _ := regexp.MatchString(`^([\w\.\_]{2,10})@(\w{1,})\.([a-z]{2,4})$`, r.Form.Get("email")); !m {
    fmt.Println("no")
}else{
    fmt.Println("yes")
}
```

手机号码

```go
if m, _ := regexp.MatchString(`^(1[3|4|5|8][0-9]\d{4,8})$`, r.Form.Get("mobile")); !m {
    return false
}
```

下拉菜单

```html
<select name="fruit">
<option value="apple">apple</option>
<option value="pear">pear</option>
<option value="banana">banana</option>
</select>
```

验证代码

```go
slice:=[]string{"apple","pear","banana"}

v := r.Form.Get("fruit")
for _, item := range slice {
    if item == v {
        return true
    }
}

return false
```

单选按钮

```go
<input type="radio" name="gender" value="1">男
<input type="radio" name="gender" value="2">女
```

```go
slice:=[]string{"1","2"}

for _, v := range slice {
    if v == r.Form.Get("gender") {
        return true
    }
}
return false
```

复选框

```html
<input type="checkbox" name="interest" value="football">足球
<input type="checkbox" name="interest" value="basketball">篮球
<input type="checkbox" name="interest" value="tennis">网球
```

```go
slice:=[]string{"football","basketball","tennis"}
a:=Slice_diff(r.Form["interest"],slice)
if a == nil{
    return true
}

return false
```

日期与时间

```go
t := time.Date(2009, time.November, 10, 23, 0, 0, 0, time.Local)
fmt.Printf("Go launched at %s\n", t.Local())
```

身份证号码

```go
// 验证 15 位身份证，15 位的是全部数字
if m, _ := regexp.MatchString(`^(\d{15})$`, r.Form.Get("usercard")); !m {
    return false
}

// 验证 18 位身份证，18 位前 17 位为数字，最后一位是校验位，可能为数字或字符 X。
if m, _ := regexp.MatchString(`^(\d{17})([0-9]|X)$`, r.Form.Get("usercard")); !m {
    return false
}
```

## 防止多次提交表单

```html
<input type="checkbox" name="interest" value="football">足球
<input type="checkbox" name="interest" value="basketball">篮球
<input type="checkbox" name="interest" value="tennis">网球    
用户名:<input type="text" name="username">
密码:<input type="password" name="password">
<input type="hidden" name="token" value="{{.}}">
<input type="submit" value="登录">
```

在模版里面增加了一个隐藏字段 token，这个值我们通过 MD5 (时间戳) 来获取唯一值，然后我们把这个值存储到服务器端 (session 来控制，我们将在第六章讲解如何保存)，以方便表单提交时比对判定

```go
func login(w http.ResponseWriter, r *http.Request) {
    fmt.Println("method:", r.Method) // 获取请求的方法
    if r.Method == "GET" {
        crutime := time.Now().Unix()
        h := md5.New()
        io.WriteString(h, strconv.FormatInt(crutime, 10))
        token := fmt.Sprintf("%x", h.Sum(nil))

        t, _ := template.ParseFiles("login.gtpl")
        t.Execute(w, token)
    } else {
        // 请求的是登录数据，那么执行登录的逻辑判断
        r.ParseForm()
        token := r.Form.Get("token")
        if token != "" {
            // 验证 token 的合法性
        } else {
            // 不存在 token 报错
        }
        fmt.Println("username length:", len(r.Form["username"][0]))
        fmt.Println("username:", template.HTMLEscapeString(r.Form.Get("username"))) // 输出到服务器端
        fmt.Println("password:", template.HTMLEscapeString(r.Form.Get("password")))
        template.HTMLEscape(w, []byte(r.Form.Get("username"))) // 输出到客户端
    }
}
```

## 处理文件上传

添加form的`enctype`属性，`enctype`属性有如下三种情况：

```
application/x-www-form-urlencoded   表示在发送前编码所有字符（默认）
multipart/form-data   不对字符编码。在使用包含文件上传控件的表单时，必须使用该值。
text/plain    空格转换为 "+" 加号，但不对特殊字符编码。
```

html文件

```html
<html>
<head>
    <title>上传文件</title>
</head>
<body>
<form enctype="multipart/form-data" action="/upload" method="post">
  <input type="file" name="uploadfile" />
  <input type="hidden" name="token" value="{{.}}"/>
  <input type="submit" value="upload" />
</form>
</body>
</html>
```

代码：

```go
http.HandleFunc("/upload", upload)

// 处理 /upload  逻辑
func upload(w http.ResponseWriter, r *http.Request) {
    fmt.Println("method:", r.Method) // 获取请求的方法
    if r.Method == "GET" {
        crutime := time.Now().Unix()
        h := md5.New()
        io.WriteString(h, strconv.FormatInt(crutime, 10))
        token := fmt.Sprintf("%x", h.Sum(nil))

        t, _ := template.ParseFiles("upload.gtpl")
        t.Execute(w, token)
    } else {
        r.ParseMultipartForm(32 << 20)
        file, handler, err := r.FormFile("uploadfile")
        if err != nil {
            fmt.Println(err)
            return
        }
        defer file.Close()
        fmt.Fprintf(w, "%v", handler.Header)
        f, err := os.OpenFile("./test/"+handler.Filename, os.O_WRONLY|os.O_CREATE, 0666)  // 此处假设当前目录下已存在test目录
        if err != nil {
            fmt.Println(err)
            return
        }
        defer f.Close()
        io.Copy(f, file)
    }
}
```

通过上面的代码可以看到，处理文件上传我们需要调用 `r.ParseMultipartForm`，里面的参数表示 `maxMemory`，调用 `ParseMultipartForm` 之后，上传的文件存储在` maxMemory` 大小的内存里面，如果文件大小超过了 `maxMemory`，那么剩下的部分将存储在系统的临时文件中。我们可以通过 `r.FormFile` 获取上面的文件句柄，然后实例中使用了 `io.Copy` 来存储文件。

> 获取其他非文件字段信息的时候就不需要调用 r.ParseForm，因为在需要的时候 Go 自动会去调用。而且 ParseMultipartForm 调用一次之后，后面再次调用不会再有效果。

通过上面的实例我们可以看到我们上传文件主要三步处理：

1. 表单中增加 enctype="multipart/form-data"
2. 服务端调用 r.ParseMultipartForm, 把上传的文件存储在内存和临时文件中
3. 使用 r.FormFile 获取文件句柄，然后对文件进行存储等处理。

文件 handler 是 multipart.FileHeader, 里面存储了如下结构信息

```go
type FileHeader struct {
    Filename string
    Header   textproto.MIMEHeader
    // contains filtered or unexported fields
}
```

## 客户端上传代码

```go
package main

import (
    "bytes"
    "fmt"
    "io"
    "io/ioutil"
    "mime/multipart"
    "net/http"
    "os"
)

func postFile(filename string, targetUrl string) error {
    bodyBuf := &bytes.Buffer{}
    bodyWriter := multipart.NewWriter(bodyBuf)

    // 关键的一步操作
    fileWriter, err := bodyWriter.CreateFormFile("uploadfile", filename)
    if err != nil {
        fmt.Println("error writing to buffer")
        return err
    }

    // 打开文件句柄操作
    fh, err := os.Open(filename)
    if err != nil {
        fmt.Println("error opening file")
        return err
    }
    defer fh.Close()

    // iocopy
    _, err = io.Copy(fileWriter, fh)
    if err != nil {
        return err
    }

    contentType := bodyWriter.FormDataContentType()
    bodyWriter.Close()

    resp, err := http.Post(targetUrl, contentType, bodyBuf)
    if err != nil {
        return err
    }
    defer resp.Body.Close()
    resp_body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        return err
    }
    fmt.Println(resp.Status)
    fmt.Println(string(resp_body))
    return nil
}

// sample usage
func main() {
    target_url := "http://localhost:9090/upload"
    filename := "./astaxie.pdf"
    postFile(filename, target_url)
}
```

上面的例子详细展示了客户端如何向服务器上传一个文件的例子，客户端通过 multipart.Write 把文件的文本流写入一个缓存中，然后调用 http 的 Post 方法把缓存传到服务器

# 访问数据库

![image-20230224184552043](Go%20Web.assets/image-20230224184552043.png)

## 使用MySQL数据库

### MySQL驱动

- github.com/go-sql-driver/mysql 支持 database/sql，全部采用 go 写。
- github.com/ziutek/mymysql 支持 database/sql，也支持自定义的接口，全部采用 go 写。
- github.com/Philio/GoMySQL 不支持 database/sql，自定义接口，全部采用 go 写。

> 推荐第一个

```go
package main

import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)

func main() {
	db, err := sql.Open("mysql", "root:hsy031122@/go_test?charset=utf8")

	//插入数据
	stmt, err := db.Prepare("INSERT userinfo SET username=?,department=?,created=?")

	res, err := stmt.Exec("astaxie", "研发部门", "2012-12-09")

	id, err := res.LastInsertId()
	fmt.Println(id)

	// 更新数据
	stmt, err = db.Prepare("update userinfo set username=? where uid=?")
	checkErr(err)

	res, err = stmt.Exec("astaxieupdate", id)
	checkErr(err)

	affect, err := res.RowsAffected()
	checkErr(err)

	fmt.Println(affect)

	// 查询数据
	rows, err := db.Query("SELECT * FROM userinfo")
	checkErr(err)

	for rows.Next() {
		var uid int
		var username string
		var department string
		var created string
		err = rows.Scan(&uid, &username, &department, &created)
		checkErr(err)
		fmt.Println(uid)
		fmt.Println(username)
		fmt.Println(department)
		fmt.Println(created)
	}

	// 删除数据
	stmt, err = db.Prepare("delete from userinfo where uid=?")
	checkErr(err)

	res, err = stmt.Exec(id)
	checkErr(err)

	affect, err = res.RowsAffected()
	checkErr(err)

	fmt.Println(affect)

	db.Close()

}

func checkErr(err error) {
	if err != nil {
		panic(err)
	}
}
```

sql.Open函数用来打开一个注册过的数据库驱动，第二个参数是DSN（Data Source Name）用于定义一些数据库链接和配置信息，支持如下格式：

```go
user@unix(/path/to/socket)/dbname?charset=utf8
user:password@tcp(localhost:5555)/dbname?charset=utf8
user:password@/dbname
user:password@tcp([de:ad:be:ef::ca:fe]:80)/dbname
```

db.Prepare () 函数用来返回准备要执行的 sql 操作，然后返回准备完毕的执行状态。

db.Query () 函数用来直接执行 Sql 返回 Rows 结果。

stmt.Exec () 函数用来执行 stmt 准备好的 SQL 语句

传入的参数都是 =? 对应的数据，这样做的方式可以一定程度上防止 SQL 注入

# session和数据存储

![img](Go%20Web.assets/navi6.png)

## session和cookie

![img](Go%20Web.assets/6.1.cookie2.png)

![img](Go%20Web.assets/6.1.session.png)

### Go设置cookie

Go语言提供net/http包中的SetCookie来设置：

```go
http.SetCookie(w ResponseWriter, cookie *Cookie)
```

w表示需要写入的response，cookie是一个struct

cookie对象：

```go
type Cookie struct {
    Name       string
    Value      string
    Path       string
    Domain     string
    Expires    time.Time
    RawExpires string

// MaxAge=0 means no 'Max-Age' attribute specified.
// MaxAge<0 means delete cookie now, equivalently 'Max-Age: 0'
// MaxAge>0 means Max-Age attribute present and given in seconds
    MaxAge   int
    Secure   bool
    HttpOnly bool
    Raw      string
    Unparsed []string // Raw text of unparsed attribute-value pairs
}
```

例子

```go
expiration := time.Now()
expiration = expiration.AddDate(1, 0, 0)
cookie := http.Cookie{Name: "username", Value: "astaxie", Expires: expiration}
http.SetCookie(w, &cookie)
```

### Go读取cookie

```go
cookie, _ := r.Cookie("username")
fmt.Fprint(w, cookie)
```

还有一种读取方式

```go
for _, cookie := range r.Cookies() {
    fmt.Fprint(w, cookie.Name)
}
```

## Go如何使用session

### Go实现session管理

#### session管理设计

- 全局session管理器
- 保证sessionid的全局唯一性
- 为每一位客户关联一个session
- session的存储（可以存储到内存、文件、数据库）
- session过期处理

#### Session管理器

定义一个全局的session管理器

```go
type Manager struct {
    cookieName  string     // private cookiename
    lock        sync.Mutex // protects session
    provider    Provider
    maxLifeTime int64
}

func NewManager(provideName, cookieName string, maxLifeTime int64) (*Manager, error) {
    provider, ok := provides[provideName]
    if !ok {
        return nil, fmt.Errorf("session: unknown provide %q (forgotten import?)", provideName)
    }
    return &Manager{provider: provider, cookieName: cookieName, maxLifeTime: maxLifeTime}, nil
}
```

在mian中创建一个全局的session管理器

```go
var globalSessions *session.Manager
// 然后在 init 函数中初始化
func init() {
    globalSessions, _ = NewManager("memory", "gosessionid", 3600)
}
```

session是存储在服务端的数据，他可以以任何方式存储。因此，抽象出一个Provider接口，用来表示session管理器底层存储结构

```go
type Provider interface {
    SessionInit(sid string) (Session, error)
    SessionRead(sid string) (Session, error)
    SessionDestroy(sid string) error
    SessionGC(maxLifeTime int64)
}
```

> - SessionInit 函数实现 Session 的初始化，操作成功则返回此新的 Session 变量
> - SessionRead 函数返回 sid 所代表的 Session 变量，如果不存在，那么将以 sid 为参数调用 
> - SessionInit 函数创建并返回一个新的 Session 变量
> - SessionDestroy 函数用来销毁 sid 对应的 Session 变量
> - SessionGC 根据 maxLifeTime 来删除过期的数据

注册存储session的结构的Register函数实现：

```go
var provides = make(map[string]Provider)

// Register makes a session provide available by the provided name.
// If Register is called twice with the same name or if driver is nil,
// it panics.
func Register(name string, provider Provider) {
    if provider == nil {
        panic("session: Register provider is nil")
    }
    if _, dup := provides[name]; dup {
        panic("session: Register called twice for provider " + name)
    }
    provides[name] = provider
}
```

#### 全局唯一的SessionID

```go
func (manager *Manager) sessionId() string {
    b := make([]byte, 32)
    if _, err := rand.Read(b); err != nil {
        return ""
    }
    return base64.URLEncoding.EncodeToString(b)
}
```

#### session创建

我们需要为每个来访的用户分配或获取与他相连的Session，以便后面根据Session信息来验证。SessionStart这个函数用来检测是否已经有某个session与当前用户产生关联，如果没用则创建：

```go
func (manager *Manager) SessionStart(w http.ResponseWriter, r *http.Request) (session Session) {
    manager.lock.Lock()
    defer manager.lock.Unlock()
    cookie, err := r.Cookie(manager.cookieName)
    if err != nil || cookie.Value == "" {
        sid := manager.sessionId()
        session, _ = manager.provider.SessionInit(sid)
        cookie := http.Cookie{Name: manager.cookieName, Value: url.QueryEscape(sid), Path: "/", HttpOnly: true, MaxAge: int(manager.maxLifeTime)}
        http.SetCookie(w, &cookie)
    } else {
        sid, _ := url.QueryUnescape(cookie.Value)
        session, _ = manager.provider.SessionRead(sid)
    }
    return
}
```

运用演示：

```go
func login(w http.ResponseWriter, r *http.Request) {
    sess := globalSessions.SessionStart(w, r)
    r.ParseForm()
    if r.Method == "GET" {
        t, _ := template.ParseFiles("login.gtpl")
        w.Header().Set("Content-Type", "text/html")
        t.Execute(w, sess.Get("username"))
    } else {
        sess.Set("username", r.Form["username"])
        http.Redirect(w, r, "/", 302)
    }
}
```

#### 操作值：设置、读取和删除

```go
func count(w http.ResponseWriter, r *http.Request) {
    sess := globalSessions.SessionStart(w, r)
    createtime := sess.Get("createtime")
    if createtime == nil {
        sess.Set("createtime", time.Now().Unix())
    } else if (createtime.(int64) + 360) < (time.Now().Unix()) {
        globalSessions.SessionDestroy(w, r)
        sess = globalSessions.SessionStart(w, r)
    }
    ct := sess.Get("countnum")
    if ct == nil {
        sess.Set("countnum", 1)
    } else {
        sess.Set("countnum", (ct.(int) + 1))
    }
    t, _ := template.ParseFiles("count.gtpl")
    w.Header().Set("Content-Type", "text/html")
    t.Execute(w, sess.Get("countnum"))
}
```

#### session重置

```go
// Destroy sessionid
func (manager *Manager) SessionDestroy(w http.ResponseWriter, r *http.Request){
    cookie, err := r.Cookie(manager.cookieName)
    if err != nil || cookie.Value == "" {
        return
    } else {
        manager.lock.Lock()
        defer manager.lock.Unlock()
        manager.provider.SessionDestroy(cookie.Value)
        expiration := time.Now()
        cookie := http.Cookie{Name: manager.cookieName, Path: "/", HttpOnly: true, Expires: expiration, MaxAge: -1}
        http.SetCookie(w, &cookie)
    }
}
```

#### session销毁

在Main启动的时候启动：

```go
func init() {
    go globalSessions.GC()
}
```

```go
func (manager *Manager) GC() {
    manager.lock.Lock()
    defer manager.lock.Unlock()
    manager.provider.SessionGC(manager.maxLifeTime)
    time.AfterFunc(time.Duration(manager.maxLifeTime), func() { manager.GC() })
}
```

GC充分利用了time包中的定时器功能，当超时`maxLifeTime`后，调用GC函数没这样就可以保证`maxLifeTime`时间内的session都是可用的

## session存储

基于内存的session存储接口的实现

```go
package memory

import (
    "container/list"
    "github.com/astaxie/session"
    "sync"
    "time"
)

var pder = &Provider{list: list.New()}

type SessionStore struct {
    sid          string                      // session id唯一标示
    timeAccessed time.Time                   // 最后访问时间
    value        map[interface{}]interface{} // session里面存储的值
}

func (st *SessionStore) Set(key, value interface{}) error {
    st.value[key] = value
    pder.SessionUpdate(st.sid)
    return nil
}

func (st *SessionStore) Get(key interface{}) interface{} {
    pder.SessionUpdate(st.sid)
    if v, ok := st.value[key]; ok {
        return v
    } else {
        return nil
    }
}

func (st *SessionStore) Delete(key interface{}) error {
    delete(st.value, key)
    pder.SessionUpdate(st.sid)
    return nil
}

func (st *SessionStore) SessionID() string {
    return st.sid
}

type Provider struct {
    lock     sync.Mutex               // 用来锁
    sessions map[string]*list.Element // 用来存储在内存
    list     *list.List               // 用来做 gc
}

func (pder *Provider) SessionInit(sid string) (session.Session, error) {
    pder.lock.Lock()
    defer pder.lock.Unlock()
    v := make(map[interface{}]interface{}, 0)
    newsess := &SessionStore{sid: sid, timeAccessed: time.Now(), value: v}
    element := pder.list.PushFront(newsess)
    pder.sessions[sid] = element
    return newsess, nil
}

func (pder *Provider) SessionRead(sid string) (session.Session, error) {
    if element, ok := pder.sessions[sid]; ok {
        return element.Value.(*SessionStore), nil
    } else {
        sess, err := pder.SessionInit(sid)
        return sess, err
    }
    return nil, nil
}

func (pder *Provider) SessionDestroy(sid string) error {
    if element, ok := pder.sessions[sid]; ok {
        delete(pder.sessions, sid)
        pder.list.Remove(element)
        return nil
    }
    return nil
}

func (pder *Provider) SessionGC(maxlifetime int64) {
    pder.lock.Lock()
    defer pder.lock.Unlock()

    for {
        element := pder.list.Back()
        if element == nil {
            break
        }
        if (element.Value.(*SessionStore).timeAccessed.Unix() + maxlifetime) < time.Now().Unix() {
            pder.list.Remove(element)
            delete(pder.sessions, element.Value.(*SessionStore).sid)
        } else {
            break
        }
    }
}

func (pder *Provider) SessionUpdate(sid string) error {
    pder.lock.Lock()
    defer pder.lock.Unlock()
    if element, ok := pder.sessions[sid]; ok {
        element.Value.(*SessionStore).timeAccessed = time.Now()
        pder.list.MoveToFront(element)
        return nil
    }
    return nil
}

func init() {
    pder.sessions = make(map[string]*list.Element, 0)
    session.Register("memory", pder)
}
```

上面的代码实现了一个内存存储的session机制。通过init函数注册到session管理器中。通过如下代码调用引擎：

```go
import (
    "github.com/astaxie/session"
    _ "github.com/astaxie/session/providers/memory"
)
```

当import的时候已经执行了memory函数里面的init函数，这样就注册到session管理器中，我们就可以使用了，通过如下方式就可以初始化一个session管理器：

```go
var globalSessions *session.Manager

// 然后在 init 函数中初始化
func init() {
    globalSessions, _ = session.NewManager("memory", "gosessionid", 3600)
    go globalSessions.GC()
}
```

## 预防session劫持





# 文本处理



# Web服务

![img](Go%20Web.assets/navi8.png)

## Socket编程

### 什么是 Socket？

Socket 起源于 Unix，而 Unix 基本哲学之一就是 “一切皆文件”，都可以用 “打开 open –> 读写 write/read –> 关闭 close” 模式来操作。Socket 就是该模式的一个实现，网络的 Socket 数据传输是一种特殊的 I/O，Socket 也是一种文件描述符。Socket 也具有一个类似于打开文件的函数调用：Socket ()，该函数返回一个整型的 Socket 描述符，随后的连接建立、数据传输等操作都是通过该 Socket 实现的。

常用的 Socket 类型有两种：流式 Socket（SOCK_STREAM）和数据报式 Socket（SOCK_DGRAM）。流式是一种面向连接的 Socket，针对于面向连接的 TCP 服务应用；数据报式 Socket 是一种无连接的 Socket，对应于无连接的 UDP 服务应用。

### Socket 如何通信

网络中的进程之间如何通过 Socket 通信呢？首要解决的问题是如何唯一标识一个进程，否则通信无从谈起！在本地可以通过进程 PID 来唯一标识一个进程，但是在网络中这是行不通的。其实 TCP/IP 协议族已经帮我们解决了这个问题，网络层的 “ip 地址” 可以唯一标识网络中的主机，而传输层的 “协议 + 端口” 可以唯一标识主机中的应用程序（进程）。这样利用三元组（ip 地址，协议，端口）就可以标识网络的进程了，网络中需要互相通信的进程，就可以利用这个标志在他们之间进行交互。

使用 TCP/IP 协议的应用程序通常采用应用编程接口：UNIX BSD 的套接字（socket）和 UNIX System V 的 TLI（已经被淘汰），来实现网络进程之间的通信。就目前而言，几乎所有的应用程序都是采用 socket，而现在又是网络时代，网络中进程通信是无处不在，这就是为什么说 “一切皆 Socket”。

### Socket 基础知识

通过上面的介绍我们知道 Socket 有两种：TCP Socket 和 UDP Socket，TCP 和 UDP 是协议，而要确定一个进程的需要三元组，需要 IP 地址和端口。

#### IPv4 地址

目前的全球因特网所采用的协议族是 TCP/IP 协议。IP 是 TCP/IP 协议中网络层的协议，是 TCP/IP 协议族的核心协议。目前主要采用的 IP 协议的版本号是 4 (简称为 IPv4)，发展至今已经使用了 30 多年。

IPv4 的地址位数为 32 位，也就是最多有 2 的 32 次方的网络设备可以联到 Internet 上。近十年来由于互联网的蓬勃发展，IP 位址的需求量愈来愈大，使得 IP 位址的发放愈趋紧张，前一段时间，据报道 IPV4 的地址已经发放完毕，我们公司目前很多服务器的 IP 都是一个宝贵的资源。

地址格式类似这样：127.0.0.1 172.122.121.111

#### IPv6 地址

IPv6 是下一版本的互联网协议，也可以说是下一代互联网的协议，它是为了解决 IPv4 在实施过程中遇到的各种问题而被提出的，IPv6 采用 128 位地址长度，几乎可以不受限制地提供地址。按保守方法估算 IPv6 实际可分配的地址，整个地球的每平方米面积上仍可分配 1000 多个地址。在 IPv6 的设计过程中除了一劳永逸地解决了地址短缺问题以外，还考虑了在 IPv4 中解决不好的其它问题，主要有端到端 IP 连接、服务质量（QoS）、安全性、多播、移动性、即插即用等。

地址格式类似这样：2002	82e7	0	c0e8:82e7

#### Go 支持的 IP 类型

在 Go 的 `net` 包中定义了很多类型、函数和方法用来网络编程，其中 IP 的定义如下：

```go
type IP []byte
```

在 net 包中有很多函数来操作 IP，但是其中比较有用的也就几个，其中 `ParseIP(s string) IP `函数会把一个 IPv4 或者 IPv6 的地址转化成 IP 类型，请看下面的例子:

```go
package main
import (
    "net"
    "os"
    "fmt"
)
func main() {
    if len(os.Args) != 2 {
        fmt.Fprintf(os.Stderr, "Usage: %s ip-addr\n", os.Args[0])
        os.Exit(1)
    }
    name := os.Args[1]
    addr := net.ParseIP(name)
    if addr == nil {
        fmt.Println("Invalid address")
    } else {
        fmt.Println("The address is ", addr.String())
    }
    os.Exit(0)
}
```

执行之后你就会发现只要你输入一个 IP 地址就会给出相应的 IP 格式

### TCP Socket

当我们知道如何通过网络端口访问一个服务时，那么我们能够做什么呢？作为客户端来说，我们可以通过向远端某台机器的的某个网络端口发送一个请求，然后得到在机器的此端口上监听的服务反馈的信息。作为服务端，我们需要把服务绑定到某个指定端口，并且在此端口上监听，当有客户端来访问时能够读取信息并且写入反馈信息。

在 Go 语言的 `net` 包中有一个类型 `TCPConn`，这个类型可以用来作为客户端和服务器端交互的通道，他有两个主要的函数：

```go
func (c *TCPConn) Write(b []byte) (int, error)
func (c *TCPConn) Read(b []byte) (int, error)
```

`TCPConn` 可以用在客户端和服务器端来读写数据。

还有我们需要知道一个 `TCPAddr` 类型，他表示一个 TCP 的地址信息，他的定义如下：

```go
type TCPAddr struct {
    IP IP
    Port int
    Zone string // IPv6 scoped addressing zone
}
```

在 Go 语言中通过 `ResolveTCPAddr` 获取一个 `TCPAddr`

```go
func ResolveTCPAddr(net, addr string) (*TCPAddr, os.Error)
```

> - net 参数是 “tcp4”、”tcp6”、”tcp” 中的任意一个，分别表示 TCP (IPv4-only), TCP (IPv6-only) 或者 TCP (IPv4, IPv6 的任意一个)
> - addr 表示域名或者 IP 地址，例如”www.google.com:80" 或者 “127.0.0.1:22”

#### TCP client

Go 语言中通过 net 包中的 `DialTCP` 函数来建立一个 TCP 连接，并返回一个 `TCPConn` 类型的对象，当连接建立时服务器端也创建一个同类型的对象，此时客户端和服务器段通过各自拥有的 `TCPConn` 对象来进行数据交换。一般而言，客户端通过 `TCPConn` 对象将请求信息发送到服务器端，读取服务器端响应的信息。服务器端读取并解析来自客户端的请求，并返回应答信息，这个连接只有当任一端关闭了连接之后才失效，不然这连接可以一直在使用。建立连接的函数定义如下：

```go
func DialTCP(network string, laddr, raddr TCPAddr) (TCPConn, error)
```

> - net 参数是 “tcp4”、”tcp6”、”tcp” 中的任意一个，分别表示 TCP (IPv4-only)、TCP (IPv6-only) 或者 TCP (IPv4, IPv6 的任意一个)
> - laddr 表示本机地址，一般设置为 nil
> - raddr 表示远程的服务地址

接下来我们写一个简单的例子，模拟一个基于 HTTP 协议的客户端请求去连接一个 Web 服务端。我们要写一个简单的 http 请求头，格式类似如下：

```go
"HEAD / HTTP/1.0\r\n\r\n"
```

从服务端接收到的响应信息格式可能如下：

```
HTTP/1.0 200 OK
ETag: "-9985996"
Last-Modified: Thu, 25 Mar 2010 17:51:10 GMT
Content-Length: 18074
Connection: close
Date: Sat, 28 Aug 2010 00:43:48 GMT
Server: lighttpd/1.4.23
```

我们的客户端代码如下所示：

```go
package main

import (
    "fmt"
    "io/ioutil"
    "net"
    "os"
)

func main() {
    if len(os.Args) != 2 {
        fmt.Fprintf(os.Stderr, "Usage: %s host:port ", os.Args[0])
        os.Exit(1)
    }
    service := os.Args[1]
    tcpAddr, err := net.ResolveTCPAddr("tcp4", service)
    checkError(err)
    conn, err := net.DialTCP("tcp", nil, tcpAddr)
    checkError(err)
    _, err = conn.Write([]byte("HEAD / HTTP/1.0\r\n\r\n"))
    checkError(err)
    result, err := ioutil.ReadAll(conn)
    checkError(err)
    fmt.Println(string(result))
    os.Exit(0)
}
func checkError(err error) {
    if err != nil {
        fmt.Fprintf(os.Stderr, "Fatal error: %s", err.Error())
        os.Exit(1)
    }
}
```

通过上面的代码我们可以看出：首先程序将用户的输入作为参数 `service` 传入 `net.ResolveTCPAddr` 获取一个 tcpAddr, 然后把 tcpAddr 传入 DialTCP 后创建了一个 TCP 连接 `conn`，通过 `conn `来发送请求信息，最后通过` ioutil.ReadAll` 从 `conn `中读取全部的文本，也就是服务端响应反馈的信息。

#### TCP server

上面我们编写了一个 TCP 的客户端程序，也可以通过 net 包来创建一个服务器端程序，在服务器端我们需要绑定服务到指定的非激活端口，并监听此端口，当有客户端请求到达的时候可以接收到来自客户端连接的请求。net 包中有相应功能的函数，函数定义如下：

```go
func ListenTCP(network string, laddr TCPAddr) (TCPListener, error)
func (l *TCPListener) Accept() (Conn, error)
```

参数说明同 DialTCP 的参数一样。下面我们实现一个简单的时间同步服务，监听 7777 端口

```go
package main

import (
    "fmt"
    "net"
    "os"
    "time"
)

func main() {
    service := ":7777"
    tcpAddr, err := net.ResolveTCPAddr("tcp4", service)
    checkError(err)
    listener, err := net.ListenTCP("tcp", tcpAddr)
    checkError(err)
    for {
        conn, err := listener.Accept()
        if err != nil {
            continue
        }
        daytime := time.Now().String()
        conn.Write([]byte(daytime)) // don't care about return value
        conn.Close()                // we're finished with this client
    }
}
func checkError(err error) {
    if err != nil {
        fmt.Fprintf(os.Stderr, "Fatal error: %s", err.Error())
        os.Exit(1)
    }
}
```

上面的服务跑起来之后，它将会一直在那里等待，直到有新的客户端请求到达。当有新的客户端请求到达并同意接受 `Accept `该请求的时候他会反馈当前的时间信息。值得注意的是，在代码中 `for `循环里，当有错误发生时，直接 continue 而不是退出，是因为在服务器端跑代码的时候，当有错误发生的情况下最好是由服务端记录错误，然后当前连接的客户端直接报错而退出，从而不会影响到当前服务端运行的整个服务。

上面的代码有个缺点，执行的时候是单任务的，不能同时接收多个请求，那么该如何改造以使它支持多并发呢？Go 里面有一个 goroutine 机制，请看下面改造后的代码

```go
package main

import (
    "fmt"
    "net"
    "os"
    "time"
)

func main() {
    service := ":1200"
    tcpAddr, err := net.ResolveTCPAddr("tcp4", service)
    checkError(err)
    listener, err := net.ListenTCP("tcp", tcpAddr)
    checkError(err)
    for {
        conn, err := listener.Accept()
        if err != nil {
            continue
        }
        go handleClient(conn)
    }
}

func handleClient(conn net.Conn) {
    defer conn.Close()
    daytime := time.Now().String()
    conn.Write([]byte(daytime)) // don't care about return value
    // we're finished with this client
}
func checkError(err error) {
    if err != nil {
        fmt.Fprintf(os.Stderr, "Fatal error: %s", err.Error())
        os.Exit(1)
    }
}
```

通过把业务处理分离到函数 `handleClient`，我们就可以进一步地实现多并发执行了。看上去是不是很帅，增加 `go `关键词就实现了服务端的多并发，从这个小例子也可以看出 goroutine 的强大之处。

有的朋友可能要问：这个服务端没有处理客户端实际请求的内容。如果我们需要通过从客户端发送不同的请求来获取不同的时间格式，而且需要一个长连接，该怎么做呢？请看：

```go
package main

import (
    "fmt"
    "net"
    "os"
    "time"
    "strconv"
    "strings"
)

func main() {
    service := ":1200"
    tcpAddr, err := net.ResolveTCPAddr("tcp4", service)
    checkError(err)
    listener, err := net.ListenTCP("tcp", tcpAddr)
    checkError(err)
    for {
        conn, err := listener.Accept()
        if err != nil {
            continue
        }
        go handleClient(conn)
    }
}

func handleClient(conn net.Conn) {
    conn.SetReadDeadline(time.Now().Add(2 * time.Minute)) // set 2 minutes timeout
    request := make([]byte, 128) // set maxium request length to 128B to prevent flood attack
    defer conn.Close()  // close connection before exit
    for {
        read_len, err := conn.Read(request)
        if err != nil {
                fmt.Println(err)
                break
            }

                if read_len == 0 {
                    break // connection already closed by client
                } else if strings.TrimSpace(string(request[:read_len])) == "timestamp" {
                    daytime := strconv.FormatInt(time.Now().Unix(), 10)
                    conn.Write([]byte(daytime))
                } else {
                    daytime := time.Now().String()
                    conn.Write([]byte(daytime))
                }

                request = make([]byte, 128) // clear last read content
        }
}

func checkError(err error) {
    if err != nil {
        fmt.Fprintf(os.Stderr, "Fatal error: %s", err.Error())
        os.Exit(1)
    }
}

```


在上面这个例子中，我们使用 conn.Read() 不断读取客户端发来的请求。由于我们需要保持与客户端的长连接，所以不能在读取完一次请求后就关闭连接。由于 conn.SetReadDeadline() 设置了超时，当一定时间内客户端无请求发送，conn 便会自动关闭，下面的 for 循环即会因为连接已关闭而跳出。需要注意的是，request 在创建时需要指定一个最大长度以防止 flood attack；每次读取到请求处理完毕后，需要清理 request，因为 conn.Read() 会将新读取到的内容 append 到原内容之后。

#### 控制 TCP 连接

TCP 有很多连接控制函数，我们平常用到比较多的有如下几个函数：

```go
func DialTimeout(net, addr string, timeout time.Duration) (Conn, error)
```

设置建立连接的超时时间，客户端和服务器端都适用，当超过设置时间时，连接自动关闭。

```go
func (c *TCPConn) SetReadDeadline(t time.Time) error
func (c *TCPConn) SetWriteDeadline(t time.Time) error
```

用来设置 写入 / 读取 一个连接的超时时间。当超过设置时间时，连接自动关闭。

```go
func (c *TCPConn) SetKeepAlive(keepalive bool) os.Error
```

设置 keepAlive 属性，是操作系统层在 tcp 上没有数据和 ACK 的时候，会间隔性的发送 keepalive 包，操作系统可以通过该包来判断一个 tcp 连接是否已经断开，在 windows 上默认 2 个小时没有收到数据和 keepalive 包的时候认为 tcp 连接已经断开，这个功能和我们通常在应用层加的心跳包的功能类似。

### UDP Socket

Go 语言包中处理 UDP Socket 和 TCP Socket 不同的地方就是在服务器端处理多个客户端请求数据包的方式不同，UDP 缺少了对客户端连接请求的 Accept 函数。其他基本几乎一模一样，只有 TCP 换成了 UDP 而已。UDP 的几个主要函数如下所示：

```go
func ResolveUDPAddr(net, addr string) (*UDPAddr, os.Error)
func DialUDP(net string, laddr, raddr *UDPAddr) (c *UDPConn, err os.Error)
func ListenUDP(net string, laddr *UDPAddr) (c *UDPConn, err os.Error)
func (c *UDPConn) ReadFromUDP(b []byte) (n int, addr *UDPAddr, err os.Error)
func (c *UDPConn) WriteToUDP(b []byte, addr *UDPAddr) (n int, err os.Error)
```

一个 UDP 的客户端代码如下所示，我们可以看到不同的就是 TCP 换成了 UDP 而已：

```go
package main

import (
    "fmt"
    "net"
    "os"
)

func main() {
    if len(os.Args) != 2 {
        fmt.Fprintf(os.Stderr, "Usage: %s host:port", os.Args[0])
        os.Exit(1)
    }
    service := os.Args[1]
    udpAddr, err := net.ResolveUDPAddr("udp4", service)
    checkError(err)
    conn, err := net.DialUDP("udp", nil, udpAddr)
    checkError(err)
    _, err = conn.Write([]byte("anything"))
    checkError(err)
    var buf [512]byte
    n, err := conn.Read(buf[0:])
    checkError(err)
    fmt.Println(string(buf[0:n]))
    os.Exit(0)
}
func checkError(err error) {
    if err != nil {
        fmt.Fprintf(os.Stderr, "Fatal error ", err.Error())
        os.Exit(1)
    }
}
我们来看一下 UDP 服务器端如何来处理：

package main

import (
    "fmt"
    "net"
    "os"
    "time"
)

func main() {
    service := ":1200"
    udpAddr, err := net.ResolveUDPAddr("udp4", service)
    checkError(err)
    conn, err := net.ListenUDP("udp", udpAddr)
    checkError(err)
	handleClient(conn)
}
func handleClient(conn *net.UDPConn) {
    var buf [512]byte
    _, addr, err := conn.ReadFromUDP(buf[0:])
    if err != nil {
        return
    }
    daytime := time.Now().String()
    conn.WriteToUDP([]byte(daytime), addr)
}
func checkError(err error) {
    if err != nil {
        fmt.Fprintf(os.Stderr, "Fatal error ", err.Error())
        os.Exit(1)
    }
}
```

## WebSocket

## Go实现WebScoket

Go语言标准包中没用提供对WebScoket的支持，但在`go.net`的子包中有这个支持，通过如下命令获取该包：

```shell
go get golang.org/x/net/websocket
```

WebSocket 分为客户端和服务端，接下来我们将实现一个简单的例子：用户输入信息，客户端通过 WebSocket 将信息发送给服务器端，服务器端收到信息之后主动 Push 信息到客户端，然后客户端将输出其收到的信息，客户端的代码如下：

```html
<html>
<head></head>
<body>
    <script type="text/javascript">
        var sock = null;
        var wsuri = "ws://127.0.0.1:1234";

        window.onload = function() {

            console.log("onload");

            sock = new WebSocket(wsuri);

            sock.onopen = function() {
                console.log("connected to " + wsuri);
            }

            sock.onclose = function(e) {
                console.log("connection closed (" + e.code + ")");
            }

            sock.onmessage = function(e) {
                console.log("message received: " + e.data);
            }
        };

        function send() {
            var msg = document.getElementById('message').value;
            sock.send(msg);
        };
    </script>
    <h1>WebSocket Echo Test</h1>
    <form>
        <p>
            Message: <input id="message" type="text" value="Hello, world!">
        </p>
    </form>
    <button onclick="send();">Send Message</button>
</body>
</html>
```

> - 1）onopen 建立连接后触发
> - 2）onmessage 收到消息后触发
> - 3）onerror 发生错误时触发
> - 4）onclose 关闭连接时触发

服务端实现如下：

```go
package main

import (
    "golang.org/x/net/websocket"
    "fmt"
    "log"
    "net/http"
)

func Echo(ws *websocket.Conn) {
    var err error

    for {
        var reply string

        if err = websocket.Message.Receive(ws, &reply); err != nil {
            fmt.Println("Can't receive")
            break
        }

        fmt.Println("Received back from client: " + reply)

        msg := "Received:  " + reply
        fmt.Println("Sending to client: " + msg)

        if err = websocket.Message.Send(ws, msg); err != nil {
            fmt.Println("Can't send")
            break
        }
    }
}

func main() {
    http.Handle("/", websocket.Handler(Echo))

    if err := http.ListenAndServe(":1234", nil); err != nil {
        log.Fatal("ListenAndServe:", err)
    }
}
```



## REST





## RPC





# 安全与加密



# 国际化与本土化



# 错误处理，调试和测试





# 部署与维护

![img](Go%20Web.assets/navi12.png)

## 应用日志



## 网站错误处理



## 应用部署

### Supervisord

supervisord会把管理的应用程序转成daemon程序，通过命令开启、关闭、重启，而且它管理的进程一旦崩溃就会重启，保证程序执行中断后的情况有自我修复的功能

安装

通过`sudo easy_install supervisor`安装

> 使用easy_install必须按照setuptools
>
> 打开 `http://pypi.python.org/pypi/setuptools#files`，根据你系统的 python 的版本下载相应的文件，然后执行 `sh setuptoolsxxxx.egg`

配置

默认配置文件路径为`/etc/supervisord.conf`

```conf

;/etc/supervisord.conf
[unix_http_server]
file = /var/run/supervisord.sock
chmod = 0777
chown= root:root

[inet_http_server]
# Web管理界面设定
port=9001
username = admin
password = yourpassword

[supervisorctl]
; 必须和'unix_http_server'里面的设定匹配
serverurl = unix:///var/run/supervisord.sock

[supervisord]
logfile=/var/log/supervisord/supervisord.log ; (main log file;default $CWD/supervisord.log)
logfile_maxbytes=50MB       ; (max main logfile bytes b4 rotation;default 50MB)
logfile_backups=10          ; (num of main logfile rotation backups;default 10)
loglevel=info               ; (log level;default info; others: debug,warn,trace)
pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
nodaemon=true              ; (start in foreground if true;default false)
minfds=1024                 ; (min. avail startup file descriptors;default 1024)
minprocs=200                ; (min. avail process descriptors;default 200)
user=root                 ; (default is current user, required if root)
childlogdir=/var/log/supervisord/            ; ('AUTO' child log dir, default $TEMP)

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

; 管理的单个进程的配置，可以添加多个 program
[program:blogdemon]
command=/data/blog/blogdemon
autostart = true
startsecs = 5
user = root
redirect_stderr = true
stdout_logfile = /var/log/supervisord/blogdemon.log
```

管理

> - supervisord，初始启动 Supervisord，启动、管理配置中设置的进程
> - supervisorctl stop programxxx，停止某一个进程 (programxxx)，programxxx 为 [program:blogdemon] 里配置的值，这个示例就是 blogdemon
> - supervisorctl start programxxx，启动某个进程
> - supervisorctl restart programxxx，重启某个进程
> - supervisorctl stop all，停止全部进程，注：start、restart、stop 都不会载入最新的配置文件
> - supervisorctl reload，载入最新的配置文件，并按新的配置启动、管理所有进程

## 备份与恢复

### 应用备份

#### rsync安装

软件包安装

```
sudo apt-get  install  rsync
```

#### rsync配置

服务端开启

```
/usr/bin/rsync --daemon  --config=/etc/rsyncd.conf
```

--daemon 参数方式，是让 rsync 以服务器模式运行。把 rsync 加入开机启动

```
echo 'rsync --daemon' >> /etc/rc.d/rc.local
```

设置rsync密码

```
echo '你的用户名:你的密码' > /etc/rsyncd.secrets
chmod 600 /etc/rsyncd.secrets
```

### MySQL备份

目前 MySQL 的备份有两种方式：热备份和冷备份，热备份目前主要是采用 master/slave 方式（master/slave 方式的同步目前主要用于数据库读写分离，也可以用于热备份数据），关于如何配置这方面的资料，大家可以找到很多。冷备份的话就是数据有一定的延迟，但是可以保证该时间段之前的数据完整

冷备份一般使用 shell 脚本来实现定时备份数据库，然后通过上面介绍 rsync 同步非本地机房的一台服务器。

下面这个是定时备份 mysql 的备份脚本，我们使用了 mysqldump 程序，这个命令可以把数据库导出到一个文件中

```sh
#!/bin/bash

# 以下配置信息请自己修改
mysql_user="USER" # MySQL备份用户
mysql_password="PASSWORD" # MySQL备份用户的密码
mysql_host="localhost"
mysql_port="3306"
mysql_charset="utf8" #MySQL编码
backup_db_arr=("db1" "db2") # 要备份的数据库名称，多个用空格分开隔开 如("db1" "db2" "db3")
backup_location=/var/www/mysql  #备 份数据存放位置，末尾请不要带"/", 此项可以保持默认，程序会自动创建文件夹
expire_backup_delete="ON" # 是否开启过期备份删除 ON为开启 OFF为关闭
expire_days=3 # 过期时间天数 默认为三天，此项只有在 expire_backup_delete 开启时有效

# 本行开始以下不需要修改
backup_time=`date +%Y%m%d%H%M`  # 定义备份详细时间
backup_Ymd=`date +%Y-%m-%d` # 定义备份目录中的年月日时间
backup_3ago=`date -d '3 days ago' +%Y-%m-%d` # 3 天之前的日期
backup_dir=$backup_location/$backup_Ymd  # 备份文件夹全路径
welcome_msg="Welcome to use MySQL backup tools!" # 欢迎语

# 判断 MYSQL 是否启动, mysql 没有启动则备份退出
mysql_ps=`ps -ef |grep mysql |wc -l`
mysql_listen=`netstat -an |grep LISTEN |grep $mysql_port|wc -l`
if [ [$mysql_ps == 0] -o [$mysql_listen == 0] ]; then
        echo "ERROR:MySQL is not running! backup stop!"
        exit
else
        echo $welcome_msg
fi

# 连接到 mysql 数据库，无法连接则备份退出
mysql -h$mysql_host -P$mysql_port -u$mysql_user -p$mysql_password <<end
use mysql;
select host,user from user where user='root' and host='localhost';
exit
end

flag=`echo $?`
if [ $flag != "0" ]; then
        echo "ERROR:Can't connect mysql server! backup stop!"
        exit
else
        echo "MySQL connect ok! Please wait......"
        # 判断有没有定义备份的数据库，如果定义则开始备份，否则退出备份
        if [ "$backup_db_arr" != "" ];then
                #dbnames=$(cut -d ',' -f1-5 $backup_database)
                #echo "arr is (${backup_db_arr[@]})"
                for dbname in ${backup_db_arr[@]}
                do
                        echo "database $dbname backup start..."
                        `mkdir -p $backup_dir`
                        `mysqldump -h$mysql_host -P$mysql_port -u$mysql_user -p$mysql_password $dbname --default-character-set=$mysql_charset | gzip > $backup_dir/$dbname-$backup_time.sql.gz`
                        flag=`echo $?`
                        if [ $flag == "0" ];then
                                echo "database $dbname success backup to $backup_dir/$dbname-$backup_time.sql.gz"
                        else
                                echo "database $dbname backup fail!"
                        fi

                done
        else
                echo "ERROR:No database to backup! backup stop"
                exit
        fi
        # 如果开启了删除过期备份，则进行删除操作
        if [ "$expire_backup_delete" == "ON" -a  "$backup_location" != "" ];then
                 #`find $backup_location/ -type d -o -type f -ctime +$expire_days -exec rm -rf {} \;`
                 `find $backup_location/ -type d -mtime +$expire_days | xargs rm -rf`
                 echo "Expired backup data delete complete!"
        fi
        echo "All database backup success! Thank you!"
        exit
fi
```

修改shell脚本的属性

```
chmod 600 /root/mysql_backup.sh
chmod +x /root/mysql_backup.sh
```

设置好属性之后，将命令加入crontab，设置的是每天00:00定时备份然后把备份的脚本目录/var/www/mysql设置为rsync同步目录

```
00 00 * * * /root/mysql_backup.sh
```

### MySQL恢复

导入

```
mysql -u username -p databse < backup.sql
```

