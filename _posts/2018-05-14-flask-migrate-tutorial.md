---
layout: post
title: "使用 flask migrate 来迁移数据结构"
tagline: ""
description: ""
category: 学习笔记
tags: [flask, sql, migrate, sqlalchemy, alembic]
last_updated: 
---

最近在学习使用 Flask 生成一个短链接服务时看到了 Flask-Migrate 这样一款插件，之前学习 Django 的时候自带数据库迁移工具， Flask 中也有这样一款，不过是以插件的形式出现，Flask Migrate 基于 [Alembic](http://alembic.readthedocs.org/en/latest/)，Alembic 是 SQLAlchemy 作者开发的数据迁移工具。

文档主页：<https://flask-migrate.readthedocs.io/en/latest/>

## 安装

    pip install Flask-Migrate

在安装完成之后需要在代码中添加如下代码

    from flask import Flask
    from flask_sqlalchemy import SQLAlchemy
    from flask_migrate import Migrate

    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'

    db = SQLAlchemy(app)
    migrate = Migrate(app, db)

    class User(db.Model):
        id = db.Column(db.Integer, primary_key=True)
        name = db.Column(db.String(128))

在添加代码之后需要运行

    flask db init

来初始化项目，命令运行之后会在项目中生成 `migrations` 文件夹，该文件夹需要添加到版本控制。

之后可以使用 `migrate` 命令来初始化迁移

    flask db migrate

迁移脚本不会检测models 的所有变更， Alembic 目前无法检测表名修改，列名修改，其他限制可以在[Alembic网站](http://alembic.zzzcomputing.com/en/latest/autogenerate.html#what-does-autogenerate-detect-and-what-does-it-not-detect)查看。

之后可以应用该迁移

    flask db upgrade

运行该命令来将修改应用到数据库，以后对model的每一次修改需要重复 `migrate` 和 `upgrade` 命令。如果要在不同机器之间同步数据库结构，只需要同步 migrations 文件夹，并且在另一台机器上运行 `flask db upgrade` 即可。

Flask Migrate 也支持直接使用脚本的方式运行，具体可参考[官方的文档](https://flask-migrate.readthedocs.io/en/latest/)，非常易懂。



