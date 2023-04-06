---
comments: true
---

# uWSGI

uWSGI是 Python 语言定义的 Web 服务器和 Web 应用程序或框架之间的一种简单而通用的接口，常用来配合nginx实现客户端响应

## 安装

pip 安装：

```shell
pip install uwsgi
```

## 基本设置

### ini 文件的配置

以我的一个flask项目为例，项目路径为`/home/flask`，入口文件为`app.py`

需要在项目根目录下创建 uwsgi_conf.ini 文件：

```
[uwsgi]
#监听端口
socket = 127.0.0.1:5000
#进程
processes = 8
#项目路径
chdir = /home/flask
#项目名称
wsgi-file = app.py
#回调
callable = app
```

对 app.py 的入口函数进行修改：

```python
if __name__ == '__main__':
    app.run(host='127.0.0.1',port=5000,debug=False)
```

注意flask接收的是内部端口5000

### uWSGI启动

个人习惯使用tmux直接挂起会话：

```shell
uwsgi uwsgi_conf.ini
```

CTRL + C 即可退出