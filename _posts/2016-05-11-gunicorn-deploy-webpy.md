---
layout: post
title: "gunicorn 部署 web.py 应用"
tagline: ""
description: ""
category: 学习笔记
tags: [python, gunicorn, webpy, web, server,]
last_updated:
---

整理文档之，部署 web.py 应用

之前有一个项目使用了 web.py 作为 web server，必然遇到的一个问题就是完成代码之后的部署，网上简单的搜索了一下就确定使用 gunicorn ，比较简单的 wsgi，全称是 web server gateway interface。

## gunicorn
Gunicorn 'Green Unicorn' 是一个 Python WSGI HTTP Server for UNIX. Gunicorn 兼容众多 Python Web 框架，能轻松集成，并且消耗资源少，速度快。

## 安装 {#installation}

    pip install gunicorn

具体可以参考[官网](http://gunicorn.org/#quickstart)，官网推荐使用 virtualenv 来安装。

    #  ...
    app = web.application(urls, globals())
    #  在这里加入下面这句，即可
    application = app.wsgifunc()

然后使用如下命令运行：

    gunicorn code:application

其中 code 就是指 code.py，application 就是那个 wsgifunc 的名字。
这样运行的话， gunicorn 默认作为一个监听 127.0.0.1:8000 的 web server，可以在本机通过： http://127.0.0.1:8000 访问。

    gunicorn -w 1 -b 127.0.0.1:8000 code:application

其中 `-w` 指定 worker 数量， `-b` 指定监听地址

- `--log-file FILE` The Error log file to write to.
- `--log-level LEVEL`  debug, info, warning, error, critical
- `-D, --daemon` 后台执行
- `-p FILE, --pid FILE` filename to use for the PID file
- `-t INT, --timeout INT` Workers silent for more than this many seconds are killed and restarted

更多参数参考[这里](http://docs.gunicorn.org/en/latest/settings.html#settings)

## Nginx 反向代理
Gunicorn 是 WSGI HTTP 服务，通常将其放到 Nginx 服务器后

	server {
		listen 80;
		server_name example.org;
		access_log  /var/log/nginx/example.log;
		location / {
			proxy_pass http://127.0.0.1:8000;
			proxy_set_header Host $host;

			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		}
	}

## 停止 gunicorn
使用如下命令停止 gunicorn

	pkill gunicorn

## reference

- <http://docs.gunicorn.org/en/latest/install.html>
- <http://blog.csdn.net/raptor/article/details/8681357>
- <http://stackoverflow.com/questions/14604653/how-to-stop-gunicorn-properly>
