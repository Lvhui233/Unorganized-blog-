---

laravel上传简单配置nginx

---

代码上传到服务器时，涉及到权限，配置等的设置

## 框架文件权限设置

```
chmod -R 777 boostrap
chmod -R 777 storage
```

## 修改nginx默认web路径

修改`/etc/nginx/nginx.conf`配置文件,修改serve下的root

```
server {
        listen       80 default_server; 
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
            index index.php index.html index.htm;
        }

        location ~ .php$ {
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include fastcgi_params;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
```

修改之后重启nginx`nginx -s reload`

