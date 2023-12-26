# 一、安装mysql

```
# 1.更新数据源
sudo apt-get update
# 2.安装mysql服务
sudo apt-get install -y  mysql-server
# 3.查看MySQL运行状态
sudo systemctl status mysql
# 4.连接MySQL
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码';
FLUSH PRIVILEGES;
```

> 安装后一般默认是启动的,可以通过下述的语句来实现服务的开启或关闭

```
/etc/init.d/mysql start  启动
 
/etc/init.d/mysql stop   停止
 
/etc/init.d/mysql restart 重启
 
/etc/init.d/mysql status  查看状态
```

> **远程访问：**
>
> **创建用户**
>
> 默认的 root 用户只能当前节点 localhost 访问，是无法远程访问的，需要创建一个 root 账户，用户远程访问
>
> `create user 'root'@'%' identified with mysql_native_password by '123456';`
>
> **给root用户分配权限**
>
> `grant all on *.* to 'root'@'%';`

# 二、安装java并运行

```
# 安装java
sudo apt-get install openjdk-8-jdk

# 测试运行
sudo java -jar jar包名.jar

# ctrl+c退出运行

# 后台运行
sudo nohup java -jar jar包名.jar &
```

# 三、安装Nginx并修改配置文件

> sudo apt-get install nginx #安装
>
> nginx -v #查看安装版本
>
> 启动或停止
>
> sudo /etc/init.d/nginx start|stop|restart|status
> 或
> sudo service nginx start|stop|restart|status

修改配置文件

> sudo vim /etc/nginx/sites-enabled/default

```
server {
	listen 2333; #Nginx监听的服务器端口
	listen [::]:2333;
	server_name _;

	location / {
		proxy_pass  http://127.0.0.1:2000; #重定向到127.0.0.1的2000duan'ko
	}
}
```

**注意：**修改配置文件之后，一定要重启Nginx代理服务器使配置生效！！！

# 四、部署常见流程

> 1. 上传文件至指定目录
>
> 2. **sudo netstat -nultp**（需要apt安装net-tools包） 查看端口占用情况
> 3. 若端口被占用使用 **kill -9 进程号** 杀掉占用进程,若未被占用则走下一步
> 4. **sudo** **nohup java -jar jar包名.jar &** 后台启动项目
> 5. 修改Nginx配置文件使其监听指定端口
> 6. 查看项目同级目录下的**nohup.out**文件的输出日志

