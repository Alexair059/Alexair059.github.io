---
comments: true
---

# Nginx

Nginx主要工作流涉及 conf 文件的配置，nginx的重启，实现反向代理

## 基本设置

### conf 文件的配置

Nginx配置文件路径：`/usr/local/nginx/conf`，文件名：`nginx.conf`

多余配置自行查找，此处给出server块的具体设置：

单纯静态资源代理

```conf
   server {
        listen       80;
        server_name  localhost;

        location / {
            root    html;
            index   river.pdf index.html;
       }
	    location /Echo_of_Starsong/introduction {
	        root    /home/myweb;
	        index   introduction.html;
        }
            location /AI/Disco_Diffusion/Official_Guide/CH {
	        root    /home/myweb;
	        index   DD_CH_Basic.pdf;
	    }
	    location /AI/Disco_Diffusion/Official_Guide/EN {
	        root    /home/myweb;
	        index   DD_EN.pdf;
	    }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```

反向代理至uWSGI，公网端口为8000，内部端口为5000

```conf
    server {
        listen       8000;
        server_name  localhost;
        location / {
            include uwsgi_params;
            uwsgi_pass  127.0.0.1:5000;
            }
    }
```

### nginx的重启

> https://blog.csdn.net/m290345792/article/details/78518360

首先建立一个软链接，以便在项目根目录直接执行nginx的bin文件:

当前根目录为：`/home`，nginx的bin文件路径为：`/usr/local/nginx/sbin`

```shell
ln -s /usr/local/nginx/sbin/nginx nginx
```

更改 conf 文件后，对语法进行检查：

```shell
./nginx -t
```

重启：

```shell
./nginx -s reload
```