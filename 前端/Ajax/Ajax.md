Ajax是一种异步、无刷新的技术

导入jQuery

# 常用方法

```js
jQuery.get()
所有参数：
	url：待载入页面的URL地址
    data：待发送的key/value参数
	success：载入成功时回调的函数
    dataType：返回内容格式，xml，json，script，text，html
jQuery.post()
所有参数：
	url：待载入页面的URL地址
    data：待发送的key/value参数
	success：载入成功时回调的函数
    dataType：返回内容格式，xml，json，script，text，html
```

```js

格式：$.ajax({});
参数：
    type：请求方式GET/POST
    url: 请求地址 url
    async：是否一步，默认是 true 表示异步
    data：发送到服务器的数据
    dataType：预期服务器返回的数据类型
    contentType：设置请求头
    success：请求成功时调用此函数
    error：请求失败时调用此函数
```

