# SQLAlchemy-2



ORM sqlalchemy
<!--more-->

# merge

在新增数据时,使用 session.merge() 方法替代 session.add()，其实就是 SELECT + UPDATE

merge的作用是合并，查找primary key是否一致，一致则合并，不一致则新建

```python
def insert_interfaceList(data_list):
    """
    数据库-写入更新接口报错数目
    :param data_list: 数据源 list<dic>
    :return: null
    """
    session = con_sql.init_sql()
    v2 = timestr
    for i in range(0, len(data_list)):
        v1 = data_list[i]["interfaceCode"]
        v3 = data_list[i]["errorNum"]
        add_data = mysql_table.InterfaceError(interfaceCode=v1, monitorTime=v2, errorNum=v3)
        session.merge(add_data)
    session.commit()
    session.close()
```

# 映射当前存在的表

```python
from sqlalchemy.orm import sessionmaker
from sqlalchemy import create_engine, MetaData, Table
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.ext.automap import automap_base, name_for_collection_relationship
from sqlalchemy.orm import Session

engine = create_engine('mysql+pymysql://username:password@127.0.0.1/dtec',encoding='utf-8', echo = False)
```

## 使用automap方式映射表

```python
Base = automap_base()
Base.prepare(engine, reflect=True)
sim_all = Base.classes.bigdata_sim_all
session = Session(bind=engine)
add_data = sim_all(supplier_1 = "test5", supplier_2 = "test5", sim_value = 0.7)
session.merge(add_data) # 可以使用session进行操作
session.commit()
session.close()
```

## 使用Table映射

```python
md = MetaData(bind=engine)
session = Session(bind=engine)
test_table= Table('bigdata_sim_all', md, autoload=True, autoload_with=engine)
add_dic = {"supplier_1": "test1","supplier_2": "test3", "sim_value": 0.9}
session.execute(test_table.insert(),add_dic)#增加新数据
session.commit()
session.close()
```

## 使用类映射

```python
Base = declarative_base()
md = MetaData(bind=engine)
session = Session(bind=engine)
class Role(Base):
	#第一个参数是数据表名，第二个参数是元数据，第三个参数表示自动加载
	__table__ = Table("bigdata_sim_all",md,autoload=True)
add_data = Role(supplier_1 = "test1", supplier_2 = "test2", sim_value = 0.7)
session.merge(add_data)
session.commit()
session.close()
```


