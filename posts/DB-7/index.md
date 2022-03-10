# 数据库系统概论 SQL-1


SQL大礼包
<!--more-->

# schema(模式)
## 定义模式
```sql
create schema <模式名> authorization <用户名>
```

**为用户wang定义一个学生-课程模式"s-t"**
```sql
create schema"s-t" authorization wang;
```

**在定义模式的同时,定义表**
```sql
create schema test authorization wei
create table tab1(
    col1 SMALLINT,
    col2 INT,
    col3 CHAR(20),
    col4 NUMBER(10,3),
    col5 DECIMAL(5,2)
);
```
## 删除模式
```sql
drop schema <模式名> <cascade|restrict>;
/*
cascade级联删除,模式下的所有对象都删除
restrict限制删除,如果模式下有对象,则拒绝执行删除
*/
```

# table(表)
## 定义表
```sql
create table <表名>(<列名><数据类型>[列级完整性约束条件],...);
```

**建立一个"学生"表student**
```sql
create table student(
    sno CHAR(9) PRIMARY KEY,
    sname CHAR(20) UNIQUE,
    sex CHAR(2),
    sage SMALLINT,
    sdept CHAR(20)
);
```

**建立一个"课程"表course**
```sql
create table course(
    cno CHAR(4) PRIMARY KEY,
    cname CHAR(40) NOT NULL,
    cpno CHAR(4),
    ccredit SMALLINT,
    FOREIGN KEY(cpno) REFERENCES course(cno)
);
```

建立学生选课表sc
```sql
create table sc(
    sno CHAR(9),
    cno CHAR(4),
    grade SMALLINT,
    PRIMARY KEY(sno,cno),
    FOREIGN KEY(sno) REFERENCES student(sno),
    FOREIGN KEY(cno) REFERENCES student(cno)
);
```

## 修改基本表
```sql
alter table <表名>
[add [column]<新列名><数据类型>[完整性约束]]
[add <表级完整性约束>]
[drop[column]<列名>[cascade|restrict]]
[drop constraint <完整性约束>[restrict|cascade]]
[alter column<列名><数据类型>];
```

**向student表中增加"入学时间"列,其数据类型为日期型**
```sql
alter table student add s_intime DATE;
```

**将年龄数据类型由字符型改为整数**
```sql
alter table student alter column sage INT;
```

**增加课程名称必须取唯一值的约束条件**
```sql
alter table course add UNIQUE(cname);
```

## 删除基本表
```sql
drop table <表名> [restrict|cascade]; /*默认为restrict*/
```

删除学生表
```sql
drop table student cascade;
```
如果表上建有视图,选择restrict时表不能被删除,需要选择cascade

# 索引(index)
## 建立索引
```sql
create [UNIQUE][CLUSTER] index <索引名>
on <表名> (<列名>[<次序>],...)
```
ASC(升序) DESC(降序)
UNIQUE 表名此索引的每一个索引值只能对应唯一的数据记录
CLUSTER 表示要建立的索引时聚簇索引

**为学生-课程数据库中的student,course,sc三个表建立索引**
```sql
create UNIQUE index on student(sno);
create UNIQUE index on course(cno);
create UNIQUE index scno on sc(sno ASC, cno DESC);
```

## 修改索引
```sql
alter index <旧索引名> rename to <新索引名> 
```

## 删除索引
```sql
drop index <索引名>
```

# 查询
```sql
select [distinct|all]<目标列表达式>,...
from <表名或视图名>,...
[where <条件表达式>]
[group by <列名1>[having<条件表达式>]]
[order by<列名2> [ASC|DESC]];
```

## 表查询
涉及一个表的查询
1. 查询指定列
```sql
select sno,sname
from student;

select sname,sno,sdept
from student;
```

2. 查询所有列
```sql
select * from student;
```

3. 查询经过计算的值
```sql
select sname-sage
from student;
```

**查询全体学生的名字,出生日期,所在院系,要求小写字母表示院系**
```sql
select sname,'year of birth',2014-sage,lower(sdept)
from student;
```

4. 指定查询结果的列名
```sql
select sname name,'year of birth:' birth,2014-sage birthday,
lower(sdept) department
from student;
```
其中name,birth,department为查询结果的列名

5. 消除重复行
```sql
select DISTINCT sno from sc
```

6. 查询满足条件的元组
 - 比较大小
  ```sql
  select sname
  from student
  where sdept = 'cs';
  ```

 - 确定范围
  ```sql
  select sname,sdept,sage
  from student
  where sage between 20 and 23;
  ```

 - 确定集合
  ```sql
  select sname,ssex
  from student
  where sdept in ('cs','ma','is');
  /*where sdept not in ('cs','ma','is')*/
  ``` 

 - 字符串匹配
  ```sql
  [NOT] LIKE '<匹配串>' [ESCAPE'<换码字符>']
  ```
  
  % 代表任意长度的字符串
  _ 代表任意单个字符

  ```sql
  select *
  from student
  where sno like '20121512'
  /*等价于 where sno ='20121512'*/
  ```
  如果like后面的匹配串中不含有通配符,则可以用=运算符取代like

  **查询所有姓刘的学生的名字,学号,性别**
  ```sql
  select sname,sno,ssex
  from student
  where sname like '欧阳_';
  ```

  **查询姓"欧阳"且全名为三个汉字的学生的姓名**
  ```sql
  select sname
  from student
  where sname like '欧阳_'
  ```

  **查询名字中第二个字为"阳"的学生的姓名和学号**
  ```sql
  select sname,sno,ssex
  from student
  where sname not like '_阳%';
  ```

  **查询所有不姓刘的学生的名字,学号,性别**
  ```sql
  select sname,sno,ssx
  from student
  where sname not like '刘%';
  ```

  **查询db_design课程的课程号和学分**
  ```sql
  select cno,ccredit
  from course
  where cname like 'db\_design' escape '\';
  ```
  escape'\' 表示 '\'为转码字符,这样匹配串中紧跟在'\'后面的字符'_'不再具有通配符的含义,而只是单纯的字符

  **查询以'db_'开头,且倒数第三个字符为i的课程的详细信息**
  ```sql
  select *
  from course
  where cname like 'db\_%i__' escape '\';
  ```
  这里第一个'_'前面有换码字符'\',而后面的两个 _ 没有换码字符,因此都仍作为通配符

 - 涉及空值的查询
  某些同学没有参加选修课的考试,因此成绩为空
  ```sql
  select sno,sname
  from dc 
  where grade is null
  ```

 - 多重查询条件
  逻辑运算符and和or可用来连接多个查询条件,and的优先级高于or,但用户可以使用括号改变优先级
  查询计算机科学系中年龄在20以下的学生姓名
  ```sql
  select sname
  from student
  where sdpt='cs' and sage<20;
  ```

  在查询条件中'in'的作用等价于or
  ```sql
  select sname ssex
  from student
  where sdept='cs' or sdpt='ma' or sdpt='is';
  ```

## order by子句
用户可以用order by子句对查询结果按照一个或多个属性的升序(ASC)或降序(DESC)排列,**默认为升序**

**查询选修了课程3的学生的学号及成绩,查询结果按照分数的降序排列**
```sql
select sno,grade
from sc 
where cno = '3'
order by grade desc;
```

**查询全体学生的情况,查询结果按照所在系的系号升序排列,同一系中的学生按照年龄的降序排列**
```sql
select *
from student
order by sdept,sage desc
```

# 聚集函数
为了进一步方便用户,增强检索功能,sql提供了许多聚集函数
|           函数名            |        含义        |
| :-------------------------: | :----------------: |
|          count(*)           |   统计元组的个数   |
| count([distinc\|all]<列名>) | 统计一列中值的个数 |
|  sum(distinct\|all)<列名>   |  计算一列值的总和  |
|  avg(distinct\|all)<列名>   | 计算一列值的平均值 |
|  max(distinct\|all)<列名>   | 计算一列值的最大值 |
|  min(distinct\|all)<列名>   | 计算一列值的最小值 |

如果指定distinct短语,则表示在计算时要取消指定列中的重复值(all为默认值)

**查询学生总人数**
```sql
select count(*)
from student;
```

**查询选修了课程的学生人数**
```sql
select count(distinct sno)
from sc;
```

**计算选修1号课程的学生平均成绩**
```sql
select avg(grade)
from sc
where cno='1';
```

**查询选修1号课程的学生的最高分数**
```sql
select max(grade)
from sc
where cno='1';
```

**查询学生20125012选修课程的总学分数**
```sql
select sum(credit)
from sc,course
where sno='20125012' and sc.cno=course.cno;
```

