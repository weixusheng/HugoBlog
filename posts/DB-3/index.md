# 数据库原理及应用 PART-3


Trigger,数据库安全性
<!--more-->

## Trigger
### 定义
 - 触发器(trigger)是由数据库更新操作引起的被系统自动执行的语句
 - 设计触发器必须:
    - 指明触发器被执行的条件
    - 指明触发器执行时所做的具体操作
即可以将触发器看作是函数,在触发条件下被调用

```sql
create trigger timeslot_check1 after insert on section
referencing new row as nrow
for each row
when ( nrow.time_slot_id not in
(select time_slot_id
from time_slot )) /* time_slot 中不存在该time_slot_id */
begin
rollback
end;
```

### 触发事件
包括insert, delete和update
 - 针对update的触发器可以指定具体修改的属性
create trigger takes_trigger after update of takes on grade
 - 更新前后的属性值可通过下列方法被引用
 - referencing old row as orow:对删除和修改有效
 - referencing new row as nrow:对插入和修改有效

### for each statement
 针对受到一个事务影响的所有行只执行一次操作
for each statement || for each row
也可以用referencing old table 或 referencing new table 来引用包含受影响的行的临时表

### 外部动作
触发器不能直接实现外部动作, 但是触发器可以在某个表中记录将采取的行动, 而让另一个外部进程不断扫描该表并执行相应的外部动作

```sql
create trigger reorder_trigger after update of level on inventory
referencing old row as orow , new row as nrow
for each row
when nrow.level < =
(select level
from minlevel
where minlevel.item = nrow.item ) and orow.level >
(select level
from minlevel
where minlevel.item = orow.item )
begin
insert into orders
(select item , amount
from reorder
where reorder.item = orow.item )
end
```
判断数值条件超过临界值 oldrow小于等于临界值 newrow大于临界值

## 数据库安全性
### 授权
 - 读权限 – 允许读,但不允许更新数据
 - 插入权限 – 允许插入新数据, 但不允许更新现有数据
 - 修改权限 – 允许修改, 但不允许删除数据
 - 删除权限 – 允许删除数据


 **对修改数据库模式的授权包括:**
 - 索引权限 – 允许创建和删除索引
 - 资源权限 – 允许创建新关系
 - 修改权限 – 允许增加或删除关系的属性
 - 删除权限 – 允许删除关系

### 视图的巧妙使用-权限管理
 - 用户可被授予关于视图的权限, 而不被授予关于该视图定义中涉及的关系的权限
 - 视图隐藏数据的能力既能简化系统的使用又能增强安全性(只允许用户存取他们工作中需要的数据)
 - 关系级安全性与视图级安全性的结合使用可精确地将用户存取限制在他所需要的数据上

例如一个工作人员只需要知道一个给定系(比如Geology系)里所有员工的工资,但无权看到其他系中员工的相关信息
 - 方法:不允许对 instructor 关系的直接访问,但授予对视图
geo_instructor 的访问权限,该视图仅由属于Geology系的那些instructor元组构成
 - 视图 geo_instructor 用SQL定义如下:
 ```sql
create view geo_instructor as
(select *
from instructor
where dept_name = 'Geology');
```

### 角色
 - 通过创建角色可以一次性对**一类用户**指定其共同的权限
 - 像对用户一样, 可以对角色授予或收回权限
 - 角色可被赋予给用户, 甚至给其他角色

#### 授权 grant
```sql
create role instructor ;
grant select on takes to instructor ;
```
#### 收回权限 revoke
```sql
REVOKE< privilege list > ON < relation name or view name >
FROM < user list > [ restrict | cascade ]
/*例如*/revoke select on instructor from U 1 ,U 2 ,U 3 cascade
```
级联回收: cascade   
阻止级联收回: restrict

## ODBC
### 定义
简单来说ODBC就是高级语言用户连接数据库的一个中间件,快捷方便调用数据的程序
 - ODBC程序会先分配一个SQL的环境变量,然后是一个数据库连接句柄
 - ODBC程序随后会利用SQLConnect打开和数据库的连接,这个调用有几个参数包括:
    1. 数据库的连接句柄
    2. 要连接的服务器
    3. 用户的身份
    4. 密码

因为之前一直搞web开发,接触了很多次ODBC,这里就不过多记录了

## 数据库设计
### 定义
数据库可被建模为:
 - 实体集合(实体集)
 - 实体间联系(联系集)
 这部分也很常识...只写干货啦

### 联系集的度
 - 参加联系的实体集的个数
 - 涉及两个实体集的联系集称为二元的(二元联系)
 - 联系集可以涉及多于两个的实体集
例如假设一个student在每个项目上最多只能有一位导师,包含三个实体集 instructor 、 student 和 project (三元联系:项目导师学生,丁字形关系)
 - 多于两个实体集之间的联系较少见,数据库系统中的联系集一般多为二元的

### 映射基数
 - 表达可与一个实体通过联系集进行关联的其他实体的个数
 - 二元联系集的映射基数有以下几种情况:
      - 一对一,如:就任总统(总统,国家)
      - 一对多,如:分班情况(班级,学生)
      - 多对一,如:就医(病人,医生)
      - 多对多,如:选课(学生,课程)

### 实体集中的键和码
 - 实体集的超码是能够唯一标识每个实体的一个或多个属性
 - 候选码是实体集的最小超码
 - 候选码可能存在多个,我们只会选择一个候选码作为主码或主键
   例, instructor ( ID , name , dept_name , salary )
   - 候选码: ID
   - 超码:{ ID },{ ID , name },{ ID ,...}


**明天更ER图,数据库马上就学完啦!**

