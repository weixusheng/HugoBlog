# 数据库系统概论 SQL-2


SQL大礼包
<!--more-->

# group by
将查询结果按某一列或多列的值分组,值相等的分为一组
对查询结果分组的目的时为了细化聚集函数的作用对象,如果未对查询结果分组,聚集函数将作用域整个查询结果,分组后的聚集函数将作用于每一个组,每一个组都有一个函数值

**求各个课程号以及相应的选课人数**
```sql
select cno,count(sno)
from sc
group by cno;
```

**查询选修了三门以上课程的学生学号**
```sql
select sno
from sc
group by sno
having count(*) > 3;
```
where和having短语的区别在于作用的对象不同,where子句作用于基本表或视图,从中选择满足条件的元组,having短语作用于组,从中选择满足条件的组

**查询平均成绩大于等于90分的学生学号和平均成绩**
```sql
select sno,avg(grade)
from sc
group by sno
having avg(grade) >= 90;
```

# 连接查询
**查询每个学生及其选修课程的情况**
```sql
select student.*,sc.*
from studen,sc
where student.sno = sc.sno;
/*如果属性列在两个表中是唯一的,则可以去掉表名前缀*/
select student.sno,sname,ssex,sage,sdept,cno,grade
from student,sc
where student.sno = sc.sno;
```

**查询选修2号课程且成绩在90分以上的所有学生的学号和姓名**
```sql
select student.sno,sname
from student,sc
where student.sno=sc.sno and
    sc.cno = '2' and sc.grade > 90;
```

# 自身连接
一个表和其自己连接
**查询每一门课的间接先修课(先修课的先修课)**
```sql
select first.cno,second.cpno
from course first,course second
where first.cpno = second.cno;
```

# 外连接
```sql
select student.sno,sname,ssex,sage,sdept,cno,grade
from student left outer join sc on (student.sno=sc.sno)
/*左外连接*/
```

# 多表连接
```sql
select student.sno,sname,cname,grade
from student,sc,course
where student.sno=sc.sno and sc.cno=course.cno;
```

# 嵌套查询
在sql中,一个select-from-where语句为一个查询块
将一个查询块嵌套在另一个查询块的**where子句或having**短语的条件的查询称为嵌套查询
```sql
select sname
from student
where sno in(
    select sno
    from sc
    where cno='2'
);
```

## 带有in的子查询
**查询与"刘晨"在同一个系学习的学生**
```sql
select sno,sname,sdept
from student
where sdept in(
    select sdept
    from student
    where sname = '刘晨'
);
```

**查询选修了课程为"信息系统"的学生的学号和姓名**
```sql
select sno,sname
from student
where sno in (
    select sno
    from sc
    where cno in (
        select cno
        from course
        where cname = '信息系统'
    )
);
```

## 带有比较运算符的子查询
**查询每个学生超过他自己选修课程的平均成绩的课程号**
```sql
select sno,cno
from sc x
where grade >= (
    select avg(grade)
    from sc y
    where y.sno = x.sno
);
```
如果子查询的条件依赖于父查询,那么这类查询称为相关子查询(上例中子查询中有夫查询元素)
整个查询语句称为相关嵌套查询

## 带有any(some)或all谓词的子查询
**查询非计算机科学系中比计算机科学系任意一个学生年龄小的学生姓名和年龄**
```sql
select sname,sage
from student
where sage < any(
    select sage
    from student
    where sdept='cs'
)
and sdept != 'cs';
```
 - ">any"表示至少大于一个值(大于最小值)
 - ">all"表示大于每一个值(大于最大值)

也可以用聚集函数表示
```sql
select sname,sage
from student
where sage < (
    select max(sage)
    from student
    where sdept = 'cs'
)
and sdept != 'cs';
```

**查询非计算机科学系中比计算机科学系所有学生年龄都小的学生姓名及年龄**
```sql
select sname,sage
from student
where sage < all(
    select sage
    from student
    where sdept='cs'
)
and sdept != 'cs';
```

也可以用聚集函数表示
```sql
select sname,sage
from student
where sage < (
    select min(sage)
    from student
    where sdept='cs'
)
and sdept != 'cs';
```

# 带有exist谓词的子查询
带有exist谓词的子查询不返回任何数据,只产生逻辑真"true"或逻辑假值"false"
**查询所有选修了1号课程的学生姓名**
```sql
select sname
from student
where exists(
    select *
    from sc
    where sno=student.sno and cno='1'
);
```

**查询选修了全部课程的学生姓名**
即没有一门课不是他不选修的
```sql
select sname
from student
where not exists(
    select *
    from course
    where not exists(
        select *
        from sc
        where sno=student.sno
              and 
              cno=course.cno
    )
);
```

**查询至少选修了学生20121512选修的全部课程的学生号码**
```sql
select distinct sno
from sc scx
where not exists(
    select *
    from sc scy
    where scy.sno = '20121512' 
          and
          not exists(
              select *
              from sc scz
              where scz.sno = scx.sno
              and
              scz.cno = scy.cno
          )
);
```

