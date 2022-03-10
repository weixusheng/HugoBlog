# 数据库原理及应用 PART-1


绪论,关系模型,SQL语言

<!--more-->

## 绪论
### 数据库解决的问题
在文件处理系统中存储组织信息的主要弊端:
 - 完整性问题
1. 完整性约束(如账户余额>0)成为程序代码的一部分
2. 增加新的约束或更改现有的约束很困难
 - 原子性问题
1. 在进行部分数据更新时,一旦发生故障,可能导致数据库处于不一致的状态,例如,从一个账户转移资金到另一个账户,此操作要么完成,要么根本不会发生
 - 并发访问异常
1. 为了提高系统的总体性能,许多系统允许并发访问
2. 不受控制的并发访问可能导致数据不一致,例如,两个用户读取同一账号余额,并在同一时间更新它
 - 安全性问题
1. 并非所有用户都可以访问所有数据

### 数据库结构
1. 物理层:描述数据实际上是怎样存储的
2. 逻辑层:描述数据库中存储什么数据及这些数据间存在什么关系
 - 如type instructor = record
ID : char(5);
name : char(20);
dept_name : char(20);
salary : numeric(8,2);
end;
3. 视图层:应用程序能够隐藏数据类型的详细信息。视图也可以出于安全目的隐藏数据信息(例如,员工的薪水)

![数据库设计结构](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-1/1.png)
数据库设计结构
<br>

![数据库实体结构](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-1/2.png)
数据库实体结构


### 实例和模式
类似编程语言中的类型(types)和变量(variables)
类型<->模式       变量<->实例
1. 模式(Schema):数据库的总体设计
 - 类似于程序中变量的类型信息
 - 物理模式:在物理层描述数据库的设计
 - 逻辑模式:在逻辑层描述数据库的设计
2. 实例(Instance):特定时刻存储在数据库中的信息的集合
 - 类似于程序中变量的值

### 物理独立性和逻辑独立性
修改一层的结构定义不影响更高层的结构定义
1. 物理数据独立性 :修改物理结构而不需要改变逻辑结构的能力
 - 应用程序依赖于逻辑结构
 - 应用程序独立于数据的结构和存储
 - 这是使用DBMS最重要的好处
2. 逻辑数据独立性:数据逻辑结构的改变不影响应用程序
 - 逻辑数据独立性一般难以实现,因为应用程序严重依赖于数据的逻辑结构

### 数据库语言
数据库语言:
 - Data Definition Language(DDL,数据定义语言)
 - Data Manipulation Language(DML,数据操纵语言)
 - Data Control Language(DCL,数据控制语言)

#### 数据定义语言(DDL)
 - 指定一个数据库模式作为一组关系模式的定义
 - 指定存储结构,访问方法和一致性约束
 - DDL语句经过编译,得到一组存储在一个特殊文件中的表,特殊文件即数据字典(data dictionary),其中包含元数据(metadata)
 - 例如该SQL语句创建了表account
 ```
 CREATE TABLE account (account_number char(10),balance integer);
 ```
 - 数据字典(data dictionary)包含元数据(metadata),包括:
    - 数据库模式
    - 数据存储结构
    - 访问方法和约束
    - 统计信息
    - 授权

#### 数据操纵语言(DML)
 - 从数据库中检索数据
 - 插入/删除/更新数据
 - DML也称为查询语言
 - 两类基本的数据操作语言:
     - **过程化DML:** 要求用户指定需要什么数据,以及如何获得这些数据(C,Pascal,Java,...)
     - **声明式DML:** 也称为非过程化DML,只要求用户指定需要什么数据,而不指明如何获得这些数据(SQL,Prolog)

#### 数据控制语言(DCL)
 - DCL（Data Control Language）是数据库控制语言。是用来设置或更改数据库用户或角色权限的语句，包括（grant,deny,revoke等）语句。
 - 在默认状态下，只有sysadmin,dbcreator,db_owner或db_securityadmin等人员才有权力执行DCL

#### SQL
**SQL = DDL + DML + DCL**

## 关系模型
### 定义
 - 关系数据库基于关系模型,是一个或多个关系组成的集合
 - 关系通俗来讲就是表(由行和列构成)
 - 关系模型的主要优点是其简单的数据表示,易于表示复杂的查询
 - SQL语言是最广泛使用的语言,用于创建,操纵和查询关系数据库,而关系模型是其基础。

1. 属性类型
 - 关系的每个属性都有一个名称
 - 域:每个属性的取值集合称为属性的域
 - 属性值必须是原子的,即不可分割(1NF,第一范式)
     - 多值属性值不是原子的
     - 复合属性值不是原子的
 - 特殊值null是每一个域的成员
 - 空值给数据库访问和更新带来很多困难,因此应尽量避免使用空值

2. 关系概念
 - 关系涉及两个概念:关系模式和关系实例
 - 关系模式描述关系的结构:
```
Instructor-schema = ( ID : string, name : string, dept_name : string,
salary: int)
Instructor-schema = ( ID , name, dept_name, salary )
```
 - 关系实例表示一个关系的特定实例,也就是所包含的一组特定的行
 - 关系、关系模式、关系实例区别:
     - 变量<->关系
    - 变量类型<->关系模式
    - 变量值<->关系实例

3. 码-键
 - 使K 属于 R
 - 如果K值能够在一个关系中唯一地标志一个元组,则K是R的超码
```
{ instructor-ID, instructor-name }
{ instructor-ID }
都是instructor 的超键
```
 - 如果K是最小超码,则K是候选码
```
{ instructor-ID }是 instructor 的候选码
因为它是一个超码,并且它的任意真子集都不能成为一个超码
```
 - 如果k是一个候选码,并由用户明确定义,则K是一个主键。主键通常用下划线标记

4. 外键
 - 假设存在关系r和s:r(A, B, C), s(B, D),则在关系r上的属性B称作参照s的外码,r也称为外码依赖的参照关系,s叫做外码被参照关系
```
instructor ( ID , name , dept _ name , salary ) - 参照关系
department ( dept_name , building , budget ) - 被参照关系
```

### 关系代数
 - 在某种程度上是过程化语言
 - 六个基本运算
     - Select 选择
     - Project 投影
     - Union 并
     - set difference 差(集合差)
     - Cartesian product 笛卡儿积
     - Rename 更名(重命名)
 - 用户输入一个或两个关系,并得到新的关系
 - 附加运算
    - Set intersection 交
    - Natural join 自然连接
    - Division 除
    - Assignment 赋值
 - 关系运算的优先级:
投影>选择>笛卡尔积>连接、除>交>并、差

 - 扩展关系代数运算
    - 广义投影
    - 聚集函数
    聚集运算的结果是没有名称的
     - 可以使用更名运算为其命名
     - 可以把重命名作为聚集运算的一部分,如:
     branch-name g sum(balance) as sum-balance ( account )

     ![聚集函数](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-1/3.png)
     聚集函数

        **1.  avg: 平均值:**
        例: g <sub>avg(balance) </sub>(account)
        **2. min: 最小值**
        **3. max: 最大值 (求平均存款余额)**
        **4. sum: 值的总和**
        **5. count:值的数量**
    - 外连接

    ![外连接](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-1/4.png)
    ![外连接](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-1/5.png)

       - 外连接运算是连接运算的扩展,可以处理缺失信息
       - 保留一侧关系中所有与另一侧关系的任意元组都不匹配的元组,再把产生的元组加到自然连接的结构上
       - 使用空值:空值表示值不知道或不存在

### 数据库的修改
数据库的内容可以使用下面的操作来修改:
 - 删除
 - 插入
 - 更新

所有这些操作都使用赋值操作表示(使用关系代数)
1. 删除可表达为:
r <- r – E
![删除操作](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-1/6.png)
其中 r是关系,E是关系代数查询

2. 插入
r <- r 并 E
![插入操作](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-1/7.png)

3. 更新
![更新操作](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-1/8.png)
![更新操作](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-1/9.png)


## SQL语言
### 组成
SQL语言有以下几个部分:
 - DDL (Data-definition Language) 数据定义语言
     - create table,alter table,drop table
     - create index,drop index
     - create view,drop view
     - create trigger,drop trigger
     - ...
 - DML (Data-manipulation Language) 数据操纵语言
     - select ... from
     - insert, delete, update
 - DCL (Data-control Language) 数据控制语言
     - grant, revoke

### 基本数据类型
 - char(n):固定长度字符串,用户指定长度n
 - varchar(n):可变长度的字符串,用户指定最大长度n
 - int:整数类型(和机器相关的整数类型的子集),等价于全程integer
 - smallint:小整数类型(和机器相关的整数类型的子集)
 - numeric(p, d):定点数,精度由用户指定。这个数有p位数字,其中d
位数字在小数点右边
 - real, double precision:浮点数与双精度浮点数,精度与机器相关
 - float(n):精度至少为n位的浮点数
 - null:每种类型都可以包含一个特殊值,即空值。可以申明属性值不为
空,禁止加入空值
 - date:日期,含年、月、日,如 ‘2015-3-20’
 - time:时间,含小时、分钟、秒,如‘ 08:15:30’或‘ 08:15:30.75’
 - timestamp:日期 + 时间,如‘2015-3-20 08:15:30.75’


 ### 视图
 #### 定义
 向用户隐藏特定的数据
 - SQL允许通过查询来定义“虚关系”,它在概念上包含查询的结果,但并不预先计算并存储。像这种作为虚关系对用户可见的关系称为视图(view)
 - 在SQL中,我们用create view命令定义视图,命令的格式为:
 ```
create view v as < query expression >
 ```
      - < query expression >可以是任何合法的查询表达式
      - v 表示视图名
      - 使用视图的目的:安全及易于使用
      - 对应地,删除视图,使用命令: drop view v
```sql
/* 举例代码:*/
create view physics_fall_2009 as
select course . course_id , sec_id , building , room_number
from course , section
where course . course_id = section . course_id
and course . dept_name = 'Physics'
and section . semester = 'Fall'
and section . year = '2009';
```


 #### SQL查询中使用视图
 - 一旦定义了一个视图,就可以用视图名指代该视图生成的虚关系。由于数据库只存储视图定义本身,那么当视图关系出现在查询中时,它就会被已存储的查询表达式代替
 ```sql
/*使用视图physics_fall_2009,找到所有于2009年秋季学期在Watson
 大楼开设的Physics课程*/
 select course_id , room_number
 from physics_fall_2009
 where building = 'Watson';
 ```


 #### SQL视图更新
 - 假设我们向视图 faculty 插入一条新元组,可写为:
insert into faculty values (‘30765’,‘Green’,‘Music’);
 - 但这个插入必须表示为对instructor关系的插入,即必须给出salary的值。存在
 - 两种合理的解决方法:
     - 拒绝插入,并向用户返回一个错误信息
     - 向instructor关系插入元组(‘30765’,‘Green’,‘Music’, null)

 - 对于视图的更新,处理方式较为严谨,所以需要判断此时定义的视图是否符合更新要求,一般地,如果定义视图的查询对下列条件都能满足,我们称SQL视图是可更新的(updatable),即视图上可以执行插入、更新或删除
          1. from子句中只有一个数据库关系
          2. select子句中只包含关系的属性名,不包含任何表达式、聚集或distinct声明
          3. 任何没有出现在select子句中的属性可以取空值;即这些属性上没有notnull约束,也不构成主码的一部分
          4. 查询中不含有group by或having子句


## 感悟
&emsp; 在开始学这本书之前,结合之前自学的数据库那点皮毛,想的有些简单
学了半本书发现有用的新知识太多了,学一门技术还是要实打实的从书看起,不然只是学了很多杂乱的知识点,只是皮毛
&emsp; 之前数据库可能只是简单的软件操作,以及sql语句的增删查改,但其实数据库很严谨,很全面立体
&emsp; 例如之前写程序都是直接直接操作表,但其实有一个明智的方法就是操作视图,对于稍大一点的mis系统,视图是一个完美的解决方案
&emsp; 并且数据库的底层是通过数学的集合运算(关系代数)来操作数据
&emsp; 对于数据的操作过程中,原子性操作的事务,以及数据库函数trigger,都是让我很惊喜的功能(感觉自己之前像个傻子...之前学了个锤子)
&emsp; 这几天加油把数据库学完!还有太多东西等着我去学

