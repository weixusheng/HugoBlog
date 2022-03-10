# 数据库原理及应用 PART-2


事务,完整性约束
<!--more-->

## 事务
### 定义
一条SQL语句被执行,就隐式地开始了一个事务
 - Commit work:提交当前事务,也就是将该事务所做的更新在数据库中持
久保存。在事务被提交后,一个新的事务自动开始
 - Rollback work:回滚当前事务,即撤销该事务中所有SQL语句对数据库
的更新。这样,数据库就恢复到执行该事务第一条语句之前的状态
```sql
update account set balance = balance–100
where account_number ='A-101';
update account set balance = balance + 100
where account_number ='A-201';
COMMIT WORK;
```
这两个更新要么全部成功,要么全部失败

### 事务的四个性质:
 - 原子性(atomic)
 - 一致性(consistency)
 - 隔离性(isolation)
 - 持久性(durability )


## 完整性约束

### 定义
 - 完整性约束保证授权用户对数据库所做的修改不会破坏数据的一致性
 - 域完整性、实体完整性(主键的约束)、参照完整性(外键的约束)
和用户定义的完整性约束

### 单个关系上的约束
 - not null
 - unique
 - check(<谓词>)

### 域约束
 - 域约束是完整性约束的最基本形式,可用于检测插入到数据库中的数
据的合法性
 - 从现有数据类型可以创建新的域
 ```sql
create domain Dollars as numeric(12, 2) not null
create domain Pounds as numeric(12,2);
create table instructor
( ID char(5) primary key,
name varchar(20),
dept_name varchar(20),
salary Dollars,
comm Pounds
);
/*其中numeric(12,2)代表精确度小数点12位整数,小数点后两位整数*/
```
 - check子句可以应用到域上
 ```sql
/*check子句保证教师工资域中只允许出现大于给定值的值*/
create domain YearlySalary numeric(8,2)
constraint salary_value_test check( value >= 29000.00);
/*YearlySalary 域有一个约束来保证年薪大于或等于29 000.00美元
constraint salary_value_test 子句是可选的,它用来将该约束命名为
salary_value_test 。系统用这个名字来指出一个更新违反了哪个约束*/
```
 - 使用in子句可以限定一个域只包含指定的一组值
```sql
create domain degree_level varchar(10)
constraint degree_level_test
check (value in ('Bachelors','Masters','Doctorate'));
```

### 数据库的修改
数据库的修改会导致参照完整性的破坏,这是要考虑使用级联修改或级联删除选项
#### SQL中的级联动作

1. 级联删除:
 由于有了与外码声明相关联的on delete cascade子句,如果删除department 中的元组导致了此参照完整性约束被违反,则删除并不被系统拒绝,而是对course关系作“级联”删除,即删除了被删除系的元组
 ```sql
 create table course (
 . . .
 foreign key( dept_name ) references department
 [ on delete cascade]
 [ on update cascade]
 . . . );
 ```
2. 级联更新也类似


 - 如果存在涉及多个关系的外码依赖链,则在链一端所做的删除或更
新可能传至整个链
 - 但是,如果一个级联更新或删除导致的对约束的违反不能通过进一
步的级联操作解决,则系统终止该事务,即,该事务所做的所有改变及级联动作将被撤销

### SQL中的参照完整性
主码、候选码和外码可在SQL的create table语句中指明
1. primary key子句包含一组构成主码的属性
2. unique子句包含一组构成候选码的属性
3. foreign key子句包含一组构成外码的属性以及被修改外码所参照的关系
名


 - 默认地,外码参照被参照关系中的主码
foreign key ( dept_name ) references department
 - 可以使用如下的简写形式定义单个列为外码
dept_name varchar (20) references department
 - 被参照关系中的属性可以被明确指定(即属性名字可以不同),但是必须被声明为主码或候选
码
foreign key ( dept_name ) references department ( dept_name2 )

