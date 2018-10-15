[TOC]

# 安装

```python
pip install sqlalchemy
```



#　版本检查

```python
import sqlalchemy
sqlalchemy.__version__
```

#　连接数据库

```python
from sqlalchemy import create_engine
    engine = create_engine("mysql+pymysql://scott:tiger@localhost/test", echo=True)
```

在create_engine函数中的echo参数用来设置sqlalchemy的日志，如果设置为True则可以看到详细的sql信息，设置为False则有较少的输出．这个函数返回一个Ｅngine实例，代表了跟数据库的接口．当执行engine.excute或者engine.connect()时会创建真正的DBAPI链接．在实际中使用ＯＲＭ时并不直接使用create_engine.

# 声明一个mapping

使用ROM要完成两项工作：

* 描述要操作的表
* 创建要映射到的表的类

sqlalchemy 通过使用Declear系统完成上面两项任务，使我们可以创建一个class，这个类包含了针对数据库表操作的指令

```python
from sqlalchemy.ext.declarative import declarative_base
Base = declarative_base()
```

可以根据Base类创建任意数量的映射类，现在我们创建一个User类继承自Base:

```python
from sqlalchemy import column, Integer, String
class User(Base):
	__tablename__ = 'users'
	id = Cloumn(Integer, primary_key=True)
    name = Column(String(50))
	password = Column(String(25))
    
    def __repr__(self):
        return '<User(name=%s, password=%s)>' % (self.name, self.password)
```

上面声明类的方式至少需要一个`__tablename__`的属性和一个代表主键的`Column`.sqlalchemy不会做任何的假设，自动帮你生成主键(django的ORM是可以的)．

# 生成表

上面已经创建了一个映射到数据库的表，那么这个类是如何生成数据库中的表的呢？

1. 当我们使用Declare创建完User类的时候，就定义了表的信息即表的元数据．在sqlalchemy中使用的`Table`类来保存这些信息．Declare系统帮我们创建了了一个．可以通过User的`__table__`属性查看：

```python
>>> User.__table__ 
Table('users', MetaData(bind=None),
            Column('id', Integer(), table=<users>, primary_key=True, nullable=False),
            Column('name', String(), table=<users>),
            Column('password', String(), table=<users>), schema=None)
```

2. 当声明类的时候，Declare使用了python的metaclass，这样在完成之前可以执行额外的操作．其中`Table`的对象就是在这个阶段完成的．然后通过构建一个`Mapper`类的对象完成了表到类的绑定．
3. `Table`对象是`Metadata`的一个元素．而Metadata是一个集合.使用Declare时可以使用`Base`类的metadata属性去查看.
4. Metadata是一个注册表，这个注册表可以执行一系列的产生数据表的命令．可以通过Metadata的方法来产生User数据库．可以通过将engine传递给Metadata.create_all()来生成书库库中不存在的表：

```python
>>>Base.metadata.create_all()
SELECT ...
PRAGMA table_info("users")
()
CREATE TABLE users (
    id INTEGER NOT NULL, name VARCHAR,
    fullname VARCHAR,
    password VARCHAR,
    PRIMARY KEY (id)
)
()
COMMIT
```

到现在为止我们已将使用sqlalchemy创建了一个User表．

# 创建一条数据

以上的内容已经在数据库创建了一张user表．现在应该向里面添加一条数据．一个类对应一张表，那么一个类的实例就对应表里面的一行数据．创建一条数据如下：

```python
>>>user1 = User(name='name_example', password='123456')
>>>user1.name
name_example
>>>str(user1.id)
'None'
```

从上面的结果可以看出现在`user.id`还是`None`.这是因为现在user还不是数据库中的数据．当向数据库执行INSERT 命令时就会设置这些属性的值．

# 创建session

通过跟数据建立session，来实现跟数据库的交互．

```python
from sqlalchemy import create_engine
from sqlalchemy import sessionmaker
engine = create_engine("mysql+pymysql://scott:tiger@localhost/test")
Session = sessionmaker(bind=engine)
```

使用上面的Session类可以创建跟数据库交互的session对象．

# 添加或更改数据

现在使用上面的Session类可以向数据库中添加User实例了．

## 添加数据

向数据库中添加数据可以通过session的add函数来完成：

```python
session = Session()
session.add(user1)
```

上面的代码还没有将user1插入到数据库中，还处于`pending`状态．当执行`flush`操作的时候数据才被插入数据库．但是当我们执行查询该条数据时，所有处于`pending`状态的数据都会被`flush`到数据库．

## 查询数据

可以通过`session.query`创建一个`Query`对象加载了`User`对象．然后通过`filter_by`函数进行有条件的过滤

```python
>>> our_user = session.query(User).filter_by(name='ed').first() 
>>> our_user
<User(name='ed', fullname='Ed Jones', password='edspassword')>
```



#　参考链接　

