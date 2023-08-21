# Jupyter Lab

## 下载

```
pip install jupyterlab
```

## 运行

```
jupyter lab --ip 0.0.0.0 --allow-root
```

## 修改配置文件

```
vim .jupyter/jupyter_notebook_config.py
```

![image-20230402202505789](Jupyter%20Lab.assets/image-20230402202505789.png)

修改ip为`0.0.0.0`

![image-20230402202526843](Jupyter%20Lab.assets/image-20230402202526843.png)

修改默认打开文件夹

## 部署

后台运行

```
nohup jupyter lab --ip 0.0.0.0 --allow-root &
```

nginx配置

```
server {
        listen 80;
        server_name  py.kbws.xyz;
        location / {
            proxy_pass          http://127.0.0.1:8888;
            proxy_set_header    Host $host;
            proxy_set_header    X-Real-Scheme $scheme;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_http_version  1.1;
            proxy_set_header    Upgrade $http_upgrade;
            proxy_set_header    Connection "upgrade";
            proxy_read_timeout  120s;
            proxy_next_upstream error;
        }
    }
```

