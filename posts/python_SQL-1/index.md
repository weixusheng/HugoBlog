# SQLAlchemy



ORM sqlalchemy
<!--more-->

### 连接mysql

```python
from sqlalchemy.orm import sessionmaker

DBSession = sessionmaker(bind=engine)
session = DBSession()
```

### 创建表

```python
from sqlalchemy import create_engine, MetaData, Table, Column, Integer, String

engine = create_engine('mysql+pymysql://fxq:123456@192.168.100.101/sqlalchemy')
metadata = MetaData(engine)


student = Table('student', metadata,
            Column('id', Integer, primary_key=True),
            Column('name', String(50), ),
            Column('age', Integer),
            Column('address', String(10)),
)

metadata.create_all(engine)
```

### 添加

```python
student1 = Student(id=1001, name='ling', age=25, address="beijing")
student2 = Student(id=1002, name='molin', age=18, address="jiangxi")
student3 = Student(id=1003, name='karl', age=16, address="suzhou")

session.add_all([student1, student2, student3])
session.commit()
session.close()
```



### 查询

```python
my_stdent = session.query(Student).filter_by(name="fengxiaoqing2").first()
print(my_stdent.id,my_stdent.name,my_stdent.age,my_stdent.address)
```

#### filter()  过滤表的条件

```python
my_stdent = session.query(Student).filter(Student.name.like("%feng%"))
# equals:
query(Student).filter(Student.id == 10001)
# not equals:
query(Student).filter(Student.id != 100)
# LIKE:
query(Student).filter(Student.name.like(“%feng%”))
# IN:
query(Student).filter(Student.name.in_(['feng', 'xiao', 'qing']))
not in
query(Student).filter(~Student.name.in_(['feng', 'xiao', 'qing']))
# AND:
from sqlalchemy import and_
query(Student).filter(and_(Student.name == 'fengxiaoqing', Student.id ==10001))
# 或者
query(Student).filter(Student.name == 'fengxiaoqing').filter(Student.address == 'chengde')
# OR:
from sqlalchemy import or_
query.filter(or_(Student.name == 'fengxiaoqing', Student.age ==18))
```

#### all()返回列表

```python
my_stdent = session.query(Student).filter(Student.name.like("%feng%")).all()
```

#### filter()和filter_by()

Filter：  可以像写 sql 的 where 条件那样写 > < 等条件，但引用列名时，需要通过 类名.属性名 的方式。
 filter_by：  可以使用 python 的正常参数传递方法传递条件，指定列名时，不需要额外指定类名。，参数名对应名类中的属性名，但似乎不能使用 > < 等条件。

当使用filter的时候条件之间是使用“=="，fitler_by使用的是"="

```python
user1 = session.query(User).filter_by(id=1).first()
user1 = session.query(User).filter(User.id==1).first()

# 多重查询
q = sess.query(IS).filter(IS.node == node and IS.password == password).all()
```

### 更新

```python
my_stdent = session.query(Student).filter(Student.id == 1002).first()
my_stdent.name = "fengxiaoqing"
my_stdent.address = "chengde"
session.commit()
student1 = session.query(Student).filter(Student.id == 1002).first()
print(student1.name, student1.address)
```

### 删除

```python
session.query(Student).filter(Student.id == 1001).delete()
session.commit()
session.close()
```

### 统计

```python
session.query(Student).filter(Student.name.like("%feng%")).count()
```

### group_by()

```python
std_group_by = session.query(Student).group_by(Student.age)
print(std_group_by)
```

### 排序反序

```python
std_ord_desc = session.query(Student).filter(Student.name.like("%feng%")).order_by(Student.id.desc()).all()
for i in std_ord_desc:
  print(i.id)
```

