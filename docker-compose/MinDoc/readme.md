
> 访问地址： 127.0.0.1:8181
> 访问账户   admin
> 访问密码   123456

### nginx 配置

```nginx configuration
server {
    listen       80;

    #此处应该配置你的域名：
    server_name  doc.iminho.me;

    charset utf-8;

    #此处配置你的访问日志，请手动创建该目录：
    access_log  /var/log/nginx/webhook.iminho.me/access.log;
    location / {
        try_files /_not_exists_ @backend;
    }

    # 这里为具体的服务代理配置
    location @backend {
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header Host            $http_host;
        proxy_set_header   X-Forwarded-Proto $scheme;
        #此处配置 MinDoc 程序的地址和端口号
        proxy_pass http://127.0.0.1:8181;
    }
}
```