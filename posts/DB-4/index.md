# 数据库原理及应用 PART-4



ER图
<!--more-->

# ER图
## 定义
E-R图也称实体-联系图(Entity Relationship Diagram)，提供了表示实体类型、属性和联系的方法，用来描述现实世界的概念模型。
## 常用表示符号
![常用符号1-w50](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/1.png)
![常用符号2-w50](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/2.png)
![常用符号3](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/3.png)
![常用符号4](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/4.png)

 - 矩形代表实体集
 - 菱形代表联系集
 - 线段将实体集连接到联系集
 - 构成主码的属性用下划线标明
 - 虚线将联系集属性连接到联系集
 ![虚线表示](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/5.png)
 - 双线显示实体在联系集中的参与度
 - 双菱形代表连接到弱实体集的标志性联系集

# ER图中的关系表示
## 角色
实体在联系集中的作用
下图给出了 course 实体集和 preq 联系集之间的角色标识 course_id 和prereq_id

![角色](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/6.png)

(角色标记是可选的,用于明确联系的语义)

## 基数约束
在联系集与实体集之间用有向直线(->)表示'一',无向直线(—)表示'多'
### 一对一联系
 - 实体集instructor和student之间的联系集可以是一对一,表示一名教师可以指导至多一名学生,并且一名学生可以有至多一位导师
![一对一](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/7.png)

### 一对多联系
实体集instructor和student之间的联系集可以是一对多,表示一名教师可以指导多名学生,但一名学生可以有至多一位导师
![一对多](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/8.png)

### 多对多联系
实体集instructor和student之间的联系集可以是多对多,表示一名教师可以指导多名学生,并且一名学生可以有多位导师
![多对多](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/9.png)

## 参与约束
### 全参与和部分参与约束
反映了一个实体参与关联的数目下限:0次,还是至少1次
 - 全参与(用双线表示):实体集中的每个实体都至少参加联系集中的一个联系
 - 部分参与:某些实体可能未参加联系集中的任何联系

![01参与](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/10.png)

### 映射基数约束
限定了一个实体与发生关联的另一端实体可能关联的数目上限
 - 例每个学生有且仅有一个导师,教师可以有零个或多个学生
![映射基数](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/11.png)

## 具有三元联系的E-R图
例如一个教师可以指导多个学生,并可以参与到多个项目
![三元联系](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/12.png)
但是三元关系显得很复杂,这就需要转为二元关系
![转换二元联系](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/13.png)

## 弱实体集
 - 不具有主键的实体集称为弱实体集
 - 例如考虑一个 section 实体,它由课程编号、学期、学年以及开课编号唯
一标识。显然,开课实体和课程实体相关联。假定我们在实体集 section
和 course 之间创建了一个联系集 sec_course。 若,实体集section为:
section ( sec_id,semester,year ),则其为弱实体集(父子关系)

![弱实体集](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/14.png)

 - 弱实体集的存在依赖于它的标识实体集(或属主实体集)的存在
 - 标识性联系:将弱实体集与其标识实体集相联的联系
标识性联系是从弱实体集到标识实体集多对一的,并且弱实体集在联系中的
参与是全部的
 - 弱实体集的分辨符(或称部分码)是指在一个弱实体集内区分所有实体
的属性集合
 - 例,弱实体集 section 的分辨符由属性 sec_id,year 以及 semester 组成
 - 弱实体集的主码由它所依赖的强实体集的主码加上它的**分辨符**组成
section的主码:
![弱实体集](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/15.png)
 - 注意:强实体集的主码并不显式地存于弱实体集中,而是隐含地通过标识性联系起作用
 - 如果 course_id 显式存在, section 就成了强实体,则 section与course 之间的联系变得冗余。因为 section 与 course 共有的属性course_id 已定义了一个隐含的联系

##  特化
 - 自顶向下设计过程中,确定实体集中的一个具有特殊性质的子集
 - 这些子集称为低层实体集,它们具有特殊的属性或者参加特殊的联系
 - **属性继承** :低层实体集继承它连接的高层实体集的所有属性及参加的联系
 ![特化继承](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/16.png)

## 概化
 - 自底向上设计过程中,将若干共享相同特性的实体集组合成一个高层实体集
 - 特化与泛化简单互逆:它们在E-R图中以相同方式表示
 - 关于实体在单个概化中是否可以属于多于一个低层实体集的约束
    - 不相交
        - 一个实体只能属于一个低层实体集
        - 在E-R图中ISA三角形旁边加注disjoint

    ![不相交](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/17.png)
    - 重叠
        - 一个实体可以属于多个低层实体集
    - 完备性约束
        - 说明高层实体集中的实体是否必须至少属于一个低层实体集
        - 全部概化或特化:每个高层实体必须属于一个低层实体集(双线)
         ![完备性约束](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/18.png)
        - 部分概化或特化:允许一些高层实体不属于任何低层实体集(默认的)

## 聚集
通过聚集消除冗余
 - 将联系视为一个抽象实体
 - 从而允许联系之间的联系
 - 联系抽象为新实体

![聚集](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/19.png)

# 构建ER图
## 例题1
&emsp; 学生学习某门课程。每个学生都有学号,姓名,年龄和性别属性。每门课程都有课程编号,课程名,学分和上课教室编号属性。每个学生可以是本科生或研究生。对于每一个本科生,我们想记录他/她的学习年份,平均绩点,以及(可能是多个)电子邮箱。对于每一个研究生,我们要记录他/她的导师的名字,以及(可能是多个)电子邮箱。此外,每门课程有一名研究生作为课程助教,我们要记录助教的开始和结束时间(例如,开始于2015年3月10日,结束于2015年6月30日)。绘制E-R图。

![例题1](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/20.png)

## 例题2
&emsp; 一个公司的数据库需要存储有关雇员(由empid唯一标识,有姓名、工资和手机号码作为属性),部门(由dno唯一标识,有部门名称与预算作为属性)和雇员的孩子信息(有姓名和年龄属性)。员工在部门工作,每个部门由一个管理员管理,孩子必须被唯一名称标识,其父母(假定只有父母中的一位在公司工作)是已知的。一旦父母离开公司,我们将不再关注孩子的信息。绘制ER图,然后将其转换为关系模式。

![例题2](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/DB-4/21.png)

**数据库就剩最后一章了!**

