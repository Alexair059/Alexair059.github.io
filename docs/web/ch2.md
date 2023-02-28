# Nginx

## 基本设置

Nginx主要工作流涉及 conf 文件的配置，nginx的重启，实现反向代理

### conf 文件的配置

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