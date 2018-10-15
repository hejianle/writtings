# 安装

## 命令行

`pip install oslo.db`

## 安装sql后端

### mysql

`pip install PyMySQL`

### psycopg2

`pip install psycopg2`

### pysqlite

`pip install pysqlite`

# 使用

## session 操作

> olso.db 提供了oslo_db.sqlalchemy.enginefacade 系统来实现session管理.这个模块实现了一个装饰context

管理器向函数或块提供session.Session 和connection对象.

Session handling is achieved using the `oslo_db.sqlalchemy.enginefacade` system. This module presents a function decorator as well as a context manager approach to delivering `session.Session` as well as `Connection` objects to a function or block.

Both calling styles require the use of a context object. This object may be of any class, though when used with the decorator form, requires special instrumentation.

# 使用Alembic来管理数据库版本

## 参考链接

<https://www.colabug.com/959711.html>



