# 入门

## 快速开始

全局安装`docsify-cli`工具

```bash
npm i docsify-cli -g
```



## 初始化项目

如果想在项目的`./docs`目录中写文档，可以通过`init`初始化项目

```bash
docsify init ./docs
```

## 开始写文档

初始化成功后，可以在`./docs`目录中看到以下文件

- `index.html`入口文件
- `README.md`会作为主页内容渲染
- `.nojekyll`用于组织Github Pages忽略掉下划线开头的文件

直接编辑`docs/README.md`就能更新文档内容，也可以添加更多的页面

## 本地预览

通过预览`docsify serve`启动一个本地服务器，可以方便地实时预览效果，默认访问地址http://localhost:3000

```bash
docsify serve docs
```



# 多页文档

## 链接

如果需要创建多个页面，或者需要多级路由的网站，在docsify里面很容易实现。例如创建一个`guide.md`文件，那么对应的路由就是`/#/guide`。

假设目录结构如下：

```text
docs
	|-README.md
	|-guide.md
	|-zh-cn
		|-README.md
		|-guide.md
```

那么对应的访问页面将是：

```text
docs/README.md		=>http://doman.com
docs/guide.md		=>http://doman.com/guide
docs/zh-cn/README.md		=>http://doman.com/zh-cn/
docs/zh-cn/guide.md			=>http://doman.com.zh-cn/guide
```

例如从首页跳转到guide页面

```md
[>>操作指南](guide.md)
```

从guide页面跳转到首页

```md
[<<返回首页](/)
```



## 侧边栏

为了获取侧边栏，需要创建自己的 _sidebar.md，也可以自定义加载的文件名。默认情况下侧边栏会通过Markdown文件自动生成

首先配置`loadSidebar`选项

```html
<!-- index.html -->

<script>
    window.$docsify = {
        name: '',
        repo: '',
        loadSidebar: true
    }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify@4"></script>
```

接着创建`_sidebar.md`文件，内容如下

```html
<!-- docs/_sidebar.md -->

* [首页](zhcn/)
* [指南](zh-cn/guide)

```



## 多级目录

```markdown
<!-- docs/_sidebar.md-->

* [首页](/)
* [操作指南](zh-cn/guide.md)

* 前端技术
    * [javascript](前端/javaScript/JavaScript.md)
    * [vue技术](前端/vue/vue.md)
```

### 显示文章的标题

开启目录功能，需要设置`subMaxLevel`配置项

数字代表要显示的标题级别

```html
<!-- index.html -->

<script>
    window.$docsify = {
        name: '',
        repo: '',
        loadSidebar: true,
        subMaxLevel: 2
    }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify@4"></script>
```



# 导航栏

## HTML

如果需要定制导航栏，则可以用HTML创建一个导航栏

```text
文档的链接都要以 #/ 开头
```

```html
<!-- index.html -->

<body>
    <nav>
    	<a href="#/">EN</a>
        <a href="#/zh-cn/">中文</a>
    </nav>
    <div id="app"></div>
</body>
```

## 配置文件

可以通过Markdown文件来配置导航。首先配置`loadNavbar`，默认加载的文件为`_navbar.md`。

```html
<!-- index.html -->

<script>
    window.$docsify = {
        name: '',
        repo: '',
        loadNavbar: true
    }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify@4"></script>
```

```markdown
<!-- _navbar.md -->

* [EN](/)
* [中文](/zh-cn/)
```

`_navbar.md`加载逻辑和`sidebar`文件一样，从每层目录下获取。例如当前路由为`/zh-cn/custom-nacbar`那么是从`/zh-cn/_navbar.md`获取导航栏



# 封面

通过设置`coverpage`参数，可以开启开启渲染封面功能

## 基本用法

封面的生成同样是从markdown文件渲染过来的。开启渲染封面功能后在文档根目录创建`_coverpage.md`文件

```html
<!-- index.html -->

<!-- index.html -->

<script>
    window.$docsify = {
        name: '',
        repo: '',
        coverpage: true
    }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify@4"></script>
```

```markdown
![logo](_media/icon.svg)

# docsify <small>3.5</small>

> 一个神奇的文档网站生成器。

- 简单、轻便 (压缩后 ~21kB)
- 无需生成 html 文件
- 众多主题

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#docsify)
```

如果想要将封面和首页分离，可以修改`onlyCover`配置项，`onlyCover`默认为false，将其修改为true即可

```html
<!-- index.html -->

<script>
    window.$docsify = {
        name: '',
        repo: '',
        coverpage: true
    }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify@4"></script>
```



# 主题

将自己喜欢的主题复制到`index.html`的head标签中即可

```html
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/vue.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/buble.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/dark.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/pure.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/dolphin.css">
```



# 部署

## 创建仓库



## 上传