# Ubuntu环境下服务器安装部署Code-server

## 安装

在下面连接下载安装包

https://github.com/cdr/code-server/releases/download/3.4.1/code-server_3.4.1_amd64.deb

安装

```
sudo dpkg -i code-server_3.4.1_amd64.deb
```

## 运行

终端输入

```
code-server
```

服务会自行开启，默认8080端口，127.0.0.1的访问ip

## 修改配置文件

```
vim ~/.config/code-server/config.yaml
```

如果你是在云服务器主机上架设的话需要将`127.0.0.1`改成`0.0.0.0:8080`，password为登录密码

修改完配置文件后需要重启code-server服务

```
systemctl --user restart code-server
## 启动
systemctl --user start code-server
## 查看状态
systemctl --user status code-server
## 关闭
systemctl --user stop code-server
```

## 让code-server后台运行

安装tmux

```
# Ubuntu 或 Debian
sudo apt-get install tmux

# CentOS 或 Fedora
sudo yum install tmux
```

创建一个新的会话。

```shell
tmux new -s code_server
```

在新会话中执行code-server启动指令即可。 关闭ssh会话以后程序继续在后台运行

```shell
code-server --cert [你的证书存放路径] --cert-key [你的key路径] --bind-addr 0.0.0.0:[你的端口号]
```

需要再查看code-server运行状态的话，只需要访问code_server会话就行了

```shell
# 接入code_server会话
tmux a -t code_server
# 杀掉code_server会话
tmux kill-session -t code_server
```

在新会话启动code-server后就可以关闭会话窗口，进程仍然会在后台运行

## 使用二级域名访问

使用nginx设置代理

```
server {
    listen 80;    # 监听端口
    server_name code.your_server_name;       # 域名
    # nginx请求日志地址
    access_log  /usr/local/webserver/nginx/logs/code.access.log;  
    location / {
        proxy_pass  http://localhost:8082;
        proxy_redirect     off;
        proxy_set_header   Host             $host;          # 传递域名
        proxy_set_header   X-Real-IP        $remote_addr;   # 传递ip
        proxy_set_header   X-Scheme         $scheme;        # 传递协议
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_set_header   Accept-Encoding  gzip;
        # code-server的websocket连接需要Upgrade、Connection这2个头部
        proxy_set_header   Upgrade          $http_upgrade;
        proxy_set_header   Connection       upgrade; 
    }
}
```

