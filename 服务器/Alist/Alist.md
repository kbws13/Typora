下载

```sh
curl -fsSL "https://nn.ci/alist.sh" | bash -s install
```

更新

```sh
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s update
```

卸载

```sh
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s uninstall
```

反向代理

```
location / {
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Forwarded-Proto $scheme;
  proxy_set_header Host $http_host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header Range $http_range;
	proxy_set_header If-Range $http_if_range;
  proxy_redirect off;
  proxy_pass http://127.0.0.1:5244;
  # the max size of file to upload
  client_max_body_size 20000m;
}
```





放行端口：5524

![QQ截图20230305190010](Alist.assets/QQ%E6%88%AA%E5%9B%BE20230305190010.jpg)

